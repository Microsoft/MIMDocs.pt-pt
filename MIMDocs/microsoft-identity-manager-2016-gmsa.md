---
title: Conversão de Serviços Específicos mim para gMSA Microsoft Docs
description: Tópico descrevendo os passos básicos para configurar gMSA.
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 06/27/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 49216a2d2077dd1be83f17719e996a20abb61cf8
ms.sourcegitcommit: d98a76d933d4d7ecb02c72c30d57abe3e7f5d015
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78289501"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>Conversão de Serviços Específicos mim para gMSA

Este Guia passará pelos passos básicos para configurar o gMSA para serviços suportados. O processo de conversão para gMSA é fácil uma vez que pré-configura o seu ambiente.

Hotfix Obrigatório: [4.5.26.0 ou mais tarde](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history)

Suporte para:

-   Serviço de Sincronização MIM (SERVIÇO DE Sincronização FIM)
-   Serviço MIM (SERVIÇO FIM)
-   Registo de senha mim
-   Reset de palavra-passe mim
-   Serviço de Monitorização PAM (PamMonitoringService)
-   Serviço de Componentes PAM (PrivilegeManagementComponentService)

Não suportado:

-   O Portal MIM não é suportado, faz parte do ambiente do sharepoint e teria de implementar em modo agrícola e configurar a mudança automática de [palavra-passe no SharePointServer](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)
-   Todos os Agentes de Gestão
-   Gestão de Certificados da Microsoft
-   BHOLD

<a name="general-information"></a>Informação Geral 
--------------------

Leitura necessária para completar configuração e entender

