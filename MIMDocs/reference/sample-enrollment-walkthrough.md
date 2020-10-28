---
title: A amostra de inscrição a passar por / Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 92b97803-9475-4b90-9a6c-430f107a167d
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5a560e68ea820211caa1dd0a40a0143f44d5f7fb
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762012"
---
# <a name="sample-enrollment-walkthrough"></a>Passagem de inscrição de amostra
Este artigo mostra os passos necessários para realizar uma inscrição de autosserviço de cartão inteligente virtual. Apresenta um pedido aprovado automaticamente com um PIN definido pelo utilizador.

1. O cliente autentica o utilizador e, em seguida, solicita uma lista de modelos de perfil para os que o utilizador autenticado pode inscrever-se:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates.
    ```
    
2. O cliente apresenta a lista resultante ao utilizador. O utilizador seleciona um modelo de perfil vSC (cartão inteligente virtual) com o nome "Virtual Smartcard VPN" e UUID `97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1` .

3. O cliente recupera a política de inscrição do modelo de perfil selecionado utilizando o UUID devolvido no passo anterior:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll
    ```

4. O cliente analisa o campo **DataCollection** na apólice devolvida, observando que aparece um único item de recolha de dados intitulado "Request Reason". O cliente também nota que a bandeira dos **CollectComments** está definida como falsa, pelo que não leva o utilizador a introduzir nenhuma.

5. Depois de o utilizador ter introduzido o motivo para exigir os certificados, o cliente cria um pedido:

    ```
    POST /CertificateManagement/api/v1.0/requests

    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```

6. O servidor cria com sucesso o pedido e devolve o URI do pedido ao cliente:  `/CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5` .

7. O cliente recupera o objeto de pedido ligando para o URI devolvido:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```

8. O cliente verifica que o **imóvel** do Estado no pedido está definido para ser aprovado. A execução do pedido pode começar.

9. O cliente examina o pedido para verificar se existe um cartão inteligente já associado ao pedido, analisando o conteúdo do parâmetro **newsmartcarduuid.**

10. Uma vez que contém apenas um GUID vazio, o cliente deve usar um cartão existente que ainda não está a ser utilizado pela MIM CM, ou criar um se o modelo de perfil estiver a ser configurado para cartões inteligentes virtuais.

11. Uma vez que este último foi indicado ao cliente através da consulta inicial para modelos de perfil inscritos (passo 1), o cliente deve agora criar um dispositivo de cartão inteligente virtual.

12. O cliente recupera a política do cartão inteligente a partir do modelo de perfil. Utiliza o UUID do modelo que foi selecionado no passo 3:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards
    ```

13. A política do cartão inteligente contém a chave de administração padrão para o cartão na propriedade **DefaultAdminKeyHex.** Ao criar o cartão inteligente, o cliente deve definir a chave de administração inicial do cartão inteligente para esta chave.  
14. Ao criar o dispositivo de cartão inteligente, o cliente deve atribuí-lo ao pedido:

    ```
    POST /CertificateManagement/api/v1.0/smartcards

    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
    }
    ```

15. O servidor responde ao cliente com um URI ao objeto de cartão inteligente recém-criado: `api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644` .

16. O cliente recupera o objeto do cartão inteligente:

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```

17. Ao verificar o valor da bandeira **de dados diversificados** na política de cartões inteligentes obtida no passo 12, o cliente sabe que tem de diversificar a chave de administração.

18. O cliente recupera a chave de administração proposta:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9
    ```

19. O cliente deve autenticar o cartão como administrador para definir a chave de administração. Para tal, o cliente solicita um desafio de autenticação do cartão inteligente e submete-o ao servidor:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=false
    ```

    O servidor envia a resposta de desafio no organismo de resposta HTTP. Localize a cadeia de desafio hexadecimal, converta-a para base64 e passe-a como parâmetro no URL. O URL devolve outra resposta, que deve ser convertida de volta para o formato hexadecimal.

20. O servidor envia a resposta de desafio no corpo de resposta HTTP e o cliente usa-o para diversificar a chave de administração.

21. O cliente observa que o utilizador deve fornecer o pino pretendido inspecionando o campo **UserPinOption** na política do cartão inteligente.

22. Depois de o utilizador ter introduzido o pino desejado num diálogo, o cliente realiza uma autenticação de resposta de desafio, tal como delineada no passo 19, sendo que a única diferença é que a bandeira diversificada deve agora ser definida como verdadeira:

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=true
    ```

23. O cliente utiliza a resposta recebida do servidor para definir o pino de utilizador pretendido.

24. O cliente está agora pronto para gerar pedidos de certificados. Consulta os parâmetros de geração do certificado de perfil para determinar como as chaves/pedidos devem ser gerados.

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```

25. O servidor responde com um único objeto de **Opções de Certificação JSON** serializado. O objeto inclui parâmetros para a exportabilidade da chave, nome amigável de certificado, algoritmo de hashing, algoritmo chave, tamanho chave, e assim por diante. O cliente utiliza estes parâmetros para gerar um pedido de certificado.

26. O pedido de certificado único é submetido ao servidor. O cliente poderia especificar ainda uma palavra-passe PFX que deve ser usada para desencriptar quaisquer bolhas PFX quando o modelo de certificado especificar arquivar os certificados na AC, ou seja, o CA gera o par de chaves e, em seguida, envia-o para o cliente. O cliente também pode optar por adicionar alguns comentários.

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata

    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```

27. Após alguns segundos, o servidor responde com um único objeto **Microsoft.Clm.Shared.Certificate,** serializado por JSON. Ao verificar a bandeira **isPkcs7,** o cliente aprende que esta resposta não é uma bolha PFX. O cliente extrai a bolha por base64 descodindo a corda pkcs7 e instala-a.

28. O cliente marca o pedido como concluído.

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5

    {
        "status":"Completed"
    }
    ```
