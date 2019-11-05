---
title: Configurar SQL Server para Microsoft Identity Manager 2016 SP2 | Microsoft Docs
description: Instale o SQL Server 2016 ou 2017 em preparação para a instalação do MIM 2016.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4be699f123bf7d48b709ee8b8e91e2222cd492e2
ms.sourcegitcommit: 323c2748dcc6b6991b1421dd8e3721588247bc17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/04/2019
ms.locfileid: "73568016"
---
# <a name="set-up-an-identity-management-server-sql-server-2016-or-2017"></a>Configurar um servidor de gerenciamento de identidade: SQL Server 2016 ou 2017

> [!div class="step-by-step"]
> [«Windows Server](prepare-server-ws2016.md)
> [SharePoint»](prepare-server-sharepoint.md)
 
> [!NOTE] 
> O procedimento de instalação do SQL Server 2017 não difere do procedimento de instalação do SQL Server 2016.

> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio- **corpdc**
> - Nome de domínio – **contoso**
> - Nome do servidor do serviço do MIM- **corpservice**
> - Nome do servidor de sincronização do MIM- **corpsync**
> - Nome do SQL Server- **corpsql**
> - Palavra-passe – <strong>Pass@word1</strong>

> [!IMPORTANT]
> O MIM 2016 SP2 dá suporte a ouvintes de AoAG (grupo de disponibilidade AlwaysOn do SQL) com a opção *RegisterAllProvidersIP* definida como 0, o que significa que não há suporte atualmente para o failover de sub-rede cruzada SQL Server AoAG.

## <a name="install-sql-server-2016-standardenterprise-edition"></a>Instalar o **SQL Server 2016 Standard/Enterprise Edition**

1. Inicie o **PowerShell** como um administrador do domínio.

2. Mude para o diretório onde está localizado o programa de configuração do SQL Server.

3. Escreva os seguintes comandos.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```
    
Mais informações as contas e os serviços de implantação do SQL podem ser encontrados [aqui](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-2017)

> [!NOTE]
> O SSMS não está mais incluído no SQL 2016. Os detalhes de download podem ser encontrados [aqui](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)

> [!div class="step-by-step"]  
> [«Windows Server](prepare-server-ws2016.md)
> [SharePoint»](prepare-server-sharepoint.md)
