---
title: Utilize o Servidor de Autenticação Multi-Factor Azure para ativar cenários PAM ou SSPR Microsoft Docs
description: Configurar o Azure Multi-Factor Authentication Server como uma segunda camada de segurança quando os seus utilizadores ativarem funções na Gestão de Acesso Privilegiado e no Reset de Palavra-Passe de Autosserviço.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 1/5/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 68f8ae0f64e136add96b3abb5083331912a0efe7
ms.sourcegitcommit: 41d399b16dc64c43da3cc3b2d77529082fe1d23a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104110"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>Utilize o Servidor de Autenticação Multi-Factor Azure para ativar o PAM ou o SSPR
O documento que se segue descreve como configurar o servidor Azure MFA como uma segunda camada de segurança quando os seus utilizadores ativam funções em Gestão de Acesso Privilegiado ou Self-Service Redefinição de Password.

> [!IMPORTANT]
> Devido à depreciação do Azure Multi-Factor Authentication (MFA) Software Development Kit (SDK), os clientes não poderão mais descarregar o Azure MFA SDK.

O artigo abaixo descreve a atualização de configuração e passos para permitir a mudança de Azure MFA SDK para Azure MFA Server.

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar o Servidor de Autenticação Multi-Factor Azure com MIM, necessita de:

- Acesso à Internet a partir de cada Serviço MIM ou Servidor MFA que fornece PAM e SSPR, para contactar o serviço Azure MFA
- Uma subscrição do Azure
- A instalação já está a utilizar o Azure MFA SDK
- Licenças Azure Ative Directory Premium para utilizadores candidatos
- Números de telefone para todos os utilizadores candidatos
- Fixação MIM 4.5. ou maior ver [história da versão](./reference/version-history.md) para anúncios

## <a name="azure-multi-factor-authentication-server-configuration"></a>Configuração do servidor de autenticação multi-factor Azure 
> [!NOTE] 
> Na configuração, necessitará de um certificado SSL válido instalado para o SDK. 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Passo 1: Descarregue o Servidor de Autenticação Multi-Factor Azure a partir do portal Azure 
Faça o sível no [portal Azure](https://portal.azure.com/) e descarregue o servidor Azure MFA.
![trabalho-com-mfaserver-for-mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### <a name="step-2-generate-activation-credentials"></a>Passo 2: Gerar credenciais de ativação
Utilize as **credenciais de ativação Gerar para iniciar a utilização** de link para gerar credenciais de ativação. Uma vez gerado, guarde para utilização posterior.

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>Passo 3: Instalar o Servidor de Autenticação Multi-Factor Azure
Uma vez descarregado o servidor, [instale-o.](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server)  As suas credenciais de ativação serão necessárias. 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>Passo 4: Crie a sua Aplicação Web IIS que irá acolher o SDK
1. Open IIS Manager ![working-with-mfaserver-for-mim_iis.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  Crie um novo website chamado "MIM MFASDK", ligue-o a um diretório vazio ![working-with-mfaserver-for-mim_sdkweb.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Abra a consola de autenticação multi-factor e clique no Serviço Web SDK ![working-with-mfaserver-for-mim_sdkinstall.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. Uma vez que os assistentes clicam através do config, selecione "MIM MFASDK" e piscina de aplicativos

> [!NOTE] 
> O Assistente exigirá a criação de um grupo de administração. Mais informações podem ser encontradas na documentação do Azure > > MFA Azure Multi-Factor Authentication Server.

5. Em seguida, precisamos importar a conta de Serviço MIM aberta Consola de Autenticação Multi-Factor selecione "Utilizadores" a. Clique em "Import from Ative Directory" b. Navegue para a conta de serviço, como "contoso\mimservice" c. Clique em "Importar" e "Fechar" ![working-with-mfaserver-for-mim_importmimserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) 
6. Edite a conta de Serviço MIM para ativar na consola de autenticação multi-factor ![working-with-mfaserver-for-mim_enableserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
7. Atualize a autenticação do IIS no website "MIM MFASDK". Primeiro desativaremos a "Autenticação Anónima" e depois ativaremos a autenticação do ![ Windows"working-with-mfaserver-for-mim_iisconfig.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
8. Passo Final: Adicione a conta de serviço MIM àworking-with-mfaserver-for-mim_addservicetomfaadmin.PNG"PhoneFactor" ![](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>Configurar o serviço MIM para o Servidor de Autenticação Multi-Factor Azure 

### <a name="step-1-patch-server-to-452020"></a>Passo 1: Patch Server para 4.5.202.0
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Passo 2: Cópia de segurança e abra o MfaSettings.xml localizado no "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-3-update-the-following-lines"></a>Passo 3: Atualizar as seguintes linhas
1. Remover/Limpar as seguintes linhas de entradas de configuração <br>
<LICENSE_KEY></LICENSE_KEY><br>
<GROUP_KEY></GROUP_KEY><br>
<CERT_PASSWORD></CERT_PASSWORD><br>
<CertFilePath></CertFilePath><br>

2. Atualizar ou adicionar as seguintes linhas ao seguinte MfaSettings.xml <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. Reinicie o Serviço MIM e teste a funcionalidade com o Servidor de Autenticação Multi-Factor Azure.

> [!NOTE] 
> Para reverter a definição substitua MfaSettings.xml pelo seu ficheiro de backup no passo 2


## <a name="see-also"></a>Ver também

-    [Introdução ao Servidor Multi-Factor Authentication do Azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)
- [O que é a autenticação multi-factor Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Utilize a API de autenticação multi-factor personalizada para ativar a PAM ou a SSPR](Working-with-custommfaserver-for-mim.md)
- [Histórico de lançamento da versão MIM](./reference/version-history.md)
