---
title: Instalar o Serviço de Sincronização do Microsoft Identity Manager | Documentos da Microsoft
description: Comece a trabalhar com os componentes do MIM 2016 ao instalar e configurar o Serviço de Sincronização.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/01/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: cec04cf430ba38ec40b61e4aad68fd8447d13c99
ms.sourcegitcommit: 4f0b2883922bcb8fbef6b4284c35c6ca62c11565
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/27/2019
ms.locfileid: "56952184"
---
# <a name="install-mim-2016-mim-synchronization-service"></a>Instale o MIM 2016: Serviço de Sincronização do MIM

> [!div class="step-by-step"]
> [« Exchange Server](prepare-server-exchange.md)
> [Serviço e Portal do MIM »](install-mim-service-portal.md)
> 
> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio - **corpdc**
> - Nome de domínio – **contoso**
> - Nome do servidor de serviço de MIM - **corpservice**
> - Nome do servidor de sincronização de MIM - **corpsync**
> - SQL Server name - **corpsql**
> - Palavra-passe – <strong>Pass@word1</strong>

Para instalar componentes do Microsoft Identity Manager 2016, configure primeiro o pacote de instalação.

1. Inicie sessão como *contoso\miminstall* para o servidor estiver a utilizar para o servidor de sincronização de gestão de identidade **corpsync**.

2. Descompacte o pacote de instalação do MIM ou monte o DVD da imagem do MIM.  Se não tiver esta DVD, consulte [downloads e licenciamento do Microsoft Identity Manager](microsoft-identity-manager-licensing.md).

## <a name="install-mim-2016-sp1-synchronization-service"></a>Instalar o serviço de sincronização de MIM 2016 SP1

1. Na pasta de instalação descompactada do MIM, navegue até à pasta **Serviço de Sincronização**.

2. Execute o **instalador do Serviço de Sincronização do MIM**. Siga as diretrizes do instalador e conclua a instalação.

3. No ecrã de boas-vindas, clique em **Seguinte**.

    ![Imagem de boas-vindas do assistente do instalador do MIM](media/install-mim-sync/MIM_Install1.png)

4. Reveja os termos de licenciamento e clique em **Seguinte** para os aceitar.

5. No ecrã **Configuração Personalizada**, clique em **Seguinte**.

    ![Imagem da Configuração Personalizada](media/install-mim-sync/MIM_Install2.png)

6. No ecrã de configuração da base de dados do Serviço de Sincronização, selecione:

   1.  O SQL Server está localizado em: **Um computador remoto** chamado **corpsql.contoso.com**.

   2.  Instância do SQL Server é: **A instância predefinida**

   ![Imagem de ligação à base de dados](media/install-mim-sync/MIM_Install3.png)

7. Configure a Conta do Serviço de Sincronização, de acordo com a conta que criou anteriormente:

   1. Conta de serviço: *MIMSync*

   2. Palavra-passe: <em>Pass@word1</em>

   3. Domínio da Conta de Serviço ou nome do computador local: *contoso*

   ![Imagem da conta de serviço](media/install-mim-sync/MIM_Install4.png)

8. Indique no instalador do Serviço de Sincronização do MIM os grupos de segurança relevantes:

   1. Administrador = *contoso\AdministradoresSincronizaçãoMIM*

   2. Operador = *contoso\OperadoresSincronizaçãoMIM*

   3. Participante = *contoso\ParticipantesSincronizaçãoMIM*

   4. Procurar conetores = *contoso\ProcurarSincronizaçãoMIM*

   5. Gestão de Palavras-passe WMI = *contoso\ReposiçãoPalavras-passeSincronizaçãoMIM*

   ![Imagem dos grupos de segurança](media/install-mim-sync/MIM_Install5.png)

9. No ecrã de definições de segurança, selecione **Ativar regras de firewall para comunicações de RPC de entrada** e clique em **Seguinte**.

10. Clique em **Instalar** para iniciar a instalação do Serviço de Sincronização do MIM.

    1. Pode ser apresentado um aviso relativo à conta de serviço de Sincronização do MIM – clique em **OK**.

    2. Irá instalar o Serviço de Sincronização de MIM.

    3. É apresentado um aviso relativo à criação de uma cópia de segurança da chave de encriptação – clique em **OK** e, em seguida, selecione uma pasta para armazenar a cópia de segurança da chave de encriptação.

        ![Imagem do aviso da chave de encriptação da cópia de segurança da Sincronização do MIM](media/MIM-Install7.png)

    4. Quando o instalador concluir a instalação com êxito, clique em **Concluir**.

    5. Tem de terminar e iniciar sessão novamente para que as alterações às associações a grupos sejam aplicadas. Clique em **Sim** para terminar sessão.

> [!div class="step-by-step"]  
> [« Exchange Server](prepare-server-exchange.md)
> [Serviço e Portal do MIM »](install-mim-service-portal.md)
