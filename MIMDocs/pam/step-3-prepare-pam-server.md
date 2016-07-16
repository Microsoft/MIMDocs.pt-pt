---
title: "Passo 3 – Preparar um servidor de PAM | Microsoft Identity Manager"
description: 
keywords: 
author: 
manager: femila
ms.date: 06/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 68ec2145-6faa-485e-b79f-2b0c4ce9eff7
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 9e5f51d5ca731b3564b8262db0f4cddeb850231a
ms.openlocfilehash: ec65078cea33b73aa9482e831a1870df477c6581


---

# Passo 3 – Preparar um servidor de PAM

>[!div class="step-by-step"]
[« Passo 2](step-2-prepare-priv-domain-controller.md)
[Passo 4 »](step-4-install-mim-components-on-pam-server.md)

## Instalar o Windows Server 2012 R2
Numa terceira máquina virtual, instale o Windows Server 2012 R2, especificamente o Windows Server 2012 R2 Standard (Servidor com uma GUI) x64, para criar *PAMSRV*. Uma vez que o SQL Server e o SharePoint 2013 serão instalados neste computador, são necessários, pelo menos, 8 GB de RAM.

1. Selecione **Windows Server 2012 R2 Standard (Servidor com uma GUI) x64**.

    ![Escolha Windows Server Standard com GUI - captura de ecrã](media/PAM_GS_Select_WS2012.png)

2. Reveja e aceite os termos de licenciamento.

3.  Uma vez que o disco estará vazio, selecione **Personalizar: instalar apenas o Windows** e utilizar o **espaço em disco não inicializado**.

4.  Inicie sessão no novo computador como administrador.  Utilize o Painel de Controlo, forneça um endereço IP estático na rede virtual, configure essa interface de rede para enviar consultas DNS para o endereço IP de PRIVDC e defina o nome do computador como *PAMSRV*.  Esta ação exige reiniciar o servidor.

5.  Se a rede virtual não fornecer conectividade à Internet, adicione uma interface de rede adicional ao computador que forneça uma ligação à Internet.  Será necessário para a instalação do SharePoint e pode ser desativado após este passo estar concluído.

6.  Após o reinício do servidor, inicie sessão como administrador. Através do Painel de Controlo, configure o computador para verificar se existem atualizações e instale as atualizações necessárias.  Pode ser necessário reiniciar o servidor.

7.  Depois de o servidor ser reiniciado, inicie sessão como Administrador, abra o Painel de Controlo e associe PAMSRV ao domínio PRIV (priv.contoso.local).  Será necessário fornecer o nome de utilizador e as credenciais de um administrador do domínio PRIV (PRIV\Administrador). Depois de aparecer a mensagem de boas-vindas, feche a caixa de diálogo e reinicie este servidor.


### Adicione o servidor Web (IIS) e as funções de servidor aplicacional
Adicione as funções de Servidor Web (IIS) e Servidor Aplicacional, as funcionalidades do .NET Framework 3.5 e o módulo Active Directory para Windows PowerShell, bem como outras funcionalidades exigidas pelo SharePoint

1.  Inicie sessão como um administrador de domínio PRIV (PRIV\Administrator) e inicie o PowerShell.

2.  Escreva os seguintes comandos. Tenha em atenção que poderá ser necessário especificar uma localização diferente para os ficheiros de origem das funcionalidades do .NET Framework 3.5. Normalmente, estas funcionalidades não estão presentes quando o Windows Server é instalado, mas estão disponíveis na pasta lado a lado (SxS) na pasta de origens do disco de instalação do SO, por exemplo, d:\Sources\SxS\.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,
    rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,
    Windows-Identity-Foundation,Server-Media-Foundation,
    Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

### Configurar a política de segurança do servidor
Configure a política de segurança do servidor para permitir que as contas recentemente criadas sejam executadas como serviços.

1.  Iniciar o programa **Política de Segurança Local**.   
2.  Navegue até **Políticas Locais** > **Atribuição de Direitos de Utilizadores**.  
3.  No painel de detalhes, clique com o botão direito do rato em **Iniciar sessão como um serviço** e selecione **Propriedades**.  
4.  Clique em **Adicionar Utilizador ou Grupo** e, nos nomes de utilizadores e grupos, escreva *priv\mimmonitor; priv\MIMService; priv\SharePoint; priv\mimcomponent; priv\SqlServer*. Clique em **Verificar Nomes** e clique em **OK**.  

