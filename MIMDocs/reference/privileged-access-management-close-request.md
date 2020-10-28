---
title: Pedido de PAM de perto ! Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ca3a1a68-9a2b-47da-bfc1-eaa360cbe609
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: be7c2ff956f928a32de343fcf47b5398fdd303f0
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762332"
---
# <a name="close-pam-request"></a>Pedido de PAM de perto
Usado por uma conta privilegiada para fechar um pedido que iniciou para elevar para um papel PAM.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
POST     |/api/pamresources/pamrequests ({requestId)/Close

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro | Descrição
----------|-----------
requestId | O identificador (GUID) do pedido de ENCERRAMENTO do PAM, especificado como `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'` .

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
Esta secção fornece um exemplo para encerrar um pedido.

### <a name="example-request"></a>Exemplo: Pedido

```
POST /api/pamresources/pamrequests(guid'5ec10e61-cdd1-404e-a18e-740467d87dbf')/Close HTTP/1.1
```

### <a name="example-response"></a>Exemplo: Resposta

```
HTTP/1.1 200 OK
```       
