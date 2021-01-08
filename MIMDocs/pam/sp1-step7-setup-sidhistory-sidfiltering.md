---
title: 'Passo 7: Configurar o histórico/filtragem do SID'
description: Passo 7 de configurar o Gestor de Identidade da Microsoft utilizando scripts. Este passo inclui a configuração do histórico do SID/da filtragem do SID.
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
ms.openlocfilehash: c09649bbecfb4608391fef1cda3d8da87ded888b
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010647"
---
# <a name="step-7-setup-sid-historysid-filtering"></a>Passo 7: Configurar o histórico/filtragem do SID

> [!div class="step-by-step"]
> [« Passo 6](sp1-step6-setup-pam-trust.md) 
>  [Passo 8 »](sp1-step8-pam-deployment-verification.md)

**Os seguintes comandos não são necessários para um ambiente apenas PRIVA** Faça login no PAMServer com a conta MIMAdmin.

1. Faça login na CORP DC como administrador
2. Execute o PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecione a Opção 8 do Menu (Configurar histórico/filtragem do SID)

Após a execução com êxito, verá as mensagens seguintes:<br/></br>
Para a filtragem do SID: <br/></br>
“A definir a fidedignidade para não filtrar SIDs” ou “A filtragem por SID não está ativada para esta fidedignidade”. </br></br>
Para o histórico do SID: </br></br>
“A ativar o histórico de SIDs para esta fidedignidade” ou “O histórico de SIDs já está ativado para esta fidedignidade”.

> [!div class="step-by-step"]
> [« Passo 6](sp1-step6-setup-pam-trust.md) 
>  [Passo 8 »](sp1-step8-pam-deployment-verification.md)
