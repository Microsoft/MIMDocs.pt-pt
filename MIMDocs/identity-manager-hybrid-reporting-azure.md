---
title: O que é reportagem híbrida em Azure AD? | Documentos da Microsoft
description: Os relatórios de atividades de auditoria híbrida sintetizadas no Azure Ative Directory permitem-lhe visualizar eventos auditados tanto na nuvem como no local.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 02/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.suite: ems
ms.openlocfilehash: 6f4f2aea998fc5682d1fb21d77e4d4f1c582d770
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79042156"
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Relatório de auditoria de gestão de identidade híbrida no Diretório Ativo do Azure
Com o relatório de atividade de auditoria do Azure Ative Directory (Azure AD), pode monitorizar a atividade de gestão de identidade no local ou na nuvem. Ao gerir todos os seus dados de identidade e acesso num único relatório, pode economizar tempo e reduzir os custos globais.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>O que é a criação de relatórios híbridos do Azure Active Directory?
O relatório de auditoria híbrida ajuda os profissionais de TI a enfrentar desafios comuns de relatórios de gestão de identidade, tais como:

* **Recolha de atividades de gestão de identidade em diferentes sistemas.** Os relatórios híbridos apresentam a atividade da gestão de identidades do Azure AD e do Identity Manager.

* **Exportação de dados de reporte e criação de relatórios personalizados.** Além de ver os seus relatórios no portal Azure, pode exportar os dados para gerar as suas próprias vistas personalizadas.

* **Redução dos custos de infraestrutura do sistema de reporte.** Relatórios híbridos na nuvem significa que você pode ajudar a eliminar os custos associados com o seu local, infraestrutura de armazém de dados.

## <a name="how-does-it-work"></a>Como é que funciona?

Para recolher os dados no local, instale primeiro um agente de relatórios no servidor do Identity Manager 2016. [Baixe o Agente de Reporte Híbrido do Gestor de Identidade da Microsoft.](https://www.microsoft.com/download/details.aspx?id=55112)

O relatório híbrido passa pelo seguinte processo:
1. Depois de instalar o agente de reporte, os dados de atividade do Gestor de Identidade são enviados para o Windows Event Log.
2. O agente de reporte processa os eventos delta a cada 10 minutos ou quando o serviço de Registo de Eventos do Windows reinicia. Em seguida, o agente envia os eventos para o portal Azure.
3. O portal Azure processa os dados recebidos no prazo de uma hora após a sua receção.
4. Os dados de atividade são armazenados no Azure durante um mês.
5. O portal Azure recupera os dados de relatórios de auditoria e exibe-os na janela de Relatórios de Auditoria do Azure.

## <a name="next-steps"></a>Próximos passos
Saiba mais sobre:
- [Trabalhar com o Gestor de Identidade Relatório Híbrido](working-with-identity-manager-hybrid-reporting.md)
- [Relatórios de atividades de auditoria no portal azure Ative Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Políticas de retenção de relatórios](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Integração de log microsoft Azure (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [Diretório Ativo Azure reportando API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