-   [Visão geral das Contas de Serviço Geridas pelo Grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/library/jj128430(v=ws.11).aspx>

Primeiro passo no controlador de domínio das janelas

1.  Crie a chave de raiz dos Serviços de Distribuição Chave (KDS) (apenas uma vez por domínio) se necessário. A Chave Raiz é utilizada pelo serviço KDS em DCs (juntamente com outras informações) para gerar palavras-passe.

    -   Add-KDSRootKey – Eficaz Imediatamente

    -   "-EficazImediatamente" significa esperar até \~10 horas / necessidade de replicar a todos os DC. Isto foi aproximadamente 1 hora para dois controladores de domínio.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>Serviço de Sincronização
-----------------------

1.  Crie um grupo chamado "MIMSync_Servers" e adicione todos os servidores de Sincronização a este grupo.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  A partir do Windows PowerShell, em seguida, executar abaixo do comando como administrador de domínio com conta de computador já unida ao domínio

    -   New-ADServiceAccount -Name MIMSyncGMSAsvc -DNSHostName MIMSyncGMSAsvc.contoso.com -PrincipaisAllowedToRetrieveManagedPassword "MIMSync_Servers"

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   Obtenha detalhes do GSMA para sincronização:

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   Se executar o Serviço PCNS, terá de atualizar a delegação

    -   Set-ADServiceAccount -Identity MIMSyncGMSAsvc -ServicePrincipaNames \@{Add="PCNSCLNT/mimsync.contoso.com"}

3. Em seguida, nos serviços de sincronização certifique-se de que reserva a chave de encriptação, uma vez que será solicitada após a instalação do modo de mudança

    -   No Servidor que o Serviço de Sincronização está instalado na localização da ferramenta de gestão da chave do serviço de sincronização

    -   Por Padrão, o conjunto de **chaves de exportação** já está selecionado

    -   Clique em **Next**

    -   Agora será solicitado a introduzir a informação da conta de sincronização existente

    -   Insira e verifique a informação da Conta Sincronizada FIM

        -   Nome da conta - Nome da conta da conta do Serviço de Sincronização utilizada durante a instalação inicial

        -   Palavra-passe - Conta de Serviço de Sincronização

        -   Domínio - Domínio que a conta do Serviço de Sincronização está fora de

    -   Clique em **Next**

    -   Se inseria algo incorretamente, receberá o seguinte erro

    -   Agora que inseriu com sucesso as informações da Conta, será-lhe apresentada a opção de alterar o destino (localização do ficheiro de exportação) da chave de encriptação de cópia de segurança

        -   Por Predefinição, a localização do ficheiro de exportação é **C:\\Windows\\system32**\\miiskeys-1.bin.

4. Instale o Serviço de Sincronização SP1 Do Gestor de Identidade da Microsoft, construa 4.4.1302.0. pode ser encontrado no Centro de Descarregamento de Licenças de Volume ou no Site de Descarregamento mSDN. Uma vez concluída a instalação, guarde o teclado miiskeys.bin.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. Instale o mais recente [hotfix 4.5.x.x](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) ou mais tarde.

- Uma vez remendado o serviço de sincronização Stop FIM.
- Programas de painéis de controlo e funcionalidades do Gestor de Identidade da Microsoft
- Serviço de sincronização Alterar -\> Seguinte -\> Configuração -\> Seguinte

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  Limpar o nome da conta
-  Nome da conta de serviço DE Tipo **MIMSyncGMSA** com símbolo \$ como no
- imagem de imagem. Deixe a palavra-passe vazia.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- Próxima\> Instalação de\>
- Restaurar o teclado do ficheiro .bin guardado.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> A permissão SQL adicionada é a conta criada para login, pelo que deve permitir ao utilizador que aplica a permissão de modo de alteração para adicionar conta e dbo na base de dados do serviço de sincronização

## <a name="mim-service"></a>Serviço MIM
-----------

>[!IMPORTANT]
>O processo seguinte deve ser utilizado quando converter as contas relacionadas com o Serviço MIM para serem contas gMSA. Os cmdlets PowerShell observados no Apêndice só podem ser utilizados para alterar a informação da conta uma vez feita a configuração inicial.*

1.  Crie Contas Geridas pelo Grupo para o Serviço MIM, PAM Rest API, Serviço de Monitorização PAM, Serviço de Componentes PAM, Portal de Registo SSPR, Portal de Reset SSPR.

    -   Certifique-se de atualizar a delegação da GMSA e a SPN
        -   Set-ADServiceAccount -Identidade \<conta\> -ServicePrincipaNames \@{Add="\<SPN\>"}
        -   Delegação
            -   Set-ADServiceAccount -Identidade \<gsmaaccount\> -TrustForDelegação \$verdadeira
        -   Delegação restrita
            -   \$delspns = 'http/mim', 'http/mim.contoso.com'
            -   New-ADServiceAccount -Name \<gsmaaccount\> -DNSHostName \<gsmaaccount\>.contoso.com -DirectorsAllowedToRetrieveManagedPassword \<grupo\> -ServicePrincipaNames \$spns -OtherATributos \@{'msDS-AllowedToDelegateTo'=\$delspns }

2.  Adicione a conta para o serviço MIM em Sync Groups. É necessário para a SSPR.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **NOTA.**  Problema conhecido que os serviços que utilizam a conta gerida ficam pendurados após reiniciar o servidor devido ao Microsoft Key Distribution Service não é iniciado após reiniciar o Windows. O serviço não pôde ser iniciado e o Windows também não pôde ser reiniciado. O problema é reprodutível pelo menos no Windows Server 2012 R2. Saque para esta questão é comando de execução 

-   **sc triggerinfo kdssvc start/networkon**

    para iniciar o Serviço de Distribuição de Chaves da Microsoft quando a rede estiver ligado (tipicamente no início do ciclo de arranque).

    Ver discussão sobre questões semelhantes: <https://social.technet.microsoft.com/Forums/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>

4.  Executar MSI elevado do serviço MIM e selecionar alterações.

5.  Na "Configure página de ligação principal do servidor" verifique "Utilize uma conta diferente para a Troca (para contas geridas)". Aqui terá a opção de usar a conta antiga que tem uma caixa de correio ou usar caixa de correio na nuvem.
    >[!NOTE]
    >Quando a opção **Use Exchange Online** é selecionada, de forma a permitir ao Serviço MIM processar respostas de aprovação a partir do Add-On do OUTLOOK MIM, é necessário definir a chave de registo HKLM\SYSTEM\CurrentControlSet\Services\FIMService value of PollExchangeEnabled to 1 after installation.
    
![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  Na conta de serviço de página "Configure MIM" com \$ símbolo no final. Também digite palavra-passe de conta de e-mail de serviço. Conta de serviço A palavra-passe deve ser desativada.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  Como a função LogonUser não funciona para contas geridas, a página seguinte irá avisar "Por favor, verifique se a Conta de Serviço está segura na sua configuração atual".

![cid:image007.png\@01D36EB7.562E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  Na página "Configure Privileged Access Management REST API", digite nome da conta de piscina de aplicação com símbolo \$ no final e deixe o campo password vazio.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  No "Configure PAM Component Service" tipo nome da conta de serviço com \$ símbolo no final e deixe o campo password vazio.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![cid:image010.png\@01D36EB8. A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  No "Configure Privileged Access Management Management Monitoring Service" página tipo nome da conta de serviço com \$ símbolo no final e deixe o campo password vazio.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  No "Configure MIM Password Registration Portal" tipo nome da conta com \$ símbolo no final e deixe o campo password vazio.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  No tipo de página "Configure MIM Password Reset Portal" tipo nome de conta com símbolo \$ no final e deixe o campo password vazio.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  Instalação completa.

Nota:

-  Após a instalação, duas novas chaves são criadas no registo por caminho
    - "HKEY_LOCAL_MACHINE software\\\\Microsoft\\Front Identity
    - Manager\\2010\\Service" para armazenar senha de troca encriptada. Um para
    - Troca online e outra para troca no local (um deles deve ser
    - vazio).

![cid:image014.jpg\@01D36F53.303D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- Para atualizar a palavra-passe, fornecemos um download de script [aqui](microsoft-identity-manager-2016-gmsascript.md) para que o cliente não tenha que executar o modo de mudança

- Para encriptar a palavra-passe de troca, o instalador cria um serviço adicional e
    - executa-o sob a conta gerida. As seguintes mensagens serão adicionadas em
    - Registo de eventos de aplicação durante a instalação.

![cid:image016.jpg\@01D36F53.303D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)
