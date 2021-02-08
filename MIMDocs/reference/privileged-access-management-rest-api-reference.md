---
title: Gestão privilegiada de acesso REST Referência API | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: a49621bd52e14be62d58f89b9b9c7146f59e5bd0
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835562"
---
# <a name="privileged-access-management-rest-api-reference"></a>Referência de API de Gestão de Acesso privilegiado
O Microsoft Identity Manager (MIM) 2016 adiciona um novo cenário chamado Gestão de Acesso Privilegiado (PAM). A PAM permite que uma organização tenha mais controlo sobre os direitos de acesso de contas de utilizadores privilegiadas elevadas, como sistemas ou administradores de serviços, a recursos sensíveis. A PAM controla o acesso à conta de alto privilégio, fornecendo direitos de acesso a tempo limitados, mesmo a tempo (JIT), quando os direitos de acesso são necessários.

Um utilizador pode pedir ao Serviço MIM direitos de acesso privilegiados (elevação) de uma de duas formas:

- Utilizando a API PAM REST.
- Utilizando o cmdlet PAM PowerShell New-PAMRequest.

Os tópicos deste guia descrevem a API PAM REST. Para obter mais informações sobre a utilização do cmdlet PowerShell, consulte _o Guia do Laboratório de Teste: Demonstrando gestão de acesso privilegiado utilizando_ o Microsoft Identity Manager, disponível no site de ligação.

## <a name="pam-rest-api-resources-and-operations"></a>RECURSOS e operações da API PAM REST
A API PAM REST opera nos seguintes recursos:
- **Papel PAM**: Uma função PAM associa uma coleção de utilizadores a uma coleção de direitos de acesso. Os direitos de acesso são definidos por referência a grupos de segurança.  Cada papel de PAM tem uma lista de contas de utilizador, chamadas candidatos, que têm o direito de elevar para o papel de PAM. Pode executar as seguintes operações nas funções PAM:

    - [Obter Funções de PAM](privileged-access-management-get-roles.md)

- **Pedido PAM**: Um utilizador que pretenda elevar os direitos de acesso à função PAM tem de apresentar um pedido de PAM e obter aprovação para o pedido de elevação. O objeto PAM Request acompanha o ciclo de vida deste pedido no Serviço MIM. Pode efetuar as seguintes operações nos pedidos da PAM:

    - [Criar Pedido de PAM](privileged-access-management-create-request.md)
    - [Criar Pedidos de PAM](privileged-access-management-get-requests.md)
    - [Fechar Pedido de PAM](privileged-access-management-close-request.md)

- **Pedido DE PAM pendente**: Usado para aprovar ou rejeitar pedidos de PAM que tenham sido apresentados pelos utilizadores. Pode efetuar as seguintes operações em pedidos DE PAM pendentes:

    - [Obter Pedidos de PAM Pendentes](privileged-access-management-get-pending-requests.md)
    - [Aprovar ou Rejeitar um Pedido de PAM Pendente](privileged-access-management-approve-reject-pending-request.md)

- **Sessão PAM**: Ao utilizar a API PAM REST, o cliente (por exemplo, um navegador web) tem uma sessão com o ponto final da API PAM REST. Nesta sessão, o cliente é autenticado no ponto final da API REST. Pode efetuar as seguintes operações nas sessões PAM:

     - [Obter Informações da Sessão de PAM](privileged-access-management-get-session-info.md)

Para obter informações mais detalhadas sobre o serviço, consulte [os Detalhes do Serviço API DA PAM REST.](privileged-access-management-rest-api-service-details.md)

## <a name="pam-sample-portal-on-github"></a>Portal da amostra PAM no GitHub
Uma forma de aprender a usar a API PAM REST é utilizando o portal da amostra PAM, uma aplicação web de exemplo que utiliza a API. Pode encontrar o código do portal PAM Sample no [repositório de amostras PAM no GitHub.](https://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409) Pode aprender a implantar o portal da amostra no Guia do Laboratório de Testes PAM.
