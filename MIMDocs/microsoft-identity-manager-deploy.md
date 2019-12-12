---
title: Passos necessários para implementar o Microsoft Identity Manager 2016 | Documentos da Microsoft
description: Obtenha a lista completa dos passos envolvidos na implementação do Microsoft Identity Manager 2016, desde a preparação do ambiente à configuração dos portais.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/17/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: 8216dc144dd7cee2ccb30d89198f6d2bb3a726c1
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "73329448"
---
# <a name="deploy-microsoft-identity-manager-2016-sp2"></a>Implantar o Microsoft Identity Manager 2016 SP2
Os artigos nesta secção fornecem instruções passo a passo para a implementação do Microsoft Identity Manager (MIM) 2016 para cenários personalizados do utilizador final num servidor novo no qual o FIM e o MIM não tenham sido anteriormente implementados.

> [!NOTE]
> A topologia de implementação descrita nesta secção destina-se apenas à introdução e à aprendizagem do MIM.  O [guia de planeamento de capacidade](capacity-planning-guide.md) fornece mais informações sobre topologias para implementações de produção.  É recomendável rever a referida documentação antes de implementar o MIM na utilização ou na escala de produção.

O cenário de gestão de acesso privilegiado é implementado de forma diferente de outros cenários MIM, porque requer um ambiente dedicado de bastião de floresta.  Se quiser saber mais sobre implementar o MIM para Privileged Identity Management, consulte [Configuração do ambiente de MIM para Privileged Access Management](./pam/configuring-mim-environment-for-pam.md).

O processo de implantação do MIM é muito semelhante ao processo para seu antecessor, FIM 2010 R2. Se pretende referir-se à documentação FIM, Consultar o [Guia de Implementação do Forefront Identity Manager 2010 R2](https://technet.microsoft.com/library/jj134310).

## <a name="first-prepare-a-domain"></a>Primeiro: preparar um domínio
O MIM funciona com o Active Directory (AD), como tal, siga estes passos para configurar o controlador de domínio do AD.
- [Configuração do domínio](preparing-domain.md)


## <a name="next-prepare-an-identity-management-servers"></a>Próximo: preparar um servidor de gerenciamento de identidades
Assim que o seu domínio estiver no local e configurado, prepare o servidor de gestão de identidades empresariais.

Para obter mais informações sobre plataformas com suporte, consulte [plataformas com suporte para o MIM 2016 ou posterior](microsoft-identity-manager-2016-supported-platforms.md)

 Isto inclui configurar:
- [Windows Server](prepare-server-ws2016.md)
- [SQL Server](prepare-server-sql2016.md)
- [SharePoint Server](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (opcional)

## <a name="finally-install-microsoft-identity-manager-2016-sp2-components"></a>Por fim: instalar Microsoft Identity Manager componentes do 2016 SP2
Depois de configurar o domínio e o servidor, está pronto para instalar os componentes MIM e configurá-los para sincronizar com o AD.
- [Serviço de Sincronização do MIM](install-mim-sync.md)
- [Serviço e Portal do MIM](install-mim-service-portal.md)
- [Sincronizar o Active Directory e as bases de dados do Serviço MIM](install-mim-sync-ad-service.md)

