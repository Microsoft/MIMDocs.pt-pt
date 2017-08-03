---
title: Criar pedido | Microsoft Docs
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
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d5af8767583cb6999de399de58b321147846291a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="create-request"></a>Criar pedido
Crie um pedido de MIM CM.

**Tenha em atenção**: URLs apresentados neste tópico são relativamente ao nome do anfitrião selecionado durante a implementação de API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Pedido


Método  |URL do pedido  
---------|---------
POST     |/CertificateManagement/API/v1.0/Requests

###<a name="url-parameters"></a>Parâmetros de URL
nenhum

###<a name="request-headers"></a>Cabeçalhos de pedido
Para cabeçalhos de pedido comuns, consulte [pedido HTTP e cabeçalhos da resposta](certificate-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do CM*.
###<a name="request-body"></a>Corpo do pedido
O corpo do pedido contém as seguintes propriedades.

Propriedade | Descrição
---------|-----------
profiletemplateuuid | Necessário. O GUID do modelo de perfil que o utilizador está a criar o pedido para.
datacollection | Necessário. Uma coleção de pares nome-valor que representa os dados que está a ser fornecida pelo enrollee. A recolha de dados necessárias que têm de ser fornecidos pode ser obtida da política de fluxo de trabalho o modelo de perfil. É possível especificar uma coleção vazia.
destino | Opcional. O GUID do utilizador de destino que o pedido está a ser criada para. Se não for especificado, este assume a predefinição do utilizador atual.
tipo | Necessário. O tipo de pedido que está a ser criado. Tipos de pedido disponíveis são: "Inscrição", "Duplicado", "OfflineUnblock", "OnlineUpdate", "Renovar", "Recuperar", "RecoverOnBehalf", "Restabelecimento", "Extinguir", "Revogar", "TemporaryCards" e "Desbloqueio".<br/>**Tenha em atenção**: nem todos os tipos de pedido são suportados em todos os modelos de perfil. Por exemplo, não é possível especificar uma operação de desbloqueio no modelo de perfil de software.
comentário | Necessário. Quaisquer comentários que podem ser introduzidos pelo utilizador. A política de fluxo de trabalho irá definir se isto é necessário. É possível especificar uma cadeia vazia.
prioridade | Opcional. A prioridade do pedido. Se não for especificado, a prioridade de pedido predefinido, conforme determinado pelas definições de modelo de perfil, será utilizada.


##<a name="response"></a>Resposta
###<a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
201     | Criado
403 | Proibido
500 | Erro interno

###<a name="response-headers"></a>Cabeçalhos de Resposta
Para cabeçalhos de resposta comuns, consulte [pedido HTTP e cabeçalhos da resposta](certificate-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do CM*.
###<a name="response-body"></a>Corpo da resposta
Com êxito, devolve o URI do pedido criado recentemente.
##<a name="example"></a>Exemplo

###<a name="request-1"></a>Pedido 1
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```
###<a name="response-1"></a>Resposta 1
```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```
###<a name="request-2"></a>Pedido de 2
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```
###<a name="response-2"></a>Resposta 2
```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

###<a name="request-3"></a>Pedido 3
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "profiletemplateuuid" : "97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
    "datacollection":
    [
        {"name" : "pavle"},
        {"city" : "seattle"}
    ],
    "target" : "97CC3493-F556-4C9B-9D8B-982434201527",
    "type" : "Enroll",
    "comment" : "LALALALA",
    "priority" :  "4"
}
```
##<a name="see-also"></a>Consulte Também

- [Método de Microsoft.Clm.Provision.RequestOperations.InitiateEnroll](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll.aspx)
- [Método de Microsoft.Clm.Provision.RequestOperations.InitiateOfflineUnblock](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock.aspx)
- [Método de Microsoft.Clm.Provision.RequestOperations.InitiateRecover](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover.aspx)
- [Método de Microsoft.Clm.Provision.RequestOperations.InitiateRetire](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire.aspx)
- [Método de Microsoft.Clm.Provision.RequestOperations.InitiateUnblock](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock.aspx)
