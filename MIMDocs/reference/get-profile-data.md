---
title: Obtenha dados de perfil Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 62b4c336122376cd7da836b9ec6c6e09eac24729
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762076"
---
# <a name="get-profile-data"></a>Obtenha dados de perfil
Obtém uma lista de perfis de certificados de software para um utilizador. A lista inclui as possíveis operações que podem ser realizadas pelo utilizador atual. Em seguida, pode ser iniciado um pedido para qualquer uma das operações especificadas.

>[!IMPORTANT]
>O servidor só define o PIN se a política do modelo de perfil indicar que deve ser feita. Caso contrário, o utilizador deve fornecer o PIN.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
GET     |/CertificadosManagement/api/v1.0/profiles<br/>/CertificadoManagement/api/v1.0/profiles/{id} <br/>/CertificadoManagement/api/v1.0/requests/{requestid}/perfis

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro | Descrição
---------|------------
ID | O identificador (GUID) do perfil a devolver.
requestId | O identificador do pedido de devolução dos perfis.

### <a name="query-parameters"></a>Parâmetros de consulta

Parâmetro | Descrição
---------|------------
status | Opcional. Indica o estado dos perfis para os quais recuperar dados. Os tipos de estado possíveis são "Ativo", "Aprovado", "Cancelado", "Concluído", "Negado", "Executante", "Falhado", "Nenhum" e "Pendente". <br/>Se não for especificado nenhum estado, todos os perfis, independentemente do estado, são devolvidos.

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
No sucesso, devolve uma lista de [objetos microsoft.clm.shared.profiles](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx) com as seguintes propriedades:

Propriedade | Descrição
---------|------------
DesignadosUserUuid | O identificador do utilizador a quem o perfil é atribuído.
Comentário | O comentário que descreve o perfil.
Sinalizadores | As bandeiras que descrevem o perfil.
ParentProfileUuid | O identificador do perfil antigo que o perfil substituiu.
PrimaryProfileUuid | O identificador do perfil principal.
Operações de Perfis | A lista de possíveis operações que podem ser realizadas pelo utilizador atual no perfil.
ProfileTemplateUuid | O identificador do modelo de perfil que contém as políticas e configurações que regem o perfil.
ProfileTemplateVersion | A versão do modelo de perfil no momento em que o perfil foi criado.
Estado | O estado do perfil.
Uuid | O identificador do perfil.


## <a name="example"></a>Exemplo
Esta secção fornece um exemplo para obter os dados de perfil de um utilizador.

### <a name="example-request"></a>Exemplo: Pedido

```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```

### <a name="example-response"></a>Exemplo: Resposta

```
HTTP/1.1 200 OK

[
    {
        "Uuid":"c0dd5c7d-ec35-4346-baca-3ad711e9722f",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"1c9e2606-fea2-4048-a6ac-b014e54c22df",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"5ad77b40-aa42-4533-9396-c9c59fd021a8",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"ff342953-c444-4dc7-b144-f5515d6460c6",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"1e3a31fe-699b-4a6b-945c-18b83c985bc1",
        "ProfileTemplateVersion":9,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable"
        ]
    }
]
```

## <a name="see-also"></a>Ver também

- [Microsoft.Clm.Shared.Profiles.Profile class](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx)
