---
title: Usar um provedor de autenticação multifator alternativo por meio de uma API para ativar o PAM ou no cenário de SSPR | Microsoft Docs
description: Configure a API MFA personalizada como uma segunda camada de segurança quando os usuários ativarem funções no Privileged Access Management e usarem a redefinição de senha de autoatendimento.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: mtillman
ms.date: 09/04/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 7fb111520f94541672fc56d0fd2ee95bfcd3a49e
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "67690751"
---
# <a name="use-a-custom-multi-factor-authentication-provider-via-an-api-during-pam-role-activation-or-in-sspr"></a>Usar um provedor de autenticação multifator personalizado por meio de uma API durante a ativação da função do PAM ou em SSPR

Os clientes do Azure AD Premium ou do Azure MFA podem integrar o Azure MFA em dois cenários do MIM – Privileged Access Management (PAM) ativação de função e redefinição de senha de autoatendimento (SSPR).

Os clientes do MIM têm duas opções adicionais:

 - Use um provedor de entrega de senha única personalizado, que é aplicável somente no cenário do MIM SSPR e documentado em guia para [Configurar a redefinição de senha de autoatendimento com a entrada do SMS de OTP](https://docs.microsoft.com/en-us/previous-versions/mim/hh824692(v=ws.10))
 - Use um provedor de telefonia de autenticação multifator personalizado. Isso é aplicável nos cenários do MIM SSPR e do PAM, descritos neste artigo

Este artigo descreve como usar o MIM com um provedor de autenticação multifator personalizado, por meio de uma API e um SDK de integração desenvolvido pelo cliente.  

## <a name="prerequisites"></a>Pré-requisitos

Para usar uma API personalizada do provedor de autenticação multifator com MIM, você precisa:

- Números de telefone para todos os utilizadores candidatos
- Hotfix do MIM [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) ou posterior-consulte o [histórico de versões](reference/version-history.md) para anúncios
- Serviço do MIM configurado para SSPR ou PAM

## <a name="approach-using-custom-multi-factor-authentication-code"></a>Abordagem usando o código de autenticação multifator personalizado

### <a name="step-1-ensure-mim-service-is-at-version-452020-or-later"></a>Etapa 1: Verifique se o serviço do MIM está na versão 4.5.202.0 ou posterior

Baixe e instale o hotfix do MIM [4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) ou uma versão posterior.

### <a name="step-2-create-a-dll-which-implements-the-iphoneserviceprovider-interface"></a>Etapa 2: criar uma DLL que implementa a interface IPhoneServiceProvider

A DLL deve incluir uma classe, que implementa três métodos:

- `InitiateCall`: o serviço do MIM invocará esse método. O serviço passa o número de telefone e a ID da solicitação como parâmetros.  O método deve retornar um valor `PhoneCallStatus` de `Pending`, `Success` ou `Failed`.
- `GetCallStatus`: se uma chamada anterior para `initiateCall` retornou `Pending`, o serviço do MIM invocará esse método. Esse método também retorna `PhoneCallStatus` valor de `Pending`, `Success` ou `Failed`.
- `GetFailureMessage`: se uma invocação anterior de `InitiateCall` ou `GetCallStatus` retornou `Failed`, o serviço do MIM invocará esse método. Esse método retorna uma mensagem de diagnóstico.

As implementações desses métodos devem ser thread-safe e, além disso, a implementação do `GetCallStatus` e `GetFailureMessage` não devem presumir que eles serão chamados pelo mesmo thread como uma chamada anterior para `InitiateCall`.

Armazene a DLL no diretório `C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\`.

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
### <a name="step-3-backup-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Etapa 3: fazer backup do MfaSettings. xml localizado em "C:\Arquivos de Programas\microsoft Forefront Identity Manager\2010\Service"

### <a name="step-4-edit-the-mfasettingsxml-file"></a>Etapa 4: editar o arquivo MfaSettings. xml

Atualize ou desmarque as seguintes linhas:

- Remover/limpar todas as linhas de entradas de configuração 

- Atualize ou adicione as linhas a seguir ao MfaSettings. XML com seu provedor de telefone personalizado <br>
`<CustomPhoneProvider>C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\CustomPhoneGate.dll</CustomPhoneProvider>`

### <a name="step-5-restart-mim-service"></a>Etapa 5: reiniciar o serviço do MIM

Depois que o serviço for reiniciado, use SSPR e/ou PAM para validar a funcionalidade com o provedor de identidade personalizado.

> [!NOTE] 
> Para reverter a configuração, substitua MfaSettings. xml pelo arquivo de backup na etapa 3


## <a name="next-steps"></a>Próximos Passos

- [Introdução ao Servidor Multi-Factor Authentication do Azure](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [O que é a autenticação multifator do Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Histórico de lançamento de versão do MIM](./reference/version-history.md)
