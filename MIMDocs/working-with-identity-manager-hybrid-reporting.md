---
title: "Trabalhar com os Relatórios Híbridos no Azure através do MIM 2016 | Documentos da Microsoft"
description: "Saiba como combinar dados no local e na cloud em relatórios híbridos no Azure e como gerir e ver estes relatórios."
keywords: 
author: fimguy
ms.author: billmath
manager: femila
ms.date: 04/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: df842309034ad68151dd8cc4151507e7ece6626d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="working-with-identity-manager-hybrid-reporting---public-preview-refresh"></a>Trabalhar com a Criação de Relatórios Híbridos do Identity Manager – Pré-visualização Pública (Atualização)

## <a name="available-hybrid-reports"></a>Relatórios híbridos disponíveis
Os primeiros três relatórios do Microsoft Identity Manager (MIM) disponíveis no Azure AD são **Atividade de reposição de palavra-passe**, **Registo de reposição de palavra-passe** e **Atividade de grupos personalizados**.

-   O relatório Atividade de reposição de palavra-passe apresenta todas as instâncias em que um utilizador efetuou a reposição de palavra-passe através da SSPR e fornece as portas ou os **Métodos** utilizados para a autenticação.

-   O relatório Registo de reposição de palavra-passe apresenta sempre que um utilizador se regista na SSPR e os **Métodos** utilizados para autenticar, por exemplo, um número de telemóvel ou perguntas e respostas.
    Tenha em atenção que no relatório Registo de reposição de palavra-passe, não é efetuada qualquer diferenciação entre a porta de SMS e a porta de MFA – ambas são consideradas **Telemóvel**.

-   O relatório Atividade de grupos personalizados apresenta todas as vezes que uma pessoa se tenta adicionar ou eliminar de um grupo e criação de grupos.

    ![Imagem da criação de relatórios híbridos do Azure – atividade de reposição de palavra-passe](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> Atualmente, os relatórios apresentam os dados até um mês atrás.</br>
> Tem de desinstalar o agente híbrido anterior.</br>
> Se pretender desinstalar a criação de relatórios híbridos, desinstale o agente MIMreportingAgent.msi.

## <a name="prerequisites"></a>Pré-requisitos

1.  Instale o Microsoft Identity Manager 2016 RTM ou o serviço SP1 MIM.

2.  Certifique-se de que tem um inquilino do Azure AD Premium com um administrador licenciado no diretório.

3.  Certifique-se de que tem ligação à Internet de saída no servidor do Microsoft Identity Manager para o Azure.

## <a name="requirements"></a>Requisitos
A tabela seguinte é uma lista de requisitos para utilizar a Criação de Relatórios Híbridos do Microsoft Identity Manager.

| Requisito | Descrição |
| --- | --- |
| Azure AD Premium | A Criação de Relatórios Híbridos é uma funcionalidade do Azure AD Premium e requer o mesmo. </br></br>Para obter mais informações, consulte [Introdução ao Azure AD Premium](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-get-started-premium). </br>Para iniciar uma avaliação gratuita de 30 dias, consulte [Iniciar uma versão de avaliação](https://azure.microsoft.com/trial/get-started-active-directory/). |
| Tem de ser um administrador global do seu Azure AD para começar a utilizar |Por predefinição, apenas os administradores globais podem instalar e configurar os agentes para começar a utilizar, aceder ao portal e executar quaisquer operações no Azure. </br></br>**Importante:** a conta utilizada para instalar os agentes deve ser uma conta escolar ou profissional. Não pode ser uma conta Microsoft. Para obter mais informações, consulte [Sign up for Azure as an organization](https://docs.microsoft.com/en-us/azure/active-directory/sign-up-organization) (Inscrever-se no Azure como uma organização). |
| O Agente Híbrido do Microsoft Identity Manager está instalado em cada servidor do Serviço MIM de destino | A criação de relatórios híbridos requer que os agentes estejam instalados e configurados nos servidores de destino para receber os dados e disponibilizar as funcionalidades de Monitorização e Análise. </br>|
| Conectividade de saída para os pontos finais de serviço do Azure | Durante a instalação e a execução, o agente precisa de conectividade aos pontos finais de serviço do Azure. Se a conectividade de saída estiver bloqueada através de Firewalls, certifique-se de que os seguintes pontos finais são adicionados à lista de permissões: </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.servicebus.windows.net – Porta: 5671 </li><li>&#42;.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|Conectividade de saída baseada em Endereços IP | Para a filtragem baseada em endereços IP nas firewalls, consulte os [Intervalos de IP do Azure](https://www.microsoft.com/en-us/download/details.aspx?id=41653).|
| A Inspeção de SSL para o tráfego de saída é filtrada ou está desativada | O passo de registo do agente ou as operações de carregamento de dados podem falhar se existir terminação ou inspeção de SSL para o tráfego de saída na camada de rede. |
| Portas da firewall do servidor que executa o agente. |O agente requer que as seguintes portas da firewall estejam abertas para poder comunicar com os pontos finais de serviço do Azure.</br></br><li>Porta TCP 443</li><li>Porta TCP 5671</li> |
| Permitir os seguintes sites se a Segurança Avançada do IE estiver ativada |Se a Segurança Avançada do IE estiver ativada, os seguintes sites têm de ser permitidos no servidor no qual será instalado o agente.</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>O servidor de federação para a sua organização considerado fidedigno pelo Azure Active Directory. Por exemplo: https://sts.contoso.com</li> |
</BR>

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

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Ver relatórios híbridos no Portal do Azure

1.  Inicie sessão no [Portal do Azure](https://portal.azure.com/) com a sua conta de administrador global do inquilino.

2.  Clique no ícone do **Azure Active Directory**.

3.  Selecione o diretório do inquilino na lista de diretórios disponíveis da sua subscrição.

4.  Clique em **Registos de Auditoria**.

5.  Certifique-se de que seleciona o **Serviço MIM** no menu pendente Categoria.

> [!WARNING]
> Pode demorar algum tempo até que os dados de auditoria do Microsoft Identity Manager sejam apresentados no Portal do Azure.

## <a name="stop-creating-hybrid-reports"></a>Parar a criação de relatórios híbridos
Se quiser parar de carregar os dados de auditoria dos relatórios do Microsoft Identity Manager para o Azure Active Directory, desinstale o agente de relatórios híbridos. Utilize a ferramenta **Adicionar ou Remover Programas** do Windows para desinstalar a Criação de Relatórios Híbridos do Microsoft Identity Manager.

## <a name="windows-events-used-for-hybrid-reporting"></a>Eventos do Windows utilizados para a criação de relatórios híbridos
Os eventos gerados pelo Microsoft Identity Manager são registados no Registo de Eventos do Windows e são visíveis no Visualizador de Eventos em: Registos de Serviços e Aplicações – &gt;**Registo de Pedido do Identity Manager**. Todos os pedidos do MIM são exportados como eventos no Registo de Eventos do Windows na estrutura JSON. Isto pode ser exportado para o SIEM.

|Tipo de evento|ID|Detalhes do evento|
|--------------|------|-----------------|
|Informações|4121|Dados do evento do MIM que incluem todos os dados de pedidos.|
|Informações|4137|Extensão 4121 do evento do MIM, no caso de existirem demasiados dados para um único evento. O cabeçalho neste evento tem o seguinte formato: `"Request: <GUID> , message <xxx> out of <xxx>`|
