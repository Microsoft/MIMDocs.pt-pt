---
title: Modelo de camada do ambiente do PAM | Documentos da Microsoft
description: Saiba mais sobre o modelo de camada que segrega o sistema com base na vulnerabilidade a riscos.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/30/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8e7b7217714f0ef74c1d959eb51dac07018d6e77
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "64517841"
---
# <a name="tier-model-for-partitioning-administrative-privileges"></a>Modelo de camada para criação de partições de privilégios administrativos

Este artigo descreve um modelo de segurança destinado a proteger contra a elevação de privilégios segregando as atividades de privilégios elevados das zonas de alto risco. Este modelo fornece uma boa experiência de utilizador ao cumprir as melhores práticas e os princípios de segurança.

## <a name="elevation-of-privilege-in-active-directory-forests"></a>Elevação de Privilégios em florestas do Active Directory

Os utilizadores, serviços ou contas de aplicações às quais são concedidos privilégios administrativos permanentes a florestas do Windows Server Active Directory (AD) introduzem uma quantidade significativa de risco para a missão e o negócio da sua organização. Essas contas são geralmente direcionadas aos invasores porque, se estiverem comprometidas, o invasor tem direitos para se conectar a outros servidores ou aplicativos no domínio.

O modelo de camada cria divisões entre os administradores com base nos recursos que gerem. Os administradores com controlo sobre as estações de trabalho do utilizador estão separados dos que controlam as aplicações ou gerem as identidades de empresa. Saiba mais sobre este modelo no [Material de referência para proteger o acesso privilegiado](http://aka.ms/tiermodel).

## <a name="restricting-credential-exposure-with-logon-restrictions"></a>Restringir a exposição de credenciais com restrições de início de sessão

A redução do risco de roubo de credenciais para contas administrativas normalmente requer a reformulação das práticas administrativas para limitar a exposição aos atacantes. Como primeiro passo, as organizações são aconselhadas a:

- Limitar o número de anfitriões nos quais as credenciais administrativas estão expostas.
- Limitar os privilégios de função para o mínimo requerido.
- Assegurar que as tarefas administrativas não são efetuadas em anfitriões utilizados para atividades de utilizador padrão (por exemplo, e-mail e navegação na Web).

O passo seguinte consiste em implementar restrições de início de sessão e ativar processos e melhores práticas para cumprir os requisitos do modelo de camada. Idealmente, a exposição de credenciais também deve ser reduzida para o menor privilégio necessário para a função em cada camada.

As restrições de início de sessão devem ser impostas para assegurar que as contas privilegiadas não têm acesso a recursos menos seguros. Por exemplo:

- Os admins de domínio (camada 0) não podem iniciar sessão nos servidores da empresa (camada 1) e estações de trabalho de utilizador padrão (camada 2).
- Os administradores de servidor (camada 1) não podem iniciar sessão em estações de trabalho de utilizador padrão (camada 2).

>[!NOTE]
> Os administradores de servidor não devem estar no grupo de administradores de domínio. Devem ser dadas contas separadas ao pessoal com responsabilidades para gerir controladores de domínio e servidores empresariais.

As restrições de início de sessão podem ser impostas com:

- Restrições de Direitos de Início de Sessão da Política de Grupo, incluindo:
    - Recusar acesso a este computador à rede
    - Negar o início de sessão como uma tarefa de lote
    - Negar o início de sessão como um serviço
    - Negar o início de sessão localmente
    - Negar o início de sessão através das definições de Ambiente de Trabalho Remoto  
- Políticas de autenticação e silos, se utilizar o Windows Server 2012 ou posterior
- Autenticação seletiva, se a conta estiver numa floresta administrativa dedicada

## <a name="next-steps"></a>Próximos passos

- O artigo seguinte, [Planear um ambiente bastion](planning-bastion-environment.md), descreve como adicionar uma floresta administrativa dedicada para o Microsoft Identity Manager para estabelecer as contas administrativas.
- As [estações de trabalho de acesso privilegiado](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations) fornecem um sistema operacional dedicado para tarefas confidenciais que são protegidas contra ataques da Internet e vetores de ameaça.
