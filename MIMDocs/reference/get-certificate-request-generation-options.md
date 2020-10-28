---
title: Obtenha opções de geração de pedido de certificados Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d8baab788c07dfb8c009857b45e38eb662f98199
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762106"
---
# <a name="get-certificate-request-generation-options"></a>Obtenha opções de geração de pedido de certificado
Obtém parâmetros para a geração de pedido de certificado do lado do cliente.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

| Método |                                       URL do Pedido                                        |
|--------|------------------------------------------------------------------------------------------|
|  GET   | /CertificadoManagement/api/v1.0/requests/{requestid}/certificaterequestgenerationops |

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro | Descrição
--------|--------------
requestid| Obrigatório. O identificador GUID do pedido MIM CM para o qual devem ser recuperados os parâmetros de geração do certificado.

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
204 | Sem conteúdo
403 | Proibido
500 | Erro Interno

### <a name="response-headers"></a>Cabeçalhos de resposta
Para cabeçalhos de pedido comuns, consulte [os cabeçalhos de pedido e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *dados do serviço cm REST API* .

### <a name="response-body"></a>Corpo da resposta
No sucesso, devolve uma lista de objetos DeCrequestGenerationOptions. Cada objeto DetetasGenerations corresponde a um único pedido de certificado que o cliente tem de gerar. Cada objeto tem as seguintes propriedades:

Propriedade| Descrição
--------|-----------
Exportável | Um valor que especifica se a chave privada criada para o pedido pode ser exportada.
FriendlyName | O nome de exibição do certificado inscrito.
Nome hashAlgorithm | O algoritmo de haxixe utilizado na criação da assinatura do pedido de certificado.
Nome keyAlgorithm | O algoritmo chave público.
KeyProtectionLevel | O nível de forte proteção das chaves.
Tamanho chave | O tamanho, em pedaços, da chave privada a ser gerada.
KeystorageProviderNames | Uma lista de fornecedores de armazenamento de chaves aceitáveis (KSPs) que podem ser usados para gerar a chave privada. Quando o primeiro KSP não pode ser usado para gerar o pedido de certificado, qualquer um dos KSPs especificados pode ser usado até que um tenha sucesso.
KeyUsages | A operação que pode ser realizada pela chave privada criada para este pedido de certificado. O valor predefinido é assinar.
Assunto | O nome do sujeito.

>[!NOTE]
>Mais informações sobre estas propriedades estão disponíveis na descrição da [classe Windows.Security.Cryptography.CertificateRequestProperties.](https://msdn.microsoft.com/library/windows/apps/br212079.aspx) Tenha em mente que não há uma correspondência um-para-um entre esta classe e objetos CertificadoRequestGenerationOptions.
>

## <a name="example"></a>Exemplo
Esta secção fornece um exemplo para obter as opções de geração para um pedido de certificado.

### <a name="example-request"></a>Exemplo: Pedido

```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1
```

### <a name="example-response"></a>Exemplo: Resposta

```
HTTP/1.1 200 OK

[
    {
        "Subject":"",
        "KeyAlgorithmName":"RSA",
        "KeySize":2048,
        "FriendlyName":"",
        "HashAlgorithmName":"SHA1",
        "KeyStorageProviderNames":[
            "Contoso Smart Card Key Storage Provider"
        ],
        "Exportable":0,
        "KeyProtectionLevel":0,
        "KeyUsages":3
    }
]
```  
