---
title: Utilizar o Azure MFA para ativar o PAM | Documentos da Microsoft
description: Configure o MFA do Azure como uma segunda camada de segurança quando os utilizadores ativarem funções de Privileged Access Management.
keywords: ''
author: billmath
ms.author: billmath
ms.reviewer: fimguy
manager: daveba
ms.date: 07/06/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.openlocfilehash: 1c26147dc1e192804011ee104989a508acb25974
ms.sourcegitcommit: 41d399b16dc64c43da3cc3b2d77529082fe1d23a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104059"
---
# <a name="using-azure-mfa-for-activation-in-mim-pam"></a>Utilização de Azure MFA para ativação em MIM PAM
> [!IMPORTANT]
> Versões anteriores da MIM utilizaram o Kit de Desenvolvimento de Software (MFA) de Autenticação Multi-Factor (MFA) para integrar com o Azure MFA.  Devido à depreciação do Azure MFA SDK, os clientes MIM existentes e novos não poderão mais descarregar Azure MFA SDK. Para obter informações sobre a utilização do Azure MFA Server no lugar do Azure MFA SDK, consulte utilizar o [Servidor MFA Azure em PAM ou SSPR](../working-with-mfaserver-for-mim.md).




Quando configurar uma função de PAM, pode escolher como autorizar os utilizadores que pedem para ativar a função. As opções que a atividade de autorização de PAM implementa são:

- Aprovação do proprietário da função
- [Azure Multi-Factor Authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)

Se nenhuma verificação estiver ativada, os utilizadores candidatos são automaticamente ativados para a respetiva função.

O Multi-Factor Authentication do Microsoft Azure (MFA) é um serviço de autenticação que requer que os utilizadores verifiquem as tentativas de início de sessão com uma aplicação móvel, uma chamada telefónica ou uma mensagem de texto. Está disponível para utilizar com o Microsoft Azure Active Directory e como um serviço para aplicações empresariais na nuvem e no local. Para o cenário PAM, o Azure MFA fornece um mecanismo adicional de autenticação. O Azure MFA pode ser utilizado para autorização, independentemente da forma como um utilizador autenticado no domínio DO Windows PRIV.

> [!NOTE]
> A abordagem PAM com um ambiente de bastião fornecido pela MIM destina-se a ser utilizada numa arquitetura personalizada para ambientes isolados onde o acesso à Internet não esteja disponível, onde esta configuração é exigida por regulamentação, ou em ambientes isolados de alto impacto, como laboratórios de investigação offline e tecnologia operacional desligada ou ambientes de controlo de supervisão e aquisição de dados.  Como o Azure MFA é um serviço de Internet, esta orientação é fornecida exclusivamente para os clientes MIM PAM existentes ou para aqueles em ambientes onde esta configuração é exigida pela regulamentação. Se o seu Ative Directory faz parte de um ambiente ligado à Internet, consulte [a garantia de acesso privilegiado](/security/compass/overview) para obter mais informações sobre por onde começar.

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar o Azure MFA com MIM PAM, você precisa:

- Acesso à Internet a partir de cada Serviço MIM que fornece o PAM, para contactar o serviço MFA do Azure
- Uma subscrição do Azure
- Azure MFA
- Licenças Azure Ative Directory Premium para utilizadores candidatos
- Números de telefone para todos os utilizadores candidatos

## <a name="downloading-the-azure-mfa-service-credentials"></a>Transferir as Credenciais do Serviço MFA do Azure

Anteriormente, você descarregaria um ficheiro ZIP do Azure MFA SDK que incluía o material de autenticação para mim entrar em contato com Azure MFA. No entanto, uma vez que o Azure MFA SDK já não está disponível, consulte [o Use Azure MFA Server em PAM ou SSPR](../working-with-mfaserver-for-mim.md) Para obter informações sobre a utilização do Azure MFA Server.


## <a name="configuring-the-mim-service-for-azure-mfa"></a>Configurar o serviço MIM para o MFA do Azure

1.  No computador onde o Serviço MIM está instalado, inicie sessão como um utilizador ou o utilizador que instalou o MIM.

