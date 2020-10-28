---
title: Atualizar o estado do cartão inteligente ! Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 598dace3-c6f2-447a-9301-c0b63ee38276
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: fe6d59377ef3218fde0df99365506ef9ec143a6f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762511"
---
# <a name="update-smart-card-status"></a>Atualizar o estado do cartão inteligente
Atualiza o estado de um cartão inteligente.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro | Descrição
---------|------------
reqid | Obrigatório. O identificador de pedidos específico do Microsoft Identity Manager (MIM) Certificate Management (CM).
scid | Obrigatório. O identificador de cartões inteligentes que é específico da MIM CM. O valor corresponde ao campo "uuid" no objeto [Microsoft.Clm.Shared.Smartcards.Smartcards.Smartcard.](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)

### <a name="request-headers"></a>Cabeçalhos do pedido
Para cabeçalhos de pedido comuns, consulte [os cabeçalhos de pedido e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *dados do serviço cm REST API* .

### <a name="request-body"></a>Corpo do pedido
O organismo de pedido contém as seguintes propriedades:

Propriedade | Descrição
---------|-----------
status | O estatuto para definir o pedido, como "Aposentado".

## <a name="response"></a>Resposta
Esta secção descreve a resposta.

### <a name="response-codes"></a>Códigos de resposta

Código  |Descrição  
---------|---------
200     | OK
204 | Sem conteúdo
403 | Proibido
500 | Erro Interno

### <a name="response-headers"></a>Cabeçalhos de resposta
Para cabeçalhos de resposta comuns, consulte [os cabeçalhos de pedido e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *dados do serviço cm REST API* .

### <a name="response-body"></a>Corpo da resposta
No sucesso, devolve um JSON-Serialized [objeto Microsoft.Clm.Shared.Smartcards.Smartcards](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) com as seguintes propriedades:

Nome | Descrição
-----|-----------
DesignadosUserUuid | O identificador do utilizador a quem o cartão inteligente é atribuído.
Atr | A cadeia de resposta a reset (ATR) do cartão que está atualmente a ser inicializada.
Comentário | O comentário que descreve o cartão inteligente.
Sinalizadores | As bandeiras que descrevem o cartão inteligente.
Middleware | O middleware para o cartão inteligente.
ParentSmartcardUuid | O identificador do antigo cartão inteligente que o cartão inteligente substituiu.
PermanenteSsmartcarduuid | O identificador do cartão inteligente permanente que está associado ao cartão inteligente.
PrimarySmartcarduuid | O identificador do cartão inteligente primário.
ProfileTemplateUuid | O identificador do modelo de perfil que contém as políticas e configurações que regem o cartão inteligente.
ProfileTemplateVersion | A versão do modelo de perfil no momento em que o perfil do cartão inteligente foi criado.
SerialNumber | O número de série do cartão inteligente.
Estado | O estado do cartão inteligente.
Uuid | O identificador do perfil do cartão inteligente.

## <a name="example"></a>Exemplo
Esta secção fornece um exemplo para atualizar o estado de um cartão inteligente.

### <a name="example-request"></a>Exemplo: Pedido

```
PUT /certificatemanagement/api/v1.0/requests/b105403d-d021-41ea-9f11-be3d677d229e/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```

### <a name="example-response"></a>Exemplo: Resposta

```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":6,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{bc88f13f-83ba-4037-8262-46eba1291c6e}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       

## <a name="see-also"></a>Ver também

- [Microsoft.Clm.Shared.Smartcards.Smartcards.Smartcard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)
