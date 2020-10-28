---
title: Criar pedido PAM / Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d58155232b3f99f32df9cb6b4c2e344d0cbad278
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762341"
---
# <a name="create-pam-request"></a>Criar pedido DE PAM
Usado por uma conta privilegiada para elevar para um papel DE PAM.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
POST     |/api/pamresources/pamrequests

### <a name="query-parameters"></a>Parâmetros de consulta

Parâmetro | Descrição
--------|-------------
Justificação | Opcional. O motivo fornecido pelo utilizador para o pedido de elevação.
RoleId | Obrigatório. O identificador único (GUID) do papel PAM para elevar.
SolicitadoTTL | Obrigatório. O tempo de validade solicitado, em segundos.
Hora Solicitada | Opcional. É hora de elevar os privilégios.  
v | Opcional. A versão API. Se não estiver incluída, utiliza-se a versão atual (mais recentemente lançada) da API. Para obter mais informações, consulte a versão em detalhes do [serviço API PAM REST](privileged-access-management-rest-api-service-details.md#versioning).

>[!NOTE]
>Pode especificar os parâmetros **justificação,** **RoleId,** **RequestedTTL** e **RequestedTime** como propriedades no organismo de pedido, em vez de como parâmetros de consulta. O parâmetro **v** só pode ser especificado como parâmetro de consulta.

### <a name="request-headers"></a>Cabeçalhos do pedido
Para cabeçalhos de pedido comuns, consulte [os cabeçalhos de pedido e resposta](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) HTTP nos *dados do serviço API PAM REST* .

### <a name="request-body"></a>Corpo do pedido
Opcional. Os parâmetros **justificação** , **RoleId,** **RequestedTTL** e **RequestedTime** podem ser especificados como propriedades de um órgão de pedido em vez de os especificar na cadeia de consulta URL.

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
Uma resposta bem sucedida contém um objeto de pedido PAM com as seguintes propriedades:

Propriedade | Descrição
--------|-------------
RequestID | O identificador único (GUID) para o pedido de PAM.
CreatorID | O identificador único (GUID) no serviço MIM para a conta que criou o pedido.
Justificação | O motivo da elevação.
HoraDaCriação | O tempo de criação do pedido.
CriaçãoMethod | O método usado para criar o pedido.
ExpirationTime | O prazo de validade do pedido.
RoleID| O identificador único (GUID) do papel PAM.
SolicitadoTTL | O prazo de validade solicitado em segundos.
Hora Solicitada | O tempo solicitado para a elevação.
PedidoStatus | O estado do pedido. Os valores possíveis são "Processamento", "Ativo", "Fechado", "Fechado", "Expirado", "Pendente de Aprovação", "PendenteMFA" e "Rejeitado".

## <a name="example"></a>Exemplo
Esta secção fornece exemplos para criar um pedido de PAM.

### <a name="example-request-1"></a>Exemplo: Pedido 1

```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```

### <a name="example-response-1"></a>Exemplo: Resposta 1

```
HTTP/1.1 201 Created

{  
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"c0112f13-b16b-40ad-b547-07f23a7fba52",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":"Sample Reason",
    "CreationTime":"2015-07-11T23:38:09.036164-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"7200",
    "RequestedTime":"2015-07-12T06:40:00Z",
    "RequestStatus":"PendingApproval"
}
```       

### <a name="example-request-2"></a>Exemplo: Pedido 2

```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```

### <a name="example-response-2"></a>Exemplo: Resposta 2

```
HTTP/1.1 201 Created

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"504f9c49-00db-42bd-a157-ee5664617189",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":null,
    "CreationTime":"2015-07-11T23:07:30.2200123-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"3600",
    "RequestedTime":"2015-07-12T06:07:27.7229894Z",
    "RequestStatus":"PendingApproval"
}
```       
