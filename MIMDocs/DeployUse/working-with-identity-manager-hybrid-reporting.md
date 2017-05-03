---
title: "Trabalhar com os Relatórios Híbridos no Azure através do MIM 2016 | Documentos da Microsoft"
description: "Saiba como combinar dados no local e na cloud em relatórios híbridos no Azure e como gerir e ver estes relatórios."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 01/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3144ee195675df5dc120896cc801a7124ee12214
ms.openlocfilehash: 6b3fda2cb78ec885d986462dcf0edb8843811095
ms.lasthandoff: 04/27/2017


---

# <a name="working-with-identity-manager-hybrid-reporting"></a>Trabalhar com a Criação de Relatórios Híbridos do Identity Manager

## <a name="available-hybrid-reports"></a>Relatórios híbridos disponíveis
Os primeiros três relatórios do Microsoft Identity Manager (MIM) disponíveis no Azure AD são **Atividade de reposição de palavra-passe**, **Registo de reposição de palavra-passe** e **Atividade de grupos personalizados**.

-   O relatório Atividade de reposição de palavra-passe apresenta todas as instâncias em que um utilizador efetuou a reposição de palavra-passe através da SSPR e fornece as portas ou os **Métodos** utilizados para a autenticação.

    ![Imagem da criação de relatórios híbridos do Azure – atividade de reposição de palavra-passe](media/MIM-Hybrid-passwordreset2.jpg)

-   O relatório Registo de reposição de palavra-passe apresenta sempre que um utilizador se regista na SSPR e os **Métodos** utilizados para autenticar, por exemplo, um número de telemóvel ou perguntas e respostas.
    Tenha em atenção que no relatório Registo de reposição de palavra-passe, não é efetuada qualquer diferenciação entre a porta de SMS e a porta de MFA – ambas são consideradas **Telemóvel**.

-   O relatório Atividade de grupos personalizados apresenta todas as vezes que uma pessoa se tenta adicionar ou eliminar de um grupo e criação de grupos.

> [!NOTE]
> Atualmente, os relatórios apresentam os dados até um mês atrás.
>
> Se pretender desinstalar a criação de relatórios híbridos, desinstale o agente MIMreportingAgent.msi.

## <a name="prerequisites"></a>Pré-requisitos

1.  Instale o Microsoft Identity Manager 2016 RTM ou o serviço SP1 MIM.

2.  Certifique-se de que tem um inquilino do Azure AD Premium com um administrador licenciado no diretório.

3.  Certifique-se de que tem ligação à Internet de saída no servidor do Microsoft Identity Manager para o Azure.

## <a name="install-microsoft-identity-manager-reporting-agent-in-azure-ad"></a>Instalar o Agente de Relatórios do Microsoft Identity Manager no Azure AD
Depois de o agente de relatórios ser instalado, os dados da atividade do Microsoft Identity Manager são exportados do MIM para o registo de eventos do Windows. O agente de relatórios do MIM processa os eventos e carrega-os para o Azure. No Azure, os eventos são analisados, desencriptados e filtrados para os relatórios necessários.

1.  Instale o Microsoft Identity Manager 2016.

2.  Transfira os agentes de relatórios do Microsoft Identity Manager:

    1.  Inicie sessão no portal de gestão do Azure AD e clique no ícone do Active Directory.

    2.  Faça duplo clique no diretório do qual é um Administrador Global e para o qual tem uma subscrição do Azure AD Premium.

    3.  Clique em **Configuração** e transfira o agente de relatórios.

3.  Instale o agente de relatórios do Microsoft Identity Manager:

    1.  Transfira o [MIMHReportingAgentSetup.exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) para o servidor do Serviço Microsoft Identity Manager.
    2.  Execute `MIMHReportingAgentSetup.exe` 
    3.  Execute o instalador do agente.

    4.  Certifique-se de que o serviço de agente de relatórios do MIM está em execução

    5.  Reinicie o Serviço MIM.

4.  Verifique se os Relatórios do Microsoft Identity Manager estão a funcionar no Azure.

    Pode criar dados do relatório utilizando o Portal de Reposição Personalizada de Palavra-passe do Microsoft Identity Manager para repor a palavra-passe de um utilizador. Certifique-se de que a reposição de palavra-passe foi concluída com êxito e, em seguida, verifique se os dados são apresentados no portal de gestão do Azure AD.

## <a name="view-hybrid-reports-in-the-azure-classic-portal"></a>Ver relatórios híbridos no portal clássico do Azure

1.  Inicie sessão no [Portal do Azure](https://portal.azure.com/) com a sua conta de administrador global do inquilino.

2.  Clique no ícone do **Azure Active Directory**.

3.  Selecione o diretório do inquilino na lista de diretórios disponíveis da sua subscrição.

4.  Clique em **Registos de Auditoria**.

5.  Certifique-se de que seleciona o **Serviço MIM** no menu pendente Categoria.

> [!WARNING]
> Pode demorar algum tempo até que os dados de auditoria do Microsoft Identity Manager sejam apresentados no Azure AD.

## <a name="stop-creating-hybrid-reports"></a>Parar a criação de relatórios híbridos
Se quiser parar de carregar os dados de auditoria dos relatórios do Microsoft Identity Manager para o Azure Active Directory, desinstale o agente de relatórios híbridos. Utilize a ferramenta **Adicionar ou Remover Programas** do Windows para desinstalar a Criação de Relatórios Híbridos do Microsoft Identity Manager.

## <a name="windows-events-used-for-hybrid-reporting"></a>Eventos do Windows utilizados para a criação de relatórios híbridos
Os eventos gerados pelo Microsoft Identity Manager são registados no Registo de Eventos do Windows e são visíveis no Visualizador de Eventos em: Registos de Serviços e Aplicações – &gt;**Registo de Pedido do Identity Manager**. Todos os pedidos do MIM são exportados como eventos no Registo de Eventos do Windows na estrutura JSON. Isto pode ser exportado para o SIEM.

|Tipo de evento|ID|Detalhes do evento|
|--------------|------|-----------------|
|Informações|4121|Dados do evento do MIM que incluem todos os dados de pedidos.|
|Informações|4137|Extensão 4121 do evento do MIM, no caso de existirem demasiados dados para um único evento. O cabeçalho neste evento tem o seguinte formato: `"Request: <GUID> , message <xxx> out of <xxx>`|

