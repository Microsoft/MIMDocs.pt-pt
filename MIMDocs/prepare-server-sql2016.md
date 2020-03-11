---
title: Configure SQL Server para Microsoft Identity Manager 2016 SP2  Microsoft Docs
description: Instale o SQL Server 2016 ou 2017 em preparação para a sua instalação MIM 2016.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 04/26/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2ca9131a6c4f38ed559618d662848b74e1bffe66
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043499"
---
# <a name="set-up-an-identity-management-server-sql-server-2016-or-2017"></a>Criar um servidor de gestão de identidade: SQL Server 2016 ou 2017

> [!div class="step-by-step"]
> [«Servidor do Windows](prepare-server-ws2016.md)
> [SharePoint»](prepare-server-sharepoint.md)
 
> [!NOTE] 
> O procedimento de configuração do Sql Server 2017 não difere do procedimento de configuração do Sql Server 2016.

> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio - **corpdc**
> - Nome de domínio – **contoso**
> - Nome do Servidor de Serviço MIM - **corpservice**
> - Nome do Servidor MIM Sync - **corpsync**
> - Nome do Servidor SQL - **corpsql**
> - Palavra-passe – <strong>Pass@word1</strong>

> [!IMPORTANT]
> Mim 2016 SP2 suporta os ouvintes do SQL AlwaysOn Availability Group (AoAG) com a opção *RegisterAllProvidersIP* definida para 0, o que significa que o sQL Server AoAG cross-subnet failover não é atualmente suportado.

## <a name="install-sql-server-2016-standardenterprise-edition"></a>Instale **a Edição Standard/Enterprise do SQL Server 2016**

1. Inicie o **PowerShell** como um administrador do domínio.

2. Mude para o diretório onde está localizado o programa de configuração do SQL Server.

3. Escreva os seguintes comandos.

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```
    
Mais informações contas e serviços de implantação sQL podem ser encontrados [aqui](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-2017)

> [!NOTE]
> O SSMS já não está incluído no SQL 2016. Detalhes do download podem ser encontrados [aqui](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)

> [!div class="step-by-step"]  
> [«Servidor do Windows](prepare-server-ws2016.md)
> [SharePoint»](prepare-server-sharepoint.md)
