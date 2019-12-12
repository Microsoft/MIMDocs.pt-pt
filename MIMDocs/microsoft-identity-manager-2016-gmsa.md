---
title: Conversão de serviços específicos do MIM para gMSA | Microsoft Docs
description: Tópico que descreve as etapas básicas para configurar o gMSA.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/27/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 96d375d82a71a21f0be444d628f387c4e1ffdd09
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "64520694"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>Conversão de serviços específicos do MIM para gMSA

Este guia irá percorrer as etapas básicas para configurar o gMSA para os serviços com suporte. O processo para converter em gMSA é fácil depois de pré-configurar o ambiente.

Hotfix necessário: \<link para o KB mais recente\>

Suporte para:

-   Serviço de sincronização do MIM (FIMSynchronizationService)
-   Serviço do MIM (FIMService)
-   Registro de senha do MIM
-   Redefinição de senha do MIM
-   Serviço de monitoramento do PAM (PamMonitoringService)
-   Serviço de componente do PAM (PrivilegeManagementComponentService)

Sem suporte:

-   O portal do MIM não é suportado ele faz parte do ambiente do SharePoint e você precisaria implantar no modo de farm e [Configurar a alteração automática de senha no sharepointserver](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)
-   Todos os agentes de gerenciamento
-   Gerenciamento de certificados da Microsoft
-   BHOLD

<a name="general-information"></a>Informações Gerais 
--------------------

Leitura necessária para concluir a instalação e entender

