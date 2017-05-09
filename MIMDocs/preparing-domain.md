---
title: "Configurar um domínio para o Microsoft Identity Manager 2016 | Documentos da Microsoft"
description: "Criar um controlador de domínio do Active Directory antes de instalar o MIM 2016"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: e16bcc36fe4bccb621ba4d649aa0b015f2adbcdd
ms.lasthandoff: 01/24/2017


---

# <a name="set-up-a-domain"></a>Configurar um domínio

>[!div class="step-by-step"]
[Windows Server 2012 R2 »](prepare-server-ws2012r2.md)

O Microsoft Identity Manager (MIM) funciona com o seu domínio do Active Directory (AD). Já deve ter o AD instalado, e certifique-se de que tem um controlador de domínio no seu ambiente para um domínio que possa administrar.

Este artigo explica os passos para preparar o seu domínio de modo a trabalhar em conjunto com o MIM.

## <a name="create-user-accounts-and-groups"></a>Configurar contas de utilizador e grupos

Todos os componentes de implementação do MIM têm as suas próprias identidades no domínio. Isto inclui os componentes MIM como Serviço e Sincronização, bem como SharePoint e SQL.

> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio – **nomedoservidormim**
> - Nome de domínio – **contoso**
> - Palavra-passe – **Pass@word1**

1. Inicie sessão no controlador de domínio como o administrador do domínio (*por exemplo, Contoso\Administrador*).

2. Crie as seguintes contas de utilizador para os serviços MIM. Inicie o PowerShell e escreva o seguinte script do PowerShell para atualizar o domínio.

    ```
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    New-ADUser –SamAccountName MIMMA –name MIMMA
    Set-ADAccountPassword –identity MIMMA –NewPassword $sp
    Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSync –name MIMSync
    Set-ADAccountPassword –identity MIMSync –NewPassword $sp
    Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMService –name MIMService
    Set-ADAccountPassword –identity MIMService –NewPassword $sp
    Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSSPR –name MIMSSPR
    Set-ADAccountPassword –identity MIMSSPR –NewPassword $sp
    Set-ADUser –identity MIMSSPR –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SharePoint –name SharePoint
    Set-ADAccountPassword –identity SharePoint –NewPassword $sp
    Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SqlServer –name SqlServer
    Set-ADAccountPassword –identity SqlServer –NewPassword $sp
    Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName BackupAdmin –name BackupAdmin
    Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp
    Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1
    ```

3.  Crie grupos de segurança para todos os grupos.

    ```
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordReset –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordReset
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    ```

4.  Adicionar SPNs para ativar a autenticação Kerberos nas contas de serviço

    ```
    setspn -S http/mimservername.contoso.local Contoso\SharePoint
    setspn -S http/mimservername Contoso\SharePoint
    setspn -S FIMService/mimservername.contoso.local Contoso\MIMService
    setspn -S FIMSynchronizationService/mimservername.contoso.local Contoso\MIMSync
    ```

>[!div class="step-by-step"]
[Windows Server 2012 R2 »](prepare-server-ws2012r2.md)

