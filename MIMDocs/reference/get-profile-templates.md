---
title: Obter modelos de perfil | Microsoft Docs
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
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8736b328926445f6434bd037a1ecf2e5de9ee466
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-templates"></a>Obter modelos de perfil
Obtém uma lista de modelos de perfil que o utilizador especificado pode inscrever para. Este método devolve uma vista limitada do modelo de perfil. Os dados do modelo de perfil devolvidos devem ser suficientes para permitir que o utilizador requerente decidir o modelo de perfil, se aplicável, que precisam para fazer a inscrição. Se nenhum fluxo de trabalho e a permissão forem especificados, serão devolvidos todos os modelos de perfil visíveis ao utilizador.

**Tenha em atenção**: URLs apresentados neste tópico são relativamente ao nome do anfitrião selecionado durante a implementação de API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Pedido


Método  |URL do pedido  
---------|---------
GET     |/ CertificateManagement/api/v1.0/profiletemplates? \[targetuser\] 

###<a name="url-parameters"></a>Parâmetros de URL
Parâmetro| Descrição
--------|-------------
targetuser| Opcional. Especifica o utilizador de destino para devolver os modelos de perfil para. Se não for especificado, será utilizada a identidade do utilizador atual. <br/>**Tenha em atenção**: atualmente, apenas o utilizador atual é suportado.

###<a name="request-headers"></a>Cabeçalhos de pedido
Consulte o tópico de serviço para os cabeçalhos de pedido comuns
###<a name="request-body"></a>Corpo do pedido
nenhum

##<a name="response"></a>Resposta
###<a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
200     | OK
204 | Nenhum conteúdo
500 | Erro interno

###<a name="response-headers"></a>Cabeçalhos de Resposta
Consulte o tópico de serviço para cabeçalhos de resposta comuns.
###<a name="response-body"></a>Corpo da resposta
Com êxito, devolve uma lista de objetos de ProfileTemplateLimitedView com as seguintes propriedades.

Propriedade| Tipo| Descrição
--------|-----|--------
Nome| cadeia| O nome a apresentar do modelo de perfil.
Descrição| cadeia| A descrição para o modelo de perfil.
UUID| GUID| O identificador para o modelo de perfil.
IsSmartcardProfileTemplate| bool| Indica se o modelo é um modelo de perfil de smart card.
IsVirtualSmartcardProfileTemplate| bool| Indica se o modelo de perfil requer um smart card virtual.

##<a name="example"></a>Exemplo

###<a name="request"></a>Pedido
```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```
###<a name="response"></a>Resposta
```
HTTP/1.1 200 OK

[
    {
        "Name":"FIM CM Sample Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"12bd5120-86a2-4ee1-8d05-131066871578",
        "IsSmartcardProfileTemplate":false,
        "IsVirtualSmartcardProfileTemplate":false
    },
    {
        "Name":"FIM CM Sample Smart Card Logon Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"2b7044cf-aa96-4911-b886-177947e9271b",
        "IsSmartcardProfileTemplate":true,
        "IsVirtualSmartcardProfileTemplate":false
    }
]

```       
