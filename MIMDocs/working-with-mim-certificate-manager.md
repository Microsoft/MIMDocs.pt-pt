---
title: "Implementar a aplicação Gestor de Certificados do MIM para Windows | Documentos da Microsoft"
description: "Saiba como implementar a aplicação Gestor de Certificados para permitir aos utilizadores gerirem os respetivos direitos de acesso."
keywords: 
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/16/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 66060045-d0be-4874-914b-5926fd924ede
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e472d7cdc07aa19464aa1f18447d8c5dc7d0f0ba
ms.sourcegitcommit: 1e0626a366a41d610e6a117cdf684241eb65ec63
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2017
---
# <a name="mim-certificate-manager-windows-store-application-deployment"></a>Implementação de aplicação da loja Windows de Gestor de certificados de MIM

Depois de ter o MIM 2016 e o Gestor de certificados em execução, pode implementar a aplicação da loja Windows do Gestor de certificados de MIM. A aplicação da loja windows permite aos utilizadores gerirem os respetivos smart cards físicos, smart cards virtuais e certificados de software. Os passos para implementar a aplicação MIM CM são os seguintes:

1. Crie um modelo de certificado.

2. Crie um modelo de perfil.

3. Prepare a aplicação.

4. Implemente a aplicação através do SCCM ou do Intune.

## <a name="create-a-certificate-template"></a>Criar um modelo de certificado

Crie um modelo de certificado para a aplicação CM normalmente, mas certifique-se de que o modelo de certificado tem uma versão igual ou superior à 3.

1. Inicie sessão no servidor que executa o AD CS (o servidor de certificado).

2. Abra a MMC.

3. Clique em **ficheiro &gt; Adicionar/Remover Snap-in**.

4. Na lista Snap-ins disponíveis, clique em **Modelos de Certificado** e, em seguida, em **Adicionar**.

5. Agora, os **Modelos de Certificado** encontram-se na **Raiz da Consola** na MMC. Faça duplo clique na mesma para ver todos os modelos de certificado disponíveis.

6. Clique com o botão direito do rato no modelo **Início de Sessão de Smart Card** e clique em **Duplicar Modelo**.

7. No separador compatibilidade, na autoridade de certificação, selecione Windows Server 2008. Em destinatário do certificado, selecione Windows 8.1 / Windows Server 2012 R2. A versão de modelo de versão é definida na primeira vez que cria e guardar o modelo de certificado. Se não tiver criado o modelo de certificado desta forma não é possível modificar para a versão correta.

    >[!NOTE]
    Este passo é crucial, porque garante que tem um modelo de certificado de 3 (ou superior) da versão. Apenas modelos da versão 3 funcionam com a aplicação do Gestor de certificados.
    
8.  No separador **Geral**, no campo **Nome a Apresentar**, escreva o nome que pretende ver apresentado na IU da aplicação, tal como **Início de Sessão de Smart Card Virtual**.

9. No separador **Processamento de Pedidos**, defina o **Objetivo** para **Assinatura e encriptação** e em **Efetue o seguinte...**, selecione **Perguntar ao utilizador durante a inscrição**.

10. No separador **Criptografia**, em **Categoria de Fornecedor**, selecione **O Fornecedor de Armazenamento de Chaves e os Pedidos podem utilizar qualquer fornecedor disponível no computador do indivíduo**.

    > [!NOTE]
    > A opção Fornecedor de Armazenamento de Chaves apenas será apresentada se o seu modelo tiver a versão 3. Se a mesma não for apresentada, é provável que não tenha criado o modelo de certificado corretamente com a versão correta. Comece novamente no passo 5 acima.

11. No separador **Segurança**, adicione o grupo de segurança ao qual pretende conceder o acesso de **Inscrição**. Por exemplo, se pretende conceder acesso a todos os utilizadores, selecione o grupo **Utilizadores autenticados** e, em seguida, selecione **Permissões de inscrição** para os mesmos.

12. Clique em **OK** para finalizar as alterações e criar o novo modelo. Deverá conseguir ver o novo modelo na lista de Modelos de Certificado.

