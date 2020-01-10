---
title: Configurar um gMSAs para o Microsoft Identity Manager 2016 | Microsoft Docs
description: Configurar contas de serviço gerenciado de grupo em um domínio para Microsoft Identity Manager 2016
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 1/7/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: a74f4074d9a0cf8378fd4972b7f51f723bd2f1c6
ms.sourcegitcommit: 80cdfd782cc6e2a4c4698decd54342f0e1460f5f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2020
ms.locfileid: "75756275"
---
# <a name="configure-a-domain-for-group-managed-service-accounts-gmsa-scenario"></a>Configurar um domínio para o cenário de grupo de contas de serviço gerenciado (gMSA)

> [!div class="step-by-step"]
> [Windows Server»](prepare-server-ws2016.md)

> [!IMPORTANT]
> Este artigo se aplica somente ao MIM 2016 SP2.

O Microsoft Identity Manager (MIM) funciona com o seu domínio do Active Directory (AD). Já deve ter o AD instalado, e certifique-se de que tem um controlador de domínio no seu ambiente para um domínio que possa administrar.  Este artigo descreve como configurar contas de serviço gerenciado de grupo nesse domínio para uso pelo MIM.

## <a name="overview"></a>Overview

As contas de serviço gerenciado de grupo eliminam a necessidade de alterar periodicamente as senhas de conta de serviço. Com o lançamento do MIM 2016 SP2, os seguintes componentes do MIM podem ter contas gMSA configuradas para serem usadas durante o processo de instalação:

-   Serviço de sincronização do MIM (FIMSynchronizationService)
-   Serviço do MIM (FIMService)
-   Pool de aplicativos do site de registro de senha do MIM
-   Pool de aplicativos do site da Web de redefinição de senha do MIM
-   Pool de aplicativos do site da API REST do PAM
-   Serviço de monitoramento do PAM (PamMonitoringService)
-   Serviço de componente do PAM (PrivilegeManagementComponentService)

Os seguintes componentes do MIM não oferecem suporte à execução como contas do gMSA:

-   Portal do MIM. Isso ocorre porque o portal do MIM faz parte do ambiente do SharePoint. Em vez disso, você pode implantar o SharePoint no modo de farm e [Configurar a alteração automática de senha no SharePoint Server](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change).
-   Todos os agentes de gerenciamento
-   Gerenciamento de certificados da Microsoft
-   BHOLD


