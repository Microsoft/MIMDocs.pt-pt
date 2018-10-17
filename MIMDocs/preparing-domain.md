---
title: Configurar um domínio para o Microsoft Identity Manager 2016 | Documentos da Microsoft
description: Criar um controlador de domínio do Active Directory antes de instalar o MIM 2016
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/26/2017
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: cbba7abe810fea0943e087206f7b0b6e3baa7cbb
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49357889"
---
# <a name="set-up-a-domain"></a>Configurar um domínio

> [!div class="step-by-step"]
> [Windows Server 2016»](prepare-server-ws2016.md)

O Microsoft Identity Manager (MIM) funciona com o seu domínio do Active Directory (AD). Já deve ter o AD instalado, e certifique-se de que tem um controlador de domínio no seu ambiente para um domínio que possa administrar.

Este artigo explica os passos para preparar o seu domínio de modo a trabalhar em conjunto com o MIM.

## <a name="create-user-accounts-and-groups"></a>Configurar contas de utilizador e grupos

Todos os componentes de implementação do MIM têm as suas próprias identidades no domínio. Isto inclui os componentes MIM como Serviço e Sincronização, bem como SharePoint e SQL.

> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio - **corpdc**
> - Nome de domínio – **contoso**
> - Nome do servidor de serviço de MIM - **corpservice**
> - Nome do servidor de sincronização de MIM - **corpsync**
> - Nome do SQL Server - **corpsql**
> - Palavra-passe – <strong>Pass@word1</strong>

1. Inicie sessão no controlador de domínio como o administrador do domínio (*por exemplo, Contoso\Administrador*).

2. Crie as seguintes contas de utilizador para os serviços MIM. Inicie o PowerShell e escreva o seguinte script do PowerShell para atualizar o domínio.

    ```PowerShell
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    New-ADUser –SamAccountName MIMINSTALL –name MIMMA
    Set-ADAccountPassword –identity MIMINSTALL –NewPassword $sp
    Set-ADUser –identity MIMINSTALL –Enabled 1 –PasswordNeverExpires 1
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
    New-ADUser –SamAccountName MIMpool –name BackupAdmin
    Set-ADAccountPassword –identity MIMPool –NewPassword $sp
    Set-ADUser –identity MIMPool –Enabled 1 -PasswordNeverExpires 1
    ```

3.  Crie grupos de segurança para todos os grupos.

    ```PowerShell
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordReset –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordReset
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMInstall
    ```

4.  Adicionar SPNs para ativar a autenticação Kerberos nas contas de serviço

    ```CMD
    setspn -S http/mim.contoso.com Contoso\mimpool
    setspn -S http/mim Contoso\mimpool
    setspn -S http/passwordreset.contoso.com Contoso\mimsspr
    setspn -S http/passwordregistration.contoso.com Contoso\mimsspr
    setspn -S FIMService/mim.contoso.com Contoso\MIMService
    setspn -S FIMService/corpservice.contoso.com Contoso\MIMService
    ```
5.  Durante a configuração, precisamos adicionar os seguintes registos DNS "A" para a resolução de nome adequada

- mim.contoso.com ponto para o endereço de ip físico do corpservice
- passwordreset.contoso.com ponto para o endereço de ip físico do corpservice
- passwordregistration.contoso.com ponto para o endereço de ip físico do corpservice

> [!div class="step-by-step"]
> [Windows Server 2016»](prepare-server-ws2016.md)
