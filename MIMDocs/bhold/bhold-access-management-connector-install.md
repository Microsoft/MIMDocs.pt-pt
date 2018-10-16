---
title: Instalação de conector de gestão de acesso do BHOLD | Documentos da Microsoft
description: O módulo de conector do BHOLD suporta sincronização inicial e contínua de dados
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: d3a4ab5203e772db80c5345aab8f1c66731c8020
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333705"
---
# <a name="access-management-connector-installation"></a>Instalação de conector de gestão de acesso

O módulo de conector de gestão de acesso do BHOLD Suite suporta sincronização inicial e contínua de dados em BHOLD. O conector de gestão de acesso funciona com o serviço de sincronização do Microsoft Identity Manager (MIM) para mover dados entre a base de dados do BHOLD Core, o metaverso do FIM 2010 e aplicativos de destino e armazenamentos de identidades. Depois de instalar o módulo de conector de gestão de acesso, poderá criar agentes de gestão de FIM que controlam o fluxo de dados entre BHOLD e MIM.

## <a name="access-management-connector-software-requirements"></a>Requisitos de software do conector de gestão de acesso

Antes de instalar o módulo de conector de gestão de acesso, tem de instalar o Microsoft .NET Framework 4. Para obter mais informações sobre o .NET Framework 4 e instruções de instalação, consulte a [home page do Microsoft .NET](http://www.microsoft.com/net).
Tem de instalar o conector de gestão de acesso num computador com o FIM sincronização de serviço de MIM.

## <a name="access-management-connector-setup"></a>Configuração do conector de gestão de acesso

Para instalar o módulo de controlo de acesso de gestão, inicie sessão como membro do grupo Admins do domínio, transfira o ficheiro seguinte e executá-lo como administrador no servidor que pretende instalar o módulo de integração do BHOLD FIM em:

- AccessManagementConnector.msi

Para executar o ficheiro de programa como administrador, o ficheiro com o botão direito e, em seguida, clique em **executar como administrador**.

## <a name="next-steps"></a>Passos Seguintes

- [Instalação do BHOLD FIM integração](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) para ativar a gestão personalizada do utilizador final das funções, pode instalar o módulo de integração do BHOLD FIM
- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)