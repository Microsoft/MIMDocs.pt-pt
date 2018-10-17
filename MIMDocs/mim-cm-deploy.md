---
title: Implementar o Gestor de certificados do Microsoft Identity Manager | Documentos da Microsoft
description: Instalar Gestor de certificados do Microsoft Identity Manager 2016
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 01c5c8357c8cb0424bd38b61836919f5c2c3e96a
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358878"
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Implementar o Gestor de certificados do Microsoft Identity Manager 2016 (MIM CM)

A instalação do Gestor de certificados do Microsoft Identity Manager 2016 (MIM CM) envolve vários passos. Como uma forma de simplificar o processo, são a divisão de coisas. Há etapas preliminares que é necessário antes de quaisquer passos reais de MIM CM. Sem o trabalho preliminar a instalação, é provável que falhe.

O diagrama abaixo mostra um exemplo do tipo de ambiente que pode ser utilizado. Os sistemas com números estão incluídos na lista abaixo o diagrama e são necessários para concluir com êxito as etapas abordadas neste artigo. Por fim, são utilizados os servidores de Datacenters do Windows 2016:

![Diagrama do ambiente](media/mim-cm-deploy/image001.png)

1. CORPDC – controlador de domínio
2. CORPCM – servidor de MIM CM
3. CORPCA – autoridade de certificação
4. CORPCMR – Web de API de Rest de MIM CM – CM Portal para a API Rest – utilizado para utilizar mais tarde
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1 – associados a um domínio do Windows 10

## <a name="deployment-overview"></a>Descrição geral da implementação

- Instalação de sistema operativo base

    O laboratório consiste em servidores do windows 2016 Datacenter.

    >[!NOTE]
    >Para obter mais detalhes sobre as plataformas suportadas para o MIM 2016 dar uma olhada no artigo intitulado [plataformas suportadas para o MIM 2016](/microsoft-identity-manager/microsoft-identity-manager-2016-supported-platforms.md)

1. Passos de pré-implementação

    - [A extensão do esquema](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)

    - A criação de contas de serviço

    - [Criar modelos de certificado](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)

    - IIS

    - Configurar o Kerberos

    - Etapas relacionadas à base de dados

        - Requisitos de configuração de SQL

        - Permissões de base de dados

2. Implementação

## <a name="pre-deployment-steps"></a>Passos de pré-implementação

O Assistente de configuração de MIM CM requer informações para ser fornecido ao longo do caminho para que ele seja concluído com êxito.

