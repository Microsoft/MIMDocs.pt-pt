---
title: Implantando Microsoft Identity Manager Gerenciador de certificados | Microsoft Docs
description: Instalar Microsoft Identity Manager Gerenciador de certificados 2016
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 9a9e00f7dca118627a5140967a104d13273cbc26
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "67690807"
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Implantando Microsoft Identity Manager Gerenciador de certificados 2016 (MIM CM)

A instalação do Microsoft Identity Manager Gerenciador de certificados 2016 (MIM CM) envolve várias etapas. Como uma maneira de simplificar o processo, estamos dividindo as coisas. Há etapas preliminares que devem ser executadas antes de qualquer etapa de MIM CM real. Sem o trabalho preliminar, a instalação provavelmente falhará.

O diagrama a seguir mostra um exemplo do tipo de ambiente que pode ser usado. Os sistemas com números estão incluídos na lista abaixo do diagrama e são necessários para concluir com êxito as etapas abordadas neste artigo. Por fim, os servidores Windows 2016 datacenter são usados:

![Diagrama de ambiente](media/mim-cm-deploy/image001.png)

1. CORPDC – controlador de domínio
2. CORPCM – servidor de MIM CM
3. CORPCA – autoridade de certificação
4. CORPCMR – MIM CM da API REST – portal CM para API REST – usado para uso posterior
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1 – ingressado no domínio do Windows 10

## <a name="deployment-overview"></a>Descrição geral da implementação

- Instalação do sistema operacional base

    O laboratório consiste em servidores Windows 2016 datacenter.

    >[!NOTE]
    >Para obter mais detalhes sobre as plataformas com suporte para o MIM 2016, dê uma olhada no artigo intitulado [plataformas com suporte para mim 2016](microsoft-identity-manager-2016-supported-platforms.md).

1. Etapas de pré-implantação

    - [Estendendo o esquema](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)

    - Criando contas de serviço

    - [Criando modelos de certificado](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)

    - IIS

    - Configurando o Kerberos

    - Etapas relacionadas ao banco de dados

        - Requisitos de configuração do SQL

        - Permissões de banco de dados

2. Automática

## <a name="pre-deployment-steps"></a>Etapas de pré-implantação

O assistente de configuração do MIM CM exige que as informações sejam fornecidas ao longo do caminho para que sejam concluídas com êxito.

