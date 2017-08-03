---
title: Atribuir Smart card a um pedido | Microsoft Docs
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
ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2886186ade7058b034565501e200ab3773b0af31
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="assign-smart-card-to-a-request"></a>Atribuir Smart card a um pedido
Vincula o smart card especificado para o pedido especificado. Uma vez vinculado, o pedido só pode ser executado com este cartão.

**Tenha em atenção**: URLs apresentados neste tópico são relativamente ao nome do anfitrião selecionado durante a implementação de API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Pedido


Método  |URL do pedido  
---------|---------
POST     |/CertificateManagement/API/v1.0/smartcards

###<a name="url-parameters"></a>Parâmetros de URL
nenhum.
###<a name="request-headers"></a>Cabeçalhos de pedido
Para cabeçalhos de pedido comuns, consulte [pedido HTTP e cabeçalhos da resposta](certificate-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do CM*.
###<a name="request-body"></a>Corpo do pedido
O corpo do pedido contém as seguintes propriedades.

Propriedade | Descrição
---------|-----------
RequestId | O ID do pedido que o smart card deve ser vinculado ao.
CardId | Cardid do smart card.
carateres ATR | A cadeia de (carateres ATR) de reposição de resposta de smart card.


##<a name="response"></a>Resposta
###<a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
201     | Criado
204 | Nenhum conteúdo
403 | Proibido
500 | Erro interno

###<a name="response-headers"></a>Cabeçalhos de Resposta
Para cabeçalhos de resposta comuns, consulte [pedido HTTP e cabeçalhos da resposta](certificate-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do CM*.
###<a name="response-body"></a>Corpo da resposta
Com êxito, devolve um URI para o objeto criado recentemente de smart card.
##<a name="example"></a>Exemplo

###<a name="request"></a>Pedido
```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}

```
###<a name="response"></a>Resposta
```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```       
##<a name="see-also"></a>Consulte Também

- [Método de Microsoft.Clm.Provision.RequestOperations.CreateSmartcard (cadeia, cadeia, pedido)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
