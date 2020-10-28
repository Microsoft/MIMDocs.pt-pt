---
title: Obtenha informações sobre sessão PAM ! Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: bc30e455-9a9c-413a-b8ca-9669286299cf
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2529ac9665344403f798cd2bf708dffb8697876a
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762321"
---
# <a name="get-pam-session-info"></a>Obtenha informações da sessão PAM
Usado por uma conta privilegiada para obter o nome de utilizador da conta que está iniciada na sessão.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
GET     |/api/sessão/sessioninfo

### <a name="query-parameters"></a>Parâmetros de consulta

Parâmetro | Descrição
----------|--------------
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
Uma resposta bem sucedida devolve um objeto de sessão PAM com as seguintes propriedades:

Propriedade | Descrição
--------|-------------
Nome de utilizador | O nome de utilizador da conta que está registada nesta sessão.

## <a name="example"></a>Exemplo
Esta secção fornece um exemplo para obter a informação da sessão PAM.

### <a name="example-request"></a>Exemplo: Pedido 

```
GET /api/session/sessioninfo/ HTTP/1.1
```

### <a name="example-response"></a>Exemplo: Resposta

```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#sessioninfo",
    "value":[
        {
            "Username":"FIMCITONEBOXAD\\Jen"
        }
    ]
}
```       
