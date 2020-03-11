---
title: Criar um gMSAs para o Microsoft Identity Manager 2016  Microsoft Docs
description: Configurar contas de serviço geridas pelo grupo num domínio para o Microsoft Identity Manager 2016
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: daveba
ms.date: 1/7/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: 32b346dd9cf99b617edfaca953389cba30d6681c
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043516"
---
# <a name="configure-a-domain-for-group-managed-service-accounts-gmsa-scenario"></a>Configure um domínio para contas de serviço geridas pelo grupo (gMSA)

> [!div class="step-by-step"]
> [Servidor windows »](prepare-server-ws2016.md)

> [!IMPORTANT]
> Este artigo aplica-se apenas ao MIM 2016 SP2.

O Microsoft Identity Manager (MIM) funciona com o seu domínio do Active Directory (AD). Já deve ter o AD instalado, e certifique-se de que tem um controlador de domínio no seu ambiente para um domínio que possa administrar.  Este artigo descreve como configurar contas de serviço geridas pelo grupo nesse domínio para utilização por MIM.

## <a name="overview"></a>Descrição geral

As Contas de Serviço Geridas pelo Grupo eliminam a necessidade de alterar periodicamente as palavras-passe da conta de serviço. Com o lançamento do MIM 2016 SP2, os seguintes componentes MIM podem ter contas gMSA configuradas para serem utilizadas durante o processo de instalação:

-   Serviço de Sincronização MIM (SERVIÇO DE Sincronização FIM)
-   Serviço MIM (SERVIÇO FIM)
-   Piscina de aplicação do site de registo de senha mim
-   Mim Password Reset site application pool
-   Pam REST API piscina de aplicação web site
-   Serviço de Monitorização PAM (PamMonitoringService)
-   Serviço de Componentes PAM (PrivilegeManagementComponentService)

Os seguintes componentes MIM não suportam o funcionamento como contas gMSA:

-   Portal MIM. Isto porque o PORTAL MIM faz parte do ambiente SharePoint. Em vez disso, pode implementar o SharePoint no modo de exploração e configurar a alteração automática da [palavra-passe no SharePoint Server](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change).
-   Todos os Agentes de Gestão
-   Gestão de Certificados da Microsoft
-   BHOLD


