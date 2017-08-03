---
title: "Detalhes do serviço de API de REST de PAM | Microsoft Docs"
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
ms.assetid: 54c78bbd-8da1-42ff-9edc-47d913011941
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6e248ba4a65e75b1e914c61de70072c7e094123a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="pam-rest-api-service-details"></a>Detalhes do serviço de API de REST de PAM
As secções seguintes abordam detalhes sobre a API de REST de Privileged Access Management (PAM) do Microsoft Identity Manager (MIM).

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

## <a name="versioning"></a>Controlo de versões 
A versão atual da API é 1. A versão da API pode ser especificada através de um parâmetro de consulta no URL do pedido, como no seguinte exemplo: `http://localhost:8086/api/pamresources/pamrequests?v=1` se a versão não é especificada no pedido, o pedido é executado em relação a versão de lançamento mais recentemente da API. 

## <a name="security"></a>Segurança 
O acesso à API requer autenticação integrada de Windows (IWA). Isto deve ser configurado manualmente no IIS antes da instalação do Microsoft Identity Manager (MIM).

HTTPS (TLS) é suportada, mas devem ser configuradas manualmente no IIS. Para informações, consulte: **implementar SSL Secure Sockets Layer () para o Portal do FIM** no [passo 9: efetuar FIM 2010 R2 as tarefas de pós-instalação] (https://technet.microsoft.com/library/hh322875 (v=ws.10%29.aspx) no instalar FIM 2010 R2 teste guia de laboratório. 

Pode gerar um novo certificado de servidor SSL, executando o seguinte comando no Visual Studio linha de comandos:
```
Makecert -r -pe -n CN="test.cwap.com" -b 05/10/2014 -e 12/22/2048 -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
 
Este comando cria um certificado autoassinado que pode ser utilizado para testar uma aplicação web que utiliza o protocolo SSL num servidor web cujo URL é test.cwap.com. O OID definido pela opção - eku identifica o certificado como um certificado de servidor SSL. O certificado é armazenado no meu arquivo e está disponível ao nível da máquina, pelo que pode exportá-lo a partir do snap-in de certificados no mmc.exe

## <a name="cross-domain-access-cors"></a>Cruzada (CORS) de acesso de domínio 
CORS é suportada, mas devem ser configuradas manualmente no IIS. Adicione os seguintes elementos para o ficheiro Web. config de API implementado para configurar a API para permitir das chamadas do domínio cruzado: 

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
<br/>

## <a name="error-handling"></a>Processamento de Erros 
A API devolve as respostas de erro HTTP para indicar as condições de erro. Erros são OData em conformidade. A tabela seguinte mostra os códigos de erro que podem ser devolvidos a um cliente.

Código de estado de HTTP | Descrição
-----------------|------------
401 | Não Autorizado 
403 | Proibido 
408 | Tempo Limite do Pedido   
500 | Erro interno do servidor 
503 | Serviço indisponível 
<br/>

## <a name="filtering"></a>Filtragem 
Pedidos de API de REST de PAM podem incluir os filtros para especificar as propriedades que devem ser incluídas na resposta. Sintaxe de filtro baseia-se em expressões de OData.

Os filtros podem especificar qualquer uma das propriedades de pedidos de PAM, funções de PAM. ou pedidos de PAM pendentes. Por exemplo: *ExpirationTime*, *DisplayName*, ou qualquer outra propriedade válida de um pedido de PAM, a função de PAM ou pedido pendente.

A API suporta os seguintes operadores em expressões de filtro: *e*, *igual*, *NotEqual*, *GreaterThan*, *LessThan*, *GreaterThenOrEqueal*, e *LessThanOrEqual*. 

Os seguintes pedidos de exemplo incluem filtros:

- Este pedido devolve todos os pedidos de PAM entre as datas específicas:`http://localhost:8086/api/pamresources/pamrequests?$filter=ExpirationTime gt datetime"2015-01-09T08:26:49.721Z" and ExpirationTime lt datetime"2015-02-10T08:26:49.722Z" `
 
- Este pedido devolve a função de Pam com o nome a apresentar "Acesso a ficheiros SQL":`http://localhost:8086/api/pamresources/pamroles?$filter=DisplayName eq "SQL File Access" `
