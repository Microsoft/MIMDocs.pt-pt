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
ms.sourcegitcommit: bc56b57a06592527bab13aad879ca13466e968b3
ms.openlocfilehash: 4ad5d755de100ad598c6cebd209296edd361e8c5


---

# Passo 7: Configurar o histórico/filtragem do SID

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

>[!div class="step-by-step"]
[« Passo 6](sp1-step6-setup-pam-trust.md)
[Passo 8 »](sp1-step8-pam-deployment-verification.md)



<!--HONumber=Oct16_HO1-->