13. Selecione **Ficheiro** e clique em **Adicionar/Remover Snap-in** para adicionar o snap-in Autoridade de Certificação à consola MMC. Quando lhe for pedido o computador que pretende gerir, selecione **Computador Local**.

14. No painel esquerdo da MMC, expanda a **Autoridade de Certificação (Local)** e, em seguida, expanda a AC na lista de Autoridades de Certificação.

15. Clique com botão direito do rato em **Modelos de Certificado**, clique em **Novo &gt; Modelo de Certificado** a Emitir.

16. Na lista, selecione o novo modelo que criou e clique em **OK**.

## <a name="create-a-profile-template"></a>Criar um modelo de perfil

Quando cria um modelo de perfil, certifique-se de que o configura para criar/destruir o vSC e remover a recolha de dados. A aplicação CM não consegue processar os dados recolhidos, pelo que é importante desativá-los da seguinte forma.

1.  Inicie sessão no portal do CM como um utilizador com privilégios administrativos.

2.  Aceda a administração &gt; gerir modelos de perfil. Certifique-se de que marca a caixa junto a **registo de Smart Card de MIM CM exemplo no modelo de perfil** e, em seguida, clique em copiar um modelo de perfil selecionado.

3.  Escreva o nome do modelo de perfil e clique em **OK**.

4.  No ecrã seguinte, clique em **Adicionar novo modelo de certificado** e certifique-se de que marca a caixa junto ao nome da AC.

5.  Selecione a caixa junto ao nome do modelo de perfil **Início de Sessão** e clique em **Adicionar**.

6.  Remova o modelo SmartCardLogon ao selecionar a caixa junto do mesmo, clique em **Eliminar modelos de certificado selecionados** e, em seguida, clique em **OK**.

7.  Desloque-se até à parte inferior e clique em **Alterar definições**.

8.  Selecione as caixas junto de **Criar/Destruir smart card virtual** e **Diversificar Chave Admin**.

9. Em **Política de PIN de Utilizador**, selecione **Fornecido pelo utilizador**.

10. No painel esquerdo, clique em **Política de Renovação &gt; Alterar definições gerais**. Selecione **Reutilizar cartão ao renovar** e clique em **OK**.

11. Tem de desativar os itens de recolha de dados para todas as políticas ao clicar na política no painel esquerdo. Em seguida, terá da caixa de verificação junto a **item de dados de exemplo** clique **eliminar itens de recolha de dados** e, em seguida, clique em **OK**.

## <a name="prepare-the-cm-app-for-deployment"></a>Preparar a aplicação CM para a implementação

1. Na linha de comandos, execute o seguinte comando para descompactar a aplicação. O comando será extrair os conteúdos para uma nova subpasta denominada appx e crie uma cópia para não modificar o ficheiro original.

    ```cmd
    makeappx unpack /l /p <app package name>.appx /d ./appx
    ren <app package name>.appx <app package name>.appx.original
    cd appx
    ```

2. Na pasta appx, altere o nome do ficheiro denominado ExemploDeDadosPersonalizados.xml para Dados.personalizados

3. Abra o ficheiro Dados.personalizados e modifique os parâmetros conforme necessário.

    |||
    |-|-|
    |URL de MIMCM|O FQDN do portal que utilizou para configurar a CM. Por exemplo, https://mimcmServerAddress/certificatemanagement|
    |URL de ADFS|Se planear utilizar o AD FS, introduza o URL de AD FS. Por exemplo, https://adfsServerSame/adfs </br> Se não não utilizado o AD FS, configure esta definição com uma cadeia vazia.  Por exemplo,```<ADFS URL=""/>``` |
    |PrivacyUrl|Pode incluir um URL numa página Web a explicar o que fazer com os detalhes de utilizador recolhidos para a inscrição de certificado.|
    |SupportMail|Pode incluir um endereço de e-mail para problemas de suporte.|
    |LobComplianceEnable|Pode definir esta opção para verdadeiro ou falso. A predefinição é verdadeiro.|
    |MinimumPinLength|A predefinição é 6.|
    |NonAdmin|Pode definir esta opção para verdadeiro ou falso. A predefinição é falso. Só deve modificar esta opção se pretender que os utilizadores que não são administradores nos respetivos computadores possam inscrever e renovar certificados.|
