---
title: Trabalhando com redefinição de senha de autoatendimento | Microsoft Docs
description: Veja as novidades da Reposição Personalizada de Palavra-passe no MIM 2016, incluindo como o SSPR funciona com a autenticação multifator.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/11/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 90452391170114270765e9a7fe08e98eea0747e4
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "67690703"
---
# <a name="self-service-password-reset-deployment-options"></a>Opções de implantação de redefinição de senha de autoatendimento

Para novos clientes [licenciados para Azure Active Directory Premium](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-licensing), é recomendável usar a [redefinição de senha de autoatendimento do Azure ad](/azure/active-directory/authentication/concept-sspr-howitworks) para fornecer a experiência do usuário final.  A redefinição de senha de autoatendimento do Azure AD fornece uma experiência integrada com o Windows e baseada na Web para que um usuário Redefina sua própria senha e dá suporte a muitos dos mesmos recursos que o MIM, incluindo email alternativo e Q & um Gates.  Ao implantar a redefinição de senha de autoatendimento do Azure AD, Azure AD Connect dá suporte à [gravação de novas senhas para AD DS](/azure/active-directory/authentication/concept-sspr-writeback), e o [serviço de notificação de alteração de senha](deploying-mim-password-change-notification-service-on-domain-controller.md) do mim pode ser usado para encaminhar as senhas para outros sistemas, como o servidor de diretório de outro fornecedor também.  A implantação do MIM para o [Gerenciamento de senhas](infrastructure/mim2016-password-management.md) não exige que o serviço do mim ou os portais de registro ou redefinição de senha de autoatendimento do mim sejam implantados.  Em vez disso, você pode seguir estas etapas:

- Primeiro, se você precisar enviar senhas a diretórios diferentes do Azure AD e AD DS, implante a sincronização do MIM com conectores para Active Directory Domain Services e quaisquer sistemas de destino adicionais, configure o MIM para o [Gerenciamento de senhas](infrastructure/mim2016-password-management.md) e implante o serviço de notificação de [alteração de senha](deploying-mim-password-change-notification-service-on-domain-controller.md).
- Em seguida, se você precisar enviar senhas para diretórios diferentes do Azure AD, configure Azure AD Connect para [gravar as novas senhas para AD DS](/azure/active-directory/authentication/concept-sspr-writeback).
- Opcionalmente, [registre os usuários previamente](/azure/active-directory/authentication/howto-sspr-authenticationdata).
- Por fim, distribua [a redefinição de senha de autoatendimento do Azure ad para seus usuários finais](/azure/active-directory/authentication/howto-sspr-deployment).

Para os clientes existentes que implantaram anteriormente o Forefront Identity Manager (FIM) para a redefinição de senha de autoatendimento e são licenciados para Azure Active Directory Premium, recomendamos planejar a transição para a redefinição de senha de autoatendimento do Azure AD.  Você pode fazer a transição dos usuários finais para a redefinição de senha de autoatendimento do Azure AD sem precisar registrar novamente, [sincronizando ou configurando por meio do PowerShell um endereço de email alternativo do usuário ou número de telefone celular](/azure/active-directory/authentication/howto-sspr-authenticationdata). Depois que os usuários são registrados para a redefinição de senha de autoatendimento do Azure AD, o portal de redefinição de senha do FIM pode ser encerrado.

Para clientes que ainda não implantaram a redefinição de senha de autoatendimento do Azure AD para seus usuários, o MIM também fornece portais de redefinição de senha de autoatendimento.  Em comparação com o FIM, o MIM 2016 inclui as seguintes alterações:

- O portal de redefinição de senha de autoatendimento do MIM e a tela de logon do Windows permitem que os usuários desbloqueiem suas contas sem alterar suas senhas.
- Um novo portão de autenticação, portão de telefone, foi adicionado ao MIM. Isso permite a autenticação do usuário por meio de chamada telefônica por meio do serviço de Autenticação Multifator do Microsoft Azure (MFA).

A versão 2016 do MIM se baseia na versão 4.5.26.0 que conta com o cliente para baixar o kit de desenvolvimento de software do Azure MFA (SDK do Azure da autenticação multifator).  Esse SDK foi preterido, e o SDK do Azure MFA terá suporte para clientes existentes somente até a data de desativação de 14 de novembro de 2018. Até essa data, os clientes devem entrar em contato com o suporte ao cliente do Azure para receber o pacote de credenciais do serviço MFA gerado, pois eles não poderão baixar o SDK do Azure MFA. 

#### <a name="new-update-current-azure-mfa-configuration-to-azure-multi-factor-authentication-server"></a>NOVIDADE! Atualizar a configuração atual do Azure MFA para o Azure Servidor de Autenticação Multifator