![diagrama](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>Extensão do esquema

O processo de estender o esquema é simples, mas deve ser abordado com cautela devido à sua natureza irreversível.

>[!NOTE]
>Esta etapa requer que a conta usada tenha direitos de administrador de esquema.

1. Navegue até o local da mídia do MIM e navegue até \\gerenciamento de certificado\\pasta x64.

2. Copie a pasta de esquema para CORPDC e, em seguida, navegue até ela.

    ![diagrama](media/mim-cm-deploy/image005.png)

3. Execute o cenário de floresta única do script resourceForestModifySchema. vbs. Para o cenário de floresta de recursos, execute os scripts:
   - Domaina – usuários localizados (userForestModifySchema. vbs)
   - ResourceForestB – local da instalação do CM (resourceForestModifySchema. vbs).

     >[!NOTE]
     >As alterações de esquema são uma operação unidirecional e exigem uma recuperação de floresta para serem revertidas para que você tenha os backups necessários. Para obter detalhes sobre as alterações feitas no esquema executando esta operação, examine o artigo [Forefront Identity Manager 2010 certificado Management Schema Changes](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx)

     ![diagrama](media/mim-cm-deploy/image007.png)

4. Execute o script e você deverá receber uma mensagem de êxito quando o script for concluído.

    ![Mensagem de êxito](media/mim-cm-deploy/image009.png)

O esquema no AD agora é estendido para dar suporte a MIM CM.

### <a name="creating-service-accounts-and-groups"></a>Criando contas de serviço e grupos

A tabela a seguir resume as contas e permissões exigidas pelo MIM CM. Você pode permitir que o MIM CM crie as contas a seguir automaticamente ou possa criá-las antes da instalação. Os nomes de conta reais podem ser alterados. Se você criar as contas por conta própria, considere nomear as contas de usuário nesse sentido de que é fácil corresponder o nome da conta de usuário à sua função.

Utilizadores:

![Diagrama](media/mim-cm-deploy/image010.png)

![Diagrama](media/mim-cm-deploy/image012.png)

| **Função**                   | **Nome de logon do usuário** |
|----------------------------|---------------------|
| Agente de MIM CM               | MIMCMAgent          |
| Agente de recuperação de chave MIM CM  | MIMCMKRAgent        |
| MIM CM agente de autorização | MIMCMAuthAgent      |
| Agente do Gerenciador de AC MIM CM    | MIMCMManagerAgent   |
| MIM CM agente de pool da Web      | MIMCMWebAgent       |
| Agente de registro MIM CM    | MIMCMEnrollAgent    |
| Serviço de atualização de MIM CM      | MIMCMService        |
| Conta de instalação do MIM        | MIMINSTALL          |
| Agente de suporte técnico            | CMHelpdesk1-2       |
| Gerenciador CM                 | CMManager1-2        |
| Usuário do assinante            | CMUser1-2           |

Grupos:

| **Função**               | **Grupo**         |
|------------------------|-------------------|
| Membros da assistência técnica CM    | MIMCM-Helpdesk    |
| Membros do Gerenciador CM     | MIMCM-gerentes    |
| Membros dos assinantes do CM | MIMCM-assinantes |

PowerShell: contas de agente:

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

| **Nome de logon do usuário** | **Descrição e permissões**   |
|------|---------------------|
| MIMCMAgent          | Fornece os seguintes serviços: </br>-Recupera chaves privadas criptografadas da AC. </br>-Protege informações de PIN do cartão inteligente no banco de dados do FIM CM. </br>-Protege a comunicação entre o FIM CM e a CA. </br></br> Essa conta de usuário requer as seguintes configurações de controle de acesso:</br>direito de usuário -   **Permitir logon local** .</br>direito **de usuário -   emitir e gerenciar certificados** . </br>-Permissão de leitura e gravação na pasta Temp do sistema no seguinte local:% WINDIR%\\Temp.</br>-Uma assinatura digital e um certificado de criptografia emitidos e instalados no armazenamento do usuário.
|MIMCMKRAgent        | Recupera chaves privadas arquivadas da AC. Essa conta de usuário requer as seguintes configurações de controle de acesso:</br> direito de usuário -   **Permitir logon local** .</br>-Associação no grupo local de **Administradores** . </br>-Permissão de registro no modelo de certificado **KeyRecoveryAgent** . </br>-O certificado do agente de recuperação de chave é emitido e instalado no armazenamento do usuário. O certificado deve ser adicionado à lista de agentes de recuperação de chave na autoridade de certificação. </br>-Permissões de leitura e permissão de gravação na pasta Temp do sistema no seguinte local: ```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | Determina os direitos e permissões de usuário para usuários e grupos. Essa conta de usuário requer as seguintes configurações de controle de acesso: </br>-Associação no grupo de domínio de acesso compatível com o anterior ao Windows 2000. </br> -Concedido o direito de usuário **gerar auditorias de segurança** .             |
| MIMCMManagerAgent   | Executa atividades de gerenciamento de CA. </br> A permissão Manage CA deve ser atribuída a esse usuário.        |
| MIMCMWebAgent       | Fornece a identidade para o pool de aplicativos do IIS. O FIM CM é executado em um processo de interface de programação de aplicativo® Microsoft Win32 que usa as credenciais desse usuário. </br> Essa conta de usuário requer as seguintes configurações de controle de acesso:</br> -Associação no IIS_WPG local **, windows 2016 =** grupo de IIS_IUSRS. </br>-Associação no grupo local de **Administradores** .</br>-Concedido o direito de usuário **gerar auditorias de segurança** . </br>-Foi concedido o direito de usuário **atuar como parte do sistema operacional** . </br>-Foi concedido o direito de usuário **substituir token de nível de processo** .</br>-Atribuído como a identidade do pool de aplicativos do IIS, **CLMAppPool**. </br>-Permissão de leitura concedida na **HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\CLM\\v 1.0\\Server\\** chave do registro do WebUser. </br>-Essa conta também deve ser confiável para delegação.|
| MIMCMEnrollAgent    | Executa o registro em nome de um usuário. Essa conta de usuário requer as seguintes configurações de controle de acesso:</br>-Um certificado de agente de registro que é emitido e instalado no armazenamento do usuário.</br>direito de usuário -   **Permitir logon local** . </br>-Permissão de registro no modelo de certificado do **agente de registro** (ou no modelo personalizado, se um for usado).                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Criando modelos de certificado para contas de serviço MIM CM

Três das contas de serviço usadas pelo MIM CM exigem um certificado e o assistente de configuração requer que você forneça o nome dos modelos de certificados que ele deve usar para solicitar certificados para eles.

As contas de serviço que exigem certificados são:

- MIMCMAgent: essa conta precisa de um certificado de usuário

- MIMCMEnrollAgent: essa conta precisa de um certificado de agente de registro

- MIMCMKRAgent: essa conta precisa de um certificado de **agente de recuperação de chave**

Já existem modelos presentes no AD, mas precisamos criar nossas próprias versões para trabalhar com MIM CM. Como precisamos para fazer modificações a partir dos modelos de linha de base originais.

Todas as três contas acima terão direitos elevados em sua organização e deverão ser tratadas com cuidado.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>Criar o modelo de certificado de assinatura MIM CM

1. Em **Ferramentas administrativas**, abra **autoridade de certificação**.

2. No console da **autoridade de certificação** , na árvore de console, expanda **contoso-CorpCA**e clique em **modelos de certificado**.

3. Clique com o botão direito do mouse em **modelos de certificado**e clique em **gerenciar**.

4. No **console modelos de certificado**, no painel de **detalhes** , selecione e clique com o botão direito do mouse em **usuário**e clique em **duplicar modelo**.

5. Na caixa de diálogo **duplicar modelo** , selecione **Windows Server 2003 Enterprise**e clique em **OK**.

    ![Mostrar alterações resultantes](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    >MIM CM não funciona com certificados com base nos modelos de certificado da versão 3. Você deve criar um modelo de certificado do Windows Server® 2003 Enterprise (versão 2). Consulte [detalhes do v3](https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade) para obter mais informações.

6. Na caixa **de diálogo Propriedades do novo modelo** , na guia **geral** , na caixa **nome de exibição do modelo** , digite **mim cm assinatura**. Altere o **período de validade** para **2 anos**e desmarque a caixa de seleção **publicar certificado no Active Directory** .

7. Na guia **tratamento de solicitação** , verifique se a caixa de seleção permitir que a **chave privada seja exportada** está marcada e clique **na guia criptografia**.

8. Na caixa de diálogo **seleção de criptografia** , desabilite **o provedor criptográfico avançado da Microsoft v 1.0**, habilite **o provedor criptográfico RSA avançado da Microsoft e o AES**e clique em **OK**.

9. Na guia **nome da entidade** , desmarque as caixas de seleção **incluir nome de email no nome da entidade** e **nome de email** .

10. Na guia **extensões** , na lista **extensões incluídas neste modelo** , verifique se **políticas de aplicativo** está selecionada e clique em **Editar**.

11. Na caixa de diálogo **Editar extensão de políticas de aplicativo** , selecione as políticas de **Encrypting File System** e de aplicativo de **email seguro** . Clique em **remover**e em **OK**.

12. Na guia **segurança** , execute as seguintes etapas:

    - Remova o **administrador**.

    - Remova os **Administradores de domínio**.

    - Remova **usuários do domínio**.

    - Atribua somente permissões de **leitura** e **gravação** a **administradores corporativos**.

    - Adicione **MIMCMAgent.**

    - Atribua permissões de **leitura** e **registro** ao **MIMCMAgent**.

13. Na caixa **de diálogo Propriedades do novo modelo** , clique em **OK**.

14. Deixe o **console modelos de certificado** aberto.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>Criar o modelo de certificado do agente de registro MIM CM

1. No **console modelos de certificado**, no painel de **detalhes** , selecione e clique com o botão direito do mouse em **agente de registro**e clique em **duplicar modelo**.

2. Na caixa de diálogo **duplicar modelo** , selecione **Windows Server 2003 Enterprise**e clique em **OK**.

3. Na caixa **de diálogo Propriedades do novo modelo** , na guia **geral** , na caixa **nome de exibição do modelo** , digite **mim cm agente de registro**. Verifique se o **período de validade** é de **2 anos**.

4. Na guia **tratamento de solicitação** , habilite **permitir que a chave privada seja exportada**e clique **na guia CSPs ou criptografia.**

5. Na caixa de diálogo **seleção de CSP** , desabilite **o provedor criptográfico do Microsoft base v 1.0**, desabilite **o provedor criptográfico avançado da Microsoft v 1.0**, habilite **o provedor criptográfico RSA aprimorado da Microsoft**e clique em **OK**.

6. Na guia **segurança** , execute o seguinte:

    - Remova o **administrador**.

    - Remova os **Administradores de domínio**.

    - Atribua somente permissões de **leitura** e **gravação** a **administradores corporativos**.

    - Adicione **MIMCMEnrollAgent**.

    - Atribua permissões de **leitura** e **registro** ao **MIMCMEnrollAgent**.

7. Na caixa **de diálogo Propriedades do novo modelo** , clique em **OK**.

8. Deixe o **console modelos de certificado** aberto.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>Criar o modelo de certificado do agente de recuperação de chave MIM CM

1. No console **modelos de certificado** , no painel **de detalhes** , selecione e clique com o botão direito do mouse em agente de **recuperação de chave**e clique em **duplicar modelo**.

2. Na caixa de diálogo **duplicar modelo** , selecione **Windows Server 2003 Enterprise**e clique em **OK**.

3. Na caixa **de diálogo Propriedades do novo modelo** , na guia **geral** , na caixa **nome de exibição do modelo** , digite **mim cm agente de recuperação de chave**. Verifique se o **período de validade** é de **2 anos** na **guia criptografia.**

4. Na caixa de diálogo **seleção de provedores** , desabilite **o provedor criptográfico avançado da Microsoft v 1.0**, habilite **o provedor criptográfico RSA avançado da Microsoft e o AES**e clique em **OK**.

5. Na guia **requisitos de emissão** , verifique se a **aprovação do Gerenciador de certificados de autoridade de certificação** está **desabilitada**.

6. Na guia **segurança** , execute o seguinte:

    - Remova o **administrador**.

    - Remova os **Administradores de domínio**.

    - Atribua somente permissões de **leitura** e **gravação** a **administradores corporativos**.

    - Adicione **MIMCMKRAgent**.

    - Atribua permissões de **leitura** e **registro** ao **KRAgent**.

7. Na caixa **de diálogo Propriedades do novo modelo** , clique em **OK**.

8. Feche a **Consola de Modelos de Certificado**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>Publicar os modelos de certificado necessários na autoridade de certificação

1. Restaure o console da **autoridade de certificação** .

2. No console **da autoridade de certificação** , na árvore de console, clique com o botão direito do mouse em modelos de **certificado**, aponte para **novo**e clique em **modelo de certificado para emitir**.

3. Na caixa de diálogo **habilitar modelos de certificado** , **selecione mim cm agente de registro**, **mim cm agente de recuperação de chave**e assinatura de **mim cm**. Clique em **OK**.

4. Na árvore de console, clique em **modelos de certificado**.

5. Verifique se os três novos modelos aparecem no painel de **detalhes** e, em seguida, feche a **autoridade de certificação**.

    ![Assinatura de MIM CM](media/mim-cm-deploy/image016.png)

6. Feche todas as janelas abertas e faça logoff.

### <a name="iis-configuration"></a>Configuração do IIS

Para hospedar o site para o CM, instale e configure o IIS.

#### <a name="install-and-configure-iis"></a>Instalar e configurar o IIS

1. Faça logon **no CORLog** como conta do **MIMINSTALL**

    >[!IMPORTANT]
    >A conta de instalação do MIM deve ser um administrador local

2. Abra o PowerShell e execute o seguinte comando

    `Install-WindowsFeature –ConfigurationFilePath`

>[!NOTE]
>Um site chamado site padrão é instalado por padrão com o IIS 7. Se esse site tiver sido renomeado ou removido, um site com o nome site padrão deverá estar disponível para que o MIM CM possa ser instalado.

#### <a name="configuring-kerberos"></a>Configurando o Kerberos

A conta MIMCMWebAgent estará executando o portal MIM CM. Por padrão, no IIS, a autenticação do modo kernel é usada no IIS por padrão. Você desabilitará a autenticação do modo kernel Kerberos e configurará os SPNs na conta MIMCMWebAgent. Alguns comandos exigirão permissões elevadas no Active Directory e no CORPCM Server.

![Diagrama](media/mim-cm-deploy/image020.png)

```powershell
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}
```

**Atualizando o IIS no CORPCM**

![diagrama](media/mim-cm-deploy/image022.png)

```powershell
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true
```

>[!NOTE]
>Você precisará adicionar um registro a do DNS para o "cm.contoso.com" e apontar para o IP do CORPCM

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>Exigindo SSL no portal de MIM CM

É altamente recomendável que você exija SSL no portal de MIM CM. Se você não for o assistente, ele mesmo o avisará sobre ele.

1. Registrar no certificado da Web para **cm.contoso.com** atribuir ao site padrão

2. Abra o **Gerenciador do IIS** e navegue até gerenciamento de **certificados**

3. Na Vista de Funcionalidades, faça duplo clique em Definições de SSL.

4. Na página Configurações de SSL, selecione **exigir SSL**.

5. No painel Ações, clique em **aplicar.**

### <a name="database-configuration-corpsql-for-mim-cm"></a>**CORPSQL** de configuração do banco de dados para mim cm

1. Verifique se você está conectado ao servidor CORPSQL01.

2. Verifique se você está conectado como DBA do SQL.

3. Execute o seguinte script T-SQL para permitir que a conta CONTOSO\\MIMINSTALL crie o banco de dados quando passarmos para a etapa de configuração

    >[!NOTE]
    >Precisaremos voltar ao SQL quando estivermos prontos para o módulo de política de & de saída

    ```sql
    create login [CONTOSO\\MIMINSTALL] from windows;
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
    ```

![MIM CM mensagem de erro do assistente de configuração](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Implantação do gerenciamento de certificados Microsoft Identity Manager 2016

1. Verifique se você está conectado ao servidor CORPCM e se a conta **MIMINSTALL** é membro do grupo local de **Administradores** .

2. Verifique se você está conectado como contoso\\MIMINSTALL.

3. Monte a ISO do Microsoft Identity Manager SP1.

4. **Abra** o diretório **\\x64 do gerenciamento de certificados** .

5. Na janela **x64** , clique com o botão direito do mouse em **instalação**e clique em **Executar como administrador**.

6. Na página Bem-vindo ao assistente de instalação do Microsoft Identity Manager Certificate Management, clique em **Avançar.**

7. Na página contrato de licença de usuário final, leia o contrato, habilite a **caixa de seleção**aceito os termos do contrato de licença e clique em Avançar.

8. Na página instalação personalizada, verifique se o **portal do mim cm** e **mim cm componentes do serviço de atualização** estão configurados para serem instalados e clique em **Avançar**.

9. Na página pasta da Web virtual, verifique se o nome da pasta virtual é **CertificateManagement**e **clique em Avançar**.

10. Na página instalar Microsoft Identity Manager Certificate Management, **clique em instalar**.

11. Na página Assistente de instalação do Microsoft Identity Manager Certificate Management **concluído** , **clique em concluir**.

![Assistente de MIM CM concluído](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Assistente de configuração do Microsoft Identity Manager gerenciamento de certificados 2016

Antes de fazer logon no CORPCM, adicione MIMINSTALL a **Administradores de domínio, administradores de esquema e grupo de administradores locais** para o assistente de configuração. Isso pode ser removido mais tarde, uma vez que a configuração estiver concluída.

![Mensagem de erro](media/mim-cm-deploy/image028.png)

1. No menu **Iniciar** , clique em **Assistente de configuração do Certificate Management**. E executar como **administrador**

2. Na página **Bem-vindo ao assistente de configuração** , clique em **Avançar**.

3. Na página **configuração da AC** , verifique se a AC selecionada é **contoso-CORPCA-CA**, verifique se o servidor selecionado é **CORPCA. CONTOSO.COM**e clique em **Avançar**.

4. Na página **Configurar o banco de dados do Microsoft® SQL Server®** , na caixa **nome da SQL Server** , digite **CORPSQL1** , habilite a caixa de seleção **usar minhas credenciais para criar o banco de dados** e clique em **Avançar**.

5. Na página **configurações do banco de dados** , aceite o nome de banco de dados padrão **FIMCertificateManagement**, verifique se a **autenticação integrada do SQL** está selecionada e clique em **Avançar**.

6. Na página **configurar Active Directory** , aceite o nome padrão fornecido para o ponto de conexão de serviço e clique em **Avançar**.

7. Na página **método de autenticação** , confirme se a **autenticação integrada do Windows** está selecionada e clique em **Avançar**.

8. Na página **agentes – fim cm** , desmarque a caixa de seleção **usar as configurações padrão do fim cm** e clique em **contas personalizadas**.

9. Na caixa de diálogo **agentes – fim cm** de várias guias, em cada guia, digite as seguintes informações:

   - Nome de usuário: **Atualizar**

   - Senha: **Pass\@word1**

   - Confirmar senha: **Pass\@word1**

   - Usar um usuário existente: **habilitado**

     >[!NOTE]
     >Criamos essas contas anteriormente. Certifique-se de que os procedimentos na etapa 8 sejam repetidos para todas as seis guias de conta de agente.

     ![Contas de MIM CM](media/mim-cm-deploy/image030.png)

10. Quando todas as informações da conta do agente forem concluídas, clique em **OK**.

11. Na página **agentes – mim cm** , clique em **Avançar**.

12. Na página **configurar certificados do servidor** , habilite os seguintes modelos de certificado:

    - Modelo de certificado a ser usado para o certificado do agente de recuperação de chave do agente de recuperação: **MIMCMKeyRecoveryAgent**.

    - Modelo de certificado a ser usado para o certificado do agente do FIM CM: **MIMCMSigning**.

    - Modelo de certificado a ser usado para o certificado do agente de registro: **FIMCMEnrollmentAgent**.

13. Na página **configurar certificados do servidor** , clique em **Avançar**.

14. Na página **servidor de email de instalação, impressão de documentos** , na caixa **especificar o nome do servidor SMTP que você deseja usar para notificações de registro de email** e clique em **Avançar.**

15. Na página **Preparado para configurar**, clique em **Configurar**.

16. Na caixa de diálogo de aviso **Assistente de configuração – Microsoft Forefront Identity Manager 2010 R2** , clique em **OK** para confirmar que o SSL não está habilitado no diretório virtual do IIS.

    ![mídia/image17. png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    >Não clique no botão concluir até que a execução do assistente de configuração seja concluída. O registro em log do assistente pode ser encontrado aqui: **% ProgramFiles%\\Microsoft Forefront Identity management\\2010\\gerenciamento de certificados\\config. log**

17. Clique em **Concluir**.

    ![Assistente de MIM CM concluído](media/mim-cm-deploy/image033.png)

18. Feche todas as janelas abertas.

19. Adicione `https://cm.contoso.com/certificatemanagement` à zona da intranet local em seu navegador.

