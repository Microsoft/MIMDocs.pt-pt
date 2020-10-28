---
title: Aprovar ou rejeitar um pedido de PAM pendente / Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 0632656f-ecf4-4090-85a8-216d5638140a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 743c28b343c374d3406600751b379e8a4695c2d0
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762351"
---
# <a name="approve-or-reject-a-pending-pam-request"></a>Aprovar ou rejeitar um pedido de PAM pendente
Usado por uma conta privilegiada para aprovar, fechar ou rejeitar um pedido de elevação para uma função PAM.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
POST     |/api/pamresources/pamrequeststoapprove({approvalId)/Approve <br/>/api/pamresources/pamrequeststoapprove({approvalId)/Reject

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro | Descrição
----------|-----------
approvalId | O identificador (GUID) do objeto de homologação em PAM, especificado como `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'` .

### <a name="query-parameters"></a>Parâmetros de consulta

Parâmetro | Descrição
----------|--------------
v | Opcional. A versão API. Se não estiver incluída, utiliza-se a versão atual (mais recentemente lançada) da API. Para obter mais informações, consulte a versão em detalhes do [serviço API PAM REST](privileged-access-management-rest-api-service-details.md#versioning).


### <a name="request-headers"></a>Pedido cabeçalhos
Para cabeçalhos de pedido comuns, consulte [os cabeçalhos de pedido e resposta](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) HTTP nos *dados do serviço API PAM REST* .

### <a name="request-body"></a>Corpo do pedido
Nenhum.

## <a name="response"></a>Resposta
Esta secção descreve a resposta.

### <a name="response-codes"></a>Códigos de resposta

Código  |Descrição  
---------|---------
200 | OK
401 | Não autorizado
403 | Proibido
408 | Tempo Limite do Pedido   
500 | Erro interno do servidor
503 | Serviço Indisponível

### <a name="response-headers"></a>Cabeçalhos de resposta
Para cabeçalhos de pedido comuns, consulte [os cabeçalhos de pedido e resposta](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) HTTP nos *dados do serviço API PAM REST* .

### <a name="response-body"></a>Corpo da resposta
Nenhum.

## <a name="example"></a>Exemplo
Esta secção dá o exemplo para aprovar um pedido de elevação a uma função PAM.

### <a name="example-request"></a>Exemplo: Pedido

```
POST /api/pamresources/pamrequeststoapprove(guid'5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143')/Approve HTTP/1.1
```

### <a name="example-response"></a>Exemplo: Resposta

```
HTTP/1.1 200 OK
```       
