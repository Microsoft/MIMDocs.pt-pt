---
title: "Operações de estado do perfil de GET | Microsoft Docs"
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
ms.assetid: f62c827b-5229-4b13-ad37-4f62ad231d30
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8e8428984404b2aebda8f53e4f7841b699a5d23a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-state-operations"></a>Operações de estado do perfil de GET
Obtém uma lista de possíveis operações que podem ser efetuadas pelo utilizador atual no perfil especificado. Um pedido, em seguida, pode ser iniciado para qualquer uma das operações especificadas.

**Tenha em atenção**: URLs apresentados neste tópico são relativamente ao nome do anfitrião selecionado durante a implementação de API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Pedido


Método  |URL do pedido  
---------|---------
GET     |/CertificateManagement/API/v1.0/Profiles/{ID}/Operations <br/>/CertificateManagement/API/v1.0/smartcards/{ID}/Operations

###<a name="url-parameters"></a>Parâmetros de URL
Parâmetro | Descrição
---------|------------
ID | O identificador (GUID) do perfil ou smart card.

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
Com êxito, devolve uma lista de possíveis operações que podem ser efetuadas pelo utilizador no smart card. Esta lista pode conter qualquer número dos seguintes procedimentos: *OnlineUpdate*, *renovar*, *recuperar*, *RecoverOnBehalf*, *extinguir*, *revogar*, e *desbloqueio*.

##<a name="example"></a>Exemplo

###<a name="request"></a>Pedido
```
GET /certificatemanagement/api/v1.0/smartcards/438d1b30-f3b4-4bed-85fa-285e08605ba7/operations HTTP/1.1
```
###<a name="response"></a>Resposta
```
HTTP/1.1 200 OK

[
    "renew",
    "unblock",
    "retire"
]
```       
