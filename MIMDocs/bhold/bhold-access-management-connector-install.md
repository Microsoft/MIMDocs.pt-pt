---
title: "Instalação de conector de gestão de acesso do BHOLD | Microsoft Docs"
description: "O módulo de conector do BHOLD suporta inicial e contínua sincronização de dados"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 6d7f19f470d0c0f82a68652115ab9265a13b3508
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2017
---
# <a name="access-management-connector-installation"></a>Instalação do conector de gestão de acesso

O módulo de conector de gestão de acesso do BHOLD Suite suporta inicial e contínua a sincronização de dados para BHOLD. O conector de gestão de acesso funciona com o serviço de sincronização do Microsoft Identity Manager (MIM) para mover dados entre a base de dados do BHOLD Core, o metaverso do FIM 2010 e aplicações de destino e arquivos de identidade. Depois de instalar o módulo de conector de gestão de acesso, poderá criar agentes de gestão de FIM que controlam o fluxo de dados entre BHOLD e de MIM.

## <a name="access-management-connector-software-requirements"></a>Requisitos de software do conector de gestão de acesso

Antes de poder instalar o módulo de conector de gestão de acesso, tem de instalar o Microsoft .NET Framework 4. Para obter mais informações sobre o .NET Framework 4 e instruções de instalação, consulte o [home page do Microsoft .NET](http://www.microsoft.com/net).
Tem de instalar o conector de gestão de acesso num computador com o FIM sincronização serviço de MIM.

## <a name="access-management-connector-setup"></a>Configuração do conector de gestão de acesso

Para instalar o módulo de controlo de gestão de acesso, inicie sessão como membro do grupo Admins do domínio, transfira o ficheiro seguinte e executá-la como administrador no servidor que tenciona instalar o módulo de integração do BHOLD FIM em:

- AccessManagementConnector.msi

Para executar o ficheiro de programa como administrador, clique com o botão direito do ficheiro e, em seguida, clique em **executar como administrador**.

## <a name="next-steps"></a>Próximos passos

- [Instalação de integração do BHOLD FIM](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) para ativar a personalização de utilizador final das funções, pode instalar o módulo de integração do BHOLD FIM
- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)