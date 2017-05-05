---
title: "Implementar o PAM passo 2 – DC PRIV | Documentos da Microsoft"
description: "Prepare o controlador de domínio PRIV, que irá fornecer o ambiente de bastião onde está o Privileged Access Management está isolada."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 0e9993a0-b8ae-40e2-8228-040256adb7e2
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: edc15b41d4248887f4a93217f68d8125f6500585
ms.lasthandoff: 05/02/2017


---

# <a name="step-2---prepare-the-first-priv-domain-controller"></a>Passo 2 - Preparar o primeiro controlador de domínio PRIV

>[!div class="step-by-step"]
[« Passo 1](step-1-prepare-corp-domain.md)
[Passo 3 »](step-3-prepare-pam-server.md)

Neste passo, irá criar um novo domínio que vai fornecer o ambiente bastion para autenticação de administrador.  Esta floresta precisará de, pelo menos, um controlador de domínio e de, pelo menos, um servidor membro. O servidor membro será configurado no passo seguinte.

## <a name="create-a-new-privileged-access-management-domain-controller"></a>Criar um novo controlador de domínio do Privileged Access Management

Nesta secção, irá configurar uma máquina virtual para funcionar como um controlador de domínio para uma nova floresta

### <a name="install-windows-server-2012-r2"></a>Instalar o Windows Server 2012 R2
Noutra máquina virtual nova sem nenhum software instalado, instale o Windows Server 2012 R2 para tornar um computador “PRIVDC”.

1. Selecione executar uma instalação personalizada (não uma atualização) do Windows Server. Na instalação, especifique **Windows Server 2012 R2 Standard (Servidor com uma GUI) x64**; _não selecione_ **Data Center ou Server Core**.

2. Reveja e aceite os termos de licenciamento.

3. Uma vez que o disco estará vazio, selecione **Personalizar: instalar o Windows apenas** e utilize o espaço em disco não inicializado.

4. Depois de instalar a versão do sistema operativo, inicie sessão neste novo computador como o novo administrador. Utilize o Painel de Controlo para definir o nome do computador como *PRIVDC*, dê-lhe um endereço IP estático na rede virtual e configure o servidor DNS para ser o controlador de domínio instalado no passo anterior. Esta ação exige reiniciar o servidor.

5. Após o reinício do servidor, inicie sessão como administrador. Através do Painel de Controlo, configure o computador para verificar se existem atualizações e instale as atualizações necessárias. Pode ser preciso reiniciar o servidor.

### <a name="add-roles"></a>Adicionar funções
Adicione as funções de Serviços de Domínio do Active Directory (AD DS) e Servidor DNS.

1. Inicie o PowerShell como administrador.

2. Escreva os comandos seguintes para se preparar para uma instalação do Windows Server Active Directory.

  ```
  import-module ServerManager

  Install-WindowsFeature AD-Domain-Services,DNS –restart –IncludeAllSubFeature -IncludeManagementTools
  ```

### <a name="configure-registry-settings-for-sid-history-migration"></a>Configurar definições de registo para a migração do Histórico do SID

Inicie o PowerShell e escreva os comandos seguintes para configurar o domínio de origem para permitir o acesso de chamada de procedimento remoto (RPC) à base de dados do Gestor de contas de segurança (SAM).

```
New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1
```

## <a name="create-a-new-privileged-access-management-forest"></a>Criar uma nova floresta do Privileged Access Management

Em seguida, promova o servidor para ser um controlador de domínio de um uma nova floresta.

Neste documento, o nome priv.contoso.local é utilizado como o nome de domínio da nova floresta.  O nome da floresta não é crítico e não precisa de ser subordinado ao nome de uma floresta existente na organização. No entanto, tanto o nomes de domínio como de NetBIOS da nova floresta têm de ser exclusivos e distintos do nome de qualquer outro domínio na organização.  

### <a name="create-a-domain-and-forest"></a>Criar um domínio e uma floresta

1. Numa janela do PowerShell, escreva os comandos seguintes para criar o novo domínio.  Esta ação irá criar também uma delegação DNS num domínio superior (contoso.local) que foi criado num passo anterior.  Se quiser configurar o DNS mais tarde, omita os parâmetros `CreateDNSDelegation -DNSDelegationCredential $ca`.

  ```
  $ca= get-credential

  Install-ADDSForest –DomainMode 6 –ForestMode 6 –DomainName priv.contoso.local –DomainNetbiosName priv –Force –CreateDNSDelegation –DNSDelegationCredential $ca
  ```

