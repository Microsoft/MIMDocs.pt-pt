---
title: Requisitos de software do PAM | Documentos da Microsoft
description: Localize os requisitos de hardware e software para uma implementação de Privileged Access Management com êxito
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 02/09/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8d3c266c78df21c6ddb24f62618621c55820bd6f
ms.sourcegitcommit: 0e2b4b47a8050737c78e3b0ad088358e5de7e929
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2021
ms.locfileid: "100395457"
---
# <a name="hardware-and-software-requirements"></a>Hardware and software requirements (Requisitos de software e hardware)

A Gestão de Acesso Privilegiado não tem requisitos de hardware para além dos requisitos das plataformas de software subjacentes. Basta certificar-se de que tem suficiente memória ou espaço em disco e conectividade de rede.

> [!IMPORTANT]
> Este artigo fornece os requisitos mínimos para uma implantação básica numa rede isolada. Não se destina a demonstrar elevada disponibilidade, desempenho ou escalabilidade, e não representa uma topologia de implementação recomendada para grandes empresas ou ambientes de produção.  Se o seu Ative Directory faz parte de um ambiente ligado à Internet, consulte em vez disso a [orientação privilegiada](/security/compass/overview) de acesso para obter mais informações sobre por onde começar.

## <a name="installing-from-software-packages"></a>Instalar a partir de pacotes de software

O software seguinte pode ser transferido a partir do Centro de Avaliação TechNet ou MSDN:

- Microsoft Identity Manager 2016
  - Serviço e Portal: contém o instalador para o Serviço do MIM e para o Portal do MIM, e para o Cenário de PAM
  - Suplementos e Extensões: contém o instalador para os cmdlets do PowerShell requerentes

O seguinte software opcional pode ser descarregado a partir do GitHub:

- [PAMSamplePortal:](https://github.com/Azure/identity-management-samples)contém aplicação web de amostra para a API REST

## <a name="required-software"></a>Software necessário

- Windows Server 2016
- Windows 10 Enterprise
- SQL Server 2012 Service Pack 1 ou SQL Server 2014

## <a name="hardware-requirements"></a>Requisitos de hardware

Para cada componente da PAM, consulte os requisitos de sistema dos produtos de software.

Para CORPDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) ou anterior

Para CORPWKSTN:

- [Windows 10](https://technet.microsoft.com/windows/dn798752.aspx)

Para PRIVDC:

- Windows Server 2016

Para PAMSRV:

- Windows Server 2016
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) ou [do SQL Server 2014](https://msdn.microsoft.com/library/ms143506(v=sql.120).aspx)
