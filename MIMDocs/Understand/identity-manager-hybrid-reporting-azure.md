---
title: "Relatórios de gestão de identidade híbrida | Microsoft Identity Manager"
description: "A criação de relatórios híbridos do Azure Active Directory permite-lhe criar relatórios personalizados que incluem eventos na nuvem e no local."
keywords: 
author: kgremban
manager: stevenpo
ms.date: 05/13/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 7e61e201b277a2e8ec9fee785e9e34fca3b1cb29
ms.openlocfilehash: b3f3982ade46932b18fb730fe5c16d52cde188a1


---

# Relatórios de gestão de identidade híbrida no Azure
Com o Azure Active Directory (AD), pode criar um único relatório para monitorizar a atividade da gestão de identidades que ocorre no local ou na nuvem. Esta funcionalidade permite gerir todos os dados de identidade e acesso num único local, poupando tempo e reduzindo os custos globais.

## O que é a criação de relatórios híbridos do Azure AD?
A criação de relatórios híbridos ajuda os profissionais de TI a resolver desafios comuns relacionados com os relatórios de gestão de identidades.

1. **Recolha atividades de gestão de identidades em sistemas diferentes.** Os relatórios híbridos apresentam a atividade da gestão de identidades do Azure AD e do Identity Manager.

2. **Exporte os dados de relatórios e crie relatórios personalizados.** Além de visualizar os seus relatórios no portal do Azure, também pode exportar os dados para gerar as suas próprias vistas personalizadas.

3. **Reduza o custo da infraestrutura do sistema de relatórios.** A criação de relatórios híbridos na nuvem significa que pode eliminar a infraestrutura do armazém de dados dos relatórios no local.

## Como é que funciona?

Para recolher os dados no local, instale primeiro um agente de relatórios no servidor do Identity Manager. O agente de relatórios é transferido da página de configuração do seu diretório no [Portal clássico do Azure](https://manage.windowsazure.com/).

O processo de criação de relatórios híbridos segue estes passos:
1. Após instalar o agente de relatórios, os dados de atividade do Identity Manager são enviados para o Registo de Eventos do Windows.
2. O agente de relatórios processa os eventos no Registo de Eventos do Windows e carrega-os para o Azure.
3. Os dados de atividade são armazenados no Azure durante um mês.
4. Quando solicita um relatório, os eventos de atividade são analisados e filtrados para os relatórios necessários.
5. O portal clássico do Azure obtém os dados de relatórios e apresenta-os como o relatório de atividades.

## Consulte também
- Obter mais detalhes sobre como [Trabalhar com a Criação de Relatórios Híbridos do Identity Manager](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)



<!--HONumber=Jun16_HO4-->


