---
title: "Obtenção do pedido | Microsoft Docs"
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
ms.assetid: dcacf36c-0670-44d7-9f40-388667235271
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 56bb25f7282879943dc624f3c24b836c7ae4a58c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="get-request"></a>Obtenção do pedido
Obtém um ou vários pedidos de MIM CM especificados.

**Tenha em atenção**: URLs apresentados neste tópico são relativamente ao nome do anfitrião selecionado durante a implementação de API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Pedido


Método  |URL do pedido  
---------|---------
GET     |/ CertificateManagement/api/v1.0/requests/{id}

###<a name="url-parameters"></a>Parâmetros de URL
Propriedade| Descrição
---------|--------
ID| Opcional. O GUID de um pedido obter.
###<a name="query-parameters"></a>Parâmetros de consulta
Propriedade| Descrição
---------|--------
targetuser| Opcional. O utilizador de destino do pedido. Não se for especificado nenhum destino, todos os pedidos independentemente do utilizador de destino, serão devolvidos. <br/> **Tenha em atenção**: "Atual" só é suportada atualmente.
Estado| Opcional. Indica o estado do pedido a obter. Os tipos de estado possíveis são: *aprovado*, *cancelado*, *concluído*, *negado*, *Executing*, *falha*, *nenhum*, e *pendente*. <br/>Não se for especificado nenhum Estado, serão devolvidos todos os pedidos independentemente do Estado.

###<a name="request-headers"></a>Cabeçalhos de pedido
Para cabeçalhos de pedido comuns, consulte [pedido HTTP e cabeçalhos da resposta](certificate-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do CM*.
###<a name="request-body"></a>Corpo do pedido
nenhum

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
Com êxito, devolve uma ou mais [pedido](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx) objetos com as seguintes propriedades:

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

###<a name="request-1"></a>Pedido 1
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1

```
###<a name="response-1"></a>Resposta 1
```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":3,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"0001-01-01T00:00:00",
    "NewProfileUuid":"00000000-0000-0000-0000-000000000000",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}```       
###Request 2
```
OBTER /certificatemanagement/api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc HTTP/1.1
```
###Response 2
```
HTTP/1.1 200 OK

{"Uuid": "0c96d73f-967b-420e-854a-43ad2a1504bc", "RequestType": 5, "Estado": 3, "sinalizadores": 2, "o DataCollection": [{"Name": "Exemplo Item de dados", "Valor": nulo}], "DataCollectionFlags": 32, "OriginatorUserUuid": "8f1590dc-d932-4b66-8e68-2e91c5880780", "TargetUserUuid": "8f1590dc-d932-4b66-8e68-2e91c5880780", "Submeter": "2015-07-07T23:40:33.133Z", "Concluída": "0001 e-01-01T00:00:00", "NewProfileUuid": "b99ff38c-6653-471f-ae3c-325bb351a6bc", "OldProfileUuid": "b99ff38c-6653-471f-ae3c-325bb351a6bc", "NewSmartcardUuid": "17cf063d-e337-4aa9-a822-346554ddd3c9", "OldSmartcardUuid": "17cf063d-e337-4aa9-a822-346554ddd3c9", "Prioridade": 0, "Comment": "", "ProfileTemplateUuid": "a039b4d0-5ce8-4eff-8651-181c6576fda3", "SecurityDescriptor": "O:S-1-5-21-2127521184-1604012920-1887927527-14134865 D:(D;; LC;; S-1-5-21-2127521184-1604012920-1887927527-14995557) (UMA; SWRC;; S-1-5-21-2127521184-1604012920-1887927527-5094533) (UMA; RC;; WD) (UMA; CCDCLCSWSDRC;; S-1-5-21-2127521184-1604012920-1887927527-14134865) (A; D CSW;; S-1-5-21-2127521184-1604012920-1887927527-14995557) (UMA; CCDCSW;; S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000 ","IsSmartcard": true,"IsEnrollmentAgent": false,"IsDataCollectionComplete": false}
```       

##See Also

- [Microsoft.Clm.Provision.FindOperations.FindRequest Method](http://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.findoperations.findrequests.aspx)
- [Microsoft.Clm.Shared.RequestPermission Enumeration](http://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requestpermission.aspx)
- [Microsoft.Clm.Shared.Requests.Request Class](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx)
