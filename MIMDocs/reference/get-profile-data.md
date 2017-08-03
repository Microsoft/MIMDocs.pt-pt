---
title: Obter dados de perfil | Microsoft Docs
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
ms.assetid: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 068497509b07b879fcadde6b60a9530d3d8db093
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-data"></a>Obter dados de perfil
Obtém uma lista de perfis de certificado de software de um utilizador com uma lista de possíveis operações que podem ser efetuadas pelo utilizador atual. Um pedido, em seguida, pode ser iniciado para qualquer uma das operações especificadas.

**Tenha em atenção**: O servidor irá definir o PIN apenas se a política de modelo de perfil indica que deve ser feito. Caso contrário, o utilizador deve fornecer.

**Tenha em atenção**: URLs apresentados neste tópico são relativamente ao nome do anfitrião selecionado durante a implementação de API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Pedido


Método  |URL do pedido  
---------|---------
GET     |/CertificateManagement/API/v1.0/Profiles<br/>/ CertificateManagement/api/v1.0/profiles/{id} <br/>/CertificateManagement/API/v1.0/Requests/{RequestId}/Profiles

###<a name="url-parameters"></a>Parâmetros de URL
Parâmetro | Descrição
---------|------------
ID | O identificador (GUID) do perfil para devolver.
requestId | O identificador do pedido para devolver os perfis para.

###<a name="query-parameters"></a>Parâmetros de consulta
Parâmetro | Descrição
---------|------------
Estado | Opcional. Indica o estado dos perfis para obter dados. Os tipos de estado possíveis são: "Ativo", "Aprovado", "Cancelada", "Concluída", "Negado", "Em execução", "Falha", "None" e "Pendentes". <br/>Não se for especificado nenhum Estado, serão devolvidos todos os perfis, independentemente do Estado.
###<a name="request-headers"></a>Cabeçalhos de pedido
Para cabeçalhos de pedido comuns, consulte [pedido HTTP e cabeçalhos da resposta](certificate-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do CM*.
###<a name="request-body"></a>Corpo do pedido
nenhum

##<a name="response"></a>Resposta
###<a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
200 | OK
204 | Nenhum conteúdo
403 | Proibido
500 | Erro interno

###<a name="response-headers"></a>Cabeçalhos de Resposta
Para cabeçalhos de resposta comuns, consulte [pedido HTTP e cabeçalhos da resposta](certificate-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do CM*.
###<a name="response-body"></a>Corpo da resposta
Com êxito, devolve uma lista de JSON serializado [Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx) objetos com as seguintes propriedades:

Propriedade | Descrição
---------|------------
AssignedUserUuid | O identificador do utilizador a quem é atribuído o perfil.
Comentário | O comentário que descreve o perfil.
Sinalizadores | Os sinalizadores que descrevem o perfil.
ParentProfileUuid | O identificador do perfil antigo que substituiu o perfil.
PrimaryProfileUuid | O identificador do perfil primário.
ProfileOperations | A lista de possíveis operações que podem ser efetuadas pelo utilizador atual no perfil.
ProfileTemplateUuid | O identificador do modelo de perfil que contém as políticas e definições que regem o perfil.
ProfileTemplateVersion | A versão do momento em que o perfil foi criado o modelo de perfil.
Estado | O estado do perfil.
UUID | Identificador do perfil.


##<a name="example"></a>Exemplo

###<a name="request"></a>Pedido
```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```
###<a name="response"></a>Resposta
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
##<a name="see-also"></a>Consulte Também

- [Classe de Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx)
