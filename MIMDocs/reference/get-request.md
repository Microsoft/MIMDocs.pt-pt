---
title: Obter | de pedido Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: dcacf36c-0670-44d7-9f40-388667235271
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1bc42eb0fb1e54a3425586350ae5ad20495534c5
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835710"
---
# <a name="get-request"></a>Obter pedido
Obtém um ou mais pedidos especificados do Microsoft Identity Manager (MIM) Certificate Management (CM).

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
GET     |/CertificadoManagement/api/v1.0/requests/{id}

### <a name="url-parameters"></a>Parâmetros de URL

Propriedade| Descrição
---------|--------
ID| Opcional. O GUID de um pedido de recuperação.

### <a name="query-parameters"></a>Parâmetros de consulta

Propriedade| Descrição
---------|--------
targetuser| Opcional. O utilizador-alvo do pedido. Se não for especificado nenhum alvo, todos os pedidos, independentemente do utilizador-alvo, serão devolvidos. <br/><br/>**Nota:** Apenas o valor "corrente" é suportado atualmente.
status| Opcional. Indica o estado do pedido de recuperação. Os tipos de estado possíveis são "Aprovados", "Cancelados", "Concluídos", "Negados", "Executando", "Falhado", "Nenhum" e "Pendente". <br/>Se não for especificado nenhum estado, todos os pedidos, independentemente do estado, são devolvidos.

### <a name="request-headers"></a>Cabeçalhos do pedido
Para cabeçalhos de pedido comuns, consulte [os cabeçalhos de pedido e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *dados do serviço cm REST API*.

### <a name="request-body"></a>Corpo do pedido
Nenhum.

## <a name="response"></a>Resposta
Esta secção descreve a resposta.

### <a name="response-codes"></a>Códigos de resposta

Código  |Description  
---------|---------
200 | OK
204 | Sem conteúdo
403 | Proibido
500 | Erro Interno

### <a name="response-headers"></a>Cabeçalhos de resposta
Para cabeçalhos de resposta comuns, consulte [os cabeçalhos de pedido e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *dados do serviço cm REST API*.

### <a name="response-body"></a>Corpo da resposta
No sucesso, devolve um ou mais objetos [de pedido](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx) com as seguintes propriedades:

Nome | Descrição
-----|------------
Comentário | O comentário que está associado ao pedido da MIM CM.
Concluído | O momento em que o pedido do CM MIM foi concluído.
DataCollection | Os itens de recolha de dados associados ao pedido mim CM.
DataCollectionFlags | As opções de recolha de dados para o pedido mim CM.
Sinalizadores | As opções que estão associadas ao pedido mim CM.
IsDataCollectionComplete | Um valor Boolean que indica se a recolha de dados foi concluída para o pedido mim CM.
IsEnrollmentAgent | Um valor Boolean que indica se um agente de inscrição é obrigado a executar o pedido mim CM.
IsSmartcard | Um valor Boolean que indica se o pedido mim CM é um pedido de cartão inteligente ou um pedido de perfil de software.
NewProfileUuid | A identidade do novo perfil de software que é produzido pelo pedido mim CM.
NewSmartcardUuid | A identidade do novo cartão inteligente que é atribuído pelo pedido mim CM.
OldProfileUuid | A identidade do perfil de software para o qual foi criado o pedido do CM MIM.
OldSmartcarduuid | A identidade do cartão inteligente para o qual foi criado o pedido do CM MIM.
OriginatorUserUuid | A identidade do utilizador que originou o pedido mim CM.
Prioridade | A prioridade do pedido mim CM.
ProfileTemplateUuid | A identidade do modelo de perfil para o qual foi criado o pedido mim CM.
RequestType | O tipo de pedido mim CM.
Descriturão de segurança | O descritor de segurança para o pedido mim CM.
Estado | O estado do pedido do CM MIM.
Submetido | O momento em que o pedido da MIM CM foi apresentado.
TargetUserUuid | A identidade do utilizador-alvo para o pedido mim CM.
Uuid | O identificador do pedido do CM MIM.

## <a name="example"></a>Exemplo
Esta secção fornece um exemplo para obter os pedidos de MIM CM especificados.

### <a name="example-request-1"></a>Exemplo: Pedido 1

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1
```

### <a name="example-response-1"></a>Exemplo: Resposta 1

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
}
```

### <a name="example-request-2"></a>Exemplo: Pedido 2

```
GET /certificatemanagement/api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc HTTP/1.1
```

### <a name="example-response-2"></a>Exemplo: Resposta 2

```
HTTP/1.1 200 OK

{
    "Uuid":"0c96d73f-967b-420e-854a-43ad2a1504bc",
    "RequestType":5,
    "Status":3,
    "Flags":2,
    "DataCollection":[
        {
            "Name":"Sample Data Item",
            "Value":null
        }
    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:40:33.133Z",
    "Completed":"0001-01-01T00:00:00",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```     

## <a name="see-also"></a>Ver também

- [Microsoft.Clm.Provision.FindOperations.FindRequest método](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.findoperations.findrequests.aspx)
- [Microsoft.Clm.Shared.RequestPermission enumeração](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requestpermission.aspx)
- [Microsoft.Clm.Shared.Requests.Requests.Request class](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.requests.request.aspx)
