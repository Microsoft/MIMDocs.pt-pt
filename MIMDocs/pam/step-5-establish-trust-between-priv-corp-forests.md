---
title: Implementar o PAM passo 5 – Ligação à Floresta | Documentos da Microsoft
description: Estabelecer confiança entre as florestas PRIV e as florestas CORP para que os utilizadores com privilégios no PRIV passa, ainda assim, aceder aos recursos no CORP.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/29/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: eef248c4-b3b6-4b28-9dd0-ae2f0b552425
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 04195febdb721291e9dcf72f5bbda04923075596
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "64518268"
---
# <a name="step-5--establish-trust-between-priv-and-corp-forests"></a>Passo 5 - Estabelecer confiança entre florestas PRIV e CORP

> [!div class="step-by-step"]
> [«Passo 4](step-4-install-mim-components-on-pam-server.md)
> [Passo 6»](step-6-transition-group-to-pam.md)

Para cada domínio CORP, como contoso.local, os controladores de domínio PRIV e CONTOSO têm de estar vinculados por uma confiança. Isto permite aos utilizadores no domínio PRIV aceder aos recursos no domínio CORP.

## <a name="connect-each-domain-controller-to-its-counterpart"></a>Ligar cada controlador de domínio ao respetivo homólogo

Antes de estabelecer confiança, cada controlador de domínio tem de ser configurado para resolução de nomes DNS para o respetivo homólogo, com base no endereço IP do servidor do outro controlador de domínio/DNS.

1.  Se os controladores de domínio ou o servidor com o software MIM forem implementados como máquinas virtuais, certifique-se de que não existem não existem outros servidores DNS que estejam a fornecer serviços de atribuição de nomes de domínio a esses computadores.
    - Se as máquinas virtuais tiverem várias interfaces de rede, incluindo interfaces de rede ligadas a redes públicas, poderá ter de desativar essas ligações temporariamente ou substituir as definições de interface de rede do Windows. É importante certificar-se de que não é utilizado um endereço de servidor DNS fornecido por DHCP por nenhuma máquina virtual.

2.  Verifique se que cada controlador de domínio CORP existente consegue encaminhar nomes para a floresta PRIV. Em cada controlador de domínio fora da floresta PRIV, como CORPDC, inicie o PowerShell e escreva o seguinte comando:

    ```cmd
    nslookup -qt=ns priv.contoso.local.
    ```
    Verifique se a saída indica um registo nameserver para o domínio PRIV com o endereço IP correto.

3.  Se o controlador de domínio não conseguir encaminhar o domínio PRIV, utilize o **Gestor de DNS** (localizado em **Iniciar** > **Ferramentas de Aplicação** > **DNS**) para configurar o encaminhamento de nomes DNS do domínio PRIV para o endereço IP de PRIVDC. Se for um domínio superior (por exemplo, contoso.local), expanda os nós para este controlador de domínio e o respetivo domínio, como **CORPDC** > **Zonas de Pesquisa Direta** > **contoso.local** e certifique-se de que existe uma chave denominada **priv** como um tipo de Servidor de Nomes (NS).

    ![estrutura do ficheiro para a chave priv - captura de ecrã](./media/PAM_GS_DNS_Manager.png)

## <a name="establish-trust-on-pamsrv"></a>Estabelecer confiança em PAMSRV

No PAMSRV, estabeleça uma confiança unidirecional com cada domínio, como CORPDC, para que os controladores de domínio CORP confiem na floresta PRIV.

1. Inicie sessão em PAMSRV como um administrador de domínio PRIV (PRIV\Administrator).

2.  Inicie o PowerShell.

3.  Escreva os seguintes comandos do PowerShell para cada floresta existente. Quando lhe for pedido, introduza as credenciais de administrador de domínio CORP (CONTOSO\Administrator).

    ```PowerShell
    $ca = get-credential
    New-PAMTrust -SourceForest "contoso.local" -Credentials $ca
    ```

4.  Escreva os seguintes comandos do PowerShell para cada domínio nas florestas existentes. Quando lhe for pedido, introduza as credenciais de administrador de domínio CORP (CONTOSO\Administrator).

    ```PowerShell
    $ca = get-credential
    New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials $ca
    ```

## <a name="give-forests-read-access-to-active-directory"></a>Conceder acesso de leitura de florestas ao Active Directory

Para cada floresta existente, ative o acesso de leitura para o AD pelos administradores do PRIV e pelo serviço de monitorização.

1. Inicie sessão no controlador de domínio da floresta CORP existente, (CORPDC), como um administrador de domínio para o domínio de nível superior dessa floresta (Contoso\Administrator).  
2. Inicie **Utilizadores e Computadores do Active Directory**.  
3. Clique com o botão direito do rato no domínio **contoso.local** e selecione **Delegar controlo**.  
4. No separador Utilizadores e Grupos Selecionados, clique em **Adicionar**.  
5. Na janela Selecionar Utilizadores, Computadores ou Grupos, clique em **Localizações** e altere a localização para *priv.contoso.local*.  No nome do objeto, escreva *Admins do domínio* e clique em **Verificar nomes**. Quando aparecer um pop-up, introduza o nome de utilizador *priv\administrator* e a respetiva palavra-passe.  
6. A seguir a Admins do Domínio, adicione " *; MIMMonitor*". Depois de os nomes de **Admins do Domínio** e **MIMMonitor** estarem sublinhados, clique em **OK** e, em seguida, clique em **Seguinte**.  
7. Na lista de tarefas comuns, selecione **Ler todas as informações do utilizador** e, em seguida, clique em **Seguinte** e em **Concluir**.  
8. Feche Computadores e Utilizadores do Active Directory.

9. Abra uma janela do PowerShell.
10. Utilize `netdom` para garantir que o histórico de SIDs está ativado e a filtragem por SID está desativada. Tipo:
    ```cmd
    netdom trust contoso.local /quarantine:no /domain priv.contoso.local
    netdom trust /enablesidhistory:yes /domain priv.contoso.local
    ```
    A saída deverá indicar **A ativar o histórico de SIDs para esta fidedignidade** ou **O histórico de SIDs já está ativado para esta fidedignidade**.

    A saída também deve indicar **A filtragem por SID não está ativada para esta fidedignidade**. Veja [Desativar quarentena do filtro por SID](http://technet.microsoft.com/library/cc772816.aspx) para obter mais informações.

## <a name="start-the-monitoring-and-component-services"></a>Iniciar os serviços de Monitorização e Componente

1.  Inicie sessão em PAMSRV como um administrador de domínio PRIV (PRIV\Administrator).

2.  Inicie o PowerShell.

3.  Escreva os seguintes comandos do PowerShell.

    ```cmd
    net start "PAM Component service"
    net start "PAM Monitoring service"
    ```

No próximo passo, irá mover um grupo para o PAM.

> [!div class="step-by-step"]
> [«Passo 4](step-4-install-mim-components-on-pam-server.md)
> [Passo 6»](step-6-transition-group-to-pam.md)
