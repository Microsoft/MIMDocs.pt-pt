---
title: Implementar o Gestor de certificados do Microsoft Identity Manager | Microsoft Docs
description: Instalar Gestor de certificados do Microsoft Identity Manager 2016
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/19/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 2473ef1c3d6fc5350d60d81bd508296a33343f01
ms.sourcegitcommit: 58d6c628d3bb770669348b987cf8f52ec0576132
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2017
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Implementação do Gestor de certificados do Microsoft Identity Manager 2016 (MIM CM)

A instalação do Gestor de certificados do Microsoft Identity Manager 2016 (MIM CM) envolve vários passos. Como uma forma para simplificar o processo, são interrompendo coisas para baixo. Existem preliminares passos que devem ser colocados antes de quaisquer passos de MIM CM reais. Sem o trabalho preliminar a instalação, é provável que falhem. 

1. Descrição geral da implementação
2. Passos de pré-implementação
3. Que outros problemas?

O diagrama abaixo mostra um exemplo do tipo de ambiente que pode ser utilizado. Os sistemas com os números são incluídos na lista abaixo o diagrama e são necessárias para concluir com êxito os passos abordados neste artigo. Por fim, são utilizados os servidores de centros de dados do Windows 2016:

![Diagrama de ambiente](media/mim-cm-deploy/image001.png)

1. CORPDC – controlador de domínio
2. CORPCM – servidor de MIM CM
3. CORPCA – autoridade de certificação
4. CORPCMR – MIM CM Rest API Web – CM Portal para a API Rest – utilizada para mais tarde
5. CORPSQL1-SP1 DO SQL SERVER 2016
6. CORPWK1 – associado a um domínio do Windows 10

## <a name="deployment-overview"></a>Descrição geral da implementação

- Instalação de sistema operativo base
  - O laboratório é composta por 2016 Centro de dados dos servidores do windows.
       >[!NOTE]
Para obter mais detalhes sobre as plataformas suportadas para o MIM 2016 observe o artigo intitulado [plataformas suportadas para o MIM 2016](/microsoft-identity-manager/microsoft-identity-manager-2016-supported-platforms.md)
- Passos de pré-implementação
  - [Expandir o esquema](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)
  - Criar contas de serviço
  - [Criar modelos de certificado](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)
  - IIS
  - Configuração de Kerberos
  - Passos relacionadas com a base de dados
    - Requisitos de configuração do SQL Server
    - Permissões de base de dados
- Implementação

## <a name="pre-deployment-steps"></a>Passos de pré-implementação

