---
title: Obtenha cartão inteligente proposto PIN / Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9d4f1444efa490f980e29440342844ac246d8a20
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762206"
---
# <a name="get-smart-card-proposed-pin"></a>Obtenha o PIN proposto por cartão inteligente
Obtém o PIN do utilizador gerado pelo servidor.

>[!IMPORTANT]
>O servidor só define o PIN se a política do modelo de perfil indicar que deve ser feita. Caso contrário, o utilizador deve fornecer o PIN.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards/{id}/serverproposedpin

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro | Descrição
---------|------------
ID | O ID do cartão inteligente específico do Microsoft Identity Manager (MIM) Certificate Management (CM). O ID é obtido a partir do objeto Microsft.Clm.Shared.Smartcard.

### <a name="query-parameters"></a>Parâmetros de consulta

Parâmetro | Descrição
---------|------------
atr | A cadeia de resposta ao reset do cartão inteligente (ATR).
cardid | A identificação do cartão inteligente.
desafio | Uma corda codificada base-64 que representa o desafio que é emitido pelo cartão inteligente.

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
No sucesso, devolve uma cadeia que representa o PIN que é proposto pelo servidor.

## <a name="example"></a>Exemplo
Esta secção fornece um exemplo para obter o PIN do utilizador gerado pelo servidor.

### <a name="example-request"></a>Exemplo: Pedido

```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```

### <a name="example-response"></a>Exemplo: Resposta

```
HTTP/1.1 200 OK

... body coming soon
```       