![Diagrama](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>A extensão do esquema

O processo de expandir o esquema é muito simples, mas deve ser abordado com cuidado devido à sua natureza irreversível.

>[!NOTE]
>Este passo requer que a conta utilizada tem direitos de administrador de esquema.

1. Navegue até à localização do suporte de dados de MIM e navegue para \\gestão de certificados\\x64 pasta.

2. Copie a pasta de esquema para CORPDC, em seguida, navegar até ele.

    ![Diagrama](media/mim-cm-deploy/image005.png)

3. Execute o cenário de floresta único do script resourceForestModifySchema.vbs. Para o cenário de floresta de recursos, execute os scripts:
   - DomainA – os utilizadores localizado (userForestModifySchema.vbs)
   - ResourceForestB – a localização da instalação do CM (resourceForestModifySchema.vbs).

     >[!NOTE]
     >Alterações de esquema são uma operação unidirecional e exigir uma floresta de recuperação para revertê-lo, por isso, certifique-se de que tem cópias de segurança necessárias. Para obter detalhes sobre as alterações efetuadas ao esquema por execução desta operação, consulte o artigo [alterações de esquema de gerenciamento do Forefront Identity Manager 2010 certificado](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx)

     ![Diagrama](media/mim-cm-deploy/image007.png)

4. Execute o script e deverá receber um sucesso mensagem uma vez que o script ser concluído.

    ![Mensagem de êxito](media/mim-cm-deploy/image009.png)

O esquema no AD agora é expandido para suportar o MIM CM.

### <a name="creating-service-accounts-and-groups"></a>Criação de contas de serviço e grupos

A tabela seguinte resume as contas e as permissões exigidas por MIM CM. Pode permitir que o MIM CM crie as seguintes contas automaticamente ou pode criá-las antes da instalação. Os nomes de conta real podem ser alterados. Se criar as contas por conta própria, considere as contas de utilizador no como, por exemplo, que é fácil de acordo com o nome da conta de utilizador para a sua função de nomenclatura.

Utilizadores:

![Diagrama](media/mim-cm-deploy/image010.png)

![Diagrama](media/mim-cm-deploy/image012.png)

| **Função**                   | **Registo de utilizador no nome** |
|----------------------------|---------------------|
| Agente de MIM CM               | MIMCMAgent          |
| MIM CM Key Recovery Agent  | MIMCMKRAgent        |
| Agente de autorização de MIM CM | MIMCMAuthAgent      |
| Agente de MIM CM Gestor de AC    | MIMCMManagerAgent   |
| MIM CM Web Pool Agent      | MIMCMWebAgent       |
| MIM CM Enrollment Agent    | MIMCMEnrollAgent    |
| Serviço de MIM CM Update      | MIMCMService        |
| Conta de instalação de MIM        | MIMINSTALL          |
| Agente de suporte técnico de ajuda            | CMHelpdesk1-2       |
| Gestor de CM                 | CMManager1-2        |
| Utilizador de subscritor            | CMUser1-2           |

Grupos:

| **Função**               | **Grupo**         |
|------------------------|-------------------|
| Membros da assistência técnica de CM    | suporte técnico do mimcm    |
| Membros do Gestor de CM     | Gestores de MIMCM    |
| Membros de subscritores do CM | Assinantes do MIMCM |

PowerShell: Contas de agente:

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

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>Atualização **CORPCM** Política Local do servidor para contas de agente 

| **Registo de utilizador no nome** | **Descrição e permissões**   |
|------|---------------------|
| MIMCMAgent          | Fornece os seguintes serviços: </br>-Obtém encriptada chaves privadas da AC. </br>-Protege as informações de PIN do smart card no banco de dados FIM CM. </br>-Protege a comunicação entre o FIM CM e a AC. </br></br> Esta conta de utilizador requer as seguintes definições de controlo de acesso:</br>-   **Permitir início de sessão localmente** direito de utilizador.</br>-   **Emitir e gerir certificados** direito de utilizador. </br>-Leitura e escrita permissão na pasta Temp do sistema, na seguinte localização: % WINDIR %\\Temp.</br>-Um certificado de assinatura e encriptação digital emitido e instalado no arquivo de utilizador.
|MIMCMKRAgent        | Recuperar arquivados chaves privadas da AC. Esta conta de utilizador requer as seguintes definições de controlo de acesso:</br> -   **Permitir início de sessão localmente** direito de utilizador.</br>-Associação a **administradores** grupo. </br>-Permissão de inscrição sobre os **KeyRecoveryAgent** modelo de certificado. </br>-Certificado do Key Recovery Agent é emitido e instalado no arquivo de utilizador. O certificado tem de ser adicionado à lista de agentes de recuperação de chaves na AC. </br>-Permissão de leitura e escrita permissão na pasta Temp do sistema, na seguinte localização: ```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | Determina as permissões para utilizadores e grupos e direitos de utilizador. Esta conta de utilizador requer as seguintes definições de controlo de acesso: </br>-Associação ao grupo de domínio de acesso de versões anteriores ao Windows 2000. </br> -Concedido a **gerar auditorias de segurança** direito de utilizador.             |
| MIMCMManagerAgent   | Executa as atividades de gestão de AC. </br> Este utilizador tem de ser atribuído a permissão Gerir AC.        |
| MIMCMWebAgent       | Fornece a identidade para o pool de aplicativos do IIS. FIM CM é executado dentro de um Microsoft Win32® programação interface processo de aplicação que utiliza credenciais deste utilizador. </br> Esta conta de utilizador requer as seguintes definições de controlo de acesso:</br> -Associação a **IIS_WPG, windows 2016 = IIS_IUSRS** grupo. </br>-Associação a **administradores** grupo.</br>-Concedido a **gerar auditorias de segurança** direito de utilizador. </br>-Concedido a **atuar como parte do sistema operativo** direito de utilizador. </br>-Concedido a **token de nível de processo de substituição** direito de utilizador.</br>-Atribuída como a identidade do conjunto aplicacional do IIS, **CLMAppPool**. </br>-Concedido permissão de leitura a **HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\CLM\\v1.0\\servidor\\WebUser** chave de registo. </br>-Esta conta também tem de ser fidedigna para delegação.|
| MIMCMEnrollAgent    | Efetua a inscrição em nome de um utilizador. Esta conta de utilizador requer as seguintes definições de controlo de acesso:</br>-Um certificado de Enrollment Agent que é emitido e instalado no arquivo de utilizador.</br>-   **Permitir início de sessão localmente** direito de utilizador. </br>-Permissão de inscrição sobre os **Enrollment Agent** modelo de certificado (ou o modelo personalizado, se for utilizada).                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Criação de modelos de certificado para contas de serviço de MIM CM

Três das contas de serviço utilizadas por MIM CM requerem um certificado e o Assistente de configuração requer que forneça o nome dos modelos de certificados que deve ser usado para solicitar certificados para os mesmos.

As contas de serviço que necessitam de certificados são:

- MIMCMAgent: Esta conta precisa de um certificado de utilizador

- MIMCMEnrollAgent: Esta conta tem um certificado de agente de inscrição

- MIMCMKRAgent: Esta conta tem uma **agente de recuperação de chaves** certificado

Existem modelos já presentes no AD, mas temos de criar nossas próprios versões para trabalhar com o MIM CM. À medida que precisamos fazer modificação entre os modelos de linha de base original.

Todas as três as contas acima irão ter direitos na sua organização elevados e devem ser tratadas com cuidado.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>Criar o modelo de certificado de assinatura de MIM CM

1. Partir **ferramentas administrativas**, abra **autoridade de certificação**.

2. Na **autoridade de certificação** consola, na árvore da consola, expanda **Contoso-CorpCA**e, em seguida, clique em **modelos de certificado**.

3. Com o botão direito **modelos de certificado**e, em seguida, clique em **gerir**.

4. Na **consola de modelos de certificado**, na **detalhes** painel, selecione e o botão direito do mouse **utilizador**e, em seguida, clique em **Duplicar modelo** .

5. Na **Duplicar modelo** caixa de diálogo, selecione **do Windows Server 2003 Enterprise**e, em seguida, clique em **OK**.

    ![Mostrar as alterações resultantes](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    >O MIM CM não funciona com certificados com base nos modelos de certificados de 3 de versão. Tem de criar um modelo de certificado de empresa do Windows Server® 2003 (versão 2). Ver [V3 detalhes](https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade) para obter mais informações.

6. Na **propriedades de novo modelo** caixa de diálogo a **gerais** separador o **nome a apresentar do modelo** , escreva **assinatura de MIM CM**. Alteração da **período de validade** para **2 anos**e, em seguida, desmarque a **Publicar certificado no Active Directory** caixa de verificação.

7. Sobre o **processamento de pedidos** separador, certifique-se de que o **permitir que a chave privada seja exportada** caixa de verificação está selecionada e, em seguida, clique em **separador criptografia**.

8. Na **seleção de criptografia** caixa de diálogo, desativar **v1.0 do fornecedor de criptografia avançada de Microsoft**, ativar **Microsoft RSA avançada e o fornecedor de criptografia AES**e, em seguida, clique em **OK**.

9. Na **nome do requerente** separador, desmarque a **incluir o nome de correio eletrónico no nome do requerente** e **nome de correio eletrónico** caixas de verificação.

10. Sobre o **extensões** separador a **extensões incluídas neste modelo** lista, certifique-se de que **políticas de aplicações** está selecionada e, em seguida, clique em **editar** .

11. Na **Editar extensão de políticas de aplicação** caixa de diálogo, selecione a **Encrypting File System** e o **proteger o E-Mail** políticas de aplicações. Clique em **Remova**e, em seguida, clique em **OK**.

12. Sobre o **segurança** separador execute os seguintes passos:

    - Remova **administrador**.

    - Remova **Admins do domínio**.

    - Remova **utilizadores de domínio**.

    - Atribuir apenas **leitura** e **escrever** permissões para **Admins de empresa**.

    - Adicionar **MIMCMAgent.**

    - Atribua **leitura** e **inscrever** permissões para **MIMCMAgent**.

13. Na **propriedades de novo modelo** caixa de diálogo, clique em **OK**.

14. Deixe o **consola de modelos de certificado** abrir.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>Criar o modelo de certificado de MIM CM Enrollment Agent

1. Na **consola de modelos de certificado**, na **detalhes** painel, selecione e o botão direito do mouse **Enrollment Agent**e, em seguida, clique em **Duplicar modelo**.

2. Na **Duplicar modelo** caixa de diálogo, selecione **do Windows Server 2003 Enterprise**e, em seguida, clique em **OK**.

3. Na **propriedades de novo modelo** caixa de diálogo a **gerais** separador o **nome a apresentar do modelo** , escreva **MIM CM Enrollment Agent**. Certifique-se de que o **período de validade** é **2 anos**.

4. Sobre o **processamento de pedidos** separador, ative **permitir que a chave privada seja exportada**e, em seguida, clique em **CSPs ou separador criptografia.**

5. Na **seleção de CSP** caixa de diálogo, desativar **v1.0 do fornecedor de criptografia de Base do Microsoft**, desativar **v1.0 do fornecedor de criptografia avançada de Microsoft**, ativar  **Microsoft RSA avançada e o fornecedor de criptografia AES**e, em seguida, clique em **OK**.

6. Sobre o **segurança** separador efetue o seguinte:

    - Remova **administrador**.

    - Remova **Admins do domínio**.

    - Atribuir apenas **leitura** e **escrever** permissões para **Admins de empresa**.

    - Adicione **MIMCMEnrollAgent**.

    - Atribua **leitura** e **inscrever** permissões para **MIMCMEnrollAgent**.

7. Na **propriedades de novo modelo** caixa de diálogo, clique em **OK**.

8. Deixe o **consola de modelos de certificado** abrir.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>Criar o modelo de certificado de MIM CM Key Recovery Agent

1. Na **modelos de certificado** consola, na **detalhes** painel, selecione e o botão direito do mouse **Key Recovery Agent**e, em seguida, clique em **Duplicar modelo**.

2. Na **Duplicar modelo** caixa de diálogo, selecione **do Windows Server 2003 Enterprise**e, em seguida, clique em **OK**.

3. Na **propriedades de novo modelo** caixa de diálogo a **gerais** separador o **nome a apresentar do modelo** , escreva **MIM CM Key Recovery Agent**. Certifique-se de que o **período de validade** é **2 anos** sobre o **separador criptografia.**

4. Na **seleção de fornecedores** caixa de diálogo, desativar **v1.0 do fornecedor de criptografia avançada de Microsoft**, ativar **Microsoft RSA avançada e o fornecedor de criptografia AES**, e, em seguida, clique em **OK**.

5. Sobre o **requisitos de emissão** separador, certifique-se de que **aprovação do Gestor de certificado de AC** é **desativada**.

6. Sobre o **segurança** separador efetue o seguinte:

    - Remova **administrador**.

    - Remova **Admins do domínio**.

    - Atribuir apenas **leitura** e **escrever** permissões para **Admins de empresa**.

    - Adicione **MIMCMKRAgent**.

    - Atribua **leitura** e **inscrever** permissões para **KRAgent**.

7. Na **propriedades de novo modelo** caixa de diálogo, clique em **OK**.

8. Feche a **Consola de Modelos de Certificado**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>Publique os modelos de certificado necessário na autoridade de certificação

1. Restaurar o **autoridade de certificação** consola.

2. Na **autoridade de certificação** consola, na árvore da consola, clique com botão direito **modelos de certificado**, aponte para **New**e, em seguida, clique em **certificado Modelo a emitir**.

3. Na **ativar modelos de certificado** caixa de diálogo, selecione **MIM CM Enrollment Agent**, **MIM CM Key Recovery Agent**, e **MIM CM assinatura**. Clique em **OK**.

4. Na árvore da consola, clique em **modelos de certificado**.

5. Certifique-se de que os novos modelos de três aparecem no **detalhes** painel e, em seguida, feche **autoridade de certificação**.

    ![Assinatura de MIM CM](media/mim-cm-deploy/image016.png)

6. Feche todas as janelas abertas e terminar sessão.

### <a name="iis-configuration"></a>Configuração do IIS

Para hospedar o Web site do CM, instalar e configurar o IIS.

#### <a name="install-and-configure-iis"></a>Instalar e configurar o IIS

1. Inicie sessão no **CORLog no** como **MIMINSTALL** conta

    >[!IMPORTANT]
    >A conta de instalação de MIM deve ser um administrador local

2. Abra o PowerShell e execute o seguinte comando

    `Install-WindowsFeature –ConfigurationFilePath`

>[!NOTE]
>Um Web Site predefinido com o nome de site é instalado por predefinição com o IIS 7. Se esse site foi renomeado ou removido um site com o nome do Web Site predefinido tem de estar disponível antes de poder instalar o MIM CM.

#### <a name="configuring-kerberos"></a>Configurar o Kerberos

A conta de MIMCMWebAgent executará o portal de MIM CM. Por padrão no IIS e segurança de kernel autenticação em modo é utilizada no IIS por predefinição. Irá desativar a autenticação de modo de kernel de Kerberos e configurar o SPN na conta MIMCMWebAgent em vez disso. Alguns comandos exigirá permissão elevada no active directory e o servidor CORPCM.

![Diagrama](media/mim-cm-deploy/image020.png)

```powershell
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}
```

**A atualizar o IIS no CORPCM**

![Diagrama](media/mim-cm-deploy/image022.png)

```powershell
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true
```

>[!NOTE]
>Terá de adicionar um registo DNS para "cm.contoso.com" e aponte para CORPCM IP

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>A exigência de SSL no portal de MIM CM

É altamente recomendável que exigem SSL no portal de MIM CM. Caso contrário, até mesmo o assistente irá avisá-lo sobre ele.

1. Inscreva-se no certificado de web **cm.contoso.com** atribuir ao site predefinido

2. Open **Gerenciador do IIS** e navegue para **gestão de certificados**

3. Na vista de funcionalidades, faça duplo clique em definições de SSL.

4. Na página de definições de SSL, selecione **exigir SSL**.

5. No painel de ações, clique em **aplicar.**

### <a name="database-configuration-corpsql-for-mim-cm"></a>Configuração de base de dados **CORPSQL** de MIM CM

1. Certifique-se de que está ligado ao servidor CORPSQL01.

2. Certifique-se de que estiver conectado como DBA do SQL.

3. Execute o seguinte script de T-SQL para permitir que a CONTOSO\\MIMINSTALL conta para criar a base de dados quando vamos para o passo de configuração

    >[!NOTE]
    >Será necessário voltar a SQL quando estamos prontos para o módulo de saída e a política

    ```sql
    create login [CONTOSO\\MIMINSTALL] from windows;
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
    ```

![Mensagem de erro de Assistente de configuração de MIM CM](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Implementação da gestão de certificados do Microsoft Identity Manager 2016

1. Certifique-se de que está ligado ao servidor CORPCM e que o **MIMINSTALL** conta é membro da **administradores locais** grupo.

2. Certifique-se de que estiver conectado como Contoso\\MIMINSTALL.

3. Monte o ISO do Microsoft Identity Manager SP1.

4. **Open** a **gestão de certificados\\x64** diretório.

5. Na **x64** janela, clique com botão direito **configuração**e, em seguida, clique em **executar como administrador**.

6. No Bem-vindo à página do Assistente de configuração do gerenciamento de certificados do Microsoft Identity Manager, clique em **seguinte.**

7. Na página do contrato de licença de utilizador final, leia o contrato, ative o aceito os termos no contrato de licença **caixa de verificação**e, em seguida, clique em seguinte.

8. Na página de configuração personalizada, certifique-se de que o **Portal de MIM CM** e **componentes do serviço de atualização de MIM CM** estiver definida para ser instalada e, em seguida **clique em seguinte**.

9. Na página de pasta Virtual da Web, certifique-se de que é o nome da pasta Virtual **CertificateManagement**e, em seguida **clique em seguinte**.

10. Na página de instalar o Microsoft Identity Manager Certificate Management, **clique em instalar**.

11. Na **concluído** a página de Assistente de configuração de gestão do Microsoft Identity Manager certificado **clique em concluir**.

![Assistente de MIM CM foi concluído](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Assistente de configuração da gestão de certificados do Microsoft Identity Manager 2016

Antes de iniciar sessão no CORPCM adicionar MIMINSTALL para **domínio administradores, administradores de esquema e os administradores locais** grupo para o Assistente de configuração. Isto pode ser removido mais tarde após a conclusão da configuração.

![Mensagem de erro](media/mim-cm-deploy/image028.png)

1. Do **começar** menu, clique em **Assistente de configuração de gestão de certificados**. E executar como **administrador**

2. Sobre o **bem-vindo ao Assistente de configuração** página, clique em **próxima**.

3. Sobre o **configuração da AC** página, certifique-se de que a AC selecionada é **Contoso-CORPCA-AC**, certifique-se de que o servidor selecionado é **CORPCA. CONTOSO.COM**e, em seguida, clique em **próxima**.

4. No **configurar o Microsoft® SQL Server® Database** na página a **nome do SQL Server** , escreva **CORPSQL1** , ativar o **utilizar as minhas credenciais para criar o base de dados** caixa de verificação e, em seguida, clique em **próxima**.

5. Na **definições de base de dados** página, aceite o nome de base de dados predefinido de **FIMCertificateManagement**, certifique-se de que **autenticação SQL integrada** estiver selecionada e, em seguida, Clique em **seguinte**.

6. Sobre o **configurar o Active Directory** página, aceite o nome predefinido fornecido para o ponto de ligação de serviço e, em seguida, clique em **próxima**.

7. Sobre o **método de autenticação** página Confirmar **autenticação integrada do windows** está selecionado, em seguida, clique em **seguinte**.

8. Sobre o **agentes-FIM CM** página, desmarque a **utilizar as predefinições FIM CM** caixa de verificação e, em seguida, clique em **contas personalizadas**.

9. Na **agentes-FIM CM** caixa de diálogo com guias de transmissões, em cada separador, escreva as informações seguintes:

   - Nome de utilizador: **Update**

   - Palavra-passe: **passar\@word1**

   - Confirmar palavra-passe: **passar\@word1**

   - Usar um utilizador existente: **ativado**

     >[!NOTE]
     >Estas contas que criámos anteriormente. Certifique-se de que os procedimentos no passo 8 são repetidos para todos os separadores de conta de agente seis.

     ![Contas de MIM CM](media/mim-cm-deploy/image030.png)

10. Quando todas as informações de conta do agente estiverem concluídas, clique em **OK**.

11. Sobre o **agentes-MIM CM** página, clique em **próxima**.

12. Sobre o **configurar certificados de servidor** página, ative os seguintes modelos de certificado:

    - Modelo de certificado a utilizar para o certificado de Key Recovery Agent do agente de recuperação: **MIMCMKeyRecoveryAgent**.

    - Modelo de certificado a utilizar para o certificado do FIM CM Agent: **MIMCMSigning**.

    - Modelo de certificado a ser utilizado para o certificado de agente de inscrição: **FIMCMEnrollmentAgent**.

13. Sobre o **certificados de servidor de configuração** página, clique em **próxima**.

14. No **configuração do servidor de email, a impressão de documentos** página, além do **especifique o nome do servidor SMTP que pretende utilizar para notificações de registo de email** caixa e, em seguida, clique em **seguinte.**

15. Sobre o **pronto para configurar** página, clique em **configurar**.

16. Na **Assistente de configuração – Microsoft Forefront Identity Manager 2010 R2** aviso de caixa de diálogo, clique em **OK** para confirmar que o SSL não está ativado no diretório virtual do IIS.

    ![suporte de dados/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    >Não clique no botão Finish até que a execução do Assistente de configuração está concluída. O registo para o assistente pode ser encontrado aqui: **% programfiles %\\Microsoft Forefront Identity Management\\2010\\gestão de certificados\\config.log**

17. Clique em **Concluir**.

    ![Assistente de MIM CM foi concluído](media/mim-cm-deploy/image033.png)

18. Feche todas as janelas abertas.

19. Adicionar https://cm.contoso.com/certificatemanagement à zona de local intranet no seu browser.

20. Visite o site do servidor CORPCM https://cm.contoso.com/certificatemanagement  

    ![Diagrama](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>Verificar se o serviço de isolamento da chave CNG

1. Partir **ferramentas administrativas**, abra **serviços**.

2. Na **detalhes** painel, faça duplo clique em **isolamento da chave CNG**.

3. Na **gerais** separador, altere a **tipo de arranque** para **automática**.

4. Sobre o **gerais** separador, inicie o serviço se não estiver num estado iniciado.

5. Sobre o **gerais** separador, clique em **OK**.

### <a name="installing-and-configuring-the-ca-modules-"></a>Instalar e configurar os módulos da AC:

Neste passo, iremos instalar e configurar os módulos de AC do FIM CM na autoridade de certificação.

1. Configurar o FIM CM em apenas inspecionar as permissões de utilizador para operações de gestão

2. Na **c:\\arquivos de programas\\Microsoft Forefront Identity Manager\\2010\\gestão de certificados\\web** janela, crie uma cópia do  **Web. config** a cópia de nomenclatura **web.1.config**.

3. Na **Web** janela, clique com botão direito **Web. config**e, em seguida, clique em **aberto**.

    >[!Note]
    >O ficheiro Web. config é aberto no bloco de notas

4. Quando o ficheiro abrir, prima CTRL + F.

5. Na **localizar e substituir** caixa de diálogo a **localizar** , escreva **UseUser**e, em seguida, clique em **Localizar seguinte** três vezes.

6. Fechar o **localizar e substituir** caixa de diálogo.

7. Deve estar na linha  **\<adicionar key="Clm.RequestSecurity.Flags" valor = "UseUser, UseGroups" /\>**. Altere a linha para ler  **\<adicionar key="Clm.RequestSecurity.Flags" valor = "UseUser" /\>**.

8. Feche o ficheiro, a guardar todas as alterações.

9. Criar uma conta de computador da AC no servidor SQL \<nenhum script\>

10. Certifique-se de que está ligado para o **CORPSQL01** server.

11. Certifique-se de que estiver conectado como **DBA**

12. Partir do **começar** menu, inicie **SQL Server Management Studio**.

13. Na **ligar ao servidor** caixa de diálogo a **nome do servidor de** , escreva **CORPSQL01,** e, em seguida, clique em **Connect**.

14. Na árvore da consola, expanda **Security**e, em seguida, clique em **inícios de sessão**.

15. Com o botão direito **inícios de sessão**e, em seguida, clique em **novo início de sessão**.

16. No **gerais** página, além do **nome de início de sessão** , escreva **contoso\\CORPCA\$**. Selecione **autenticação do Windows**. É a base de dados predefinida **FIMCertificateManagement**.

17. No painel esquerdo, selecione **mapeamento de utilizadores**. No painel da direita, clique na caixa de verificação no **mapa** coluna ao lado **FIMCertificateManagement**. No **associação de função para a base de dados: FIMCertificateManagement** lista, ative os **clmApp** função.

18. Clique em **OK**.

19. Fechar **Microsoft SQL Server Management Studio**.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>Instalar os módulos de AC do FIM CM na autoridade de certificação

1. Certifique-se de que está ligado para o **CORPCA** server.

2. Na **X64** windows, clique com botão direito **Setup.exe**e, em seguida, clique em **executar como administrador**.

3. Sobre o **bem-vindo ao Assistente de configuração de gestão de certificado Microsoft Identity Manager** página, clique em **próxima**.

4. Sobre o **contrato de licença de utilizador final** página, leia o contrato. Selecione o **aceito os termos no contrato de licença** caixa de verificação e, em seguida, clique em **próxima**.

5. Sobre o **configuração personalizada** página, selecione **Portal de MIM CM**e, em seguida, clique em **esta funcionalidade não estará disponível**.

6. Sobre o **configuração personalizada** página, selecione **serviço de atualização de MIM CM**e, em seguida, clique em **esta funcionalidade não estará disponível**.

    >[!Note]
    >Isto deixará os arquivos de AC de MIM CM como o único recurso ativado para a instalação.

7. Sobre o **configuração personalizada** página, clique em **próxima**.

8. Sobre o **instalar o Microsoft Identity Manager Certificate Management** página, clique em **instalar**.

9. Sobre o **concluir o Assistente de configuração de gestão de certificados do Microsoft Identity Manager** página, clique em **concluir**.

10. Feche todas as janelas abertas.

### <a name="configure-the-mim-cm-exit-module"></a>Configurar o módulo de saída de CM de MIM

1. Partir **ferramentas administrativas**, abra **autoridade de certificação**.

2. Na árvore da consola, clique com botão direito **contoso-CORPCA-AC**e, em seguida, clique em **propriedades**.

3. Sobre o **módulo de saída** separador, selecione **módulo de saída do FIM CM**e, em seguida, clique em **propriedades**.

4. Na **especifique a cadeia de ligação de base de dados do CM** , escreva **tempo limite da ligação = 15; Manter informações de segurança = True; Segurança integrada = sspi; Initial Catalog = FIMCertificateManagement; origem de dados = CORPSQL01**. Deixe o **encriptar a cadeia de ligação** ativada de caixa de verificação e, em seguida, clique em **OK**.
5. Na **Microsoft FIM Certificate Management** caixa de mensagens, clique em **OK**.

6. Na **propriedades de contoso-CORPCA-AC** caixa de diálogo, clique em **OK**.

7. Com o botão direito **contoso-CORPCA-AC** *,* aponte para **todas as tarefas**e, em seguida, clique em **parar serviço**. Aguarde até que deixa de serviços de certificados do Active Directory.

8. Com o botão direito **contoso-CORPCA-AC** *,* aponte para **todas as tarefas**e, em seguida, clique em **iniciar serviço**.

9. Minimizar a **autoridade de certificação** consola.

10. Partir **ferramentas administrativas**, abra **Visualizador de eventos**.

11. Na árvore da consola, expanda **registos de serviços e aplicativos**e, em seguida, clique em **FIM Certificate Management**.

12. Na lista de eventos, certifique-se de que os eventos mais recentes façam *não* incluir qualquer **aviso** ou **erro** eventos desde o último reinício dos serviços de certificados.

    >[!NOTE] 
    >O último evento deve indicar que o módulo de saída carregado usando as configurações de: `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`

13. Minimizar a **Visualizador de eventos**.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>Copie o thumbprint do certificado MIMCMAgent Windows® área de transferência

1. Restaurar o **autoridade de certificação** consola.

2. Na árvore da consola, expanda **contoso-CORPCA-AC**e, em seguida, clique em **certificados emitidos pela**.

3. Na **detalhes** painel, faça duplo clique no certificado com **CONTOSO\\MIMCMAgent** no **nome do autor do pedido** coluna e com **FIM CM Assinatura** no **modelo de certificado** coluna.

4. Sobre o **detalhes** separador, selecione a **Thumbprint** campo.

5. Selecione o thumbprint e, em seguida, prima CTRL + C.

    >[!NOTE]
    >Fazer **não** incluem o espaço à esquerda na lista de carateres de thumbprint.

6. Na **certificado** caixa de diálogo, clique em **OK**.

7. Do **começar** menu, na **procurar programas e ficheiros** , escreva **bloco de notas**, e, em seguida, prima ENTER.

8. Na **bloco de notas**, da **editar** menu, clique em **colar**.

9. Partir do **editar** menu, clique em **substituir**.

10. Na **localizar** caixa, escreva um caractere de espaço e, em seguida, clique em **Replace All**.

    >[!Note]
    >Esta ação remove todos os espaços entre os carateres da impressão digital.

11. Na **substitua** caixa de diálogo, clique em **Cancelar**.

12. Selecione o convertido *thumbprintstring*, e, em seguida, prima CTRL + C.

13. Fechar **bloco de notas** sem a guardar as alterações.

### <a name="configure-the-fim-cm-policy-module"></a>Configurar o módulo de política de CM do FIM

1. Restaurar o **autoridade de certificação** consola.

2. Com o botão direito **contoso-CORPCA-AC**e, em seguida, clique em **propriedades**.

3. Na **propriedades de contoso-CORPCA-AC** caixa de diálogo a **módulo de política** separador, clique em **propriedades**.

    - Sobre o **gerais** separador, certifique-se de que **passar pedidos não - FIM CM ao módulo de política predefinido para processamento** está selecionada.

    - Sobre o **certificados de assinatura** separador, clique em **Add**.

    - Na caixa de diálogo de certificado, clique com botão direito a **especifique o hash do certificado codificado em hex** caixa e, em seguida, clique em **colar**.

    - Na **certificado** caixa de diálogo, clique em **OK**.

        >[!Note]
        >Se o **OK** botão não estiver ativado, que incluiu acidentalmente um caractere oculto na cadeia de thumbprint quando copiou o thumbprint do certificado clmAgent. Repita os passos de todos os a partir **Tarefa 4: Copie o Thumbprint do certificado MIMCMAgent para área de transferência do Windows** neste exercício.

4. Na **propriedades de configuração** caixa de diálogo caixa, certifique-se de que o thumbprint aparece no **certificados de assinatura válidos** lista e, em seguida, clique em **OK**.

5. Na **FIM Certificate Management** caixa de mensagens, clique em **OK**.

6. Na **propriedades de contoso-CORPCA-AC** caixa de diálogo, clique em **OK**.

7. Com o botão direito **contoso-CORPCA-AC** *,* aponte para **todas as tarefas**e, em seguida, clique em **parar serviço**.

8. Aguarde até que deixa de serviços de certificados do Active Directory.

9. Com o botão direito **contoso-CORPCA-AC** *,* aponte para **todas as tarefas**e, em seguida, clique em **iniciar serviço**.

10. Fechar o **autoridade de certificação** consola.

11. Feche todas as janelas abertas e, em seguida, terminar a sessão.

**Último passo da implementação** é o que queremos tornar-se de que CONTOSO\\gestores de MIMCM podem implementar e criar modelos e configurar o sistema sem ser de esquema e Admins do domínio. O próximo script será ACL as permissões para os modelos de certificado utilizando o dsacls. Execute com a conta que tenha permissão total para alterar a segurança de leitura e escrita permissões a cada modelo de certificado existente na floresta.

Primeiras etapas: **configurar permissões de grupo de destino e de ponto de ligação de serviço e delegar a gestão de modelos de perfil**

1. Configure as permissões no ponto de ligação de serviço (SCP).

2. Configure a gestão de modelo de perfil de delegado.

3. Configure as permissões no ponto de ligação de serviço (SCP). **\<nenhum script\>**

4.   Certifique-se de que está ligado para o **CORPDC** do virtual server.

5. Inicie sessão como **contoso\\corpadmin**

6. Partir **ferramentas administrativas**, abra **Active Directory Users and Computers**.

7. Na **Active Directory Users and Computers**, na **vista** menu, certifique-se de que **funcionalidades avançadas** está ativada.

8. Na árvore da consola, expanda **Contoso.com** \| **sistema** \| **Microsoft** \| **ciclo de vida do certificado Gestor**e, em seguida, clique em **CORPCM**.

9. Com o botão direito **CORPCM**e, em seguida, clique em **propriedades**.

10. Na **propriedades de CORPCM** caixa de diálogo a **segurança** separador, adicione os seguintes grupos com as permissões correspondentes:

    | Grupo          | Permissões      |
    |----------------|------------------|
    | Gestores de mimcm | Ler </br> Auditoria do FIM CM</br> FIM CM Enrollment Agent</br> Inscrever o pedido do FIM CM</br> Recuperar do FIM CM pedido</br> Renovar a pedido do FIM CM</br> Revogação do FIM CM pedido </br> Pedido do FIM CM desbloquear o Smart Card |
    | suporte técnico do mimcm | Ler</br> FIM CM Enrollment Agent</br> Revogação do FIM CM pedido</br> Pedido do FIM CM desbloquear o Smart Card |

11. Na **propriedades do CORPDC** caixa de diálogo, clique em **OK**.

12. Deixe **Active Directory Users and Computers** abrir.

**Configurar as permissões nos objetos de utilizador de subordinados**

1. Certifique-se de que está ainda na **Active Directory Users and Computers** consola.

2. Na árvore da consola, clique com botão direito **Contoso.com**e, em seguida, clique em **propriedades**.

3. Sobre o **Security** separador, clique em **avançadas**.

4. Na **definições de segurança avançadas para a Contoso** caixa de diálogo, clique em **Add**.

5. Na **selecionar utilizador, computador, conta de serviço ou grupo** caixa de diálogo a **introduza o nome de objeto a selecionar** , escreva **gestores de mimcm**e, em seguida, clique em **OK**.

6. Na **entrada de permissão para Contoso** caixa de diálogo a **aplicam-se a** lista, selecione **objetos de utilizador de descendente** e, em seguida, ative o **permitir**caixa de verificação para o seguinte **permissões**:

    - **Ler todas as propriedades**

    - **Permissões de leitura**

    - **Auditoria do FIM CM**

    - **FIM CM Enrollment Agent**

    - **Inscrever o pedido do FIM CM**

    - **Recuperar do FIM CM pedido**

    - **Renovar a pedido do FIM CM**

    - **Revogação do FIM CM pedido**

    - **Pedido do FIM CM desbloquear o Smart Card**

7. Na **entrada de permissão para Contoso** caixa de diálogo, clique em **OK**.

8. Na **definições de segurança avançadas para a Contoso** caixa de diálogo, clique em **Add**.

9. Na **selecionar utilizador, computador, conta de serviço ou grupo** caixa de diálogo a **introduza o nome de objeto a selecionar** , escreva **mimcm-suporte técnico**e, em seguida, clique em **OK**.

10. Na **entrada de permissão para Contoso** caixa de diálogo a **aplicam-se ao** lista, selecione **objetos de utilizador de descendente** e, em seguida, selecione o **permitir**caixa de verificação para o seguinte **permissões**:

    - **Ler todas as propriedades**

    - **Permissões de leitura**

    - **FIM CM Enrollment Agent**

    - **Revogação do FIM CM pedido**

    - **Pedido do FIM CM desbloquear o Smart Card**

11. Na **entrada de permissão para Contoso** caixa de diálogo, clique em **OK**.

12. Na **definições de segurança avançadas para a Contoso** caixa de diálogo, clique em **OK**.

13. Na **contoso.com propriedades** caixa de diálogo, clique em **OK**.

14. Deixe **Active Directory Users and Computers** abrir.

**Configurar as permissões nos objetos de utilizador descendentes \<nenhum script\>**

1. Certifique-se de que está ainda na **Active Directory Users and Computers** consola.

2. Na árvore da consola, clique com botão direito **Contoso.com**e, em seguida, clique em **propriedades**.

3. Sobre o **Security** separador, clique em **avançadas**.

4. Na **definições de segurança avançadas para a Contoso** caixa de diálogo, clique em **Add**.

5. Na **selecionar utilizador, computador, conta de serviço ou grupo** caixa de diálogo a **introduza o nome de objeto a selecionar** , escreva **gestores de mimcm**e, em seguida, clique em **OK**.

6. Na **entrada de permissão para CONTOSO** caixa de diálogo a **aplicam-se a** lista, selecione **objetos de utilizador de descendente** e, em seguida, ative o **permitir**caixa de verificação para o seguinte **permissões**:

    - **Ler todas as propriedades**

    - **Permissões de leitura**

    - **Auditoria do FIM CM**

    - **FIM CM Enrollment Agent**

    - **Inscrever o pedido do FIM CM**

    - **Recuperar do FIM CM pedido**

    - **Renovar a pedido do FIM CM**

    - **Revogação do FIM CM pedido**

    - **Pedido do FIM CM desbloquear o Smart Card**

7. Na **entrada de permissão para CONTOSO** caixa de diálogo, clique em **OK**.

8. Na **definições de segurança avançadas para a CONTOSO** caixa de diálogo, clique em **Add**.

9. Na **selecionar utilizador, computador, conta de serviço ou grupo** caixa de diálogo a **introduza o nome de objeto a selecionar** , escreva **mimcm-suporte técnico**e, em seguida, clique em **OK**.

10. Na **entrada de permissão para CONTOSO** caixa de diálogo a **aplicam-se ao** lista, selecione **objetos de utilizador de descendente** e, em seguida, selecione o **permitir**caixa de verificação para o seguinte **permissões**:

    - **Ler todas as propriedades**

    - **Permissões de leitura**

    - **FIM CM Enrollment Agent**

    - **Revogação do FIM CM pedido**

    - **Pedido do FIM CM desbloquear o Smart Card**

11. Na **entrada de permissão para contoso** caixa de diálogo, clique em **OK**.

12. Na **definições de segurança avançadas para a Contoso** caixa de diálogo, clique em **OK**.

13. Na **contoso.com propriedades** caixa de diálogo, clique em **OK**.

14. Deixe **Active Directory Users and Computers** abrir.

Passos segundo: **permissões de gestão de modelo de certificado Delegating \<script\>**

- Delegar permissões no contentor de modelos de certificado.

- Delegar permissões no contentor OID.

- Delegar permissões nos modelos de certificados existente.

Defina as permissões no contentor de modelos de certificado:

1. Restaurar o **serviços e locais do Active Directory** consola.

2. Na árvore da consola, expanda **serviços**, expanda **Public Key Services**e, em seguida, clique em **modelos de certificado**.

3. Na árvore da consola, clique com botão direito **modelos de certificado**e, em seguida, clique em **delegar controlo**.

4. Na **delegação do controle** assistente, clique em **próxima**.

5. Sobre o **utilizadores ou grupos** página, clique em **Add**.

6. Na **selecionar utilizadores, computadores ou grupos** caixa de diálogo a **introduza os nomes de objeto a selecionar** , escreva **gestores de mimcm**e, em seguida, clique em **OK**.

7. Sobre o **utilizadores ou grupos** página, clique em **próxima**.

8. Sobre o **tarefas a delegar** página, clique em **criar uma tarefa personalizada para delegar**e, em seguida, clique em **seguinte**.

9.  Sobre o **tipo de objeto do Active Directory** página, certifique-se de que **nesta pasta, objetos existentes nesta pasta, e a criação de novos objetos nesta pasta** está selecionada e, em seguida, clique em **próxima**.

10. No **permissões** página, além do **permissões** lista, selecione o **controlo total** caixa de verificação e, em seguida, clique em **seguinte**.

11. Sobre o **concluir o Assistente de delegação do controle** página, clique em **concluir**.

Defina as permissões no contentor OID:

1. Na árvore da consola, clique com botão direito **OID**e, em seguida, clique em **propriedades**.

2. Na **propriedades de OID** caixa de diálogo a **Security** separador, clique em **avançadas**.

3. Na **definições de segurança avançadas para OID** caixa de diálogo, clique em **Add**.

4. Na **selecionar utilizador, computador, conta de serviço ou grupo** caixa de diálogo a **introduza o nome de objeto a selecionar** , escreva **gestores de mimcm**e, em seguida, clique em **OK**.

5. Na **entrada de permissões para OID** caixa de diálogo caixa, certifique-se de que as permissões se aplicam aos **este objeto e todos os objetos subordinados**, clique em **controlo total**e, em seguida, clique em  **OK**.

6. Na **definições de segurança avançadas para OID** caixa de diálogo, clique em **OK**.

7. Na **propriedades de OID** caixa de diálogo, clique em **OK**.

8. Fechar **serviços e locais do Active Directory**.

**Scripts: Permissões no contentor OID, modelo de perfil e modelos de certificado**

![Diagrama](media/mim-cm-deploy/image021.png)

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

**Scripts: Delegar permissões nos modelos de certificados existente.**  

![Diagrama](media/mim-cm-deploy/image039.png)

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
