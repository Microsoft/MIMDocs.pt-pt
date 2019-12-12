---
title: Usar o SDK de Servidor de Autenticação Multifator do Azure para ativar cenários do PAM ou SSPR | Microsoft Docs
description: Configure o SDK do Azure Servidor de Autenticação Multifator como uma segunda camada de segurança quando os usuários ativarem funções no Privileged Access Management e na redefinição de senha de autoatendimento.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 69b7f8f4b94f9f94b2aef6afd9573ad8173e148e
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "64517599"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>Usar o Servidor de Autenticação Multifator do Azure para ativar o PAM ou o SSPR
O documento a seguir descreve como configurar o servidor do Azure MFA como uma segunda camada de segurança quando os usuários ativam funções no gerenciamento de acesso de privilégio ou redefinição de senha de autoatendimento.

> [!IMPORTANT]
> Devido ao anúncio de substituição do kit de desenvolvimento de software da autenticação multifator do Azure. O SDK do Azure MFA terá suporte para clientes existentes até a data de desativação de 14 de novembro de 2018. Novos clientes e clientes atuais não poderão baixar mais o SDK por meio do portal clássico do Azure. Para baixar, você precisará entrar em contato com o suporte ao cliente do Azure para receber o pacote de credenciais do serviço MFA gerado. <br> A equipe de desenvolvimento da Microsoft está trabalhando em alterações no MFA, integrando-se ao SDK do Servidor de Autenticação Multifator do Azure.

O artigo abaixo descreverá a atualização de configuração e as etapas para habilitar o para um comutador simples do SDK do Azure MFA para o SDK do Azure Servidor de Autenticação Multifator quando lançado, pois isso será incluído em um hotfix futuro, consulte o [histórico de versões](./reference/version-history.md) para obter comunicados. 

## <a name="prerequisites"></a>Pré-requisitos

Para usar o Azure Servidor de Autenticação Multifator com o MIM, você precisa:

- Acesso à Internet de cada serviço do MIM ou servidor MFA fornecendo PAM e SSPR, para entrar em contato com o serviço do Azure MFA
- Uma subscrição do Azure
- A instalação já está usando o SDK do Azure MFA
- Licenças do Azure Active Directory Premium para utilizadores candidatos ou um meio alternativo de licenciamento do MFA do Azure
- Números de telefone para todos os utilizadores candidatos
- Hotfix do MIM 4,5. ou superior, consulte o [histórico de versão](./reference/version-history.md) para anúncios

## <a name="azure-multi-factor-authentication-server-configuration"></a>Configuração de Servidor de Autenticação Multifator do Azure 
> [!NOTE] 
> Na configuração, você precisará de um certificado SSL válido instalado para o SDK. 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>Etapa 1: baixar Servidor de Autenticação Multifator do Azure no portal do Azure 
Entre no [portal do Azure](https://portal.azure.com/) e baixe o servidor Azure MFA.
![trabalho com o mfaserver-for-mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### <a name="step-2-generate-activation-credentials"></a>Etapa 2: gerar credenciais de ativação
Use o link **gerar credenciais de ativação para iniciar o uso** para gerar credenciais de ativação. Depois de gerada, salve para uso posterior.

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>Etapa 3: instalar o Servidor de Autenticação Multifator do Azure
Depois de baixar o servidor, [Instale](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server) -o.  Suas credenciais de ativação serão necessárias. 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>Etapa 4: criar seu aplicativo Web do IIS que hospedará o SDK
1. Abra o Gerenciador do IIS ![trabalho-com-mfaserver-for-mim_iis.](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG) PNG
2.  Crie uma nova chamada de site "MIM MFASDK", vincule-a a um diretório vazio ![trabalho-com-mfaserver-for-mim_sdkweb.](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG) PNG
3. Abra o console da autenticação multifator e clique em SDK do serviço Web ![trabalho-com-mfaserver-for-mim_sdkinstall.](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG) PNG
4. Depois que os assistentes clicarem na configuração, selecione "MIM MFASDK" e pool de aplicativos

> [!NOTE] 
> O assistente exigirá que um grupo de administradores seja criado. Mais informações podem ser encontradas na documentação do Azure > > MFA do Azure Servidor de Autenticação Multifator.

5. Em seguida, precisamos importar a conta de serviço do MIM abrir console da autenticação multifator selecione "usuários" a. Clique em "importar do Active Directory" b. Navegue até conta de serviço aka "contoso\mimservice" c. Clique em "importar" e em "fechar" ![trabalho-com-mfaserver-for-mim_importmimserviceaccount.](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) PNG 
6. Edite a conta de serviço do MIM para habilitar no console de autenticação multifator ![trabalho-com-mfaserver-for-mim_enableserviceaccount.](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG) PNG
7. Atualize a autenticação do IIS no site "MIM MFASDK". Primeiro, desabilitaremos a "autenticação anônima" e Habilitaremos a autenticação do Windows "![Working-with-mfaserver-for-mim_iisconfig.](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG) PNG
8. Etapa final: Adicione a conta de serviço do MIM ao "PhoneFactor admins" ![trabalho-com-mfaserver-for-mim_addservicetomfaadmin.](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG) PNG

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>Configurando o serviço do MIM para o Azure Servidor de Autenticação Multifator 

### <a name="step-1-patch-server-to-452020"></a>Etapa 1: servidor de patch para 4.5.202.0
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>Etapa 2: faça backup e abra o MfaSettings. xml localizado em "C:\Arquivos de Programas\microsoft Forefront Identity Manager\2010\Service"

### <a name="step-3-update-the-following-lines"></a>Etapa 3: atualizar as seguintes linhas
1. Remover/limpar as seguintes linhas de entradas de configuração <br>
< LICENSE_KEY > </LICENSE_KEY ><br>
< GROUP_KEY > </GROUP_KEY ><br>
< CERT_PASSWORD > </CERT_PASSWORD ><br>
<CertFilePath></CertFilePath><br>

2. Atualize ou adicione as linhas a seguir ao MfaSettings. XML a seguir <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. Reinicie o serviço do MIM e a funcionalidade de teste com o Azure Servidor de Autenticação Multifator.

> [!NOTE] 
> Para reverter a configuração, substitua MfaSettings. xml pelo arquivo de backup na etapa 2


## <a name="next-steps"></a>Próximos Passos

-    [Introdução ao Servidor Multi-Factor Authentication do Azure](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [O que é a autenticação multifator do Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [Usar a API de autenticação multifator personalizada para ativar o PAM ou o SSPR](Working-with-custommfaserver-for-mim.md)
- [Histórico de lançamento de versão do MIM](./reference/version-history.md)
