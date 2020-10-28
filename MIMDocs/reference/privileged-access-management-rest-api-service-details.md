---
title: Detalhes do serviço da PAM REST API Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 54c78bbd-8da1-42ff-9edc-47d913011941
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 00a2f4d9c44747d50139655d368e42b11fbd388c
ms.sourcegitcommit: c214bb0b1373b65b1c9c215379fd820ab0c13f0f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/14/2020
ms.locfileid: "92762522"
---
# <a name="pam-rest-api-service-details"></a>Detalhes do serviço API PAM REST
As secções seguintes discutem detalhes da API de Gestão de Acesso Privilegiado (PAM) do Gestor de Identidade da Microsoft (MIM).

<h2 id="http-request-and-response-headers">HTTP solicitam cabeçalhos</h2>

Os pedidos HTTP que são enviados para a API devem incluir os seguintes cabeçalhos (esta lista não é exaustiva):

Cabeçalho | Descrição
-------|------------
Autorização | Obrigatório. O conteúdo depende do método de autenticação, que é configurável e pode ser baseado na WIA (Autenticação Integrada do Windows) ou ADFS.
Content-Type | Obrigatório se o pedido tiver um corpo. Deve ser definido para `application/json` .
Comprimento do conteúdo | Obrigatório se o pedido tiver um corpo. 
Cookie | O biscoito da sessão. Pode ser necessário dependendo do método de autenticação.

## <a name="http-response-headers"></a>Cabeçalhos de resposta HTTP

As respostas HTTP devem incluir os seguintes cabeçalhos (esta lista não é exaustiva):

Cabeçalho | Descrição
-------|------------
Content-Type | A API regressa `application/json` sempre.
Comprimento do conteúdo | O comprimento do corpo pedido, se presente, em bytes.

## <a name="versioning"></a>Controlo de versões 
A versão atual da API é 1. A versão API pode ser especificada através de um parâmetro de consulta no URL de pedido, como no exemplo seguinte: `http://localhost:8086/api/pamresources/pamrequests?v=1` Se a versão não for especificada no pedido, o pedido é executado contra a versão mais recente da API. 

## <a name="security"></a>Segurança 
O acesso à API requer autenticação integrada do Windows (IWA). Isto deve ser configurado manualmente no IIS antes da instalação do Microsoft Identity Manager (MIM).

HTTPS (TLS) é suportado, mas deve ser configurado manualmente no IIS. Para obter informações, consulte: **Implementar camada de tomadas seguras (SSL) para o Portal FIM** no passo [9: Executar as tarefas de pós-instalação FIM 2010 R2](https://technet.microsoft.com/library/hh322875.aspx) no Guia de Laboratório de Ensaios DE Instalação FIM 2010 R2. 

Pode gerar um novo certificado de servidor SSL executando o seguinte comando no Pedido de Comando do Estúdio Visual:

```
Makecert -r -pe -n CN="test.cwap.com" -b 05/10/2014 -e 12/22/2048 -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
 
O comando cria um certificado auto-assinado que pode ser usado para testar uma aplicação web que utiliza SSL num servidor web onde o URL está `test.cwap.com` . O OID definido pela `-eku` opção identifica o certificado como um certificado de servidor SSL. O certificado está guardado na **minha** loja e está disponível ao nível da máquina. Pode exportar o certificado a partir do encaixe de certificados em mmc.exe.

## <a name="cross-domain-access-cors"></a>Acesso ao domínio transversal (CORS) 
O CORS é suportado, mas deve ser configurado manualmente no IIS. Adicione os seguintes elementos ao ficheiro API web.config implantado para configurar a API para permitir chamadas de domínio cruzado: 

```
<system.webServer>       
    <httpProtocol> 
        <customHeaders> 
            <add name="Access-Control-Allow-Credentials" value="true"  /> 
            <add name="Access-Control-Allow-Headers" value="content-type" /> 
            <add name="Access-Control-Allow-Origin" value="http://<hostname>:8090" /> 
        </customHeaders> 
    </httpProtocol> 
</system.webServer> 
```

## <a name="error-handling"></a>Processamento de erros 
A API devolve respostas de erro HTTP para indicar as condições de erro. Os erros são compatíveis com o OData. A tabela a seguir mostra os códigos de erro que podem ser devolvidos a um cliente:

Código de estado de HTTP | Descrição
-----------------|------------
401 | Não autorizado 
403 | Proibido 
408 | Tempo Limite do Pedido   
500 | Erro interno do servidor 
503 | Serviço Indisponível 

## <a name="filtering"></a>Filtragem 
Os pedidos da API PAM REST podem incluir filtros para especificar as propriedades que devem ser incluídas na resposta. A sintaxe do filtro baseia-se em expressões OData.

Os filtros podem especificar qualquer uma das propriedades dos Pedidos PAM, Funções PAM. ou pedidos de PAM pendentes. Por exemplo: *ExpirationTime* , *DisplayName,* ou qualquer outro imóvel válido de um Pedido PAM, Função PAM ou Pedido Pendente.

A API suporta os seguintes operadores em expressões de filtro: *E,* *Equal,* *NotEqual,* *GreaterThan,* *LessThan,* *GreaterThenOrEqueal* , e *LessThanOrEqual* . 

Os seguintes pedidos de amostra incluem filtros:

- Este pedido devolve todos os Pedidos DE PAM entre datas específicas: `http://localhost:8086/api/pamresources/pamrequests?$filter=ExpirationTime gt datetime'2015-01-09T08:26:49.721Z' and ExpirationTime lt datetime'2015-02-10T08:26:49.722Z' `
 
- Este pedido devolve a Função Pam com o nome de exibição "SQL File Access": `http://localhost:8086/api/pamresources/pamroles?$filter=DisplayName eq 'SQL File Access' `
