---
title: Converter serviços específicos do Microsoft Identity Manager para o gMSA Microsoft Docs
description: Este artigo apresenta os pré-requisitos e os passos básicos para configurar uma conta de serviço gerido em grupo (gMSA).
author: EugeneSergeev
ms.author: esergeev
manager: daveba
ms.date: 03/10/2020
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 5985ded45a53a804728572404fb0db43e988ac1d
ms.sourcegitcommit: f87be3d09cee6a8880b3a6babf32e0d064fde36b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2020
ms.locfileid: "87176766"
---
# <a name="convert-microsoft-identity-manager-specific-services-to-use-group-managed-service-accounts"></a>Converter serviços específicos do Gestor de Identidade da Microsoft para utilizar contas de serviço geridas pelo grupo

Este artigo é um guia para configurar os serviços suportados do Microsoft Identity Manager para utilizar contas de serviço geridas do grupo (gMSA). Depois de configurar o seu ambiente, o processo de conversão para gMSA é fácil.

## <a name="prerequisites"></a>Pré-requisitos

- Faça o download e instale o seguinte hotfix necessário: [Microsoft Identity Manager 4.5.26.0 ou mais tarde](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history).

    Serviços apoiados:

    -   Serviço de Sincronização do Gestor de Identidade da Microsoft (FIMSynchronizationService)
    -   Serviço de Gestor de Identidade da Microsoft (FIMService)
    -   Registo de senha do Gestor de Identidade da Microsoft
    -   Reset da palavra-passe do Gestor de Identidade do Microsoft
    -   Serviço privilegiado de Gestão do Acesso (PAM) (PamMonitoringService)
    -   Serviço de Componentes PAM (PrivilegeManagementComponentService)

    Serviços não apoiados:

    -   O portal Microsoft Identity Manager não é suportado. Faz parte do ambiente SharePoint, e você precisaria implantá-lo em modo fazenda e [configurar a mudança automática de senha no SharePoint Server](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change).
    -   Todos os agentes de gestão, exceto o agente de gestão do Microsoft Identity Manager Service
    -   Gestão de Certificados da Microsoft
    -   BHOLD

- Para obter informações de antecedentes e referências gerais sobre a configuração do seu ambiente, consulte: 

    -   [Visão geral das contas de serviço geridas pelo grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)  
    -   [New-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps)  

