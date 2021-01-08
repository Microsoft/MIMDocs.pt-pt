---
title: Configurar o MIM 2016 para Privileged Access Management | Documentos da Microsoft
description: Plano para a instalação e MIM e configuração da mesma para Privileged Access Management.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 64ed3184ee38102f488c61bf0b41bc0fec34b049
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010612"
---
# <a name="configure-the-mim-environment-for-privileged-access-management"></a>Configuração do ambiente de MIM para Privileged Access Management

Existem sete passos para concluir ao configurar o ambiente para o acesso entre florestas, instalar e configurar o Active Directory e o Microsoft Identity Manager, e demonstrar um pedido de acesso just-in-time.

Estes passos são apresentados para que possa começar do zero e criar um ambiente de teste. Se estiver a aplicar PAM a um ambiente existente, pode utilizar os seus próprios controladores de domínio ou contas de utilizador para o domínio *CONTOSO,* em vez de criar novos para combinar com os exemplos.

1. Preparar o servidor *CORPDC* como um controlador de domínio e o *CORPWKSTN* como uma estação de trabalho de membro.

2. Preparar o servidor *PRIVDC* como um controlador de domínio.

3.  Preparar o servidor *PAMSRV* na floresta *PRIV*.

4.  Instalar componentes de MIM *PAMSRV* e os cmdlets numa estação de trabalho de membro de floresta *CONTOSO* e prepará-los para Privileged Access Management.

5.  Estabelecer fidedignidade entre o *PRIV* e as florestas *CONTOSO*.

6.  Preparar grupos de segurança com privilégios com acesso a recursos protegidos e contas de membro para Just-in-time Privileged Access Management.

7.  Demonstram a utilização de pedidos, receções e execuções de acesso elevado privilegiado a um recurso protegido.

> [!div class="step-by-step"]
> [Iniciar »](step-1-prepare-corp-domain.md)
