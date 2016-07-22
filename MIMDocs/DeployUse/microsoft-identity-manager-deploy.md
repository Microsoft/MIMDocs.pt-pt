---
title: Implementar o MIM 2016 | Microsoft Identity Manager
description: "Obtenha a lista completa dos passos envolvidos na implementação do Microsoft Identity Manager 2016, desde a preparação do ambiente à configuração dos portais."
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: db062cf8dafe0b480db06cd8d913583c5b709246


---

# Implementar o MIM 2016
Os artigos nesta secção fornecem instruções passo a passo para a implementação do Microsoft Identity Manager (MIM) 2016 para cenários personalizados do utilizador final num servidor novo no qual o FIM e o MIM não tenham sido anteriormente implementados.

> [!NOTE]
> A topologia de implementação descrita nesta secção destina-se apenas à introdução e à aprendizagem do MIM.  O [guia de planeamento de capacidade](/microsoft-identity-manager/plan-design/capacity-planning-guide) fornece mais informações sobre topologias para implementações de produção.  É recomendável rever a referida documentação antes de implementar o MIM na utilização ou na escala de produção.

<!---
Comment: Restore after PAM content is included

The privileged access management scenario is deployed differently than other MIM scenarios, as it requires a dedicated bastion forest environment.  If you want to learn more about deploying MIM for Privileged Identity Management, see [Getting Started with Privileged Access Management](privileged-access-management-get-started.md).
--->

## Primeiro: preparar um domínio
O MIM funciona com o Active Directory (AD), como tal, siga estes passos para configurar o controlador de domínio do AD.
- [Configuração do domínio](preparing-domain.md)

## Seguinte: preparar um servidor de gestão de identidades
Assim que o seu domínio estiver no local e configurado, prepare o servidor de gestão de identidades empresariais. Isto inclui configurar:
- [Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [SQL Server 2014](prepare-server-sql2014.md)
- [SharePoint](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (opcional)

## Por último: instalar os componentes do Microsoft Identity Manager 2016
Depois de configurar o domínio e o servidor, está pronto para instalar os componentes MIM e configurá-los para sincronizar com o AD.
- [Serviço de Sincronização do MIM](install-mim-sync.md)
- [Portal e Serviço do MIM](install-mim-service-portal.md)
- [Sincronizar o Active Directory e as bases de dados do Serviço MIM](install-mim-sync-ad-service.md)



<!--HONumber=Jul16_HO3-->


