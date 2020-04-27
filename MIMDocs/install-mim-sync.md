---
title: Instalar o Serviço de Sincronização do Microsoft Identity Manager | Documentos da Microsoft
description: Comece a trabalhar com os componentes do MIM 2016 ao instalar e configurar o Serviço de Sincronização.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/18/2019
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: 8e4371c8d3caac06f7200d8439b30b7aa978a336
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042462"
---
# <a name="install-mim-2016-mim-synchronization-service"></a>Instalar o MIM 2016: Serviço de Sincronização do MIM

> [!div class="step-by-step"]
> [« Serviço](prepare-server-exchange.md)
> e Portal do Servidor de Intercâmbio[MIM»](install-mim-service-portal.md)
 
> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio - **corpdc**
> - Nome de domínio – **contoso**
> - Nome do Servidor de Serviço MIM - **corpservice**
> - Nome do Servidor MIM Sync - **corpsync**
> - Nome do Servidor SQL - **corpsql**
> - Senha -<strong>Pass@word1</strong>

Para instalar componentes do Microsoft Identity Manager 2016, configure primeiro o pacote de instalação.

1. Inscreva-se como *contoso\miminstall* para o servidor que está a usar para **sincronização**de sincronização de gestão de identidade .

2. Descompacte o pacote de instalação do MIM ou monte o DVD da imagem do MIM.  Se não tiver este DVD, consulte o [licenciamento e os downloads](microsoft-identity-manager-licensing.md)do Microsoft Identity Manager.

## <a name="install-mim-2016-sp1-synchronization-service"></a>Instalar serviço de sincronização MIM 2016 SP1

1. Na pasta de instalação descompactada do MIM, navegue até à pasta **Serviço de Sincronização**.

2. Execute o **instalador do Serviço de Sincronização do MIM**. Siga as diretrizes do instalador e conclua a instalação.

3. No ecrã de boas-vindas, clique em **Seguinte**.

    ![Imagem de boas-vindas do assistente do instalador do MIM](media/install-mim-sync/MIM_Install1.png)

4. Reveja os termos de licenciamento e clique em **Seguinte** para os aceitar.

5. No ecrã **Configuração Personalizada**, clique em **Seguinte**.

    ![Imagem da Configuração Personalizada](media/install-mim-sync/MIM_Install2.png)

6. No ecrã de configuração da base de dados do Serviço de Sincronização, selecione:

   1.  O Servidor SQL está localizado em: **Uma máquina remota** chamada **corpsql.contoso.com**.

   2.  A instância do SQL Server é: **A instância predefinida**

   ![Imagem de ligação à base de dados](media/install-mim-sync/MIM_Install3.png)

    3. *MIM 2016 SP2 e mais tarde:* Configure o nome de base de dados do serviço de sincronização MIM

7. Configure a Conta do Serviço de Sincronização, de acordo com a conta que criou anteriormente:

   1. Conta de serviço: *SincronizaçãoMIM*

   2. Senha:<em>Pass@word1</em>

   3. Domínio da Conta de Serviço ou nome do computador local: *contoso*

    >[!NOTE]
    >MIM 2016 SP2 e mais tarde: para Contas **$** de Serviço Geridas pelo Grupo, certifique-se de que o personagem está no final do Nome da Conta de Serviço, por exemplo, MIMSync$, e deixe o campo password vazio.

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
> [« Serviço](prepare-server-exchange.md)
> e Portal do Servidor de Intercâmbio[MIM»](install-mim-service-portal.md)
