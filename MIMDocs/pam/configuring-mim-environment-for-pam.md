---
title: Configurar o Ambiente de MIM Privileged Access Management | Microsoft Identity Manager
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: 9cf126d898c93faf89d7119136cce4e4963bb63d
ms.openlocfilehash: c9f2cf2ba1f42ea1513ae38d8089839d85ae5553


---

# Configurar o Ambiente de MIM para Privileged Access Management
Existem sete passos para concluir ao configurar o ambiente para o acesso entre florestas, instalar e configurar o Active Directory e o Microsoft Identity Manager, e demonstrar um pedido de acesso just-in-time.

Estes passos são apresentados para que possa começar do zero e criar um ambiente de teste. Se estiver a aplicar a PAM num ambiente existente, pode utilizar os controladores de domínio ou contas de utilizador próprios em vez de criar novos para corresponder aos exemplos.

1.  Preparar o servidor *CORPDC* como um controlador de domínio e o *CORPWKSTN* como uma estação de trabalho de membro.

2.  Preparar o servidor *PRIVDC* como um controlador de domínio.

3.  Preparar o servidor *PAMSRV* na floresta *PRIV*.

4.  Instalar componentes de MIM *PAMSRV* e os cmdlets numa estação de trabalho de membro de floresta *CONTOSO* e prepará-los para Privileged Access Management.

5.  Estabelecer fidedignidade entre o *PRIV* e as florestas *CONTOSO*.

6.  Preparar grupos de segurança com privilégios com acesso a recursos protegidos e contas de membro para Just-in-time Privileged Access Management.

7.  Demonstram a utilização de pedidos, receções e execuções de acesso elevado privilegiado a um recurso protegido.

>[!div class="step-by-step"]
[[!div class="step-by-step"] [ Iniciar »](step-1-prepare-corp-domain.md)](step-1-prepare-corp-domain.md)



<!--HONumber=Jul16_HO2-->