20. Visite o site do servidor CORPCM `https://cm.contoso.com/certificatemanagement`  

    ![diagrama](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>Verificar o serviço de isolamento de chave CNG

1. Em **Ferramentas administrativas**, abra **Serviços**.

2. No painel de **detalhes** , clique duas vezes em **isolamento de chave CNG**.

3. Na guia **geral** , altere o **tipo de inicialização** para **automático**.

4. Na guia **geral** , inicie o serviço se ele não estiver em um estado iniciado.

5. Na guia **geral** , clique em **OK**.

### <a name="installing-and-configuring-the-ca-modules-"></a>Instalando e configurando os módulos de CA:

Nesta etapa, vamos instalar e configurar os módulos de AC do FIM CM na autoridade de certificação.

1. Configurar o FIM CM para inspecionar apenas as permissões de usuário para operações de gerenciamento

2. Na janela **C:\\arquivos de programas\\Microsoft Forefront Identity Manager\\2010\\gerenciamento de certificados\\** , faça uma cópia do **Web. config** que nomeia a cópia **Web. 1. config**.

3. Na janela **da Web** , clique com o botão direito do mouse em **Web. config**e clique em **abrir**.

    >[!Note]
    >O arquivo Web. config é aberto no bloco de notas

4. Quando o arquivo for aberto, pressione CTRL + F.

5. Na caixa de diálogo **Localizar e substituir** , na caixa **Localizar** , digite **UseUser**e clique em **Localizar próxima** três vezes.

6. Feche a caixa de diálogo **Localizar e substituir** .

7. Você deve estar na linha **\<adicionar Key = "CLM. RequestSecurity. Flags" value = "UseUser, UseGroups"/\>** . Altere a linha para ler **\<adicionar chave = "CLM. RequestSecurity. Flags" valor = "UseUser"/\>** .

8. Feche o arquivo, salvando todas as alterações.

9. Crie uma conta para o computador de autoridade de certificação no SQL Server \<nenhum script\>

10. Verifique se você está conectado ao servidor **CORPSQL01** .

11. Verifique se você está conectado como **DBA**

12. No menu **Iniciar** , inicie o **SQL Server Management Studio**.

13. Na caixa de diálogo **conectar ao servidor** , na caixa **nome do servidor** , digite **CORPSQL01** e clique em **conectar**.

14. Na árvore de console, expanda **segurança**e clique em **logons**.

15. Com o botão direito **inícios de sessão**e, em seguida, clique em **novo início de sessão**.

16. Na página **geral** , na caixa **nome de logon** , digite **contoso\\CORPCA\$** . Selecione **autenticação do Windows**. O banco de dados padrão é **FIMCertificateManagement**.

17. No painel esquerdo, selecione **mapeamento de usuário**. No painel direito, clique na caixa de seleção na coluna **mapa** ao lado de **FIMCertificateManagement**. Na lista **Associação de função de banco de dados para: FIMCertificateManagement** , habilite a função **clmApp** .

18. Clique em **OK**.

19. Fechar **Microsoft SQL Server Management Studio**.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>Instalar os módulos de AC do FIM CM na autoridade de certificação

1. Verifique se você está conectado ao servidor **CORPCA** .

2. Nas janelas **x64** , clique com o botão direito do mouse em **Setup. exe**e clique em **Executar como administrador**.

3. Na página **Bem-vindo ao assistente de instalação do Microsoft Identity Manager Certificate Management** , clique em **Avançar**.

4. Na página **contrato de licença de usuário final** , leia o contrato. Marque a caixa de seleção **eu aceito os termos do contrato de licença** e clique em **Avançar**.

5. Na página **instalação personalizada** , selecione **mim cm portal**e, em seguida, clique em **esse recurso não estará disponível**.

6. Na página **instalação personalizada** , selecione **mim cm serviço de atualização**e, em seguida, clique em **este recurso não estará disponível**.

    >[!Note]
    >Isso deixará o MIM CM arquivos de AC como o único recurso habilitado para a instalação.

7. Na página **instalação personalizada** , clique em **Avançar**.

8. Na página **instalar Microsoft Identity Manager Certificate Management** , clique em **instalar**.

9. Na página **Assistente de instalação do Microsoft Identity Manager Certificate Management concluído** , clique em **concluir**.

10. Feche todas as janelas abertas.

### <a name="configure-the-mim-cm-exit-module"></a>Configurar o módulo de saída de MIM CM

1. Em **Ferramentas administrativas**, abra **autoridade de certificação**.

2. Na árvore de console, clique com o botão direito do mouse em **contoso-CORPCA-CA**e clique em **Propriedades**.

3. Na guia **módulo de saída** , selecione **módulo de saída do fim cm**e clique em **Propriedades**.

4. Na caixa **especificar a cadeia de conexão do banco de dados cm** , digite **Connect Timeout = 15; Informações de persistência de segurança = verdadeiro; Segurança integrada = SSPI; catálogo inicial = FIMCertificateManagement; fonte de dados = CORPSQL01**. Deixe a caixa de seleção **criptografar a cadeia de conexão** habilitada e clique em **OK**.
5. Na caixa de mensagem **Gerenciamento de certificados do Microsoft fim** , clique em **OK**.

6. Na caixa de diálogo **Propriedades da Contoso-CORPCA-CA** , clique em **OK**.

7. Clique com o botão direito do mouse em **contoso-CORPCA-CA** *,* aponte para **todas as tarefas**e clique em **parar serviço**. Aguarde até que deixa de serviços de certificados do Active Directory.

8. Clique com o botão direito do mouse em **contoso-CORPCA-CA** *,* aponte para **todas as tarefas**e clique em **Iniciar serviço**.

9. Minimize o console da **autoridade de certificação** .

10. Em **Ferramentas administrativas**, abra **Visualizador de eventos**.

11. Na árvore de console, expanda **logs de aplicativos e serviços**e clique em **Gerenciamento de certificados do fim**.

12. Na lista de eventos, verifique se os eventos mais recentes *não* incluem nenhum evento de **aviso** ou **erro** desde a última reinicialização dos serviços de certificados.

    >[!NOTE] 
    >O último evento deve indicar que o módulo de saída foi carregado usando as configurações de: `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`

13. Minimize o **Visualizador de eventos**.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>Copie a impressão digital do certificado MIMCMAgent para a área de transferência do Windows®

1. Restaure o console da **autoridade de certificação** .

2. Na árvore de console, expanda **contoso-CORPCA-CA**e clique em **certificados emitidos**.

3. No painel de **detalhes** , clique duas vezes no certificado com **contoso\\MIMCMAgent** na coluna **nome do solicitante** e com a **assinatura do fim cm** na coluna **modelo de certificado** .

4. No separador **Detalhes**, selecione o campo **Thumbprint**.

5. Selecione a impressão digital e pressione CTRL + C.

    >[!NOTE]
    >**Não** inclua o espaço à esquerda na lista de caracteres de impressão digital.

6. Na caixa de diálogo **certificado** , clique em **OK**.

7. No menu **Iniciar** , na caixa **Pesquisar programas e arquivos** , digite **notepad**e pressione Enter.

8. No **bloco de notas**, no menu **Editar** , clique em **colar**.

9. No menu **Editar** , clique em **substituir**.

10. Na caixa **Localizar** , digite um caractere de espaço e clique em **substituir tudo**.

    >[!Note]
    >Isso remove todos os espaços entre os caracteres na impressão digital.

11. Na caixa de diálogo **substituir** , clique em **Cancelar**.

12. Selecione a *impressão digital*convertida e pressione CTRL + C.

13. Feche o **bloco de notas** sem salvar as alterações.

### <a name="configure-the-fim-cm-policy-module"></a>Configurar o módulo de política do FIM CM

1. Restaure o console da **autoridade de certificação** .

2. Clique com o botão direito do mouse em **contoso-CORPCA-CA**e clique em **Propriedades**.

3. Na caixa de diálogo **Propriedades da Contoso-CORPCA-CA** , na guia **módulo de política** , clique em **Propriedades**.

    - Na guia **geral** , certifique-se de que **passar solicitações de não fim cm para o módulo de política padrão para processamento** esteja selecionado.

    - Na guia **certificados de autenticação** , clique em **Adicionar**.

    - Na caixa de diálogo certificado, clique com o botão direito do mouse na caixa **especifique o hash de certificado codificado em Hex** e clique em **colar**.

    - Na caixa de diálogo **certificado** , clique em **OK**.

        >[!Note]
        >Se o botão **OK** não estiver habilitado, você incluiu acidentalmente um caractere oculto na cadeia de caracteres de impressão digital quando copiou a impressão digital do certificado clmAgent. Repita todas as etapas a partir da **tarefa 4: Copie a impressão digital do certificado MIMCMAgent para a área de transferência do Windows** neste exercício.

4. Na caixa de diálogo **Propriedades de configuração** , verifique se a impressão digital é exibida na lista **certificados de assinatura válidos** e clique em **OK**.

5. Na caixa de mensagem **Gerenciamento de certificados do fim** , clique em **OK**.

6. Na caixa de diálogo **Propriedades da Contoso-CORPCA-CA** , clique em **OK**.

7. Clique com o botão direito do mouse em **contoso-CORPCA-CA** *,* aponte para **todas as tarefas**e clique em **parar serviço**.

8. Aguarde até que deixa de serviços de certificados do Active Directory.

9. Clique com o botão direito do mouse em **contoso-CORPCA-CA** *,* aponte para **todas as tarefas**e clique em **Iniciar serviço**.

10. Feche o console da **autoridade de certificação** .

11. Feche todas as janelas abertas e, em seguida, faça logoff.

**A última etapa na implantação** é que queremos garantir que contoso\\MIMCM-Managers possam implantar e criar modelos e configurar o sistema sem ser administradores de esquema e domínio. O próximo script obterá as permissões para os modelos de certificado usando Dsacls. Execute com uma conta que tenha permissão total para alterar as permissões de leitura e gravação de segurança para cada modelo de certificado existente na floresta.

Primeiras etapas: **configurando as permissões de ponto de conexão de serviço e grupo de destino & delegando o gerenciamento de modelo de perfil**

1. Configure permissões no SCP (ponto de conexão de serviço).

2. Configure o gerenciamento de modelo de perfil delegado.

3. Configure permissões no SCP (ponto de conexão de serviço). **Não \<nenhum script\>**

4.   Verifique se você está conectado ao servidor virtual **CORPDC** .

5. Fazer logon como **contoso\\corpadmin**

6. Em **Ferramentas administrativas**, abra **Active Directory usuários e computadores**.

7. Em **Active Directory usuários e computadores**, no menu **Exibir** , verifique se os **recursos avançados** estão habilitados.

8. Na árvore de console, expanda **Contoso.com** \| **sistema** \| **Gerenciador de ciclo de vida de certificado** **do Microsoft** \| e clique em **CORPCM**.

9. Clique com o botão direito do mouse em **CORPCM**e clique em **Propriedades**.

10. Na caixa de diálogo **Propriedades de CORPCM** , na guia **segurança** , adicione os seguintes grupos com as permissões correspondentes:

    | Grupo          | Permissões      |
    |----------------|------------------|
    | mimcm-gerentes | Ler </br> Auditoria do FIM CM</br> Agente de registro do FIM CM</br> Registro de solicitação do FIM CM</br> Recuperação de solicitação do FIM CM</br> Renovar solicitação do FIM CM</br> Solicitação REVOKE do FIM CM </br> Solicitação de desbloqueio de cartão inteligente do FIM CM |
    | mimcm-HelpDesk | Ler</br> Agente de registro do FIM CM</br> Solicitação REVOKE do FIM CM</br> Solicitação de desbloqueio de cartão inteligente do FIM CM |

11. Na caixa de diálogo **Propriedades de CORPDC** , clique em **OK**.

12. Deixe **Active Directory usuários e computadores** abertos.

**Configurar permissões nos objetos de usuário descendentes**

1. Verifique se você ainda está no console **Active Directory usuários e computadores** .

2. Na árvore de console, clique com o botão direito do mouse em **contoso.com**e clique em **Propriedades**.

3. No separador **Segurança**, clique em **Avançadas**.

4. Na caixa de diálogo **configurações de segurança avançadas para contoso** , clique em **Adicionar**.

5. Na caixa de diálogo **Selecionar usuário, computador, conta de serviço ou grupo** , na caixa **Inserir o nome do objeto a ser selecionado** , digite **mimcm-Managers**e clique em **OK**.

6. Na caixa de diálogo **entrada de permissão para contoso** , na lista **aplicar a** , selecione **objetos de usuário descendentes** e, em seguida, habilite a caixa de seleção **permitir** para as seguintes **permissões**:

    - **Ler todas as propriedades**

    - **Permissões de leitura**

    - **Auditoria do FIM CM**

    - **Agente de registro do FIM CM**

    - **Registro de solicitação do FIM CM**

    - **Recuperação de solicitação do FIM CM**

    - **Renovar solicitação do FIM CM**

    - **Solicitação REVOKE do FIM CM**

    - **Solicitação de desbloqueio de cartão inteligente do FIM CM**

7. Na caixa de diálogo **entrada de permissão para contoso** , clique em **OK**.

8. Na caixa de diálogo **configurações de segurança avançadas para contoso** , clique em **Adicionar**.

9. Na caixa de diálogo **Selecionar usuário, computador, conta de serviço ou grupo** , na caixa **Inserir o nome do objeto a ser selecionado** , digite **mimcm-helpdesk**e clique em **OK**.

10. Na caixa de diálogo **entrada de permissão para contoso** , na lista **aplicar a** , selecione **objetos de usuário descendentes** e marque a caixa de seleção **permitir** para as seguintes **permissões**:

    - **Ler todas as propriedades**

    - **Permissões de leitura**

    - **Agente de registro do FIM CM**

    - **Solicitação REVOKE do FIM CM**

    - **Solicitação de desbloqueio de cartão inteligente do FIM CM**

11. Na caixa de diálogo **entrada de permissão para contoso** , clique em **OK**.

12. Na caixa de diálogo **configurações de segurança avançadas para contoso** , clique em **OK**.

13. Na caixa de diálogo **Propriedades de contoso.com** , clique em **OK**.

14. Deixe **Active Directory usuários e computadores** abertos.

**Configurar permissões nos objetos de usuário descendentes \<nenhum script\>**

1. Verifique se você ainda está no console **Active Directory usuários e computadores** .

2. Na árvore de console, clique com o botão direito do mouse em **contoso.com**e clique em **Propriedades**.

3. No separador **Segurança**, clique em **Avançadas**.

4. Na caixa de diálogo **configurações de segurança avançadas para contoso** , clique em **Adicionar**.

5. Na caixa de diálogo **Selecionar usuário, computador, conta de serviço ou grupo** , na caixa **Inserir o nome do objeto a ser selecionado** , digite **mimcm-Managers**e clique em **OK**.

6. Na caixa de diálogo **entrada de permissão para contoso** , na lista **aplicar a** , selecione **objetos de usuário descendentes** e, em seguida, habilite a caixa de seleção **permitir** para as seguintes **permissões**:

    - **Ler todas as propriedades**

    - **Permissões de leitura**

    - **Auditoria do FIM CM**

    - **Agente de registro do FIM CM**

    - **Registro de solicitação do FIM CM**

    - **Recuperação de solicitação do FIM CM**

    - **Renovar solicitação do FIM CM**

    - **Solicitação REVOKE do FIM CM**

    - **Solicitação de desbloqueio de cartão inteligente do FIM CM**

7. Na caixa de diálogo **entrada de permissão para contoso** , clique em **OK**.

8. Na caixa de diálogo **configurações de segurança avançadas para contoso** , clique em **Adicionar**.

9. Na caixa de diálogo **Selecionar usuário, computador, conta de serviço ou grupo** , na caixa **Inserir o nome do objeto a ser selecionado** , digite **mimcm-helpdesk**e clique em **OK**.

10. Na caixa de diálogo **entrada de permissão para contoso** , na lista **aplicar a** , selecione **objetos de usuário descendentes** e marque a caixa de seleção **permitir** para as seguintes **permissões**:

    - **Ler todas as propriedades**

    - **Permissões de leitura**

    - **Agente de registro do FIM CM**

    - **Solicitação REVOKE do FIM CM**

    - **Solicitação de desbloqueio de cartão inteligente do FIM CM**

11. Na caixa de diálogo **entrada de permissão para contoso** , clique em **OK**.

12. Na caixa de diálogo **configurações de segurança avançadas para contoso** , clique em **OK**.

13. Na caixa de diálogo **Propriedades de contoso.com** , clique em **OK**.

14. Deixe **Active Directory usuários e computadores** abertos.

Segunda etapa: **delegando permissões de gerenciamento de modelo de certificado \<script\>**

- Delegando permissões no contêiner de modelos de certificado.

- Delegando permissões no contêiner de OID.

- Delegando permissões nos modelos de certificado existentes.

Defina permissões no contêiner de modelos de certificado:

1. Restaure o console **sites e serviços do Active Directory** .

2. Na árvore de console, expanda **Serviços**, expanda **serviços de chave pública**e clique em **modelos de certificado**.

3. Na árvore de console, clique com o botão direito do mouse em **modelos de certificado**e clique em **delegar controle**.

4. No assistente **de delegação de controle** , clique em **Avançar**.

5. Na página **usuários ou grupos** , clique em **Adicionar**.

6. Na caixa de diálogo **Selecionar usuários, computadores ou grupos** , na caixa **Inserir os nomes de objeto a serem selecionados** , digite **mimcm-Managers**e clique em **OK**.

7. Na página **usuários ou grupos** , clique em **Avançar**.

8. Na página **tarefas a serem delegadas** , clique em **criar uma tarefa personalizada para delegar**e clique em **Avançar**.

9.  Na página **tipo de objeto Active Directory** , verifique se **essa pasta, os objetos existentes nesta pasta e a criação de novos objetos nesta pasta** estão selecionados e clique em **Avançar**.

10. Na página **permissões** , na lista **permissões** , marque a caixa de seleção **controle total** e clique em **Avançar**.

11. Na página **concluindo o assistente de delegação de controle** , clique em **concluir**.

Defina permissões no contêiner OID:

1. Na árvore de console, clique com o botão direito do mouse em **OID**e clique em **Propriedades**.

2. Na caixa de diálogo **Propriedades de OID** , na guia **segurança** , clique em **avançado**.

3. Na caixa de diálogo **configurações de segurança avançadas para OID** , clique em **Adicionar**.

4. Na caixa de diálogo **Selecionar usuário, computador, conta de serviço ou grupo** , na caixa **Inserir o nome do objeto a ser selecionado** , digite **mimcm-Managers**e clique em **OK**.

5. Na caixa de diálogo **entrada de permissões para OID** , verifique se as permissões se aplicam a **esse objeto e a todos os objetos descendentes**, clique em **controle total**e clique em **OK**.

6. Na caixa de diálogo **configurações de segurança avançadas para OID** , clique em **OK**.

7. Na caixa de diálogo **Propriedades de OID** , clique em **OK**.

8. Feche **Active Directory sites e serviços**.

**Scripts: permissões na OID, modelo de perfil & contêiner de modelos de certificado**

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

**Scripts: delegando permissões nos modelos de certificado existentes.**  

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
