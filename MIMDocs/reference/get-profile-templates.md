---
title: Obtenha modelos de perfil / Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9f14853b3895dece1098270d35439d6c4999be2b
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762056"
---
# <a name="get-profile-templates"></a>Obtenha modelos de perfil
Obtém uma lista de modelos de perfil para os quais o utilizador especificado pode inscrever-se. Este método devolve uma visão limitada do modelo de perfil. Os dados do modelo de perfil devolvidos devem ser suficientes para permitir ao utilizador que solicita decidir qual o modelo de perfil, caso existam, que precisam de se inscrever. Se não for especificado fluxo de trabalho e permissão, todos os modelos de perfil que sejam visíveis ao utilizador são devolvidos.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates? \[ targetuser\] 

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro| Descrição
--------|-------------
targetuser| Opcional. Especifica o utilizador-alvo para retornar os modelos de perfil para. Se não for especificada, utiliza-se a identidade do utilizador atual. <br/><br/>**Nota:** Atualmente, apenas o utilizador atual está suportado.

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
500 | Erro Interno

### <a name="response-headers"></a>Cabeçalhos de resposta
Para cabeçalhos de resposta comuns, consulte [os cabeçalhos de pedido e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *dados do serviço cm REST API* .

### <a name="response-body"></a>Corpo da resposta
No sucesso, devolve uma lista de objetos ProfileTemplateLimitedView com as seguintes propriedades:

Propriedade| Tipo| Descrição
--------|-----|--------
Nome| cadeia| O nome de exibição do modelo de perfil.
Description| cadeia (de carateres)| A descrição para o modelo de perfil.
Uuid| GUID| O identificador para o modelo de perfil.
IsSmartcardProfileTemplate| bool| Indica se o modelo é um modelo de perfil de cartão inteligente.
IsVirtualSmartcardProfileTemplate| bool| Indica se o modelo de perfil requer um cartão inteligente virtual.

## <a name="example"></a>Exemplo
Esta secção fornece um exemplo para obter a lista de modelos de perfil para o utilizador especificado.

### <a name="example-request"></a>Exemplo: Pedido

```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```

### <a name="example-response"></a>Exemplo: Resposta

```
HTTP/1.1 200 OK

[
    {
        "Name":"FIM CM Sample Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"12bd5120-86a2-4ee1-8d05-131066871578",
        "IsSmartcardProfileTemplate":false,
        "IsVirtualSmartcardProfileTemplate":false
    },
    {
        "Name":"FIM CM Sample Smart Card Logon Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"2b7044cf-aa96-4911-b886-177947e9271b",
        "IsSmartcardProfileTemplate":true,
        "IsVirtualSmartcardProfileTemplate":false
    }
]
```       
