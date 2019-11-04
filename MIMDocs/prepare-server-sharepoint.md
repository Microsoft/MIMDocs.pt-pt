---
title: Configurar o SharePoint para o Microsoft Identity Manager 2016 | Documentos da Microsoft
description: Instalar e configurar o SharePoint Foundation para que possa alojar a página do Portal do MIM.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 46320c8c2d1ae7c530c4670159e393ee1be7165c
ms.sourcegitcommit: b09a8c93983d9d92ca4871054650b994e9996ecf
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2019
ms.locfileid: "73329457"
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>Configurar um servidor de gestão de identidades: SharePoint

> [!div class="step-by-step"]
> [«SQL Server](prepare-server-sql2016.md)
> [Exchange Server»](prepare-server-exchange.md)
> 

> [!NOTE]
O procedimento de instalação do SharePoint Server 2019 não difere do procedimento de instalação do SharePoint Server 2016 **, exceto** uma etapa extra que deve ser executada para desbloquear os arquivos ASHX usados pelo portal do mim.

> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio- **corpdc**
> - Nome de domínio – **contoso**
> - Nome do servidor do serviço do MIM- **corpservice**
> - Nome do servidor de sincronização do MIM- **corpsync**
> - Nome do SQL Server- **corpsql**
> - Palavra-passe – <strong>Pass@word1</strong>


## <a name="install-sharepoint-2016"></a>Instalar o **SharePoint 2016**

> [!NOTE]
> O instalador requer uma ligação à Internet para transferir os respetivos pré-requisitos. Se o computador estiver numa rede virtual que não fornece conetividade à Internet, adicione uma interface de rede adicional ao computador que forneça uma ligação à Internet. Isto pode ser desativado depois de concluída a instalação.

Siga estas etapas para instalar o SharePoint 2016. Depois de concluir a instalação, o servidor será reiniciado.

1.  Inicie o **PowerShell** como uma conta de domínio com administrador local no **corpservice** e **sysadmin** no servidor de banco de dados SQL que usaremos **contoso\miminstall**.

    -   Mude para o diretório onde o SharePoint foi descompactado.

    -   Escreva o seguinte comando.
    ```CMD
    .\prerequisiteinstaller.exe
    ```

2.  Depois que os pré-requisitos **do SharePoint** forem instalados, instale o **SharePoint 2016** digitando o seguinte comando:

    ```CMD
    .\setup.exe
    ```

3.  Selecione o tipo de servidor completo.

4.  Depois de concluída a instalação, execute o assistente.

## <a name="run-the-wizard-to-configure-sharepoint"></a>Executar o assistente para configurar o SharePoint

Siga os passos delineados no **Assistente de Configuração de Produtos SharePoint** para configurar o SharePoint para funcionar com o MIM.

1. No separador **Ligar a um farm de servidores**, mude para criar um novo farm de servidores.

2. Especifique esse servidor como o servidor de banco de dados como **corpsql** para o banco de dados de configuração e *Contoso\SharePoint* como a conta de acesso ao banco de dados para o SharePoint usar.
3. Crie uma palavra-passe para a frase de acesso de segurança do farm.

4. No assistente de configuração, é recomendável selecionar o tipo de [MinRole](/sharepoint/install/overview-of-minrole-server-roles-in-sharepoint-server) de **front-end**

5. Quando o assistente de configuração concluir a tarefa de configuração 10 de 10, clique em concluir e um navegador da Web será aberto.

6. Se o pop-up do Internet Explorer for solicitado, autentique como *Contoso\miminstall* (ou a conta de administrador equivalente) para continuar.

7. No assistente da Web (dentro do aplicativo Web), clique em **Cancelar/ignorar**.


## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>Preparar o SharePoint para alojar o Portal do MIM

> [!NOTE]
> Inicialmente, o SSL não será configurado. Certifique-se de que configura o SSL ou equivalente antes de ativar o acesso a este portal.

