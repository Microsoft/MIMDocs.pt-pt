---
title: Trabalhar com o portal de Reposição Personalizada de Palavra-passe | Documentos da Microsoft
description: Veja as novidades da Reposição Personalizada de Palavra-passe no MIM 2016, incluindo como o SSPR funciona com a autenticação multifator.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.reviewer: davidste
ms.date: 06/26/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: b1b30b744a5f735512f31d98184a561ce3f9b047
ms.sourcegitcommit: 03617b441135a55b664e0d81cce4d17541bee93b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/26/2018
ms.locfileid: "36963380"
---
# <a name="working-with-self-service-password-reset"></a>Trabalhar com a Reposição Personalizada de Palavra-passe

> [!IMPORTANT]
> Devido ao anúncio de preterição do multi-factor Authentication Software Development Kit do Azure. O SDK de MFA do Azure será suportado para os clientes existentes até a data de retirada de 14 de Novembro de 2018. Novos clientes e os clientes atuais serão não é possível transferir o SDK mais através do portal clássico do Azure. Para transferir terá de contactar o suporte de cliente do Azure para receber o seu pacote de credenciais do serviço de MFA gerado. <br> A equipe de desenvolvimento da Microsoft está trabalhando em alterações para a MFA através da integração com o SDK do servidor MFA.  Isso será incluído numa futura correção, consulte [histórico de versões](/reference/version-history.md) para anúncios.

O Microsoft Identity Manager 2016 fornece funcionalidades adicionais para a opção Reposição Personalizada de Palavra-passe. Esta opção foi melhorada com várias funcionalidades importantes:

-   O portal de reposição de palavra-passe Self-Service e o ecrã de início de sessão do Windows agora permitem que os utilizadores desbloqueiem as respetivas contas sem alterar as palavras-passe ou sem os administradores de suporte. Os utilizadores podem obter com as contas bloqueados por variados motivos legítimos, como se eles introduzam uma palavra-passe antiga, utilizarem computadores bilingues e ter o teclado definido para o idioma errado ou a tentativa de início de sessão para uma estação de trabalho partilhada já aberta para alguém de outra conta.

-   Foi adicionada a nova porta de autenticação Porta de Telefone. Esta porta permite a autenticação de utilizador através de uma chamada telefónica.

-   Foi adicionado suporte para o serviço do Microsoft Azure multi-factor Authentication (MFA). Este serviço pode ser utilizado para qualquer uma das existentes SMS porta de uma-palavra-passe ou a nova porta de telefone.

## <a name="azure-for-multi-factor-authentication"></a>Multi-Factor Authentication do Azure
O Multi-Factor Authentication do Microsoft Azure é um serviço de autenticação que requer que os utilizadores verifiquem as tentativas de início de sessão com uma aplicação móvel, uma chamada telefónica ou uma mensagem de texto. Está disponível para utilizar com o Microsoft Azure Active Directory e como um serviço para as aplicações empresariais na nuvem e no local.

O Azure MFA fornece um mecanismo de autenticação adicional que pode reforçar os processos de autenticação existentes, tal como o processo realizado pelo MIM para obter assistência de início de sessão personalizada.

Ao utilizar o Azure MFA, os utilizadores autenticam-se no sistema para verificar a sua identidade, enquanto tentam recuperar o acesso às respetivas contas e recursos. Pode efetuar a autenticação por SMS ou através de uma chamada telefónica.   Quanto mais forte for a autenticação, maior será a segurança de que a pessoa que tenta obter acesso é, de facto, o utilizador verdadeiro ao qual pertence a identidade. Uma vez autenticado, o utilizador pode escolher uma nova palavra-passe para substituir a antiga.

## <a name="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa"></a>Pré-requisitos para configurar o desbloqueio personalizado da conta e a reposição de palavra-passe através do MFA
Esta secção assume que transferiu e concluiu a implementação do Microsoft Identity Manager 2016, incluindo os seguintes componentes e serviços:

-   Foi configurado um Windows Server 2008 R2 ou posterior como um servidor do Active Directory, incluindo os Serviços de Domínio do AD e o Controlador de Domínio com um domínio designado (um domínio “empresarial”)

-   Uma Política de Grupo está definida para o Bloqueio da conta

-   O Serviço de Sincronização do MIM 2016 (Sincronização) está instalado e em execução num servidor que está associado ao domínio do AD

-   O Portal e o Serviço MIM 2016, incluindo o Portal de Registo SSPR e o Portal de Reposição SSPR, estão instalados e em execução num servidor (possivelmente, em colocalização com a Sincronização)

-   A Sincronização do MIM está configurada para a sincronização de identidade AD-MIM, incluindo:

    -   Configuração do Agente de Gestão do Active Directory (ADMA) para conetividade ao AD DS e capacidade para importar dados de identidade do Active Directory e respetiva exportação.

    -   Configuração do Agente de Gestão do MIM (MIM MA) para conetividade à Base de Dados do Serviço FIM e capacidade para importar dados de identidade da base de dados do FIM e respetiva exportação.

    -   Configuração das Regras de Sincronização no Portal do MIM para permitir a sincronização dos dados do utilizador e facilitar as atividades baseadas na sincronização no Serviço MIM.

