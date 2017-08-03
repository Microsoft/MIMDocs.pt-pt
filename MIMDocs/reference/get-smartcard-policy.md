---
title: "Obter uma política de smart card | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c015ffc7-5c94-427e-a3b3-870ec8ab92b6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1164795e81ff0ef2f93086144b721e760f8dd028
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-policy"></a>Obter uma política de smart card

Obtém a política de modelo de perfil do fluxo de trabalho especificado. Estes dados são utilizados durante a criação do pedido. A política de fluxo de trabalho Especifica que dados é necessária pelo cliente para criar um pedido. Esses dados podem incluir: vários itens de recolha de dados, comentários de pedido e uma política de palavra-passe de tempo.

**Tenha em atenção**: URLs apresentados neste tópico são relativamente ao nome do anfitrião selecionado durante a implementação de API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Pedido


Método  |URL do pedido  
---------|---------
GET     |/ CertificateManagement/api/v1.0/profiletemplates/{id}/policy/workflow/{type}

###<a name="url-parameters"></a>Parâmetros de URL
Parâmetro| Descrição
--------|-------------
ID| Necessário. O GUID correspondente para o modelo de perfil que a política está a ser extraído da.
tipo| Necessário. O tipo de política que está a ser solicitada. Os valores possíveis são: *inscrever*, *duplicado*, *OfflineUnblock*, *OnlineUpdate*, *renovar*, *recuperar*, *RecoverOnBehalf*, *restabelecimento*, *extinguir*, *revogar*, *TemporaryEnroll*, *desbloqueio*.

###<a name="request-headers"></a>Cabeçalhos de pedido
Para cabeçalhos de pedido comuns, consulte [pedido HTTP e cabeçalhos da resposta](certificate-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do CM*.
###<a name="request-body"></a>Corpo do pedido
nenhum

##<a name="response"></a>Resposta
###<a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
200     | OK
403 | Proibido
204 | Nenhum conteúdo
500 | Erro interno

###<a name="response-headers"></a>Cabeçalhos de Resposta
Para cabeçalhos de resposta comuns, consulte [pedido HTTP e cabeçalhos da resposta](certificate-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do CM*.
###<a name="response-body"></a>Corpo da resposta
Com êxito, devolve um objeto de política com base num [ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx) objeto. No mínimo, o objeto de política irá conter as propriedades na tabela seguinte, mas pode conter propriedades adicionais, consoante a política de pedido. Por exemplo, um pedido para uma política de inscrição irá devolver um [EnrollPolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy) objeto. Para obter mais informações, consulte a documentação para o objeto de política associada com o parâmetro de {type} no pedido. A documentação para os diferentes tipos de objetos de política pode ser encontrada no [Microsoft.Clm.Shared.ProfileTemplates espaço de nomes](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates) documentação.

Propriedade | Descrição
---------|------------
ApprovalsNeeded | O número de aprovações que são necessárias para pedidos de FIM CM para a política.
AuthorizedApprover | O descritor de segurança para os utilizadores que têm autorização para aprovar pedidos de FIM CM para a política.
AuthorizedEnrollmentAgent | O descritor de segurança para os utilizadores que pode atuar como agentes de inscrição para a política.
AuthorizedInitiator | O descritor de segurança para os utilizadores que podem iniciar pedidos de FIM CM para a política.
CollectComments | Um valor booleano que indica se a coleção de comentário está ativada para pedidos de FIM CM para a política.
CollectRequestPriority | Um valor booleano que indica se a recolha da prioridade do pedido está ativada para pedidos de FIM CM para a política.
DefaultRequestPriority | A prioridade de predefinido para pedidos de FIM CM para a política.
Documentos | Os documentos de política que estão configurados para a política.
Ativado | Um valor booleano que indica se a política for ativada.
EnrollAgentRequired | Um valor booleano que indica se os agentes de inscrição são necessários para pedidos de FIM CM para a política.
OneTimePasswordPolicy | Obtém as palavras-passe Monouso como para pedidos de FIM CM para a política são distribuídas.
Personalização | As opções de personalização de smart card para a política.
PolicyDataCollection | Os itens de recolha de dados que estão associados à política.
SelfServiceEnabled | Um valor booleano que indica se o início de self-service de mensagens em fila de pedidos de FIM CM está ativado para a política.

##<a name="example"></a>Exemplo

###<a name="request-1"></a>Pedido 1
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```
###<a name="response-1"></a>Resposta 1
```
HTTP/1.1 200 OK

... body coming soon
```       
###<a name="request-2"></a>Pedido de 2
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```
###<a name="response-2"></a>Resposta 2
```
HTTP/1.1 200 OK

... body coming soon
```       
##<a name="see-also"></a>Consulte Também

- [Classe de Microsoft.Clm.Shared.ProfileTemplates.ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)
- [Espaço de nomes de Microsoft.Clm.Shared.ProfileTemplates](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)
