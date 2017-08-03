---
title: Obter a chave de administrador Diversified smart card | Microsoft Docs
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
ms.assetid: 68beeec1-8350-4e0e-946f-d94606e1e756
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 57a8481b2a4f976717b07061e96ccb041164a5a6
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-diversified-admin-key"></a>Obter a chave de administrador Diversified smart card
Obtém a chave de administrador diversified para o smart card especificado.

**Tenha em atenção**: A chave de administrador apenas tem de ser diversified se a política de modelo de perfil indica que devem ser.

**Tenha em atenção**: URLs apresentados neste tópico são relativamente ao nome do anfitrião selecionado durante a implementação de API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Pedido


Método  |URL do pedido  
---------|---------
GET     |/CertificateManagement/API/v1.0/Requests/{reqid}/smartcards/{scid}/diversifiedkey

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
Com êxito, devolve um byte BLOB que representa a chave de administrador diversified.

##<a name="example"></a>Exemplo

###<a name="request"></a>Pedido
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/diversifiedkey?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e HTTP/1.1
```
###<a name="response"></a>Resposta
```
HTTP/1.1 200 OK

"mBVA+HopB/gc+6FuKsQqx+OX01hK1WQI"
```       
##<a name="see-also"></a>Consulte também

- [Método de método Microsoft.Clm.Provision.RequestOperations.CreateSmartcard (cadeia, cadeia, pedido)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
