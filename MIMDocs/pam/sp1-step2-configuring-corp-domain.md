---
title: "Passo 2: Configurar o domínio CORP"
description: "Preparar o domínio CORP com identidades novas ou existentes para ser gerido pelo Privileged Identity Manager através de scripts"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: 7919fa3fdbb6613fbfa636ddb34762faa85925b9


---

# Passo 2: Configurar o domínio CORP

Depois de o SIDs.txt ser copiado para o CORPDC, **não será necessário para as implementações PRIVOnly**

1. Inicie sessão no CORPDC como Administrador
2. Execute o PowerShell como Administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecione a Opção 2 do Menu (Configuração da Floresta CORP)



<!--HONumber=Sep16_HO4-->


