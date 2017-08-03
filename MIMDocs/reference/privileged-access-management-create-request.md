---
title: Criar pedido de PAM | Microsoft Docs
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
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: de28af5c49eb98c5a1ccfbd8ed9353cf202e9e66
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="create-pam-request"></a>Criar pedido de PAM
Utilizado por uma conta com privilégios de elevação a uma função de PAM.

**Tenha em atenção**: URLs apresentados neste tópico são relativamente ao nome do anfitrião selecionado durante a implementação de API; por exemplo: `http://api.contoso.com`.
##<a name="request"></a>Pedido


Método  |URL do pedido  
---------|---------
POST     |/API/pamresources/pamrequests

###<a name="query-parameters"></a>Parâmetros de consulta
Parâmetro | Descrição
--------|-------------
Justificação | Opcional. O motivo fornecido pelo utilizador para o pedido de elevação.
RoleId| Necessário. O identificador exclusivo (GUID) da função de PAM para elevar para.
RequestedTTL| Necessário. A hora de expiração de pedido, em segundos.
RequestedTime | Optoinal. O tempo para efetuar a elevação de privilégios.  
v | Opcional. A versão de API. Se não incluído, será utilizada a versão atual (mais recentemente lançada) da API. Para obter mais informações, consulte [os detalhes do serviço de API de REST do PAM de controlo de versões](privileged-access-management-rest-api-service-details.md#versioning)

**Tenha em atenção**: pode especificar o *justificação*, *RoleId*, *RequestedTTL*, e *RequestedTime* parâmetros como propriedades no corpo do pedido, em vez de parâmetros de consulta. O *v* parameteer só pode ser especificado como um parâmetro de consulta.

###<a name="request-headers"></a>Cabeçalhos de pedido
Para cabeçalhos de pedido comuns, consulte [pedido HTTP e cabeçalhos da resposta](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do PAM*.
###<a name="request-body"></a>Corpo do pedido
Opcional. Conforme indicado acima, o *justificação*, *RoleId*, *RequestedTTL*, e *RequestedTime* parâmetros podem ser especificados como cadeia de consulta de propriedades de um corpo do pedido em vez de especificá-los no URL.

##<a name="response"></a>Resposta
###<a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
200 | OK
401 | Não Autorizado
403 | Proibido
408 | Tempo Limite do Pedido   
500 | Erro interno do servidor
503 | Serviço indisponível

###<a name="response-headers"></a>Cabeçalhos de Resposta
Para cabeçalhos de resposta comuns, consulte [pedido HTTP e cabeçalhos da resposta](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do PAM*.
###<a name="response-body"></a>Corpo da resposta
Uma resposta com êxito contém um objeto de pedido de PAM com as seguintes propriedades.

Propriedade | Descrição
--------|-------------
RequestID | O identificador exclusivo (GUID) para o pedido de PAM.
CreatorID | O identificador exclusivo (GUID) no serviço MIM para a conta que criou o pedido.
Justificação | O motivo para a elevação.
HoraDaCriação | A hora de criação do pedido.
CreationMethod | O método utilizado para criar o pedido.
ExpirationTime | A hora de expiração do pedido.
RoleID| O identificador exclusivo (GUID) da função de PAM.
RequestedTTL | O expiração solicitado o tempo limite em segundos.
RequestedTime | O pedido de tempo para a elevação.
RequestStatus | O estado do pedido. Os valores possíveis são: "Processar", "Ativo", "Fechado", "Fechar", "Expirado", "PendingApproval", "PendingMFA" e "Rejeitado".

##<a name="example"></a>Exemplo

###<a name="request-1"></a>Pedido 1
```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```
###<a name="response-1"></a>Resposta 1
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

###<a name="request-2"></a>Pedido de 2
```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```
###<a name="response-2"></a>Resposta 2
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
