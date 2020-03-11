---
title: Utilize o servidor de autenticação multi-factor Azure para ativar cenários PAM ou SSPR  Microsoft Docs
description: Configurar o Servidor de Autenticação De Vários Fatores Azure como uma segunda camada de segurança quando os seus utilizadores ativarem funções na Gestão de Acesso Privilegiado e no Reset de Passwords de Autosserviço.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/29/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 1dd87db01a5c1100c8206d82bedf96a8a5e702ad
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044315"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>Utilize o servidor de autenticação azure multi-factor para ativar PAM ou SSPR
O seguinte documento descreve como configurar o servidor Azure MFA como uma segunda camada de segurança quando os seus utilizadores ativam funções em Gestão de Acesso Privilegiado ou Reset de Palavras-passe de Self-Service.

> [!IMPORTANT]
> Devido ao anúncio da Deprecation do Kit de Desenvolvimento de Software de Autenticação Multifactor Azure, o Azure MFA SDK será suportado para os clientes existentes até à data de reforma de 14 de novembro de 2018. Novos clientes e clientes atuais não poderão mais descarregar SDK através do portal clássico Azure. Para descarregar terá de contactar o suporte ao cliente do Azure para receber o seu pacote de Credenciais de Serviço MFA gerado.

O artigo abaixo descreve a atualização de configuração e os passos para permitir a mudança de Azure MFA SDK para O Servidor de Autenticação Multi-Factor Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar o Servidor de Autenticação Multi-Factor Azure com MIM, necessita de:

- Acesso à Internet a partir de cada serviço MIM ou Servidor MFA que forneça PAM e SSPR, para contactar o serviço Azure MFA
- Uma subscrição do Azure
- A instalação já está a utilizar o Azure MFA SDK
- Licenças do Azure Active Directory Premium para utilizadores candidatos ou um meio alternativo de licenciamento do MFA do Azure
- Números de telefone para todos os utilizadores candidatos
- Hotfix MIM 4.5. ou ver [a história](./reference/version-history.md) da versão para anúncios

## <a name="azure-multi-factor-authentication-server-configuration"></a>Configuração do servidor de autenticação de vários fatores Azure 
> [!NOTE] 
> Na configuração, necessitará de um certificado SSL válido instalado para o SDK. 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Passo 1: Descarregue o servidor de autenticação multi-factor Azure a partir do portal azure 
Inscreva-se no [portal Azure](https://portal.azure.com/) e descarregue o servidor Azure MFA.
![](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG) de trabalho com o mfaserver-para-mim_downloadmfa

### <a name="step-2-generate-activation-credentials"></a>Passo 2: Gerar credenciais de ativação
Utilize as credenciais de **ativação Gerar para iniciar** o link de utilização para gerar credenciais de ativação. Uma vez gerado, guarde para posterior utilização.

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>Passo 3: Instalar o servidor de autenticação multi-factor Azure
Depois de ter descarregado o servidor, [instale-o.](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server)  As suas credenciais de ativação serão necessárias. 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>Passo 4: Crie a sua Aplicação Web IIS que irá acolher o SDK
1. Open IIS Manager ![a trabalhar com o mfaserver-para-mim_iis.](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG) do PNG
2.  Crie um novo website chamado "MIM MFASDK", ligue-o a um diretório vazio ![trabalhar com o mfaserver-para-mim_sdkweb.](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG) do PNG
3. Abra a Consola de Autenticação multi-factor e clique em Web Service SDK ![trabalhar com o mfaserver-para-mim_sdkinstall.](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG) do PNG
4. Uma vez que os assistentes clique através de config, selecione "MIM MFASDK" e app pool

> [!NOTE] 
> O Assistente exigirá a criação de um grupo de administração. Mais informações podem ser encontradas na documentação do Servidor de Autenticação Multifactor Do MFA Azure.

5. Em seguida, precisamos importar a conta de serviço MIM abrir consola de autenticação multi-factor selecione "Utilizadores" a. Clique em "Import from Ative Diretório" b. Navegue para a conta de serviço aka "contoso\mimservice" c. Clique em "Importar" e "Fechar" ![trabalhar com o mfaserver-para-mim_importmimserviceaccount.](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) do PNG 
6. Editar a conta de serviço MIM para ativar na consola de autenticação multi-factor ![trabalhar com o mfaserver-for-mim_enableserviceaccount.](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG) do PNG
7. Atualize a autenticação IIS no website "MIM MFASDK". Primeiro, desativaremos a "Autenticação Anónima" e depois ativamos a autenticação do Windows" ![a trabalhar com o mfaserver-por-mim_iisconfig.](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG) do PNG
8. Passo final: Adicione a conta de serviço MIM aos "PhoneFactor Admins" ![trabalho com o mfaserver-para-mim_addservicetomfaadmin.](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG) do PNG

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>Configurar o serviço MIM para o servidor de autenticação de vários fatores Azure 

### <a name="step-1-patch-server-to-452020"></a>Passo 1: Patch Server para 4.5.202.0
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Passo 2: Backup e Abra as MfaSettings.xml localizadas no "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-3-update-the-following-lines"></a>Passo 3: Atualizar as seguintes linhas
1. Remover/Limpar as seguintes linhas de entradas de configuração <br>
<LICENSE_KEY></LICENSE_KEY><br>
<GROUP_KEY></GROUP_KEY><br>
<CERT_PASSWORD><CERT_PASSWORD><br>
<CertFilePath></CertFilePath><br>

2. Atualizar ou adicionar as seguintes linhas às seguintes Linhas para MfaSettings.xml <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. Reiniciar o serviço MIM e testar a funcionalidade com o Servidor de Autenticação de Multi-Factors Azure.

> [!NOTE] 
> Para reverter a definição substitua MfaSettings.xml com o seu ficheiro de backup no passo 2


## <a name="see-also"></a>Veja também

-    [Começando com o servidor de autenticação multi-factor Azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)
- [O que é a autenticação de multi-factor estoque Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Utilize API de autenticação personalizada de vários fatores para ativar PAM ou SSPR](Working-with-custommfaserver-for-mim.md)
- [História de lançamento da versão MIM](./reference/version-history.md)
