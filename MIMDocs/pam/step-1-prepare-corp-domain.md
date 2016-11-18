---
title: "Implementar o PAM Passo 1 – Domínio CORP| Documentos da Microsoft"
description: "Preparar o domínio CORP com identidades novas ou existentes para ser gerido por Privileged Identity Manager"
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 127d368c15cce125ba7f69302cfa329b600d9498


---

# <a name="step-1-prepare-the-host-and-the-corp-domain"></a>Passo 1 – preparar o anfitrião e o domínio CORP

>[!div class="step-by-step"]
[Passo 2 »](step-2-prepare-priv-domain-controller.md)


Neste passo, irá preparar para alojar o ambiente bastion. Se for necessário, também pode criar um controlador de domínio e uma estação de trabalho do membro num novo domínio e floresta (uma floresta *CORP*) com identidades para serem geridas pelo ambiente bastion. Esta floresta CORP simula uma floresta existente que tem recursos para serem geridos. Este documento inclui um recurso de exemplo para ser protegido, uma partilha de ficheiros.

Se já tiver um domínio existente do Active Directory (AD) com um controlador de domínio com o Windows Server 2012 R2 ou posterior, onde é um administrador de domínio, ao invés, pode utilizar esse domínio.  

## <a name="prepare-the-corp-domain-controller"></a>Preparar o controlador de domínio CORP

Esta secção descreve como configurar um controlador de domínio para um domínio CORP. No domínio CORP, os utilizadores administrativos são geridos pelo ambiente bastion. O nome do Sistema de Nomes de Domínio (DNS) do domínio CORP utilizado neste exemplo é *contoso.local*.

### <a name="install-windows-server"></a>Instale o Windows Server

Instale o Windows Server 2012 R2 ou a Pré-visualização Técnica 4 do Windows Server 2016 ou posterior, numa máquina virtual, para criar um computador designado *CORPDC*.

1. Escolha ** Windows Server 2012 R2 Standard (Servidor com uma GUI) x64** ou **Pré-visualização Técnica do Windows Server 2016 (Servidor com Experiência de Ambiente de Trabalho)**.

2. Reveja e aceite os termos do licenciamento.

3. Uma vez que o disco estará vazio, selecione **Personalizar: instalar apenas o Windows** e utilizar o espaço em disco não inicializado.

4. Inicie sessão no novo computador como administrador. Navegue para o Painel de Controlo. Defina o nome do computador como *CORPDC* e conceda-lhe um endereço IP estático na rede virtual. Reinicie o servidor.

5. Após o reinício do servidor, inicie sessão como administrador. Navegue para o Painel de Controlo. Configure o computador para verificar se existem atualizações e instale as atualizações necessárias. Reinicie o servidor.

### <a name="add-roles-to-establish-a-domain-controller"></a>Adicione funções para estabelecer um controlador de domínio

Nesta secção, irá adicionar os Serviços de Domínio do Active Directory (AD DS), o Servidor DNS e funções de Servidor de Ficheiros (parte da secção de Ficheiros e Serviços de Armazenamento) e promover este servidor para um controlador de domínio de uma nova floresta contoso.local.