-   MIM 2016 e os suplementos &amp; extensões, incluindo o Windows SSPR cliente integrado do início de sessão é implementado no servidor ou num computador cliente separado.

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>Preparar o MIM para funcionar com a autenticação multifator
Configure a Sincronização do MIM para Suportar a Reposição de Palavra-passe e a Funcionalidade de Desbloqueio de Conta. Para obter mais informações, consulte [Instalar Suplementos e Extensões do FIM](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [Instalar SSPR do FIM](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [Portas de Autenticação da SSPR](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) e o [Guia do Laboratório de Teste da SSPR](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx)

Na secção seguinte, irá configurar o fornecedor do Azure MFA no Microsoft Azure Active Directory. Irá gerar um ficheiro que inclui o material de autenticação que o MFA necessita para ser capaz de contactar o MFA do Azure.  Para continuar, precisará de uma subscrição do Azure.

### <a name="register-your-multi-factor-authentication-provider-in-azure"></a>Registar o fornecedor de autenticação multifator no Azure

1.  Criar uma [fornecedor de MFA](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider).

2. Abra um pedido de suporte e pedir o SDK direto para o ASP.net 2.0 c#. O SDK apenas será fornecido para que os utilizadores atuais de MIM com a MFA porque o SDK direct foi preterido. Novos clientes devem adotar a próxima versão de MIM que se integrará ao servidor MFA.

3.  Clique em **Serviços Aplicacionais &gt; Active Directory &gt; Fornecedor de Autenticação Multifator &gt; Criação Rápida**.

![Portais do Azure rápidos criar a imagem do MFA](media/MIM-SSPR-Azureportal.png)

4.  No campo **Nome**, introduza **SSPRMFA** e clique em **Criar**.

![Imagem do MFA do Portal do Azure](media/MIM-SSPR-Azureportal-2.png)

5.  Clique em **Active Directory**, no menu do portal clássico do Azure e, em seguida, no separador **Fornecedores de Autenticação Multifator**.

6.  Clique no **SSPRMFA** e, em seguida, em **Gerir** na parte inferior do ecrã.

    ![Ícone Gerir do portal do Azure](media/MIM-SSPR-ManageButton.png)

7.  Na nova janela, no painel esquerdo abaixo de **Configurar**, clique em **Definições**.

8.  Sob **alerta de fraude**, desmarque a opção * * Bloquear utilizador quando é reportada fraude. Ao desmarcar a caixa está evitar bloquear todo o serviço.

9. Na janela **Multi-Factor Authentication do Azure** apresentada, clique em **SDK**, abaixo de **Transferências**, no menu à esquerda.

10. Clique na ligação **Transferir** na coluna ZIP do ficheiro com o idioma **SDK para ASP.net 2.0 C#**.

    ![Imagem do ficheiro zip de transferência do Azure MFA](media/MIM-SSPR-Azure-MFA.png)

11. Copie o ficheiro ZIP resultante para todos os sistemas onde o serviço MIM estiver instalado.  Tenha em atenção que o ficheiro ZIP contém material para chaves, que é utilizado para efetuar a autenticação no serviço Azure MFA.

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

10. No elemento `<DefaultCountryCode>`, introduza o seu código de país predefinido. Em casos em que os números de telefone são registrados para utilizadores sem um código de país, os utilizadores receberão este código. Em casos em que um utilizador tem um código de país internacional, ele deve ser incluído no número de telefone registado.

11. Guarde o ficheiro MfaSettings.xml com o mesmo nome na mesma localização.

#### <a name="configure-the-phone-gate-or-the-one-time-password-sms-gate"></a>Configurar a Porta de telefone ou a Porta de SMS para Palavra-passe Monouso

1.  Inicie o Internet Explorer e navegue para o Portal de MIM, efetue a autenticação como o administrador de MIM, em seguida, clique em **fluxos de trabalho** na barra de navegação esquerdo.

    ![Imagem de navegação do Portal do MIM](media/MIM-SSPR-workflow.jpg)

2.  Selecione **Fluxo de Trabalho de AuthN de Reposição de Palavra-passe**.

    ![Imagem de Fluxos de Trabalho do Portal do MIM](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  Clique no separador **Atividades** e desloque ecrã para baixo até **Adicionar Atividade**.

4.  Selecione **Porta de Telefone** ou **Porta de SMS para Palavra-passe Monouso**, clique em **Selecione** e, em seguida, em **OK**.

Os utilizadores na sua organização podem agora registar-se para a reposição de palavra-passe.  Durante este processo, deverão introduzir o respetivo número de telefone do trabalho ou do telemóvel para que o sistema possa contactá-los (ou enviar mensagens SMS).

#### <a name="register-users-for-password-reset"></a>Registar utilizadores para a reposição de palavra-passe

1.  Um utilizador irá iniciar um browser e navegue para o Portal de registo de reposição de MIM palavra-passe.  (Geralmente, este portal será configurado com a autenticação do Windows.)  No portal, os utilizadores deverão fornecer o nome de utilizador e a palavra-passe novamente para confirmar a identidade.

    Têm de aceder ao Portal de Registo de Palavra-passe e efetuar a autenticação através do respetivo nome de utilizador e palavra-passe.

2.  No campo **Número de Telefone** ou **Telemóvel**, têm de introduzir um código de país, um espaço e o número de telefone e clicar em **Seguinte**.

    ![Imagem de Verificação do Telefone do MIM](media/MIM-SSPR-PhoneVerification.JPG)

    ![Imagem de Verificação do Telemóvel do MIM](media/MIM-SSPR-mobilephoneverification.JPG)

## <a name="how-does-it-work-for-your-users"></a>Como é que funciona para os seus utilizadores?
Agora que está tudo configurado e em execução, deve querer saber o que os seus utilizadores terão de fazer quando repõem as palavras-passe antes das férias e, quando regressam, não conseguem lembrar-se delas.

Existem duas formas através das quais um utilizador pode utilizar a funcionalidade de desbloqueio da conta e de reposição de palavra-passe: a partir do ecrã de início de sessão do Windows ou do Portal Self-Service.

Ao instalar os Suplementos e as Extensões do MIM num computador associado a um domínio ligado através da rede organizacional ao Serviço MIM, os utilizadores podem recuperar uma palavra-passe esquecida na experiência de início de sessão do ambiente de trabalho.  Os seguintes passos guiá-lo-ão através do processo.

#### <a name="windows-desktop-login-integrated-password-reset"></a>Redefinição de senha de integrada de início de sessão de ambiente de trabalho do Windows

1.  Se o utilizador introduzir uma palavra-passe errada várias vezes, no ecrã de início de sessão, terá a opção de clicar em **Não consegue iniciar sessão?** .

    ![Imagem do ecrã de início de sessão](media/MIM-SSPR-problemsloggingin.JPG)

    Ao clicar nesta ligação, o utilizador é reencaminhado para o ecrã Reposição de Palavra-passe do MIM, onde pode alterar a palavra-passe ou desbloquear a conta.

    ![Imagem de Reposição de Palavra-passe do MIM](media/MIM-SSPR-keepcurrentorsetnewpwd.JPG)

2.  O utilizador será direcionado para a autenticação. Se o MFA tiver sido configurado, o utilizador irá receber uma chamada telefónica.

3.  Em segundo plano, o que acontece é que MFA do Azure, em seguida, os lugares uma chamada telefónica para o número o usuário forneceu quando se inscreveram para o serviço.

4.  Quando um utilizador atender o telemóvel, ser-lhe-á pedido que prima a tecla cardinal # no telemóvel. Em seguida, o utilizador clica em **Seguinte** no portal.

    Se também configurar outras portas, será pedido ao utilizador que forneça mais informações em ecrãs subsequentes.

    > [!NOTE]
    > Se o utilizador for impaciente e clicar em **Seguinte** antes de premir a tecla cardinal #, a autenticação falha.

5.  Após a autenticação com êxito, o utilizador terá duas opções: desbloquear a conta e manter a palavra-passe atual ou definir uma nova.

6.  Em seguida, o utilizador tem de introduzir uma nova palavra-passe duas vezes e a palavra-passe é reposta.

#### <a name="access-from-the-self-service-portal"></a>Aceder a partir do Portal Self-Service

1.  Os utilizadores podem abrir um browser, navegar até ao **Portal de Reposição de Palavra-passe**, introduzir o respetivo nome de utilizador e clicar em **Seguinte**.

    Se o MFA tiver sido configurado, o utilizador irá receber uma chamada telefónica. Em segundo plano, o que acontece é que MFA do Azure, em seguida, os lugares uma chamada telefónica para o número o usuário forneceu quando se inscreveram para o serviço.

    Quando um utilizador atender o telemóvel, ser-lhe-á pedido que prima a tecla cardinal # no telemóvel. Em seguida, o utilizador clica em **Seguinte** no portal.

2.  Se também configurar outras portas, será pedido ao utilizador que forneça mais informações em ecrãs subsequentes.

    > [!NOTE]
    > Se o utilizador for impaciente e clicar em **Seguinte** antes de premir a tecla cardinal #, a autenticação falha.

3.  O utilizador terá de escolher se pretende repor a palavra-passe ou desbloquear a conta. Se escolher desbloquear a conta, a conta será desbloqueada.

    ![Imagem de Desbloqueio da Conta do Assistente de Início de Sessão do MIM](media/MIM-SSPR-accountUnlock.JPG)

4.  Após a autenticação com êxito, o utilizador terá duas opções: manter a palavra-passe atual ou definir uma nova.

5.  ![Imagem da conta do MIM desbloqueada com êxito](media/MIM-SSPR-account-unlock.JPG)

6.  Se o utilizador escolher repor a palavra-passe, terá de escrever uma nova palavra-passe duas vezes e clicar em **Seguinte** para alterar a palavra-passe.

    ![Imagem de Reposição de Palavra-passe do Assistente de Início de Sessão do MIM](media/MIM-SSPR-PR1.JPG)