2. Quando o pop-up aparecer, forneça as credenciais do administrador da floresta CORP (por exemplo, o nome de utilizador CONTOSO\\Administrador e a palavra-passe correspondente do passo 1).

3. A janela do PowerShell irá pedir uma Palavra-passe de Administrador de Modo de Segurança para utilizar. Introduza uma nova palavra-passe duas vezes. Irão aparecer mensagens de aviso relativas a definições de criptografia e delegação DNS. Estas mensagens são normais.

Quando a criação da floresta estiver concluída, o servidor irá reiniciar automaticamente.

### <a name="create-user-and-service-accounts"></a>Criar contas de utilizador e de serviço
Crie as contas de utilizador e de serviço para a configuração do Portal e do Serviço MIM. Estas contas ficarão no contentor de utilizadores do domínio priv.contoso.local.

1. Após o reinício do servidor, inicie sessão em PRIVDC como o administrador do domínio (PRIV\\Administrador).

2. Inicie o PowerShell e escreva os comandos seguintes. A palavra-passe 'Pass@word1' é apenas um exemplo e deve utilizar uma palavra-passe diferente para as contas.

  ```
  import-module activedirectory

  $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

  New-ADUser –SamAccountName MIMMA –name MIMMA

  Set-ADAccountPassword –identity MIMMA –NewPassword $sp

  Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMMonitor –name MIMMonitor -DisplayName MIMMonitor

  Set-ADAccountPassword –identity MIMMonitor –NewPassword $sp

  Set-ADUser –identity MIMMonitor –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMComponent –name MIMComponent -DisplayName MIMComponent

  Set-ADAccountPassword –identity MIMComponent –NewPassword $sp

  Set-ADUser –identity MIMComponent –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMSync –name MIMSync

  Set-ADAccountPassword –identity MIMSync –NewPassword $sp

  Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMService –name MIMService

  Set-ADAccountPassword –identity MIMService –NewPassword $sp

  Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SharePoint –name SharePoint

  Set-ADAccountPassword –identity SharePoint –NewPassword $sp

  Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SqlServer –name SqlServer

  Set-ADAccountPassword –identity SqlServer –NewPassword $sp

  Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName BackupAdmin –name BackupAdmin

  Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp

  Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1

  New-ADUser -SamAccountName MIMAdmin -name MIMAdmin

  Set-ADAccountPassword –identity MIMAdmin  -NewPassword $sp

  Set-ADUser -identity MIMAdmin -Enabled 1 -PasswordNeverExpires 1

  Add-ADGroupMember "Domain Admins" SharePoint

  Add-ADGroupMember "Domain Admins" MIMService
  ```

### <a name="configure-auditing-and-logon-rights"></a>Configurar os direitos de início de sessão e de auditoria

Tem de configurar a auditoria para que a configuração do PAM seja estabelecida entre florestas.  

1. Certifique-se de que tem sessão iniciada como administrador do domínio (PRIV\\Administrador).

2. Aceda a **Iniciar** > **Ferramentas administrativas** > **Gestão de Políticas de Grupo**.

3. Navegue para **Floresta: priv.contoso.local** > **Domínios** > **priv.contoso.local** > **Controladores de Domínio** > **Política de Controladores de Domínio Predefinida**. Será apresentada uma mensagem de aviso.

4. Clique com o botão direito do rato em **Política de Controladores de Domínio Predefinida** e selecione **Editar**.

5. Na árvore da consola do Editor de Gestão de Políticas de Grupo, navegue para **Configuração do Computador** > **Políticas** > **Definições do Windows** > **Definições de Segurança** > **Políticas locais** > **Política de auditoria**.

6. No painel de detalhes, clique com o botão direito do rato em **Gestão de contas de auditoria** e selecione **Propriedades**. Clique em **Definir estas definições de política**, marque a caixa de verificação **Êxito**, marque a caixa de verificação **Falha**, clique em **Aplicar** e, em seguida, em **OK**.

7. No painel Detalhes, clique com o botão direito do rato em **Auditar acesso ao serviço de diretórios** e selecione **Propriedades**. Clique em **Definir estas definições de política**, marque a caixa de verificação **Êxito**, marque a caixa de verificação **Falha**, clique em **Aplicar** e, em seguida, em **OK**.

8. Navegue para **Configuração do Computador** > **Políticas** > **Definições do Windows** > **Definições de Segurança** > **Políticas de conta** > **Política Kerberos**.

