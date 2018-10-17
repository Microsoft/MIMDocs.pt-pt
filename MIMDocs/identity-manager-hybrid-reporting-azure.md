---
title: O que é criação de relatórios híbridos no Azure AD? | Microsoft Docs
description: Relatórios de atividade de auditoria híbridos no Azure Active Directory permite-lhe ver eventos auditados na cloud e no local.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.suite: ems
ms.openlocfilehash: dd87f00fb3faded60671a47a0ba1dab7e4c2a531
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358181"
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Relatórios no Azure Active Directory de auditoria de gestão de identidade híbrida
Com o Azure Active Directory (Azure AD) auditoria relatórios de atividade, pode monitorizar a atividade de gestão de identidades no local ou na cloud. Ao gerir todos os dados de identidades e acesso num único relatório, pode poupar tempo e reduzir os custos globais.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>O que é a criação de relatórios híbridos do Azure Active Directory?
Ajuda de geração de relatórios híbridos auditoria os profissionais de TI abordar os desafios de geração de relatórios de gestão de identidades comuns, tais como:

* **Recolha atividades de gestão de identidades em sistemas diferentes**. Os relatórios híbridos apresentam a atividade da gestão de identidades do Azure AD e do Identity Manager.

* **Exportar dados de relatórios e criar relatórios personalizados**. Além de visualizar os relatórios no portal do Azure, pode exportar os dados para gerar as suas próprias vistas personalizadas.

* **Reduzindo o custo de infraestrutura sistema de relatórios**. Os relatórios híbridos no cloud significa que pode ajudar a eliminar os custos associados a sua infraestrutura no local, armazém de dados.

## <a name="how-does-it-work"></a>Como é que funciona?

Para recolher os dados no local, instale primeiro um agente de relatórios no servidor do Identity Manager 2016. [Transferir o agente de relatórios híbridos do Microsoft Identity Manager](https://www.microsoft.com/download/details.aspx?id=55112).

Criação de relatórios híbridos passarão pelo seguinte processo:
1. Depois de instalar o agente de relatórios, os dados de atividade do Identity Manager são enviados para o registo de eventos do Windows.
2. O agente de relatórios processa os eventos delta a cada 10 minutos ou quando o serviço de registo de eventos do Windows é reiniciado. O agente, em seguida, carrega os eventos para o portal do Azure.
3. O portal do Azure processa os dados recebidos dentro de uma hora de recebê-los.
4. Os dados de atividade são armazenados no Azure durante um mês.
5. O portal do Azure obtém os dados de relatórios de auditoria e apresenta-o na janela de geração de relatórios de auditoria do Azure.

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre:
- [Trabalhar com a criação de relatórios híbridos do Identity Manager](working-with-identity-manager-hybrid-reporting.md)
- [Relatórios de atividade de auditoria no portal do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Políticas de retenção de relatórios](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Integração de registos do Microsoft Azure (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [O Azure Active Directory reporting API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