-   [Visão geral de contas de serviço gerenciado de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/library/jj128430(v=ws.11).aspx>

Primeira etapa no controlador de domínio do Windows

1.  Crie a chave raiz do KDS (serviços de distribuição de chaves) (somente uma vez por domínio), se necessário. A chave raiz é usada pelo serviço KDS em DCs (juntamente com outras informações) para gerar senhas.

    -   Add-KDSRootKey – EffectiveImmediately

    -   "– EffectiveImmediately" significa aguardar até \~10 horas/precisar replicar para todos os controladores de domínio. Isso foi de aproximadamente 1 hora para dois controladores de domínio.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>Serviço de Sincronização
-----------------------

1.  Crie um grupo chamado "MIMSync_Servers" e adicione todos os servidores de sincronização a esse grupo.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  No Windows PowerShell, execute o comando abaixo como administrador de domínio com a conta de computador já ingressada no domínio

    -   New-ADServiceAccount-Name MIMSyncGMSAsvc-DNSHostName MIMSyncGMSAsvc.contoso.com-PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   Obter detalhes do GSMA para sincronização:

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   Se estiver executando o serviço PCNS, será necessário atualizar a delegação

    -   Set-ADServiceAccount-Identity MIMSyncGMSAsvc-servicePrincipalName \@{Add = "PCNSCLNT/mimsync. contoso. com"}

3. Em seguida, nos serviços de sincronização, certifique-se de fazer backup da chave de criptografia, pois ela será solicitada após a instalação do modo de alteração

    -   No servidor em que o serviço de sincronização está instalado, localize a ferramenta de gerenciamento de chaves do serviço de sincronização

    -   Por padrão, o **conjunto de chaves de exportação** já está selecionado

    -   Clique em **Avançar**

    -   Agora, você será solicitado a inserir as informações da conta de sincronização existente

    -   Inserir e verificar as informações da conta de sincronização do FIM

        -   Nome da conta-nome da conta de serviço de sincronização usado durante a instalação inicial

        -   Senha-senha da conta de serviço de sincronização

        -   Domínio-domínio do qual a conta de serviço de sincronização está separada

    -   Clique em **Avançar**

    -   Se você inseriu algo incorretamente, receberá o seguinte erro

    -   Agora que você inseriu com êxito as informações da conta, será exibida uma opção para alterar o destino (local do arquivo de exportação) da chave de criptografia de backup

        -   Por padrão, o local do arquivo de exportação é **C:\\Windows\\system32**\\miiskeys-1. bin.

4. Instale o serviço de sincronização do Microsoft Identity Manager SP1 Build 4.4.1302.0. Você pode encontrar no centro de download de licença de volume ou no site de download do MSDN. Depois de concluir a instalação, verifique se você salvou o conjunto de chaves miiskeys. bin.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. Instale o último [hotfix 4.5. x. x](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) ou posterior.

- Depois do patch, pare o serviço de sincronização do FIM.
- Programas e recursos do painel de controle Microsoft Identity Manager
- Alteração do serviço de sincronização-\> Next-\> configure-\> avançar

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  Limpar o nome da conta
-  Digite nome da conta de serviço **MIMSyncGMSA** com o símbolo de \$ como no
- captura. Deixe a senha vazia.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- Próxima\> instalação do próximo\>
- Restaure o conjunto de chaves do arquivo. bin salvo.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> A permissão SQL adicionada é a conta criada para logon, portanto, você deve permitir que o usuário aplique a permissão modo de alteração para adicionar conta e dbo no banco de dados do serviço de sincronização

## <a name="mim-service"></a>Serviço MIM
-----------

>[!IMPORTANT]
>O processo a seguir deve ser usado ao converter pela primeira vez as contas relacionadas ao serviço do MIM para serem gMSA contas. Os cmdlets do PowerShell indicados no apêndice só podem ser usados para alterar as informações da conta após a conclusão da configuração inicial. *

1.  Crie contas gerenciadas de grupo para o serviço do MIM, a API REST do PAM, o serviço de monitoramento do PAM, o serviço de componentes do PAM, o portal de registro SSPR, o portal de

    -   Certifique-se de atualizar a delegação e o SPN do gMSA
        -   Set-ADServiceAccount-Identity \<conta\>-servicePrincipalName do \@{Add = "\<SPN\>"}
        -   Delegação
            -   Set-ADServiceAccount-Identity \<gsmaaccount\>-TrustedForDelegation \$true
        -   Delegação Restrita
            -   \$delspns = ' http/mim ', ' http/mim. contoso. com '
            -   New-ADServiceAccount-Name \<gsmaaccount\>-DNSHostName \<gsmaaccount\>. contoso.com-PrincipalsAllowedToRetrieveManagedPassword \<Group\>-ServiceName \$SPNs-Outroattributes \@{' msDS-AllowedToDelegateTo ' =\$delspns}

2.  Adicionar conta para o serviço do MIM em grupos de sincronização. É necessário para o SSPR.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **Observação**.  Problema conhecido que os serviços que usam a conta gerenciada travam após a reinicialização do servidor porque o serviço de distribuição de chaves da Microsoft não foi iniciado após a reinicialização do Windows. Não foi possível iniciar o serviço e o Windows também não pôde ser reiniciado. O problema é reproduzível pelo menos no Windows Server 2012 R2. A solução alternativa para esse problema é executar comando 

-   **f triggerinfo kdssvc início/rede**

    para iniciar o serviço de distribuição de chaves da Microsoft quando a rede estiver ativada (normalmente no início do ciclo de inicialização).

    Consulte a discussão sobre o problema semelhante: <https://social.technet.microsoft.com/Forums/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>

4.  Execute o MSI elevado do serviço MIM e selecione Alterar.

5.  Na caixa de seleção "configurar a página de conexão do servidor principal", marque "usar conta diferente para o Exchange (para contas gerenciadas)". Aqui, você terá a opção de usar a conta antiga que tem uma caixa de correio ou usar a caixa de correio de nuvem.

![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  Na página "configurar conta de serviço do MIM", digite a conta de serviço com \$ símbolo no final. Digite também senha da conta de email do serviço. A senha da conta de serviço deve ser desabilitada.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  Como a função LogonUser não funciona para contas gerenciadas, a próxima página será aviso "Verifique se a conta de serviço é segura em sua configuração atual".

![CID: image007. png\@01D36EB 7.562 E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  Na página "configurar a API REST do Privileged Access Management", digite o nome da conta do pool de aplicativos com \$ símbolo no final e deixe o campo senha vazio.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  Na página "configurar serviço de componente do PAM", digite o nome da conta de serviço com \$ símbolo no final e deixe o campo senha vazio.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![CID: image010. png\@01D36EB8. A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  No tipo de página "configurar serviço de monitoramento de Privileged Access Management", digite o nome da conta de serviço com \$ símbolo no final e deixe o campo de senha vazio.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  Na página "configurar o portal de registro de senha do MIM", digite o nome da conta com \$ símbolo no final e deixe o campo senha vazio.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  Na página "configurar o portal de redefinição de senha do MIM", digite o nome da conta com \$ símbolo no final e deixe o campo senha vazio.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  Concluir a instalação.

Nota:

-  Após a instalação, duas novas chaves são criadas no registro por caminho
    - "HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Forefront Identity
    - Gerenciador\\2010\\serviço "para armazenar a senha criptografada do Exchange. Um para
    - Exchange Online e outro para o Exchange local (um deles deve ser
    - vazio).

![CID: image014. jpg\@01D36F 53.303 D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- Para atualizar a senha, fornecemos um download de script [aqui](microsoft-identity-manager-2016-gmsascript.md) , portanto, o cliente não precisará executar o modo de alteração

- Para criptografar a senha do Exchange, o instalador cria um serviço adicional e
    - executa-o na conta gerenciada. As seguintes mensagens serão adicionadas em
    - Log de eventos do aplicativo durante a instalação.

![CID: image016. jpg\@01D36F 53.303 D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)
