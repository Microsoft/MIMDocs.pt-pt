---
title: "Obter funções de PAM | Microsoft Docs"
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
ms.assetid: d3c4f528-c3c8-41c1-905e-7eb84f074ce4
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4cb4bbc6c7696f354e5759a677ece88a4e606544
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-roles"></a>Obter funções de PAM
Utilizado por uma conta com privilégios para listar as funções de PAM para o qual a conta é uma candidata.

**Tenha em atenção**: URLs apresentados neste tópico são relativamente ao nome do anfitrião selecionado durante a implementação de API; por exemplo: `http://api.contoso.com`.
##<a name="request"></a>Pedido


Método  |URL do pedido  
---------|---------
GET     |/API/pamresources/pamroles

###<a name="query-parameters"></a>Parâmetros de consulta
Parâmetro | Descrição
----------|--------------
$filter | Opcional. Especifica qualquer uma das propriedades da função de PAM numa expressão de filtro para devolver uma lista filtrada de respostas. Para mais informações sobre os operadores suportados, consulte [filtragem nos detalhes do serviço de API de REST de PAM](privileged-access-management-rest-api-service-details.md#filtering)
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
Uma resposta com êxito contém uma coleção de uma ou mais funções de PAM, cada um dos quais tem as seguintes propriedades.

Propriedade | Descrição
--------|-------------
RoleID | O identificador exclusivo (GUID) da função de PAM.
Nome a Apresentar | Nome a apresentar da função de PAM no serviço MIM.
Descrição | Descrição da função de PAM no serviço MIM.
TTL | Acesso a função de direitos de limite de tempo de expiração máxima em segundos.
AvailableFrom | A hora mais antigo do dia em que um requst será ativado.
AvailableTo | A hora mais recente do dia que irão estar ativado um pedido.
MFAEnabled | Um valor booleano que indica se os pedidos de ativação para esta função de solicitar um desafio MFA.
ApprovalEnabled | Um valor booleano que indica se os pedidos de ativação para esta função de exigem a aprovação por um proprietário de função.
AvailibitlyWindowEnabled | Um valor booleano que indica se a função só pode ser ativada durante um intervalo de tempo especificado.

##<a name="example"></a>Exemplo

###<a name="request"></a>Pedido
```
GET /api/pamresources/pamroles HTTP/1.1
```
###<a name="response"></a>Resposta
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamroles",
    "value":[
        {
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "DisplayName":"Allow AD Access ",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":false,
            "AvailabilityWindowEnabled":false
        },
        {
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "DisplayName":"ApprovalRole",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":true,
            "AvailabilityWindowEnabled":false
        }
    ]
}
```       
