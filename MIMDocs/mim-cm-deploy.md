---
title: Implementação do Gestor de Certificados do Gestor de Identidade da Microsoft ! Microsoft Docs
description: Instalar o Gestor de Certificados microsoft 2016
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/19/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 1885f01d1010565cef9ce50028ae1c86e8769003
ms.sourcegitcommit: 78c2d7e5ba4bec276d5a9bf8860bc126d9bd9c33
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/11/2020
ms.locfileid: "94492545"
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Implementação do Gestor de Certificados do Gestor de Identidade da Microsoft 2016 (MIM CM)

A instalação do Microsoft Identity Manager Certificate Manager 2016 (MIM CM) envolve uma série de etapas. Como forma de simplificar o processo, estamos a desmoronar as coisas. Há passos preliminares que devem ser dados antes de quaisquer passos de MIM CM reais. Sem o trabalho preliminar, é provável que a instalação falhe.

O diagrama abaixo mostra um exemplo do tipo de ambiente que pode ser usado. Os sistemas com números estão incluídos na lista abaixo do diagrama e são necessários para completar com sucesso os passos abrangidos neste artigo. Finalmente, os Servidores do Datacenter do Windows 2016 são utilizados:

![Diagrama ambiental](media/mim-cm-deploy/image001.png)

1. CORPDC – Controlador de Domínio
2. CORPCM – Servidor MIM CM
3. CORPCA – Autoridade de Certificados
4. CORPCMR – Mim CM Rest API Web – CM Portal para repouso API – Usado para mais tarde
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1 - Domínio Windows 10 Unidos

## <a name="deployment-overview"></a>Descrição geral da implementação

- Instalação do sistema operativo base

    O laboratório é composto por servidores do Datacenter windows 2016.

    >[!NOTE]
    >Para mais detalhes sobre as plataformas apoiadas para MIM 2016, veja o artigo intitulado [Plataformas Apoiadas para MIM 2016](microsoft-identity-manager-2016-supported-platforms.md).

1. Etapas de pré-implantação

    - [Estender o esquema](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)

    - Criação de contas de serviço

    - [Criação de modelos de certificados](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)

    - IIS

    - Configurar Kerberos

    - Etapas relacionadas com a base de dados

        - Requisitos de configuração SQL

        - Permissões de base de dados

2. Implementação

## <a name="pre-deployment-steps"></a>Etapas de pré-implantação

O assistente de configuração MIM CM requer que sejam fornecidas informações ao longo do caminho para que esta se complete com sucesso.

