---
title: "O que é a criação de relatórios híbridos | Documentos da Microsoft"
description: "Os relatórios híbridos de Atividade de Auditoria no Azure Active Directory permitem-lhe ver eventos auditados na cloud e no local."
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
ms.openlocfilehash: ffe372c273aae55278f9b18b45b65425734aa6f7
ms.sourcegitcommit: e52bab207117390997c6fa8450de24335b502673
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/20/2017
---
# <a name="hybrid-identity-management-audit-reports-in-azure-active-directory---public-previewrefresh"></a>Relatórios híbridos de Auditoria da gestão de identidades no Azure Active Directory – Pré-visualização Pública (Atualização)
Com os relatórios de Atividade de Auditoria do Azure Active Directory (AD) pode ver um único relatório para monitorizar a atividade de gestão de identidades que ocorre no local ou na cloud. Esta funcionalidade permite gerir todos os dados de identidade e acesso num único local, poupando tempo e reduzindo os custos globais.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>O que é a criação de relatórios híbridos do Azure Active Directory?
A criação de relatórios híbridos de auditoria ajuda os profissionais de TI a resolver desafios comuns relacionados com os relatórios de gestão de identidades.

1. **Recolha atividades de gestão de identidades em sistemas diferentes.** Os relatórios híbridos apresentam a atividade da gestão de identidades do Azure AD e do Identity Manager.

2. **Exporte os dados de relatórios e crie relatórios personalizados.** Além de visualizar os seus relatórios no portal do Azure, também pode exportar os dados para gerar as suas próprias vistas personalizadas.

3. **Reduza o custo da infraestrutura do sistema de relatórios.** A criação de relatórios híbridos na cloud significa que pode eliminar a infraestrutura do armazém de dados dos relatórios no local.

## <a name="how-does-it-work"></a>Como é que funciona?

Para recolher os dados no local, instale primeiro um agente de relatórios no servidor do Identity Manager 2016. O agente de relatórios é transferido a partir da Página de Transferências da Microsoft [aqui](https://www.microsoft.com/download/details.aspx?id=55112).

O processo de criação de relatórios híbridos segue estes passos:
1. Após instalar o agente de relatórios, os dados de atividade do Identity Manager são enviados para o Registo de Eventos do Windows.
2. O agente de relatórios processa os eventos delta de dez em dez minutos ou durante o reinício do sistema no Registo de Eventos do Windows e carrega-os para o Portal do Azure.
3. O Portal do Azure processa os dados recebidos no prazo de uma hora após a receção.
4. Os dados de atividade são armazenados no Azure durante um mês.
5. O portal do Azure obtém os dados dos relatórios de auditoria e apresenta-os como a auditoria no Painel Relatórios de Auditoria do Azure.

## <a name="next-steps"></a>Passos Seguintes
- Obter mais detalhes sobre como [Trabalhar com a Criação de Relatórios Híbridos do Identity Manager](working-with-identity-manager-hybrid-reporting.md)
- Obter mais detalhes sobre os [Relatórios de atividade de auditoria no portal do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- Obter mais detalhes [políticas de retenção de relatórios](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- Obter mais detalhes [integração de registo do Microsoft Azure (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- Obter mais detalhes [Azure Active Directory API do relatório](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
