---
title: "Renovação do self-service de smart card | Microsoft Identity Manager"
description: Saiba como inscrever smart cards para utilizadores sem acesso de administrador aos respetivos computadores para que possam utilizar o Gestor de Certificados.
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: bfabc562-a2f0-4cff-ac31-36927f41e102
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: 2fddede481b5ba677d0d463be4b14cda4b463865


---

# Inscrição de smart cards para não administradores
Se um utilizador não for um administrador local no respetivo computador, não poderá inscrever smart cards no respetivo computador por predefinição. O procedimento seguinte permite-lhe contornar esta limitação.

## Ativar a renovação do smart card para não administradores no Gestor de Certificados do MIM 2016

1.  **Descompactar o ficheiro appx**

    Obtenha um certificado de assinatura. Siga os passos para [Assinar aplicações do Windows 8 com um PKI interno](http://blogs.technet.com/b/deploymentguys/archive/2013/06/14/signing-windows-8-applications-using-an-internal-pki.aspx). Pare quando chegar a “Assinar a Aplicação”. Dê um nome ao ficheiro pfx exportado. Exporte também para um ficheiro .cer e importe-o para o cliente através do ficheiro cer do novo certificado de assinatura.

    Execute o seguinte para descompactar o ficheiro appx:

    `makeappx unpack /l /p <app package name>.appx /d ./appx`

    `ren <app package name>.appx <app package name>.appx.old`

    `cd appx`

2.  **Modificar o ficheiro de configuração**

    Mude o nome do ficheiro denominado `CustomDataExample.xml custom.data`. A aplicação CM irá procurar este nome de ficheiro.

    Edite o ficheiro custom.data e modifique o seguinte:

    1.  No elemento &lt;NonAdmin&gt;, altere o valor do atributo Value para “True”

    2.  Guarde o ficheiro e saia do editor

    3.  Elimine o ficheiro denominado AppxSignature.p7x

    4.  Edite o ficheiro denominado AppxManifest.xml

    5.  No elemento &lt;Identity&gt;, modifique o valor do atributo Publisher para o assunto do seu certificado de assinatura, por exemplo, “CN=ABCD”

        Neste campo, o assunto deve ser o mesmo que o assunto do certificado de assinatura que estiver a utilizar para assinar a aplicação.

    6.  Guarde o ficheiro e saia do editor.

3.  **Voltar a compactar e assinar o pacote de aplicação (ficheiro appx)**

    Execute o seguinte para compactar e assinar o ficheiro appx:

    `cd ..`

    `makeappx pack /l /d .\appx /p <app package name>.appx`

    s`igntool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.appx`

4.  Duplique o modelo de perfil e adicione a chave de administrador inicial para configurar o servidor MIM:

    1.  Inicie sessão no portal do CM como um utilizador com privilégios administrativos.

    2.  Aceda a **Administração** &gt; **Gerir Modelos de Perfil** e certifique-se de que seleciona a caixa junto ao modelo de perfil que criou e, em seguida, clique em Copiar um modelo de perfil selecionado.

    3.  Escreva o nome do modelo de perfil, adicione “nonAdmin” e clique em **OK**.

    4.  Quando as definições gerais do modelo de perfil forem apresentadas, desloque-se para baixo e, em **Configuração do Smart Card**, clique em **Alterar Definições**.

    5.  Em **Valor inicial da chave admin (hex)**, introduza a chave de administrador predefinida: “010203040506070801020304050607080102030405060708”

    6.  Desloque-se para baixo e clique em **OK**.

5.  **Criar uma conta não administrativa no computador cliente**

    Os utilizadores que não são administradores não podem criar smart cards virtuais no TPM, pelo que tem de os criar em seu nome.

6.  **Criar um smart card virtual através do TpmVscMgr**

    Efetue o seguinte (ainda como o administrador) para criar um smart card virtual em branco num computador. Isto pode ser efetuado através do Intune, do SCCM ou das políticas de grupo.

    `TpmVscMgr create /name MyVSC /pin default /adminkey default /generate`

7.  **Instalar a aplicação CM na conta não administrativa**

8.  **Iniciar a aplicação CM e inscrever-se num smart card virtual**



<!--HONumber=Jul16_HO3-->


