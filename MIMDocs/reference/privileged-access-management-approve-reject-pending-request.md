---
title: Aprovar ou rejeitar um pedido pendente de PAM | Microsoft Docs
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
ms.assetid: 0632656f-ecf4-4090-85a8-216d5638140a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3e5506f96a22d1918cff7d0c9b822babb0f6cfa9
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="approve-or-reject-a-pending-pam-request"></a>Aprovar ou rejeitar um pedido de PAM pendente
Utilizar uma conta com privilégios para aprovar, fechar ou rejeitar um pedido de elevação a uma função de PAM.

**Tenha em atenção**: URLs apresentados neste tópico são relativamente ao nome do anfitrião selecionado durante a implementação de API; por exemplo: `http://api.contoso.com`.
##<a name="request"></a>Pedido


Método  |URL do pedido  
---------|---------
POST     |/API/pamresources/pamrequeststoapprove({approvalId)/Approve <br/>/API/pamresources/pamrequeststoapprove({approvalId)/Reject

###<a name="url-parameters"></a>Parâmetros de URL
Parâmetro | Descrição
----------|-----------
approvalId | O identificador (GUID) do objeto de aprovação na PAM especificado da seguinte forma`guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'`

###<a name="query-parameters"></a>Parâmetros de consulta
Parâmetro | Descrição
----------|--------------
v | Opcional. A versão de API. Se não incluído, será utilizada a versão atual (mais recentemente lançada) da API. Para obter mais informações, consulte [os detalhes do serviço de API de REST do PAM de controlo de versões](privileged-access-management-rest-api-service-details.md#versioning)


###<a name="request-headers"></a>Cabeçalhos de pedido
Para cabeçalhos de pedido comuns, consulte [pedido HTTP e cabeçalhos da resposta](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do PAM*.
###<a name="request-body"></a>Corpo do pedido
nenhum.

##<a name="response"></a>Resposta
###<a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
200 | OK
401 | Não Autorizado
403 | Proibido
408 | Tempo Limite do Pedido   
500 | Erro interno do servidor
503 | Serviço indisponível

###<a name="response-headers"></a>Cabeçalhos de Resposta
Para cabeçalhos de resposta comuns, consulte [pedido HTTP e cabeçalhos da resposta](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do PAM*.
###<a name="response-body"></a>Corpo da resposta
nenhum
##<a name="example"></a>Exemplo

###<a name="request"></a>Pedido
```
POST /api/pamresources/pamrequeststoapprove(guid'5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143')/Approve HTTP/1.1


```
###<a name="response"></a>Resposta
```
HTTP/1.1 200 OK

```       
