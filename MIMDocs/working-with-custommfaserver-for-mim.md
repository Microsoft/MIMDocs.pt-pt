---
title: Utilizar um fornecedor de multi-factor Authentication alternativo por meio de uma API para ativar o PAM ou cenário de SSPR | Documentos da Microsoft
description: Configure a MFA a API personalizada como uma segunda camada de segurança, quando os utilizadores ativarem funções de Privileged Access Management e utilizam a reposição personalizada de palavra-passe.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: mtillman
ms.date: 09/04/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 7fb111520f94541672fc56d0fd2ee95bfcd3a49e
ms.sourcegitcommit: f58926a9e681131596a25b66418af410a028ad2c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/09/2019
ms.locfileid: "67690751"
---
# <a name="use-a-custom-multi-factor-authentication-provider-via-an-api-during-pam-role-activation-or-in-sspr"></a>Utilizar um fornecedor de multi-factor Authentication personalizado por meio de uma API durante a ativação de função de PAM ou na SSPR

Os clientes do Azure AD Premium ou o MFA do Azure podem integrar o MFA do Azure em dois cenários de MIM - ativação de função de Privileged Access Management (PAM) e de reposição de palavra-passe Self-Service (SSPR).

Os clientes de MIM tem duas opções adicionais:

 - Utilizar um fornecedor de entrega de uma-palavra-passe personalizada, que é aplicável apenas no cenário de MIM SSPR e documentado no manual para [configurar a reposição personalizada de palavra-passe com a porta de SMS OTP](https://docs.microsoft.com/en-us/previous-versions/mim/hh824692(v=ws.10))
 - Utilize um fornecedor de telefonia a autenticação multifator personalizada. Isso se aplica a MIM SSPR e o PAM cenários, descritos neste artigo

Este artigo descreve como utilizar o MIM com um provedor de autenticação personalizado de multi-factor, por meio de uma API e uma integração SDK desenvolvido pelo cliente.  

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar uma API do fornecedor de multi-factor Authentication personalizada com o MIM, terá de:

- Números de telefone para todos os utilizadores candidatos
- Correção MIM [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) ou superior - veja [histórico de versões](reference/version-history.md) anúncios de
- Serviço MIM configurado para SSPR ou PAM

## <a name="approach-using-custom-multi-factor-authentication-code"></a>Abordagem usando código personalizado multi-factor authentication

### <a name="step-1-ensure-mim-service-is-at-version-452020-or-later"></a>Passo 1: Certifique-se de que o serviço MIM está na versão 4.5.202.0 ou posterior

Transfira e instale a correção MIM [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) ou uma versão posterior.

### <a name="step-2-create-a-dll-which-implements-the-iphoneserviceprovider-interface"></a>Passo 2: Criar uma DLL que implementa a interface de IPhoneServiceProvider

A DLL tem de incluir uma classe que implementa três métodos:

- `InitiateCall`: O serviço MIM irá invocar esse método. O serviço passa o ID de número e a pedido do telefone como parâmetros.  O método deve retornar um `PhoneCallStatus` valor de `Pending`, `Success` ou `Failed`.
- `GetCallStatus`: Se uma chamada anterior para `initiateCall` devolvido `Pending`, o serviço MIM irá invocar esse método. Esse método também retorna `PhoneCallStatus` valor de `Pending`, `Success` ou `Failed`.
- `GetFailureMessage`: Se uma invocação anterior do `InitiateCall` ou `GetCallStatus` devolvido `Failed`, o serviço MIM irá invocar esse método. Este método devolve uma mensagem de diagnóstico.

As implementações dos seguintes métodos tem de ser thread-safe e, além da implementação do `GetCallStatus` e `GetFailureMessage` não deve supor que serão chamados pelo mesmo thread como uma chamada anterior para `InitiateCall`.

Store DLL no `C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\` diretório.

Código de exemplo, que pode ser compilado usando o Visual Studio 2010 ou posterior.

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using Microsoft.IdentityManagement.PhoneServiceProvider;

namespace CustomPhoneGate
{
    public class CustomPhoneGate: IPhoneServiceProvider
    {
        string path = @"c:\Test\phone.txt";
        public PhoneCallStatus GetCallStatus(string callId)
        {
            int res = 2;
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        bool b = Int32.TryParse(info[2], out res);
                        if (!b)
                        {
                            res = 2;
                        }
                    }
                    break;
                }
            }
            switch(res)
            {
                case 0:
                    return PhoneCallStatus.Pending;
                case 1:
                    return PhoneCallStatus.Success;
                case 2:
                    return PhoneCallStatus.Failed;
                default:
                    return PhoneCallStatus.Failed;
            }       
        }
        public string GetFailureMessage(string callId)
        {
            string res = "Call ID is not found";
            foreach (string line in File.ReadAllLines(path))
            {
                var info = line.Split(new char[] { ';' });
                if (string.Compare(info[0], callId) == 0)
                {
                    if (info.Length > 2)
                    {
                        res = info[3];
                    }
                    else
                    {
                        res = "Description is not found";
                    }
                    break;
                }
            }
            return res;            
        }
        
        public PhoneCallStatus InitiateCall(string phoneNumber, Guid requestId, Dictionary<string,object> deliveryAttributes)
        {
            // Here should be some logic for performing voice call
            // For testing purposes we just write details in file             
            string info = string.Format("{0};{1};{2};{3}", requestId, phoneNumber, 0, string.Empty);
            using (StreamWriter sw = File.AppendText(path))
            {
                sw.WriteLine(info);                
            }
            return PhoneCallStatus.Pending;    
        }
    }
}
```
### <a name="step-3-backup-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Passo 3: Cópia de segurança a mfasettings. XML localizado no "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-4-edit-the-mfasettingsxml-file"></a>Passo 4: Edite o ficheiro mfasettings. XML.

Atualizar ou desmarque as seguintes linhas:

- Remover/limpar todas as linhas de entradas de configuração 

- Atualize ou adicione as seguintes linhas para o seguinte para mfasettings. XML com o seu fornecedor de telefone personalizado <br>
`<CustomPhoneProvider>C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\CustomPhoneGate.dll</CustomPhoneProvider>`

### <a name="step-5-restart-mim-service"></a>Passo 5: Reinicie o serviço MIM

Depois do serviço foi reiniciado, utilize o SSPR e/ou de PAM para validar a funcionalidade com o fornecedor de identidade personalizada.

> [!NOTE] 
> Para reverter definição substituir mfasettings. XML com o ficheiro de cópia de segurança no passo 3


## <a name="next-steps"></a>Próximos Passos

- [Introdução ao servidor de autenticação do multi-factor do Azure](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [O que é o Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Histórico de lançamento de versão de MIM](./reference/version-history.md)
