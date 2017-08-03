---
title: "Cancelar, abandonar ou conclua uma requisição | Microsoft Docs"
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
ms.assetid: e29e0068-7602-455e-8a3a-690da9ca8eb5
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4f81c9d86006c477993b2ac8cc2e845de2c1d2f3
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="cancel-abandon-or-complete-a-request"></a>Cancelar, abandonar ou concluir um pedido
Marcar um pedido de MIM CM como concluída, cancelada ou abandonado.

**Tenha em atenção**: URLs apresentados neste tópico são relativamente ao nome do anfitrião selecionado durante a implementação de API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Pedido


Método  |URL do pedido  
---------|---------
COLOCAR     |/ CertificateManagement/api/v1.0/requests/{id}

###<a name="url-parameters"></a>Parâmetros de URL
Propriedade| Descrição
---------|--------
ID| Necessário. O GUID do pedido para concluir.


###<a name="request-headers"></a>Cabeçalhos de pedido
Para cabeçalhos de pedido comuns, consulte [pedido HTTP e cabeçalhos da resposta](certificate-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do CM*.
###<a name="request-body"></a>Corpo do pedido
O corpo do pedido contém as seguintes propriedades.

Propriedade | Descrição
---------|-----------
Estado | O estado a definir o pedido. Os valores possíveis são: "Concluída", "Cancelado" ou "Abandoned".


##<a name="response"></a>Resposta
###<a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
200     | OK
204 | Nenhum conteúdo
403 | Proibido
500 | Erro interno

###<a name="response-headers"></a>Cabeçalhos de Resposta
Para cabeçalhos de resposta comuns, consulte [pedido HTTP e cabeçalhos da resposta](certificate-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do CM*.
###<a name="response-body"></a>Corpo da resposta
Com êxito, devolve uma [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx) objeto com as seguintes propriedades que descreve o pedido de MIM CM foi marcado como concluída:

Nome | Descrição
-----|------------
Comentário | O comentário que estão associado com o pedido de MIM CM.
Concluído | O tempo que o pedido de MIM CM foi concluído.
DataCollection | Os itens de recolha de dados que estão associados o pedidos de MIM CM.
DataCollectionFlags | As opções para a recolha de dados para o pedido de MIM CM.
Sinalizadores | As opções que estão associadas o pedidos de MIM CM.
IsDataCollectionComplete | Um valor booleano que indica se a recolha de dados foi concluída para o pedido de MIM CM.
IsEnrollmentAgent | Um valor booleano que indica se um agente de inscrição é requerido para executar o pedido de MIM CM.
IsSmartcard | Um valor booleano que indica se o pedido de MIM CM é um pedido de smart card ou um pedido de perfil de software.
NewProfileUuid | A identidade do novo perfil de software que é produzida por pedido de MIM CM.
NewSmartcardUuid | A identidade do novo smart card atribuído pelo pedido de MIM CM.
OldProfileUuid | A identidade do perfil de software para o qual o pedido de MIM CM foi criado.
OldSmartcardUuid | A identidade do smart card para o qual o pedido de MIM CM foi criado.
OriginatorUserUuid | A identidade do utilizador que teve origem do pedido de MIM CM
Prioridade | Prioridade do pedido de MIM CM.
ProfileTemplateUuid | A identidade de modelo de perfil para o qual o pedido de MIM CM foi criado.
RequestType | O tipo de pedido de MIM CM.
SecurityDescriptor | O descritor de segurança para o pedido de MIM CM.
Estado | O estado do pedido de MIM CM.
Foi submetido | O tempo que o pedido de MIM CM foi submetido.
TargetUserUuid | A identidade do utilizador de destino para o pedido de MIM CM.
UUID | O identificador para o pedido de MIM CM.

##<a name="example"></a>Exemplo

###<a name="request"></a>Pedido
```
PUT /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1


{
    “status”:”Completed”
}
```
###<a name="response"></a>Resposta
```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":8,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"2015-07-07T23:37:37.767Z",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```       
##<a name="see-also"></a>Consulte Também

- [Método de Microsoft.Clm.Provision.ExecuteOperations.Complete](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.complete.aspx)
- [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx)
