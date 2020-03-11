---
title: Trabalhar com a Palavra-passe self-service reset  Microsoft Docs
description: Veja as novidades da Reposição Personalizada de Palavra-passe no MIM 2016, incluindo como o SSPR funciona com a autenticação multifator.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 05/11/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 41aba931111d6ef46e60dfed173362e59c411dfe
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044281"
---
# <a name="self-service-password-reset-deployment-options"></a>Opções de implementação de reset de palavra-passe self-service

Para novos clientes licenciados [para o Azure Ative Directory Premium,](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-licensing)recomendamos a utilização de [uma redefinição da palavra-passe autosserviço Azure AD](/azure/active-directory/authentication/concept-sspr-howitworks) para proporcionar a experiência do utilizador final.  O reset de palavras-passe autosserviço Azure AD fornece uma experiência baseada na Web e no Windows para um utilizador redefinir a sua própria palavra-passe, e suporta muitas das mesmas capacidades que mim, incluindo e-mails alternativos e portões de Q&A.  Ao implementar o reset de palavras-passe de autosserviço Azure AD, o Azure AD Connect suporta a [reescrita das novas palavras-passe para AD DS](/azure/active-directory/authentication/concept-sspr-writeback), e o Serviço de Notificação de Alterações de [Palavras-Passe](deploying-mim-password-change-notification-service-on-domain-controller.md) MIM pode ser usado para encaminhar as palavras-passe para outros sistemas, como o servidor de diretório de outro fornecedor, também.  A implementação de MIM para gestão de [passwords](infrastructure/mim2016-password-management.md) não requer a implementação do Serviço MIM ou do reset ou dos portais de registo do autosserviço MIM.  Em vez disso, pode seguir estes passos:

- Em primeiro lugar, se precisar de enviar senhas para diretórios que não o Azure AD e a AD DS, implemente o MIM Sync com conectores para Serviços de Domínio de Diretório Ativo e quaisquer sistemas de destino adicionais, configure mim para gestão de [passwords](infrastructure/mim2016-password-management.md) e implemente o Serviço de Notificação de [Alterações de Passwords.](deploying-mim-password-change-notification-service-on-domain-controller.md)
- Em seguida, se precisar de enviar senhas para diretórios que não o Azure AD, configure o Azure AD Connect para [reescrever as novas palavras-passe para AD DS](/azure/active-directory/authentication/concept-sspr-writeback).
- Opcionalmente, [utilizadores pré-registo.](/azure/active-directory/authentication/howto-sspr-authenticationdata)
- Por fim, envie a [palavra-passe de autosserviço Azure AD para os utilizadores finais](/azure/active-directory/authentication/howto-sspr-deployment).

Para os clientes existentes que já tinham implementado o Forefront Identity Manager (FIM) para reposição de passwords de self-service e que estão licenciados para o Azure Ative Directory Premium, recomendamos o planeamento da transição para o reset de senha de autosserviço da Azure AD.  Pode transitar os utilizadores finais para o reset da palavra-passe de autosserviço da AD Azure sem precisar que se registem novamente, [sincronizando ou definindo através do endereço de e-mail alternativo de um utilizador ou número](/azure/active-directory/authentication/howto-sspr-authenticationdata)de telemóvel . Depois de os utilizadores estarem registados para reset de palavra-passe autosserviço Azure AD, o portal de reset de palavra-passe FIM pode ser desativado.

Para os clientes, que ainda não implementaram o reset de senha de autosserviço da Azure AD para os seus utilizadores, a MIM também fornece portais de redefinição de senha de autosserviço.  Em comparação com o FIM, mim 2016 inclui as seguintes alterações:

- O portal de reset de palavras-passe autosserviço MIM e o ecrã de login do Windows permitem que os utilizadores desbloqueiem as suas contas sem alterarem as suas palavras-passe.
- Um novo portão de autenticação, Phone Gate, foi adicionado à MIM. Isto permite a autenticação do utilizador através de uma chamada telefónica através do serviço de autenticação multi-factor (MFA) do Microsoft Azure.

O lançamento do MIM 2016 acumula-se até à versão 4.5.26.0 contacom o cliente para descarregar o Kit de Desenvolvimento de Software de Autenticação Multifactor Azure (Azure MFA SDK).  O SDK foi depreciado e o Azure MFA SDK será suportado para os clientes existentes apenas até à data de reforma de 14 de novembro de 2018. Até essa data, os clientes devem contactar o suporte ao cliente da Azure para receber o seu pacote de Credenciais de Serviço MFA gerado, uma vez que não poderão descarregar o Azure MFA SDK. 

#### <a name="new-update-current-azure-mfa-configuration-to-azure-multi-factor-authentication-server"></a>NOVIDADE! Atualizar a configuração atual do Azure MFA para o Servidor de Autenticação de Multi-Factors Azure

Este [artigo](working-with-mfaserver-for-mim.md) descreve como atualizar o portal de redefinição de palavra-passe autosserviço MIM de implementação e configuração PAM, utilizando o Servidor de Autenticação Multi-Factor Azure para autenticação de vários fatores.

## <a name="deploying-mim-self-service-password-reset-portal-using-azure-mfa-for-multi-factor-authentication"></a>Implementação do portal de reset de palavra-passe self-service MIM utilizando o Azure MFA para autenticação multi-factor

