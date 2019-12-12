---
title: 'Passo 3: Configurar o SQL'
description: Este artigo inclui o passo 3 da série de artigos que descreve como configurar o Privileged Identity Manager através de scripts e os passos de configuração do SQL Server.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 69f86c5366f7b662f3a11fa4ac8d44159421f909
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "64518156"
---
# <a name="step-3-configuring-sql"></a>Passo 3: Configurar o SQL

> [!div class="step-by-step"]
> [« Passo 2](sp1-step2-configuring-corp-domain.md)
> [Passo 4 »](sp1-step4-configuring-sharepoint.md)

Antes de avançar com os passos abaixo, verifique se está a utilizar o SQL Server 2012 SP1 ou posterior ou o SQL server 2014. Para servidores associados a um domínio, inicie sessão com a conta de MIMAdmin; caso contrário, inicie sessão como administrador local
1. Execute o PowerShell como Administrador
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Selecione a Opção 3 do Menu (Configuração do SQL Server)

   Se o servidor ainda não estiver associado ao domínio PRIV, terá de fornecer as credenciais e associar o servidor ao domínio.
   Após a associação a um domínio, o computador será reiniciado. Após a reinicialização bem sucedida, inicie sessão no servidor com a conta de MIMAdmin.

5. Execute o PowerShell como administrador
6. cd $env:SYSTEMDRIVE\PAM
7. .\PAMDeployment.ps1
8. Selecione a Opção 3 do Menu (Configuração do SQL Server)

Quando lhe for pedido, forneça a palavra-passe da conta de serviço de MIMAdmin e deixe a instalação continuar. Depois de concluído, avance para o Passo 4.

> [!div class="step-by-step"]
> [« Passo 2](sp1-step2-configuring-corp-domain.md)
> [Passo 4 »](sp1-step4-configuring-sharepoint.md)
