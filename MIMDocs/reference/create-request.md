---
title: Criar pedidos Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 51016951f9dec22e2a40b44a2b76a840192b518f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762257"
---
# <a name="create-request"></a>Criar pedido
Crie um pedido de Gestão de Certificados (MIM) do Microsoft Identity Manager (MIM).

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
POST     |/CertificadosManagement/api/v1.0/requests

### <a name="url-parameters"></a>Parâmetros de URL
Nenhum.

### <a name="request-headers"></a>Cabeçalhos do pedido
Para cabeçalhos de pedido comuns, consulte [os cabeçalhos de pedido e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *dados do serviço cm REST API* .

### <a name="request-body"></a>Corpo do pedido
O organismo de pedido contém as seguintes propriedades:

Propriedade | Descrição
---------|-----------
profiletemplateuid | Obrigatório. O GUID do modelo de perfil que o utilizador está a criar o pedido.
recolha de dados | Obrigatório. Uma recolha de pares de valor-nome que representam os dados que devem ser fornecidos pela inscrição. A recolha de dados necessários que devem ser fornecidos pode ser recuperada da política de fluxo de trabalho do modelo de perfil. Uma coleção vazia pode ser especificada.
alvo | Opcional. O GUIA do utilizador-alvo para o qual o pedido deve ser criado. Se não for especificado, o alvo é padrão para o utilizador atual.
tipo | Obrigatório. O tipo de pedido que está a ser criado. Os tipos de pedidos disponíveis incluem "Matriculado", "Duplicado", "OfflineUnblock", "OnlineUpdate", "Renovar", "Recuperar", "Recuperar Metade", "Reintegrar", "Aposentar", "Revogar", "Cartões Temporários" e "Desbloquear".<br/><br/>**Nota:** Nem todos os tipos de pedidos são suportados em todos os modelos de perfil. Por exemplo, não é possível especificar a operação Desbloqueio num modelo de perfil de software.
comentário | Obrigatório. Quaisquer comentários que possam ser introduzidos pelo utilizador. A política de fluxo de trabalho define se a propriedade de comentário é necessária. Uma corda vazia pode ser especificada.
prioridade | Opcional. A prioridade do pedido. Se não for especificado, é utilizada a prioridade de pedido por defeito, determinada pelas definições do modelo de perfil.


## <a name="response"></a>Resposta
Esta secção descreve a resposta.

### <a name="response-codes"></a>Códigos de resposta

Código  |Descrição  
---------|---------
201 | Criado
403 | Proibido
500 | Erro Interno

### <a name="response-headers"></a>Cabeçalhos de resposta
Para cabeçalhos de resposta comuns, consulte [os cabeçalhos de pedido e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *dados do serviço cm REST API* .

### <a name="response-body"></a>Corpo da resposta
No sucesso, devolve o URI para o pedido recém-criado.

## <a name="example"></a>Exemplo
Esta secção fornece um exemplo para criar pedidos de inscrição e desbloqueio.

### <a name="example-request-1"></a>Exemplo: Pedido 1

```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```

### <a name="example-response-1"></a>Exemplo: Resposta 1

```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```

### <a name="example-request-2"></a>Exemplo: Pedido 2

```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```

### <a name="example-response-2"></a>Exemplo: Resposta 2

```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

### <a name="example-request-3"></a>Exemplo: Pedido 3

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

## <a name="see-also"></a>Ver também

- [Microsoft.Clm.Provision.RequestOperations.Inimétodo tiateEnroll](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll.aspx)
- [Microsoft.Clm.Provision.RequestOperations.Inimétodo tiateOfflineUnblock](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock.aspx)
- [ método tiaterecoverMicrosoft.Clm.Provision.RequestOperations.Ini](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover.aspx)
- [Microsoft.Clm.Provision.RequestOperations.Inimétodo tiateRetire](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire.aspx)
- [Microsoft.Clm.Provision.RequestOperations.Inimétodo tiateUnblock](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock.aspx)