A secção seguinte descreve como implementar o portal de redefinição de senha de autosserviço MIM, utilizando o Azure MFA para a autenticação de vários fatores.  Estes passos são apenas necessários para os clientes que não estão a utilizar o reset de senha de autosserviço da Azure AD para os seus utilizadores.

O Multi-Factor Authentication do Microsoft Azure é um serviço de autenticação que requer que os utilizadores verifiquem as tentativas de início de sessão com uma aplicação móvel, uma chamada telefónica ou uma mensagem de texto. Está disponível para utilizar com o Microsoft Azure Active Directory e como um serviço para as aplicações empresariais na nuvem e no local.

O Azure MFA fornece um mecanismo de autenticação adicional que pode reforçar os processos de autenticação existentes, tal como o processo realizado pelo MIM para obter assistência de início de sessão personalizada.

Ao utilizar o Azure MFA, os utilizadores autenticam-se no sistema para verificar a sua identidade, enquanto tentam recuperar o acesso às respetivas contas e recursos. Pode efetuar a autenticação por SMS ou através de uma chamada telefónica.   Quanto mais forte for a autenticação, maior será a segurança de que a pessoa que tenta obter acesso é, de facto, o utilizador verdadeiro ao qual pertence a identidade. Uma vez autenticado, o utilizador pode escolher uma nova palavra-passe para substituir a antiga.

## <a name="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa"></a>Pré-requisitos para configurar o desbloqueio personalizado da conta e a reposição de palavra-passe através do MFA

Esta secção pressupõe que descarregou e concluiu a implementação dos componentes MIM Sync, MIM Service 2016 do Microsoft Identity [Manager, mim Service e mim Portal,](microsoft-identity-manager-deploy.md)incluindo os seguintes componentes e serviços:

-   Foi configurado um Windows Server 2008 R2 ou posterior como um servidor do Active Directory, incluindo os Serviços de Domínio do AD e o Controlador de Domínio com um domínio designado (um domínio “empresarial”)

-   Uma Política de Grupo está definida para o Bloqueio da conta

-   O Serviço de Sincronização do MIM 2016 (Sincronização) está instalado e em execução num servidor que está associado ao domínio do AD

-   O Portal e o Serviço MIM 2016, incluindo o Portal de Registo SSPR e o Portal de Reposição SSPR, estão instalados e em execução num servidor (possivelmente, em colocalização com a Sincronização)

-   A Sincronização do MIM está configurada para a sincronização de identidade AD-MIM, incluindo:

    -   Configuração do Agente de Gestão do Active Directory (ADMA) para conetividade ao AD DS e capacidade para importar dados de identidade do Active Directory e respetiva exportação.

    -   Configuração do Agente de Gestão do MIM (MIM MA) para conetividade à Base de Dados do Serviço FIM e capacidade para importar dados de identidade da base de dados do FIM e respetiva exportação.

    -   Configuração das Regras de Sincronização no Portal do MIM para permitir a sincronização dos dados do utilizador e facilitar as atividades baseadas na sincronização no Serviço MIM.

-   As Extensões e os Suplementos do MIM 2016, incluindo o cliente integrado do Início de Sessão do Windows SSPR, estão implementados no servidor ou num computador cliente separado.

Este cenário requer que tenha CALs MIM para os seus utilizadores, bem como subscrição para O MFA Azure.

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>Preparar o MIM para funcionar com a autenticação multifator
Configure a Sincronização do MIM para Suportar a Reposição de Palavra-passe e a Funcionalidade de Desbloqueio de Conta. Para obter mais informações, consulte [Instalar Suplementos e Extensões do FIM](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx), [Instalar SSPR do FIM](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx), [Portas de Autenticação da SSPR](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx) e o [Guia do Laboratório de Teste da SSPR](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx)

Na secção seguinte, irá configurar o fornecedor do Azure MFA no Microsoft Azure Active Directory. Como parte deste processo, irá gerar um ficheiro que inclui o material de autenticação que o MFA necessita para contactar o Azure MFA.  Para continuar, precisará de uma subscrição do Azure.

### <a name="register-your-multi-factor-authentication-provider-in-azure"></a>Registar o fornecedor de autenticação multifator no Azure

1.  Criar um fornecedor de [MFA.](/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider)

2. Abra um caso de suporte e solicite o C#SDK direto para ASP.net 2.0 . O SDK só será fornecido aos utilizadores atuais de MIM com MFA porque o SDK direto foi depreciado. Os novos clientes devem adotar a próxima versão da MIM que se integrará com o servidor MFA.

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

Nota: se utilizar o Servidor Azure MFA, ou outro fornecedor que gere a própria senha única, certifique-se de que o campo de comprimento configurado acima é o mesmo comprimento que o gerado pelo fornecedor de MFA.  Este comprimento deve ser de 6 para o Servidor Azure MFA.  O Azure MFA Server também gera o seu próprio texto de mensagem para que a mensagem de texto SMS seja ignorada.

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

5.  ! [MIM ac
6.  contar imagem de sucesso desbloqueada](media/MIM-SSPR-account-unlock.JPG)

6.  Se o utilizador escolher repor a palavra-passe, terá de escrever uma nova palavra-passe duas vezes e clicar em **Seguinte** para alterar a palavra-passe.
