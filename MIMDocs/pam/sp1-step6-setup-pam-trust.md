---
title: "Passo 6: Configurar a confiança da PAM"
description: "Passo 6 da configuração do PAM através de scripts. Esta secção inclui a configuração da fidedignidade necessária entre os domínios CORP e PRIV"
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
ms.openlocfilehash: 3b232dfa515b42fd42a5606d1beff9d3fe50389c
ms.contentlocale: pt-pt
ms.lasthandoff: 07/10/2017


---

# Passo 6: Configurar a confiança da PAM
<a id="step-6-set-up-the-pam-trust" class="xliff"></a>

>[!div class="step-by-step"]
[« Passo 5](sp1-step5-configuring-pam.md)
[Passo 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)

**A configuração não é necessária para um ambiente apenas PRIV**. Inicie sessão no PAMServer com a conta de MIMAdmin.

1. Inicie sessão no PAMServer com a Conta de MIMAdmin
2. Execute o PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecione a Opção 6 do Menu (Configuração da confiança da PAM)

  Quando lhe for pedido, introduza as credenciais da conta de administrador de CORP. Depois de fornecer as credenciais, poderá estabelecer a fidedignidade e a configuração ficará concluída.

>[!div class="step-by-step"]
[« Passo 5](sp1-step5-configuring-pam.md)
[Passo 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)

