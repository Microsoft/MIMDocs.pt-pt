---
title: Requisitos de software do PAM | Documentos da Microsoft
description: Localize os requisitos de hardware e software para uma implementação de Privileged Access Management com êxito
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/06/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 417f26b123aec7e8ab1b8cf254e0771f799dcb9d
ms.sourcegitcommit: 41d399b16dc64c43da3cc3b2d77529082fe1d23a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2021
ms.locfileid: "98103974"
---
# <a name="hardware-and-software-requirements"></a>Hardware and software requirements (Requisitos de software e hardware)

A Privileged Access Management não tem requisitos de hardware para além das plataformas de software subjacentes. Basta certificar-se de que tem suficiente memória ou espaço em disco e conectividade de rede.

> [!IMPORTANT]
> Este artigo fornece os requisitos mínimos para uma implementação básica. Não se destina a demonstrar desempenho, escalabilidade ou elevada disponibilidade. Não representa uma topologia de implantação recomendada para grandes empresas ou ambientes de produção.

## <a name="installing-from-software-packages"></a>Instalar a partir de pacotes de software

O software seguinte pode ser transferido a partir do Centro de Avaliação TechNet ou MSDN:

- Microsoft Identity Manager 2016
  - Serviço e Portal: contém o instalador para o Serviço do MIM e para o Portal do MIM, e para o Cenário de PAM
  - Suplementos e Extensões: contém o instalador para os cmdlets do PowerShell requerentes

O software seguinte pode ser transferido a partir do GitHub:

- [PAMSamplePortal:](https://github.com/Azure/identity-management-samples)contém aplicação web de amostra para a API REST

## <a name="required-software"></a>Software necessário

- Windows Server 2012 R2
- Windows 10 Enterprise
- SQL Server 2012 Service Pack 1 ou SQL Server 2014

## <a name="evaluation-software"></a>Software de avaliação

Caso não tenha licenças para o Windows, para o SQL Server ou para o Windows Server, pode transferir as versões de avaliação.

### <a name="technet-evaluation-center"></a>Centro de avaliação TechNet

- Windows Server 2012 R2 ou mais tarde
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)

### <a name="microsoft-download-center"></a>Centro de Transferências da Microsoft

- [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads)  
- [SharePoint Foundation 2013 SP1 e pré-requisitos](https://www.microsoft.com/download/details.aspx?id=42039)

## <a name="hardware-requirements"></a>Requisitos de hardware

Para cada componente da PAM, consulte os requisitos de sistema dos produtos de software.

Para CORPDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) ou anterior

Para CORPWKSTN:

- [Windows 10](https://technet.microsoft.com/windows/dn798752.aspx)

Para PRIVDC:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

Para PAMSRV:

- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) ou [do SQL Server 2014](https://msdn.microsoft.com/library/ms143506(v=sql.120).aspx)
