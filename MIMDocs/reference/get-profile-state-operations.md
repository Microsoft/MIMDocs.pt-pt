---
title: Obtenha operações de perfil do estado ! Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: f62c827b-5229-4b13-ad37-4f62ad231d30
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 35246570b52285f916b74785c17ce55f2967fb7d
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762246"
---
# <a name="get-profile-state-operations"></a>Obtenha operações de estado de perfil
Obtém uma lista de possíveis operações que podem ser realizadas pelo utilizador atual no perfil especificado. Em seguida, pode ser iniciado um pedido para qualquer uma das operações especificadas.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
GET     |/CertificadoManagement/api/v1.0/profiles/{id}/operações <br/>/CertificadoManagement/api/v1.0/smartcards/{id}/operações

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro | Descrição
---------|------------
ID | O identificador (GUID) do perfil ou smartcard.

### <a name="request-headers"></a>Cabeçalhos do pedido
Para cabeçalhos de pedido comuns, consulte [os cabeçalhos de pedido e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *dados do serviço cm REST API* .

### <a name="request-body"></a>Corpo do pedido
Nenhum.

## <a name="response"></a>Resposta
Esta secção descreve a resposta.

### <a name="response-codes"></a>Códigos de resposta

Código  |Descrição  
---------|---------
200 | OK
204 | Sem conteúdo
403 | Proibido
500 | Erro Interno

### <a name="response-headers"></a>Cabeçalhos de resposta
Para cabeçalhos de resposta comuns, consulte [os cabeçalhos de pedido e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *dados do serviço cm REST API* .

### <a name="response-body"></a>Corpo da resposta
No sucesso, devolve uma lista de possíveis operações que podem ser realizadas pelo utilizador no cartão inteligente. A lista pode conter qualquer uma das seguintes operações: **OnlineUpdate** , **Renovar,** **Recuperar, Recuperar,** **Recuperar, Recuperar,** **Reformar,** **Revogar** e **Desbloquear** .

## <a name="example"></a>Exemplo
Esta secção fornece um exemplo para obter operações estatais de perfil para o utilizador atual.

### <a name="example-request"></a>Exemplo: Pedido

```
GET /certificatemanagement/api/v1.0/smartcards/438d1b30-f3b4-4bed-85fa-285e08605ba7/operations HTTP/1.1
```

### <a name="example-response"></a>Exemplo: Resposta

```
HTTP/1.1 200 OK

[
    "renew",
    "unblock",
    "retire"
]
```       
