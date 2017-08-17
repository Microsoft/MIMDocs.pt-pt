---
title: Microsoft Identity Manager 2016 | Documentos da Microsoft
description: "O MIM inclui as funcionalidades de gestão de acesso do FIM 2010 e ajuda-o a gerir utilizadores, credenciais, políticas e acesso dentro da sua organização."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b3cdc1a71b6e9eb14a132429ea66bb4ab33fe3c4
ms.sourcegitcommit: 0a78e39976cd03225a8e24a508e9ee23585e67cc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/11/2017
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016
O Microsoft Identity Manager (MIM) 2016 baseia-se nas capacidades de gestão de identidades e acessos do [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx). Tal como o respetivo antecessor, o MIM ajuda-o a gerir os utilizadores, as credenciais, as políticas e o acesso na sua organização.  Além disso, o MIM 2016 adiciona uma experiência híbrida, capacidades de gestão de acesso privilegiado e suporte para novas plataformas.

Esta versão do Microsoft Identity Manager fornece novas funcionalidades, tais como o Privileged Identity Management e o suporte na Gestão de Certificados para o acesso à API REST. Na gestão de certificados, não há agora suporte adicionado para topologias de várias florestas, uma aplicação do Windows para gestão de ciclo de vida do certificado e de smart card virtual atualizado eventos e capacidades de resolução de problemas. Os cenários autónomos incluem agora o Desbloqueio de Conta e acesso ao MFA do Azure (autenticação multifator) para a Reposição da Palavra-passe.

## <a name="hybrid-experience"></a>Experiência híbrida
O Microsoft Identity Manager 2016 funciona em conjunto com o Azure AD para lhe dar controlo sobre o seu ambiente completo. Os relatórios híbridos no Azure AD apresentam os seus dados da cloud e do local no mesmo sítio. Além disso, o portal Reposição Personalizada de Palavra-passe suporta o Multi-Factor Authentication (MFA) do Azure.

## <a name="privileged-identity-management"></a>Privileged Identity Management
O Privileged Identity Management controla e gere o acesso administrativo ao conceder acesso temporário e baseado em tarefas a recursos confidenciais. Isto significa que pode dar aos utilizadores apenas a permissão necessária, o que reduz as possibilidades de um atacante informático obter acesso administrativo total. Além disso, o Privileged Identity Management extrai e isola as contas administrativas de florestas do Active Directory existentes.

O MIM suporta uma solução de Privileged Identity Management no local para gerir Active Directory. Para começar, [Utilize o Privileged Access Management](./pam/privileged-identity-management-for-active-directory-domain-services.md).

## <a name="related-topics"></a>Tópicos relacionados

- Microsoft Identity Manager está ainda intimamente relacionado com o respetivo predecessor, o Forefront Identity Manager. Se ainda utilizar FIM ou desejar referir-se a documentação adicional, observe o [Mapa de Documentação do FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Considerações sobre a topologia da implementação do MIM](topology-considerations.md)
- [Guia de planeamento de capacidade](capacity-planning-guide.md)