Este [artigo](working-with-mfaserver-for-mim.md) descreve como atualizar o portal de redefinição de senha de autoatendimento e a configuração do Pam de implantação do mim usando o Azure servidor de autenticação multifator para autenticação multifator.

## <a name="deploying-mim-self-service-password-reset-portal-using-azure-mfa-for-multi-factor-authentication"></a>Implantando o portal de redefinição de senha de autoatendimento do MIM usando o Azure MFA para autenticação multifator

A seção a seguir descreve como implantar o portal de redefinição de senha de autoatendimento do MIM, usando o Azure MFA para autenticação multifator.  Essas etapas só são necessárias para clientes que não estão usando a redefinição de senha de autoatendimento do Azure AD para seus usuários.

O Multi-Factor Authentication do Microsoft Azure é um serviço de autenticação que requer que os utilizadores verifiquem as tentativas de início de sessão com uma aplicação móvel, uma chamada telefónica ou uma mensagem de texto. Está disponível para utilizar com o Microsoft Azure Active Directory e como um serviço para as aplicações empresariais na nuvem e no local.

O Azure MFA fornece um mecanismo de autenticação adicional que pode reforçar os processos de autenticação existentes, tal como o processo realizado pelo MIM para obter assistência de início de sessão personalizada.

Ao utilizar o Azure MFA, os utilizadores autenticam-se no sistema para verificar a sua identidade, enquanto tentam recuperar o acesso às respetivas contas e recursos. Pode efetuar a autenticação por SMS ou através de uma chamada telefónica.   Quanto mais forte for a autenticação, maior será a segurança de que a pessoa que tenta obter acesso é, de facto, o utilizador verdadeiro ao qual pertence a identidade. Uma vez autenticado, o utilizador pode escolher uma nova palavra-passe para substituir a antiga.

## <a name="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa"></a>Pré-requisitos para configurar o desbloqueio personalizado da conta e a reposição de palavra-passe através do MFA

Esta seção pressupõe que você tenha baixado e concluído a implantação do Microsoft Identity Manager 2016 a [sincronização do mim, o serviço do mim e os componentes do portal do mim](microsoft-identity-manager-deploy.md), incluindo os seguintes componentes e serviços:

-   Foi configurado um Windows Server 2008 R2 ou posterior como um servidor do Active Directory, incluindo os Serviços de Domínio do AD e o Controlador de Domínio com um domínio designado (um domínio “empresarial”)

-   Uma Política de Grupo está definida para o Bloqueio da conta

-   O Serviço de Sincronização do MIM 2016 (Sincronização) está instalado e em execução num servidor que está associado ao domínio do AD

-   O Portal e o Serviço MIM 2016, incluindo o Portal de Registo SSPR e o Portal de Reposição SSPR, estão instalados e em execução num servidor (possivelmente, em colocalização com a Sincronização)

-   A Sincronização do MIM está configurada para a sincronização de identidade AD-MIM, incluindo:

    -   Configuração do Agente de Gestão do Active Directory (ADMA) para conetividade ao AD DS e capacidade para importar dados de identidade do Active Directory e respetiva exportação.

    -   Configuração do Agente de Gestão do MIM (MIM MA) para conetividade à Base de Dados do Serviço FIM e capacidade para importar dados de identidade da base de dados do FIM e respetiva exportação.

    -   Configuração das Regras de Sincronização no Portal do MIM para permitir a sincronização dos dados do utilizador e facilitar as atividades baseadas na sincronização no Serviço MIM.

-   As Extensões e os Suplementos do MIM 2016, incluindo o cliente integrado do Início de Sessão do Windows SSPR, estão implementados no servidor ou num computador cliente separado.

