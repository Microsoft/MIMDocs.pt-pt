---
title: Configurar o SQL Server para o Microsoft Identity Manager 2016 | Documentos da Microsoft
description: "Instale o SQL Server 2014 em preparação para a instalação do MIM 2016."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: 7f16c3a054f0a2c59f118ba33bf64fca10034690
ms.openlocfilehash: 105d2320ed5a0d610e8e6c5f459366680e3f8a77
ms.contentlocale: pt-pt
ms.lasthandoff: 07/10/2017


---

# Configurar um servidor de gestão de identidades: SQL Server 2014
<a id="set-up-an-identity-management-server-sql-server-2014" class="xliff"></a>

>[!div class="step-by-step"]
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)

> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio – **nomedoservidormim**
> - Nome de domínio – **contoso**
> - Palavra-passe – **Pass@word1**

## Instalar o **SQL Server 2014 Standard Edition**
<a id="install-sql-server-2014-standard-edition" class="xliff"></a>

1. Inicie o **PowerShell** como um administrador do domínio.

2. Mude para o diretório onde está localizado o programa de configuração do SQL Server.

3. Escreva os seguintes comandos.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>[!div class="step-by-step"]  
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)

