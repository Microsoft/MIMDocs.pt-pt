---
title: Microsoft Identity Manager 2016 | Documentos da Microsoft
description: O MIM inclui as funcionalidades de gestão de acesso do FIM 2010 e ajuda-o a gerir utilizadores, credenciais, políticas e acesso dentro da sua organização.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: de00cc6da8c431519c9bdef3a2e97a8333cbb4ac
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332294"
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016

O Microsoft Identity Manager (MIM) 2016 baseia-se nas capacidades de gestão de identidades e acessos do [FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx). Tal como o respetivo antecessor, o MIM ajuda-o a gerir os utilizadores, as credenciais, as políticas e o acesso na sua organização.  Além disso, o MIM 2016 adiciona uma experiência híbrida, capacidades de gestão de acesso privilegiado e suporte para novas plataformas.

Além da funcionalidade de gestão de identidade existente incluída no [FIM](https://technet.microsoft.com/library/jj133868). O MIM 2016 fornece aprimoramentos e novos recursos, tais como:

- Privileged Identity Management
- Novas funcionalidades na gestão de certificados
  - [Referência à API REST da Gestão de Certificados](./reference/certificate-management-rest-api-reference.md)
  - Suporte para topologias de múltiplas florestas.
  - [Uma aplicação do Windows para smart card virtual](working-with-mim-certificate-manager.md)
  - Eventos atualizados e capacidades de resolução de problemas. 
- [Cenários de Self-serviços](working-with-self-service-password-reset.md) incluem agora o desbloqueio de conta e o MFA do Azure a porta (autenticação multifator) de reposição de palavra-passe.

## <a name="hybrid-experience"></a>Experiência híbrida

Microsoft Identity Manager 2016 funciona em conjunto com [do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) para lhe dar controlo sobre o seu ambiente completo. Os relatórios híbridos no Azure AD apresentam os seus dados da cloud e do local no mesmo sítio. Além disso, o [portal de reposição personalizada de palavra-passe](working-with-self-service-password-reset.md) suporta o multi-factor authentication (MFA).

## <a name="privileged-identity-management"></a>Privileged Identity Management

O Privileged Identity Management controla e gere o acesso administrativo ao conceder acesso temporário e baseado em tarefas a recursos confidenciais. Isto significa que pode dar aos utilizadores apenas a permissão necessária, o que reduz as possibilidades de um atacante informático obter acesso administrativo total. Além disso, o Privileged Identity Management extrai e isola as contas administrativas de florestas do Active Directory existentes.

O MIM suporta uma solução de Privileged Identity Management no local para gerir Active Directory. Para começar, [Utilize o Privileged Access Management](./pam/privileged-identity-management-for-active-directory-domain-services.md).

## <a name="related-topics"></a>Tópicos relacionados

- Microsoft Identity Manager está ainda intimamente relacionado com o respetivo predecessor, o Forefront Identity Manager. Se ainda utilizar FIM ou desejar referir-se a documentação adicional, observe o [Mapa de Documentação do FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Considerações sobre a topologia da implementação de MIM](topology-considerations.md) este artigo apresenta várias topologias de implementação que pode considerar implementar.
- [Guia de planeamento de capacidade](capacity-planning-guide.md) pode utilizar este guia, juntamente com ambientes de teste, para compreender o âmbito adequado para a sua implementação.