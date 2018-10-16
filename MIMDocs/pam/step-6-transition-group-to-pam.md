---
title: Implementar o PAM passo 6 – Mover grupo | Documentos da Microsoft
description: Migrar um grupo para a floresta PRIV, para que possam ser geridos com Privilege Access Management.
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 7b689eff-3a10-4f51-97b2-cb1b4827b63c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f449ca475d8b1fe72203bf4cd3b5dd3c65329d13
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332940"
---
# <a name="step-6--transition-a-group-to-privileged-access-management"></a>Passo 6 - Transição de um grupo o Privileged Access Management

> [!div class="step-by-step"]
> [« Passo 5 ](step-5-establish-trust-between-priv-corp-forests.md)
> [Passo 7 »](step-7-elevate-user-access.md)

A criação da conta com privilégios na floresta PRIV é efetuada utilizando cmdlets do PowerShell. Estes cmdlets executam as seguintes funções:

- Crie um novo grupo na floresta PRIV com o mesmo identificador de segurança (SID) que um grupo na floresta CORP.  
- Crie um objeto na base de dados do Serviço MIM correspondente ao grupo da floresta PRIV.  
- Para cada conta de utilizador, crie dois objetos na base de dados do Serviço MIM, correspondentes ao utilizador na floresta CORP e à nova conta de utilizador na floresta PRIV.  
- Crie um objeto de Função de PAM na base de dados do Serviço MIM.  

Os cmdlets devem ser executados uma vez para cada grupo e uma vez para cada membro de um grupo. Os cmdlets de migração não alteram ou modificam qualquer utilizador ou grupos na floresta CORP: o administrador de PAM fá-lo-á de forma manual, posteriormente.

1. Inicie sessão no PAMSRV, diretamente ou a partir de uma estação de trabalho PRIV, como *PRIV\MIMAdmin*.

2.  Inicie o PowerShell, e escreva os seguintes comandos.

```PowerShell
   Import-Module MIMPAM
   Import-Module ActiveDirectory
```

3. Crie uma conta de utilizador correspondente no PRIV para uma conta de utilizador numa floresta existente, para fins de demonstração.

   Escreva os seguintes comandos no PowerShell.  Se não utilizou o nome *Jen* para criar o utilizador em contoso.local anteriormente, altere os parâmetros do comando conforme adequado. A palavra-passe 'Pass@word1' é apenas um exemplo e deve ser alterada para um valor de palavra-passe exclusivo.

   ```PowerShell
       $sj = New-PAMUser –SourceDomain CONTOSO.local –SourceAccountName Jen
       $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
       Set-ADAccountPassword –identity priv.Jen –NewPassword $jp
       Set-ADUser –identity priv.Jen –Enabled 1
   ```

4. Copie um grupo e o membro, Jen, a partir da CONTOSO para o domínio PRIV, para fins de demonstração.

    Execute os seguintes comandos, especificando a palavra-passe do administrador (CONTOSO\Administrator) do domínio CORP onde pedido:

   ```PowerShell
        $ca = get-credential –UserName CONTOSO\Administrator –Message "CORP forest domain admin credentials"
        $pg = New-PAMGroup –SourceGroupName "CorpAdmins" –SourceDomain CONTOSO.local                 –SourceDC CORPDC.contoso.local –Credentials $ca
        $pr = New-PAMRole –DisplayName "CorpAdmins" –Privileges $pg –Candidates $sj
   ```

    Para referência, o comando **Novo PAMGroup** aceita os seguintes parâmetros:

     -   O nome de domínio de floresta CORP no formulário de NetBIOS  
     -   O nome do grupo para copiar a partir desse domínio  
     -   O nome de NetBIOS de controlador de domínio de floresta CORP  
     -   As credenciais de um utilizador de administrador de domínio na floresta CORP  

5. (Opcional) No CORPDC, remova a conta da Jen do grupo **CONTOSO CorpAdmins**, caso ainda esteja presente.  Isto só é necessário para efeitos de demonstração, para ilustrar a forma como as permissões podem ser associadas com contas criadas na floresta PRIV.

   1.  Inicie sessão em CORPDC como *CONTOSO\Administrator*.

   2.  Inicie o PowerShell, execute o seguinte comando e confirme a alteração.

       ```PowerShell
       Remove-ADGroupMember -identity "CorpAdmins" -Members "Jen"
       ```


Se pretender demonstrar que direitos de acesso de múltiplas florestas são eficazes para a conta de administrador do utilizador, continue para o passo seguinte.

> [!div class="step-by-step"]
> [« Passo 5 ](step-5-establish-trust-between-priv-corp-forests.md)
> [Passo 7 »](step-7-elevate-user-access.md)
