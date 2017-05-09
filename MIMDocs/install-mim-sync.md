---
title: "Instalar o Serviço de Sincronização do Microsoft Identity Manager | Documentos da Microsoft"
description: "Comece a trabalhar com os componentes do MIM 2016 ao instalar e configurar o Serviço de Sincronização."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 54d03fbd03f6c44298139324ea2dc7d945f008bc
ms.openlocfilehash: c6cf0c93679319716c34904ea6239902010e0860
ms.lasthandoff: 01/24/2017


---

# <a name="install-mim-2016-mim-synchronization-service"></a>Instalar o MIM 2016: Serviço de Sincronização do MIM

>[!div class="step-by-step"]
[« Exchange Server](prepare-server-exchange.md)
[Serviço e Portal do MIM »](install-mim-service-portal.md)

> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio – **nomedoservidormim**
> - Nome de domínio – **contoso**
> - Palavra-passe – **Pass@word1**

Para instalar componentes do Microsoft Identity Manager 2016, configure primeiro o pacote de instalação.

1. Inicie sessão como *contoso\Administrador* no servidor que está a utilizar para a gestão de identidades.

2. Descompacte o pacote de instalação do MIM ou monte o DVD da imagem do MIM.

## <a name="install-mim-2016-synchronization-service"></a>Instalar o Serviço de Sincronização do MIM 2016

1. Na pasta de instalação descompactada do MIM, navegue até à pasta **Serviço de Sincronização**.

2. Execute o **instalador do Serviço de Sincronização do MIM**. Siga as diretrizes do instalador e conclua a instalação.

3. No ecrã de boas-vindas, clique em **Seguinte**.

    ![Imagem de boas-vindas do assistente do instalador do MIM](media/MIM-Install1.png)

4. Reveja os termos de licenciamento e clique em **Seguinte** para os aceitar.

5. No ecrã **Configuração Personalizada**, clique em **Seguinte**.

    ![Imagem da Configuração Personalizada](media/MIM-Install2.png)

6.  No ecrã de configuração da base de dados do Serviço de Sincronização, selecione:

    1.  O SQL Server está localizado: **Neste computador**.

    2.  A instância do SQL Server é: **A instância predefinida**.

    ![Imagem de ligação à base de dados](media/MIM-Install3.png)

7.  Configure a Conta do Serviço de Sincronização, de acordo com a conta que criou anteriormente:

    1.  Conta de serviço: *SincronizaçãoMIM*

    2.  Palavra-passe: *Pass@word1*

    3.  Domínio da Conta de Serviço ou nome do computador local: *contoso*

    ![Imagem da conta de serviço](media/MIM-Install4.png)

8.  Indique no instalador do Serviço de Sincronização do MIM os grupos de segurança relevantes:

    1. Administrador = *contoso\AdministradoresSincronizaçãoMIM*

    2. Operador = *contoso\OperadoresSincronizaçãoMIM*

    3. Participante = *contoso\ParticipantesSincronizaçãoMIM*

    4. Procurar conetores = *contoso\ProcurarSincronizaçãoMIM*

    5. Gestão de Palavras-passe WMI = *contoso\ReposiçãoPalavras-passeSincronizaçãoMIM*

    ![Imagem dos grupos de segurança](media/MIM-Install5.png)

9. No ecrã de definições de segurança, selecione **Ativar regras de firewall para comunicações de RPC de entrada** e clique em **Seguinte**.

10. Clique em **Instalar** para iniciar a instalação do Serviço de Sincronização do MIM.

    1. Pode ser apresentado um aviso relativo à conta de serviço de Sincronização do MIM – clique em **OK**.

    2. Irá instalar o Serviço de Sincronização de MIM.

    3. É apresentado um aviso relativo à criação de uma cópia de segurança da chave de encriptação – clique em **OK** e, em seguida, selecione uma pasta para armazenar a cópia de segurança da chave de encriptação.

        ![Imagem do aviso da chave de encriptação da cópia de segurança da Sincronização do MIM](media/MIM-Install7.png)

    4. Quando o instalador concluir a instalação com êxito, clique em **Concluir**.

    5. Tem de terminar e iniciar sessão novamente para que as alterações às associações a grupos sejam aplicadas. Clique em **Sim** para terminar sessão.

>[!div class="step-by-step"]  
[« Exchange Server](prepare-server-exchange.md)
[Serviço e Portal do MIM »](install-mim-service-portal.md)

