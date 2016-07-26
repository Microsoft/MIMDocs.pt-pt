---
title: "Modelo de camada para criação de partições de privilégios administrativos | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/14/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 9e5f51d5ca731b3564b8262db0f4cddeb850231a
ms.openlocfilehash: 509c05bbda5f0a0b936518fb023000771c45d4f7


---

# Modelo de camada para criação de partições de privilégios administrativos

No ambiente de ameaças atual, a questão não é se um atacante irá obter acesso aos seus sistemas, mas quando. Isto significa que a segurança interna é tão importante como uma defesa de perímetro forte. Este artigo descreve um modelo de segurança destinado a proteger contra a elevação de privilégios segregando as atividades de privilégios elevados das zonas de alto risco. Este modelo fornece uma boa experiência de utilizador ao cumprir as melhores práticas e os princípios de segurança.

## Elevação de Privilégios em florestas do Active Directory

Os utilizadores, serviços ou contas de aplicações às quais são concedidos privilégios administrativos permanentes a florestas do Windows Server Active Directory (AD) introduzem uma quantidade significativa de risco para a missão e o negócio da sua organização. Estas contas são muitas vezes alvos para os pelos atacantes porque, se ficarem comprometidas, o atacante terá o privilégio de se ligar a outros servidores ou aplicações no domínio.

O modelo de camada cria divisões entre os administradores com base nos recursos que gerem. Os administradores com controlo sobre as estações de trabalho do utilizador estão separados dos que controlam as aplicações ou gerem as identidades de empresa. Saiba mais sobre este modelo no [Material de referência para proteger o acesso privilegiado](http://aka.ms/tiermodel).

## Restringir a exposição de credenciais com restrições de início de sessão

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
    - Negar o acesso a este computador a partir da rede  
    - Negar o início de sessão como uma tarefa de lote  
    - Negar o início de sessão como um serviço  
    - Negar o início de sessão localmente  
    - Negar o início de sessão através das definições de Ambiente de Trabalho Remoto  
- Políticas de autenticação e silos, se utilizar o Windows Server 2012 ou posterior
- Autenticação seletiva, se a conta estiver numa floresta administrativa dedicada

O artigo seguinte, [Planear um ambiente bastion](planning-bastion-environment.md), descreve como adicionar uma floresta administrativa dedicada para o Microsoft Identity Manager para estabelecer as contas administrativas.



<!--HONumber=Jun16_HO5-->

