---
title: 'Passo 6: Configurar a confiança da PAM'
description: Passo 6 da configuração do PAM através de scripts. Esta secção inclui a configuração da fidedignidade necessária entre os domínios CORP e PRIV
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
ms.openlocfilehash: ad1482718693c9ae7004a71334013de68f7c20da
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "64518303"
---
# <a name="step-6-set-up-the-pam-trust"></a>Passo 6: Configurar a confiança da PAM

> [!div class="step-by-step"]
> [« Passo 5](sp1-step5-configuring-pam.md)
> [Passo 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)

**A configuração não é necessária para um ambiente apenas PRIV**. Inicie sessão no PAMServer com a conta de MIMAdmin.

1. Inicie sessão no PAMServer com a Conta de MIMAdmin
2. Execute o PowerShell como administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecione a Opção 6 do Menu (Configuração da confiança da PAM)

   Quando lhe for pedido, introduza as credenciais da conta de administrador de CORP. Depois de fornecer as credenciais, poderá estabelecer a fidedignidade e a configuração ficará concluída.

> [!div class="step-by-step"]
> [« Passo 5](sp1-step5-configuring-pam.md)
> [Passo 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)