- Antes de começar, [crie a chave raiz dos Serviços de Distribuição chave](https://technet.microsoft.com/library/jj128430(v=ws.11).aspx) no seu controlador de domínio Windows. Tenha em mente as seguintes informações:  

    - As teclas de raiz são utilizadas pelo serviço Key Distribution Services (KDS) para gerar palavras-passe e outras informações sobre controladores de domínio.
    - Crie uma chave de raiz apenas uma vez por domínio, se for necessário.  
    - `Add-KDSRootKey –EffectiveImmediately`Incluir. "-EficazImmediatemente" significa que pode levar até 10 horas para replicar a chave raiz de todos os controladores de domínio. Pode levar cerca de 1 hora para replicar em dois controladores de domínio. 
    ![A cadeia "-EficazImmediate"](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="actions-to-run-on-the-active-directory-domain-controller"></a>Ações a executar no controlador de domínio do diretório ativo

1.  Crie um grupo chamado *MIMSync_Servers*e adicione todos os servidores de sincronização.

    ![Criar um grupo MIMSync_Servers](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

1.  Inscreva-se no Windows PowerShell como um Administrador de *Domínio* com uma conta que já se juntou ao domínio e, em seguida, execute o seguinte comando: 

    `New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"`

    ![O comando em PowerShell](media/f6deb0664553e01bcc6b746314a11388.png)

    - Ver detalhes do gMSA para sincronização:  
     ![Detalhes do gMSA para sincronização](media/c80b0a7ed11588b3fb93e6977b384be4.png)

    - Se estiver a executar o Serviço de Notificação de Alteração de Password (PCNS), atualize a delegação executando o seguinte comando:

        `Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipalNames
        @{Add="PCNSCLNT/mimsync.contoso.com"}`

## <a name="actions-to-run-on-the-microsoft-identity-manager-synchronization-server"></a>Ações a executar no servidor de sincronização do Gestor de Identidade da Microsoft

1. No Gestor de Serviço de Sincronização, reloque a chave de encriptação. Será solicitado com a instalação do modo de alteração. Faça o seguinte:

    a. No servidor onde está instalado o Gestor de Serviços de Sincronização, procure a ferramenta de Gestão de Chaves de Serviço de Sincronização. O **conjunto de teclas exportação**   já está selecionado por padrão.

    b. Selecione **Seguinte**. 
    
    c. No momento, insira e verifique as informações do Serviço de Sincronização do Gestor de Identidade da Microsoft ou do Gestor de Identidade (FIM) do Serviço de Sincronização:

    -   **Nome da conta**: O nome da conta serviço de sincronização que é utilizada durante a instalação inicial.  
    -   **Palavra-passe**: A palavra-passe da conta serviço de sincronização.  
    -   **Domínio**: O domínio do qual a conta do Serviço de Sincronização faz parte.

    d. Selecione **Seguinte**.

    Se tiver introduzido as informações da conta com sucesso, tem a opção de alterar o destino, ou exportar a localização do ficheiro, da chave de encriptação de backup. Por predefinição, a localização do ficheiro de exportação é *C:\Windows\system32\miiskeys-1.bin*.

1. Instale o Microsoft Identity Manager SP1, que pode encontrar no Centro de Serviços de Licenciamento de Volume ou no site de Downloads MSDN. Depois de concluída a instalação, guarde o tecla *miiskeys.bin*.

   ![A janela de progresso do Serviço de Sincronização do Gestor de Identidade da Microsoft](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)

1. Instale [o hotfix 4.5.2.6.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) ou mais tarde.

1. Depois de instalado o patch, pare o Serviço de Sincronização FIM fazendo o seguinte:

   a. No Painel de Controlo, selecione **Programas e Funcionalidades**  >  **Microsoft Identity Manager**.  
   b. Na página **'Serviço de Sincronização',** selecione **Change**  >  **Next**.  
   c. Na janela **Opções de Manutenção,** selecione **Configure**.

   ![Janela opções de manutenção](media/dc98c011bec13a33b229a0e792b78404.png)

   d. Na janela do Serviço de **Sincronização do Gestor de Identidade** do Microsoft, limpe o valor predefinido na caixa de conta de **Serviço** e introduza **MIMSyncGMSA$**. Certifique-se de incluir o símbolo do sinal de dólar ($), como mostra a seguinte imagem. Deixe a caixa **de senha** vazia.

   ![A janela do Serviço de Sincronização do Gestor de Identidade do Microsoft Configure](media/38df9369bf13e1c3066a49ed20e09041.png)

   e. Selecione **Next**  >  **a próxima**  >  **instalação.**  
   f. Restaurar o teclas do ficheiro *miiskeys.bin* que guardou anteriormente.

   ![Opção para restaurar a configuração do conjunto de chaves](media/44cd474323584feb6d8b48b80cfceb9b.png)

   ![A lista de agentes de gestão no Gestor de Serviços de Sincronização](media/03e7762f34750365e963f0b90e43717c.png)

## <a name="microsoft-identity-manager-service"></a>Serviço de Gestor de Identidade da Microsoft

>[!IMPORTANT]
>Siga cuidadosamente as instruções desta secção quando converter contas relacionadas com o Microsoft Identity Manager em contas gMSA.

1. Criar Contas Geridas pelo Grupo para o Serviço de Gestor de Identidade da Microsoft, o PAM Rest API, o Serviço de Monitorização PAM, o Serviço de Componentes PAM, o Portal de Registo de Autosserviço (SSPR) e o Portal de Reset SSPR.

    -   Atualizar a delegação gMSA e o nome principal do serviço (SPN):

        - `Set-ADServiceAccount -Identity \<account\> -ServicePrincipalNames
            @{Add="\<SPN\>"}`
    -   Delegação:

        - `Set-ADServiceAccount -Identity \<gmsaaccount\>
                -TrustedForDelegation $true`
    -   Delegação restrita:
        -   `$delspns = 'http/mim', 'http/mim.contoso.com'`
        -   `New-ADServiceAccount -Name \<gmsaaccount\> -DNSHostName
                \<gmsaaccount\>.contoso.com
                -PrincipalsAllowedToRetrieveManagedPassword \<group\>
                -ServicePrincipalNames $spns -OtherAttributes
                @{'msDS-AllowedToDelegateTo'=$delspns }`

1. Adicione uma conta para o Serviço de Gestor de Identidade da Microsoft em Grupos de Sincronização. Este passo é necessário para a SSPR.

    ![A janela ative directory users and computers](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

    > [!NOTE]  
    > Um problema conhecido no Windows Server 2012 R2 é que os serviços que utilizam uma conta gerida deixam de responder após o início do servidor, porque o Microsoft Key Distribution Service não é iniciado após o reinício do Windows. A solução para esta questão é executar o seguinte comando: 
    >
    > `sc triggerinfo kdssvc start/networkon`
    >
    > O comando inicia o Microsoft Key Distribution Service quando a rede está acesa (normalmente no início do ciclo de arranque).
    >
    > Para uma discussão sobre um problema semelhante, consulte [O FS FS Windows 2012 R2: adfssrv está pendurada no modo de partida](https://social.technet.microsoft.com/Forums/en-US/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS).

1.  Executar MSI elevado do Microsoft Identity Manager Service e selecione **Change**.

1.  Na janela **de ligação do servidor de correio eletrónico Configure,** selecione a caixa de verificação **Utilizar o utilizador diferente para Troca (para contas geridas).** É-lhe dada a opção de utilizar a conta 'Troca de Corrente' ou a caixa de correio em nuvem.

    >[!NOTE]
    >Se selecionar a opção **Use Exchange Online,** para permitir que o Serviço de Gestores de Identidade do Microsoft processe respostas de aprovação a partir do add-in do Microsoft Identity Manager Outlook, defina a chave de registo **HKLM\SYSTEM\CurrentControlSet\Services\FIMService** value of *PollExchangeEnabled* para **1** após a instalação.
    
    ![A janela "Configurar a ligação do servidor de correio"](media/0cd8ce521ed7945c43bef6100f8eb222.png)

1.  Na **configuração da** janela da conta de serviço MIM, na caixa **'Nome de Conta de Serviço',** insira o nome. Certifique-se de incluir o símbolo do sinal de dólar ($). Introduza também uma palavra-passe na caixa **de senha de conta de e-mail de serviço.** A caixa **de senha de conta** de serviço deve não estar disponível.

    ![A janela "Configurar a conta de serviço MIM"](media/db0d543df6e1b0174a47135617c23fcb.png)

    Como a função LogonUser não funciona para contas geridas, a página seguinte apresenta o aviso: "Por favor, verifique se a Conta de Serviço está segura na sua configuração atual."

    ![Janela de aviso de segurança da conta](media/d350bc13751b2d0a884620db072ed019.png)

1.  Na janela **Configure Privileged Access Management REST API,** na caixa **'Nome da Conta De Grupo de Aplicação',** insira o nome da conta. Certifique-se de incluir o símbolo do sinal de dólar ($). Deixe a caixa **de senha de conta de conta de grupo de aplicação** vazia.

    ![A janela Configure Privileged Access Management REST API](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

1.  Na janela **Configure O Serviço de Componentes PAM,** na caixa **'Nome de Conta de Serviço',** insira o nome da conta. Certifique-se de incluir o símbolo do sinal de dólar ($). Deixe a caixa **de senha de conta de serviço** vazia.

    ![A janela de serviço de componente configure PAM](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

    ![A janela de aviso de segurança da conta](media/9d2b52f6faed10601e7e2166a339fb47.png)

1.  Na janela **Configure a janela do Serviço de Monitorização da Gestão** de Acesso Privilegiado, na caixa Nome da **Conta de Serviço,** digite o nome da conta de serviço. Certifique-se de incluir o símbolo do sinal de dólar ($). Deixe a caixa **de senha de conta de serviço** vazia.

    ![A janela do Serviço de Monitorização de Gestão de Acesso Privilegiado Configure](media/d1e824248edf12a77fc9ffb011475164.png)

1.  Na janela do **Portal do Registo de Passwords MIM Configurar,** na caixa Nome de **Conta,** insira o nome da conta. Certifique-se de incluir o símbolo do sinal de dólar ($). Deixe a caixa **de senha** vazia.

    ![A janela do Portal de Registo de Passwords MIM Configurar](media/601e935cdfda298b61ae753a2a152996.png)

1.  Na janela **Configure PASSWORD Reset Portal,** na caixa **Nome de Conta,** insira o nome da conta. Certifique-se de incluir o símbolo do sinal de dólar ($). Deixe a caixa **de senha** vazia.

    ![A janela do Portal reset da palavra-passe MIM Configure](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

1.  Conclua a instalação.

    > [!NOTE]
    > Durante a instalação, são criadas duas novas teclas no caminho do registo **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft\Front Identity Manager\2010\Serviço** para armazenar a palavra-passe de Troca encriptada. Uma entrada é para *ExchangeOnline,* e a outra é para *ExchangeOnPremise*. Para uma das entradas, o valor na coluna **Data** deve estar vazio.

    > ![O Editor de Registos](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

Para atualizar a palavra-passe nas suas contas armazenadas sem ter de executar o modo de alteração, [descarregue este script PowerShell](microsoft-identity-manager-2016-gmsascript.md).

Para encriptar a palavra-passe De Troca, o instalador cria um serviço adicional e executa-o sob a conta gerida. As seguintes mensagens são adicionadas no registo do evento **da Aplicação** durante a instalação:

![A janela do Espectador de Eventos](media/95b315454705cd4d939b55ac5ad910f5.jpg)