5.  Clique em **OK** para fechar a janela Propriedades.  
6.  No painel de detalhes, clique com o botão direito do rato em **Negar acesso a este computador a partir da rede** e selecione **Propriedades**.  
7.  Clique em **Adicionar Utilizador ou Grupo** e, nos nomes de utilizadores e grupos, escreva *priv\mimmonitor; priv\MIMService; priv\mimcomponent* e clique em **OK**.  
8.  Clique em **OK** para fechar a janela Propriedades.  

9. No painel de detalhes, clique com o botão direito do rato em **Recusar início de sessão local** e selecione **Propriedades**.  
10. Clique em **Adicionar Utilizador ou Grupo** e, nos nomes de utilizadores e grupos, escreva *priv\mimmonitor; priv\MIMService; priv\mimcomponent* e clique em **OK**.  
11. Clique em **OK** para fechar a janela Propriedades.  
12. Feche a janela Política de Segurança Local.  

13. Abra o Painel de Controlo e mude para **Contas de Utilizador**.  
14. Clique em **Atribuir a outras pessoas acesso a este computador**.  
15. Clique em **Adicionar**, introduza o utilizador *MIMADMIN* no domínio *PRIV* e, no ecrã seguinte do assistente, clique em **Adicionar este utilizador como Administrador**.  
16. Clique em **Adicionar**, introduza o utilizador *SharePoint* no domínio *PRIV* e, no ecrã seguinte do assistente, clique em **Adicionar este utilizador como Administrador**.  
17. Feche o Painel de Controlo.  

### Alterar a configuração do IIS
Existem duas formas de alterar a configuração do IIS para permitir que as aplicações utilizem o modo de Autenticação do Windows. Certifique-se de que tem sessão iniciada como MIMAdmin e, em seguida, siga uma destas opções.

Se pretender utilizar o PowerShell:
1.  Clique com o botão direito do rato no PowerShell e selecione **Executar como administrador**.  
2.  Pare o IIS e desbloqueie as definições de anfitrião das aplicações utilizando estes comandos  
    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```  

Se pretender utilizar um editor de texto, como o Bloco de Notas::   
1. Abra o ficheiro **C:\Windows\System32\inetsrv\config\applicationHost.config**   
2. Desloque para baixo para a linha 82 desse ficheiro. O valor de etiqueta de **overrideModeDefault** deve ser **<section name="windowsAuthentication" overrideModeDefault="Deny" />**  
3. Altere o valor de **overrideModeDefault** para *Permitir*  
4. Guarde o ficheiro e reinicie o IIS com o comando do PowerShell `iisreset /START`

## Instalar o SQL Server
Se o SQL Server ainda não estiver no ambiente bastion, instale o SQL Server 2012 (Service Pack 1 ou posterior) ou o SQL Server 2014. Os passos seguintes assumem o SQL 2014.

1. Certifique-se de que tem sessão iniciada como MIMAdmin.
2. Clique com o botão direito do rato no PowerShell e selecione **Executar como administrador**.   
3. Navegue para o diretório onde está localizado o programa de configuração do SQL Server.  
4. Escreva o seguinte comando.  
    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="PRIV\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="PRIV\MIMAdmin"
    ```

## Instalar o SharePoint Foundation 2013

Utilizando o SharePoint Foundation 2013 com o instalador SP1, instale os pré-requisitos de software do SharePoint no PAMSRV.

> [!NOTE] 
> Este instalador requer uma ligação à Internet para transferir os pré-requisitos. Depois de serem instalados, o servidor será reiniciado.

1. Clique com o botão direito do rato no PowerShell e selecione **Executar como administrador**.  
2. Mude para o diretório onde o SharePoint foi descompactado.  
3. Escreva o comando `.\prerequisiteinstaller.exe`.

Depois de instalar os pré-requisitos do SharePoint, instale o SharePoint Foundation 2013 com SP1.

1.  Clique com o botão direito do rato no PowerShell e selecione **Executar como administrador**.  
2.  Mude para o diretório onde o SharePoint foi descompactado.  
3.  Escreva o comando `.\setup.exe`.  
4.  Selecione o tipo de **servidor completo**.  
5.  Depois de concluída a instalação, selecione para executar o assistente.  

### Configurar o Sharepoint
Execute o Assistente de Configuração de Produtos SharePoint para configurar o SharePoint.

