---
title: "Passo 7: Configurar o histórico/filtragem do SID"
description: "Preparar o domínio CORP com identidades novas ou existentes para ser gerido pelo Privileged Identity Manager através de scripts"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: 4c5cfa92f3111a6d298f586ba547a1eca2502853


---

# Configurar o histórico/filtragem do SID

**A configuração não é necessária para um ambiente apenas PRIV**. Inicie sessão no PAMServer com a conta de MIMAdmin.

1. Inicie sessão no DC do CORP como administrador
2. Execute o PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecione a Opção 8 do Menu (Configurar histórico/filtragem do SID)

Após a execução com êxito, verá as mensagens seguintes:<br/></br>
Para a filtragem do SID: <br/></br>
“A definir a fidedignidade para não filtrar SIDs” ou “A filtragem por SID não está ativada para esta fidedignidade”. </br></br>
Para o histórico do SID: </br></br>
“A ativar o histórico de SIDs para esta fidedignidade” ou “O histórico de SIDs já está ativado para esta fidedignidade”.



<!--HONumber=Sep16_HO4-->