1. Inicie o **Shell de gerenciamento do SharePoint 2016** e execute o seguinte script do PowerShell para criar um **aplicativo Web do SharePoint 2016**.

    ```PowerShell
    New-SPManagedAccount ##Will prompt for new account enter contoso\mimpool 
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\mimpool
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 80 -URL http://mim.contoso.com
    ```

    > [!NOTE]
    > Será apresentada uma mensagem de aviso a indicar que está a ser utilizado o método de autenticação Clássico do Windows e o comando final poderá demorar alguns minutos a responder. Quando concluir, a saída indicará o URL do novo portal. Mantenha a janela do **Shell de gerenciamento do SharePoint 2016** aberta para referência posterior.

2. Inicie o Shell de gerenciamento do SharePoint 2016 e execute o seguinte script do PowerShell para criar um **conjunto de sites do SharePoint** associado a esse aplicativo Web.
   ```PowerShell
    $t = Get-SPWebTemplate -compatibilityLevel 15 -Identity "STS#1"
    $w = Get-SPWebApplication http://mim.contoso.com/
    New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\miminstall -CompatibilityLevel 15 -Name "MIM Portal"
    $s = SpSite($w.Url)
    $s.CompatibilityLevel
   ```
   > [!NOTE]
   > Verifique se o resultado da variável *CompatibilityLevel* é "15". Se o resultado for diferente de "15", o conjunto de sites não foi criado para a versão correta da experiência; Exclua o conjunto de sites e recrie-o.

    > [!IMPORTANT]
O SharePoint Server 2019 usa uma propriedade de aplicativo Web diferente para manter uma lista de extensões de arquivo bloqueadas. Portanto, para desbloquear. Arquivos ASHX usados pelo portal do MIM três comandos extras devem ser executados manualmente no Shell de gerenciamento do SharePoint.
<br/>
    **Execute os próximos três comandos somente para o SharePoint 2019:**

   ```PowerShell
    $w.BlockedASPNetExtensions.Remove("ashx")
    $w.Update()
    $w.BlockedASPNetExtensions
   ```
   > [!NOTE]
   > Verifique se a lista *BlockedASPNetExtensions* não contém mais a extensão ashx, caso contrário, várias páginas do portal do mim não serão renderizadas corretamente.


3. Desabilite o **ViewState do lado do servidor do SharePoint** e a tarefa do SharePoint "trabalho de análise de integridade (por hora, temporizador do Microsoft SharePoint Foundation, todos os servidores)" executando os seguintes comandos do PowerShell no Shell de gerenciamento do **SharePoint 2016**:

   ```PowerShell
   $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
   $contentService.ViewStateOnServer = $false;
   $contentService.Update();
   Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
   ```

4. No servidor de gerenciamento de identidade, abra uma nova guia do navegador da Web, navegue até http://mim.contoso.com/ e faça logon como *contoso\miminstall*.  Será apresentado um site do SharePoint vazio denominado *Portal do MIM*.

    ![Portal do MIM na imagem http://mim.contoso.com/](media/prepare-server-sharepoint/MIM_DeploySP1new.png)

5. Copie o URL, em seguida, no Internet Explorer, abra as **Opções da Internet**, mude para o **separador Segurança**, selecione **Intranet local** e clique em **Sites**.

    ![Imagem das Opções da Internet](media/MIM-DeploySP2.png)

6. Na janela **Intranet local**, clique em **Avançado** e cole o URL copiado na caixa de texto **Adicionar este Web site à zona**. Clique em **Adicionar** e feche as janelas.

7. Abra o programa **Ferramentas Administrativas**, navegue até **Serviços**, localize o serviço de Administração do SharePoint e inicie-o se ainda não estiver em execução.

> [!div class="step-by-step"]  
> [«SQL Server](prepare-server-sql2016.md)
> [Exchange Server»](prepare-server-exchange.md)