O Assistente de configuração de MIM CM requer informações devem ser fornecidas ao longo do processo para que este seja concluído com êxito. 
![](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>Expandir o esquema

O processo de expandir o esquema é simples, mas tem de ser approached com cuidado devido ao respetivo natureza irreversível.

>[!NOTE]
Este passo necessita que a conta utilizada tem direitos de administrador de esquema.

- Navegue para a localização do suporte de dados de MIM e navegue para \\Certificate Management\\x64 pasta.

- Copie a pasta de esquema para CORPDC, em seguida, navegue para a mesma.

    ![](media/mim-cm-deploy/image005.png)

- Execute o cenário de floresta único do script resourceForestModifySchema.vbs

- Para o cenário de floresta de recursos, execute os scripts:
  - DomainA – os utilizadores localizados (userForestModifySchema.vbs)
  - ResourceForestB – localização da instalação do CM (resourceForestModifySchema.vbs)

>[!NOTE]
Alterações do esquema são uma operação unidirecional e necessitam de uma floresta recuperação ao reverter por isso certifique-se de que tem cópias de segurança necessárias. Para obter detalhes sobre as alterações efetuadas ao esquema por efetuar esta operação, consulte o artigo [alterações do esquema de gestão de certificado do Forefront Identity Manager 2010](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx)

![](media/mim-cm-deploy/image007.png)

Execute o script e deverá receber um êxito mensagem uma vez que a conclusão do script.

![Mensagem de êxito](media/mim-cm-deploy/image009.png)

O esquema no AD agora é expandido para suportar o MIM CM.

### <a name="creating-service-accounts-and-groups"></a>Criar contas de serviço e grupos

A tabela seguinte resume as contas e permissões necessárias por MIM CM.
Pode permitir que o MIM CM crie as seguintes contas automaticamente, ou pode criá-los antes da instalação. Os nomes da conta real podem ser alterados. Se criar as contas por si, considere as contas de utilizador de forma a que seja fácil corresponder ao nome de conta de utilizador para a sua função de nomenclatura.

Utilizadores:

![](media/mim-cm-deploy/image010.png)

![](media/mim-cm-deploy/image012.png)

| **Função**                   | **Registo de utilizador no nome** |
|----------------------------|---------------------|
| Agente MIM CM               | MIMCMAgent          |
| MIM CM Key Recovery Agent  | MIMCMKRAgent        |
| Agente de autorização de MIM CM | MIMCMAuthAgent      |
| MIM CM CA Manager Agent    | MIMCMManagerAgent   |
| Agente de agrupamento de MIM CM Web      | MIMCMWebAgent       |
| Agente de inscrição de MIM CM    | MIMCMEnrollAgent    |
| Serviço MIM CM atualização      | MIMCMService        |
| Conta de instalação de MIM        | MIMINSTALL          |
| Agente de suporte técnico de ajuda            | CMHelpdesk1-2       |
| Gestor de CM                 | CMManager1-2        |
| Utilizador de subscritor            | CMUser1-2           |

Grupos:

| **Função**               | **Grupo**         |
|------------------------|-------------------|
| Membros de suporte técnico de CM    | Suporte técnico de MIMCM    |
| Membros do Gestor de CM     | Gestores de MIMCM    |
| Membros de subscritores de CM | Subscritores de MIMCM |

PowerShell: Contas de agente

```
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

| **Registo de utilizador no nome** | **A descrição e permissões**   |
|------|---------------------|
| MIMCMAgent          | Fornece os seguintes serviços: </br>-Obtém encriptados chaves privadas da AC. </br>-Protege as informações de PIN de smart card na base de dados FIM CM. </br>-Protege a comunicação entre o FIM CM e a AC. </br></br> Esta conta de utilizador requer as seguintes definições de controlo de acesso:</br>-   **Permitir início de sessão localmente** direito de utilizador.</br>-   **Emitir e gerir certificados** direito de utilizador. </br>-Ler e escrever permissão na pasta Temp sistema disponíveis na seguinte localização: % WINDIR %\\Temp.</br>-Um certificado de encriptação e assinatura digital emitidos e instalados no arquivo do utilizador.
|MIMCMKRAgent        | Recuperar arquivado chaves privadas da AC. Esta conta de utilizador requer as seguintes definições de controlo de acesso:</br> -   **Permitir início de sessão localmente** direito de utilizador.</br>-A associação ao local **administradores** grupo. </br>-Inscrever permissão no **KeyRecoveryAgent** modelo de certificado. </br>-Certificado Key Recovery Agent é emitido e instalado no arquivo do utilizador. O certificado tem de ser adicionado à lista de agentes a recuperação de chaves na AC. </br>-A permissão de leitura e escrita permissão na pasta Temp sistema disponíveis na seguinte localização:```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | Determina as permissões para utilizadores e grupos e direitos de utilizador. Esta conta de utilizador requer as seguintes definições de controlo de acesso: </br>-Associação no grupo de domínio de acesso de compatibilidade anterior ao Windows 2000. </br> -Concedidos a **gerar auditorias de segurança** direito de utilizador.             |
| MIMCMManagerAgent   | Executa as atividades de gestão de AC. </br> Este utilizador deve ser atribuído a permissão Gerir AC.        |
| MIMCMWebAgent       | Fornece a identidade para o conjunto aplicacional do IIS. FIM CM é executado dentro de um Microsoft Win32® programação interface processo de aplicação que utiliza as credenciais do utilizador. </br> Esta conta de utilizador requer as seguintes definições de controlo de acesso:</br> -A associação ao local **IIS_WPG, windows 2016 = IIS_IUSRS** grupo. </br>-A associação ao local **administradores** grupo.</br>-Concedidos a **gerar auditorias de segurança** direito de utilizador. </br>-Concedidos a **agir como parte do sistema operativo** direito de utilizador. </br>-Concedidos a **token de nível de processo de substituição** direito de utilizador.</br>-Atribuída como a identidade do conjunto aplicacional do IIS, **CLMAppPool**. </br>-Concedidos permissão de leitura a **HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\CLM\\v 1.0\\servidor\\WebUser** chave de registo. </br>-Esta conta também tem de ser fidedigna para delegação.|
| MIMCMEnrollAgent    | Efetua a inscrição em nome de um utilizador. Esta conta de utilizador requer as seguintes definições de controlo de acesso:</br>-Um certificado de agente de inscrição que é emitido e instalado no arquivo do utilizador.</br>-   **Permitir início de sessão localmente** direito de utilizador. </br>-Inscrever permissão no **Enrollment Agent** modelo de certificado (ou o modelo personalizado, se um é utilizado).                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Criar modelos de certificado para contas de serviço de MIM CM

Três as contas de serviço utilizadas por MIM CM requerem um certificado e o Assistente de configuração requer que forneça o nome dos modelos de certificados que deve utilizar para solicitar certificados para os mesmos.

As contas de serviço que necessitam de certificados são:

- MIMCMAgent: Esta conta necessita de um certificado de utilizador

- MIMCMEnrollAgent: Esta conta necessita de um certificado de agente de inscrição

- MIMCMKRAgent: Esta conta necessita um **agente de recuperação de chaves** certificado

Existem modelos já está presentes no AD, mas temos de criar o nossas própria versões para trabalhar com o MIM CM. Como é necessário efetuar a modificação a partir de modelos de linha de base original.

Todas as três das contas acima irão ter direitos na sua organização elevados e devem ser processadas cuidadosamente.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>Criar o modelo de certificado de assinatura de MIM CM

1. De **ferramentas administrativas**, abra **autoridade de certificação**.
2. No **autoridade de certificação** consola, na árvore da consola, expanda **Contoso CorpCA**e, em seguida, clique em **modelos de certificado**.
3. Clique com botão direito **modelos de certificado**e, em seguida, clique em **gerir**.
4. No **consola de modelos de certificado**, no **detalhes** painel, selecione e rato **utilizador**e, em seguida, clique em **Duplicar modelo** .
5. No **Duplicar modelo** caixa de diálogo, selecione **Windows Server 2003 Enterprise**e, em seguida, clique em **OK**.

![Mostrar as alterações resultantes](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    MIM CM does not work with certificates based on version 3 certificate templates. You must create a Windows Server® 2003 Enterprise (version 2)certificate template. See the following link for V3 details https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade

6. No **propriedades de novo modelo** caixa de diálogo a **geral** separador o **nome a apresentar do modelo** caixa, escreva **assinatura de MIM CM**. Alterar o **período de validade** para **2 anos**e, em seguida, desmarque a **Publicar certificado no Active Directory** caixa de verificação.

7. No **processamento de pedidos** separador, certifique-se de que o **permitir que a chave privada seja exportada** caixa de verificação está selecionada e, em seguida, clique em **separador criptografia**.

8. No **seleção de criptografia** caixa de diálogo, desativar **Microsoft avançada Cryptographic Provider v 1.0**, ativar **Microsoft avançada RSA e fornecedor de criptografia AES**e, em seguida, clique em **OK**.

No **nome do requerente** separador, limpe o **incluir o nome de correio eletrónico no nome do requerente** e **nome de correio eletrónico** caixas de verificação.

No **extensões** separador o **extensões incluídas neste modelo** lista, certifique-se de que **políticas de aplicações** está selecionada e, em seguida, clique em **editar **.

No **Editar extensão de políticas de aplicação** caixa de diálogo, selecione o **sistema de encriptação de ficheiros** e **proteger o E-Mail** políticas de aplicações. Clique em **remover**e, em seguida, clique em **OK**.

No **segurança** separador execute os seguintes passos:

- Remover **administrador**.

- Remover **Admins do domínio**.

- Remover **utilizadores de domínio**.

- Atribua apenas **leitura** e **escrever** permissões para **Admins de empresa**.

- Adicionar **MIMCMAgent.**

- Atribuir **leitura** e **inscrever** permissões para **MIMCMAgent**.

No **propriedades de novo modelo** caixa de diálogo, clique em **OK**.

Deixe o **consola de modelos de certificado** abrir.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>Criar o modelo de certificado de MIM CM Enrollment Agent

-   No **consola de modelos de certificado**, no **detalhes** painel, selecione e rato **Enrollment Agent**e, em seguida, clique em **Duplicar modelo**.

No **Duplicar modelo** caixa de diálogo, selecione **Windows Server 2003 Enterprise**e, em seguida, clique em **OK**.

No **propriedades de novo modelo** caixa de diálogo a **geral** separador o **nome a apresentar do modelo** caixa, escreva **MIM CM Enrollment Agent**. Certifique-se de que o **período de validade** é **2 anos**.

No **processamento de pedidos** separador, ative **permitir que a chave privada seja exportada**e, em seguida, clique em **CSPs ou separador criptografia.**

No **seleção de CSP** caixa de diálogo, desativar **Microsoft Base Cryptographic Provider v 1.0**, desativar **Microsoft avançada Cryptographic Provider v 1.0**, ativar ** Microsoft avançada RSA e fornecedor de criptografia AES**e, em seguida, clique em **OK**.

No **segurança** separador efetue o seguinte:

- Remover **administrador**.

- Remover **Admins do domínio**.

- Atribua apenas **leitura** e **escrever** permissões para **Admins de empresa**.

- Adicionar **MIMCMEnrollAgent**.

- Atribuir **leitura** e **inscrever** permissões para **MIMCMEnrollAgent**.

No **propriedades de novo modelo** caixa de diálogo, clique em **OK**.

Deixe o **consola de modelos de certificado** abrir.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>Criar o modelo de certificado de MIM CM Key Recovery Agent

1. No **modelos de certificado** na consola do **detalhes** painel, selecione e rato **Key Recovery Agent**e, em seguida, clique em **Duplicar modelo**.

2. No **Duplicar modelo** caixa de diálogo, selecione **Windows Server 2003 Enterprise**e, em seguida, clique em **OK**.

3. No **propriedades de novo modelo** caixa de diálogo a **geral** separador o **nome a apresentar do modelo** caixa, escreva **MIM CM Key Recovery Agent**. Certifique-se de que o **período de validade** é **2 anos** no **separador criptografia.**

4. No **fornecedores seleção** caixa de diálogo, desativar **Microsoft avançada Cryptographic Provider v 1.0**, ativar **Microsoft avançada RSA e fornecedor de criptografia AES**, e, em seguida, clique em **OK**.

5. No **requisitos de emissão** separador, certifique-se de que **aprovação do Gestor de certificados de AC** é **desativada**.

6. No **segurança** separador efetue o seguinte:

    - Remover **administrador**.

    - Remover **Admins do domínio**.

    - Atribua apenas **leitura** e **escrever** permissões para **Admins de empresa**.

    - Adicionar **MIMCMKRAgent**.

    - Atribuir **leitura** e **inscrever** permissões para **KRAgent**.

7. No **propriedades de novo modelo** caixa de diálogo, clique em **OK**.

8. Fechar o **consola de modelos de certificado**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>Publicar os modelos de certificado necessário em autoridade de certificação

1. Restaurar o **autoridade de certificação** consola.

2. No **autoridade de certificação** consola, na árvore da consola, faça duplo clique **modelos de certificado**, aponte para **novo**e, em seguida, clique em **certificado Modelo a emitir**.
3. No **ativar modelos de certificado** caixa de diálogo, selecione **MIM CM Enrollment Agent**, **MIM CM Key Recovery Agent**, e **MIM CM assinatura**. Clique em **OK**.
4. Na árvore da consola, clique em **modelos de certificado**.
5. Certifique-se de que os novos modelos de três aparecem no **detalhes** painel e, em seguida, feche **autoridade de certificação**.
    ![Assinatura de MIM CM](media/mim-cm-deploy/image016.png)
6. Feche todas as janelas abertas e terminar sessão.

### <a name="iis-configuration"></a>Configuração do IIS 

Por ordem para ordenação hoIn para alojar o Web site para CM, compartimento e configurar o IIS

#### <a name="install-and-configure-iis"></a>Instalar e configurar o IIS

1. Início de sessão para * * CORLog na como **MIMINSTALL** conta

>[!IMPORTANT]
A conta de instalação de MIM deve ser um administrador local

2. Abra o powershell e execute o seguinte comando

   - ```Install-WindowsFeature –ConfigurationFilePath```

>[!NOTE]
 Um site com o nome do Web Site predefinido está instalado por predefinição com o IIS 7. Se esse site foi alterado ou removido um site com o nome do Web Site predefinido deve estar disponível antes de instalar o MIM CM.

#### <a name="configuring-kerberos"></a>Configuração de Kerberos

A conta de MIMCMWebAgent executará o portal de MIM CM. Por predefinição no IIS e cópia de segurança kernel autenticação de modo é utilizada no IIS por predefinição. Irá desativar a autenticação de modo de kernel de Kerberos e configurar o SPN na conta MIMCMWebAgent em vez disso. Alguns comandos irão necessitar de permissão elevado no active directory e o servidor CORPCM.

![](media/mim-cm-deploy/image020.png)

```
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}

```

* * Atualizar o IIS no **CORPCM**


![](media/mim-cm-deploy/image022.png)

```
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true

```


>[!NOTE]
Terá de adicionar um registo DNS para "cm.contoso.com" e aponte para CORPCM IP

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>Exigir SSL no portal de MIM CM

Recomenda-se vivamente que necessitam de SSL no portal de MIM CM. Se não o assistente será o mesmo avise sobre o assunto.

1. Inscrever-se no certificado de web **cm.contoso.com** atribuir ao site predefinido

2. Abra **Gestor de IIS** e navegue para **gestão de certificados**

3. Na vista de funcionalidades, faça duplo clique em definições de SSL.

4. Na página de definições de SSL, selecione **exigir SSL**.

5. No painel ações, clique em **aplicar.**

### <a name="database-configuration-corpsql-for-mim-cm"></a>Base de dados de configuração **CORPSQL** de MIM CM

1. Certifique-se de que está ligado ao servidor CORPSQL01.

2. Certifique-se de que a sessão iniciada como DBA do SQL Server

3. Execute o seguinte script T-SQL para permitir que a CONTOSO\\MIMINSTALL conta para criar a base de dados quando, avance para o passo de configuração

>[!NOTE]
Vamos precisar de voltar a SQL quando que estão prontas para o módulo de saída & política

```
create login [CONTOSO\\MIMINSTALL] from windows;
exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
```

![Mensagem de erro de Assistente de configuração de MIM CM](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Implementação da gestão de certificados do Microsoft Identity Manager 2016

1. Certifique-se de que está ligado ao servidor CORPCM e que o **MIMINSTALL** conta é membro do **local de administradores** grupo.

2. Certifique-se de que tem sessão iniciada como Contoso\\MIMINSTALL.

3. Monte o ISO do SP1 do Microsoft Identity Manager.

4. **Abra** o **Certificate Management\\x64** diretório.

5. No **x64** janela, clique com botão direito **configuração**e, em seguida, clique em **executar como administrador**.

6. Bem-vindo à página do Assistente de configuração de gestão do Microsoft Identity Manager certificado, clique em **seguinte.**

7. Na página do contrato de licença de utilizador final, leia o contrato, ative o aceito os termos no contrato de licença **caixa de verificação**e, em seguida, clique em seguinte.

8. Na página de configuração personalizada, certifique-se de que o **Portal de MIM CM** e **componentes do serviço de atualização de MIM CM** estiver definida para ser instalada e, em seguida, **clique em seguinte**.

9. Na página de pasta do Web Virtual, certifique-se de que o nome da pasta Virtual * * CertificateManagement e, em seguida, **clique em seguinte**.

10. Na página de instalar o Microsoft Identity Manager Certificate Management, **clique em instalar**.

11. No **concluído** página do Assistente de configuração de gestão do Microsoft Identity Manager certificado, **clique em concluir**.

![Concluída o Assistente de MIM CM](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Assistente de configuração da gestão de certificados do Microsoft Identity Manager 2016

Antes de iniciar sessão CORPCM adicionar MIMINSTALL para **domínio administradores, administradores de esquema e dos administradores locais** grupo para o Assistente de configuração. Isto pode ser removido mais tarde após a conclusão da configuração.      
    
![Mensagem de erro](media/mim-cm-deploy/image028.png)

1. Do **iniciar** menu, clique em **Assistente de configuração de gestão de certificados**. E execute como **administrador**
2. No **bem-vindo ao Assistente de configuração** página, clique em **seguinte**.
3. No **configuração da AC** página, certifique-se de que a AC selecionada **CORPCA-Contoso-CA**, certifique-se de que o servidor selecionado **CORPCA. CONTOSO.COM**e, em seguida, clique em **seguinte**.
4. No **definir a segurança da Microsoft® SQL Server®** na página a **nome do SQL Server** caixa, escreva **CORPSQL1** , ativar o **utilizar as minhas credenciais para criar o base de dados** caixa de verificação e, em seguida, clique em **seguinte**.
5. No **definições de base de dados** página, aceite o nome de base de dados predefinido do **FIMCertificateManagement**, certifique-se de que **autenticação integrada do SQL Server** estiver selecionada e, em seguida, Clique em **seguinte**.

6. No **configurar o Active Directory** página, aceite o nome predefinido fornecido para o ponto de ligação de serviço e, em seguida, clique em **seguinte**.

7. No **método de autenticação** página Confirmar **autenticação integrada do windows** está selecionado, em seguida, clique em **seguinte**.

8. No **agentes – FIM CM** página, limpe o **utilizar as predefinições FIM CM** caixa de verificação e, em seguida, clique em **contas personalizadas**.

9. No **agentes – FIM CM** da caixa de diálogo várias separadores em cada separador, introduza as seguintes informações:
   - Nome de utilizador: **atualização** 
   - Palavra-passe: **passar\@word1**
   - O confirmar palavra-passe: **passar\@word1**
   - Utilize um utilizador existente: **ativado**
>[!NOTE]
Criámos anteriormente estas contas. Certifique-se de que os procedimentos no passo 8 são repetidos para todos os separadores de conta de agente seis.

![Contas de MIM CM](media/mim-cm-deploy/image030.png)

10. Quando todas as informações de conta do agente estiver concluídas, clique em **OK**.

11. No **agentes – MIM CM** página, clique em **seguinte**.

12. No **configurar certificados de servidor** página, ative os seguintes modelos de certificado:
    - Modelo de certificado a ser utilizado para o certificado de agente de recuperação de chaves do agente de recuperação: **MIMCMKeyRecoveryAgent**.
    - Modelo de certificado a ser utilizado para o certificado do FIM CM Agent: **MIMCMSigning**.
    - Modelo de certificado a ser utilizado para o certificado de agente de inscrição: **FIMCMEnrollmentAgent**.
13. No **certificados de servidor de configuração** página, clique em **seguinte**.
14. No **a configuração do servidor de correio electrónico, impressão de documentos** na página de **especifique o nome do servidor SMTP que pretende utilizar para notificações de registo de mensagens de correio electrónico** caixa e, em seguida, clique em **seguinte.**
15. No **pronto para configurar** página, clique em **configurar**.
16. No **Assistente de configuração – Microsoft Forefront Identity Manager 2010 R2** aviso caixa de diálogo, clique em **OK** a confirmar que o SSL não está ativado no diretório virtual do IIS.

    ![suporte de dados/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    Não clique no botão Concluir até que a execução do Assistente de configuração está concluída. Registo para o assistente pode ser encontrado aqui:**% programfiles %\\Microsoft Forefront Identity Management\\2010\\Certificate Management\\config.log**
17. Clique em **Concluir**.

![Concluída o Assistente de MIM CM](media/mim-cm-deploy/image033.png)

18. Feche todas as abra windows.

19. Adicione https://cm.contoso.com/certificatemanagement à zona local intranet no seu browser.

20. Visite o site do servidor CORPCM https://cm.contoso.com/certificatemanagement  

    ![](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>Verifique se o serviço de isolamento da chave CNG

1. De **ferramentas administrativas**, abra **serviços**.

2. No **detalhes** painel, faça duplo clique em **isolamento de chave CNG**.

3. No **geral** separador, altere o **o tipo de arranque** para **automática**.

4. No **geral** separador, inicie o serviço se não estiver num estado iniciado.

5. No **geral** separador, clique em **OK**.

### <a name="installing-and-configuring-the-ca-modules-"></a>Instalar e configurar os módulos de AC:

Neste passo, vamos irão instalar e configurar os módulos de AC do FIM CM na autoridade de certificação.

1. Configurar o FIM CM para inspecionar apenas permissões de utilizador para operações de gestão

2. No **c:\\os ficheiros de programa\\Microsoft Forefront Identity Manager\\2010\\Certificate Management\\web** janela, crie uma cópia do ** Web. config** a cópia de nomenclatura **web.1.config**.

3. No **Web** janela, clique com botão direito **Web. config**e, em seguida, clique em **abra**.

    >[!Note]
    Abrir o ficheiro Web. config no bloco de notas

4. Quando abre o ficheiro, prima CTRL + F.

5. No **localizar e substituir** caixa de diálogo a **localizar** caixa, escreva **UseUser**e, em seguida, clique em **Localizar seguinte** três vezes.

6. Fechar o **localizar e substituir** caixa de diálogo.

7. Deve estar na linha ** \<adicionar key="Clm.RequestSecurity.Flags" valor = "UseUser, UseGroups" /\>**. Altere a linha ler ** \<adicionar key="Clm.RequestSecurity.Flags" valor = "UseUser" /\>**.

8. Feche o ficheiro, guardar todas as alterações.

9. Criar uma conta de computador da AC no servidor SQL \<não existe nenhum script\>

10. Certifique-se de que estão ligadas ao **CORPSQL01** servidor.

11. Certifique-se de que tem sessão iniciada como **DBA**

12. Do **iniciar** menu, inicie **SQL Server Management Studio**.

13. No **ligar ao servidor** caixa de diálogo a **nome do servidor** caixa, escreva **CORPSQL01,** e, em seguida, clique em **Connect**.

14. Na árvore da consola, expanda **segurança**e, em seguida, clique em **inícios de sessão**.

15. Clique com botão direito **inícios de sessão**e, em seguida, clique em **novo início de sessão**.

16. No **geral** na página de **nome de início de sessão** caixa, escreva **contoso\\CORPCA\$**. Selecione **autenticação do Windows**. Base de dados predefinida é **FIMCertificateManagement**.

17. No painel esquerdo, selecione **mapeamento de utilizadores**. No painel direito, clique na caixa de verificação no **mapa** coluna junto **FIMCertificateManagement**. No **membro da função para a base de dados: FIMCertificateManagement** lista, ative o **clmApp** função.

18. Clique em **OK**.

19. Fechar **Microsoft SQL Server Management Studio**.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>Instalar os módulos de AC do FIM CM na autoridade de certificação

1. Certifique-se de que estão ligadas ao **CORPCA** servidor.

2. No **X64** windows, clique com botão direito **Setup.exe**e, em seguida, clique em **executar como administrador**.

3. No **bem-vindo ao Assistente de configuração de gestão de certificado Microsoft Identity Manager** página, clique em **seguinte**.

4. No **contrato de licença de utilizador final** página, leia o contrato. Selecione o **aceito os termos no contrato de licença** caixa de verificação e, em seguida, clique em **seguinte**.

5. No **configuração personalizada** página, selecione **Portal de MIM CM**e, em seguida, clique em **esta funcionalidade não estará disponível**.

6. No **configuração personalizada** página, selecione **o serviço de atualização de MIM CM**e, em seguida, clique em **esta funcionalidade não estará disponível**.

    >[!Note]
    Isto irá deixar os ficheiros de AC de MIM CM como a única funcionalidade ativada para a instalação.

7. No **configuração personalizada** página, clique em **seguinte**.

8. No **instalar o Microsoft Identity Manager Certificate Management** página, clique em **instalar**.

9. No **concluir o Assistente de configuração de gestão do Microsoft Identity Manager certificado** página, clique em **concluir**.

10. Feche todas as abra windows.

### <a name="configure-the-mim-cm-exit-module"></a>Configurar o módulo de saída do CM de MIM

1. De **ferramentas administrativas**, abra **autoridade de certificação**.

2. Na árvore da consola, faça duplo clique **CORPCA-contoso-CA**e, em seguida, clique em **propriedades**.

3. No **módulo de saída** separador, selecione **módulo de saída do FIM CM**e, em seguida, clique em **propriedades**.

4. No **especifique a cadeia de ligação de base de dados do CM** caixa, escreva **Connect Timeout = 15; Manter informações de segurança = True; Segurança integrada = sspi; Initial Catalog = FIMCertificateManagement; origem de dados = CORPSQL01**. Deixe o **encriptar a cadeia de ligação** ativada de caixa de verificação e, em seguida, clique em **OK**.
5. No **Microsoft FIM Certificate Management** caixa de mensagens, clique em **OK**.

6. No **CORPCA-contoso-CA propriedades** caixa de diálogo, clique em **OK**.

7. Clique com botão direito **CORPCA-contoso-CA***,* aponte para **todas as tarefas**e, em seguida, clique em **parar o serviço**. Aguarde até que deixa de serviços de certificados do Active Directory.

8. Clique com botão direito **CORPCA-contoso-CA***,* aponte para **todas as tarefas**e, em seguida, clique em **iniciar serviço**.

9. Minimizar o **autoridade de certificação** consola.

10. De **ferramentas administrativas**, abra **Visualizador de eventos**.

11. Na árvore da consola, expanda **registos de serviços e aplicações**e, em seguida, clique em **FIM Certificate Management**.

12. Na lista de eventos, certifique-se de que os eventos mais recentes fazer *não* incluir qualquer **aviso** ou **erro** eventos desde o último reinício dos serviços de certificados.

    >[!NOTE] 
    O último evento deve indicar que o módulo de saída carregado utilizando as definições do```SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit```

13. Minimizar o **Visualizador de eventos**.

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>Copie o thumbprint do certificado MIMCMAgent para Windows® área de transferência

1. Restaurar o **autoridade de certificação** consola.

2. Na árvore da consola, expanda **CORPCA-contoso-CA**e, em seguida, clique em **certificados emitidos pela**.

3. No **detalhes** painel, faça duplo clique no certificado com **CONTOSO\\MIMCMAgent** no **nome do autor do pedido** coluna e com **FIM CM Assinatura** no **modelo de certificado** coluna.

4. No **detalhes** separador, selecione o **Thumbprint** campo.

5. Selecione o thumbprint e, em seguida, prima CTRL + C.

    >[!NOTE]
    Efetue **não** incluem o espaço à esquerda na lista de carateres de thumbprint.

6. No **certificado** caixa de diálogo, clique em **OK**.

7. Do **iniciar** menu, no **procurar programas e ficheiros** caixa, escreva **bloco de notas**, e, em seguida, prima ENTER.

8. No **bloco de notas**, do **editar** menu, clique em **colar**.

9. Do **editar** menu, clique em **substituir**.

10. No **localizar** caixa, escreva um caráter de espaço e, em seguida, clique em **substituir todos os**.

    >[!Note]
    Esta ação remove todos os espaços entre os carateres da impressão digital.
    
11. No **substituir** caixa de diálogo, clique em **Cancelar**.

12. Selecione o convertida *thumbprintstring*, e, em seguida, prima CTRL + C.

13. Fechar **bloco de notas** sem a guardar as alterações.

### <a name="configure-the-fim-cm-policy-module"></a>Configurar o módulo de política de CM do FIM

1. Restaurar o **autoridade de certificação** consola.

2. Clique com botão direito **CORPCA-contoso-CA**e, em seguida, clique em **propriedades**.

3.  No **CORPCA-contoso-CA propriedades** caixa de diálogo a **módulo de política** separador, clique em **propriedades**.

- No **geral** separador, certifique-se de que **passar pedidos não - FIM CM ao módulo de política predefinido para processamento** está selecionada.
- No **certificados de assinatura** separador, clique em **adicionar**.
- Na caixa de diálogo certificado, clique com botão direito do **especifique o hash do certificado codificado em hex** caixa e, em seguida, clique em **colar**.
- No **certificado** caixa de diálogo, clique em **OK**.
    >[!Note]
    Se o **OK** botão não estiver ativado, que incluiu acidentalmente um caráter ocultado na cadeia de thumbprint quando copiou o thumbprint do certificado clmAgent. Repita os passos de todos os a partir de **Tarefa 4: copiar o MIMCMAgent Thumbprint do certificado para a área de transferência do Windows** neste exercício.

- No **propriedades de configuração** diálogo caixa, certifique-se de que o thumbprint aparece no **certificados de assinatura válidos** lista e, em seguida, clique em **OK**.

- No **FIM Certificate Management** caixa de mensagens, clique em **OK**.

- No **CORPCA-contoso-CA propriedades** caixa de diálogo, clique em **OK**.

- Clique com botão direito **CORPCA-contoso-CA***,* aponte para **todas as tarefas**e, em seguida, clique em **parar o serviço**.

- Aguarde até que deixa de serviços de certificados do Active Directory.

- Clique com botão direito **CORPCA-contoso-CA***,* aponte para **todas as tarefas**e, em seguida, clique em **iniciar serviço**.

- Fechar o **autoridade de certificação** consola.

- Feche todas as janelas abertas e, em seguida, termine a sessão.

- **Último passo na implementação** é queremos certificar-se de que CONTOSO\\gestores de MIMCM podem implementar e criar modelos e configurar o sistema sem ser de esquema e Admins do domínio. O script seguinte irá ACL as permissões para os modelos de certificado utilizando dsacls. Execute com a conta que tenha permissão total para alterar a segurança de leitura e escrita permissões para cada modelo de certificado existente na floresta.

- Passos primeiro: **configurar permissões de grupo de destino e de ponto de ligação de serviço & delegar a gestão de modelo de perfil**
  - Configure as permissões no ponto de ligação de serviço (SCP).

  - Configure a gestão de modelo de perfil de delegado.

  - Configure as permissões no ponto de ligação de serviço (SCP). **\<não existe nenhum script\>**

        -   Certifique-se de que estão ligadas ao **CORPDC** servidor virtual.

        -   Inicie sessão como **contoso\\corpadmin**

        -   De **ferramentas administrativas**, abra **computadores e utilizadores do Active Directory**.

        -   No **computadores e utilizadores do Active Directory**, no **vista** menu, certifique-se de que **funcionalidades avançadas** está ativada.

        -   Na árvore da consola, expanda **Contoso.com** \| **sistema** \| **Microsoft** \| **ciclo de vida do certificado Gestor de**e, em seguida, clique em **CORPCM**.

        -   Clique com botão direito **CORPCM**e, em seguida, clique em **propriedades**.

        -   No **CORPCM propriedades** caixa de diálogo a **segurança** separador, adicione os seguintes grupos com as permissões correspondentes:

    | Grupo          | Permissões                                                                                                                                                         |
    |----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Gestores de mimcm | Ler </br> Auditoria do FIM CM</br> Agente de inscrição do FIM CM</br> Inscrever o pedido do FIM CM</br> Recuperar do FIM CM pedido</br> Renovar o pedido do FIM CM</br> Revogação do FIM CM pedido </br> Pedido do FIM CM desbloqueio de Smart Card |
    | Suporte técnico de MIMCM | Ler</br> Agente de inscrição do FIM CM</br> Revogação do FIM CM pedido</br> Pedido do FIM CM desbloqueio de Smart Card                                                                                |
- No **CORPDC propriedades** caixa de diálogo, clique em **OK**.

- Deixe **computadores e utilizadores do Active Directory** abrir.

- **Configurar as permissões sobre os objetos de utilizador de subordinados**
    - Certifique-se de que ainda no **computadores e utilizadores do Active Directory** consola.
    - Na árvore da consola, faça duplo clique **Contoso.com**e, em seguida, clique em **propriedades**.
    - No **segurança** separador, clique em **avançadas**.
    - No **definições de segurança avançadas para a Contoso** caixa de diálogo, clique em **adicionar**.
    - No **selecionar utilizador, computador, conta de serviço ou grupo** caixa de diálogo a **introduza o nome de objeto a selecionar** caixa, escreva **gestores de mimcm**e, em seguida, clique em **OK**.
    - No **entrada de permissão para Contoso** caixa de diálogo a **aplicam-se a** lista, selecione **objetos de utilizador descendente** e, em seguida, ative o **permitir**caixa de verificação para o seguinte **permissões**:
        - **Ler todas as propriedades**
        - **Permissões de leitura**
        - **Auditoria do FIM CM**
        - **Agente de inscrição do FIM CM**
        - **Inscrever o pedido do FIM CM**
        - **Recuperar do FIM CM pedido**
        - **Renovar o pedido do FIM CM**
        - **Revogação do FIM CM pedido**
        - **Pedido do FIM CM desbloqueio de Smart Card**
    - No **entrada de permissão para Contoso** caixa de diálogo, clique em **OK**.
    - No **definições de segurança avançadas para a Contoso** caixa de diálogo, clique em **adicionar**.
    - No **selecionar utilizador, computador, conta de serviço ou grupo** caixa de diálogo a **introduza o nome de objeto a selecionar** caixa, escreva **mimcm técnico**e, em seguida, clique em **OK**.
    - No **entrada de permissão para Contoso** caixa de diálogo a **aplicam-se a** lista, selecione **objetos de utilizador descendente** e, em seguida, selecione o **permitir**caixa de verificação para o seguinte **permissões**: - **ler todas as propriedades** - **permissões de leitura** - **FIM CM Enrollment Agent** - **FIM CM pedido revogar** - **FIM CM pedido de desbloqueio de Smart Card**
    - No **entrada de permissão para Contoso** caixa de diálogo, clique em **OK**.
    -Na **definições de segurança avançadas para a Contoso** caixa de diálogo, clique em **OK**.
    - No **contoso.com propriedades** caixa de diálogo, clique em **OK**.
    - Deixe **computadores e utilizadores do Active Directory** abrir.

    - **Configurar as permissões sobre os objetos de utilizador subordinados \<não existe nenhum script\>**
        - Certifique-se de que ainda no **computadores e utilizadores do Active Directory** consola.
        - Na árvore da consola, faça duplo clique **Contoso.com**e, em seguida, clique em **propriedades**.
        - No **segurança** separador, clique em **avançadas**.
        - No **definições de segurança avançadas para a Contoso** caixa de diálogo, clique em **adicionar**.
        - No **selecionar utilizador, computador, conta de serviço ou grupo** caixa de diálogo a **introduza o nome de objeto a selecionar** caixa, escreva **gestores de mimcm**e, em seguida, clique em **OK**.
        - No **entrada de permissão para CONTOSO** caixa de diálogo a **aplicam-se a** lista, selecione **objetos de utilizador descendente** e, em seguida, ative o **permitir**caixa de verificação para o seguinte **permissões**:
            - **Ler todas as propriedades**
            - **Permissões de leitura**
            - **Auditoria do FIM CM**
            - **Agente de inscrição do FIM CM**
            - **Inscrever o pedido do FIM CM**
            - **Recuperar do FIM CM pedido**
            - **Renovar o pedido do FIM CM**
            - **Revogação do FIM CM pedido**
            - **Pedido do FIM CM desbloqueio de Smart Card**
    - No **entrada de permissão para CONTOSO** caixa de diálogo, clique em **OK**.
    - No **definições de segurança avançadas para a CONTOSO** caixa de diálogo, clique em **adicionar**.
    - No **selecionar utilizador, computador, conta de serviço ou grupo** caixa de diálogo a **introduza o nome de objeto a selecionar** caixa, escreva **mimcm técnico**e, em seguida, clique em **OK**.
    - No **entrada de permissão para CONTOSO** caixa de diálogo a **aplicam-se a** lista, selecione **objetos de utilizador descendente** e, em seguida, selecione o **permitir**caixa de verificação para o seguinte **permissões**: - **ler todas as propriedades** - **permissões de leitura** - **FIM CM Enrollment Agent** - **FIM CM pedido revogar** - **FIM CM pedido de desbloqueio de Smart Card**
    - No **entrada de permissão para contoso** caixa de diálogo, clique em **OK**.
    - No **definições de segurança avançadas para a Contoso** caixa de diálogo, clique em **OK**.
    - No **contoso.com propriedades** caixa de diálogo, clique em **OK**.
    - Deixe **computadores e utilizadores do Active Directory** abrir.
- Passos de segundo: **permissões de gestão de modelo de certificado Delegating \<script\>**
    - Delegar permissões no contentor de modelos de certificado.
    - Delegar permissões no contentor de OID.
    - Delegar permissões nos modelos de certificados existente.
- Definir as permissões no contentor de modelos de certificado
     1. Restaurar o **serviços e locais do Active Directory** consola.
     2. Na árvore da consola, expanda **serviços**, expanda **Public Key Services**e, em seguida, clique em **modelos de certificado**.
     3. Na árvore da consola, faça duplo clique **modelos de certificado**e, em seguida, clique em **delegar controlo**.
     4. No **delegação de controlo** assistente, clique em **seguinte**.
     5. No **utilizadores ou grupos** página, clique em **adicionar**.
     6. No **selecionar utilizadores, computadores ou grupos** caixa de diálogo a **introduzir os nomes de objeto a selecionar** caixa, escreva **gestores de mimcm**e, em seguida, clique em **OK**.
     7. No **utilizadores ou grupos** página, clique em **seguinte**.
     8. No **tarefas a delegar** página, clique em **criar uma tarefa personalizada para delegar**e, em seguida, clique em **seguinte**.
     9.  No **o tipo de objeto do Active Directory** página, certifique-se de que **nesta pasta, objetos existentes na pasta e, a criação de novos objetos nesta pasta** está selecionada e, em seguida, clique em **seguinte**.
     10. No **permissões** na página de **permissões** lista, selecione o **controlo total** caixa de verificação e, em seguida, clique em **seguinte**.
     11. No **a concluir o Assistente de delegação de controlo** página, clique em **concluir**.

- Definir as permissões no contentor de OID
     1. Na árvore da consola, faça duplo clique **OID**e, em seguida, clique em **propriedades**.
     2. No **OID propriedades** caixa de diálogo a **segurança** separador, clique em **avançadas**.
     3. No **definições de segurança avançadas para OID** caixa de diálogo, clique em **adicionar**.
     4. No **selecionar utilizador, computador, conta de serviço ou grupo** caixa de diálogo a **introduza o nome de objeto a selecionar** caixa, escreva **gestores de mimcm**e, em seguida, clique em **OK**.
     5. No **permissões de entrada de OID** diálogo caixa, certifique-se de que as permissões se aplicam a **este objeto e todos os objetos subordinados**, clique em **controlo total**e, em seguida, clique em ** OK**.
     6. No **definições de segurança avançadas para OID** caixa de diálogo, clique em **OK**.
     7. No **OID propriedades** caixa de diálogo, clique em **OK**.
     8. Fechar **serviços e locais do Active Directory**.

**Scripts: Permissões no contentor de OID, modelo de perfil e modelos de certificado**

![](media/mim-cm-deploy/image021.png)

' ' import-module activedirectory $adace = @{"OID" = "AD:\\CN = OID, CN = Public Key Services, CN = Services, CN = Configuration, DC = contoso, DC = com"; "Cionar" = "AD:\\CN = modelos de certificado, CN = Public Key Services, CN = Services, CN = Configuration, DC = contoso, DC = com"; "PT" = "AD:\\CN = modelos de perfil, CN = Public Key Services, CN = Services, CN = Configuration, DC = contoso, DC = com"} $adace. GetEnumerator() | **Foreach-Object** {$acl = **Get-Acl** *-caminho* $_. Valor $sid = (**Get-ADGroup** "Gestores de MIMCM"). SID $p = **novo objeto** System.Security.Principal.SecurityIdentifier($sid)
##<a name="httpsmsdnmicrosoftcomen-uslibrarysystemdirectoryservicesactivedirectorysecurityinheritancevvs110aspx"></a>https://msdn.microsoft.com/en-us/library/System.DirectoryServices.activedirectorysecurityinheritance (v=vs.110).aspx
$ace = **New-Object** System.DirectoryServices.ActiveDirectoryAccessRule ($p,[System.DirectoryServices.ActiveDirectoryRights]"GenericAll",[System.Security.AccessControl.AccessControlType]::Allow, [ DirectoryServices.ActiveDirectorySecurityInheritance]::All) $acl. AddAccessRule($ace) **conjunto Acl** *-caminho* $_. Valor *- AclObject* $acl}
```

**Scripts: Delegating permissions on the existing certificate templates.**

![](media/mim-cm-deploy/image039.png)

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
