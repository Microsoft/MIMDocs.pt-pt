---
# required metadata

title: Configurar um servidor de gestão de identidades&#58; SQL Server 2014 | Microsoft Identity Manager
description: Instale o SQL Server 2014 em preparação para a instalação do MIM 2016.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Configurar um servidor de gestão de identidades: SQL Server 2014

>[!div class="passo a passo"]
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)

> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio – **nomedoservidormim**
> - Nome de domínio – **contoso**
> - Palavra-passe – **Palavra@passe1**

## Instalar o **SQL Server 2014 Standard Edition**

1. Inicie o **PowerShell** como um administrador do domínio.

2. Mude para o diretório onde está localizado o programa de configuração do SQL Server.

3. Escreva os seguintes comandos.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>[!div class="passo a passo"]  
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)


<!--HONumber=Apr16_HO4-->