2.  Crie uma nova pasta de diretório no diretório onde o Serviço MIM foi instalado, como ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts```.

3.  Utilizando o Windows Explorer, navegue na ```pf\certs``` pasta do ficheiro ZIP descarregada na secção anterior. Copie o ficheiro ```cert\_key.p12``` para o novo diretório.

4.  Utilizando o Windows Explorer, navegue na ```pf``` pasta do ZIP e abra o ficheiro num editor de texto como o ```pf\_auth.cs``` Notepad.

5. Encontre estes três parâmetros: ```LICENSE\_KEY``` ```GROUP\_KEY``` . . ```CERT\_PASSWORD``` .

![Copiar os valores do ficheiro pf\_auth.cs - captura de ecrã](media/PAM-Azure-MFA-Activation-Image-2.png)

6. Com o Bloco de Notas, abra **MfaSettings.xml** localizado em ```C:\Program Files\Microsoft Forefront Identity Manager\2010\Service```.

7. Copie os valores dos parâmetros LICENSE\_KEY, GROUP\_KEY e CERT\_PASSWORD no ficheiro pf\_auth.cs para os respetivos elementos xml no ficheiro MfaSettings.xml.

8. No **<CertFilePath>** elemento XML, especifique o nome do caminho completo do \_ ficheiro cert.p12 extraído anteriormente.

9. No **<username>** elemento introduza qualquer nome de utilizador.

10. No **<DefaultCountryCode>** elemento introduza o código do país para marcar os seus utilizadores, como 1 para os Estados Unidos e Canadá. Este valor é utilizado no caso dos utilizadores estarem registados com números de telefone que não tenham um código de país. Se o número de telefone de um utilizador tiver um código de país internacional distinto do configurado para a organização, esse código de país tem de ser incluído no número de telefone que será registado.

11. Guarde e substitua o ficheiro **MfaSettings.xml** na pasta do Serviço MIM ```C:\Program Files\Microsoft Forefront Identity Manager\2010\\Service```.

> [!NOTE]
> No final do processo, certifique-se de que o ficheiro **MfaSettings.xml**, ou quaisquer cópias do mesmo, ou o ficheiro ZIP não são legíveis publicamente.

## <a name="configure-pam-users-for-azure-mfa"></a>Configurar utilizadores do PAM para o MFA do Azure

Para um utilizador ativar uma função que requeira o MFA do Azure, o respetivo número de telefone tem de ser armazenado no MIM. Existem duas formas de definir este atributo.

Primeiro, o comando `New-PAMUser` copia um atributo de número de telefone da entrada de diretório do utilizador no domínio CORP para a base de dados do Serviço MIM. Tenha em atenção que se trata de uma operação única.

Segundo, o comando `Set-PAMUser` atualiza o atributo de número de telefone na base de dados do Serviço MIM. Por exemplo, o seguinte substitui o número de telefone de um utilizador do PAM existente no Serviço MIM. A respetiva entrada de diretório permanece inalterada.

```PowerShell
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```

## <a name="configure-pam-roles-for-azure-mfa"></a>Configurar funções de PAM para o MFA do Azure

Depois de todos os utilizadores candidatos a uma função de PAM terem os números de telefone armazenados na base de dados do Serviço MIM, a função pode ser configurada para requerer o MFA do Azure. Isto é feito utilizando os comandos `New-PAMRole` ou `Set-PAMRole`. Por exemplo,

```PowerShell
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

O MFA do Azure pode ser desativado para uma função especificando o parâmetro "-MFAEnabled 0" no comando `Set-PAMRole`.

## <a name="troubleshooting"></a>Resolução de problemas

Os eventos seguintes podem ser encontrados no registo de eventos da Gestão de Acesso Privilegiado:

| ID  | Gravidade | Gerado por | Descrição |
|-----|----------|--------------|-------------|
| 101 | Error       | Serviço MIM            | O utilizador não concluiu o MFA do Azure (por exemplo, não atendeu o telefone) |
| 103 | Informações | Serviço MIM            | O utilizador concluiu o MFA do Azure durante a ativação                       |
| 825 | Aviso     | Serviço de Monitorização de PAM | O número de telefone foi alterado                                |

## <a name="next-steps"></a>Passos Seguintes

- [O que é a autenticação multi-factor Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