![diagrama](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>Estender o esquema

O processo de extensão do esquema é simples, mas deve ser abordado com cautela devido à sua natureza irreversível.

>[!NOTE]
>Este passo requer que a conta utilizada tenha direitos de administração de esquemas.

1. Navegue pela localização do mídia MIM e navegue para a \\ pasta Certifi Management \\ x64.

2. Copie a pasta Schema para CORPDC e, em seguida, navegue para ela.

    ![diagrama](media/mim-cm-deploy/image005.png)

3. Executar o guião resourceForestModifySchema.vbs cenário único da Floresta. Para o cenário de floresta de recursos executar os scripts:
   - DomainA – Utilizadores localizados (userForestModifySchema.vbs)
   - ResourceForestB – Localização da instalação CM (resourceForestModifySchema.vbs).

     >[!NOTE]
     >As alterações de esquema são uma operação de sentido único e requerem uma recuperação da floresta para recuar, por isso certifique-se de que tem as cópias de segurança necessárias. Para mais informações sobre as alterações esfectuando o esquema através da realização desta operação, reveja o artigo [Forefront Identity Manager 2010 Certificate Management Schema Changes](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx)

     ![diagrama](media/mim-cm-deploy/image007.png)

4. Execute o script e receberá uma mensagem de sucesso assim que o script estiver concluído.

    ![Mensagem de êxito](media/mim-cm-deploy/image009.png)

O esquema em AD é agora estendido para apoiar a MIM CM.

### <a name="creating-service-accounts-and-groups"></a>Criação de contas de serviços e grupos

A tabela seguinte resume as contas e permissões exigidas pela MIM CM. Pode permitir que o MIM CM crie automaticamente as seguintes contas ou pode criá-las antes da instalação. Os nomes das contas podem ser alterados. Se você mesmo criar as contas, considere nomear as contas do utilizador de tal forma que é fácil combinar o nome da conta do utilizador com a sua função.

Utilizadores:

![Diagrama](media/mim-cm-deploy/image010.png)

![Diagrama](media/mim-cm-deploy/image012.png)

| **Role**                   | **Registo do utilizador no nome** |
|----------------------------|---------------------|
| Agente MIM CM               | MIMCMAgent          |
| Agente de recuperação de chaves MIM CM  | MIMCMKRAgent        |
| Agente de Autorização MIM CM | MIMCMAuthAgent      |
| Agente gerente da MIM CM CA    | MIMCMManagerAgent   |
| Agente de piscina web MIM CM      | MIMCMWebAgent       |
| Agente de inscrição mim CM    | MIMCMEnrollagent    |
| Serviço de Atualização MIM CM      | Serviço MIMCM        |
| Conta de Instalação MIM        | MIMINSTALL          |
| Agente de mesa de ajuda            | CMHelpdesk1-2       |
| Gestor cm                 | CMManager1-2        |
| Utilizador assinante            | CMUSER1-2           |

Grupos:

| **Role**               | **Grupo**         |
|------------------------|-------------------|
| Membros do CM Helpdesk    | MIMCM-Helpdesk    |
| Membros gerentes do CM     | MIMCM-Managers    |
| Membros dos Assinantes do CM | MIMCM-Subscribers |

Powershell: Contas do agente:

```powershell
import-module activedirectory
## Agent accounts used during setup
$cmagents = @{
"MIMCMKRAgent" = "MIM CM Key Recovery Agent"; 
"MIMCMAuthAgent" = "MIM CM Authorization Agent"
"MIMCMManagerAgent" = "MIM CM CA Manager Agent";
"MIMCMWebAgent" = "MIM CM Web Pool Agent";
"MIMCMEnrollAgent" = "MIM CM Enrollment Agent";
"MIMCMService" = "MIM CM Update Service";
"MIMCMAgent" = "MIM CM Agent";
}
##Groups Used for CM Management
$cmgroups = @{
"MIMCM-Managers" = "MIMCM-Managers"
"MIMCM-Helpdesk" = "MIMCM-Helpdesk"
"MIMCM-Subscribers" = "MIMCM-Subscribers" 
}
##Users Used during testlab
$cmusers = @{
"CMManager1" = "CM Manager1"
"CMManager2" = "CM Manager2"
"CMUser1" = "CM User1"
"CMUser2" = "CM User2"
"CMHelpdesk1" = "CM Helpdesk1"
"CMHelpdesk2" = "CM Helpdesk2"
}

## OU Paths
$aoupath = "OU=Service Accounts,DC=contoso,DC=com" ## Location of Agent accounts
$oupath = "OU=CMLAB,DC=contoso,DC=com" ## Location of Users and Groups for CM Lab


#Create Agents – Update UserprincipalName
$cmagents.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -UserPrincipalName ($_.Name + "@contoso.com")  -Path $aoupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}


#Create Users
$cmusers.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -Path $oupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}
```

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>Atualizar a política local do servidor **CORPCM** para contas de agente 

| **Registo do utilizador no nome** | **Descrição e permissões**   |
|------|---------------------|
| MIMCMAgent          | Presta os seguintes serviços: </br>- Recupera chaves privadas encriptadas da AC. </br>- Protege as informações pin do cartão inteligente na base de dados FIM CM. </br>- Protege a comunicação entre a FIM CM e a AC. </br></br> Esta conta de utilizador requer as seguintes definições de controlo de acesso:</br>-   **Permita o início de são local** direito.</br>-   **Emitir e Gerir os Certificados** direito do utilizador. </br>- Ler e escrever permissão sobre a pasta temp do sistema no seguinte local: %WINDIR% \\ Temp.</br>- Um certificado de assinatura e encriptação digital emitido e instalado na loja do utilizador.
|MIMCMKRAgent        | Recupera chaves privadas arquivadas da AC. Esta conta de utilizador requer as seguintes definições de controlo de acesso:</br> -   **Permita o início de são local** direito.</br>- Membro do grupo de **administradores** locais. </br>- Inscreva a permissão no modelo de certificado **KeyRecoveryAgent.** </br>- O certificado key Recovery Agent é emitido e instalado na loja do utilizador. O certificado deve ser adicionado à lista dos principais agentes de recuperação da AC. </br>- Ler permissão e escrever permissão na pasta temp do sistema no seguinte local: ```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | Determina os direitos e permissões do utilizador para utilizadores e grupos. Esta conta de utilizador requer as seguintes definições de controlo de acesso: </br>- Adesão ao grupo de domínio de acesso compatível pré-Windows 2000. </br> - Concedido o direito de utilizador **de auditorias de segurança gerar.**             |
| MIMCMManagerAgent   | Realiza atividades de gestão de CA. </br> Este utilizador deve ser atribuído a permissão De Gestão de CA.        |
| MIMCMWebAgent       | Fornece a identidade para o conjunto de aplicações IIS. Fim CM funciona dentro de um processo de interface de programação de aplicações ® Microsoft Win32 que utiliza as credenciais deste utilizador. </br> Esta conta de utilizador requer as seguintes definições de controlo de acesso:</br> - Adesão ao **IIS_WPG local, windows 2016 = IIS_IUSRS** grupo. </br>- Membro do grupo de **administradores** locais.</br>- Concedido o direito de utilizador **de auditorias de segurança gerar.** </br>- Concede-se a Lei como parte do direito do utilizador **do sistema operativo.** </br>- Certo é certo que o direito do utilizador **do token de nível de processo de substituição.**</br>- Atribuída como identidade do conjunto de aplicações IIS, **CLMAppPool**. </br>- Concede-se a permissão de leitura na chave de registo    **\\ \\ \\ \\ \\ \\ webuser do software HKEY_LOCAL_MACHINE Microsoft CLM v1.0** Server. </br>- (EN) Esta conta deve também ser confiada à delegação.|
| MIMCMEnrollagent    | Realiza inscrição em nome de um utilizador. Esta conta de utilizador requer as seguintes definições de controlo de acesso:</br>- Um certificado de Agente de Matrícula que é emitido e instalado na loja do utilizador.</br>-   **Permita o início de são local** direito. </br>- Inscreva a permissão no modelo de certificado **do Agente de Inscrição** (ou no modelo personalizado, se for utilizado).                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Criação de modelos de certificados para contas de serviço MIM CM

Três das contas de serviço utilizadas pela MIM CM requerem um certificado e o assistente de configuração requer que forneça o nome dos modelos de certificados que deve utilizar para solicitar certificados para os mesmos.

As contas de serviço que requerem certificados são:

- MIMCMAgent: Esta conta precisa de um certificado de utilizador

- MIMCMEnrollAgent: Esta conta precisa de um certificado de agente de inscrição

- MIMCMKRAgent: Esta conta precisa de um certificado **de agente de recuperação chave**

Existem modelos já presentes em AD, mas precisamos criar as nossas próprias versões para trabalhar com MIM CM. Como precisamos fazer modificação a partir dos modelos de base original.

Todas as três contas acima terão direitos elevados dentro da sua organização e devem ser tratadas cuidadosamente.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>Criar o modelo de certificado de assinatura MIM CM

1. A partir de **Ferramentas Administrativas,** **Autoridade de Certificação Aberta.**

2. Na consola da **Autoridade de Certificação,** na árvore das consolas, **expanda a Contoso-CorpCA** e, em seguida, clique em **Modelos de Certificados**.

3. Clique nos **modelos de certificado à** direita e, em seguida, clique em **Gerir**.

4. Na **consola de modelos de certificado,** no painel de **detalhes,** selecione e clique à direita **Utilizador** e, em seguida, clique no **Modelo Duplicado**.

5. Na caixa de diálogo **do modelo duplicado,** selecione **o Windows Server 2003 Enterprise** e, em seguida, clique em **OK**.

    ![Mostrar as alterações resultantes](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    >A MIM CM não funciona com certificados baseados nos modelos de certificados da versão 3. Tem de criar um modelo de certificado do Windows Server® 2003 Enterprise (versão 2).). Consulte [os detalhes da V3](https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade) para mais informações.

6. Nas propriedades da caixa de diálogo **do novo modelo,** no separador **Geral,** na caixa de **nome de exibição** do modelo, **escreva a assinatura MIM CM**. Altere o **Período de Validade** para **2 anos** e, em seguida, limpe o certificado de Publicação na caixa de verificação do **Diretório Ativo.**

7. No **separador 'Tratamento de** Pedidos', certifique-se de que a **chave privada a exportar** é selecionada e, em seguida, clique no **separador Criptografia**.

8. Na caixa de diálogo **de seleção de criptografia,** desative o **Microsoft Enhanced Cryptographic Provider v1.0** , ativar **o Microsoft Enhanced RSA e o AES Cryptographic Provider** , e depois clicar em **OK**.

9. No **separador Nome do Assunto,** limpe o **nome do e-mail incluir o nome do e-mail no nome do assunto** e caixas de verificação de **nomes de e-mail.**

10. No separador **Extensões,** nas **Extensões incluídas nesta lista de modelos,** certifique-se de que **as Políticas de Aplicação** e, em seguida, clique em **Editar**.

11. Na caixa de diálogo **de extensão de políticas de edição,** selecione tanto o **Sistema de Ficheiros encriptadores** como as políticas de aplicação **de e-mail seguro.** Clique **em Remover** e, em seguida, clique em **OK**.

12. No separador **Segurança** execute os seguintes passos:

    - Remover **Administrador**.

    - Remover **Administradores de Domínio**.

    - Remover **Utilizadores de Domínio**.

    - Atribua apenas permissões **de Leitura** e **Escrita** aos Administradores **da Empresa.**

    - Adicione **MIMCMAgent.**

    - Atribuir permissões **de Leitura** e **Inscrição** à **MIMCMAgent**.

13. Na caixa de diálogo **de novas propriedades do novo modelo,** clique em **OK**.

14. Deixe a **consola de modelos de certificado** aberta.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>Criar o modelo de certificado de agente de inscrição MIM CM

1. Na **consola de modelos de certificado,** no painel de **detalhes,** selecione e clique à direita **Agente de inscrição** e, em seguida, clique no **Modelo duplicado**.

2. Na caixa de diálogo **do modelo duplicado,** selecione **o Windows Server 2003 Enterprise** e, em seguida, clique em **OK**.

3. Na caixa de diálogo **de novas propriedades do novo modelo,** no separador **Geral,** na caixa de **nome de exibição** do modelo, **escreva o Agente de Inscrição MIM CM**. Certifique-se de que o **Período de Validade** é **de 2 anos**.

4. No **separador Tratamento de Pedidos,** **ativar a chave privada a exportar** e, em seguida, clique em **CSPs ou No separador criptografia.**

5. Na caixa de diálogo **de seleção CSP,** desative o **Microsoft Base Cryptographic Provider v1.0** , desative **o Microsoft Enhanced Cryptographic Provider v1.0** , ativar **o Microsoft Enhanced RSA e o AES Cryptographic Provider** , e depois clique em **OK**.

6. No separador **Segurança** efetuar o seguinte:

    - Remover **Administrador**.

    - Remover **Administradores de Domínio**.

    - Atribua apenas permissões **de Leitura** e **Escrita** aos Administradores **da Empresa.**

    - Adicione **MIMCMEnrollagent**.

    - Atribuir permissões **de Leitura** e **Inscrição** à **MIMCMEnrollAgent**.

7. Na caixa de diálogo **de novas propriedades do novo modelo,** clique em **OK**.

8. Deixe a **consola de modelos de certificado** aberta.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>Crie o modelo de certificado de agente de recuperação de chave MIM CM

1. Na consola **De Modelos de Certificado,** no painel de **detalhes,** selecione e clique com o botão direito **do Agente de Recuperação** da Chave e, em seguida, clique no Modelo de **Duplicado**.

2. Na caixa de diálogo **do modelo duplicado,** selecione **o Windows Server 2003 Enterprise** e, em seguida, clique em **OK**.

3. Nas propriedades da caixa de diálogo **do novo modelo,** no separador **Geral,** na caixa de **nome de exibição** do modelo, **escreva o Agente de Recuperação da Chave MIM CM**. Certifique-se de que o **Período de Validade** é **de 2 anos** no **separador criptografia.**

4. Na caixa de diálogo **de seleção de fornecedores, desative** o **Microsoft Enhanced Cryptographic Provider v1.0** , ativar **o Microsoft Enhanced RSA e o AES Cryptographic Provider** , e depois clicar em **OK**.

5. No **separador Requisitos de Emissão,** certifique-se de que a **aprovação do gestor de certificados de CA** está **desativada.**

6. No separador **Segurança** efetuar o seguinte:

    - Remover **Administrador**.

    - Remover **Administradores de Domínio**.

    - Atribua apenas permissões **de Leitura** e **Escrita** aos Administradores **da Empresa.**

    - Adicione **MIMCMKRAgent**.

    - Atribua permissões **de Leitura** e **Inscrição** à **KRAgent**.

7. Na caixa de diálogo **de novas propriedades do novo modelo,** clique em **OK**.

8. Feche a **consola de modelos de certificado.**

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>Publicar os modelos de certificados necessários na Autoridade de Certificação

1. Restaurar a consola **da Autoridade de Certificação.**

2. Na consola da **Autoridade de Certificação,** na consola, clique à direita **nos modelos de certificados,** aponte para **Novo** e, em seguida, clique no Modelo de Certificado **para Emitir**.

3. Na caixa de diálogo **de modelos de certificados de ativação,** selecione **o Agente de Inscrição MIM CM,** **o Agente de Recuperação da Chave MIM CM** e a assinatura mim **CM**. Clique em **OK**.

4. Na árvore da consola, clique em **Modelos de Certificado.**

5. Verifique se os três novos modelos aparecem no painel de **detalhes** e, em seguida, feche **a Autoridade de Certificação**.

    ![Assinatura MIM CM](media/mim-cm-deploy/image016.png)

6. Feche todas as janelas abertas e faça login.

### <a name="iis-configuration"></a>Configuração do IIS

Para hospedar o website para CM, instale e configuure o IIS.

#### <a name="install-and-configure-iis"></a>Instalar e configurar o IIS

1. Iniciar sessão **no CORLog como** conta **MIMINSTALL**

    >[!IMPORTANT]
    >A conta de instalação MIM deve ser um administrador local

2. Abrir PowerShell e executar o seguinte comando

    `Install-WindowsFeature –ConfigurationFilePath`

>[!NOTE]
>Um site chamado Site Padrão é instalado por predefinição com iIS 7. Se esse site tiver sido renomeado ou removido um site com o nome Padrão Web site deve estar disponível antes de mim CM ser instalado.

#### <a name="configuring-kerberos"></a>Configurar Kerberos

A conta MIMCMWebAgent estará a executar o portal MIM CM. Por predefinição no IIS e na autenticação do modo kernel é utilizado no IIS por padrão. Em vez disso, irá desativar a autenticação do modo de kernel Kerberos e configurar as SPNs na conta MIMCMWebAgent. Algum comando requer permissão elevada no diretório ativo e servidor CORPCM.

![Diagrama](media/mim-cm-deploy/image020.png)

```powershell
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}
```

**Atualizar iIS na CORPCM**

![diagrama](media/mim-cm-deploy/image022.png)

```powershell
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true
```

>[!NOTE]
>Você precisará adicionar um DNS A Record para o "cm.contoso.com" e apontar para CORPCM IP

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>Requerendo SSL no portal MIM CM

É altamente recomendável que necessite de SSL no portal MIM CM. Se não o fizeres, o feiticeiro vai mesmo avisá-lo sobre isso.

1. Inscreva-se no certificado web para **cm.contoso.com** atribuir ao site predefinido

2. Abra **o Gestor do IIS** e navegue para a **Gestão de Certificados**

3. Em Features View, clique duas vezes nas Definições SSL.

4. Na página SSL Settings, selecione **Require SSL**.

5. No painel Ações, clique em **Aplicar.**

### <a name="database-configuration-corpsql-for-mim-cm"></a>Configuração da base **de dados CORPSQL** para MIM CM

1. Certifique-se de que está ligado ao Servidor CORPSQL01.

2. Certifique-se de que está ligado como SQL DBA.

3. Execute o seguinte script T-SQL para permitir que a \\ Conta CONTOSO MIMINSTALL crie a base de dados quando formos para o passo de configuração

    >[!NOTE]
    >Teremos de voltar ao SQL quando estivermos prontos para a saída & módulo de política

    ```sql
    create login [CONTOSO\\MIMINSTALL] from windows;
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
    ```

![Mensagem de erro do assistente de configuração MIM CM](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Implementação do Microsoft Identity Manager 2016 Certificate Management

1. Certifique-se de que está ligado ao Servidor CORPCM e que a conta **MIMINSTALL** é membro do grupo de **administradores locais.**

2. Certifique-se de que está ligado como Contoso \\ MIMINSTALL.

3. Monte o Microsoft Identity Manager 2016 SP1 ou mais tarde o pacote de serviço ISO.

4. **Abra** o **diretório de Gestão de \\ Certificados x64.**

5. Na janela **x64,** clique à direita **Configuração** e, em seguida, clique em **Executar como administrador**.

6. Na página "Bem-vindo à página do Assistente de Configuração de Gestão de Certificados de Gestão de Certificados do Gestor de Identidade da Microsoft", clique em **Seguinte.**

7. Na página End-User Contrato de Licença, leia o contrato, permita que eu aceite os termos na caixa de **verificação** do contrato de licença , e, em seguida, clique em Seguinte.

8. Na página Configuração Personalizada, certifique-se de que os componentes do Serviço de **Atualização MIM** **CM** e MIM CM estão definidos para serem instalados e, em seguida, **clique em Seguinte**.

9. Na página de pasta Web Virtual, certifique-se de que o nome da pasta Virtual é **CertificateManagement** e, em seguida, **clique em Seguinte**.

10. Na página De Gestão de Certificados do Gestor de Identidade do Microsoft, **clique em Instalar**.

11. Na página **'Completed'** The Microsoft Identity Manager Certificate Management Wizard, **clique em Terminar**.

![Assistente MIM CM concluído](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Assistente de Configuração do Gestor de Identidade da Microsoft 2016 Gestão de Certificados

Antes de iniciar sessão na CORPCM, adicione MIMINSTALL ao **grupo de administração de domínios, Admins e administradores locais** para o assistente de configuração . Isto pode ser removido mais tarde uma vez que a configuração esteja completa.

![Mensagem de erro](media/mim-cm-deploy/image028.png)

1. A partir do menu **Iniciar,** clique em **Certificate Management Config Wizard**. E executar como **administrador**

2. Na página **"Bem-vindo à página do Assistente de Configuração",** clique em **Seguinte**.

3. Na página **de Configuração ca,** certifique-se de que o CA selecionado é **Contoso-CORPCA-CA, certifique-se** de que o servidor selecionado é     **CORPCA. CONTOSO.COM** , e depois clique em **Seguinte**.

4. Na página **De Bases de Dados ® Microsoft® SQ®L,** no Nome da caixa **SQL Server,** tipo **CORPSQL1** , permita **ao Utilizar as minhas credenciais para criar a** caixa de verificação da base de dados e, em seguida, clique em **Seguinte**.

5. Na página **Definições de Base de Dados,** aceite o nome de base de dados predefinido do **FIMCertificateManagement** , certifique-se de que a **autenticação integrada SQL** é selecionada e, em seguida, clique em **Seguinte**.

6. Na página Configurar o **Diretório Ativo,** aceite o nome predefinido fornecido para o ponto de ligação de serviço e, em seguida, clique em **Seguinte**.

7. Na página do **método de autenticação** confirmam que a **autenticação integrada** do Windows é selecionada e, em seguida, clique em **Seguinte**.

8. Na página **Agentes – FIM CM,** limpe a caixa **de verificação de definições padrão FIM CM** e, em seguida, clique em Contas **Personalizadas**.

9. Na caixa de diálogo multi-tafado **Fim CM,** em cada separador, digite as seguintes informações:

   - Nome do utilizador: **Atualização**

   - Palavra-passe: **Passe \@ palavra1**

   - Confirmar senha: **Passe \@ palavra1**

   - Utilize um utilizador existente: **Ativado**

     >[!NOTE]
     >Criámos estas contas mais cedo. Certifique-se de que os procedimentos do passo 8 são repetidos para as seis contas de conta do agente.

     ![Contas MIM CM](media/mim-cm-deploy/image030.png)

10. Quando todas as informações da conta do agente estiverem completas, clique **em OK**.

11. Na página **Agentes – MIM CM,** clique em **Seguinte**.

12. Na página **de certificados do servidor Configurar,** ative os seguintes modelos de certificado:

    - Modelo de certificado a utilizar para o certificado de agente de recuperação do agente de recuperação: **MIMCMKeyRecoveryAgent**.

    - Modelo de certificado a utilizar para o certificado fim CM Agent: **MIMCMSigning**.

    - Modelo de certificado a utilizar para o certificado de agente de inscrição: **FIMCMEnrollmentAgent**.

13. Na página **de certificados do servidor configurado,** clique em **Seguinte**.

14. No **Servidor de E-mail de Configuração,** página de impressão de documentos, na página **Desíduo o nome do servidor SMTP que pretende utilizar para caixa de notificações de registo de correio** eletrónico e, em seguida, clique em **Seguinte.**

15. Na página **Preparado para configurar** , clique em **Configurar**.

16. Na caixa de diálogo de aviso **Do Microsoft Forefront Identity Manager 2010 R2,** clique em **OK** para reconhecer que o SSL não está ativado no diretório virtual do IIS.

    ![meios de comunicação/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    >Não clique no botão 'Terminar' até que a execução do assistente de configuração esteja concluída. O registo de assistentes pode ser consultado aqui: **%programfiles% \\ Microsoft Forefront Identity Management \\ 2010 \\ Certificate Management \\ config.log**

17. Clique em **Concluir**.

    ![Assistente MIM CM concluído](media/mim-cm-deploy/image033.png)

18. Fechem todas as janelas abertas.

19. Adicione `https://cm.contoso.com/certificatemanagement` à zona intranet local no seu browser.

