---
title: Detalhes do serviço API de Gestão de Certificados REST / Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 530047f1-e43b-4a69-9542-75bc1da57bf7
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9a2dd84e121217772a8831653b2e4790436c32ec
ms.sourcegitcommit: c214bb0b1373b65b1c9c215379fd820ab0c13f0f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/14/2020
ms.locfileid: "92762532"
---
# <a name="certificate-management-rest-api-service-details"></a>Detalhes do serviço API rest gestão de certificados
As seguintes secções descrevem detalhes da API do Gestor de Identidade da Microsoft (MIM)

## <a name="architecture"></a>Arquitetura 
As chamadas API DE MIM CM REST são tratadas por controladores. A tabela a seguir mostra a lista completa dos controladores e amostras do contexto em que podem ser utilizados.


|                  Controlador                   |                                                                                                                                                           Rota da amostra                                                                                                                                                           |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           CertificadoDataController           |                                                                                                                                         /api/v1.0/requests/{requestid}/certificatedata/                                                                                                                                          |
| CertificadoRequestGenerationOptionsController |                                                                                                                                                  /api/v1.0/requests/{requestid}                                                                                                                                                  |
|            Certificadocontrolador             |                                                                                                                /api/v1.0/smartcards/{smartcardid}/certificados <br/> /api/v1.0/profiles/{profileid}/certificados                                                                                                                 |
|             Controlador de Operações              |                                                                                                                  /api/v1.0/smartcards/{smartcardid}/operações <br/> /api/v1.0/profiles/{profileid}/operações                                                                                                                   |
|              PolíticasControlador               |                                                                                                                                   /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}                                                                                                                                   |
|              PerfisControlador               |                                                                                                                                                     /api/v1.0/profiles/{id}                                                                                                                                                      |
|          ProfileTemplatesController           |                                                                                               /api/v1.0/profiletemplates/{id} <br/> /api/v1.0/profiletemplates <br/> /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}                                                                                                |
|              RequeriaControlador               |                                                                                                                                         /api/v1.0/requests/{id} <br/> /api/v1.0/pedidos                                                                                                                                         |
|             SmartcardsController              | /api/v1.0/requests/{requestid}/smartcards/{id}/diversifiedkey <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/serverproposedpin <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/authenticationresponse <br/> /api/v1.0/requests/{requestid}/smartcards/{id} <br/> /api/v1.0/smartcards/{id} <br/> /api/v1.0/smartcards |
|       SmartcardsConfigurationController       |                                                                                                                             /api/v1.0/profiletemplates/{profiletemplateid}/configuration/smartcards                                                                                                                              |

## <a name="http-request-and-response-headers"></a>Http pedido e cabeçalhos de resposta
Os pedidos HTTP enviados à API devem incluir os seguintes cabeçalhos (esta lista não é exaustiva):

Cabeçalho | Descrição
-------|------------
Autorização | Obrigatório. O conteúdo depende do método de autenticação. O método é configurável e pode basear-se em Serviços de Autenticação Integrada do Windows (WIA)) ou serviços da Federação de Diretórios Ativos (ADFS).
Content-Type | Obrigatório se o pedido tiver um corpo. Deve `application/json` ser.
Comprimento do conteúdo | Obrigatório se o pedido tiver um corpo. 
Cookie | O biscoito da sessão. Pode ser necessário dependendo do método de autenticação.


As respostas HTTP incluem os seguintes cabeçalhos (esta lista não é exaustiva):

Cabeçalho | Descrição
-------|------------
Content-Type | A API regressa `application/json` sempre.
Comprimento do conteúdo | O comprimento do corpo pedido, se presente, em bytes.


## <a name="api-versioning"></a>Versão API 
A versão atual da CM REST API é 1.0. A versão é especificada no segmento que segue diretamente o `/api` segmento no URI: `/api/v1.0` . O número da versão muda quando são feitas grandes modificações na interface API.


## <a name="enable-the-api"></a>Ativar a API 
A `<ClmConfiguration>` secção do ficheiro Web.config foi alargada com uma nova chave:

```
<add key="Clm.WebApi.Enabled" value="false" />
```

Esta chave determina se a CM REST API está exposta aos clientes. Se a chave estiver definida como "falsa", o mapeamento de rota para a API não é realizado no arranque de aplicações. Os pedidos subsequentes aos pontos finais da API devolvem um código de erro HTTP 404. A chave está definida para "desativado" por defeito.

>[!NOTE]
>Depois de mudar este valor para "verdadeiro", lembre-se de executar **iisreset** no servidor.

## <a name="enable-tracing-and-logging"></a>Ativar o rastreio e a exploração madeireira 
A API MIM CM REST emite vestígios de cada pedido HTTP que lhe é enviado. Pode definir o nível de verbosidade das informações de traços emitidas definindo o seguinte valor de configuração:

```
<add name="Microsoft.Clm.Web.API" value="0" />
```

## <a name="error-handling-and-troubleshooting"></a>Manipulação de erros e resolução de problemas 
Quando ocorrem exceções durante o processamento de um pedido, a API MIM CM REST devolve um código de estado HTTP ao cliente web. Para erros comuns, a API devolve um código de estado HTTP adequado e um código de erro. 

As exceções não tratadas são convertidas para um `HttpResponseException` código de estado HTTP 500 ("Erro Interno") e rastreados tanto no registo de eventos como no ficheiro de traços MIM CM. Cada exceção não manipulada é escrita no registo do evento com um ID de correlação correspondente. O ID de correlação também é enviado ao consumidor da API na mensagem de erro. Para resolver o erro, um administrador pode pesquisar o registo do evento para obter o iD de correlação correspondente e detalhes de erro.

>[!NOTE]
>Os vestígios de stack que correspondem a erros, que são gerados como resultado do consumo da API MIM CM REST, não são devolvidos ao cliente. Os vestígios não são enviados de volta para o cliente devido a questões de segurança.