> [!NOTE]  
> Se já tiver um domínio para utilizar como o seu domínio CORP e esse domínio utilizar o Windows Server 2012 R2 ou posterior como o respetivo controlador de domínio, pode saltar para [Criar outros utilizadores e grupos para fins de demonstração](#create-additional-users-and-groups-for-demonstration-purposes).

1. Enquanto tiver sessão iniciada como administrador, inicie o PowerShell.

2. Escreva os seguintes comandos.

  ```
  import-module ServerManager

  Add-WindowsFeature AD-Domain-Services,DNS,FS-FileServer –restart –IncludeAllSubFeature -IncludeManagementTools

  Install-ADDSForest –DomainMode Win2008R2 –ForestMode Win2008R2 –DomainName contoso.local –DomainNetbiosName contoso –Force -NoDnsOnNetwork
  ```

  Esta ação irá solicitar uma Palavra-passe de Administrador de Modo Seguro a utilizar. Tenha em atenção que as definições de criptografia e a delegação de DNS serão apresentadas. Estas são normais.

3. Depois de a criação da floresta estar concluída, termine sessão. O servidor irá reiniciar automaticamente.

4. Depois de reiniciar o servidor, inicie sessão no CORPDC como um administrador do domínio. É, geralmente, o Administrador\\CONTOSO do utilizador, que terá a palavra-passe que foi criada quando instalou o Windows no CORPDC.

### <a name="create-a-group"></a>Criar um grupo

Crie um grupo para fins de auditoria do Active Directory, se o grupo ainda não existir. O nome do grupo tem de ser o nome de domínio NetBIOS seguido de três cifrões, por exemplo *CONTOSO$$$*.

Para cada domínio, inicie sessão no controlador de domínio como um administrador de domínio e execute os seguintes passos:

1. Inicie o PowerShell.

2. Escreva os comandos seguintes, mas substitua "CONTOSO" pelo nome NetBIOS do seu domínio.

  ```
  import-module activedirectory

  New-ADGroup –name 'CONTOSO$$$' –GroupCategory Security –GroupScope DomainLocal –SamAccountName 'CONTOSO$$$'
  ```

Em alguns casos o grupo já pode existir - isto é normal se o domínio também foi utilizado em cenários de migração do AD.

### <a name="create-additional-users-and-groups-for-demonstration-purposes"></a>Crie utilizadores e grupos adicionais para fins de demonstração

Se tiver criado um novo domínio CORP, deve criar utilizadores e grupos adicionais para demonstrar o cenário de PAM. O utilizador e o grupo para fins de demonstração não devem ser administradores de domínio ou controlados pelas definições de adminSDHolder no AD.

> [!NOTE]
> Se já tiver um domínio que irá utilizar como o domínio CORP e tem um utilizador e um grupo que pode utilizar para fins de demonstração, pode saltar para a secção [Configurar a auditoria](#configure-auditing).

Vamos criar um grupo de segurança denominado *CorpAdmins* e um utilizador com o nome *Jen*. Pode utilizar nomes diferentes se desejar.

1. Inicie o PowerShell.

2. Escreva os seguintes comandos. Substitua a palavra-passe 'Pass@word1' por uma cadeia de palavra-passe diferente.

  ```
  import-module activedirectory

  New-ADGroup –name CorpAdmins –GroupCategory Security –GroupScope Global –SamAccountName CorpAdmins

  New-ADUser –SamAccountName Jen –name Jen

  Add-ADGroupMember –identity CorpAdmins –Members Jen

  $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

  Set-ADAccountPassword –identity Jen –NewPassword $jp

  Set-ADUser –identity Jen –Enabled 1 -DisplayName "Jen"
  ```

### <a name="configure-auditing"></a>Configurar a auditoria

Tem de ativar a auditoria em florestas existentes para estabelecer a configuração de PAM nessas florestas.  

Para cada domínio, inicie sessão no controlador de domínio como um administrador de domínio e execute os seguintes passos:

1. Aceda a **Iniciar** > **Ferramentas Administrativas** (ou, no Windows Server 2016, **Ferramentas Administrativas do Windows**) e inicie a **Gestão de Políticas de Grupo**.

2. Navegue para a política de controladores de domínio deste domínio.  Se tiver criado um novo domínio para contoso.local, navegue para **Floresta: contoso.local** > **Domínios** > **contoso.local** > **Controladores de Domínio** > **Política de Controladores de Domínio Predefinida**. Aparece uma mensagem informativa.

3. Clique com o botão direito do rato em **Política de Controladores de Domínio Predefinida** e selecione **Editar**. Aparece uma nova janela.

4. Na janela Editor de Gestão de Políticas de Grupo, na árvore da Política de Controladores de Domínio Predefinida, navegue para **Configuração do Computador** > **Políticas** > **Definições do Windows** > **Definições de Segurança** > **Políticas Locais** > **Política de Auditoria**.

5. No painel de detalhes, clique com o botão direito do rato em **Gestão de contas de auditoria** e selecione **Propriedades**. Selecione **Definir estas definições de política**, marque a caixa de verificação **Êxito** e a caixa de verificação **Falha**, clique em **Aplicar** e em **OK**.

6. No painel de detalhes, clique com o botão direito do rato em **Auditar acesso ao serviço de diretórios** e selecione **Propriedades**. Selecione **Definir estas definições de política**, marque a caixa de verificação **Êxito** e a caixa de verificação **Falha**, clique em **Aplicar** e **OK**.

7. Feche a janela Editor de Gestão de Políticas de Grupo e a janela Gestão de Políticas de Grupo.

8. Aplique as definições de auditoria ao iniciar uma janela do PowerShell e escreva:

  ```
  gpupdate /force /target:computer
  ```

A mensagem **A atualização da Política de Computador foi concluída com êxito** deve aparecer após alguns minutos.

### <a name="configure-registry-settings"></a>Configurar definições de registo

Nesta secção irá configurar as definições de registo que são necessárias para a migração do Histórico do sID, que será utilizado para a criação do grupo de Gestão de Acesso Privilegiado.

1. Inicie o PowerShell.

2. Escreva os seguintes comandos para configurar o domínio de origem e permitir o acesso de chamada de procedimento remoto (RPC) à base de dados do gestor de contas de segurança (SAM).

  ```
  New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1

  Restart-Computer
  ```

Esta ação irá reiniciar o controlador de domínio, CORPDC. Para obter mais informações sobre esta definição de registo, consulte [Como resolver problemas de migração sIDHistory entre florestas com o ADMTv2](http://support.microsoft.com/kb/322970).

## <a name="prepare-a-corp-workstation-and-resource"></a>Prepare um recurso e estação de trabalho CORP

Se ainda não tiver um computador de estação de trabalho associado ao domínio, siga estas instruções para preparar um.  

> [!NOTE]
> Se já tiver uma estação de trabalho associada ao domínio, avance para [Criar um recurso para fins de demonstração](#create-a-resource-for-demonstration-purposes).

### <a name="install-windows-81-or-windows-10-enterprise-as-a-vm"></a>Instalar o Windows 8.1 ou Windows 10 Enterprise como uma VM

Noutra máquina virtual nova sem nenhum software instalado, instale o Windows 8.1 Enterprise ou Windows 10 Enterprise para tornar um computador *CORPWKSTN*.

1. Utilize as definições Express durante a instalação.

2. Tenha em atenção que a instalação poderá não conseguir estabelecer ligação à Internet. Selecione **Criar uma conta local**. Especifique um nome de utilizador diferente; não utilize "Administrador" ou "Joana".

3. Com o Painel de Controlo, dê um endereço IP estático a este computador na rede virtual e defina o servidor DNS preferencial da interface para ser do servidor do CORPDC.

4. Com o Painel de Controlo, o domínio associa o computador CORPWKSTN ao domínio contoso.local. Terá de fornecer credenciais de administrador do domínio Contoso. Quando estiver concluído, reinicie o computador CORPWKSTN.

### <a name="create-a-resource-for-demonstration-purposes"></a>Crie um recurso para fins de demonstração

É necessário um recurso para demonstrar o controlo de acesso baseado no grupo de segurança com PAM.  Se ainda não tiver um recurso, pode utilizar uma pasta de ficheiros para efeitos de demonstração.  Este procedimento irá utilizar os objetos do AD "Jen" e "CorpAdmins" que criou no domínio contoso.local.

1. Ligue à estação de trabalho CORPWKSTN. Clique no ícone **Trocar utilizador** e, em seguida, **Outro utilizador**. Certifique-se de que o utilizador CONTOSO\\Jen pode iniciar sessão no CORPWKSTN.

2. Crie uma nova pasta com o nome *CorpFS* e partilhe-a com o grupo *CorpAdmins*.

3. Abra o PowerShell como Administrador.

4. Escreva os seguintes comandos.

  ```
  mkdir c:\corpfs

  New-SMBShare –Name corpfs –Path c:\corpfs –ChangeAccess CorpAdmins

  $acl = Get-Acl c:\corpfs

  $car = New-Object System.Security.AccessControl.FileSystemAccessRule( "CONTOSO\CorpAdmins", "FullControl", "Allow")

  $acl.SetAccessRule($car)

  Set-Acl c:\corpfs $acl
  ```

No próximo passo, irá preparar o controlador de domínio do PRIV.

>[!div class="step-by-step"]
[Passo 2 »](step-2-prepare-priv-domain-controller.md)



<!--HONumber=Nov16_HO2-->


