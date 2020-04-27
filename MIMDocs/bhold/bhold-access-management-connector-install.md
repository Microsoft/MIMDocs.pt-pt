---
title: Instalação de conector de gestão de acesso BHOLD Microsoft Docs
description: O módulo de conector BHOLD suporta a sincronização inicial e contínua dos dados
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: ae9cc0bb4c63089c6733c06b7b035b2b9566fdd0
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2020
ms.locfileid: "79041868"
---
# <a name="access-management-connector-installation"></a>Instalação de conector de gestão de acesso

O módulo BHOLD Suite Access Management Connector suporta sincronização inicial e contínua de dados em BHOLD. O Conector de Gestão de Acesso trabalha com o Serviço de Sincronização do Gestor de Identidade da Microsoft (MIM) para mover dados entre a base de dados BHOLD Core, o metaverso FIM 2010 e aplicações-alvo e lojas de identidade. Após a instalação do módulo de Conector de Gestão de Acesso, poderá criar Agentes de Gestão FIM que controlam o fluxo de dados entre o BHOLD e o MIM.

## <a name="access-management-connector-software-requirements"></a>Requisitos de software de conector de gestão de acesso

Antes de poder instalar o módulo de conector de gestão de acesso, tem de instalar a Microsoft .NET Framework 4. Para obter mais informações sobre a .NET Framework 4 e instruções de instalação, consulte a página inicial da [Microsoft .NET](https://www.microsoft.com/net).
Deve instalar o Conector de Gestão de Acesso num computador que executa o Serviço de Sincronização FIM da MIM.

## <a name="access-management-connector-setup"></a>Configuração do Conector de Gestão de Acesso

Para instalar o módulo de Controlo de Gestão de Acesso, inicie sessão como membro do grupo Domain Admins, descarregue o seguinte ficheiro e execute-o como administrador no servidor que pretende instalar o módulo de integração BHOLD FIM em:

- AccessManagementConnector.msi

Para executar o ficheiro do programa como administrador, clique no ficheiro à direita e, em seguida, clique em **Executar como administrador**.

## <a name="next-steps"></a>Passos seguintes

- [Instalação de integração BHOLD FIM](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) Para ativar o autosserviço de funções do utilizador final, pode instalar o módulo de integração BHOLD FIM
- [Guia de instalação BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