20. Visite o site do servidor CORPCM `https://cm.contoso.com/certificatemanagement`  

    ![diagrama](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>Verifique o serviço de isolamento da chave de CNG

1. A partir de **Ferramentas Administrativas,** **Serviços Abertos.**

2. No painel de **detalhes,** clique duas vezes no **isolamento da chave CNG.**

3. No separador **Geral,** altere o **Tipo de Arranque** para **Automático**.

4. Na aba **Geral,** inicie o serviço se não estiver em estado de início.

5. No separador **Geral,** clique **em OK**.

### <a name="installing-and-configuring-the-ca-modules-"></a>Instalação e Configuração dos Módulos CA:

Neste passo, vamos instalar e configurar os módulos FIM CM CA na autoridade de certificação.

1. Configure fim CM para apenas inspecionar permissões de utilização para operações de gestão

2. No **C: \\ Ficheiros de programa \\ Microsoft Front Identity Manager \\ 2010 \\ Certificate Management \\** web window, faça uma cópia de **web.config** nomeando a cópia **web.1.config**.

3. Na janela **Web,** **clique** Web.configà direita e, em seguida, clique em **Abrir**.

    >[!Note]
    >O ficheiro Web.config é aberto em bloco de notas

4. Quando o ficheiro abrir, prima CTRL+F.

5. Na caixa de diálogo **Localizar e Substituir,** na caixa **Localizar que** caixa, **digitar UseUser,** e, em seguida, clicar em **Localizar A Seguir** três vezes.

6. Feche a caixa **de diálogo Localizar e Substituir.**

7. Devia estar em **\<add key=”Clm.RequestSecurity.Flags” value=”UseUser,UseGroups” /\>** linha. Mude a linha para **\<add key=”Clm.RequestSecurity.Flags” value=”UseUser” /\>** ler.

8. Feche o ficheiro, guardando todas as alterações.

9. Criar uma conta para o computador CA no servidor SQL \<no script\>

10. Certifique-se de que está ligado ao servidor **CORPSQL01.**

11. Certifique-se de que está ligado como **DBA**

12. A partir do menu **Iniciar,** lance **o SQL Server Management Studio**.

13. Na caixa de diálogo **'Ligar ao Servidor',** na caixa de nomes do **Servidor,** **digite CORPSQL01 e,** em seguida, clique em **Connect**.

14. Na árvore da consola, expanda **a Segurança** e, em seguida, clique em **Logins**.

15. Clique em **Login à** direita e, em seguida, clique em **'Novo Início de Sessão'.**

16. Na página **Geral,** na caixa de **nomes de Login,** **escreva contoso \\ CORPCA \$**. Selecione **a autenticação do Windows**. A base de dados predefinida é **FIMCertificateManagement**.

17. No painel esquerdo, selecione **User Mapping**. No painel direito, clique na caixa de verificação na coluna **Mapa** ao lado **do FIMCertificateManagement**. Na **base de dados de função para: LISTA FIMCertificateManagement,** habilitar o papel **do clmApp.**

18. Clique em **OK**.

19. Fechar **o Microsoft SQL Server Management Studio**.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>Instalar os módulos FIM CM CA na Autoridade de Certificação

1. Certifique-se de que está ligado ao servidor **CORPCA.**

2. Nas janelas **X64,** **clique** Setup.exeà direita e, em seguida, clique em **Executar como administrador**.

3. Na página "Bem-vindo à página do Assistente de **Configuração de Gestão de Certificados de Gestão de Certificados do Gestor de Identidade da Microsoft",** clique em **Seguinte**.

4. Na página **do Contrato de Licença de Utilizador Final,** leia o contrato. Selecione os termos da caixa **de verificação do contrato de licença** e, em seguida, clique em **Seguinte**.

5. Na página **Configuração Personalizada,** selecione **o Portal MIM CM** e, em seguida, clique nesta **função não estará disponível**.

6. Na página **configuração personalizada,** selecione **o Serviço de Atualização MIM CM** e, em seguida, clique em Esta Funcionalidade não estará **disponível**.

    >[!Note]
    >Isto deixará os Ficheiros MIM CM CA como a única funcionalidade ativada para a instalação.

7. Na página **'Configuração Personalizada',** clique em **Seguinte**.

8. Na página **De Gestão de Certificados do Gestor de Identidade** do Microsoft, clique em **Instalar**.

9. Na página **'Completed' The Microsoft Identity Manager Certificate Management Wizard,** clique em **Terminar**.

10. Fechem todas as janelas abertas.

### <a name="configure-the-mim-cm-exit-module"></a>Configure o Módulo de Saída MIM CM

1. A partir de **Ferramentas Administrativas,** **Autoridade de Certificação Aberta.**

2. Na árvore da consola, clique à direita **contoso-CORPCA-CA** e, em seguida, clique em  **Propriedades**.

3. No **separador Módulo de Saída,** selecione **o Módulo de Saída FIM CM** e, em seguida, clique em  **Propriedades**.

4. Na **especificação da caixa de cadeia de ligação de base de dados CM,** tipo **Desaligá-lo Tempo limite=15; Persistência Informação de Segurança=Verdade; Segurança Integrada=sspi;Catálogo inicial=FIMCertificateManagement;Data Source=CORPSQL01**. Deixe a encriptação a caixa  **de verificação de cadeia de ligação** ativada e, em seguida, clique em **OK**.
5. Na caixa de **mensagens Microsoft FIM Certificate Management,** clique em **OK**.

6. Na caixa de diálogo **contoso-CORPCA-CA Properties,** clique **em OK**.

7. Clique à direita **contoso-CORPCA-CA** *,* aponte para Todas **as Tarefas** e, em seguida, clique em **Stop Service**. Aguarde até que os Serviços de Certificados de Diretório Ativo param.

8. Clique à direita **contoso-CORPCA-CA** *,* aponte para Todas **as Tarefas** e, em seguida, clique em **Iniciar Serviço**.

9. Minimize a consola **da Autoridade de Certificação.**

10. A partir de **Ferramentas Administrativas,** Visualizador de **Eventos aberto.**

11. Na árvore da consola, expanda os **Registos de Aplicações e Serviços** e, em seguida, clique em **Fim Certificate Management**.

12. Na lista de eventos, verifique se os eventos mais recentes *não* incluem quaisquer eventos **de Aviso** ou **Erro** desde o último reinício dos Serviços de Certificado.

    >[!NOTE] 
    >O último evento deve indicar que o Módulo de Saída carregado com configurações a partir de: `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`

13. Minimizar o **Visualizador de Eventos.**

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>Copie a impressão digital do Certificado MIMCMAgent para a área de transferência ® Windows

1. Restaurar a consola **da Autoridade de Certificação.**

2. Na árvore da consola, **expanda contoso-CORPCA-CA** e, em seguida, clique em **Certificados Emitidos**.

3. No painel de **detalhes,** clique duas vezes no certificado com **CONTOSO \\ MIMCMAgent** na coluna **Nome do Solicitador** e com **a Assinatura FIM CM** na coluna Modelo de **Certificado.**

4. No separador **Detalhes** , selecione o campo **Thumbprint**.

5. Selecione a impressão digital e, em seguida, pressione CTRL+C.

    >[!NOTE]
    >**Não** inclua o espaço principal na lista de caracteres de impressão digital.

6. Na caixa de diálogo **do Certificado,** clique **em OK**.

7. A partir do menu **Iniciar,** na caixa **de programas de pesquisa e ficheiros,** **digite o Bloco de Notas** e, em seguida, prima ENTER.

8. No **Bloco de Notas,** a partir do menu **Editar,** clique em **Pasta**.

9. A partir do menu **Editar,** clique em **Substituir.**

10. Na caixa **Localizar que** caixa, digitar um personagem de espaço e, em seguida, clicar em **Substituir Tudo**.

    >[!Note]
    >Isto remove todos os espaços entre os caracteres na impressão digital.

11. Na caixa de diálogo **Substituir,** clique em **Cancelar**.

12. Selecione o *cordão de impressão digital* convertido e, em seguida, pressione CTRL+C.

13. Feche **o Bloco de Notas** sem guardar alterações.

### <a name="configure-the-fim-cm-policy-module"></a>Configure o Módulo De Política FIM CM

1. Restaurar a consola **da Autoridade de Certificação.**

2. Clique à direita **contoso-CORPCA-CA** e, em seguida, clique em **Propriedades**.

3. Na caixa de diálogo **contoso-CORPCA-CA Properties,** no **separador Módulo de Política,** clique em **Propriedades**.

    - No separador **Geral,** certifique-se de que é selecionado **o passe não-FIM CM para o módulo de política predefinido para processamento.**

    - No **separador Certificados de Assinatura,** clique em **Adicionar**.

    - Na caixa de diálogo do Certificado, clique à direita na caixa **de haxixe codificada por hex e,** em seguida, clique em **Pasta**.

    - Na caixa de diálogo **do Certificado,** clique **em OK**.

        >[!Note]
        >Se o botão **OK** não estiver ativado, acidentalmente incluiu um personagem oculto na cadeia de impressão digital quando copiou a impressão digital do certificado clmAgent. Repita todos os passos a partir da **Tarefa 4: Copie a impressão digital do Certificado MIMCMAgent para a Área de Transferência do Windows** neste exercício.

4. Na caixa de diálogo **Configuration Properties,** certifique-se de que a impressão digital aparece na lista **de Certificados de Assinatura Válido** e, em seguida, clique em **OK**.

5. Na caixa de **mensagens FIM Certificate Management,** clique **em OK**.

6. Na caixa de diálogo **contoso-CORPCA-CA Properties,** clique **em OK**.

7. Clique à direita **contoso-CORPCA-CA** *,* aponte para Todas **as Tarefas** e, em seguida, clique em **Stop Service**.

8. Aguarde até que os Serviços de Certificados de Diretório Ativo param.

9. Clique à direita **contoso-CORPCA-CA** *,* aponte para Todas **as Tarefas** e, em seguida, clique em **Iniciar Serviço**.

10. Feche a consola **da Autoridade de Certificação.**

11. Feche todas as janelas abertas e depois faça o disparo.

**O último passo na implementação** é garantir que os CONTOSO \\ MIMCM-Managers podem implementar e criar modelos e configurar o sistema sem serem administradores de esquemas e de domínio. O próximo script irá ACL as permissões para os modelos de certificado usando dsacls. Por favor, execute com conta que tenha permissão total para alterar permissões de segurança Leia e Escreva para cada modelo de certificado existente na floresta.

Primeiros passos: **Configurar ponto de conexão de serviço e permissões de grupo alvo & delegar gestão do modelo de perfil**

1. Configure permissões no ponto de ligação de serviço (SCP).

2. Configurar a gestão do modelo de perfil delegado.

3. Configure permissões no ponto de ligação de serviço (SCP). **\<no script\>**

4.   Certifique-se de que está ligado ao servidor virtual **CORPDC.**

5. Faça login como **contoso \\ corpadmin**

6. A partir de **Ferramentas Administrativas,** abra **Utilizadores e Computadores de Diretório Ativo.**

7. Em **Utilizadores e Computadores de Diretório Ativo,** no menu **Ver,** certifique-se de que **as Funcionalidades Avançadas** estão ativadas.

8. Na árvore das consolas, expanda **Contoso.com** \| **System** \| **Microsoft** \| **Certificate Lifecycle Manager** e, em seguida, clique em **CORPCM**.

9. Clique com o botão direito **CORPCM** e, em seguida, clique em **Propriedades**.

10. Na caixa de diálogo **CORPCM Properties,** no separador **Segurança,** adicione os seguintes grupos com as permissões correspondentes:

    | Grupo          | Permissões      |
    |----------------|------------------|
    | mimcm-Managers | Ler </br> Auditoria FIM CM</br> Agente de inscrição FIM CM</br> Inscrição de pedido de PEDIDO FIM CM</br> Pedido fim CM Recuperar</br> Fim CM Pedido De renovação</br> Fim CM Pedido Revogação </br> Fim CM Pedido Desbloquear Cartão Inteligente |
    | mimcm-HelpDesk | Ler</br> Agente de inscrição FIM CM</br> Fim CM Pedido Revogação</br> Fim CM Pedido Desbloquear Cartão Inteligente |

11. Na caixa de diálogo **CORPDC Properties,** clique **em OK**.

12. Deixe **os utilizadores e computadores do Diretório Ativo abertos.**

**Configure permissões nos objetos de utilizador descendentes**

1. Certifique-se de que ainda está na consola **Ative Directory Users and Computers.**

2. Na árvore da consola, **clique** Contoso.com à direita e, em seguida, clique em **Propriedades**.

3. No separador **Segurança** , clique em **Avançadas**.

4. Nas **Definições avançadas de segurança para** a caixa de diálogo Contoso, clique em **Adicionar**.

5. Na caixa de diálogo **Select User, Computer, Service Account ou Group,** na caixa de diálogo **do objeto para selecionar** caixa, escrever **mimcm-Managers,** e clicar **em OK**.

6. Na caixa de diálogo **De entrada de Permisse para Contoso,** na lista **aplicar à** lista, selecione **objetos de utilizador descendentes** e, em seguida, ative a caixa de verificação **Permitir** as **seguintes Permissões** :

    - **Ler todas as propriedades**

    - **Ler permissões**

    - **Auditoria FIM CM**

    - **Agente de inscrição FIM CM**

    - **Inscrição de pedido de PEDIDO FIM CM**

    - **Pedido fim CM Recuperar**

    - **Fim CM Pedido De renovação**

    - **Fim CM Pedido Revogação**

    - **Fim CM Pedido Desbloquear Cartão Inteligente**

7. Na caixa de diálogo De Acesso a **Contador de Pontos de Banho para Contoso,** clique **em OK**.

8. Nas **Definições avançadas de segurança para** a caixa de diálogo Contoso, clique em **Adicionar**.

9. Na caixa de diálogo **Select User, Computer, Service Account ou Group,** na caixa de diálogo **do objeto para selecionar** caixa, escrever **mimcm-HelpDesk** e, em seguida, clicar **OK**.

10. Na caixa de diálogo **De entrada de Permisse para Contoso,** na lista **aplicar à** lista, selecione **objetos de utilizador descendentes** e, em seguida, selecione a caixa de verificação **'Permitir'** para as **seguintes Permissões** :

    - **Ler todas as propriedades**

    - **Ler permissões**

    - **Agente de inscrição FIM CM**

    - **Fim CM Pedido Revogação**

    - **Fim CM Pedido Desbloquear Cartão Inteligente**

11. Na caixa de diálogo De Acesso a **Contador de Pontos de Banho para Contoso,** clique **em OK**.

12. Nas **Definições avançadas de segurança para** a caixa de diálogo Contoso, clique em **OK**.

13. Na caixa de diálogo **contoso.com Properties,** clique **em OK**.

14. Deixe **os utilizadores e computadores do Diretório Ativo abertos.**

**Configure permissões nos objetos de utilizador descendentes \<no script\>**

1. Certifique-se de que ainda está na consola **Ative Directory Users and Computers.**

2. Na árvore da consola, **clique** Contoso.com à direita e, em seguida, clique em **Propriedades**.

3. No separador **Segurança** , clique em **Avançadas**.

4. Nas **Definições avançadas de segurança para** a caixa de diálogo Contoso, clique em **Adicionar**.

5. Na caixa de diálogo **Select User, Computer, Service Account ou Group,** na caixa de diálogo **do objeto para selecionar** caixa, escrever **mimcm-Managers,** e clicar **em OK**.

6. Na caixa de diálogo **de entrada de permissões para CONTOSO,** na lista **aplicar à** lista, selecione **objetos de utilizador descendentes** e, em seguida, ative a caixa de verificação **Permitir** as **seguintes permissões** :

    - **Ler todas as propriedades**

    - **Ler permissões**

    - **Auditoria FIM CM**

    - **Agente de inscrição FIM CM**

    - **Inscrição de pedido de PEDIDO FIM CM**

    - **Pedido fim CM Recuperar**

    - **Fim CM Pedido De renovação**

    - **Fim CM Pedido Revogação**

    - **Fim CM Pedido Desbloquear Cartão Inteligente**

7. Na caixa de diálogo **de entrada de permissões para CONTOSO,** clique **em OK**.

8. Nas **Definições avançadas de segurança para** a caixa de diálogo CONTOSO, clique em **Adicionar**.

9. Na caixa de diálogo **Select User, Computer, Service Account ou Group,** na caixa de diálogo **do objeto para selecionar** caixa, escrever **mimcm-HelpDesk** e, em seguida, clicar **OK**.

10. Na caixa de diálogo **de entrada de permissões para CONTOSO,** na **lista,** selecione **objetos de utilizador descendentes** e, em seguida, selecione a caixa de verificação **'Permitir'** para as **seguintes Permissões** :

    - **Ler todas as propriedades**

    - **Ler permissões**

    - **Agente de inscrição FIM CM**

    - **Fim CM Pedido Revogação**

    - **Fim CM Pedido Desbloquear Cartão Inteligente**

11. Na Caixa de **Diálogo De Permissão para contoso,** clique **em OK**.

12. Nas **Definições avançadas de segurança para** a caixa de diálogo Contoso, clique em **OK**.

13. Na caixa de diálogo **contoso.com Properties,** clique **em OK**.

14. Deixe **os utilizadores e computadores do Diretório Ativo abertos.**

Segundas etapas: **Delegating \<script\> Certificado De Gestão de Modelos**

- Delegando permissões no recipiente De Modelos de Certificado.

- Delegando permissões no contentor OID.

- Delegando permissões nos modelos de certificado existentes.

Defina permissões no recipiente de modelos de certificado:

1. Restaurar a consola **De Sites e Serviços de Diretório Ativo.**

2. Na árvore das **consolas,** expanda os Serviços, expanda **os Serviços de Chaves Públicas** e, em seguida, clique em **Modelos de Certificados**.

3. Na árvore da consola, clique com o botão direito **Modelos de Certificado** e, em seguida, clique em **Controlo de Delegados**.

4. Na **Delegação do** Assistente de Controlo, clique **em Seguinte**.

5. Na página **De Utilizadores ou Grupos,** clique em **Adicionar**.

6. Na caixa de diálogo **Select Users, Computers ou Groups,** na caixa de diálogo **'Introduza os nomes do objecto' para selecionar** caixa, **escreva mimcm-Managers** e, em seguida, clique **em OK**.

7. Na página **Utilizadores ou Grupos,** clique em **Seguinte**.

8. Na página **Tarefas a Delegar,** clique em **Criar uma tarefa personalizada para delegar** e, em seguida, clique em **Seguinte**.

9.  Na página **Ative Directory Object Type,** certifique-se de que **esta pasta, os objetos existentes nesta pasta e a criação de novos objetos nesta pasta** são selecionados e, em seguida, clique em **Seguinte**.

10. Na página **Permissões,** na lista **de Permisses,** selecione a caixa de verificação **de Controlo Total** e, em seguida, clique em **Seguinte**.

11. Na página **"Completar a Delegação do Assistente de Controlo",** clique em **Terminar**.

Defina permissões no recipiente OID:

1. Na árvore da consola, clique com o botão direito **OID** e, em seguida, clique em **Propriedades**.

2. Na caixa de diálogo **OID Properties,** no separador **Segurança,** clique em **Advanced**.

3. Nas **Definições avançadas de segurança para a** caixa de diálogo OID, clique em **Adicionar**.

4. Na caixa de diálogo **Select User, Computer, Service Account ou Group,** na caixa de diálogo **do objeto para selecionar** caixa, escrever **mimcm-Managers,** e clicar **em OK**.

5. Na caixa de diálogo **de permissões,** certifique-se de que as permissões se aplicam a **Este objeto e a todos os objetos descendentes,** clique em **Full Control** e, em seguida, clique em **OK**.

6. Nas **Definições avançadas de segurança para a** caixa de diálogo OID, clique em **OK**.

7. Na caixa de diálogo **OID Properties,** clique **em OK**.

8. Fechar **Sites e Serviços de Diretório Ativo.**

**Scripts: Permissões no OID, Modelo de perfil & certificados de design**

![diagrama](media/mim-cm-deploy/image021.png)

```powershell
import-module activedirectory
$adace = @{
"OID" = "AD:\\CN=OID,CN=Public Key Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"CT" = "AD:\\CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"PT" = "AD:\\CN=Profile Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com"
}
$adace.GetEnumerator() | **Foreach-Object** {
$acl = **Get-Acl** *-Path* $_.Value
$sid=(**Get-ADGroup** "MIMCM-Managers").SID
$p = **New-Object** System.Security.Principal.SecurityIdentifier($sid)
##https://msdn.microsoft.com/library/system.directoryservices.activedirectorysecurityinheritance(v=vs.110).aspx
$ace = **New-Object** System.DirectoryServices.ActiveDirectoryAccessRule
($p,[System.DirectoryServices.ActiveDirectoryRights]"GenericAll",[System.Security.AccessControl.AccessControlType]::Allow,
[DirectoryServices.ActiveDirectorySecurityInheritance]::All)
$acl.AddAccessRule($ace)
**Set-Acl** *-Path* $_.Value *-AclObject* $acl
}
```

**Scripts: Delegando permissões nos modelos de certificado existentes.**  

![diagrama](media/mim-cm-deploy/image039.png)

```shell
dsacls "CN=Administrator,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CAExchange,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CEPEncryption,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ClientAuth,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CodeSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CrossCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CTLSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DirectoryEmailReplication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainController,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainControllerAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFS,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFSRecovery,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgentOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMKeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOnline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KerberosAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Machine,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=MachineEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OCSPResponseSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OfflineRouter,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=RASAndIASServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardLogon,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SubCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=User,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=UserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=WebServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Workstation,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO
```
