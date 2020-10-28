---
title: Atribuir um cartão inteligente a um pedido Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: a8acf3defc2087fc6ea08bf8063579058465fcc8
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762126"
---
# <a name="assign-a-smart-card-to-a-request"></a>Atribuir um cartão inteligente a um pedido
Liga o cartão inteligente especificado ao pedido especificado. Depois de um cartão inteligente ser vinculado, o pedido só pode ser executado com o cartão especificado.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
POST     |/CertificadoManagement/api/v1.0/smartcards

### <a name="url-parameters"></a>Parâmetros de URL
Nenhum.

### <a name="request-headers"></a>Cabeçalhos do pedido
Para cabeçalhos de pedido comuns, consulte [os cabeçalhos de pedido e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *dados do serviço cm REST API* .

### <a name="request-body"></a>Corpo do pedido
O organismo de pedido contém as seguintes propriedades:

Propriedade | Descrição
---------|-----------
requestid | A identificação do pedido para ligar ao cartão inteligente.
cardid | O cardídeo do cartão inteligente.
atr | A cadeia de resposta ao reset do cartão inteligente (ATR).


## <a name="response"></a>Resposta
Esta secção descreve a resposta.

### <a name="response-codes"></a>Códigos de resposta

Código  |Descrição  
---------|---------
201 | Criado
204 | Sem conteúdo
403 | Proibido
500 | Erro Interno

### <a name="response-headers"></a>Cabeçalhos de resposta
Para cabeçalhos de resposta comuns, consulte [os cabeçalhos de pedido e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *dados do serviço cm REST API* .

### <a name="response-body"></a>Corpo da resposta
No sucesso, devolve um URI ao objeto de cartão inteligente recém-criado.

## <a name="example"></a>Exemplo
Esta secção fornece um exemplo para ligar um cartão inteligente.

### <a name="example-request"></a>Exemplo: Pedido

```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}
```

### <a name="example-response"></a>Exemplo: Resposta

```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```

## <a name="see-also"></a>Ver também

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard método (String, String, Request)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