Esse cenário exige que você tenha CALs do MIM para seus usuários, bem como assinatura do Azure MFA.

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>Preparar o MIM para funcionar com a autenticação multifator
Configure a Sincronização do MIM para Suportar a Reposição de Palavra-passe e a Funcionalidade de Desbloqueio de Conta. Para obter mais informações, consulte [Instalar Suplementos e Extensões do FIM](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [Instalar SSPR do FIM](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [Portas de Autenticação da SSPR](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) e o [Guia do Laboratório de Teste da SSPR](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx)

Na secção seguinte, irá configurar o fornecedor do Azure MFA no Microsoft Azure Active Directory. Como parte deste processo, irá gerar um ficheiro que inclui o material de autenticação que o MFA necessita para contactar o Azure MFA.  Para continuar, precisará de uma subscrição do Azure.

### <a name="register-your-multi-factor-authentication-provider-in-azure"></a>Registar o fornecedor de autenticação multifator no Azure

1.  Crie um [provedor de MFA](/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider).

2. Abra um caso de suporte e solicite o SDK direto para C#ASP.NET 2,0. O SDK só será fornecido aos usuários atuais do MIM com MFA porque o SDK direto foi preterido. Novos clientes devem adotar a próxima versão do MIM que será integrada ao servidor MFA.

3. Copie o ficheiro ZIP resultante para todos os sistemas onde o serviço MIM estiver instalado.  Tenha em atenção que o ficheiro ZIP contém material para chaves, que é utilizado para efetuar a autenticação no serviço Azure MFA.

### <a name="update-the-configuration-file"></a>Atualizar o ficheiro de configuração

1. Inicie sessão no computador onde o Serviço MIM está instalado, como o utilizador que instalou o MIM.

2. Crie uma nova pasta de diretório localizada abaixo do diretório onde o Serviço MIM foi instalado, tal como **C:\Programas\Microsoft Forefront Identity Manager\2010\Service\MfaCerts**.

3. Através do Explorador do Windows, navegue até à pasta **\pf\certs** do ficheiro ZIP transferido na secção anterior e copie o ficheiro **cert_key.p12** para o novo diretório.

4.  No ficheiro zip SDK, na pasta **\pf**, abra o ficheiro **pf_auth.cs**.

5.  Localize estes três parâmetros: `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD`.

    ![Imagem do código pf_auth.cs](media/MIM-SSPR-pFile.png)

6.  Em **C:\Programas\Microsoft Forefront Identity Manager\2010\Service**, abra o ficheiro: **MfaSettings**.xml.

7.  Copie os valores dos parâmetros `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD` no ficheiro pf_aut.cs para os respetivos elementos xml no ficheiro MfaSettings.xml.

8.  No ficheiro zip SDK, em \pf\certs, extraia o ficheiro **cert_key.p12** e introduza o caminho completo no ficheiro MfaSettings.xml para o elemento xml `<CertFilePath>`.

9. No elemento `<username>`, introduza qualquer nome de utilizador.

10. No elemento `<DefaultCountryCode>`, introduza o seu código de país predefinido. No caso de estarem registados números de telefone para os utilizadores sem um código de país, este é o código de país que irão obter. No caso de um utilizador ter um código de país internacional, tem de ser incluído no número de telefone registado.

11. Guarde o ficheiro MfaSettings.xml com o mesmo nome na mesma localização.

#### <a name="configure-the-phone-gate-or-the-one-time-password-sms-gate"></a>Configurar a Porta de telefone ou a Porta de SMS para Palavra-passe Monouso

1.  Inicie o Internet Explorer e navegue para o Portal do MIM, efetue a autenticação como o administrador do MIM e clique em **Fluxos de Trabalho** na barra de navegação esquerda.

    ![Imagem de navegação do Portal do MIM](media/MIM-SSPR-workflow.jpg)

2.  Selecione **Fluxo de Trabalho de AuthN de Reposição de Palavra-passe**.

    ![Imagem de Fluxos de Trabalho do Portal do MIM](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  Clique no separador **Atividades** e desloque ecrã para baixo até **Adicionar Atividade**.

4.  Selecione **Porta de Telefone** ou **Porta de SMS para Palavra-passe Monouso**, clique em **Selecione** e, em seguida, em **OK**.

Observação: se estiver usando o servidor do Azure MFA ou outro provedor que gera a senha de uso único, verifique se o campo de comprimento configurado acima tem o mesmo comprimento que o gerado pelo provedor de MFA.  Esse comprimento deve ser 6 para o servidor do Azure MFA.  O servidor MFA do Azure também gera seu próprio texto de mensagem para que a mensagem de texto SMS seja ignorada.

Os utilizadores na sua organização podem agora registar-se para a reposição de palavra-passe.  Durante este processo, deverão introduzir o respetivo número de telefone do trabalho ou do telemóvel para que o sistema possa contactá-los (ou enviar mensagens SMS).

#### <a name="register-users-for-password-reset"></a>Registar utilizadores para a reposição de palavra-passe

1.  O utilizador deverá iniciar um browser para navegar até ao Portal de Registo de Reposição de Palavra-passe do MIM.  (Geralmente, este portal será configurado com a autenticação do Windows.)  No portal, os utilizadores deverão fornecer o nome de utilizador e a palavra-passe novamente para confirmar a identidade.

    Têm de aceder ao Portal de Registo de Palavra-passe e efetuar a autenticação através do respetivo nome de utilizador e palavra-passe.

2.  No campo **Número de Telefone** ou **Telemóvel**, têm de introduzir um código de país, um espaço e o número de telefone e clicar em **Seguinte**.

    ![Imagem de Verificação do Telefone do MIM](media/MIM-SSPR-PhoneVerification.JPG)

    ![Imagem de Verificação do Telemóvel do MIM](media/MIM-SSPR-mobilephoneverification.JPG)

## <a name="how-does-it-work-for-your-users"></a>Como é que funciona para os seus utilizadores?
Agora que está tudo configurado e em execução, deve querer saber o que os seus utilizadores terão de fazer quando repõem as palavras-passe antes das férias e, quando regressam, não conseguem lembrar-se delas.

Existem duas formas através das quais um utilizador pode utilizar a funcionalidade de desbloqueio da conta e de reposição de palavra-passe: a partir do ecrã de início de sessão do Windows ou do Portal Self-Service.

Ao instalar os Suplementos e as Extensões do MIM num computador associado a um domínio ligado através da rede organizacional ao Serviço MIM, os utilizadores podem recuperar uma palavra-passe esquecida na experiência de início de sessão do ambiente de trabalho.  Os seguintes passos guiá-lo-ão através do processo.

#### <a name="windows-desktop-login-integrated-password-reset"></a>Reposição de palavra-passe integrada do início de sessão do ambiente de trabalho do Windows

1.  Se o utilizador introduzir uma palavra-passe errada várias vezes, no ecrã de início de sessão, terá a opção de clicar em **Não consegue iniciar sessão?** .

    ![Imagem do ecrã de início de sessão](media/MIM-SSPR-problemsloggingin.JPG)

    Ao clicar nesta ligação, o utilizador é reencaminhado para o ecrã Reposição de Palavra-passe do MIM, onde pode alterar a palavra-passe ou desbloquear a conta.

    ![Imagem de Reposição de Palavra-passe do MIM](media/MIM-SSPR-keepcurrentorsetnewpwd.JPG)

2.  O utilizador será direcionado para a autenticação. Se o MFA tiver sido configurado, o utilizador irá receber uma chamada telefónica.

3.  Em segundo plano, o Azure MFA efetua uma chamada telefónica para o número que o utilizador forneceu quando se inscreveu no serviço.

4.  Quando um utilizador atender o telemóvel, ser-lhe-á pedido que prima a tecla cardinal # no telemóvel. Em seguida, o utilizador clica em **Seguinte** no portal.

    Se também configurar outras portas, será pedido ao utilizador que forneça mais informações em ecrãs subsequentes.

    > [!NOTE]
    > Se o utilizador for impaciente e clicar em **Seguinte** antes de premir a tecla cardinal #, a autenticação falha.

5.  Após a autenticação com êxito, o utilizador terá duas opções: desbloquear a conta e manter a palavra-passe atual ou definir uma nova.

6.  Em seguida, o utilizador tem de introduzir uma nova palavra-passe duas vezes e a palavra-passe é reposta.

#### <a name="access-from-the-self-service-portal"></a>Aceder a partir do Portal Self-Service

1.  Os utilizadores podem abrir um browser, navegar até ao **Portal de Reposição de Palavra-passe**, introduzir o respetivo nome de utilizador e clicar em **Seguinte**.

    Se o MFA tiver sido configurado, o utilizador irá receber uma chamada telefónica. Em segundo plano, o Azure MFA efetua uma chamada telefónica para o número que o utilizador forneceu quando se inscreveu no serviço.

    Quando um utilizador atender o telemóvel, ser-lhe-á pedido que prima a tecla cardinal # no telemóvel. Em seguida, o utilizador clica em **Seguinte** no portal.

2.  Se também configurar outras portas, será pedido ao utilizador que forneça mais informações em ecrãs subsequentes.

    > [!NOTE]
    > Se o utilizador for impaciente e clicar em **Seguinte** antes de premir a tecla cardinal #, a autenticação falha.

3.  O utilizador terá de escolher se pretende repor a palavra-passe ou desbloquear a conta. Se escolher desbloquear a conta, a conta será desbloqueada.

    ![Imagem de Desbloqueio da Conta do Assistente de Início de Sessão do MIM](media/MIM-SSPR-accountUnlock.JPG)

4.  Após a autenticação com êxito, o utilizador terá duas opções: manter a palavra-passe atual ou definir uma nova.

5.  ! [MIM AC
6.  contar imagem de êxito desbloqueada] (mídia/MIM-SSPR-account-unlock. JPG)

6.  Se o utilizador escolher repor a palavra-passe, terá de escrever uma nova palavra-passe duas vezes e clicar em **Seguinte** para alterar a palavra-passe.
