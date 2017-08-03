---
title: "Privileged Access Management referência da API REST | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 02de09a7de843cb42080daa4ce43a5a0c74f2c18
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="privileged-access-management-rest-api-reference"></a>Referência à API REST do Privileged Access Management
Microsoft Identity Manager (MIM) 2016 adiciona um novo cenário chamado Privileged Access Management (PAM). PAM permite uma organização ter mais controlo sobre os direitos de acesso elevado privilegiado de contas de utilizador, tais como os administradores de sistema ou serviço, a recursos confidenciais. PAM controla o acesso de conta de privilégio elevado, fornecendo os direitos de acesso de período de tempo limitado, apenas na time (JIT), quando são necessários os direitos de acesso.

Um utilizador pode pedir ao serviço MIM para o privileged direitos (elevação) de uma das duas formas de acesso:

- Utilizando a API de REST de PAM
- Utilizando o cmdlet do PowerShell de PAM New-PAMRequest

Os tópicos neste guia descrevem a API de REST de PAM. Para obter mais informações sobre como utilizar o PowerShell cmdlet consulte o guia de laboratório de teste: demonstrar o Privileged Access Management através do Microsoft Identity Manager, disponível no site do connect.

##<a name="pam-rest-api-resources-and-operations"></a>Operações e de recursos de API de REST de PAM
A API de REST de PAM funciona com os seguintes recursos
- **Função de PAM**: função de A PAM associa uma coleção de utilizadores com uma coleção de direitos de acesso. Os direitos de acesso definidos por referência aos grupos de segurança.  Cada função de PAM tem uma lista de contas de utilizador, denominada candidatos, que são elegível para efetuar a elevação à função de PAM. Pode efetuar as seguintes operações em funções de PAM:

    - [Obter Funções de PAM](privileged-access-management-get-roles.md)

- **Pedido de PAM**: um utilizador que pretende efetuar a elevação a uma direitos de acesso de função de PAM tem de submeter um pedido de PAM e obtenha a aprovação para o pedido para efetuar a elevação. O objeto de pedido de PAM controla o ciclo de vida deste pedido no serviço MIM. Pode efetuar as seguintes operações nos pedidos de PAM:

    - [Criar Pedido de PAM](privileged-access-management-create-request.md)
    - [Criar Pedidos de PAM](privileged-access-management-get-requests.md)
    - [Fechar Pedido de PAM](privileged-access-management-close-request.md)

- **Pedido de PAM pendente**: utilizado para aprovar ou rejeitar pedidos de PAM que foram submetidos pelos utilizadores. Pode efetuar as seguintes operações nos pedidos de PAM pendentes:

    - [Obter Pedidos de PAM Pendentes](privileged-access-management-get-pending-requests.md)
    - [Aprovar ou Rejeitar um Pedido de PAM Pendente](privileged-access-management-approve-reject-pending-request.md)

- **Sessão de PAM**: ao utilizar a API de REST de PAM, o cliente (por exemplo, um web browser) tem uma sessão com o ponto final de API de REST de PAM. Nesta sessão, o cliente é autenticado para o ponto final de REST API. Pode efetuar as seguintes operações em sessões de PAM:

     - [Obter Informações da Sessão de PAM](privileged-access-management-get-session-info.md)

Para obter mais informações sobre o serviço, consulte [detalhes do serviço de API de REST de PAM](privileged-access-management-rest-api-service-details.md)

##<a name="pam-sample-portal-on-github"></a>Portal de amostra de PAM no GitHub
Uma forma para aprender a utilizar a API de REST de PAM é através do portal de amostra de PAM, uma aplicação web de exemplo que utiliza a API. Pode encontrar o código para o portal de amostra de PAM no [repositório de amostra de PAM no GitHub](http://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409). Pode saber como implementar o portal de exemplo no guia de laboratório de teste de PAM.
