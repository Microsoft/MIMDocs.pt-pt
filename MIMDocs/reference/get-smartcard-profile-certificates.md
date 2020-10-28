---
title: Obtenha certificados de cartão inteligente ou perfis ! Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: 206bde70-4af8-46fa-9c0b-f574745b0977
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6e089caa78dd9a52517f2cae3fa42176225c73b5
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762027"
---
# <a name="get-smart-card-or-profile-certificates"></a>Obtenha certificados de cartão inteligente ou de perfil
Obtém uma lista de certificados associados a um determinado perfil de cartão inteligente ou software.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
GET     |/CertificadoManagement/api/v1.0/profiles/{id}/certificates <br/>/CertificadoManagement/api/v1.0/smartcards/{id}/certificados

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro | Descrição
---------|------------
ID | O identificador (GUID) do perfil ou cartão inteligente.

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
Para cabeçalhos de resposta comuns, consulte [os cabeçalhos de pedido e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *dados do serviço cm REST API* .

### <a name="response-body"></a>Corpo da resposta
No sucesso, devolve uma lista de [objetos microsoft.clm.shared.certificates.X509ClmCertificate](https://msdn.microsoft.com/library/microsoft.clm.shared.certificates.x509clmcertificate.aspx) com as seguintes propriedades:

Nome | Descrição
-----|------------
ArchivedOnCa | Um valor Boolean que indica se o certificado é arquivado na autoridade de certificação (CA).
CertificateType | O tipo de certificado.
IsKeyHistory | Um valor Boolean que indica se o certificado é um certificado de histórico chave.
SerialNumber | O número de série do certificado.
Nome modeloCommonName | O nome comum do modelo de certificado para o certificado.
Thumbprint | A impressão digital do certificado.

## <a name="example"></a>Exemplo
Esta secção fornece um exemplo para obter os certificados que estão associados a um perfil de cartão inteligente ou software.

### <a name="example-request"></a>Exemplo: Pedido

```
GET /certificatemanagement/api/v1.0/smartcards/5badfea3-de31-4837-99f9-8249515a5473/certificates HTTP/1.1
```

### <a name="example-response"></a>Exemplo: Resposta

```
HTTP/1.1 200 OK

[
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B01052AFA01313FB77AC00010000B010",
        "Thumbprint":"C52B0C5FB8AAD31A5B239FF2712ED14122D67D30"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B011AB48AE7D664ED5D900010000B011",
        "Thumbprint":"E7C4324896271BE869544FF28AA2B1BF3B3BDFCF"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B01ABCF2D38A0CCEAD8F00010000B01A",
        "Thumbprint":"D9293B5414C644888444541B64631E90F2612425"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B1F02865CF2C3A9DD96D00010000B1F0",
        "Thumbprint":"6615DDC8603DBA789D724502627682F37D0FC2D0"
    }
]
```       

## <a name="see-also"></a>Ver também

- [Microsoft.Clm.Provision.FindOperations.FindCertificates method](https://msdn.microsoft.com/library/microsoft.clm.provision.findoperations.findcertificates.aspx)
- [Microsoft.Clm.Shared.Certificates.X509 ClasseClmCertificate](https://msdn.microsoft.com/library/microsoft.clm.shared.certificates.x509clmcertificate.aspx)
