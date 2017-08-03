---
title: Fechar o pedido de PAM | Microsoft Docs
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
ms.assetid: ca3a1a68-9a2b-47da-bfc1-eaa360cbe609
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 181bf1a2953e461925d1b7efc5da4a2fa0c86914
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="close-pam-request"></a>Fechar o pedido de PAM
Utilizado por uma conta com privilégios para fechar um pedido que iniciou elevação a uma função de PAM.

**Tenha em atenção**: URLs apresentados neste tópico são relativamente ao nome do anfitrião selecionado durante a implementação de API; por exemplo: `http://api.contoso.com`.
##<a name="request"></a>Pedido


Método  |URL do pedido  
---------|---------
POST     |/API/pamresources/pamrequests({requestId)/Close

###<a name="url-parameters"></a>Parâmetros de URL
Parâmetro | Descrição
----------|-----------
requestId | O identificador (GUID) do pedido de PAM fechar especificado da seguinte forma:`guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'`

###<a name="query-parameters"></a>Parâmetros de consulta
Parâmetro | Descrição
----------|--------------
v | Opcional. A versão de API. Se não incluído, será utilizada a versão atual (mais recentemente lançada) da API. Para obter mais informações, consulte [os detalhes do serviço de API de REST do PAM de controlo de versões](privileged-access-management-rest-api-service-details.md#versioning)

###<a name="request-headers"></a>Cabeçalhos de pedido
Para cabeçalhos de pedido comuns, consulte [pedido HTTP e cabeçalhos da resposta](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do PAM*.
###<a name="request-body"></a>Corpo do pedido
nenhum

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
POST /api/pamresources/pamrequests(guid'5ec10e61-cdd1-404e-a18e-740467d87dbf')/Close HTTP/1.1


```
###<a name="response"></a>Resposta
```
HTTP/1.1 200 OK

```       
