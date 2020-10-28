---
title: Obtenha respostas de certificados Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: b0559e7b-eaff-499b-8bcf-c6cdf5a89545
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 75c92bd31cda28c5eaef7d16832c819e27737525
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762267"
---
# <a name="get-certificate-responses"></a>Obtenha respostas de certificado
Submete um conjunto de pedidos de certificado à Autoridade de Certificados (CA) para processamento.

>[!IMPORTANT]
>O número de respostas de certificado pode exceder o número de pedidos. Alguns modelos de perfil podem ser configurados para gerar teclas/pedidos no servidor ou ter certificados externos.

>[!NOTE]
>Os URLs deste artigo são relativos ao nome de anfitrião escolhido durante a implantação da API, tais como `https://api.contoso.com` .

## <a name="request"></a>Pedir

Método  |URL do Pedido  
---------|---------
POST     |/CertificadoManagement/api/v1.0/requests/{id}/certificatedata

### <a name="url-parameters"></a>Parâmetros de URL

Parâmetro | Descrição
--------|--------------
ID| Obrigatório. O GUID corresponde ao modelo de perfil de onde a política deve ser extraída.

### <a name="request-headers"></a>Cabeçalhos do pedido
Para cabeçalhos de pedido comuns, consulte [os cabeçalhos de pedido e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *dados do serviço cm REST API* .

### <a name="request-body"></a>Corpo do pedido
O organismo de pedido contém as seguintes propriedades:

Propriedade | Descrição
---------|-----------
pfxpassword | A palavra-passe utilizada para proteger quaisquer resultados da Troca de Informações Pessoais (PFX) que são devolvidos da chamada.
certificadoreques | A lista de pedidos de certificado que são gerados pelo cliente. Os pedidos devem ser gerados utilizando os parâmetros especificados no objeto Opções deGenerations De Certificado. Para obter mais informações, consulte [obter opções de gerações de pedido de certificado.](get-certificate-request-generation-options.md)
comentários | Uma cadeia contendo quaisquer comentários adicionais que sejam gravados no histórico da operação.


## <a name="response"></a>Resposta
Esta secção descreve a resposta.

### <a name="response-codes"></a>Códigos de resposta

Código  |Descrição  
---------|---------
200 | OK
403 | Proibido
500 | Erro Interno

### <a name="response-headers"></a>Cabeçalhos de resposta
Para cabeçalhos de resposta comuns, consulte [os cabeçalhos de pedido e resposta HTTP](certificate-management-rest-api-service-details.md#http-request-and-response-headers) nos *dados do serviço cm REST API* .

### <a name="response-body"></a>Corpo da resposta
No sucesso, devolve uma lista de [objetos Microsoft.Clm.Shared.Certificates.Certificate](https://msdn.microsoft.com/library/microsoft.clm.shared.certificates.certificate.aspx) com as seguintes propriedades:

Propriedade| Descrição
------|-------------
CertificateId | A identificação do certificado.
Erro | Qualquer erro que tenha sido encontrado ao processar o pedido de certificado.
Sinalizadores | As bandeiras que descrevem o certificado.
IsExternal | Um valor Boolean que indica se o certificado é um certificado externo.
isKeyHistory | Um valor Boolean que indica se o certificado é um certificado de histórico chave.
isPfx | Um valor Boolean que indica se a resposta do pedido de certificado está no formato PFX.
isPkcs7 | Um valor Boolean que indica se a resposta do pedido de certificado está no formato PKCS#7.
isServerGenerated | Um valor Boolean que indica se o certificado foi gerado por um servidor.
Pfx | A bolha de certificado PFX.
Pkcs7 | A bolha de certificado PKCS#7.
Nome modeloCommonName | O nome comum do modelo de certificado para o certificado.
UseLocalMachineStore | Um valor Boolean que indica se o certificado é para a loja de certificados de computador local.

## <a name="example"></a>Exemplo
Esta secção dá o exemplo de apresentar um conjunto de pedidos de certificado à AC para processamento.

### <a name="example-request"></a>Exemplo: Pedido

```
POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata HTTP/1.1

{
    "pfxpassword":"6b75162c-b3b4-4fc1-a653-f12dae98f0c3",
    "certificaterequests":[
        "MIIDXDCCAkQCAQAwADCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAJp3\r\ncYkKPjyXCHlnoIhRg8vzdiCTNNOkxu9TDuoDuc6mX3Z4fyGCXFJXvPfTe5/s5lDK\r\n93JoVM4k7zjgSwA6fE9t9Jh3wxYM8A4i+gWgoxl/NWv7YUyK6WsI/pYTDEcJZ6Tf\r\nqy3yIORO8NxWOGoyolDXJOZSv67UyBxcEntt249iIY8xQe5V4OtgesBI6kyg+Iux\r\nYQXX2gJj3HKTFXqbuluO5hsktJWGx25PHsJeyaNbqPpVnXxlMUoQRu/ZYjypMkwL\r\n+tttx51XB8GZ4qIgb/vmEJLBPnWtrSXf0KljJ+pls0onUYm1oToADjIaJAji9V/1\r\nvoeIN42tCEGu0jk5dfkCAwEAAaCCARUwGgYKKwYBBAGCNw0CAzEMFgo2LjMuOTYw\r\nMC4yMC4GCSqGSIb3DQEJDjEhMB8wHQYDVR0OBBYEFG4Gs6VZ7mLlHY0hT9u89PiQ\r\nRf5JMFsGCSsGAQQBgjcVFDFOMEwCAQUMIUppbWFjby5yZWRtb25kLmNvcnAubWlj\r\ncm9zb2Z0LmNvbQwQUkVETU9ORFx2LWppYnJhbgwSRklNQ01Nb2Rlcm5BcHAuZXhl\r\nMGoGCisGAQQBgjcNAgIxXDBaAgEAHlIATQBpAGMAcgBvAHMAbwBmAHQAIABTAG0A\r\nYQByAHQAIABDAGEAcgBkACAASwBlAHkAIABTAHQAbwByAGEAZwBlACAAUAByAG8A\r\ndgBpAGQAZQByAwEAMA0GCSqGSIb3DQEBBQUAA4IBAQAufxWTdhOvsxGTQfDS2Wsu\r\nQpGRPWdM7JA/9IAv4XvfMeJIsrhUHPadjQHwAoOsZ15dvErcysbXN1Y2ZfyCKm0K\r\nEiBn3rQqSldwJH4C5zpPD3jV4j4v3a7w9G7Z8eMjMxS+tnj2FjOCUjYohDPo2bJk\r\nLFn1X4+fP6oMdWRO8VWGDAVLy1nnlvnHIiWst1oOKUAdY7kG42FjYQ4HoexrYwfe\r\nrCWubI8248BljPicszpZpP1wL7DTU6z+er/pSHN3tN9Z//be+hY0rbJ430KgNFzZ\r\nV6bEQIpKO6SJX4aL4h7GO9goqBtTT4XbV3yAHwxNdnZLId/4HPOEwSSTtey6eMAd\r\n"
    ]
}
```

### <a name="example-response"></a>Exemplo: Resposta

```
HTTP/1.1 200 OK

[
    {
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "CertificateId":0,
        "Flags":65536,
        "UseLocalMachineStore":false,
        "Error":null,
        "Pkcs7":"MIIUhgYJKoZIhvcNAQcCoIIUdzCCFHMCAQMxCzAJBgUrDgMCGgUAMH0GCCsGAQUF\r\nBwwDoHEEbzBtMGcwIQIBAQYIKwYBBQUHBwExEjAQAgEAMAMCAQEMBklzc3VlZDBC\r\nAgECBgorBgEEAYI3CgoBMTEwLwIBADADAgEBMSUwIwYJKwYBBAGCNxURMRYEFKX+\r\nPFP5+eY7IqVYhhqNGso36YkfMAAwAKCCEhgwggXkMIIDzKADAgECAhBEPCpUtZzW\r\nnUwJsYqbAutVMA0GCSqGSIb3DQEBBQUAMEYxHjAcBgNVBAoTFU1pY3Jvc29mdCBD\r\nb3Jwb3JhdGlvbjEkMCIGA1UEAxMbTWljcm9zb2Z0IENvcnBvcmF0ZSBSb290IENB\r\nMB4XDTAzMDkxOTE5MjE0MFoXDTE5MDkxOTE5Mjg0MVowRjEeMBwGA1UEChMVTWlj\r\ncm9zb2Z0IENvcnBvcmF0aW9uMSQwIgYDVQQDExtNaWNyb3NvZnQgQ29ycG9yYXRl\r\nIFJvb3QgQ0EwggIiMA0GCSqGSIb3DQEBAQUAA4ICDwAwggIKAoICAQCwlxxfvND/\r\nRSTgWHITf6YQcTz9M3hPStZ0N7akq7hcOBFmK3gDSw8ikTkh+2ux6ClEfN6PMy89\r\nFAmg3JFZppd5mkTgbyJk53G63w5Kj//x/3PxjFl7xr+UBR1Q5RUt8RI28/Gp6TJO\r\nzDD8/3zw5daxHrSQ7ZnzDpauna06Nt3DTDHtY0himCAzirENec/F9RrK3zznROJ3\r\nXG7NwtVPYJGOVz2FcK0ElRy++UUKBEOmUqkpQwKfh1R8PRQQZ4Qm69vgoMgosV1q\r\nqQjYPEFu4PCEE2Xqrcg7rzTUICLkXr0XVt5fJXL+ATwnFLyIrebWTiPs5lkzsTiw\r\nK3ypNwuu+Vkq19tn54k5A7L14VOo+JsnLfm1+fn6cni5WjCnu64qRVjjOZ6xBs5b\r\nLG4g4P5DaMbtOSdhZgwUSN/HEByXkJRTCjZrmoPI3sjV8/OFrGyYav4vvDu93uum\r\n28wb2MtRxAmkJn118IWhaJuGv7rUxKax4keFh6L5q7n9RSvx1byiAqmJZ6hd1k/a\r\nnT83ZSJ0kUIvTLlekvboNvnMMCT+KWgKoihhGxHGn2f0jTVmKN6n81xtNgvmShv6\r\nz2A+RZ1KkMuGkkVDHRTd5j4TsqMYfFS3iT4Z7mu/I5A28JSPmBo1T5Ee0rLOfCee\r\nnTz3gV4ExvcoIIpAzWOhxsPgKyM3nCvdyQIDAQABo4HNMIHKMAsGA1UdDwQEAwIB\r\nhjAPBgNVHRMBAf8EBTADAQH/MB0GA1UdDgQWBBQjDJiGt7thkVDAM7EG1URohvkI\r\n+DAQBgkrBgEEAYI3FQEEAwIBADB5BgNVHSAEcjBwMG4GBFUdIAAwZjBkBggrBgEF\r\nBQcCAjBYHlYAaAB0AHQAcAA6AC8ALwB3AHcAdwAuAG0AaQBjAHIAbwBzAG8AZgB0\r\nAC4AYwBvAG0ALwBwAGsAaQAvAG0AcwBjAG8AcgBwAC8AYwBwAHMALgBoAHQAbTAN\r\nBgkqhkiG9w0BAQUFAAOCAgEAGT7qt8VUYB2BvQ+OnWE7TUyUzfR03Qt5LeTVqTRh\r\njQV2KgxAk15IsJSbtN80MnoPtWGFacj3KOtclD5gAtM2RHjK/LIdbOz2x2Rvwthq\r\nuVgK4MP5n08BzThQc2QK1GGmYYDom6H3cRGCZNraCKWtH0R7poNgt+xS/gBE9kk/\r\nRcnOlTRKMBKp9fwTcQYZMtWbQZgXOfiC/rIB23j1HfNxE6/lOTT+NQDlh0jxPCir\r\nVRCR1muAZfmzi2QBfuSFOAOXVfRrr17Ad7F70qkKlpoC6CKBSy7Khz1pnyF5Q2kR\r\nnqPoTxa0l4G3Js6/jNxgA+cw9r5//1v2TUeOnVjKuqzLd68edCQWTIKWvIqJ2mZ4\r\n75qVSjwvSLLY9Gzc7wkVFsBd6Ncj47YXJMMaihmbMcKqFvCFQWTk7aQpezNuJSJ/\r\nvhnuktQ87Ngi3cToSE1XYKpb7fRuz9heStigE7ODZ3y7lR/nK5MsrO0z9Fot5h8v\r\ndclQeodBLQhCggniBj5RrQG/EW71UzwNvyzQ7jz0qlBax8OwJMHSHrBwPnHHJxT1\r\nWx0f6Hd0vRztkxXWUxe7Yjork5LpRqA4Kldjj5w+Sb1zMIAns7I6fWlKCAOYw6Ob\r\nHiJL+wKADhXIgXktufKvUwbwUyR9g/RcVTUSJ1pXjlgpIiCFGvJvsY7qiICU34Ws\r\nTaEwggYEMIIE7KADAgECAhMbAACx9OEai7qYjyyzAAEAALH0MA0GCSqGSIb3DQEB\r\nBQUAMDAxFTATBgNVBAsTDE1pY3Jvc29mdCBJVDEXMBUGA1UEAxMOTVNJVCBVc2Vy\r\nIENBIDIwHhcNMTUwNzA3MjMyNzMzWhcNMTYwNzA2MjMyNzMzWjCBnTETMBEGCgmS\r\nJomT8ixkARkWA2NvbTEZMBcGCgmSJomT8ixkARkWCW1pY3Jvc29mdDEUMBIGCgmS\r\nJomT8ixkARkWBGNvcnAxFzAVBgoJkiaJk/IsZAEZFgdyZWRtb25kMRUwEwYDVQQL\r\nEwxVc2VyQWNjb3VudHMxJTAjBgNVBAMTHEppbWFjbyBCcmFubmlhbiAoQXF1ZW50\r\nIExMQykwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCad3GJCj48lwh5\r\nZ6CIUYPL83YgkzTTpMbvUw7qA7nOpl92eH8hglxSV7z303uf7OZQyvdyaFTOJO84\r\n4EsAOnxPbfSYd8MWDPAOIvoFoKMZfzVr+2FMiulrCP6WEwxHCWek36st8iDkTvDc\r\nVjhqMqJQ1yTmUr+u1MgcXBJ7bduPYiGPMUHuVeDrYHrASOpMoPiLsWEF19oCY9xy\r\nkxV6m7pbjuYbJLSVhsduTx7CXsmjW6j6VZ18ZTFKEEbv2WI8qTJMC/rbbcedVwfB\r\nmeKiIG/75hCSwT51ra0l39CpYyfqZbNKJ1GJtaE6AA4yGiQI4vVf9b6HiDeNrQhB\r\nrtI5OXX5AgMBAAGjggKnMIICozAdBgNVHQ4EFgQUbgazpVnuYuUdjSFP27z0+JBF\r\n/kkwHwYDVR0jBBgwFoAUpXbGPDORam2fEWcb+yBqCK3CR1gwgccGA1UdHwSBvzCB\r\nvDCBuaCBtqCBs4YraHR0cDovL2NvcnBwa2kvY3JsL01TSVQlMjBVc2VyJTIwQ0El\r\nMjAyLmNybIZCaHR0cDovL21zY3JsLm1pY3Jvc29mdC5jb20vcGtpL21zY29ycC9j\r\ncmwvTVNJVCUyMFVzZXIlMjBDQSUyMDIuY3JshkBodHRwOi8vY3JsLm1pY3Jvc29m\r\ndC5jb20vcGtpL21zY29ycC9jcmwvTVNJVCUyMFVzZXIlMjBDQSUyMDIuY3JsMIGZ\r\nBggrBgEFBQcBAQSBjDCBiTA6BggrBgEFBQcwAoYuaHR0cDovL2NvcnBwa2kvYWlh\r\nL01TSVQlMjBVc2VyJTIwQ0ElMjAyKDEpLmNydDBLBggrBgEFBQcwAoY/aHR0cDov\r\nL3d3dy5taWNyb3NvZnQuY29tL3BraS9tc2NvcnAvTVNJVCUyMFVzZXIlMjBDQSUy\r\nMDIoMSkuY3J0MAsGA1UdDwQEAwIF4DA8BgkrBgEEAYI3FQcELzAtBiUrBgEEAYI3\r\nFQiDz4lNrfIChaGfDIL6yn2B4ft0gU//6HCC5+lqAgFkAgEaMCsGA1UdJQQkMCIG\r\nCisGAQQBgjcUAgIGCisGAQQBgjcqAgEGCCsGAQUFBwMCMDcGCSsGAQQBgjcVCgQq\r\nMCgwDAYKKwYBBAGCNxQCAjAMBgorBgEEAYI3KgIBMAoGCCsGAQUFBwMCMDEGA1Ud\r\nEQQqMCigJgYKKwYBBAGCNxQCA6AYDBZ2LWppYnJhbkBtaWNyb3NvZnQuY29tMBcG\r\nA1UdIAQQMA4wDAYKKwYBBAGCNyoBBTANBgkqhkiG9w0BAQUFAAOCAQEAY1PgSJAe\r\nwYmcupneUC8JPNL1SvRFjuOR/OH6gPc27zIHlFiYGHsSzNxho0odSuQLcYUOi8v6\r\n4xA4mf1Gr8o/J3j+y1rEq6PaWJ7m4knaiwnfMQ2+uRKoo84N3XC0+YSrKO05k/WL\r\nOhFaJvi5YBfC0HbGKjpEL+U3uCUhvfD+S0/ksWLIfkhpNo/+rwk/ewIGneofGHjZ\r\nA2oQNDbPPcSaZOzE1ChmmPQmm7TyunDy2ZmWAmSR56OasmaAUijOOZql5UN5ilXY\r\nzD+BCn3xHBOpr3kzyFbqAsC2lXpuS+NG94l+dOMQJPEdOukfISNjcgYgtpFmXjui\r\nGfwtfdVE/KTxBjCCBiQwggQMoAMCAQICE38AAADfhuWsYq/tacgAAAAAAN8wDQYJ\r\nKoZIhvcNAQEFBQAwRjEeMBwGA1UEChMVTWljcm9zb2Z0IENvcnBvcmF0aW9uMSQw\r\nIgYDVQQDExtNaWNyb3NvZnQgQ29ycG9yYXRlIFJvb3QgQ0EwHhcNMTQxMDEzMjAz\r\nMzA0WhcNMTgxMDEzMjA0MzA0WjAwMRUwEwYDVQQLEwxNaWNyb3NvZnQgSVQxFzAV\r\nBgNVBAMTDk1TSVQgVXNlciBDQSAyMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIB\r\nCgKCAQEAwcZeZu0/p1gSOVOqO+nqPLg4mkLuouH1mwoxOTAobyPBqGRbqVVPSjYo\r\nBiz2v2gzp6xgZgptLIohbueeqdhaOMU1jahvkqS6ip+CesmZcY9J0nknH4T6SZcu\r\np7By+qXOJkdaeNkQHpuREOdM/0cBAfaKl08tFD/ayhUBv/ScAXQ7Xl6ECMn+tGpG\r\nsGmd4yg7mX8HDoK+rcoBpVgelisMOf7WgYrN+8jVKDdgb6WSgJH+H+AxSRyJN3MY\r\nDUwZMMyv40k/a7II+7H1A4Z/EvqZrLJhVQAkWb1Zl2f+Gh8O+G9I3U7Dan1Vlzs6\r\ndpGJdYl9+Y7DAmcEClHtuvYxDxwizQIDAQABo4ICHzCCAhswEAYJKwYBBAGCNxUB\r\nBAMCAQEwIwYJKwYBBAGCNxUCBBYEFLS/ty4QFzkKvojF1IuvOIqBAxTeMB0GA1Ud\r\nDgQWBBSldsY8M5FqbZ8RZxv7IGoIrcJHWDAXBgNVHSAEEDAOMAwGCisGAQQBgjcq\r\nAQUwNgYDVR0lBC8wLQYIKwYBBQUHAwIGCisGAQQBgjcUAgIGCisGAQQBgjcqAgEG\r\nCSsGAQQBgjcVBTAZBgkrBgEEAYI3FAIEDB4KAFMAdQBiAEMAQTALBgNVHQ8EBAMC\r\nAYYwEgYDVR0TAQH/BAgwBgEB/wIBADAfBgNVHSMEGDAWgBQjDJiGt7thkVDAM7EG\r\n1URohvkI+DCBnQYDVR0fBIGVMIGSMIGPoIGMoIGJhh1odHRwOi8vY29ycHBraS9j\r\ncmwvbXNjcmNhLmNybIY0aHR0cDovL21zY3JsLm1pY3Jvc29mdC5jb20vcGtpL21z\r\nY29ycC9jcmwvbXNjcmNhLmNybIYyaHR0cDovL2NybC5taWNyb3NvZnQuY29tL3Br\r\naS9tc2NvcnAvY3JsL21zY3JjYS5jcmwwdQYIKwYBBQUHAQEEaTBnMCkGCCsGAQUF\r\nBzAChh1odHRwOi8vY29ycHBraS9haWEvbXNjcmNhLmNydDA6BggrBgEFBQcwAoYu\r\naHR0cDovL3d3dy5taWNyb3NvZnQuY29tL3BraS9tc2NvcnAvbXNjcmNhLmNydDAN\r\nBgkqhkiG9w0BAQUFAAOCAgEAlVSTBr2dtBMZC3akmfhO+Be2MJ6fL5795Sx28+T0\r\nDKeHcM7N3G/7dTvA8+5hEj1DXPM2BItOJ9Jr9fpOxG7VqAjqiO4xKMXtPhjcMUWW\r\nZYzhK8ZBFFdurHfmF7f+9IC0Dbs8Ghh18+7zcIZt0AVmL+LWLMzfjaTC8E+PaZod\r\nVaTqAF/0pe0ca7VjN+1v1u540Ucp6YZUd8BREMOQQ9Z/P7zMsYNf3kSPtGUBKpo2\r\niSfDPQ6Qri1D/toP9neK08xpyZIItWiWAnYjJEbMu8wBu1yQGLX8oWWm2KU6DwEL\r\nlj9uy23CTeCyhyx/hGXIZWJzHfRlUFyG73PhOECd0aM8CPqAWyXPsVqQza1u2USu\r\nfHcAGlel1j5LmHtcXQPp6tlWrRzGaYDuXUXiOErUfnp8OZiZDLX72yRslfIVBIJe\r\ntM9qA09mirpGIBrO+8h4l+Vyb+nnaLhqsOm1RiR4IB3K3F//VdTvZwUeNIZ54Fc4\r\n9xXnDYtJiAIzkVan+1GXHEwGmm2c1JnYqseKiVaI+HQUkZIgsXU2XNhmpZkuauah\r\nt84xV1Iwm3UPJOKCOSEBPCQ6o3dxKhyYT9c2aOjl1rFj5HdqWdFu7766TCwidCHS\r\nbjBXt05l9qxZp/gcIQ21Z+zlwFwFV/ajwR2gTF3hXoyhool4mL9UDh4BtlrD8rR/\r\nCEYxggHEMIIBwAIBATBdMEYxHjAcBgNVBAoTFU1pY3Jvc29mdCBDb3Jwb3JhdGlv\r\nbjEkMCIGA1UEAxMbTWljcm9zb2Z0IENvcnBvcmF0ZSBSb290IENBAhN/AAAA34bl\r\nrGKv7WnIAAAAAADfMAkGBSsOAwIaBQCgPjAXBgkqhkiG9w0BCQMxCgYIKwYBBQUH\r\nDAMwIwYJKoZIhvcNAQkEMRYEFCX8cMHfnlERZ7ljqFmAMtU5Eb9vMA0GCSqGSIb3\r\nDQEBAQUABIIBAAh8M9OdfUroL4G4KTi33r00870fCY3FFdjSAYVC81I7v/L70FaB\r\nysqM/hD0t8uzCF0PgHNHtFS2uB7TrQfGSItTGThzMu3QENpUzQBLeotvuBvg2YP5\r\nlYTQ5+7j4Q6bYw2Zo489894nfJSPy3fe10raHRsPqbGB+4SB403Mvlvc4A+OHK/I\r\n9jA8J7GBjeJKhfG3jOV/u0m+zvQ7mynttG2LRk+RHJjrVkq4uLo0ImTUue70Lzqj\r\ns2Dvd7aAJsOks4V6msQLQVGftF4WPrxMtpLnPzOK7n2MVnGxD6tXIqCDgvrbiffi\r\ne2+2JNFqiXxj/jTtmZSeDUJQpYnxVzcgoyI=\r\n",
        "Pfx":null,
        "IsExternal":false,
        "isServerGenerated":false,
        "isPfx":false,
        "isPkcs7":true,
        "isKeyHistory":false
    }
]
```

## <a name="see-also"></a>Ver também

- [Microsoft.Clm.Provision.Exemétodo de operações fofas.Inscreva-se](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.enroll.aspx)
- [Microsoft.Clm.Shared.Certificates.Certificate class](https://msdn.microsoft.com/library/microsoft.clm.shared.certificates.certificate.aspx)
