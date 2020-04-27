---
title: Utilize um fornecedor alternativo de autenticação multi-factor através de uma API para ativar pam ou em cenário SSPR [ Microsoft Docs
description: Configurar o Custom MFA API como uma segunda camada de segurança quando os seus utilizadores ativarem funções na Gestão de Acesso Privilegiado e utilizarem o Reset de Passwords self service.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: daveba
ms.date: 09/04/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: b157b2a8716d20ce3b472d5655d393e64f2baa6b
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044366"
---
# <a name="use-a-custom-multi-factor-authentication-provider-via-an-api-during-pam-role-activation-or-in-sspr"></a>Utilize um fornecedor personalizado de autenticação multi-factor através de uma API durante a ativação de funções PAM ou em SSPR

Os clientes do Azure AD Premium ou do Azure MFA podem integrar o Azure MFA em dois cenários MIM - Ativação de funções de Gestão de Acesso Privilegiado (PAM) e Reset de Passwordself-Service (SSPR).

Os clientes mim têm duas opções adicionais:

 - Utilize um fornecedor de entrega de senha de senha de uma só vez, que é aplicável apenas no cenário MIM SSPR e documentado no guia para configurar o reset de [palavra-passe self-service com o Portão SMS da OTP](https://docs.microsoft.com/previous-versions/mim/hh824692(v=ws.10))
 - Utilize um fornecedor personalizado de telefonia de autenticação multifactor. Isto é aplicável tanto nos cenários mim SSPR como PAM, descrito sºL e PAM, descrito neste artigo

Este artigo descreve como usar mim com um fornecedor de autenticação de vários fatores personalizado, através de uma API e um SDK de integração desenvolvido pelo cliente.  

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar um Fornecedor de Autenticação Multifactor personalizado com MIM, necessita de:

- Números de telefone para todos os utilizadores candidatos
- HOTFix MIM 4.5.202.0 ou mais tarde - veja [o histórico da versão](reference/version-history.md) para anúncios
- Serviço MIM configurado para SSPR ou PAM

## <a name="approach-using-custom-multi-factor-authentication-code"></a>Abordagem usando código de autenticação de vários fatores personalizado

### <a name="step-1-ensure-mim-service-is-at-version-452020-or-later"></a>Passo 1: Garantir que o serviço MIM está na versão 4.5.202.0 ou mais tarde

Descarregue e instale o hotfix [MIM 4.5.202.0](https://www.microsoft.com/download/details.aspx?id=57278) ou uma versão posterior.

### <a name="step-2-create-a-dll-which-implements-the-iphoneserviceprovider-interface"></a>Passo 2: Criar um DLL que implementa a interface iPhoneServiceProvider

O DLL deve incluir uma classe, que implementa três métodos:

- `InitiateCall`: O Serviço MIM invocará este método. O serviço passa o número de telefone e pede identificação como parâmetros.  O método deve `PhoneCallStatus` devolver `Pending` `Success` um `Failed`valor de, ou .
- `GetCallStatus`: Se uma chamada `initiateCall` `Pending`anterior para regressar, o Serviço MIM invocará este método. Este método `PhoneCallStatus` também `Pending`devolve `Success` `Failed`o valor de, ou .
- `GetFailureMessage`: Se uma invocação `GetCallStatus` `Failed`anterior de `InitiateCall` ou devolva, o Serviço MIM invocará este método. Este método devolve uma mensagem de diagnóstico.

As implementações destes métodos devem ser seguras e, além disso, a implementação do `GetCallStatus` e `GetFailureMessage` não `InitiateCall`deve assumir que serão chamadas pelo mesmo fio que uma chamada anterior para .

Guarde o DLL no `C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\` diretório.

Código de amostra, que pode ser compilado usando o Visual Studio 2010 ou mais tarde.

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
### <a name="step-3-backup-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Passo 3: Backup the MfaSettings.xml localizado no "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-4-edit-the-mfasettingsxml-file"></a>Passo 4: Editar o ficheiro MfaSettings.xml

Atualizar ou limpar as seguintes linhas:

- Remover/Limpar todas as linhas de entradas de configuração 

- Atualize ou adicione as seguintes linhas ao MfaSettings.xml com o seu fornecedor de telefone personalizado <br>
`<CustomPhoneProvider>C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\CustomPhoneGate.dll</CustomPhoneProvider>`

### <a name="step-5-restart-mim-service"></a>Passo 5: Reiniciar o serviço MIM

Depois de o serviço ter reiniciado, utilize sSPR e/ou PAM para validar a funcionalidade com o fornecedor de identidade personalizada.

> [!NOTE] 
> Para reverter a definição substitua MfaSettings.xml com o seu ficheiro de backup no passo 3


## <a name="next-steps"></a>Passos Seguintes

- [Introdução ao Servidor Multi-Factor Authentication do Azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)
- [O que é a autenticação de multi-factor estoque Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [História de lançamento da versão MIM](./reference/version-history.md)
