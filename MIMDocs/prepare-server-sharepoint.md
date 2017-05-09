---
title: Configurar o SharePoint para o Microsoft Identity Manager 2016 | Documentos da Microsoft
description: "Instalar e configurar o SharePoint Foundation para que possa alojar a página do Portal do MIM."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: 2af432036033f8914d00228cd3d2d1af84f13054
ms.lasthandoff: 01/24/2017


---

# <a name="set-up-an-identity-management-server-sharepoint"></a>Configurar um servidor de gestão de identidades: SharePoint

>[!div class="step-by-step"]
[« SQL Server 2014](prepare-server-sql2014.md)
[ Exchange Server »](prepare-server-exchange.md)

> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio – **nomedoservidormim**
> - Nome de domínio – **contoso**
> - Palavra-passe – **Pass@word1**


## <a name="install-sharepoint-foundation-2013-with-sp1"></a>Instalar o **SharePoint Foundation 2013 com SP1**

> [!NOTE]
> O instalador requer uma ligação à Internet para transferir os respetivos pré-requisitos. Se o computador estiver numa rede virtual que não fornece conetividade à Internet, adicione uma interface de rede adicional ao computador que forneça uma ligação à Internet. Isto pode ser desativado depois de concluída a instalação.

Siga estes passos para instalar o SharePoint Foundation 2013 SP1. Depois de concluir a instalação, o servidor será reiniciado.

1.  Inicie o **PowerShell** como um administrador do domínio.

    -   Mude para o diretório onde o SharePoint foi descompactado.

    -   Escreva o seguinte comando.

        ```
        .\prerequisiteinstaller.exe
        ```

2.  Depois de instalar os pré-requisitos do **SharePoint**, instale o **SharePoint Foundation 2013 com SP1** ao escrever o seguinte comando:

    ```
    .\setup.exe
    ```

3.  Selecione o tipo de servidor completo.

4.  Depois de concluída a instalação, execute o assistente.

## <a name="run-the-wizard-to-configure-sharepoint"></a>Executar o assistente para configurar o SharePoint

Siga os passos delineados no **Assistente de Configuração de Produtos SharePoint** para configurar o SharePoint para funcionar com o MIM.

1. No separador **Ligar a um farm de servidores**, mude para criar um novo farm de servidores.

2. Especifique este servidor como o servidor da base de dados de configuração e *Contoso\SharePoint* como a conta de acesso à base de dados para o SharePoint utilizar.

3. Crie uma palavra-passe para a frase de acesso de segurança do farm.

4. Quando o assistente de configuração concluir a tarefa de configuração 10 de 10, clique em Concluir e um browser será aberto.

5. Na janela de pop-up do Internet Explorer, efetue a autenticação como *Contoso\Administrador* (ou a conta de administrador do domínio equivalente) para continuar.

6. Inicie o assistente (na aplicação Web) para configurar o farm do SharePoint.

7. Selecione a opção para utilizar a conta gerida existente (*Contoso\SharePoint*) e clique em **Seguinte**.

8. Na janela **Criar uma Coleção de Sites**, clique em **Ignorar**.  Em seguida, clique em **Concluir**.

## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>Preparar o SharePoint para alojar o Portal do MIM

> [!NOTE]
> Inicialmente, o SSL não será configurado. Certifique-se de que configura o SSL ou equivalente antes de ativar o acesso a este portal.

1. Inicie o **Shell de Gestão do SharePoint 2013** e execute o seguinte script do PowerShell para criar uma **Aplicação Web do SharePoint Foundation 2013**.

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://corpidm.contoso.local
    ```

    > [!NOTE]
    > Será apresentada uma mensagem de aviso a indicar que está a ser utilizado o método de autenticação Clássico do Windows e o comando final poderá demorar alguns minutos a responder. Quando concluir, a saída indicará o URL do novo portal. Mantenha a janela do **Shell de Gestão do SharePoint 2013** aberta para consultar mais tarde.

2. Inicie o Shell de Gestão do SharePoint 2013 e execute o seguinte script do PowerShell para criar uma **Coleção de Sites do SharePoint** associada a essa aplicação Web.

  ```
  $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
  $w = Get-SPWebApplication http://corpidm.contoso.local:82
  New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\Administrator
  -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias contoso\BackupAdmin
  $s = SpSite($w.Url)
  $s.AllowSelfServiceUpgrade = $false
  $s.CompatibilityLevel
  ```

  > [!NOTE]
  > Verifique se o resultado da variável *NívelDeCompatibilidade* é “14”. Se o resultado for “15”, significa que a coleção de sites não foi criada para a versão da experiência de 2010; elimine a coleção de sites e reformule-a.

3. Desative o **Viewstate do Lado do Servidor do SharePoint** e a tarefa do SharePoint “Tarefa de Análise do Estado de Funcionamento (Hora a Hora, Temporizador do Microsoft SharePoint Foundation, Todos os Servidores)” ao executar os seguintes comandos do PowerShell no **Shell de Gestão do SharePoint 2013**:

  ```
  $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
  $contentService.ViewStateOnServer = $false;
  $contentService.Update();
  Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
  ```

4. No servidor de gestão de identidades, abra um novo separador do browser, navegue para http://localhost:82/ e inicie sessão como *contoso\Administrador*.  Será apresentado um site do SharePoint vazio denominado *Portal do MIM*.

    ![Imagem do Portal do MIM em http://localhost:82/](media/MIM-DeploySP1.png)

5. Copie o URL, em seguida, no Internet Explorer, abra as **Opções da Internet**, mude para o **separador Segurança**, selecione **Intranet local** e clique em **Sites**.

    ![Imagem das Opções da Internet](media/MIM-DeploySP2.png)

6. Na janela **Intranet local**, clique em **Avançado** e cole o URL copiado na caixa de texto **Adicionar este Web site à zona**. Clique em **Adicionar** e feche as janelas.

7. Abra o programa **Ferramentas Administrativas**, navegue até **Serviços**, localize o serviço de Administração do SharePoint e inicie-o se ainda não estiver em execução.

>[!div class="step-by-step"]  
[« SQL Server 2014](prepare-server-sql2014.md)
[ Exchange Server »](prepare-server-exchange.md)

