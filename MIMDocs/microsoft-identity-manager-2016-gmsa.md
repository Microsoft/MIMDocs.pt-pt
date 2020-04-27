---
title: Converter serviços específicos do Microsoft Identity Manager para gMSA [ Microsoft Docs
description: Este artigo apresenta os pré-requisitos e passos básicos para a configuração de uma Conta de Serviço Gerida por grupo (gMSA).
author: EugeneSergeev
ms.author: esergeev
manager: daveba
ms.date: 03/10/2020
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 4586b9998a9526a867ffe7ace9489fe56fff146c
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044213"
---
# <a name="convert-microsoft-identity-manager-specific-services-to-use-group-managed-service-accounts"></a>Converter serviços específicos do Microsoft Identity Manager para utilizar contas de serviço geridas pelo grupo

Este artigo é um guia para configurar serviços de Gestor de Identidade da Microsoft suportados para utilizar contas de serviço geridas pelo grupo (gMSA). Depois de ter reconfigurado o seu ambiente, o processo de conversão para gMSA é fácil.

## <a name="prerequisites"></a>Pré-requisitos

- Descarregue e instale o seguinte hotfixo necessário: [Microsoft Identity Manager 4.5.26.0 ou mais tarde](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history).

    Serviços apoiados:

    -   Serviço de Sincronização do Gestor de Identidade da Microsoft (FIMSynchronizationService)
    -   Serviço de Gestor de Identidade da Microsoft (FIMService)
    -   Registo de passwords do Gestor de Identidade da Microsoft
    -   Reset de palavra-passe do Gestor de Identidade da Microsoft
    -   Serviço de Monitorização de Acesso Privilegiado (PAM) (PamMonitoringService)
    -   Serviço de Componentes PAM (PrivilegeManagementComponentService)

    Serviços não apoiados:

    -   O portal Microsoft Identity Manager não é suportado. Faz parte do ambiente SharePoint, e precisaria de o implementar em modo de exploração e configurar a mudança automática de [palavra-passe no SharePoint Server](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change).
    -   Todos os agentes de gestão, exceto o agente de gestão do Serviço de Gestor de Identidade da Microsoft
    -   Gestão de Certificados da Microsoft
    -   BHOLD

- Para obter informações sobre o fundo e as informações gerais de referência sobre a configuração do seu ambiente, consulte: 

    -   [Visão geral das Contas de Serviço geridas pelo grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)  
    -   [New-AdServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps)  

- Antes de começar, [crie a chave de raiz](https://technet.microsoft.com/library/jj128430(v=ws.11).aspx) dos Serviços de Distribuição de Chaves no seu controlador de domínio Windows. Tenha em mente as seguintes informações:  

    - As chaves-raiz são utilizadas pelo serviço Key Distribution Services (KDS) para gerar palavras-passe e outras informações sobre controladores de domínio.
    - Crie uma chave de raiz apenas uma vez por domínio, se for necessário.  
    - Incluir `Add-KDSRootKey –EffectiveImmediately`. "-EficazImediatamente" significa que pode levar até 10 horas para replicar a chave de raiz de todos os controladores de domínio. Pode levar cerca de 1 hora para replicar dois controladores de domínio. 
    ![A corda "-Eficaz Imediatamente"](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="actions-to-run-on-the-active-directory-domain-controller"></a>Ações a executar no controlador de domínio de diretório ativo

1.  Crie um grupo chamado *MIMSync_Servers*e adicione todos os servidores de sincronização.

    ![Criar um grupo MIMSync_Servers](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

1.  Inscreva-se no Windows PowerShell como *um Administrador* de Domínio com uma conta que já está unida ao domínio e, em seguida, executar o seguinte comando: 

    `New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"`

    ![O comando na PowerShell](media/f6deb0664553e01bcc6b746314a11388.png)

    - Consulte os detalhes do gMSA para sincronizar:  
     ![Detalhes do gMSA para sincronização](media/c80b0a7ed11588b3fb93e6977b384be4.png)

    - Se estiver a executar o Serviço de Notificação de Alteração de Passwords (PCNS), atualize a delegação executando o seguinte comando:

        `Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames
        @{Add="PCNSCLNT/mimsync.contoso.com"}`

## <a name="actions-to-run-on-the-microsoft-identity-manager-synchronization-server"></a>Ações para executar no servidor de sincronização do Gestor de Identidade da Microsoft

1. No Gestor de Serviços de Sincronização, volte a fazer o backon da chave de encriptação. Será solicitado com a instalação do modo de alteração. Faça o seguinte:

    a. No servidor em que o Gestor de Serviços de Sincronização está instalado, procure a ferramenta de Gestão de Chaves do Serviço de Sincronização. O  **conjunto de chaves de exportação**já está selecionado por defeito.

    b. Selecione **Seguinte**. 
    
    c. No momento útil, insira e verifique a informação do Serviço de Sincronização do Microsoft Identity Manager ou do Gestor de Identidade da Linha da Frente (FIM):

    -   **Nome da conta**: O nome da conta do Serviço de Sincronização que é utilizada durante a instalação inicial.  
    -   **Palavra-passe**: A palavra-passe da conta do Serviço de Sincronização.  
    -   **Domínio**: O domínio do que a conta do Serviço de Sincronização faz parte.

    d. Selecione **Seguinte**.

    Se inseriu as informações da conta com sucesso, tem a opção de alterar o destino, ou a localização do ficheiro de exportação, da chave de encriptação de cópia de segurança. Por predefinição, a localização do ficheiro de exportação é *C:\Windows\system32\miiskeys-1.bin*.

1. Instale o Microsoft Identity Manager SP1, que pode encontrar no Centro de Serviços de Licenciamento de Volume ou no site de descarregamentos da MSDN. Depois de ter concluído a instalação, guarde o *teclado miiskeys.bin*.

   ![A janela de progresso do serviço de sincronização do Microsoft Identity Manager](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)

1. Instale [a correção de hotéis 4.5.2.6.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) ou posteriormente.

1. Depois de instalado o patch, pare o Serviço de Sincronização FIM fazendo o seguinte:

   a. No Painel de Controlo, selecione **Programas e Funcionalidades** > **Microsoft Identity Manager**.  
   b. Na página do Serviço de **Sincronização,** selecione **Change** > **Next**.  
   c. Na janela **Opções** de Manutenção, selecione **Configurar**.

   ![A janela Opções de Manutenção](media/dc98c011bec13a33b229a0e792b78404.png)

   d. Na janela do Serviço de Sincronização do Gestor de Identidade da **Microsoft, limpe** o valor predefinido na caixa **de conta de serviço** e introduza o **MIMSyncGMSA$**. Certifique-se de incluir o símbolo do sinal do dólar ($), como mostra a imagem seguinte. Deixe a caixa **de palavra-passe** vazia.

   ![A janela do Serviço de Sincronização do Gestor de Identidade da Microsoft](media/38df9369bf13e1c3066a49ed20e09041.png)

   e. Selecione **Next** > **Next** > **Install**.  
   f. Restaure o teclado do ficheiro *miiskeys.bin* que guardou anteriormente.

   ![Opção para restaurar a configuração do conjunto de chaves](media/44cd474323584feb6d8b48b80cfceb9b.png)

   ![A lista de Agentes de Gestão no Gestor de Serviços de Sincronização](media/03e7762f34750365e963f0b90e43717c.png)

## <a name="microsoft-identity-manager-service"></a>Serviço de Gestor de Identidade da Microsoft

>[!IMPORTANT]
>Siga cuidadosamente as instruções nesta secção quando converter contas relacionadas com o Serviço do Microsoft Identity Manager para contas gMSA.

1. Crie Contas Geridas pelo Grupo para o Serviço de Gestor de Identidade da Microsoft, o Pam Rest API, o Serviço de Monitorização PAM, o Serviço de Componentes PAM, o Portal de Registo de passwords de autosserviço (SSPR) e o Portal de Reset SSPR.

    -   Atualizar a delegação do GMSA e o nome principal do serviço (SPN):

        - `Set-ADServiceAccount -Identity \<account\> -ServicePrincipalNames
            @{Add="\<SPN\>"}`
    -   Delegação:

        - `Set-ADServiceAccount -Identity \<gmsaaccount\>
                -TrustedForDelegation $true`
    -   Delegação constrangida:
        -   `$delspns = 'http/mim', 'http/mim.contoso.com'`
        -   `New-ADServiceAccount -Name \<gmsaaccount\> -DNSHostName
                \<gmsaaccount\>.contoso.com
                -PrincipalsAllowedToRetrieveManagedPassword \<group\>
                -ServicePrincipalNames $spns -OtherAttributes
                @{'msDS-AllowedToDelegateTo'=$delspns }`

1. Adicione uma conta para o Serviço de Gestor de Identidade da Microsoft em Sync Groups. Este passo é necessário para a SSPR.

    ![A janela De Utilizadores e Computadores de Diretório Ativo](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

    > [!NOTE]  
    > Um problema conhecido no Windows Server 2012 R2 é que os serviços que utilizam uma conta gerida ficam pendurados após o reinício do servidor, porque o Microsoft Key Distribution Service não é iniciado após o reinício do Windows. A suver para esta questão é executar o seguinte comando: 
    >
    > `sc triggerinfo kdssvc start/networkon`
    >
    > O comando inicia o Microsoft Key Distribution Service quando a rede está ligado (normalmente no início do ciclo de arranque).
    >
    > Para uma discussão sobre um problema semelhante, consulte [AD FS Windows 2012 R2: adfssrv fica em modo de partida](https://social.technet.microsoft.com/Forums/en-US/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS).

1.  Executar MSI elevado do Serviço de Gestor de Identidade da Microsoft e selecionar **Alterar**.

1.  Na janela de ligação do servidor de **correio Configurar,** selecione a caixa de verificação de verificação do **utilizador para troca (para contas geridas).** É-lhe dada a opção de utilizar a conta de câmbio corrente ou a caixa de correio na nuvem.

    >[!NOTE]
    >Se selecionar a opção **Use Exchange Online,** para permitir ao Microsoft Identity Manager Service processar respostas de aprovação a partir do add-in do Microsoft Identity Manager Outlook, defina a chave de registo **HKLM\SYSTEM\CurrentControlSet\Services\FIMService** value of *PollExchangeEnabled* para **1** após a instalação.
    
    ![A janela "Configurar a ligação do servidor de correio"](media/0cd8ce521ed7945c43bef6100f8eb222.png)

1.  Na janela de conta de **serviço DAM,** na caixa de nome da **conta de serviço,** introduza o nome. Certifique-se de incluir o símbolo do sinal de dólar ($). Introduza também uma palavra-passe na caixa de **palavra-passe** da conta de email do serviço. A caixa **de palavra-passe da conta de serviço** não deve estar disponível.

    ![A janela "Configurar a conta de serviço MIM"](media/db0d543df6e1b0174a47135617c23fcb.png)

    Uma vez que a função LogonUser não funciona para contas geridas, a página seguinte apresenta o aviso: "Por favor, verifique se a Conta de Serviço está segura na sua configuração atual."

    ![Janela de aviso de segurança da conta](media/d350bc13751b2d0a884620db072ed019.png)

1.  Na janela **Configure Privileged Access Management REST API,** na caixa de nome da conta de piscina de **aplicação,** insira o nome da conta. Certifique-se de incluir o símbolo do sinal de dólar ($). Deixe a caixa **de palavra-passe da conta de piscina** de aplicação vazia.

    ![A janela API de Gestão de Acesso Privilegiado Configure](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

1.  Na janela de Serviço de **Componentes De Configuração PAM,** na caixa de Nome da **Conta de Serviço,** introduza o nome da conta. Certifique-se de incluir o símbolo do sinal de dólar ($). Deixe a caixa **de palavra-passe** da conta de serviço vazia.

    ![A janela de serviço de componentes de Configuração PAM](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

    ![A janela de aviso de segurança da conta](media/9d2b52f6faed10601e7e2166a339fb47.png)

1.  Na janela de Serviço de Monitorização de Gestão de **Acesso Privilegiado,** na caixa de Nome da **Conta de Serviço,** digite o nome da conta de serviço. Certifique-se de incluir o símbolo do sinal de dólar ($). Deixe a caixa **de palavra-passe** da conta de serviço vazia.

    ![A janela do Serviço de Monitorização de Acesso Privilegiado Configure](media/d1e824248edf12a77fc9ffb011475164.png)

1.  Na janela do Portal de Registo de **Passwords Da Configure MIM,** na caixa nome da **conta,** introduza o nome da conta. Certifique-se de incluir o símbolo do sinal de dólar ($). Deixe a caixa **de palavra-passe** vazia.

    ![A janela do portal de registo de passwords DAMI Configurar](media/601e935cdfda298b61ae753a2a152996.png)

1.  Na janela **'Reconfigurar mim Password Reset Portal',** na caixa nome de **conta,** introduza o nome da conta. Certifique-se de incluir o símbolo do sinal de dólar ($). Deixe a caixa **de palavra-passe** vazia.

    ![A janela do portal de reset de palavra-passe DAM Configurar](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

1.  Conclua a instalação.

    > [!NOTE]
    > Durante a instalação, duas novas teclas são criadas na rota de registo **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Forefront Identity Manager\2010\Service** para armazenar a senha de troca encriptada. Uma entrada é para *ExchangeOnline,* e a outra é para *ExchangeOnPremise*. Para uma das entradas, o valor na coluna **Dados** deve estar vazio.

    > ![O Editor de Registo](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

Para atualizar a palavra-passe para as suas contas armazenadas sem ter de executar o modo de alteração, [descarregue este script PowerShell](microsoft-identity-manager-2016-gmsascript.md).

Para encriptar a palavra-passe Do Exchange, o instalador cria um serviço adicional e executa-o sob a conta gerida. As seguintes mensagens são adicionadas no registo do evento **da Aplicação** durante a instalação:

![A janela do Espectador de Eventos](media/95b315454705cd4d939b55ac5ad910f5.jpg)
