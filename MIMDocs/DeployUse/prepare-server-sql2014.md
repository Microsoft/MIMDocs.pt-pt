---
title: Configurar o SQL Server para o Microsoft Identity Manager 2016 | Documentos da Microsoft
description: "Instale o SQL Server 2014 em preparação para a instalação do MIM 2016."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 01/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: 3a40bf3bd5251ef101b25cc29251f33062e44cdc


---

# <a name="set-up-an-identity-management-server-sql-server-2014"></a>Configurar um servidor de gestão de identidades: SQL Server 2014

>[!div class="step-by-step"]
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)

> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio – **nomedoservidormim**
> - Nome de domínio – **contoso**
> - Palavra-passe – **Pass@word1**

## <a name="install-sql-server-2014-standard-edition"></a>Instalar o **SQL Server 2014 Standard Edition**

1. Inicie o **PowerShell** como um administrador do domínio.

2. Mude para o diretório onde está localizado o programa de configuração do SQL Server.

3. Escreva os seguintes comandos.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>[!div class="step-by-step"]  
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)



<!--HONumber=Jan17_HO4-->


