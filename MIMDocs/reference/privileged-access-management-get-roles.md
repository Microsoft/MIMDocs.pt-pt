---
title: Obtenha papéis PAM ! Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: d3c4f528-c3c8-41c1-905e-7eb84f074ce4
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f802dc22019f1ad97fc5cce8c9ed2e000349dc58
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762442"
---
# <a name="get-pam-roles"></a>Obtenha papéis DE PAM
Usado por uma conta privilegiada para listar as funções PAM para as quais a conta é candidata.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
GET     |/api/pamresources/pamroles

### <a name="query-parameters"></a>Parâmetros de consulta

Parâmetro | Descrição
----------|--------------
$filter | Opcional. Especifique qualquer uma das propriedades da função PAM numa expressão de filtro para devolver uma lista filtrada de respostas. Para obter mais informações sobre operadores apoiados, consulte [a filtragem nos dados do serviço API PAM REST](privileged-access-management-rest-api-service-details.md#filtering).
v | Opcional. A versão API. Se não estiver incluída, utiliza-se a versão atual (mais recentemente lançada) da API. Para obter mais informações, consulte a versão em detalhes do [serviço API PAM REST](privileged-access-management-rest-api-service-details.md#versioning).

### <a name="request-headers"></a>Cabeçalhos do pedido
Para cabeçalhos de pedido comuns, consulte [os cabeçalhos de pedido e resposta](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) HTTP nos *dados do serviço API PAM REST* .

### <a name="request-body"></a>Corpo do pedido
Nenhum.

## <a name="response"></a>Resposta
Esta secção descreve a resposta.

### <a name="response-codes"></a>Códigos de resposta

Código  |Descrição  
---------|---------
200 | OK
401 | Não autorizado
403 | Proibido
408 | Tempo Limite do Pedido   
500 | Erro interno do servidor
503 | Serviço Indisponível

### <a name="response-headers"></a>Cabeçalhos de resposta
Para cabeçalhos de pedido comuns, consulte [os cabeçalhos de pedido e resposta](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) HTTP nos *dados do serviço API PAM REST* .

### <a name="response-body"></a>Corpo da resposta
Uma resposta bem sucedida contém uma coleção de uma ou mais funções PAM, cada uma das quais tem as seguintes propriedades:

Propriedade | Descrição
--------|-------------
RoleID | O identificador único (GUID) do papel PAM.
DisplayName | O nome de exposição da função THe PAM no serviço MIM.
Descrição | A descrição do papel PAM no serviço MIM.
TTL | O tempo máximo de validade dos direitos de acesso da função em segundos.
Disponível De | A hora mais cedo do dia quando um pedido é ativado.
Disponível Para | A última hora do dia quando um pedido é ativado.
MFAEnabled | Um valor Boolean que indica se os pedidos de ativação para esta função requerem um desafio MFA.
AprovaçãoEnabled | Um valor Boolean que indica se os pedidos de ativação para esta função requerem a aprovação de um proprietário de funções.
DisponibilidadeWindowEnabled | Um valor Boolean que indica se a função só pode ser ativada durante um intervalo de tempo especificado.

## <a name="example"></a>Exemplo
Esta secção fornece um exemplo para obter as funções PAM.

### <a name="example-request"></a>Exemplo: Pedido

```
GET /api/pamresources/pamroles HTTP/1.1
```

### <a name="example-response"></a>Exemplo: Resposta

```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamroles",
    "value":[
        {
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "DisplayName":"Allow AD Access ",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":false,
            "AvailabilityWindowEnabled":false
        },
        {
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "DisplayName":"ApprovalRole",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":true,
            "AvailabilityWindowEnabled":false
        }
    ]
}
```       