Mais informações sobre gMSA podem ser encontradas nestes artigos:
-   [Visão geral de contas de serviço gerenciado de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   [New-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps)

-   [Criar a chave raiz KDS dos serviços de distribuição de chaves](https://technet.microsoft.com/library/jj128430(v=ws.11).aspx)

## <a name="create-user-accounts-and-groups"></a>Configurar contas de utilizador e grupos

Todos os componentes de implementação do MIM têm as suas próprias identidades no domínio. Isto inclui os componentes MIM como Serviço e Sincronização, bem como SharePoint e SQL.


> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio- **DC**
> - Nome de domínio – **contoso**
> - Nome do servidor do serviço do MIM- **mimservice**
> - Nome do servidor de sincronização do MIM- **mimsync**
> - Nome do SQL Server- **SQL**
> - Palavra-passe – <strong>Pass@word1</strong>

1. Inicie sessão no controlador de domínio como o administrador do domínio (*por exemplo, Contoso\Administrador*).

2. Crie as seguintes contas de utilizador para os serviços MIM. Inicie o PowerShell e digite o seguinte script do PowerShell para criar novos usuários de domínio do AD (nem todas as contas são obrigatórias, embora o script seja fornecido apenas para fins informativos, é uma prática recomendada usar uma conta *MIMAdmin* dedicada para o mim e o processo de instalação do SharePoint).

    ```PowerShell
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

    New-ADUser –SamAccountName MIMAdmin –name MIMAdmin
    Set-ADAccountPassword –identity MIMAdmin –NewPassword $sp
    Set-ADUser –identity MIMAdmin –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcSharePoint –name svcSharePoint
    Set-ADAccountPassword –identity svcSharePoint –NewPassword $sp
    Set-ADUser –identity svcSharePoint –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcMIMSql –name svcMIMSql
    Set-ADAccountPassword –identity svcMIMSql –NewPassword $sp
    Set-ADUser –identity svcMIMSql –Enabled 1 –PasswordNeverExpires 1

    New-ADUser –SamAccountName svcMIMAppPool –name svcMIMAppPool
    Set-ADAccountPassword –identity svcMIMAppPool –NewPassword $sp
    Set-ADUser –identity svcMIMAppPool –Enabled 1 -PasswordNeverExpires 1
    ```

3.  Crie grupos de segurança para todos os grupos.

    ```PowerShell
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordSet –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordSet
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupMember -identity MIMSyncAdmins -Members MIMAdmin
    ```

4.  Adicionar SPNs para ativar a autenticação Kerberos nas contas de serviço

    ```PowerShell
    Set-ADServiceAccount -Identity svcMIMAppPool -ServicePrincipalNames @{Add="http/mim.contoso.com"}
    ```

5.  Certifique-se de registrar os seguintes registros de ' A ' DNS para a resolução de nomes apropriada (supondo que o serviço MIM, o portal do MIM, a redefinição de senha e os sites de registro de senha serão hospedados no mesmo computador)

    - mim.contoso.com-apontar para o serviço do MIM e o endereço IP físico do servidor do portal
    - passwordreset.contoso.com-apontar para o serviço do MIM e o endereço IP físico do servidor do portal
    - passwordregistration.contoso.com-apontar para o serviço do MIM e o endereço IP físico do servidor do portal

## <a name="create-key-distribution-service-root-key"></a>Criar chave raiz do serviço de distribuição de chaves

Verifique se você está conectado ao controlador de domínio como administrador para preparar o serviço de distribuição de chave de grupo.

Se já houver uma chave raiz para o domínio (use **Get-KdsRootKey** para verificar), continue na próxima seção.

6.  Crie a chave raiz do KDS (serviços de distribuição de chaves) (somente uma vez por domínio), se necessário. A chave raiz é usada pelo serviço KDS em controladores de domínio (juntamente com outras informações) para gerar senhas. Como administrador de domínio, digite o seguinte comando do PowerShell:

    ```PowerShell
    Add-KDSRootKey –EffectiveImmediately
    ```
    *– O EffectiveImmediately* pode exigir um atraso de até \~10 horas, pois será necessário replicar para todos os controladores de domínio. Esse atraso foi de aproximadamente 1 hora para dois controladores de domínio.

    ![](media/7fbdf01a847ea0e330feeaf062e30668.png)

    >[!NOTE]
    >No laboratório ou no ambiente de teste, você pode evitar o atraso de replicação de 10 horas executando o seguinte comando em vez disso:
    ><br/>
    >Add-KDSRootKey-efetivo (Get-Date). AddHours (-10))

## <a name="create-mim-synchronization-service-account-group-and-service-principal"></a>Criar conta de serviço de sincronização do MIM, grupo e entidade de serviço
-----------------------

Verifique se todas as contas de computador de computadores em que o software MIM deve ser instalado já estão associadas ao domínio.  Em seguida, execute estas etapas no PowerShell como um administrador de domínio.

7.  Crie um grupo *MIMSync_Servers* e adicione todos os servidores de sincronização do mim a esse grupo.
    Digite o seguinte para criar um novo grupo do AD para servidores de sincronização do MIM. Em seguida, o Add servidor de sincronização do MIM Active Directory contas de computador, por exemplo, *contoso\MIMSync $* , nesse grupo.

    ```PowerShell
    New-ADGroup –name MIMSync_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMSync_Servers
    Add-ADGroupmember -identity MIMSync_Servers -Members MIMSync$
    ```

8.  Crie o serviço de sincronização do MIM gMSA. Digite o PowerShell a seguir.

    ```PowerShell
    New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"
    ```

    Verifique os detalhes do GSMA criado ao executar o comando *Get-ADServiceAccount* do PowerShell:

    ![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

9.  Se você planeja executar o serviço de notificação de alteração de senha, você precisa registrar o nome da entidade de serviço executando este comando do PowerShell:

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames @{Add="PCNSCLNT/mimsync.contoso.com"}
    ```

10. Reinicialize o servidor de sincronização do MIM para atualizar um token Kerberos associado ao servidor, pois a associação ao grupo "MIMSync_Server" foi alterada.

## <a name="create-mim-service-management-agent-service-account"></a>Criar conta de serviço do agente de gerenciamento de serviço do MIM

11. Normalmente, ao instalar o serviço do MIM, você criará uma nova conta para o agente de gerenciamento de serviço do MIM (conta do MIM MA).  Com o gMSA, há duas opções disponíveis:

- Usar a conta de serviço gerenciado do grupo de serviço de sincronização do MIM e não criar uma conta separada

    Você pode ignorar a criação da conta de serviço do agente de gerenciamento do serviço do MIM. Nesse caso, use o gMSA nome do serviço de sincronização do MIM, por exemplo, *contoso\MIMSyncGMSAsvc $* , em vez da conta do mim ma ao instalar o serviço do mim. Posteriormente, na configuração do agente de gerenciamento de serviço do MIM, habilite a opção *' usar conta de MIMSync '* .

    Não habilite ' Negar logon da rede ' para o serviço de sincronização do MIM gMSA como a conta do MIM MA requer a permissão ' permitir logon na rede '.

- Usar uma conta de serviço regular para a conta de serviço do agente de gerenciamento de serviço do MIM

    Inicie o PowerShell como administrador de domínio e digite o seguinte para criar um novo usuário de domínio do AD:

    ```PowerShell
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    
    New-ADUser –SamAccountName svcMIMMA –name svcMIMMA
    Set-ADAccountPassword –identity svcMIMMA –NewPassword $sp
    Set-ADUser –identity svcMIMMA –Enabled 1 –PasswordNeverExpires 1
    ```

    Não habilite ' Negar logon da rede ' para a conta do MIM MA, pois ela requer a permissão ' permitir logon na rede '.

## <a name="create-mim-service-accounts-groups-and-service-principal"></a>Criar contas de serviço, grupos e entidade de serviço do MIM

Continue usando o PowerShell como um administrador de domínio.
   
12. Crie um grupo *MIMService_Servers* e adicione todos os servidores de serviço do mim a esse grupo.  Digite o PowerShell a seguir para criar um novo grupo do AD para servidores de serviço do MIM e adicionar o servidor de serviço do MIM Active Directory conta de computador, por exemplo, *contoso\MIMPortal $* , a esse grupo.

    ```PowerShell
    New-ADGroup –name MIMService_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMService_Servers
    Add-ADGroupMember -identity MIMService_Servers -Members MIMPortal$
    ```

1.  Crie o serviço do MIM gMSA.

    ```PowerShell
    New-ADServiceAccount -Name MIMSrvGMSAsvc -DNSHostName MIMSrvGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMService_Servers" -OtherAttributes @{'msDS-AllowedToDelegateTo'='FIMService/mimportal.contoso.com'} 
    ```

1.  Registre o nome da entidade de serviço e habilite a delegação Kerberos executando este comando do PowerShell:

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSrvGMSAsvc -TrustedForDelegation $true -ServicePrincipalNames @{Add="FIMService/mimportal.contoso.com"}
    ```

1.  Para cenários de SSPR, você precisa que a conta de serviço do MIM seja capaz de se comunicar com o serviço de sincronização do MIM, portanto, a conta de serviço do MIM deve ser membro do MIMSyncAdministrators ou do MIM:

    ```PowerShell
    Add-ADGroupmember -identity MIMSyncPasswordSet -Members MIMSrvGMSAsvc$ 
    Add-ADGroupmember -identity MIMSyncBrowse -Members MIMSrvGMSAsvc$ 
    ```

16. Reinicialize o servidor do serviço do MIM para atualizar um token Kerberos associado ao servidor, pois a associação ao grupo "MIMService_Servers" foi alterada.

## <a name="create-other-mim-accounts-and-groups-if-needed"></a>Crie outras contas e grupos do MIM, se necessário

Se você estiver configurando o MIM SSPR, seguindo as mesmas diretrizes descritas acima para o serviço de sincronização do MIM e o serviço do MIM, você poderá criar outros gMSA para:

- Pool de aplicativos do site da Web de redefinição de senha do MIM
- Pool de aplicativos do site de registro de senha do MIM

Se você estiver configurando o PAM do MIM, seguindo as mesmas diretrizes descritas acima para o serviço de sincronização do MIM e o serviço do MIM, você pode criar outros gMSA para:

- Pool de aplicativos do site da API REST do PAM do MIM
- Serviço de componente do PAM do MIM
- Serviço de monitoramento do PAM do MIM

## <a name="specifying-a-gmsa-when-installing-mim"></a>Especificando um gMSA ao instalar o MIM

Como regra geral, na maioria dos casos, ao usar um instalador do MIM, para especificar que você deseja usar um gMSA em vez de uma conta normal, acrescente um caractere de cifrão ao nome gMSA, por exemplo, **contoso\MIMSyncGMSAsvc $** , e deixe o campo de senha vazio. Uma exceção é a ferramenta *miisactivate. exe* que aceita o nome do gMSA sem o cifrão.
<br/>

> [!div class="step-by-step"]
> [Windows Server»](prepare-server-ws2016.md)
