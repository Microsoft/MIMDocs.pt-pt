---
title: "Detalhes do serviço de API de REST do CM | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 530047f1-e43b-4a69-9542-75bc1da57bf7
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3d724dea8b1536f0155f9fc287d0cd74f5f16b3c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="cm-rest-api-service-details"></a>Detalhes do serviço de API de REST de CM
As secções seguintes abordam detalhes sobre a API de REST de gestão de certificados (CM) do Microsoft Identity Manager (MIM).

## <a name="architecture"></a>Arquitetura 
Chamadas da API de REST de MIM CM são processadas pela controladores. A tabela seguinte mostra a lista completa dos controladores e exemplos de contexto em que pode ser utilizados.

Controlador| Rota de exemplo
----------|-------------
CertificateDataController| /API/v1.0/Requests/{RequestId}/certificatedata /
CertificateRequestGenerationOptionsController| /API/v1.0/Requests/{RequestId}/certificaterequestgenerationoptions
CertificatesController| /API/v1.0/smartcards/{smartcardid}/Certificates <br/> /API/v1.0/Profiles/{profileid}/Certificates
OperationsController| /API/v1.0/smartcards/{smartcardid}/Operations <br/> /API/v1.0/Profiles/{profileid}/Operations
PoliciesController| / api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
ProfilesController| / api/v1.0/profiles/{id} <br/> /API/v1.0/Profiles <br/> / api/v1.0/requests/{requestid}/profiles/{id}
ProfileTemplatesController| / api/v1.0/profiletemplates/{id} <br/> /API/v1.0/profiletemplates <br/> / api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
RequestsController| / api/v1.0/requests/{id} <br/> /API/v1.0/Requests
SmartcardsController| /API/v1.0/Requests/{RequestId}/smartcards/{ID}/diversifiedkey <br/> /API/v1.0/Requests/{RequestId}/smartcards/{ID}/serverproposedpin <br/> /API/v1.0/Requests/{RequestId}/smartcards/{ID}/authenticationresponse <br/> / api/v1.0/requests/{requestid}/smartcards/{id} <br/> / api/v1.0/smartcards/{id} <br/> /API/v1.0/smartcards
SmartcardsConfigurationController| /API/v1.0/profiletemplates/{profiletemplateid}/Configuration/smartcards
<br/>

## <a name="http-request-and-response-headers"></a>Pedido de HTTP e cabeçalhos de resposta

Pedidos HTTP enviados para a API devem incluir os seguintes cabeçalhos (esta lista não é exaustiva):

Cabeçalho | Descrição
-------|------------
Autorização | Necessário. O conteúdo dependerá do método de autenticação, que é configurável e pode ser baseado em WIA (autenticação integrada do Windows) ou AD FS.
Tipo de conteúdo | Necessário se o pedido tem um corpo. Tem de ser "application/json".
Comprimento do conteúdo | Necessário se o pedido tem um corpo. 
Cookie | O cookie de sessão. Poderá ser necessário dependendo do método de autenticação.
<br/>
As respostas de HTTP irão incluir os seguintes cabeçalhos (esta lista não é exaustiva):

Cabeçalho | Descrição
-------|------------
Tipo de conteúdo | A API devolve sempre "application/json".
Comprimento do conteúdo | O comprimento do corpo do pedido, se estiver presente, em bytes.


## <a name="api-versioning"></a>Controlo de versões de API 
A versão atual da API de REST do CM é 1.0. A versão especificada no segmento diretamente seguir o `/api` segmento no URI: `/api/v1.0`. No futuro, irá alterar o número de versão devem existir alterações principais para a interface de API.


## <a name="enabling-the-api"></a>Ativar a API 
O `<ClmConfiguration>` secção do ficheiro Web. config tiver sido expandida com uma nova chave:

```
<add key="Clm.WebApi.Enabled" value="false" />
```
<br/>

Esta chave determina se a API de REST do CM fica exposta aos clientes. Se a chave está definida como "false", o mapeamento de rota para a API não é efetuado no arranque de aplicação. Isto significa que quaisquer pedidos subsequentes para pontos finais da API irão devolver um código de erro HTTP 404. Por predefinição, a chave está definida para "desactivado".
Depois de alterar este valor como "true", não se esqueça de executar **iisreset** no servidor.

## <a name="enabling-tracing-and-logging"></a>Ativar o rastreio e de registo 
A API de REST de MIM CM emite dados de rastreio de cada pedido HTTP enviado ao mesmo. Pode definir o nível de verbosidade as informações de rastreio emitidos definindo o valor de configuração seguinte:

```
<add name="Microsoft.Clm.Web.API" value="0" />
```
<br/>

## <a name="error-handling-and-troubleshooting"></a>Processamento de erros e resolução de problemas 
Quando ocorrem exceções ao processar um pedido, a API de REST de MIM CM devolve um código de estado HTTP ao cliente web. Para erros comuns, a API devolve um código de estado HTTP adequado e um código de erro. 

Exceções não processadas são convertidas num `HttpResponseException` com o estado HTTP de código 500 ("Error interno") e rastreado no registo de eventos e o ficheiro de rastreio de MIM CM. Cada exceção não processada é escrita no registo de eventos com um ID de correlação correspondente. O ID de correlação é também enviado para o consumidor da API na mensagem de erro. Para resolver o erro, um administrador pode procurar o registo de eventos para os detalhes de erro e ID de correlação correspondentes.

Devido a problemas de segurança, rastreios de pilha correspondente erros gerados como resultado de consumo da API de REST de MIM CM não são enviados para o cliente.
