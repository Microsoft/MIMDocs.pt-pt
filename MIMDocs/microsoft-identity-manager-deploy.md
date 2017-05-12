---
title: "Passos necessários para implementar o Microsoft Identity Manager 2016 | Documentos da Microsoft"
description: "Obtenha a lista completa dos passos envolvidos na implementação do Microsoft Identity Manager 2016, desde a preparação do ambiente à configuração dos portais."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: 3797f5789bb4e48836eb21776dafd5a2e0e11613
ms.openlocfilehash: fa200bb18871387420743af64ca196565397e5d5
ms.contentlocale: pt-pt
ms.lasthandoff: 05/09/2017


---

# <a name="deploy-mim-2016"></a>Implementar o MIM 2016
Os artigos nesta secção fornecem instruções passo a passo para a implementação do Microsoft Identity Manager (MIM) 2016 para cenários personalizados do utilizador final num servidor novo no qual o FIM e o MIM não tenham sido anteriormente implementados.

> [!NOTE]
> A topologia de implementação descrita nesta secção destina-se apenas à introdução e à aprendizagem do MIM.  O [guia de planeamento de capacidade](capacity-planning-guide.md) fornece mais informações sobre topologias para implementações de produção.  É recomendável rever a referida documentação antes de implementar o MIM na utilização ou na escala de produção.

O cenário de gestão de acesso privilegiado é implementado de forma diferente de outros cenários MIM, porque requer um ambiente dedicado de bastião de floresta.  Se quiser saber mais sobre implementar o MIM para Privileged Identity Management, consulte [Configuração do ambiente de MIM para Privileged Access Management](./pam/configuring-mim-environment-for-pam.md).

O processo de implementação MIM 2016 é muito semelhante ao processo para o respetivo predecessor, FIM 2010 R2. Se pretende referir-se à documentação FIM, Consultar o [Guia de Implementação do Forefront Identity Manager 2010 R2](https://technet.microsoft.com/library/jj134310).

## <a name="first-prepare-a-domain"></a>Primeiro: preparar um domínio
O MIM funciona com o Active Directory (AD), como tal, siga estes passos para configurar o controlador de domínio do AD.
- [Configuração do domínio](preparing-domain.md)

## <a name="next-prepare-an-identity-management-server"></a>Seguinte: preparar um servidor de gestão de identidades
Assim que o seu domínio estiver no local e configurado, prepare o servidor de gestão de identidades empresariais. Isto inclui configurar:
- [Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [SQL Server 2014](prepare-server-sql2014.md)
- [SharePoint](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (opcional)

## <a name="finally-install-microsoft-identity-manager-2016-components"></a>Por último: instalar os componentes do Microsoft Identity Manager 2016
Depois de configurar o domínio e o servidor, está pronto para instalar os componentes MIM e configurá-los para sincronizar com o AD.
- [Serviço de Sincronização do MIM](install-mim-sync.md)
- [Serviço e Portal do MIM](install-mim-service-portal.md)
- [Sincronizar o Active Directory e as bases de dados do Serviço MIM](install-mim-sync-ad-service.md)