1.  No separador Ligar a um Farm de Servidores, mude para **Criar um novo farm de servidores**.  
2.  Especifique **PAMSRV** como o servidor da base de dados de configuração e **PRIV\SharePoint** como a conta de acesso à base de dados para o SharePoint utilizar.  
3.  Especifique uma palavra-passe como frase de acesso de segurança do farm (que não será utilizada posteriormente nestas instruções).  
4.  Por agora, aceite as restantes predefinições do assistente de configuração do SharePoint para tornar um farm de servidor único.    
5.  Quando o assistente de configuração concluir a tarefa de configuração 10 de 10, clique em **Concluir** e um browser será aberto.  
6.  No pop-up do Internet Explorer, autentique-se como administrador de domínio (PRIV\MIMAdmin) para continuar.  
7.  Inicie o assistente na aplicação Web para configurar o farm do SharePoint.  
8.  Selecione para utilizar a conta gerida existente (PRIV\SharePoint), desmarque para desativar quaisquer serviços opcionais e clique em **Seguinte**.  
9. Depois de ser apresentada a janela Criar uma Coleção de Sites, clique em **Ignorar** e, em seguida, em **Concluir**.  

## Criar uma aplicação Web do SharePoint Foundation 2013
Após a conclusão dos assistentes, utilize o PowerShell para criar uma aplicação Web do SharePoint Foundation 2013 para alojar o Portal do MIM. Uma vez que estas instruções se destinam a fins de demonstração, o SSL não será ativado.

1.  Clique com o botão direito do rato na Shell de Gestão do SharePoint 2013, selecione **Executar como administrador** e execute o seguinte script do PowerShell:

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity PRIV\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"            -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://PAMSRV.priv.contoso.local
    ```

2. Será apresentada uma mensagem de aviso a indicar que está a ser utilizado o método de autenticação Clássico do Windows e o comando final poderá demorar alguns minutos a responder.  Quando concluir, a saída indicará o URL do novo portal.

> [!NOTE] 
> Mantenha a janela da Shell de Gestão do SharePoint 2013 aberta para utilizá-la no passo seguinte.

## Criar uma coleção de sites do Sharepoint
Em seguida, crie uma Coleção de Sites do SharePoint associada a essa aplicação Web para alojar o Portal do MIM.

1.  Inicie a **Shell de Gestão do SharePoint 2013**, se ainda não estiver aberta, e execute o seguinte script do PowerShell

    ```
    $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
    $w = Get-SPWebApplication http://pamsrv.priv.contoso.local:82
    New-SPSite -Url $w.Url -Template $t -OwnerAlias PRIV\MIMAdmin                -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias PRIV\BackupAdmin
    $s = SpSite($w.Url)
    $s.AllowSelfServiceUpgrade = $false
    $s.CompatibilityLevel
    ```

    Certifique-se de que variável **CompatibilityLevel** está definida como *14*. Se devolver *15*, a coleção de sites não foi criada para a versão da experiência de 2010; elimine a coleção de sites e reformule-a.

2.  Execute os seguintes comandos do PowerShell na **Shell de Gestão do SharePoint 2013**. Isto desativará o viewstate do lado do servidor do SharePoint e a tarefa do SharePoint **Tarefa de Análise do Estado de Funcionamento (Hora a Hora, Temporizador do Microsoft SharePoint Foundation, Todos os Servidores)**.

    ```
    $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
    $contentService.ViewStateOnServer = $false;
    $contentService.Update();
    Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
    ```

## Alterar as definições de atualização

1. Abra o Painel de Controlo, navegue para **Windows Update** e clique para **alterar as definições**.  
2. Altere as definições para receber atualizações do Windows Update e de outros produtos do Microsoft Update.  
3. Verifique a existência de novas atualizações e certifique-se de que todas as atualizações importantes pendentes são instaladas antes de continuar.

## Definir o site como a intranet local

1. Inicie o Internet Explorer e abra um novo separador do browser
2. Navegue para http://pamsrv.priv.contoso.local:82/ e inicie sessão como PRIV\MIMAdmin.  Será apresentado um site do SharePoint vazio denominado ’’Portal do MIM’’.  
3. No Internet Explorer, abra as **Opções da Internet**, mude para o separador **Segurança**, selecione **Intranet local** e adicione o URL `http://pamsrv.priv.contoso.local:82/`.

Se o início de sessão falhar, os SPNs Kerberos criados anteriormente no [Passo 2](step-2-prepare-priv-domain-controller.md) podem ter de ser atualizados.

## Iniciar o serviço de administração do SharePoint

Utilizando os **Serviços** (localizado em Ferramentas Administrativas), inicie o serviço **Administração do SharePoint**, se ainda não estiver em execução.

No Passo 4, começará a instalar os componentes do MIM no servidor de PAM.

>[!div class="step-by-step"]
[« Passo 2](step-2-prepare-priv-domain-controller.md)
[Passo 4 »](step-4-install-mim-components-on-pam-server.md)



<!--HONumber=Jun16_HO5-->


