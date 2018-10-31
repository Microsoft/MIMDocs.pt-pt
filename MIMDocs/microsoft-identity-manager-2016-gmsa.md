---
title: Conversão de serviços específicos de MIM a gMSA | Documentos da Microsoft
description: Tópico que descreve os passos básicos para configurar o gMSA.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/27/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 63f2509d35355a8fe3a59b173756257298079a92
ms.sourcegitcommit: 6374aa4f7d58b7218626d36d0fc2dc4b38cb8332
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50237235"
---
# <a name="conversion-of-mim-specific-services-to-gmsa"></a>Conversão de serviços específicos de MIM a gMSA

Este guia irá percorrer os passos básicos para configurar o gMSA para os serviços suportados. O processo para converter a gMSA é fácil assim que a pré-configurar o seu ambiente.

A correção necessária: \<ligação para o KB mais recente\>

Suporte para:

-   Service(FIMSynchronizationService) de sincronização de MIM
-   Service(FIMService) de MIM
-   Registo de palavra-passe de MIM
-   Reposição de palavra-passe de MIM
-   Service(PamMonitoringService) de monitorização de PAM
-   Serviço de componente PAM (PrivilegeManagementComponentService)

Não suportado:

-   Portal de MIM não é suportado é parte do ambiente do sharepoint e terá de implementar no modo de farm e [configurar a alteração de palavra-passe automático no SharePointServer](https://docs.microsoft.com/sharepoint/administration/configure-automatic-password-change)
-   Todos os agentes de gestão
-   Gestão de certificados da Microsoft
-   BHOLD

<a name="general-information"></a>Informações Gerais 
--------------------

Leitura necessários para concluir a configuração e compreender

-   [Descrição geral das contas de serviço geridas de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)

-   <https://docs.microsoft.com/powershell/module/addsadministration/new-adserviceaccount?view=win10-ps>

-   <https://technet.microsoft.com/library/jj128430(v=ws.11).aspx>

Primeira etapa no seu controlador de domínio do windows

1.  Crie a chave de raiz de Services(KDS) de distribuição de chave (apenas uma vez por domínio), se necessário. Chave de raiz é utilizada pelo serviço KDS em controladores de domínio (juntamente com outras informações) para gerar palavras-passe.

    -   Adicionar-KDSRootKey – EffectiveImmediately

    -   "– EffectiveImmediately" significa que aguardar até \~10 horas / precisa de replicar para todos os DC. Isso era aproximadamente 1 hora para dois controladores de domínio.

![](media/7fbdf01a847ea0e330feeaf062e30668.png)

## <a name="synchronization-service"></a>Serviço de Sincronização
-----------------------

1.  Crie um grupo chamado "MIMSync_Servers" e adicione todos os servidores de sincronização para este grupo.

![](media/a4dc3f6c0cb1f715ba690744f54dce5c.png)

2.  Do windows PowerShell, em seguida, executar o comando abaixo como administrador de domínio com a conta de computador já associados ao domínio

    -   Novo-ADServiceAccount-nome MIMSyncGMSAsvc - DNSHostName MIMSyncGMSAsvc.contoso.com - PrincipalsAllowedToRetrieveManagedPassword "MIMSync_Servers"

![](media/f6deb0664553e01bcc6b746314a11388.png)

-   Para obter detalhes do GSMA para sincronização:

![](media/c80b0a7ed11588b3fb93e6977b384be4.png)

-   Se executar o serviço do PCNS, terá de atualizar a delegação

    -   Set-ADServiceAccount-identidade MIMSyncGMSAsvc - ServicePrincipalNames \@{Add="PCNSCLNT/mimsync.contoso.com"}

3. Em seguida na sincronização serviços não se esqueça da chave de encriptação de cópias de segurança, como este será solicitada na instalação do modo de alteração

    -   No servidor que o serviço de sincronização é instalado em localizar a ferramenta de gestão de chaves do serviço de sincronização

    -   Por predefinição, o **exportar o conjunto de chaves** já está selecionada

    -   Clique em **seguinte**

    -   Agora será solicitado que introduza as informações de conta de sincronização existente

    -   Introduza e verifique as informações de conta de sincronização do FIM

        -   Nome da conta - nome da conta da conta de serviço de sincronização utilizada durante a instalação inicial

        -   Palavra-passe – palavra-passe da conta de serviço de sincronização

        -   Domínio - domínio de que a conta de serviço de sincronização é distância de

    -   Clique em **seguinte**

    -   Se tiver introduzido algo incorretamente, receberá o erro seguinte

    -   Agora que introduziu com êxito as informações da conta, será apresentada com uma opção para alterar o destino (localização do ficheiro de exportação) a cópia de segurança da chave de encriptação

        -   Por predefinição, é a localização do ficheiro de exportação **c:\\Windows\\system32**\\miiskeys 1.bin.

4. Instale a compilação de serviço de sincronização do Microsoft Identity Manager SP1 4.4.1302.0. pode ser encontrado no Centro de transferências de licença de Volume ou o Centro de transferências MSDN. Depois de concluída a instalação, certifique-se, guardar o conjunto de chaves miiskeys.bin.

![](media/ef5f16085ec1b2b1637fa3d577a95dbf.png)


5. Instalar a versão mais recente [correção 4.5.x.x](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history) ou posterior.

- Uma vez o serviço de sincronização de FIM de parar de patches.
- Programas do painel de controle e funcionalidades do Microsoft Identity Manager
- Altere - o serviço de sincronização\> seguinte -\> configurar –\> seguinte

![](media/dc98c011bec13a33b229a0e792b78404.png)

-  Limpar o nome da conta
-  Nome de conta de serviço do tipo **MIMSyncGMSA** com \$ símbolo como no
- captura de ecrã. Deixe a palavra-passe vazia.

![](media/38df9369bf13e1c3066a49ed20e09041.png)

- Próxima -\> seguinte -\> instalar
- Restaure conjunto de chaves a partir do ficheiro. bin guardado.

![](media/44cd474323584feb6d8b48b80cfceb9b.png)

![](media/03e7762f34750365e963f0b90e43717c.png)
> [!NOTE]
> Permissão de SQL adicionado é a conta é criada para início de sessão, por isso, deve permitir ao usuário a aplicar a permissão de modo de alteração para adicionar a conta e dbo na base de dados de serviço de sincronização

## <a name="mim-service"></a>Serviço MIM
-----------

>[!IMPORTANT]
>Tem de utilizar o seguinte processo quando o primeiro converter o serviço MIM relacionados com contas para que sejam contas de gMSA. Os cmdlets do PowerShell observados no apêndice só pode ser utilizados para alterar as informações de conta, uma vez que a configuração inicial tiver sido done.*

1.  Criar o grupo gerido Portal de reposição de contas para o serviço MIM, API de Rest de PAM, serviço de monitorização PAM, o serviço de componentes de PAM, Portal de registo SSPR, SSPR.

    -   Certifique-se de que atualizar a delegação de gMSA e o SPN
        -   Set-ADServiceAccount-Identity \<conta\> - ServicePrincipalNames \@{adicionar = "\<SPN\>"}
        -   Delegação
            -   Set-ADServiceAccount-Identity \<gsmaaccount\> - TrustedForDelegation \$verdadeiro
        -   Delegação restrita
            -   \$delspns = ' http/mim', 'http/mim.contoso.com'
            -   Novo-ADServiceAccount-Name \<gsmaaccount\> - DNSHostName \<gsmaaccount\>. contoso.com - PrincipalsAllowedToRetrieveManagedPassword \<grupo\> - ServicePrincipalNames \$spns - OtherAttributes \@{msDS-AllowedToDelegateTo =\$delspns}

2.  Adicione a conta para o serviço MIM em grupos de sincronização. É necessário para SSPR.

![](media/0201f0281325c80eb70f91cbf0ac4d5b.jpg)

3.  **TENHA EM ATENÇÃO**.  Questão conhecida que os serviços que utilizam a conta gerida hang depois de reiniciar o servidor devido ao serviço de distribuição de chaves Microsoft não está iniciado depois de reiniciar o Windows. Não foi possível iniciar o serviço e o Windows não foi possível reiniciar demasiado. O problema é reproduzível, pelo menos, no Windows Server 2012 R2. Solução para este problema está a executar o comando 

-   **sc triggerinfo kdssvc início/networkon**

    para iniciar o serviço de distribuição de chaves Microsoft quando a rede está ativado (normalmente cedo no ciclo de arranque).

    Consulte a discussão sobre o problema semelhante: <https://social.technet.microsoft.com/Forums/a290c5c0-3112-409f-8cb0-ff23e083e5d1/ad-fs-windows-2012-r2-adfssrv-hangs-in-starting-mode?forum=winserverDS>

4.  Execute o MSI elevada do serviço MIM e selecione a alteração.

5.  Na "Página de ligação do servidor principal de configurar" selecione caixa de verificação "Utilizar outra conta para o Exchange (para contas geridas)". Aqui terá uma opção para utilizar a conta antiga que tem uma caixa de correio ou utilize a caixa de correio na nuvem.

![](media/0cd8ce521ed7945c43bef6100f8eb222.png)

6.  Na conta do serviço de tipo de página do "Conta de configurar o serviço MIM" com \$ símbolo no final. Também escreva a palavra-passe de conta do serviço de E-Mail. Palavra-passe da conta de serviço devem ser desativado.

![](media/db0d543df6e1b0174a47135617c23fcb.png)

7.  Como a função do LogonUser não funciona para contas gerenciadas, a página seguinte irá aviso ". Verifique se a conta de serviço está protegida na configuração atual".

![CID:image007.PNG\@01D36EB7.562E6CF0](media/d350bc13751b2d0a884620db072ed019.png)

8.  Na página de "Configurar Privileged acesso à API REST de gestão", escreva o nome de conta do conjunto aplicacional com \$ símbolo no final e deixar o campo de palavra-passe vazia.

![](media/88db2f6f291fff8bcdd0da5d538aafc6.png)

9.  Na página de "Configurar o serviço do componente PAM" escreva o nome da conta de serviço com \$ símbolo no final e deixar o campo de palavra-passe vazia.

![](media/93cfbcefb4d17635dd35c5ead690fd1e.png)

![CID:image010.PNG\@01D36EB8. A295A3F0](media/9d2b52f6faed10601e7e2166a339fb47.png)

10.  Na página de "Configurar Privileged Gestão monitorização do serviço de acesso" escreva o nome da conta de serviço com \$ símbolo no final e deixar o campo de palavra-passe vazia.

![](media/d1e824248edf12a77fc9ffb011475164.png)

11.  Na página de "Configurar a palavra-passe Portal de registo MIM" escreva o nome da conta com \$ símbolo no final e deixar o campo de palavra-passe vazia.

![](media/601e935cdfda298b61ae753a2a152996.png)

12.  Na página de "Configurar a palavra-passe de reposição de Portal de MIM" escreva o nome da conta com \$ símbolo no final e deixar o campo de palavra-passe vazia.

![](media/10c8cfa8ff2b6d703d14bd0b7ddc6949.png)

13.  Concluir a instalação.

Nota:

-  Após a instalação duas novas chaves são criadas no registo através do caminho
    - "HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Forefront Identity
    - Gestor\\2010\\serviço "para armazenar a palavra-passe encriptada Exchange. Um para
    - Exchange Online e outro para o Exchange no local (uma delas deve ser
    - vazio).

![CID:image014.jpg\@01D36F53.303D5190](media/73e2b8a3c149a4ec6bacb4db2c749946.jpg)

- Para atualizar a palavra-passe, fornecemos um script [baixe aqui](microsoft-identity-manager-2016-gmsascript.md) pelo cliente não tem de alterar modo de execução

- Para encriptar Exchange palavra-passe o instalador cria o serviço adicional e
    - é executado sob a conta gerenciada. Mensagens a seguir será adicionada a
    - Registo de eventos de aplicações durante a instalação.

![CID:image016.jpg\@01D36F53.303D5190](media/95b315454705cd4d939b55ac5ad910f5.jpg)
