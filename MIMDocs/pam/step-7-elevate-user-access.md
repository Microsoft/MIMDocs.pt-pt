---
title: Implementar o PAM passo 7 – acesso de utilizador| Documentos da Microsoft
description: Como passo final, conceda acesso temporário de utilizador com privilégios para demonstrar que a implementação Privileged Access Management foi concluída com êxito.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/17/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.openlocfilehash: bdb02eed8e22b373c6cfa5028153cad6aee9a536
ms.sourcegitcommit: 80507a128d2bc28ff3f1b96377c61fa97a4e7529
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/13/2020
ms.locfileid: "83279968"
---
# <a name="step-7--elevate-a-users-access"></a>Passo 7 – Elevar o acesso de um utilizador

> [!div class="step-by-step"]
> [« Passo 6 ](step-6-transition-group-to-pam.md)


Este passo demonstra que um utilizador pode pedir acesso a uma função através de MIM.

## <a name="verify-that-jen-cannot-access-the-privileged-resource"></a>Certifique-se que a Jen não pode aceder ao recurso com privilégios

Sem privilégios elevados, a Jen não pode aceder ao recurso com privilégios na floresta CORP.

1. Terminar sessão no CORPWKSTN para remover quaisquer ligações abertas em cache.
2. Inicie sessão no CORPWKSTN como *CONTOSO\Jen* e mude para a vista de **Ambiente de trabalho**.
3. Abra uma linha de comandos DOS.
4. Escreva o comando `dir \\corpwkstn\corpfs`. A mensagem de erro **Acesso negado** deve aparecer.
5. Deixe a janela de linha de comandos aberta.

## <a name="request-privileged-access-from-mim"></a>Pedir acesso privilegiado do MIM.

> [!NOTE]
> Recomenda-se que a estação de trabalho seja uma estação de trabalho privilegiada (PAW).  Para mais informações consulte [PAW](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations).

1. Na PRIVWKSTN, logon como PRIV\priv.jen.
2. Clique em **Iniciar,** **Executar,** e **introduza PowerShell.exe**.
3. Escreva o seguinte comando.

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. Quando lhe for pedido, escreva a palavra-passe para a conta PRIV.Jen. Será apresentada uma nova janela de linha de comandos.
3. Quando for apresentada a janela do PowerShell, escreva os comandos seguintes.

    > [!NOTE]
    > Depois de executar estes comandos, os seguintes passos são sensíveis ao tempo.

    ```PowerShell
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. Quando concluído, feche a janela do PowerShell.
5. Na janela de linha de comandos DOS, escreva o seguinte comando

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. Escreva a palavra-passe para a conta PRIV.Jen. Será apresentada uma nova janela de linha de comandos.

## <a name="validate-the-elevated-access"></a>Valide o acesso elevado.
Na janela de linha de comandos recém aberta, escreva os seguintes comandos.

```cmd
whoami /groups
dir \\corpwkstn\corpfs
```

Se o comando dir falhar com a mensagem de erro **Acesso negado**, verifique novamente a relação de fidedignidade.

## <a name="activate-the-privileged-role"></a>Ativar a função com privilégios

Ative pedindo acesso privilegiado através do portal de amostra de PAM.

1. No CORPWKSTN, certifique-se de que tem sessão iniciada como CORP\Jen.
2. Escreva o seguinte comando na janela de linha de comandos DOS.

    ```cmd
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. Quando lhe for pedido, escreva a palavra-passe para a conta PRIV.Jen. Será apresentada uma nova janela de browser.
4. Navegue `http://pamsrv.priv.contoso.local:8090` para garantir que uma página web do portal da amostra seja visível.
5. No Internet Explorer, selecione **Tools**  >  **Internet Options** e clique no separador **Segurança.**
6. Clique na **zona intranet local**  >  **Sites**  >  **Avançados** e adicione o site à zona.
7. Feche a caixa de diálogo **Opções da Internet**.
8. No separador à esquerda, clique em **Ativar**. Selecione a **Função PAM** e, em seguida, clique em **Ativar**.

> [!Note]
> Neste ambiente, também pode aprender como desenvolver aplicações que utilizam a API REST do PAM, descrita em [Referência à API REST do Privileged Access Management](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference).

## <a name="summary"></a>Resumo

Depois de concluir os passos nestas instruções, terá demonstrado um cenário de Privileged Access Management, no qual os privilégios de utilizador são elevados durante um período limitado de tempo, permitindo que o utilizador aceda a recursos protegidos com uma conta com privilégios separada. Assim que a sessão de elevação expira, a conta com privilégios já não consegue aceder ao recurso protegido. A decisão sobre que grupos de segurança representam funções com privilégios é coordenada pelo administrador de PAM. Depois dos direitos de acesso serem migrados para o sistema de Privileged Access Management, o acesso que era anteriormente possível com a conta de utilizador original é, agora, possível apenas ao iniciar sessão com uma conta com privilégios especiais, e disponibilizado após pedido. Como resultado, as associações de grupo para os grupos com privilégios elevados são eficazes durante um período limitado de tempo.

> [!div class="step-by-step"]
> [« Passo 6 ](step-6-transition-group-to-pam.md)
