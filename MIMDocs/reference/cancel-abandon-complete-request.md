---
title: Cancelar, abandonar ou completar um pedido Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2016
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: e29e0068-7602-455e-8a3a-690da9ca8eb5
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e971309963968ddd52ef44644fb9319738df6242
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762107"
---
# <a name="cancel-abandon-or-complete-a-request"></a>Cancelar, abandonar ou completar um pedido
Marque um pedido de Gestão de Certificados (CM) do Microsoft Identity Manager (MIM) conforme concluído, cancelado ou abandonado.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
PUT     |/CertificadoManagement/api/v1.0/requests/{id}

### <a name="url-parameters"></a>Parâmetros de URL

Propriedade| Descrição
---------|--------
ID| Obrigatório. O GUID do pedido para completar.


### <a name="request-headers"></a>Cabeçalhos do pedido
Para cabeçalhos de pedido comuns, consulte [os cabeçalhos de pedido e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *dados do serviço cm REST API* .

### <a name="request-body"></a>Corpo do pedido
O organismo de pedido contém as seguintes propriedades:

Propriedade | Descrição
---------|-----------
status | O estado para definir o pedido para. Os valores possíveis são "Concluídos", "Cancelados" ou "Abandonados".


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
No sucesso, retorna um objeto [Microsoft.Clm.Shared.Requests.Requests.Request.](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx) O objeto descreve o pedido mim CM que foi marcado como concluído. O objeto contém as seguintes propriedades:

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
Esta secção fornece um exemplo para marcar um pedido como concluído.

### <a name="example-request"></a>Exemplo: Pedido

```
PUT /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1


{
    "status":"Completed"
}
```

### <a name="example-response"></a>Exemplo: Resposta

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

## <a name="see-also"></a>Ver também

- [Microsoft.Clm.Provision.Exemétodo fofoOperações.Complete](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.complete.aspx)
- [Microsoft.Clm.Shared.Requests.Requests.Request class](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx)
