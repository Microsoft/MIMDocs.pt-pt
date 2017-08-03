---
title: Obter pedidos de PAM pendente | Microsoft Docs
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
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: aa1e8cd48b1bcca6e856bd553b6b92a062a08ff4
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="get-pending-pam-requests"></a>Obter pedidos de PAM pendente
Utilizado por uma conta com privilégios para devolver uma lista de pedidos que necessitam de aprovação pendentes.

**Tenha em atenção**: URLs apresentados neste tópico são relativamente ao nome do anfitrião selecionado durante a implementação de API; por exemplo: `http://api.contoso.com`.
##<a name="request"></a>Pedido

Método  |URL do pedido  
---------|---------
GET     |/API/pamresources/pamrequeststoapprove

###<a name="query-parameters"></a>Parâmetros de consulta
Parâmetro | Descrição
----------|--------------
$filter | Opcional. Especifica qualquer uma das propriedades do pedido de PAM Perding numa expressão de filtro para devolver uma lista filtrada de respostas. Para mais informações sobre os operadores suportados, consulte [filtragem nos detalhes do serviço de API de REST de PAM](privileged-access-management-rest-api-service-details.md#filtering)
v | Opcional. A versão de API. Se não incluído, será utilizada a versão atual (mais recentemente lançada) da API. Para obter mais informações, consulte [os detalhes do serviço de API de REST do PAM de controlo de versões](privileged-access-management-rest-api-service-details.md#versioning)

###<a name="request-headers"></a>Cabeçalhos de pedido
Para cabeçalhos de pedido comuns, consulte [pedido HTTP e cabeçalhos da resposta](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do PAM*.
###<a name="request-body"></a>Corpo do pedido
nenhum

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
Uma resposta com êxito contém uma lista de objetos de aprovação de pedidos de PAM com as seguintes propriedades.

Propriedade | Descrição
---------|-------------
RoleName | O nome a apresentar da função para os quais é necessária aprovação.
Requerente | O nome de utilizador do requerente seja aprovada.
Justificação | A justificação fornecida pelo utilizador.
RequestedTTL | A hora de expiração de pedido em segundos.
RequestedTime | O pedido de tempo para a elevação.
HoraDaCriação | A hora de criação do pedido.
FIMRequestID | Contém um único elemento, "Valor", com o identificador exclusivo (GUID) do pedido de PAM.
RequestorID | Contém um único elemento, "Valor", com o identificador exclusivo (GUID) para a conta do Active Directory que criou o pedido de PAM.
ApprovalObjectID | Contém um único elemento, "Valor", com o identificador exclusivo (GUID) para o objeto de aprovação.

##<a name="example"></a>Exemplo

###<a name="request"></a>Pedido
```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```
###<a name="response"></a>Resposta
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
