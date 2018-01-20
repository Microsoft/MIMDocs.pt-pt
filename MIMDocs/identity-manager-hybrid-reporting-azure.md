---
title: "O que é de relatórios híbridos no Azure AD? | Documentos da Microsoft"
description: "Auditoria atividade criação de relatórios híbridos no Azure Active Directory permite-lhe ver eventos auditados na nuvem e no local."
keywords: 
author: fimguy
ms.author: fimguy
manager: bhu
ms.date: 09/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: fimguy
ms.suite: ems
ms.openlocfilehash: e2391be3d05f61335c134c104673a31ad7fc3830
ms.sourcegitcommit: 3d8a2493eae1218bfdb75a399ffa4adc8c2a8fdf
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/20/2018
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory-public-preview-refresh"></a>Gestão de identidade híbrida de auditoria de relatórios na atualização do Azure Active Directory pré-visualização pública
Com o Azure Active Directory (Azure AD) auditoria atividade reporting, pode monitorizar a atividade da gestão de identidades no local ou na nuvem. Ao gerir todos os dados identidades e acessos num único relatório, pode poupar tempo e reduzir os custos globais.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>O que é a criação de relatórios híbridos do Azure Active Directory?
Ajuda de relatórios híbridos auditoria profissionais de TI abordar os desafios de relatórios de gestão de identidades comuns, tais como:

* **Recolha atividades de gestão de identidades em sistemas diferentes**. Os relatórios híbridos apresentam a atividade da gestão de identidades do Azure AD e do Identity Manager.

* **Exportar dados de relatórios e criar relatórios personalizados**. Para além de visualizar os seus relatórios no portal do Azure, pode exportar os dados para gerar as suas próprias vistas personalizadas.

* **Reduzir o custo da infraestrutura de sistema relatórios**. Os relatórios híbridos no nuvem significa que pode ajudar a eliminar os custos associados a sua infraestrutura no local, do armazém de dados.

## <a name="how-does-it-work"></a>Como é que funciona?

Para recolher os dados no local, instale primeiro um agente de relatórios no servidor do Identity Manager 2016. [Transferir o agente de relatórios híbridos do Microsoft Identity Manager](https://www.microsoft.com/download/details.aspx?id=55112).

Criação de relatórios híbridos sofre o seguinte processo:
1. Depois de instalar o agente de relatórios, os dados de atividade do Identity Manager são enviados para o registo de eventos do Windows.
2. O agente de relatórios processa os eventos de delta a cada 10 minutos ou quando o serviço de registo de eventos do Windows é reiniciado. O agente, em seguida, carrega os eventos para o portal do Azure.
3. O portal do Azure processa os dados recebidos dentro de uma hora de receção.
4. Os dados de atividade são armazenados no Azure durante um mês.
5. O portal do Azure obtém a auditoria de dados de relatórios e apresenta-o na janela do relatório de auditoria do Azure.

## <a name="next-steps"></a>Próximos passos
Saiba mais sobre:
- [Trabalhar com os relatórios híbridos do Identity Manager](working-with-identity-manager-hybrid-reporting.md)
- [Relatórios de atividade de auditoria no portal do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [As políticas de retenção de relatórios](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Integração de registo do Microsoft Azure (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [Azure Active Directory API do relatório](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
