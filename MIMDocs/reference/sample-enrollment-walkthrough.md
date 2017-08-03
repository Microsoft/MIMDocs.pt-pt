---
title: "Instruções de inscrição de exemplo | Microsoft Docs"
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
ms.assetid: 92b97803-9475-4b90-9a6c-430f107a167d
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 574a4d6fc2d5d4a51483a371cf96c2d48c582a8b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="sample-enrollment-walkthrough"></a>Instruções de inscrição de exemplo
Este tópico mostra os passos necessários para efetuar uma Virtual Smart inscrição Self-Service do cartão. Mostra um pedido aprovado automaticamente, com um PIN de utilizador definido.
1.  O cliente autentica o utilizador, em seguida, pede uma lista de modelos de perfil que o utilizador autenticado pode inscrever para:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates.
    ```
    <br/>
2.  O cliente apresenta a lista resultante para o utilizador. O utilizador seleciona um modelo de perfil vSC (Smart Virtual Card) com o nome "Virtual smart card VPN" e o UUID *97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1*.

3.  O cliente obtém a política de inscrição do modelo de perfil selecionado, utilizando o UUID devolvido no passo anterior:

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll
    ```
 <br/>   
4.  O cliente analisa o campo "O DataCollection" na política de devolvido, pena realçar que é apresentada uma coleção de dados único item designada por "Razão do pedido". O cliente também indica que o sinalizador "collectComments" está definido como *falso*, pelo que não solicitar ao utilizador introduzir qualquer.

5.  Após o utilizador ter introduzido a razão para exigir certificados, o cliente cria um pedido:

    ```
    POST /CertificateManagement/api/v1.0/requests

    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```
<br/>
6.  O servidor com êxito cria o pedido e devolve o URI do pedido para o cliente: /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5.

7.  O cliente obtém o objeto pedido ao chamar o URI devolvido:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```
<br/>
8.  O cliente verifica que a propriedade de "state" no pedido está definida como "aprovada". Pode iniciar a execução do pedido.

9.  O cliente examina o pedido para ver se existe um smart card já associado ao pedido ao analisar o conteúdo do parâmetro "newsmartcarduuid".

10. Uma vez que apenas contém um GUID vazio, o cliente tem de utilizar um cartão existente não já em utilização por MIM CM, ou crie uma no caso do modelo de perfil que está a ser configurado para smart cards virtuais.

11. Uma vez que a última opção tem sido indicado para o cliente através da consulta inicial para modelos de perfil enrollable (passo 1), o cliente terá agora de criar um dispositivo de virtual smart card.

12. O cliente obtém a política de smart card do modelo de perfil (utilizando o UUID do modelo selecionado no passo 3):

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards
    ```
13. A política de smart card irá conter a chave de administrador predefinida para o cartão na propriedade "DefaultAdminKeyHex". Ao criar o smart card, o cliente tem de definir a chave de administrador inicial do smart card para esta chave.  

14. Após criar o dispositivo de smart card, o cliente deve atribuí-lo para o pedido:

    ```
    POST /CertificateManagement/api/v1.0/smartcards

    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
    }
    ```
<br/>
15. O servidor irá responder ao cliente com um URI para o objeto criado recentemente de smart card: api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644.

16. O cliente obtém o objeto de smart card:

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```
<br/>
17. Ao verificar o valor do sinalizador "diversifyadminkey" na política de smart card obtido no passo 12, o cliente sabe tem diversificar a chave de administrador.

18. O cliente obtém a chave de administrador proposto:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9
    ```
<br/>
19. O cliente tem de autenticar para o cartão como administrador para definir a chave de administrador. Para tal, o cliente pede um desafio de autenticação do smart card e envia-o para o servidor:

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=false
    ```

    O servidor envia a resposta de desafio no corpo da resposta HTTP. Localize a cadeia de desafio hexadecimal, converter para base64 e transmita-o como um parâmetro no URL. O URL irá devolver a resposta de outra, que tem de ser convertida para o formato hexadecimal.
<br/>
20. O servidor envia a resposta de desafio no corpo da resposta HTTP e o cliente utiliza-o para diversificar a chave de administrador.

21. O cliente indica que o utilizador tem de fornecer o pin pretendido inspecionando o campo "UserPinOption" na política de smart card.

22. Após o utilizador ter introduzido o pin pretendido para uma caixa de diálogo, o cliente executa uma autenticação de resposta de desafio, conforme descrito no passo 19, a única diferença é que o sinalizador diversified agora deve ser definido como "true":

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&diversified=true
    ```
<br/>
23. O cliente utiliza a resposta recebida do servidor para definir o pin de utilizador pretendido.

24. O cliente está agora pronto para gerar pedidos de certificado. Consulta os parâmetros de geração do certificado do modelo do perfil para determinar a forma como os pedidos/chaves deve ser gerados.

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```
<br/>
25. O servidor responde com um único objeto JSON serializado CertificateRequestGenerationOptions com detalhes sobre o exportability do chave, certificado nome amigável, hash algoritmo, o algoritmo de chave, o tamanho da chave, etc. O cliente utiliza estes parâmetros para gerar um pedido de certificado.

26. O pedido de certificado único é submetido ao servidor. O cliente foi, além disso, especifique uma palavra-passe PFX deve ser utilizada para desencriptar blobs PFX no caso em que o modelo de certificado Especifica arquivar os certificados na AC, ou seja, a AC gera o par de chaves, em seguida, envia-a para o cliente. O cliente também pode optar por adicionar alguns comentários.

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata

    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```
<br/>
27. Após alguns segundos, o servidor responde com um objeto de Microsoft.Clm.Shared.Certificate único, serializado para JSON. Ao verificar o sinalizador "isPkcs7", o cliente toma esta resposta não é um blob PFX. O cliente extrai o blob por base64 descodificar a cadeia "pkcs7" e instala-o.

28. O cliente marca pedido como concluída.

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5

    {
        "status":"Completed"
    }
    ```
