---
title: Instalação do conector de gerenciamento de acesso do BHOLD | Microsoft Docs
description: O módulo conector do BHOLD dá suporte à sincronização inicial e contínua de dados
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 60886a84c6105e94a2cd3d42f17b86b2d69c8c0a
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "64516050"
---
# <a name="access-management-connector-installation"></a>Instalação do conector de gerenciamento de acesso

O módulo do conector de gerenciamento de acesso do BHOLD Suite dá suporte à sincronização inicial e contínua de dados no BHOLD. O conector de gerenciamento de acesso funciona com o serviço de sincronização de Microsoft Identity Manager (MIM) para mover os dados entre o BHOLD Core Database, o metaverso do FIM 2010 e os aplicativos de destino e armazenamentos de identidade. Depois de instalar o módulo do conector de gerenciamento de acesso, você poderá criar agentes de gerenciamento do FIM que controlam o fluxo de dados entre o BHOLD e o MIM.

## <a name="access-management-connector-software-requirements"></a>Requisitos de software do conector de gerenciamento de acesso

Antes de instalar o módulo do conector de gerenciamento de acesso, você deve instalar o Microsoft .NET Framework 4. Para obter mais informações sobre .NET Framework 4 e instruções de instalação, consulte o [home page Microsoft .net](http://www.microsoft.com/net).
Você deve instalar o conector de gerenciamento de acesso em um computador que executa o serviço de sincronização do FIM do MIM.

## <a name="access-management-connector-setup"></a>Instalação do conector de gerenciamento de acesso

Para instalar o módulo de controle de gerenciamento de acesso, faça logon como um membro do grupo Admins. do domínio, baixe o arquivo a seguir e execute-o como administrador no servidor no qual você pretende instalar o módulo integração de FIM com o BHOLD:

- AccessManagementConnector. msi

Para executar o arquivo de programa como administrador, clique com o botão direito do mouse no arquivo e clique em **Executar como administrador**.

## <a name="next-steps"></a>Próximos passos

- [Instalação da integração do fim com o BHOLD](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) Para habilitar o autoatendimento de funções do usuário final, você pode instalar o módulo integração do FIM com o BHOLD
- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