9. No painel Detalhes, com o botão direito do rato em **Duração máxima para autorização de utilizador** e selecione **Propriedades**. Clique em **Definir estas definições de política**, defina o número de horas como *1*, clique em **Aplicar** e, em seguida, em **OK**. Tenha em atenção que as outras definições na janela também serão alteradas.

10. Na janela Gestão de Políticas de Grupo, selecione **Política de Domínio Predefinida**, clique com o botão direito do rato e selecione **Editar**.

11. Expanda **Configuração do Computador** > **Políticas** > **Definições do Windows** > **Definições de Segurança** > **Políticas locais** e selecione **Atribuição de direitos de utilizadores**.

12. No painel Detalhes, clique com o botão direito do rato em **Negar início de sessão como uma tarefa batch** e selecione **Propriedades**.

13. Marque a caixa de verificação **Definir estas definições de política**, clique em **Adicionar utilizador ou grupo** e, no campo Nomes de utilizador ou grupo, escreva *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* e clique em **OK**.

14. Clique em **OK** para fechar a janela.

15. No painel Detalhes, clique com o botão direito do rato em **Negar início de sessão através dos Serviços de Ambiente de Trabalho Remoto** e selecione **Propriedades**.

16. Clique na caixa de verificação **Definir estas definições de política**, clique em **Adicionar utilizador ou grupo** e, no campo Nomes de utilizador ou grupo, escreva *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* e clique em **OK**.

17. Clique em **OK** para fechar a janela.

18. Feche a janela Editor de Gestão de Políticas de Grupo e a janela Gestão de Políticas de Grupo.

19. Inicie uma janela do PowerShell como administrador e escreva o seguinte comando para atualizar o DC com as definições da política de grupos.

  ```
  gpupdate /force /target:computer
  ```

  Passado um minuto, a atualização será concluída com a mensagem “A atualização da Política de Computador foi concluída com êxito”.


### <a name="configure-dns-name-forwarding-on-privdc"></a>Configurar o reencaminhamento do nome DNS no PRIVDC

Com o PowerShell no PRIVDC, configure o reencaminhamento do nome DNS para que o domínio PRIV reconheça outras florestas existentes.

1. Inicie o PowerShell.

2. Para cada domínio no topo de cada floresta existente, escreva o comando seguinte, especificando o domínio DNS existente (por ex., contoso.local) e o endereço IP do servidor mestre desse domínio.  

  Se tiver criado um domínio contoso.local no passo anterior, especifique *10.1.1.31* para o endereço IP da rede virtual do computador CORPDC.

  ```
  Add-DnsServerConditionalForwarderZone –name "contoso.local" –masterservers 10.1.1.31
  ```

> [!NOTE]
> As outras florestas também têm de estar preparadas para encaminhar consultas DNS da floresta PRIV para este controlador de domínio.  Se tiver várias florestas do Active Directory existentes, tem também de adicionar um reencaminhador condicional de DNS a cada uma dessas florestas.

### <a name="configure-kerberos"></a>Configurar o Kerberos

1. Com o PowerShell, adicione SPNs para que o SharePoint, a API REST do PAM e o Serviço MIM possam utilizar a autenticação Kerberos.

  ```
  setspn -S http/pamsrv.priv.contoso.local PRIV\SharePoint
  setspn -S http/pamsrv PRIV\SharePoint
  setspn -S FIMService/pamsrv.priv.contoso.local PRIV\MIMService
  setspn -S FIMService/pamsrv PRIV\MIMService
  ```

