---
title: 'Passo 5: Instalar/Configurar a PAM'
description: Este é o passo 5 de configurar o Gestor de Identidade do Microsoft usando scripts e cobre as etapas de implementação no servidor PAM.
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
ms.openlocfilehash: 01fe9cd7704674f408e0b9b5a27673989d0eaecf
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010595"
---
# <a name="step-5-installingconfiguring-pam"></a>Passo 5: Instalar/configurar a PAM

> [!div class="step-by-step"]
> [« Passo 4](sp1-step4-configuring-sharepoint.md) 
>  [Passo 6 »](sp1-step6-setup-pam-trust.md)

Para um PAMServer associado a um domínio, inicie sessão como MIMAdmin; caso contrário, inicie sessão como administrador local.
1. Execute o PowerShell como Administrador
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeploymnet.ps1
4. Selecione a Opção 5 do Menu (Configuração da PAM do MIM)

>[!NOTE]
>Se o computador ainda não estiver associado a um do domínio do PRIV, solicitará as credenciais. Após a associação a um domínio, o computador será reiniciado.

Depois de o PAMServer reiniciar, inicie sessão novamente no computador com a conta de MIMAdmin.

1. Execute o PowerShell como Administrador
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Selecione a Opção 5 do Menu (Configuração da PAM do MIM)

   Quando lhe for pedido, introduza a palavra-passe da Conta do Monitor do MIM, da Conta do Componente do MIM, da Conta de MA do MIM, da Conta do Serviço MIM, da Conta de Administrador do MIM e da Conta do SharePoint.
   Depois de concluída a instalação, o computador será reiniciado.

> [!div class="step-by-step"]
> [« Passo 4](sp1-step4-configuring-sharepoint.md) 
>  [Passo 6 »](sp1-step6-setup-pam-trust.md)
