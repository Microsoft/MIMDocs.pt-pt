---
title: Receber pedidos de PAM pendentes ! Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0ec4388ce5d5f748d9f779819d0a2d0bdec06791
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762342"
---
# <a name="get-pending-pam-requests"></a>Receber pedidos de PAM pendentes
Usado por uma conta privilegiada para devolver uma lista de pedidos pendentes que precisam de aprovação.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
GET     |/api/pamresources/pamrequeststoapprove

### <a name="query-parameters"></a>Parâmetros de consulta

Parâmetro | Descrição
----------|--------------
$filter | Opcional. Especifique qualquer uma das propriedades de pedido pam pendentes numa expressão de filtro para devolver uma lista filtrada de respostas. Para obter mais informações sobre operadores apoiados, consulte [a filtragem nos dados do serviço API PAM REST](privileged-access-management-rest-api-service-details.md#filtering).
v | Opcional. A versão API. Se não estiver incluída, utiliza-se a versão atual (mais recentemente lançada) da API. Para obter mais informações, consulte a versão em detalhes do [serviço API PAM REST](privileged-access-management-rest-api-service-details.md#versioning).

### <a name="request-headers"></a>Cabeçalhos do pedido
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
Uma resposta bem sucedida contém uma lista de objetos de aprovação de pedidos DE PAM com as seguintes propriedades:

Propriedade | Descrição
---------|-------------
RoleName | O nome de exibição da função para a qual é necessária a aprovação.
Requerente | O nome de utilizador do solicitador a aprovar.
Justificação | A justificação fornecida pelo utilizador.
SolicitadoTTL | O tempo de validade solicitado em segundos.
Hora Solicitada | O tempo solicitado para a elevação.
HoraDaCriação | O tempo de criação do pedido.
FIMRequestID | Contém um único elemento, "Value", com o identificador único (GUID) do pedido PAM.
RequestorID | Contém um único elemento, "Value", com o identificador único (GUID) para a conta Ative Directory que criou o pedido DE PAM.
AprovaçãoObjectID | Contém um único elemento, "Value", com o identificador único (GUID) para o Objeto de Aprovação.

## <a name="example"></a>Exemplo
Esta secção fornece um exemplo para obter os pedidos de PAM pendentes.

### <a name="example-request"></a>Exemplo: Pedido

```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```

### <a name="example-response"></a>Exemplo: Resposta

```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequeststoapprove",
    "value":[
        {
            "RoleName":"ApprovalRole",
            "Requestor":"PRIV.Jen",
            "Justification":"Justification Reason",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-11T22:25:00Z",
            "CreationTime":"2015-07-11T22:24:52.51Z",
            "FIMRequestID":{
                "Value":"9802d7b7-b4e9-4fe4-8f5c-649cda127e49"
            },
            "RequestorID":{
                "Value":"73257e5e-00b3-4309-a330-f1e607ff113a"
            },
            "ApprovalObjectID":{
                "Value":"5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143"
            }
        }
    ]
}
```       
