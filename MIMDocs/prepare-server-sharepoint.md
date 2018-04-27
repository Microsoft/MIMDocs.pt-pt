---
title: Configurar o SharePoint para o Microsoft Identity Manager 2016 | Documentos da Microsoft
description: Instalar e configurar o SharePoint Foundation para que possa alojar a página do Portal do MIM.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 04/26/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: eceb1ed31b0212970d5cf0eae0bc8d96aa087ff5
ms.sourcegitcommit: 32d9a963a4487a8649210745c97a3254645e8744
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/27/2018
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>Configurar um servidor de gestão de identidades: SharePoint

>[!div class="step-by-step"]
[«SQL Server 2016](prepare-server-sql2016.md)
[do Exchange Server»](prepare-server-exchange.md)

> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio - **corpdc**
> - Nome de domínio – **contoso**
> - Nome do servidor do serviço MIM - **corpservice**
> - Nome do servidor de sincronização de MIM - **corpsync**
> - Nome do SQL Server - **corpsql**
> - Palavra-passe – **Pass@word1**


## <a name="install-sharepoint-2016"></a>Instalar **SharePoint 2016**

> [!NOTE]
> O instalador requer uma ligação à Internet para transferir os respetivos pré-requisitos. Se o computador estiver numa rede virtual que não fornece conetividade à Internet, adicione uma interface de rede adicional ao computador que forneça uma ligação à Internet. Isto pode ser desativado depois de concluída a instalação.

Siga estes passos para instalar o SharePoint 2016. Depois de concluir a instalação, o servidor será reiniciado.

1.  Iniciar **PowerShell** como uma conta de domínio com um administrador local no **corpservice** e **sysadmin** no servidor de base de dados do SQL Server que iremos utilizar saída **contoso \ miminstall**.

    -   Mude para o diretório onde o SharePoint foi descompactado.

    -   Escreva o seguinte comando.

        ```
        .\prerequisiteinstaller.exe
        ```

2.  Depois de **SharePoint** pré-requisitos estão instalados, instale **SharePoint 2016** , escrevendo o seguinte comando:

    ```
    .\setup.exe
    ```

3.  Selecione o tipo de servidor completo.

4.  Depois de concluída a instalação, execute o assistente.

## <a name="run-the-wizard-to-configure-sharepoint"></a>Executar o assistente para configurar o SharePoint

Siga os passos delineados no **Assistente de Configuração de Produtos SharePoint** para configurar o SharePoint para funcionar com o MIM.

1. No separador **Ligar a um farm de servidores**, mude para criar um novo farm de servidores.

2. Especifique este servidor como o servidor de base de dados como **corpsql** para a base de dados de configuração e *Contoso\SharePoint* como a conta de acesso à base de dados para o SharePoint utilizar.
    a. O Assistente de configuração Recomendamos que selecione [MinRole](https://docs.microsoft.com/en-us/sharepoint/install/overview-of-minrole-server-roles-in-sharepoint-server-2016) tipo de **front-end**
3. Crie uma palavra-passe para a frase de acesso de segurança do farm.

4. Quando o assistente de configuração concluir a tarefa de configuração 10 de 10, clique em Concluir e um browser será aberto.

5. No pop-up do Internet Explorer, efetue a autenticação como *Contoso\miminstall* (ou a conta de administrador equivalentes) para continuar.

6. No Assistente de web (na aplicação web) clique em **Cancelar/Skip**.


## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>Preparar o SharePoint para alojar o Portal do MIM

> [!NOTE]
> Inicialmente, o SSL não será configurado. Certifique-se de que configura o SSL ou equivalente antes de ativar o acesso a este portal.

1. Iniciar **Shell de gestão do SharePoint 2016** e execute o seguinte script do PowerShell para criar um **aplicação Web do SharePoint 2016**.

    ```
    New-SPManagedAccount ##Will prompt for new account enter contoso\mimpool 
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\mimpool
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 80 -URL http://mim.contoso.com
    ```

    > [!NOTE]
    > Será apresentada uma mensagem de aviso a indicar que está a ser utilizado o método de autenticação Clássico do Windows e o comando final poderá demorar alguns minutos a responder. Quando concluir, a saída indicará o URL do novo portal. Manter o **Shell de gestão do SharePoint 2016** janela aberta para consultar mais tarde.

2. Inicie o Shell de Gestão do SharePoint 2013 e execute o seguinte script do PowerShell para criar uma **Coleção de Sites do SharePoint** associada a essa aplicação Web.

  ```
    $t = Get-SPWebTemplate -compatibilityLevel 15 -Identity "STS#1"
    $w = Get-SPWebApplication http://mim.contoso.com/
    New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\miminstall -CompatibilityLevel 15 -Name "MIM Portal"
    $s = SpSite($w.Url)
    $s.AllowSelfServiceUpgrade = $false
    $s.CompatibilityLevel
  ```

  > [!NOTE]
  > Certifique-se de que o resultado do *CompatibilityLevel* variável for "15". Se o resultado é diferente de "15", em seguida, a coleção de sites não foi criada a versão correta experiência; Elimine a coleção de sites e recriá-lo.

3. Desativar **Viewstate do lado do servidor do SharePoint** e a tarefa do SharePoint "Estado de funcionamento tarefa de análise (hora a hora, temporizador do Microsoft SharePoint Foundation, todos os servidores)" ao executar o PowerShell seguinte comandos no  **Shell de gestão do SharePoint 2016**:

  ```
  $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
  $contentService.ViewStateOnServer = $false;
  $contentService.Update();
  Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
  ```

4. No servidor de gestão de identidades, abra um novo separador do browser, navegue para http://mim.contoso.com/ e inicie sessão como *contoso\miminstall*.  Será apresentado um site do SharePoint vazio denominado *Portal do MIM*.

    ![Portal de MIM em http://mim.contoso.com/ imagem](media/MIM-DeploySP1.png)

5. Copie o URL, em seguida, no Internet Explorer, abra as **Opções da Internet**, mude para o **separador Segurança**, selecione **Intranet local** e clique em **Sites**.

    ![Imagem das Opções da Internet](media/MIM-DeploySP2.png)

6. Na janela **Intranet local**, clique em **Avançado** e cole o URL copiado na caixa de texto **Adicionar este Web site à zona**. Clique em **Adicionar** e feche as janelas.

7. Abra o programa **Ferramentas Administrativas**, navegue até **Serviços**, localize o serviço de Administração do SharePoint e inicie-o se ainda não estiver em execução.

>[!div class="step-by-step"]  
[«SQL Server 2016](prepare-server-sql2016.md)
[do Exchange Server»](prepare-server-exchange.md)
