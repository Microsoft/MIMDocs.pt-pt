---
title: "Trabalhar com a Criação de Relatórios Híbridos do Identity Manager | Microsoft Identity Manager"
description: "Saiba como combinar dados no local e na nuvem em relatórios híbridos no Azure e como gerir e ver estes relatórios."
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f9b01ac2cee2b96f64a9fda917f4f4146ca2eeda
ms.openlocfilehash: e2d0bd6120628d4fd2a14718fc205cde976c7785


---

# Trabalhar com a Criação de Relatórios Híbridos do Identity Manager

## Relatórios híbridos disponíveis
Os primeiros três relatórios do Microsoft Identity Manager (MIM) disponíveis no Azure AD são **Atividade de reposição de palavra-passe**, **Registo de reposição de palavra-passe** e **Atividade de grupos personalizados**.

-   O relatório Atividade de reposição de palavra-passe apresenta todas as instâncias em que um utilizador efetuou a reposição de palavra-passe através da SSPR e fornece as portas ou os **Métodos** utilizados para a autenticação.

    ![Imagem da criação de relatórios híbridos do Azure – atividade de reposição de palavra-passe](media/MIM-Hybrid-passwordreset.jpg)

-   O relatório Registo de reposição de palavra-passe apresenta sempre que um utilizador se regista na SSPR e os **Métodos** utilizados para autenticar, por exemplo, um número de telemóvel ou perguntas e respostas.
    Tenha em atenção que no relatório Registo de reposição de palavra-passe, não é efetuada qualquer diferenciação entre a porta de SMS e a porta de MFA – ambas são consideradas **Telemóvel**.

-   O relatório Atividade de grupos personalizados apresenta todas as vezes que uma pessoa se tenta adicionar ou eliminar de um grupo e criação de grupos.

> [!NOTE]
> Atualmente, os relatórios apresentam os dados até um mês atrás.
>
> Se pretender desinstalar a criação de relatórios híbridos, desinstale o agente MIMreportingAgent.msi.

## Pré-requisitos

1.  Instale o Microsoft Identity Manager 2016, incluindo o serviço MIM.

2.  Certifique-se de que tem um inquilino do Azure AD Premium com um administrador licenciado no diretório.

3.  Certifique-se de que tem ligação à Internet de saída no servidor do Microsoft Identity Manager para o Azure.

## Instalar os Relatórios do Microsoft Identity Manager no Azure AD
Depois de o agente de relatórios ser instalado, os dados da atividade do Microsoft Identity Manager são exportados do MIM para o registo de eventos do Windows. O agente de relatórios do MIM processa os eventos e carrega-os para o Azure. No Azure, os eventos são analisados, desencriptados e filtrados para os relatórios necessários.

1.  Instale o Microsoft Identity Manager 2016.

2.  Transfira os agentes de relatórios do Microsoft Identity Manager:

    1.  Inicie sessão no portal de gestão do Azure AD e clique no ícone do Active Directory.

    2.  Faça duplo clique no diretório do qual é um Administrador Global e para o qual tem uma subscrição do Azure AD Premium.

    3.  Clique em **Configuração** e transfira o agente de relatórios.

3.  Instale o agente de relatórios do Microsoft Identity Manager:

    1.  Crie um diretório no computador.

    2.  Extraia os ficheiros `MIMHybridReportingAgent.msi` e `tenant.cert` para o diretório.

    3.  Execute o instalador do agente.

    4.  Certifique-se de que o serviço de agente de relatórios do MIM está em execução

    5.  Reinicie o Serviço MIM.

4.  Verifique se os Relatórios do Microsoft Identity Manager estão a funcionar no Azure.

    Pode criar dados do relatório utilizando o Portal Reposição Personalizada de Palavra-passe do Microsoft Identity Manager para repor a palavra-passe de um utilizador. Certifique-se de que a reposição de palavra-passe foi concluída com êxito e, em seguida, verifique se os dados são apresentados no portal de gestão do Azure AD.

## Ver relatórios híbridos no portal clássico do Azure

1.  Inicie sessão no [Portal clássico do Azure](https://manage.windowsazure.com/) com a sua conta de administrador global do inquilino.

2.  Clique no ícone do **Active Directory**.

3.  Selecione o diretório do inquilino na lista de diretórios disponíveis da sua subscrição.

4.  Clique em **Relatórios** e em **Atividade de Reposição de Palavra-passe**.

5.  Certifique-se de que seleciona **Identity Manager** no menu pendente de origem.

> [!WARNING]
> Pode demorar algum tempo até que os dados do Microsoft Identity Manager sejam apresentados no Azure AD.

## Parar a criação de relatórios híbridos
Se pretender parar de carregar dados de relatórios do Microsoft Identity Manager para o Azure Active Directory, desinstale o agente de relatórios híbridos. Utilize a ferramenta **Adicionar ou Remover Programas** do Windows para desinstalar a Criação de Relatórios Híbridos do Microsoft Identity Manager.

## Eventos do Windows utilizados para a criação de relatórios híbridos
Os eventos gerados pelo Microsoft Identity Manager são registados no Registo de Eventos do Windows e são visíveis no Visualizador de Eventos em: Registos de Serviços e Aplicações – &gt;**Registo de Pedido do Identity Manager**. Todos os pedidos do MIM são exportados como eventos no Registo de Eventos do Windows na estrutura JSON. Isto pode ser exportado para o SIEM.

|Tipo de evento|ID|Detalhes do evento|
|--------------|------|-----------------|
|Informações|4121|Dados do evento do MIM que incluem todos os dados de pedidos.|
|Informações|4137|Extensão 4121 do evento do MIM, no caso de existirem demasiados dados para um único evento. O cabeçalho neste evento tem o seguinte formato: `"Request: <GUID> , message <xxx> out of <xxx>`|



<!--HONumber=Jun16_HO4-->


