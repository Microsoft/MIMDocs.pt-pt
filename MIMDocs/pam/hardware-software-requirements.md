---
title: Requisitos de software do PAM | Documentos da Microsoft
description: "Localize os requisitos de hardware e software para uma implementação de Privileged Access Management com êxito"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2985215821db843d2f90d8a34250a8ca6a84b592
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# Requisitos de hardware e software
<a id="hardware-and-software-requirements" class="xliff"></a>

A Privileged Access Management não tem requisitos de hardware para além das plataformas de software subjacentes. Basta certificar-se de que tem suficiente memória ou espaço em disco e conectividade de rede.

Este artigo fornece os requisitos mínimos para uma implementação básica. Não se destina a demonstrar elevada disponibilidade, desempenho ou escalabilidade, e não representa uma topologia de implementação recomendada para grandes empresas ou ambientes de produção.

## Instalar a partir de pacotes de software
<a id="installing-from-software-packages" class="xliff"></a>

O software seguinte pode ser transferido a partir do Centro de Avaliação TechNet ou MSDN:  
- Microsoft Identity Manager 2016
  - Serviço e Portal: contém o instalador para o Serviço do MIM e para o Portal do MIM, e para o Cenário de PAM
  - Suplementos e Extensões: contém o instalador para os cmdlets do PowerShell requerentes

O software seguinte pode ser transferido a partir do GitHub:  
- PAMSamplePortal: contém a aplicação de web de amostra para a REST API

## Software necessário
<a id="required-software" class="xliff"></a>

- Windows Server 2012 R2  
- Windows 8.1 Enterprise ou Windows 10 Enterprise  
- SQL Server 2012 Service Pack 1 ou SQL Server 2014  

## Software de avaliação
<a id="evaluation-software" class="xliff"></a>

Caso não tenha licenças para o Windows, para o SQL Server ou para o Windows Server, pode transferir as versões de avaliação.

### Centro de avaliação TechNet
<a id="technet-evaluation-center" class="xliff"></a>

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)  
- [Windows 8.1 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-8-1-enterprise)  
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)  

### Centro de Transferências da Microsoft
<a id="microsoft-download-center" class="xliff"></a>

- [SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 e pré-requisitos](https://www.microsoft.com/download/details.aspx?id=42039)

## Requisitos de hardware
<a id="hardware-requirements" class="xliff"></a>

Para cada componente da PAM, consulte os requisitos de sistema dos produtos de software.

Para CORPDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) ou anterior

Para CORPWKSTN:  
- [Windows 8.1](http://windows.microsoft.com/windows-8/system-requirements)

Para PRIVDC:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

Para PAMSRV:
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)  
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) ou [do SQL Server 2014](https://msdn.microsoft.com/en-us/library/ms143506(v=sql.120).aspx)