Mais informações sobre o gMSA podem ser encontradas nestes artigos:
-   [Visão geral das Contas de Serviço Geridas pelo Grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   [New-AdServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps)

-   [Criar a chave raiz dos serviços de distribuição kDS](https://technet.microsoft.com/library/jj128430(v=ws.11).aspx)

## <a name="create-user-accounts-and-groups"></a>Configurar contas de utilizador e grupos

Todos os componentes de implementação do MIM têm as suas próprias identidades no domínio. Isto inclui os componentes MIM como Serviço e Sincronização, bem como SharePoint e SQL.


> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio - **dc**
> - Nome de domínio – **contoso**
> - Nome do Servidor de Serviço MIM - **mimservice**
> - Nome do Servidor Mim Sync - **mimsync**
> - Nome do Servidor SQL - **sql**
> - Palavra-passe – <strong>Pass@word1</strong>

1. Inicie sessão no controlador de domínio como o administrador do domínio (*por exemplo, Contoso\Administrador*).

2. Crie as seguintes contas de utilizador para os serviços MIM. Iniciar o PowerShell e escrever o seguinte script PowerShell para criar novos utilizadores de domínio AD (nem todas as contas são obrigatórias, embora o script seja fornecido apenas para fins informísticos, é uma melhor prática usar uma conta *MIMAdmin* dedicada para o processo de instalação mime e sharePoint).

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
    setspn -S http/mim.contoso.com contoso\svcMIMAppPool
    ```

5.  Certifique-se de que regista os seguintes registos DNS 'A' para resolução de nomes adequados (assumindo que o Serviço MIM, o Portal MIM, o Reset de Passwords e os web sites de registo de passwords serão hospedados na mesma máquina)

    - mim.contoso.com - apontar para o endereço IP físico do serviço MIM e do servidor portal
    - passwordreset.contoso.com - apontar para o endereço IP físico do serviço MIM e do servidor portal
    - passwordregistration.contoso.com - apontar para o endereço IP físico do serviço MIM e do servidor portal

## <a name="create-key-distribution-service-root-key"></a>Criar chave raiz de serviço de distribuição chave

Certifique-se de que está inscrito no seu controlador de domínio como administrador para preparar o serviço de distribuição de chaves do grupo.

Se já existe uma chave-raiz para o domínio (use **Get-KdsRootKey** para verificar), então continue para a secção seguinte.

6.  Crie a chave de raiz dos Serviços de Distribuição de Chaves (KDS) (apenas uma vez por domínio) se necessário. A Chave Raiz é utilizada pelo serviço KDS em controladores de domínio (juntamente com outras informações) para gerar palavras-passe. Como administrador de domínio, digite o seguinte comando PowerShell:

    ```PowerShell
    Add-KDSRootKey –EffectiveImmediately
    ```
    *–EffectiveImmediately* pode exigir um atraso de até \~10 horas, uma vez que terá de se replicar a todos os controladores de domínio. Este atraso foi de aproximadamente 1 hora para dois controladores de domínio.

    ![](media/7fbdf01a847ea0e330feeaf062e30668.png)

    >[!NOTE]
    >No ambiente lab ou teste pode evitar 10 horas de atraso de replicação executando o seguinte comando em vez disso:
    ><br/>
    >Add-KDsrootkey -effectivetime ((data-tacada). AddHours (-10))

## <a name="create-mim-synchronization-service-account-group-and-service-principal"></a>Criar conta de Serviço de Sincronização MIM, grupo e diretor de serviço
-----------------------

Certifique-se de que todas as contas do computador para computadores onde o software MIM está a ser instalado já estão unidas ao domínio.  Em seguida, execute estes passos no PowerShell como administrador de domínio.

7.  Crie um *grupo MIMSync_Servers* e adicione todos os servidores de Sincronização MIM a este grupo.
    Digite o seguinte para criar um novo grupo de AD para servidores de sincronização MIM. Em seguida, adicione as contas de computador Ative Directory do servidor de sincronização MIM, por exemplo, *contoso\MIMSync$,* neste grupo.

    ```PowerShell
    New-ADGroup –name MIMSync_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMSync_Servers
    Add-ADGroupmember -identity MIMSync_Servers -Members MIMSync$
    ```

8.  Crie o Serviço de Sincronização mim gMSA. Digite o seguinte PowerShell.

    ```PowerShell
    New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"
    ```

    Verifique os detalhes do GSMA criado pela execução do comando *Get-ADServiceAccount* PowerShell:

    ![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

9.  Se planeia executar o Serviço de Notificação de Alteração de Passwords, tem de registar o Nome Principal do Serviço executando este comando PowerShell:

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames @{Add="PCNSCLNT/mimsync.contoso.com"}
    ```

10. Reinicie o seu servidor DE Sincronização MIM para atualizar um símbolo kerberos associado ao servidor à medida que a associação do grupo "MIMSync_Server" mudou.

## <a name="create-mim-service-management-agent-service-account"></a>Criar a conta de serviço do Agente de Gestão de Serviços MIM

11. Normalmente, ao instalar o Serviço MIM, irá criar uma nova conta para o Agente de Gestão de Serviços MIM (conta MIM MA).  Com o gMSA, existem duas opções disponíveis:

- Utilize a conta de serviço gerida pelo grupo MIM Synchronization Service e não crie uma conta separada

    Pode ignorar a criação da conta de serviço do Agente de Gestão de Serviços MIM. Neste caso, utilize o nome gMSA do Serviço de Sincronização MIM, por exemplo, *contoso\MIMSyncGMSAsvc$,* em vez da conta MIM MA ao instalar o Serviço MIM. Mais tarde, na configuração do Agente de Gestão de Serviço mim a activaa a opção *"Use mimSync Account".*

    Não ative 'Deny Logon from Network' para o Serviço de Sincronização MIM gMSA, uma vez que a conta MIM MA requer permissão de 'Permitir logon de rede'.

- Utilize uma conta de serviço regular para a conta de serviço do Agente de Gestão de Serviços MIM

    Inicie o PowerShell como administrador de domínio e escreva o seguinte para criar um novo utilizador de domínio AD:

    ```PowerShell
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    
    New-ADUser –SamAccountName svcMIMMA –name svcMIMMA
    Set-ADAccountPassword –identity svcMIMMA –NewPassword $sp
    Set-ADUser –identity svcMIMMA –Enabled 1 –PasswordNeverExpires 1
    ```

    Não ative 'Deny Logon from Network' para a conta MIM MA, uma vez que requer permissão de logon de rede "Permitir a rede".

## <a name="create-mim-service-accounts-groups-and-service-principal"></a>Criar contas de serviço mim, grupos e diretor de serviço

Continue a usar o PowerShell como administrador de domínio.
   
12. Crie um *grupo MIMService_Servers* e adicione todos os servidores do Serviço MIM a este grupo.  Digite o seguinte PowerShell para criar um novo grupo de AD para servidores de serviço MIM e adicionar conta de computador Ative Directory do servidor de serviço MIM, por exemplo, *contoso\MIMPortal$,* neste grupo.

    ```PowerShell
    New-ADGroup –name MIMService_Servers –GroupCategory Security –GroupScope Global –SamAccountName MIMService_Servers
    Add-ADGroupMember -identity MIMService_Servers -Members MIMPortal$
    ```

1.  Crie o Serviço MIM gMSA.

    ```PowerShell
    New-ADServiceAccount -Name MIMSrvGMSAsvc -DNSHostName MIMSrvGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMService_Servers" -OtherAttributes @{'msDS-AllowedToDelegateTo'='FIMService/mimportal.contoso.com'} 
    ```

1.  Registre o nome principal do serviço e permita a delegação kerberos executando este comando PowerShell:

    ```PowerShell
    Set-ADServiceAccount -Identity MIMSrvGMSAsvc -TrustedForDelegation $true -ServicePrincipalNames @{Add="FIMService/mimportal.contoso.com"}
    ```

1.  Para os cenários sSPR, precisa de conta de serviço MIM ser capaz de comunicar com o Serviço de Sincronização MIM, pelo que a conta de serviço MIM deve ser um membro dos MIMSyncAdministrators ou dos grupos de reset e navegação de senha sincronia MIM:

    ```PowerShell
    Add-ADGroupmember -identity MIMSyncPasswordSet -Members MIMSrvGMSAsvc$ 
    Add-ADGroupmember -identity MIMSyncBrowse -Members MIMSrvGMSAsvc$ 
    ```

16. Reinicie o seu servidor de serviço MIM para atualizar um token Kerberos associado ao servidor à medida que a associação do grupo "MIMService_Servers" mudou.

## <a name="create-other-mim-accounts-and-groups-if-needed"></a>Criar outras contas e grupos MIM, se necessário

Se estiver a configurar mim SSPR, seguindo as mesmas diretrizes acima descritas para o Serviço de Sincronização MIM e o Serviço MIM, pode criar outro gMSA para:

- Mim Password Reset site application pool
- Piscina de aplicação do site de registo de senha mim

Se estiver a configurar mim PAM, seguindo as mesmas diretrizes acima descritas para o Serviço de Sincronização MIM e serviço MIM, pode criar outro gMSA para:

- Mim PAM REST API piscina de aplicação web site
- Serviço de Componente MIM PAM
- Serviço de Monitorização MIM PAM

## <a name="specifying-a-gmsa-when-installing-mim"></a>Especificar um gMSA ao instalar mim

Regra geral, na maioria dos casos ao utilizar um instalador MIM, para especificar que pretende utilizar um gMSA em vez de uma conta regular, anexar um personagem de sinal de dólar ao nome gMSA, por exemplo, **contoso\MIMSyncGMSAsvc$,** e deixar o campo de palavra-passe vazio. Uma exceção é a ferramenta *miisactivate.exe* que aceita o nome gMSA sem o sinal de dólar.
<br/>

> [!div class="step-by-step"]
> [Servidor windows »](prepare-server-ws2016.md)
