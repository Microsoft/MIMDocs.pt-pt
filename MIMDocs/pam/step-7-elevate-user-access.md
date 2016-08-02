---
title: "Implementação PAM passo 7 – acesso de utilizador| Microsoft Identity Manager"
description: "Como passo final, conceda acesso temporário de utilizador com privilégios para demonstrar que a implementação Privileged Access Management foi concluída com êxito."
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 9b5b7460e6307ab38b1b9356a638eb0200fd97d1
ms.openlocfilehash: 009091a65dba31de2066e45930e438442fcd89a0


---

# Passo 7 – Elevar o acesso de um utilizador

>[!div class="step-by-step"]
[« Passo 6 ](step-6-transition-group-to-pam.md)


Este passo demonstra que um utilizador pode pedir acesso a uma função através de MIM.

## Certifique-se que a Jen não pode aceder ao recurso com privilégios
Sem privilégios elevados, a Jen não pode aceder ao recurso com privilégios na floresta CORP.

1. Terminar sessão no CORPWKSTN para remover quaisquer ligações abertas em cache.
2. Inicie sessão no CORPWKSTN como *CONTOSO\Jen* e mude para a vista de **Ambiente de trabalho**.
3. Abra uma linha de comandos DOS.
4. Escreva o comando `dir \\corpwkstn\corpfs`. A mensagem de erro **Acesso negado** deve aparecer.
5. Deixe a janela de linha de comandos aberta.

## Pedir acesso privilegiado do MIM.
1. No CORPWKSTN, ainda como CONTOSO\Jen, escreva o seguinte comando.

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. Quando lhe for pedido, escreva a palavra-passe para a conta PRIV.Jen. Será apresentada uma nova janela de linha de comandos.
3. Quando for apresentada a janela do PowerShell, escreva os comandos seguintes.

    > [!NOTE]
    > Depois de executar estes comandos, os seguintes passos são sensíveis ao tempo.

    ```
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. Quando concluído, feche a janela do PowerShell.
5. Na janela de linha de comandos DOS, escreva o seguinte comando

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. Escreva a palavra-passe para a conta PRIV.Jen. Será apresentada uma nova janela de linha de comandos.

## Valide o acesso elevado.
Na janela de linha de comandos recém aberta, escreva os seguintes comandos.

```
whoami /groups
dir \\corpwkstn\corpfs
```

Se o comando dir falhar com a mensagem de erro **Acesso negado**, verifique novamente a relação de fidedignidade.

## Ativar a função com privilégios
Ative pedindo acesso privilegiado através do portal de amostra de PAM.

1. No CORPWKSTN, certifique-se de que tem sessão iniciada como CORP\Jen.
2. Escreva o seguinte comando na janela de linha de comandos DOS.

    ```
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. Quando lhe for pedido, escreva a palavra-passe para a conta PRIV.Jen. Será apresentada uma nova janela de browser.
4. Navegue para http://pamsrv.priv.contoso.local:8090 e certifique-se de que uma página web do portal de amostra está visível.
5. No Internet Explorer, selecione **Ferramentas** > **Opções da Internet** e clique no separador **Segurança**.
6. Clique em **Zona de intranet local** > **Sites** > **Avançadas** e, em seguida, adicione o site à zona.
7. Feche a caixa de diálogo **Opções da Internet**.
8. No separador à esquerda, clique em **Ativar**. Selecione a **Função PAM** e, em seguida, clique em **Ativar**.

> [!Note]
> Neste ambiente, também pode aprender como desenvolver aplicações que utilizam a API REST do PAM, descrita em [Referência à API REST do Privileged Access Management](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference).

## Resumo
Depois de concluir os passos nestas instruções, terá demonstrado um cenário de Privileged Access Management, no qual os privilégios de utilizador são elevados durante um período limitado de tempo, permitindo que o utilizador aceda a recursos protegidos com uma conta com privilégios separada. Assim que a sessão de elevação expira, a conta com privilégios já não consegue aceder ao recurso protegido. A decisão sobre que grupos de segurança representam funções com privilégios é coordenada pelo administrador de PAM. Depois dos direitos de acesso serem migrados para o sistema de Privileged Access Management, o acesso que era anteriormente possível com a conta de utilizador original é, agora, possível apenas ao iniciar sessão com uma conta com privilégios especiais, e disponibilizado após pedido. Como resultado, as associações de grupo para os grupos com privilégios elevados são eficazes durante um período limitado de tempo.

>[!div class="step-by-step"]
[« Passo 6 ](step-6-transition-group-to-pam.md)



<!--HONumber=Jul16_HO4-->


