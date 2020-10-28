---
title: Obtenha o smart card diversificado chave de administração / Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 68beeec1-8350-4e0e-946f-d94606e1e756
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6668ac823607436c2472a076f7c5ea2d9c727b04
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762036"
---
# <a name="get-smart-card-diversified-admin-key"></a>Obtenha chave de administração diversificada de cartão inteligente
Obtém a chave de administração diversificada para o cartão inteligente especificado.

>[!IMPORTANT]
>A chave de administração só deve ser diversificada quando a política do modelo de perfil indicar que deve ser.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/diversifiedkey

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro | Descrição
---------|------------
reqid | Obrigatório. O identificador de pedidos específico do Microsoft Identity Manager (MIM) Certificate Management (CM).
scid | Obrigatório. O identificador de cartões inteligentes que é específico da MIM CM. O scid é obtido a partir do objeto [Microsoft.Clm.Shared.Smartcards.Smartcards.Smartcard.](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)

### <a name="query-parameters"></a>Parâmetros de consulta

Parâmetro | Descrição
---------|------------
atr | Opcional. A cadeia de resposta ao reset do cartão inteligente (ATR).
cardid | Obrigatório. A identificação do cartão.

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
No sucesso, devolve um byte BLOB representando a chave de administração diversificada.

## <a name="example"></a>Exemplo
Esta secção fornece um exemplo para obter a chave de administração diversificada para um cartão inteligente.

### <a name="example-request"></a>Exemplo: Pedido

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/diversifiedkey?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e HTTP/1.1
```

### <a name="example-response"></a>Exemplo: Resposta

```
HTTP/1.1 200 OK

"mBVA+HopB/gc+6FuKsQqx+OX01hK1WQI"
```       

## <a name="see-also"></a>Ver também

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard método (String, String, Request)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