>[!IMPORTANT]
Tem de ser especificado um valor para o URL de ADFS. Se for especificado nenhum valor, a aplicação moderna será erro terminar na primeira utilização.
4.  Guarde o ficheiro e saia do editor.

5.  A assinatura do pacote cria um ficheiro de assinatura, pelo que terá de eliminar o ficheiro de assinatura original denominado AppxSignature.p7x.

6.  O ficheiro AppxManifest.xml especifica o nome do requerente do certificado de assinatura. Abra este ficheiro para o editar.

7.  Tem de obter um certificado de assinatura antes de iniciar esta secção. Consulte abaixo o passo 1 Ativar a renovação do smart card para não administradores no Gestor de Certificados do MIM 2016.

8.  No elemento &lt;Identity&gt;, modifique o valor do atributo Publisher para ficar idêntico ao indivíduo listado no seu certificado de assinatura, por exemplo, “CN=INDIVÍDUO”.

9. Guarde o ficheiro e saia do editor.

10. Na linha de comandos, execute o seguinte comando para voltar a compactar e assinar o ficheiro .appx.

    ```cmd
    cd ..
    makeappx pack /l /d .\appx /p <app package name>.appx
    ```
    em que o nome do pacote de aplicação é o mesmo nome que utilizou quando criou a cópia.

    ```cmd
    signtool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.ap
    px
    ```
    Isto fornece o novo ficheiro .appx. O ficheiro pfx fornece uma localização para o certificado de assinatura e uma palavra-passe para o ficheiro .pfx.

