---
title: "Passo 6: Configurar a confiança da PAM"
description: "Preparar o domínio CORP com identidades novas ou existentes para ser gerido pelo Privileged Identity Manager através de scripts"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 10/04/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bc56b57a06592527bab13aad879ca13466e968b3
ms.openlocfilehash: 52226bfa5742e39d834f80dac69317e10b6259c7


---

# Passo 6: Configurar a confiança da PAM

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



<!--HONumber=Oct16_HO1-->


