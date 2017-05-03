---
title: "O que é a criação de relatórios híbridos | Documentos da Microsoft"
description: "Os relatórios híbridos de Atividade de Auditoria no Azure Active Directory permitem-lhe ver eventos auditados na cloud e no local."
keywords: 
author: kgremban
ms.author: fimguy
manager: femila
ms.date: 04/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3144ee195675df5dc120896cc801a7124ee12214
ms.openlocfilehash: 8ca0af93f2d72ccf2e314b20d13323b631eb02bc
ms.lasthandoff: 04/27/2017


---

# <a name="hybrid-identity-management-audit-reports-in-azure-active-directory"></a>Relatórios híbridos de auditoria da gestão de identidades no Azure Active Directory
Com os relatórios de Atividade de Auditoria do Azure Active Directory (AD) pode ver um único relatório para monitorizar a atividade de gestão de identidades que ocorre no local ou na cloud. Esta funcionalidade permite gerir todos os dados de identidade e acesso num único local, poupando tempo e reduzindo os custos globais.

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>O que é a criação de relatórios híbridos do Azure Active Directory?
A criação de relatórios híbridos de auditoria ajuda os profissionais de TI a resolver desafios comuns relacionados com os relatórios de gestão de identidades.

1. **Recolha atividades de gestão de identidades em sistemas diferentes.** Os relatórios híbridos apresentam a atividade da gestão de identidades do Azure AD e do Identity Manager.

2. **Exporte os dados de relatórios e crie relatórios personalizados.** Além de visualizar os seus relatórios no portal do Azure, também pode exportar os dados para gerar as suas próprias vistas personalizadas.

3. **Reduza o custo da infraestrutura do sistema de relatórios.** A criação de relatórios híbridos na cloud significa que pode eliminar a infraestrutura do armazém de dados dos relatórios no local.

## <a name="how-does-it-work"></a>Como é que funciona?

Para recolher os dados no local, instale primeiro um agente de relatórios no servidor do Identity Manager 2016. O agente de relatórios é transferido a partir da Página de Transferências da Microsoft [aqui](https://www.microsoft.com/en-us/download/details.aspx?id=####/).

O processo de criação de relatórios híbridos segue estes passos:
1. Após instalar o agente de relatórios, os dados de atividade do Identity Manager são enviados para o Registo de Eventos do Windows.
2. O agente de relatórios processa os eventos no Registo de Eventos do Windows e carrega-os para o Portal do Azure.
3. Os dados de atividade são armazenados no Azure durante um mês.
4. Quando solicita um relatório, os eventos de atividade são analisados e filtrados para os relatórios necessários.
5. O portal do Azure obtém os dados dos relatórios de auditoria e apresenta-os como a auditoria no relatório de atividade.

## <a name="see-also"></a>Consulte também
- Obter mais detalhes sobre como [Trabalhar com a Criação de Relatórios Híbridos do Identity Manager](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)
- Obter mais detalhes sobre os [Relatórios de atividade de auditoria no portal do Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-audit-logs)