> [!NOTE]
> Os passos seguintes deste documento descrevem como instalar os componentes do servidor MIM 2016 num único computador. Se planear adicionar outro servidor para elevada disponibilidade, precisará de configuração de Kerberos adicional, conforme descrito em [FIM 2010: Kerberos Authentication Setup (FIM 2010: Configuração de Autenticação Kerberos)](http://social.technet.microsoft.com/wiki/contents/articles/3385.fim-2010-kerberos-authentication-setup.aspx).

### <a name="configure-delegation-to-give-mim-service-accounts-access"></a>Configurar a delegação para dar acesso a contas de serviço MIM

Execute os passos seguintes no PRIVDC como administrador do domínio.

1. Inicie **Utilizadores e Computadores do Active Directory**.  
2. Clique com o botão direito do rato no domínio **priv.contoso.local** e selecione **Delegar Controlo**.  
3. No separador Utilizadores e Grupos Selecionados, clique em **Adicionar**.  
4. Na janela Selecionar Utilizadores, Computadores ou Grupos, escreva *mimcomponent; mimmonitor; mimservice* e clique em **Verificar Nomes**. Depois de os nomes estarem sublinhados, clique em **OK** e em **seguinte**.  
5. Na lista de tarefas comuns, selecione **Criar, eliminar e gerir contas de utilizador** e **Modificar a associação de um grupo**, clique em **Seguinte** e em **Concluir**.

6. Clique novamente com o botão direito do rato no domínio **priv.contoso.local** e selecione **Delegar Controlo**.  
7. No separador Utilizadores e Grupos Selecionados, clique em **Adicionar**.  
8. Na janela Selecionar Utilizadores, Computadores ou Grupos, introduza *MIMAdmin* e clique em **Verificar Nomes**. Depois de os nomes estarem sublinhados, clique em **OK** e em **seguinte**.  
9. Selecione **tarefa personalizada**, aplique a **Esta pasta**, com **Permissões gerais**.    
10. Na lista de permissões, selecione o seguinte:  
  - **Ler**  
  - **Escrever**  
  - **Criar todos os Objetos Subordinados**  
  - **Eliminar todos os Objetos Subordinados**  
  - **Ler Todas as Propriedades**  
  - **Escrever Todas as Propriedades**  
  - **Migrar Histórico do SID**  
  Clique em **Seguinte** e em **Concluir**.

11. Mais uma vez, clique com o botão direito do rato no domínio **priv.contoso.local** e selecione **Delegar Controlo**.  
12. No separador Utilizadores e Grupos Selecionados, clique em **Adicionar**.  
13. Na janela Selecionar Utilizadores, Computadores ou Grupos, introduza *MIMAdmin* e clique em **Verificar Nomes**. Depois de os nomes estarem sublinhados, clique em **OK** e em **seguinte**.  
14. Selecione **tarefa personalizada**, aplique a **Esta pasta** e clique em **Apenas objetos de utilizador**.    
15. Na lista de permissões, selecione **Alterar palavra-passe** e **Repor palavra-passe**. Em seguida, clique em **Seguinte** e em **Concluir**.  
16. Feche Computadores e Utilizadores do Active Directory.

17.    Abra uma linha de comandos.  
18.    Reveja a lista de controlo de acesso do objeto Titular de SD Admin nos domínios PRIV. Por exemplo, se o seu domínio era "priv.contoso.local", escreva o comando  
  ```
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local"
  ```
19.    Atualize a lista de controlo de acesso conforme necessário para garantir que o serviço MIM e o serviço de componentes MIM consegue atualizar as associações de grupos protegidos por esta ACL.  Escreva o comando:  
  ```
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimservice:WP;"member"  
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimcomponent:WP;"member"
  ```
20. Reinicie o servidor PRIVDC para que estas alterações sejam aplicadas.

## <a name="prepare-a-priv-workstation"></a>Preparar uma estação de trabalho PRIV

Se ainda não tiver um computador de estação de trabalho que irá ser associado ao domínio PRIV para fazer a manutenção de recursos PRIV (como MIM), siga estas instruções para preparar uma estação de trabalho.  

### <a name="install-windows-81-or-windows-10-enterprise"></a>Instalar o Windows 8.1 ou o Windows 10 Enterprise

Noutra máquina virtual nova sem nenhum software instalado, instale o Windows 8.1 Enterprise ou o Windows 10 Enterprise para tornar um computador *“PRIVWKSTN”*.

1. Utilize as Definições rápidas durante a instalação.

2. Tenha em atenção que a instalação poderá não conseguir estabelecer ligação à Internet. Clique em **Criar uma conta local**. Especifique um nome de utilizador diferente; não utilize "Administrador" ou "Joana".

3. Com o Painel de Controlo, atribua um endereço IP estático a este computador na rede virtual e defina o servidor DNS preferencial da interface para ser o do servidor PRIVDC.

4. Com o Painel de Controlo, o domínio associa o computador PRIVWKSTN ao domínio priv.contoso.local. Será preciso fornecer as credenciais do administrador do domínio PRIV. Quando estiver concluído, reinicie o computador PRIVWKSTN.

Se quiser mais detalhes, veja [proteger estações de trabalho com acesso privilegiado](https://technet.microsoft.com/en-us/library/mt634654.aspx).

No próximo passo, irá preparar um servidor de PAM.

>[!div class="step-by-step"]
[« Passo 1](step-1-prepare-corp-domain.md)
[Passo 3 »](step-3-prepare-pam-server.md)

