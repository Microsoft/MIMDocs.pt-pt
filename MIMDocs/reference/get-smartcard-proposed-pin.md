---
title: Obter propostas PIN de smart card | Microsoft Docs
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
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 08d28819402dd56f996d39aa03b8ac829bef0838
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-proposed-pin"></a>Obter propostas PIN de smart card
Obtém o PIN do utilizador gerado pelo servidor.

**Tenha em atenção**: O servidor irá definir o PIN apenas se a política de modelo de perfil indica que deve ser feito. Caso contrário, o utilizador deve fornecer.

**Tenha em atenção**: URLs apresentados neste tópico são relativamente ao nome do anfitrião selecionado durante a implementação de API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Pedido


Método  |URL do pedido  
---------|---------
GET     |/CertificateManagement/API/v1.0/smartcards/{ID}/serverproposedpin

###<a name="url-parameters"></a>Parâmetros de URL
Parâmetro | Descrição
---------|------------
ID | O identificador de Smart Card (específica de MIM CM). Obtidos a partir do objeto Microsft.Clm.Shared.Smartcard.
###<a name="query-parameters"></a>Parâmetros de consulta
Parâmetro | Descrição
---------|------------
carateres ATR | O carateres atr cartão.
CardId | O id de cartão.
desafio | Uma base-64 codificado cadeia que representa o desafio emitido pelo smartcard.

###<a name="request-headers"></a>Cabeçalhos de pedido
Para cabeçalhos de pedido comuns, consulte [pedido HTTP e cabeçalhos da resposta](certificate-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do CM*.
###<a name="request-body"></a>Corpo do pedido
nenhum

##<a name="response"></a>Resposta
###<a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
200     | OK
204 | Nenhum conteúdo
403 | Proibido
500 | Erro interno

###<a name="response-headers"></a>Cabeçalhos de Resposta
Para cabeçalhos de resposta comuns, consulte [pedido HTTP e cabeçalhos da resposta](certificate-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do CM*.
###<a name="response-body"></a>Corpo da resposta
Com êxito, devolve uma cadeia que representa o PIN proposto pelo servidor.

##<a name="example"></a>Exemplo

###<a name="request"></a>Pedido
```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```
###<a name="response"></a>Resposta
```
HTTP/1.1 200 OK

... body coming soon
```       
