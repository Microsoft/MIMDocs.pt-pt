---
title: "Obter opções de geração de pedido de certificado | Microsoft Docs"
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
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5822da1b9cf8558becccff815fd0208923fcaaa0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="get-certificate-request-generation-options"></a>Obter opções de geração de pedido de certificado

Obtém os parâmetros para a geração de pedido de certificado do lado do cliente.

**Tenha em atenção**: URLs apresentados neste tópico são relativamente ao nome do anfitrião selecionado durante a implementação de API; por exemplo: `https://api.contoso.com`.
##<a name="request"></a>Pedido


Método  |URL do pedido  
---------|---------
GET     |/CertificateManagement/API/v1.0/Requests/{RequestId}/certificaterequestgenerationoptions

###<a name="url-parameters"></a>Parâmetros de URL
Parâmetro | Descrição
--------|--------------
RequestId| Necessário. O identificador GUID do pedido de MIM CM para que os parâmetros de geração de pedido de certificado devem ser obtidos.

###<a name="request-headers"></a>Cabeçalhos de pedido
Para cabeçalhos de pedido comuns, consulte [pedido HTTP e cabeçalhos da resposta](certificate-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do CM*.
###<a name="request-body"></a>Corpo do pedido
nenhum.


##<a name="response"></a>Resposta
###<a name="response-codes"></a>Códigos de resposta
Código  |Descrição  
---------|---------
200 | OK
204 | Nenhum conteúdo
403 | Proibido
500 | Erro interno

###<a name="response-headers"></a>Cabeçalhos de Resposta
Para cabeçalhos de resposta comuns, consulte [pedido HTTP e cabeçalhos da resposta](certificate-management-rest-api-service-details.md#http-request-and-response-headers) no *os detalhes do serviço de API de REST do CM*.
###<a name="response-body"></a>Corpo da resposta
Com êxito, devolve a lista de objetos de CertificateRequestGenerationOptions. Cada objeto CertificateRequestGenerationOptions corresponde a um pedido de certificado único que o cliente tem de gerar e tem as seguintes propriedades:

Propriedade| Descrição
--------|-----------
Exportável | Um valor que especifica se a chave privada criada para o pedido pode ser exportada.
FriendlyName | O nome a apresentar do certificado inscrito.
HashAlgorithmName | O algoritmo hash utilizado ao criar a assinatura do pedido de certificado.
KeyAlgorithmName | O algoritmo de chave pública.
KeyProtectionLevel | O nível de proteção forte por chave.
KeySize | O tamanho, em bits, a chave privada para ser gerado.
KeyStorageProviderNames | Uma lista de fornecedores de armazenamento de chaves aceitável (KSPs) que podem ser utilizadas para gerar a chave privada. No caso de que o primeiro KSP não pode ser utilizado para gerar o pedido de certificado, qualquer um dos KSPs especificados pode ser utilizado até uma ter êxito.
KeyUsages | A operação que pode ser efetuada pela chave privada criada para este pedido de certificado. O valor predefinido é a assinatura.
Assunto | O nome do requerente.

**Tenha em atenção**: estão disponíveis em mais informações sobre estas propriedades a [Windows.Security.Cryptography.Certificates.CertificateRequestProperties classe](https://msdn.microsoft.com/library/windows/apps/br212079.aspx) descrição, mas tenha em atenção de que não há uma correspondência de e-mail de um para um entre esta classe e objetos CertificateRequestGenerationOptions.

##<a name="example"></a>Exemplo

###<a name="request"></a>Pedido
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1

```
###<a name="response"></a>Resposta
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
