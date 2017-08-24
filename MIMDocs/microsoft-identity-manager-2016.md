---
title: Microsoft Identity Manager 2016 | Documentos da Microsoft
description: "O MIM inclui as funcionalidades de gestão de acesso do FIM 2010 e ajuda-o a gerir utilizadores, credenciais, políticas e acesso dentro da sua organização."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 21bb12a70850a5f835ca6715d9683558ac6fad1d
ms.sourcegitcommit: f2778c5fa5f0cd04e8a74fc15fa340cd118dded5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2017
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016

O Microsoft Identity Manager (MIM) 2016 baseia-se nas capacidades de gestão de identidades e acessos do [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx). Tal como o respetivo antecessor, o MIM ajuda-o a gerir os utilizadores, as credenciais, as políticas e o acesso na sua organização.  Além disso, o MIM 2016 adiciona uma experiência híbrida, capacidades de gestão de acesso privilegiado e suporte para novas plataformas.

Para além das funcionalidades de gestão de identidade existentes incluída no [FIM](https://technet.microsoft.com/library/jj133868). O MIM 2016 fornece novas funcionalidades e melhoramentos, tais como:

- Privileged Identity Management
- Nova funcionalidade na gestão de certificados
  - [Referência à API REST da Gestão de Certificados](./reference/certificate-management-rest-api-reference.md)
  - Suporte para topologias de múltiplas florestas.
  - Uma aplicação do Windows para smart card virtual
  - Eventos atualizados e capacidades de resolução de problemas. 
- Os cenários autónomos incluem agora o Desbloqueio de Conta e acesso ao MFA do Azure (autenticação multifator) para a Reposição da Palavra-passe.

## <a name="hybrid-experience"></a>Experiência híbrida

Microsoft Identity Manager 2016 funciona em conjunto com [do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) para lhe dar controlo sobre o seu ambiente completo. Os relatórios híbridos no Azure AD apresentam os seus dados da cloud e do local no mesmo sítio. Além disso, o [portal reposição personalizada de palavra-passe](working-with-self-service-password-reset.md) suporta Azure autenticação multifator (MFA).

## <a name="privileged-identity-management"></a>Privileged Identity Management

O Privileged Identity Management controla e gere o acesso administrativo ao conceder acesso temporário e baseado em tarefas a recursos confidenciais. Isto significa que pode dar aos utilizadores apenas a permissão necessária, o que reduz as possibilidades de um atacante informático obter acesso administrativo total. Além disso, o Privileged Identity Management extrai e isola as contas administrativas de florestas do Active Directory existentes.

O MIM suporta uma solução de Privileged Identity Management no local para gerir Active Directory. Para começar, [Utilize o Privileged Access Management](./pam/privileged-identity-management-for-active-directory-domain-services.md).

## <a name="related-topics"></a>Tópicos relacionados

- Microsoft Identity Manager está ainda intimamente relacionado com o respetivo predecessor, o Forefront Identity Manager. Se ainda utilizar FIM ou desejar referir-se a documentação adicional, observe o [Mapa de Documentação do FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Considerações sobre a topologia de implementação de MIM](topology-considerations.md) este artigo apresenta várias topologias de implementação que pode considerar implementar.
- [Guia de planeamento de capacidade](capacity-planning-guide.md) pode utilizar este guia, juntamente com ambientes de teste, para compreender o âmbito adequado para a sua implementação.