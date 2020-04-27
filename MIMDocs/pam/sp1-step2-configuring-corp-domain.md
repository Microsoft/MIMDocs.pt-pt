---
title: 'Passo 2: Configurar o domínio CORP'
description: Este artigo descreve o segundo passo necessário para configurar o domínio CORP, que envolve a execução de um script depois de o sids.txt ser copiado para o CORPDC
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
ms.openlocfilehash: 1215e0be4d978e879ebc09ecdd99223fb3667a85
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043839"
---
# <a name="step-2-configuring-the-corp-domain"></a>Passo 2: Configurar o domínio CORP

> [!div class="step-by-step"]
> [« Passo 1](sp1-step1-configuring-priv-domain.md)
> [Passo 3»](sp1-step3-installing-configuring-sql.md)

Depois de o SIDs.txt ser copiado para o CORPDC, **não será necessário para as implementações PRIVOnly**

1. Inicie sessão no CORPDC como Administrador
2. Execute o PowerShell como Administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecione a Opção 2 do Menu (Configuração da Floresta CORP)

> [!div class="step-by-step"]
> [« Passo 1](sp1-step1-configuring-priv-domain.md)
> [Passo 3»](sp1-step3-installing-configuring-sql.md)
