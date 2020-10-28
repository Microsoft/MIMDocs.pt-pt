---
title: Obtenha pedidos de PAM ! Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 620eebd6-e4c3-473b-b824-ee6cfe83e509
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0eeaee3a6355229de94bcc390ad07da3e00b829f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762312"
---
# <a name="get-pam-requests"></a>Receba pedidos de PAM
Usado por uma conta privilegiada para devolver um histórico de pedidos de PAM previamente publicados.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
GET     |/api/pamresources/pamrequests

### <a name="query-parameters"></a>Parâmetros de consulta

Parâmetro | Descrição
----------|--------------
$filter | Opcional. Especifique qualquer uma das propriedades de pedido de PAM numa expressão de filtro para devolver uma lista filtrada de respostas. Para obter mais informações sobre operadores apoiados, consulte [a filtragem nos dados do serviço API PAM REST](privileged-access-management-rest-api-service-details.md#filtering).
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
Uma resposta bem sucedida contém uma lista de objetos de pedido PAM com as seguintes propriedades:

Propriedade | Descrição
--------|-------------
RequestID | O identificador único (GUID) para o pedido de PAM.
CreatorID | Um identificador único (GUID) para a conta Ative Directory que criou o pedido PAM.
Justificação | O motivo da elevação.
DisplayName | O nome de exposição do pedido PAM na MIM.
HoraDaCriação | O tempo de criação do pedido.
CriaçãoMethod | O método usado para criar o pedido.
ExpirationTime | O prazo de validade do pedido.
RoleID| O identificador único (GUID) do papel PAM.
SolicitadoTTL | O prazo de validade solicitado em segundos.
Hora Solicitada | O tempo solicitado para a elevação.
Status solicitado | O estado do pedido. Os valores possíveis são "Ativo", "Fechado", "Fechado", "Expirado", "Aprovação Pendente" e "Rejeitado".

## <a name="example"></a>Exemplo
Esta secção fornece um exemplo para obter os pedidos de PAM publicados.

### <a name="example-request"></a>Exemplo: Pedido

```
GET /api/pamresources/pamrequests?$filter=CreationTime%20gt%20datetime'2015-06-12T04:49:32.431Z'%20and%20CreationTime%20lt%20datetime'2015-07-13T04:49:32.432Z' HTTP/1.1
```

### <a name="example-response"></a>Exemplo: Resposta

```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests",
    "value":[
        {
            "RequestId":"b22e1343-9a2b-4e33-a70a-1bb7b2d405b9",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-06-23T11:34:38.58Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-06-23T12:34:38.847Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-23T11:34:36.417Z",
            "RequestStatus":"Expired"
        },
        {
            "RequestId":"3a98d1c7-524d-4b72-9da7-bd9f907eab55",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Reason for Request",
            "CreationTime":"2015-07-12T04:35:14.433Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T04:43:51.95Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:35:00Z",
            "RequestStatus":"Closed"
        },
        {
            "RequestId":"f5e80be1-e9a3-42c4-81f8-4be5a4a429f4",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-07-12T04:48:17.46Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T05:48:17.853Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-12T04:48:14.057Z",
            "RequestStatus":"Active"
        },
        {
            "RequestId":"b0f0ddc0-c809-4770-9d39-0d706f97a2de",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"",
            "CreationTime":"2015-06-30T07:01:13.147Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-30T07:01:13.119Z",
            "RequestStatus":"Rejected"
        },
        {
            "RequestId":"5ec10e61-cdd1-404e-a18e-740467d87dbf",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Example Reason",
            "CreationTime":"2015-07-12T04:49:09.963Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:50:00Z",
            "RequestStatus":"PendingApproval"
        }
    ]
}
```       
