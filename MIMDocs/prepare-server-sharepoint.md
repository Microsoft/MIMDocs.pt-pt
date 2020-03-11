---
title: Configurar o SharePoint para o Microsoft Identity Manager 2016 | Documentos da Microsoft
description: Instalar e configurar o SharePoint Foundation para que possa alojar a página do Portal do MIM.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 04/26/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 7fb65eec7a42da94c4f27a30e59c09739279e882
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043533"
---
# <a name="set-up-an-identity-management-server-sharepoint"></a>Configurar um servidor de gestão de identidades: SharePoint

> [!div class="step-by-step"]
> [«Servidor SQL](prepare-server-sql2016.md)
> [Servidor de Câmbio »](prepare-server-exchange.md)
> 

> [!NOTE]
> O procedimento de configuração do SharePoint Server 2019 não difere do procedimento de configuração do SharePoint Server 2016, **exceto** um passo extra que deve ser dado para desbloquear ficheiros ASHX utilizados pelo Portal MIM.

> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio - **corpdc**
> - Nome de domínio – **contoso**
> - Nome do Servidor de Serviço MIM - **corpservice**
> - Nome do Servidor MIM Sync - **corpsync**
> - Nome do Servidor SQL - **corpsql**
> - Palavra-passe – <strong>Pass@word1</strong>


## <a name="install-sharepoint-2016"></a>Instalar **o SharePoint 2016**

> [!NOTE]
> O instalador requer uma ligação à Internet para transferir os respetivos pré-requisitos. Se o computador estiver numa rede virtual que não fornece conetividade à Internet, adicione uma interface de rede adicional ao computador que forneça uma ligação à Internet. Isto pode ser desativado depois de concluída a instalação.

Siga estes passos para instalar o SharePoint 2016. Depois de concluir a instalação, o servidor será reiniciado.

1.  Lance **powerShell** como uma conta de domínio com administração local sobre o **serviço de corpo** e **sisadmina** no servidor de base de dados SQL usaremos **contoso\miminstall**.

    -   Mude para o diretório onde o SharePoint foi descompactado.

    -   Escreva o seguinte comando.
    ```
    .\prerequisiteinstaller.exe
    ```

2.  Depois de instalados pré-requisitos do **SharePoint,** instale **o SharePoint 2016** digitando o seguinte comando:

    ```
    .\setup.exe
    ```

3.  Selecione o tipo de servidor completo.

4.  Depois de concluída a instalação, execute o assistente.

## <a name="run-the-wizard-to-configure-sharepoint"></a>Executar o assistente para configurar o SharePoint

Siga os passos delineados no **Assistente de Configuração de Produtos SharePoint** para configurar o SharePoint para funcionar com o MIM.

1. No separador **Ligar a um farm de servidores**, mude para criar um novo farm de servidores.

2. Especifique este servidor como o servidor de base de dados como o **corpsql** para a base de dados de configuração, e *Contoso\SharePoint* como a conta de acesso à base de dados para o SharePoint usar.
3. Crie uma palavra-passe para a frase de acesso de segurança do farm.

4. Na configuração Wizard recomendamos selecionar o tipo [MinRole](/sharepoint/install/overview-of-minrole-server-roles-in-sharepoint-server) de **Front-end**

5. Quando o assistente de configuração completar a tarefa de configuração 10 de 10, clique em Terminar e um navegador web será aberto..

6. Se solicitado o popup do Internet Explorer, autenticar como *Contoso\miminstall* (ou a conta de administrador equivalente) para prosseguir.

7. No assistente web (dentro da aplicação web) clique **em Cancelar/Saltar**.


## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>Preparar o SharePoint para alojar o Portal do MIM

> [!NOTE]
> Inicialmente, o SSL não será configurado. Certifique-se de que configura o SSL ou equivalente antes de ativar o acesso a este portal.

1. Lançar A Shell management **SharePoint 2016** e executar o seguinte script PowerShell para criar uma **Aplicação Web SharePoint 2016**.

    ```PowerShell
    New-SPManagedAccount ##Will prompt for new account enter contoso\mimpool 
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\mimpool
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 80 -URL http://mim.contoso.com
    ```

    > [!NOTE]
    > Será apresentada uma mensagem de aviso a indicar que está a ser utilizado o método de autenticação Clássico do Windows e o comando final poderá demorar alguns minutos a responder. Quando concluir, a saída indicará o URL do novo portal. Mantenha a janela **SharePoint 2016 Management Shell** aberta para referência mais tarde.

2. Lance a SharePoint 2016 Management Shell e execute o seguinte script PowerShell para criar uma Coleção de **Site SharePoint** associada a essa aplicação web.
    ```PowerShell
    $t = Get-SPWebTemplate -compatibilityLevel 15 -Identity "STS#1"
    $w = Get-SPWebApplication http://mim.contoso.com/
    New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\miminstall -CompatibilityLevel 15 -Name "MIM Portal"
    $s = SpSite($w.Url)
    $s.CompatibilityLevel
    ```
    > [!NOTE]
    > Verifique se o resultado da variável *CompatibilidadeN* é "15". Se o resultado for diferente de "15", então a coleção do site não foi criada a versão correta da experiência; apagar a recolha do site e recriá-la.

    > [!IMPORTANT]
    > O SharePoint Server 2019 utiliza diferentes propriedades de aplicações web para manter uma lista de extensões de ficheiros bloqueadas. Portanto, para desbloquear . Os ficheiros ASHX utilizados pelo Portal MIM três comandos extra devem ser executados manualmente a partir da Shell de Gestão sharePoint.
    <br/>
    **Execute os próximos três comandos apenas para o SharePoint 2019:**

    ```PowerShell
    $w.BlockedASPNetExtensions.Remove("ashx")
    $w.Update()
    $w.BlockedASPNetExtensions
    ```
   > [!NOTE]
   > Verifique se a lista de *extensões bloqueadas* não contém mais extensão ASHX, caso contrário, várias páginas do Portal MIM não serão corretamente corretas.


3. Desative o **SharePoint Server-Side Viewstate** e a tarefa SharePoint "Health Analysis Job (Hora, Microsoft SharePoint Foundation Timer, All Servers)" executando os seguintes comandos PowerShell na **Shell management SharePoint 2016**:

   ```PowerShell
   $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
   $contentService.ViewStateOnServer = $false;
   $contentService.Update();
   Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
   ```

4. No seu servidor de gestão de identidade, abra um novo separador de navegador web, navegue para http://mim.contoso.com/ e faça login como *contoso\miminstall*.  Será apresentado um site do SharePoint vazio denominado *Portal do MIM*.

    ![Portal MIM na imagem http://mim.contoso.com/](media/prepare-server-sharepoint/MIM_DeploySP1new.png)

5. Copie o URL, em seguida, no Internet Explorer, abra as **Opções da Internet**, mude para o **separador Segurança**, selecione **Intranet local** e clique em **Sites**.

    ![Imagem das Opções da Internet](media/MIM-DeploySP2.png)

6. Na janela **Intranet local**, clique em **Avançado** e cole o URL copiado na caixa de texto **Adicionar este Web site à zona**. Clique em **Adicionar** e feche as janelas.

7. Abra o programa **Ferramentas Administrativas**, navegue até **Serviços**, localize o serviço de Administração do SharePoint e inicie-o se ainda não estiver em execução.

> [!div class="step-by-step"]  
> [«Servidor SQL](prepare-server-sql2016.md)
> [Servidor de Câmbio »](prepare-server-exchange.md)
