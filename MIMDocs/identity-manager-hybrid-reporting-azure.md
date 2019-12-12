---
title: O que é relatório híbrido no Azure AD? | Documentos da Microsoft
description: Os relatórios de atividade de auditoria híbrida no Azure Active Directory permitem exibir eventos auditados na nuvem e no local.
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
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "64516766"
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory"></a>Relatório de auditoria do gerenciamento de identidade híbrida no Azure Active Directory
Com o relatório de atividades de auditoria do Azure Active Directory (Azure AD), você pode monitorar a atividade de gerenciamento de identidade local ou na nuvem. Ao gerenciar todos os dados de identidade e acesso em um único relatório, você pode economizar tempo e reduzir os custos gerais.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>O que é a criação de relatórios híbridos do Azure Active Directory?
O relatório de auditoria híbrida ajuda os profissionais de ti a enfrentar os desafios comuns de relatórios de gerenciamento de identidades, como:

* **Coletando atividades de gerenciamento de identidades em diferentes sistemas**. Os relatórios híbridos apresentam a atividade da gestão de identidades do Azure AD e do Identity Manager.

* **Exportando dados de relatórios e criando relatórios personalizados**. Além de exibir seus relatórios no portal do Azure, você pode exportar os dados para gerar seus próprios modos de exibição personalizados.

* **Redução do custo da infraestrutura do sistema de relatórios**. O relatório híbrido na nuvem significa que você pode ajudar a eliminar os custos associados à sua infraestrutura de data warehouse local.

## <a name="how-does-it-work"></a>Como é que funciona?

Para recolher os dados no local, instale primeiro um agente de relatórios no servidor do Identity Manager 2016. [Baixe o Microsoft Identity Manager agente de relatório híbrido](https://www.microsoft.com/download/details.aspx?id=55112).

O relatório híbrido passa pelo seguinte processo:
1. Depois de instalar o agente de relatório, os dados da atividade do Identity Manager são enviados ao log de eventos do Windows.
2. O agente de relatório processa os eventos Delta a cada 10 minutos ou quando o serviço de log de eventos do Windows é reiniciado. Em seguida, o agente carrega os eventos no portal do Azure.
3. O portal do Azure processa os dados recebidos dentro de uma hora após o recebimento.
4. Os dados de atividade são armazenados no Azure durante um mês.
5. O portal do Azure recupera os dados de relatório de auditoria e os exibe na janela relatório de auditoria do Azure.

## <a name="next-steps"></a>Próximos passos
Saiba mais sobre:
- [Trabalhando com o relatório híbrido do Identity Manager](working-with-identity-manager-hybrid-reporting.md)
- [Relatórios de atividade de auditoria no portal do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [Relatando políticas de retenção](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [SIEM (integração de log de Microsoft Azure)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [API de relatório do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
