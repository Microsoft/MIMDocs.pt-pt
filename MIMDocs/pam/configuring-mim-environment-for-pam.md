---
title: Configurar o MIM 2016 para Privileged Access Management | Documentos da Microsoft
description: "Plano para a instalação e MIM e configuração da mesma para Privileged Access Management."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/31/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 79de8ef08b49a41638a09e829efbd8c17301004b
ms.sourcegitcommit: 210195369d2ecd610569d57d0f519d683ea6a13b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/01/2017
---
# <a name="configure-the-mim-environment-for-privileged-access-management"></a>Configuração do ambiente de MIM para Privileged Access Management

Existem sete passos para concluir ao configurar o ambiente para o acesso entre florestas, instalar e configurar o Active Directory e o Microsoft Identity Manager, e demonstrar um pedido de acesso just-in-time.

Estes passos são apresentados para que possa começar do zero e criar um ambiente de teste. Se estiver a aplicar a PAM num ambiente existente, pode utilizar os controladores de domínio ou contas de utilizador próprios em vez de criar novos para corresponder aos exemplos.

1. Preparar o servidor *CORPDC* como um controlador de domínio e o *CORPWKSTN* como uma estação de trabalho de membro.

2. Preparar o servidor *PRIVDC* como um controlador de domínio.

3.  Preparar o servidor *PAMSRV* na floresta *PRIV*.

4.  Instalar componentes de MIM *PAMSRV* e os cmdlets numa estação de trabalho de membro de floresta *CONTOSO* e prepará-los para Privileged Access Management.

5.  Estabelecer fidedignidade entre o *PRIV* e as florestas *CONTOSO*.

6.  Preparar grupos de segurança com privilégios com acesso a recursos protegidos e contas de membro para Just-in-time Privileged Access Management.

7.  Demonstram a utilização de pedidos, receções e execuções de acesso elevado privilegiado a um recurso protegido.

>[!div class="step-by-step"]
[Iniciar »](step-1-prepare-corp-domain.md)
