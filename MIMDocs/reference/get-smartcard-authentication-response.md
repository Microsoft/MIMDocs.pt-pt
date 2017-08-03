---
title: "Obter a resposta de autenticação de smart card | Microsoft Docs"
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
ms.assetid: e05ec898-06cd-4c17-a4f4-8f3545af0f14
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1c7a6f5e7ebd1e6e4cfc0992607adb91743f343e
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-authentication-response"></a>Obter a resposta de autenticação de smart card
Obtém a resposta a um desafio de autenticação do Base CSP.

**Tenha em atenção**: URLs apresentados neste tópico são relativamente ao nome do anfitrião selecionado durante a implementação de API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Pedido


Método  |URL do pedido  
---------|---------
GET     |/CertificateManagement/API/v1.0/Requests/{reqid}/smartcards/{scid}/authenticationresponse

###<a name="url-parameters"></a>Parâmetros de URL
Parâmetro | Descrição
---------|------------
reqid | Necessário. O identificador de pedido (específica de MIM CM).
scid | Necessário. O identificador de Smart Card (específica de MIM CM). Obtidos a partir de [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) objeto.
###<a name="query-parameters"></a>Parâmetros de consulta
Parâmetro | Descrição
---------|------------
carateres ATR | Opcional. A cadeia de (carateres ATR) de reposição de resposta de smart card.
CardId | Necessário. O id de cartão.
desafio | Necessário. Uma base-64 codificado cadeia que representa o desafio emitido pelo smartcard.
diversified | Necessário. Um sinalizador booleano que indica Meteorologia a chave de administrador de smart card foi diversified.


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
Com êxito, devolve um byte BLOB que representa a resposta de desafio.

##<a name="example"></a>Exemplo

###<a name="request"></a>Pedido
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/authenticationresponse?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e&challenge=1hFD%2Bcz%2F0so%3D&diversified=False HTTP/1.1

```
###<a name="response"></a>Resposta
```
HTTP/1.1 200 OK

"F0Zudm4wPLY="
```       
##<a name="see-also"></a>Consulte Também

- [Método de Microsoft.Clm.Provision.ExecuteOperations.GetBaseCspResponse](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.getbasecspresponse.aspx)