11. Para funcionar com a Autenticação AD FS:

    -  Abra a aplicação Smart Card Virtual. Isto torna mais fácil localizar os valores necessários para o passo seguinte.

    -  Para adicionar a aplicação como um cliente ao servidor AD FS e configurar a CM no servidor, no servidor AD FS, abra o Windows PowerShell e execute o comando `ConfigureMimCMClientAndRelyingParty.ps1 –redirectUri <redirectUriString> -serverFQDN <MimCmServerFQDN>`

        Segue-se o script ConfigureMimCMClientAndRelyingParty.ps1.

       ```PowerShell
        # HELP

        <#
        .SYNOPSIS
                        Configure ADFS for CM client app and server.
        .DESCRIPTION
           What the Script does:
                                        a. Registers the MIM CM client app on the ADFS server.
                                        b. Registers the MIM CM server relying party (Tells the ADFS server that it issues tokens for the CM server).
                        For parameter information, see 'get-help -detailed'
        .PARAMETER redirectUri
                        The redirectUri for CM client app. Will be added to ADFS server.
                        It can be found as follows:
                        1. Open the settings panel. Under settings, there is a "Redirect Uri" text box (an ADFS server address must be configured in order for the text to display).
                        2. Open MIM CM client app. Navigate to 'C:\Users\<your_username>\AppData\Local\Packages\CmModernAppv.<version>\LocalState', and open 'Logs_Virtual Smart Card Certificate Manager_<version>'. Search for "Redirect URI".
        .PARAMETER serverFqdn
                        Your deployed MIM CM server’s FQDN.
        .EXAMPLE
           .\ConfigureMimCMClientAndRelyingParty.ps1 -redirectUri ms-app://s-1-15-2-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789/ -serverFqdn WIN-TRUR24L4CFS.corp.cmteam.com
        #>

        # Parameter declaration
        [CmdletBinding()]
        Param(
          [Parameter(Mandatory=$True)]
           [string]$redirectUri,

           [Parameter(Mandatory=$True)]
           [string]$serverFqdn
        )

        Write-Host "Configuring ADFS Objects for OAuth.."

        #Configure SSO to get persistent sign on cookie
        Set-ADFSProperties -SsoLifetime 2880

        #Configure Authentication Policy
        #Intranet to use Kerberos
        #Extranet to use U/P

        #Create Client Objects

        Write-Host "Creating Client Objects..."

        $existingClient = Get-ADFSClient -Name "MIM CM Modern App"

        if ($existingClient -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Modern App, removing"
            Remove-ADFSClient -TargetName "MIM CM Modern App"
            Write-Host "Client object removed"
        }

        Write-Host "Adding Client Object for MIM CM Modern App client"
        Add-ADFSClient -Name "MIM CM Modern App" -ClientId "70A8B8B1-862C-4473-80AB-4E55BAE45B4F" -RedirectUri $redirectUri
        Write-Host "Client Object for MIM CM Modern App client Created"

        #Create Relying Parties
        Write-Host "Creating Relying Party Objects"

        $existingRp = Get-ADFSRelyingPartyTrust -Name "MIM CM Service"
        if ($existingRp -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Service RP, removing"
            Remove-ADFSRelyingPartyTrust -TargetName "MIM CM Service"
            Write-Host "RP object Removed"
        }

        $authorizationRules =
        "@RuleTemplate = `"AllowAllAuthzRule`"
        => issue(Type = `"http://schemas.microsoft.com/authorization/claims/permit`", Value = `"true`");"

        $issuanceRules =
        "@RuleTemplate = `"LdapClaims`"
        @RuleName = `"Emit UPN and common name`"
        c:[Type == `"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`", Issuer == `"AD AUTHORITY`"]
        => issue(store = `"Active Directory`", types =
        (`"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`",
        `"http://schemas.xmlsoap.org/claims/CommonName`"), query =
        `";userPrincipalName,cn;{0}`", param = c.Value);

        @RuleTemplate = `"PassThroughClaims`"
        @RuleName = `"Pass through Windows Account Name`"
        c:[Type ==`"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`"] => issue(claim = c);"

        Write-Host "Creating RP Trust for MIM CM Service"
        Add-ADFSRelyingPartyTrust -Name "MIM CM Service" -Identifier ("https://"+$serverFqdn+"/certificatemanagement") -IssuanceAuthorizationRules $authorizationRules -IssuanceTransformRules $issuanceRules
        Write-Host "RP Trust for MIM CM Service has been created"
        ```

    - Atualize os valores de redirectUri e serverFQDN.

    - Para localizar o redirectUri, na aplicação Smart Card Virtual, abra o painel de definições da aplicação, clique em **Definições** e o URI de redirecionamento deverá estar listado abaixo da barra de endereço do servidor AD FS. O URI só será apresentado se um endereço do servidor ADFS estiver configurado.

    - O serverFQDN é apenas o nome completo do computador do servidor MIMCM apenas.

    - Para obter ajuda com a **ConfigureMIimCMClientAndRelyingParty.ps1** script, execute: </br> 
    ```Powershell
     get-help  -detailed ConfigureMimCMClientAndRelyingParty.ps1
    ```

## <a name="deploy-the-app"></a>Implementar a aplicação

Ao configurar a aplicação CM, no Centro de Transferências, transfira o ficheiro MIMDMModernApp_&lt;version&gt;_AnyCPU_Test.zip e extraia todos os respetivos conteúdos. O ficheiro .appx é o instalador. Pode implementá-la da forma que normalmente implementa aplicações da loja Windows, através do [System Center Configuration Manager](https://technet.microsoft.com/library/dn613840.aspx) ou do [Intune](https://technet.microsoft.com/library/dn613839.aspx) para o sideload da aplicação para que os utilizadores tenham de aceder através do Portal da Empresa ou obtenham a aplicação diretamente nos respetivos computadores.

## <a name="next-steps"></a>Próximos passos

- [Configurar modelos de perfil](https://technet.microsoft.com/library/cc708656)
- [Gerir Aplicações para Smart Cards](https://technet.microsoft.com/library/cc708681)