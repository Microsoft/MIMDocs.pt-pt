---
title: "Passo 2: Configurar o domínio CORP"
description: "Este artigo descreve o segundo passo necessário para configurar o domínio CORP, que envolve a execução de um script depois de o sids.txt ser copiado para o CORPDC"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: f08b0197341351bd5f33552f26b96132b1356239
ms.openlocfilehash: 8480350f85b3543a32d4db3dbc6a388afcb16352
ms.contentlocale: pt-pt
ms.lasthandoff: 07/10/2017


---

# Passo 2: Configurar o domínio CORP
<a id="step-2-configuring-the-corp-domain" class="xliff"></a>

>[!div class="step-by-step"]
[« Passo 1](sp1-step1-configuring-priv-domain.md)
[Passo 3 »](sp1-step3-installing-configuring-sql.md)

Depois de o SIDs.txt ser copiado para o CORPDC, **não será necessário para as implementações PRIVOnly**

1. Inicie sessão no CORPDC como Administrador
2. Execute o PowerShell como Administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecione a Opção 2 do Menu (Configuração da Floresta CORP)

>[!div class="step-by-step"]
[« Passo 1](sp1-step1-configuring-priv-domain.md)
[Passo 3 »](sp1-step3-installing-configuring-sql.md)

