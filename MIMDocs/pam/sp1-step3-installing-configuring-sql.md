---
title: 'Passo 3: Configurar o SQL'
description: Este artigo é o passo 3 da série de artigos que cobrem como configurar o Microsoft Identity Manager usando scripts e discute os passos de configuração do servidor SQL.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: e4317363084da53e5683cd1a9b3d7bd0ce3ebcda
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010528"
---
# <a name="step-3-configuring-sql"></a>Passo 3: Configurar o SQL

> [!div class="step-by-step"]
> [« Passo 2](sp1-step2-configuring-corp-domain.md) 
>  [Passo 4 »](sp1-step4-configuring-sharepoint.md)

Antes de avançar com os passos abaixo, verifique se está a utilizar o SQL Server 2012 SP1 ou posterior ou o SQL server 2014. Para os servidores de ligação de domínio, inicie sessão utilizando a conta MIMAdmin, caso contrário faça login como administrador local.
1. Execute o PowerShell como Administrador
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Selecione a Opção 3 do Menu (Configuração do SQL Server)

   Se o servidor ainda não estiver associado ao domínio PRIV, terá de fornecer as credenciais e associar o servidor ao domínio.
   Após a associação a um domínio, o computador será reiniciado. Após o reboot bem sucedido, inicie sessão no servidor com a conta MIMAdmin.

5. Execute o PowerShell como administrador
6. cd $env:SYSTEMDRIVE\PAM
7. .\PAMDeployment.ps1
8. Selecione a Opção 3 do Menu (Configuração do SQL Server)

Quando lhe for pedido, forneça a palavra-passe da conta de serviço de MIMAdmin e deixe a instalação continuar. Depois de concluído, avance para o Passo 4.

> [!div class="step-by-step"]
> [« Passo 2](sp1-step2-configuring-corp-domain.md) 
>  [Passo 4 »](sp1-step4-configuring-sharepoint.md)
