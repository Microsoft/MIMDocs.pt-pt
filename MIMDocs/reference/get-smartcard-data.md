---
title: Obtenha perfis de cartões inteligentes Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 81f4b7cd-e4d9-4b11-b125-78cc9f183cf0
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0666a17abe63b0efbccd59aa0b9e0bb5daf80fe5
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762217"
---
# <a name="get-smart-card-profiles"></a>Obtenha perfis de cartões inteligentes
Obtém uma lista de perfis de cartões inteligentes para um utilizador. A lista inclui as possíveis operações que podem ser realizadas pelo utilizador atual. Em seguida, pode ser iniciado um pedido para qualquer uma das operações especificadas.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
GET     |/CertificadoManagement/api/v1.0/smartcards <br/> /CertificateManagement/api/v1.0/smartcards/{smartcarduuid}


### <a name="url-parameters"></a>Parâmetros de URL

Propriedade| Descrição
---------|--------
smartcarduid | Opcional. O cartão inteligente UUID, tal como denotado pelo Microsoft Identity Manager (MIM) Certificate Management (CM). O valor corresponde ao campo "uuid" no objeto [Microsoft.Clm.Shared.Smartcards.Smartcards.Smartcard.](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)

### <a name="query-parameters"></a>Parâmetros de consulta

Propriedade| Descrição
---------|--------
cardid | Opcional. O cartão inteligente UUID como denotado pela MIM CM. O valor corresponde ao campo "uuid" no objeto [Microsoft.Clm.Shared.Smartcards.Smartcards.Smartcard.](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)

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
Esta secção fornece um exemplo para obter perfis de cartões inteligentes para um utilizador.

### <a name="example-request-1"></a>Exemplo: Pedido 1

```
GET /certificatemanagement/api/v1.0/smartcards?cardid=d1ef6869-5517-42a0-8f05-16ca1a0b834d HTTP/1.1
```

### <a name="example-response-1"></a>Exemplo: Resposta 1

```
HTTP/1.1 200 OK

{
    "Uuid":"438d1b30-f3b4-4bed-85fa-285e08605ba7",
    "Status":3,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{d1ef6869-5517-42a0-8f05-16ca1a0b834d}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       

### <a name="example-request-2"></a>Exemplo: Pedido 2

```
GET /certificatemanagement/api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```

### <a name="example-response-2"></a>Exemplo: Resposta 2

```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":2,
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
