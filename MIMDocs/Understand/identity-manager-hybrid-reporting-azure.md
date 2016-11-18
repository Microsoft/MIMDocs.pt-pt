---
title: "O que é a criação de relatórios híbridos | Documentos da Microsoft"
description: "A criação de relatórios híbridos do Azure Active Directory permite-lhe criar relatórios personalizados que incluem eventos na nuvem e no local."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: f21c15fdaa5fba9176cfc60a3c49017fa97fa935


---

# <a name="hybrid-identity-management-reports-in-azure"></a>Relatórios de gestão de identidade híbrida no Azure
Com o Azure Active Directory (AD), pode criar um único relatório para monitorizar a atividade da gestão de identidades que ocorre no local ou na nuvem. Esta funcionalidade permite gerir todos os dados de identidade e acesso num único local, poupando tempo e reduzindo os custos globais.

## <a name="what-is-azure-ad-hybrid-reporting"></a>O que é a criação de relatórios híbridos do Azure AD?
A criação de relatórios híbridos ajuda os profissionais de TI a resolver desafios comuns relacionados com os relatórios de gestão de identidades.

1. **Recolha atividades de gestão de identidades em sistemas diferentes.** Os relatórios híbridos apresentam a atividade da gestão de identidades do Azure AD e do Identity Manager.

2. **Exporte os dados de relatórios e crie relatórios personalizados.** Além de visualizar os seus relatórios no portal do Azure, também pode exportar os dados para gerar as suas próprias vistas personalizadas.

3. **Reduza o custo da infraestrutura do sistema de relatórios.** A criação de relatórios híbridos na nuvem significa que pode eliminar a infraestrutura do armazém de dados dos relatórios no local.

## <a name="how-does-it-work"></a>Como é que funciona?

Para recolher os dados no local, instale primeiro um agente de relatórios no servidor do Identity Manager. O agente de relatórios é transferido da página de configuração do seu diretório no [Portal clássico do Azure](https://manage.windowsazure.com/).

O processo de criação de relatórios híbridos segue estes passos:
1. Após instalar o agente de relatórios, os dados de atividade do Identity Manager são enviados para o Registo de Eventos do Windows.
2. O agente de relatórios processa os eventos no Registo de Eventos do Windows e carrega-os para o Azure.
3. Os dados de atividade são armazenados no Azure durante um mês.
4. Quando solicita um relatório, os eventos de atividade são analisados e filtrados para os relatórios necessários.
5. O portal clássico do Azure obtém os dados de relatórios e apresenta-os como o relatório de atividades.

## <a name="see-also"></a>Consulte também
- Obter mais detalhes sobre como [Trabalhar com a Criação de Relatórios Híbridos do Identity Manager](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)



<!--HONumber=Nov16_HO2-->


