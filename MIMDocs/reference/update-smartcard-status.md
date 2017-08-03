---
title: Atualizar o estado do smart card | Microsoft Docs
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
ms.assetid: 598dace3-c6f2-447a-9301-c0b63ee38276
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: cb7b04539b8568a8b2ae83172b2aa8cddbf6174d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="update-smartcard-status"></a>Atualizar o estado do smart card
Atualiza o estado de um smart card.

**Tenha em atenção**: URLs apresentados neste tópico são relativamente ao nome do anfitrião selecionado durante a implementação de API; por exemplo: `https://api.contoso.com`.
## <a name="request"></a>Pedido


Método  |URL do pedido  
---------|---------
GET     |/ CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}

### <a name="url-parameters"></a>Parâmetros de URL
Parâmetro | Descrição
---------|------------
reqid | Necessário. O identificador de pedido (específica de MIM CM).
scid | Necessário. O identificador de Smart Card (específica de MIM CM). Esta é a propriedade "uuid" no [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) objeto.

### <a name="request-headers"></a>Cabeçalhos de pedido
Para cabeçalhos de pedido comuns, consulte [pedido HTTP e cabeçalhos da resposta](certificate-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do CM*.
### <a name="request-body"></a>Corpo do pedido
O corpo do pedido contém as seguintes propriedades.

Propriedade | Descrição
---------|-----------
Estado | O estado a definir o pedido. Por exemplo, "extinto."


## <a name="response"></a>Resposta
### <a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
200     | OK
204 | Nenhum conteúdo
403 | Proibido
500 | Erro interno

### <a name="response-headers"></a>Cabeçalhos de Resposta
Para cabeçalhos de resposta comuns, consulte [pedido HTTP e cabeçalhos da resposta](certificate-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do CM*.
### <a name="response-body"></a>Corpo da resposta
Com êxito, devolve uma JSON serializado [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) objeto com as seguintes propriedades:

Nome | Descrição
-----|-----------
AssignedUserUuid | O identificador do utilizador a quem é atribuído o smart card.
carateres ATR | A smart card reposição de resposta () cadeia de carateres ATR para o cartão que está atualmente a ser inicializado.
Comentário | O comentário que descreve o smart card.
Sinalizadores | Os sinalizadores que descrevem o smart card.
Middleware | O middleware para o smart card.
ParentSmartcardUuid | O identificador do smart card antigo que substituiu o smart card.
PermanentSmartcardUuid | O identificador do smart card permanente que estão associado com o smart card.
PrimarySmartcardUuid | O identificador do smart card primário.
ProfileTemplateUuid | O identificador do modelo de perfil que contém as políticas e definições que regem o smart card.
ProfileTemplateVersion | A versão do momento em que o perfil de smart card foi criado o modelo de perfil.
SerialNumber | Número de série do smart card.
Estado | O estado do smart card.
UUID | Identificador do perfil de smart card.

## <a name="example"></a>Exemplo

### <a name="request"></a>Pedido
```
PUT /certificatemanagement/api/v1.0/requests/b105403d-d021-41ea-9f11-be3d677d229e/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1

```
### <a name="response"></a>Resposta
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
## <a name="see-also"></a>Consulte Também

- [Classe de Microsoft.Clm.Shared.Smartcards.Smartcard](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx))
