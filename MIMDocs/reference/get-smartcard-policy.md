---
title: Obtenha a política de cartões inteligentes . Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c015ffc7-5c94-427e-a3b3-870ec8ab92b6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e8e8b2ac7dcf8f72d4989d458272d78b8367d923
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762216"
---
# <a name="get-smart-card-policy"></a>Obtenha a política de cartões inteligentes
Obtém a política do modelo de perfil para o fluxo de trabalho especificado. Estes dados são utilizados durante a criação do pedido. A política de fluxo de trabalho especifica quais os dados necessários pelo cliente para criar um pedido. Os dados podem incluir vários itens de recolha de dados, comentários de pedido e uma política de senha única.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates/{id}/policy/workflow/{type}

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro| Descrição
--------|-------------
ID| Obrigatório. O GUID corresponde ao modelo de perfil de onde a política deve ser extraída.
tipo| Obrigatório. O tipo de política que está a ser solicitada. Os valores possíveis são "Inscrever", "Duplicar", "OfflineUnblock", "OnlineUpdate", "Renovar", "Recuperar", "Recuperar Metade", "Reintegrar", "Reformar", "Revogar", "Invocar", "TemporariEnroll" e "Desbloquear".

### <a name="request-headers"></a>Cabeçalhos do pedido
Para cabeçalhos de pedido comuns, consulte [os cabeçalhos de pedido e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *dados do serviço cm REST API* .

### <a name="request-body"></a>Corpo do pedido
Nenhum.

## <a name="response"></a>Resposta
Esta secção descreve a resposta.

### <a name="response-codes"></a>Códigos de resposta

Código  |Descrição  
---------|---------
200 | OK
403 | Proibido
204 | Sem conteúdo
500 | Erro Interno

### <a name="response-headers"></a>Cabeçalhos de resposta
Para cabeçalhos de resposta comuns, consulte [os cabeçalhos de pedido e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *dados do serviço cm REST API* .

### <a name="response-body"></a>Corpo da resposta
No sucesso, devolve um objeto de política baseado num objeto [ProfileTemplatePolicy.](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx) No mínimo, o objeto da apólice contém as propriedades na tabela seguinte, mas pode conter propriedades adicionais dependendo da política solicitada. Por exemplo, um pedido de inscrição de uma apólice devolve um objeto [Desacência Política.](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy) Para obter mais informações, consulte a documentação do objeto de política associado ao parâmetro {type} no pedido. A documentação para os diferentes tipos de objetos de política pode ser encontrada na documentação do [espaço de nomes Microsoft.Clm.Shared.ProfileTemplates.](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates)

Propriedade | Descrição
---------|------------
AprovaçõesNesed | O número de aprovações necessárias para pedidos de Gestão de Certificados (FIM) do Gestor de Identidade (FIM) da Gestão de Certificados (CM).
Aprovadorover Autorizado | O descritor de segurança para utilizadores autorizados a aprovar pedidos fim CM para a apólice.
AutorizadoEnrollmentAgent | O descritor de segurança para utilizadores que podem atuar como agentes de inscrição para a apólice.
Iniciador Autorizado | O descritor de segurança para utilizadores que possam iniciar pedidos fim CM para a apólice.
Coleccionamentos | Um valor Boolean que indica se a recolha de comentários está ativada para pedidos fim CM para a apólice.
ColeccionarRequestPriority | Um valor Boolean que indica se a recolha prioritária de pedidos está ativada para os pedidos fim CM para a apólice.
PredefiniçãoRequestPrioridade | A prioridade padrão para os pedidos fim CM para a apólice.
Documentos | Os documentos políticos que estão configurados para a política.
Ativado | Um valor Boolean que indica se a apólice está ativada.
Inscreva-se AgentRequired | Um valor Boolean que indica se os agentes de inscrição são necessários para os pedidos fim CM para a apólice.
OneTimePasswordPolicy | O método de distribuição de senhas únicas para pedidos fim CM para a apólice.
Personalização | As opções de personalização de cartões inteligentes para a apólice.
PolicyDataCollection | Os itens de recolha de dados associados à apólice.
SelfServiceEnabled | Um valor Boolean que indica se o início de autosserviço dos pedidos FIM CM está ativado para a apólice.

## <a name="example"></a>Exemplo
Esta secção fornece um exemplo para obter a política do modelo de perfil para um fluxo de trabalho. 

### <a name="example-request-1"></a>Exemplo: Pedido 1

```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```

### <a name="example-response-1"></a>Exemplo: Resposta 1

```
HTTP/1.1 200 OK

... body coming soon
```       

### <a name="example-request-2"></a>Exemplo: Pedido 2

```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```

### <a name="example-response-2"></a>Exemplo: Resposta 2

```
HTTP/1.1 200 OK

... body coming soon
```       

## <a name="see-also"></a>Ver também

- [Microsoft.Clm.Shared.ProfileTemplates.ProfileTemplatePolicy class](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)
- [Microsoft.Clm.Shared.ProfileTemplates namespace](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)
