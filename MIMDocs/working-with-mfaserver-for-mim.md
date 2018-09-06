---
title: Utilizar o SDK de servidor multi-factor Authentication do Azure para ativar o PAM ou cenários de SSPR | Documentos da Microsoft
description: Configure a SDK de servidor multi-factor Authentication do Azure como uma segunda camada de segurança, quando os utilizadores ativarem funções de Privileged Access Management e a reposição personalizada de palavra-passe.
keywords: ''
author: fimguy
ms.author: billmath
manager: mtillman
ms.date: 09/02/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 7191e445688cc9e3c5c02b9c6852c869a28a937a
ms.sourcegitcommit: ad0690bd57e3d056397108bf1c8417965d69a32c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43772683"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>Utilizar o servidor do Azure multi-factor Authentication para ativar o PAM ou SSPR
O documento seguinte descreve como configurar o servidor MFA do Azure como uma segunda camada de segurança quando os utilizadores ativarem funções de gestão de privilégios de acesso ou a reposição de palavra-passe Self-Service.

> [!IMPORTANT]
> Devido ao anúncio de preterição do multi-factor Authentication Software Development Kit do Azure. O SDK de MFA do Azure será suportado para os clientes existentes até a data de retirada de 14 de Novembro de 2018. Novos clientes e os clientes atuais não será capazes de transferir o SDK mais através do portal clássico do Azure. Para transferir terá de contactar o suporte de cliente do Azure para receber o seu pacote de credenciais do serviço de MFA gerado. <br> A equipe de desenvolvimento da Microsoft está trabalhando em alterações para a MFA através da integração com o SDK de servidor multi-factor Authentication do Azure.

No artigo abaixo descrevem a atualização de configuração e os passos para ativar para um comutador simple do SDK de MFA do Azure para o Azure multi-factor Authentication Server SDK quando lançada como a isso será incluído numa futura correção consulte [histórico de versões ](/reference/version-history.md) para anúncios. 

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar o servidor do Azure multi-factor Authentication com o MIM, terá de:

- Acesso à Internet de cada serviço MIM ou o servidor de MFA fornece o PAM e o SSPR, para contactar o serviço de MFA do Azure
- Uma subscrição do Azure
- Instalação já está a utilizar o SDK de MFA do Azure
- Licenças do Azure Active Directory Premium para utilizadores candidatos ou um meio alternativo de licenciamento do MFA do Azure
- Números de telefone para todos os utilizadores candidatos
- Correção MIM 4.5. ou consulte maior [histórico de versões](/reference/version-history.md) anúncios de

## <a name="azure-multi-factor-authentication-server-configuration"></a>Configuração do servidor multi-factor Authentication 
> [!NOTE] 
> A configuração terá um certificado SSL válido instalado para o SDK. 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Passo 1: Transferir o servidor do Azure multi-factor Authentication no portal do azure 
Início de sessão para o [portal do Azure](https://portal.azure.com/) e transferir o servidor MFA do Azure.
![trabalhar com mfaserver para mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### <a name="step-2-generate-activation-credentials"></a>Passo 2: Gerar credenciais de ativação
Utilizar o **gerar credenciais de ativação para iniciar a utilização** ligação para gerar credenciais de ativação. Depois de gerada guardar para utilização posterior.

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>Passo 3: Instalar o servidor multi-factor Authentication
Depois de transferir o servidor [instalar](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server) -lo.  As suas credenciais de ativação será necessárias. 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>Passo 4: Criar o aplicativo Web do IIS que irá alojar o SDK
1. Abra o Gestor de IIS ![trabalhar com mfaserver para mim_iis. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  Criar novo Web site chamada "MFASDK de MIM", ligá-lo num diretório vazio ![trabalhar com mfaserver para mim_sdkweb. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Abra a consola do multi-factor Authentication e clique no SDK do serviço Web ![trabalhar com mfaserver para mim_sdkinstall. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. Uma vez assistentes, clique em através de configuração, selecione "MFASDK de MIM" e conjunto de aplicações

> [!NOTE] 
> Assistente exigirá um grupo de administrador a ser criada. Podem encontrar mais informações sobre o Azure >> documentação do servidor do MFA do Azure multi-factor Authentication.

5. Em seguida, precisamos importar de serviço MIM conta abrir consola de autenticação Multifator, selecione "Utilizadores" um. Clique em "Importar do Active Directory" b. Navegue para a conta de serviço também conhecido como "contoso\mimservice" c. Clique em "Importar" e "Fechar" ![trabalhar com mfaserver para mim_importmimserviceaccount. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) 
6. Editar a conta de serviço MIM para ativar na consola do multi-factor Authentication ![trabalhar com mfaserver para mim_enableserviceaccount. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
7. Atualize a autenticação do IIS no site da "MFASDK de MIM". Em primeiro lugar será desativada a "Autenticação anónima", em seguida, ativar a autenticação do Windows" ![trabalhar com mfaserver para mim_iisconfig. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
8. A etapa final: Adicione a conta de serviço MIM para o "PhoneFactor Admins" ![trabalhar com mfaserver para mim_addservicetomfaadmin. PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>Configurar o serviço MIM para o servidor multi-factor Authentication 

### <a name="step-1-patch-server-to-452020"></a>Passo 1: O servidor de Patch para 4.5.202.0
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Passo 2: Cópia de segurança e abra o mfasettings. XML localizado no "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service"

### <a name="step-3-update-the-following-lines"></a>Passo 3: Atualizar as seguintes linhas
1. Remover/limpar as seguintes linhas de entradas de configuração <br>
&LT; CHAVE_LICENÇA &GT;&LT; / CHAVE_LICENÇA &GT;<br>
&LT; GROUP_KEY &GT;&LT; / GROUP_KEY &GT;<br>
&LT; CERT_PASSWORD &GT;&LT; / CERT_PASSWORD &GT;<br>
<CertFilePath></CertFilePath><br>

2. Atualize ou adicione as seguintes linhas para o seguinte procedimento para mfasettings. XML <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. Reinicie o serviço MIM e testar a funcionalidade com o servidor do Azure multi-factor Authentication.

> [!NOTE] 
> Para reverter definição substituir mfasettings. XML com o ficheiro de cópia de segurança no passo 2


## <a name="next-steps"></a>Passos Seguintes

-    [Introdução ao servidor de autenticação do multi-factor do Azure](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [O que é o Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Utilize a API personalizada multi-factor Authentication para ativar o PAM ou SSPR](Working-with-custommfaserver-for-mim.md)
- [Histórico de lançamento de versão de MIM](./reference/version-history.md)