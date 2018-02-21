---
title: "Trabalhar com relatórios híbridos no Azure através do Identity Manager 2016 | Microsoft Docs"
description: "Saiba como combinar dados no local e na cloud em relatórios híbridos no Azure e como gerir e ver estes relatórios."
keywords: 
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 2/20/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: e135cc5066220765d97568b3a1e1b984a876b2a2
ms.sourcegitcommit: b4a39928c5fa1d7718046563c0809bcbf11d833d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/20/2018
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Trabalhar com relatórios no Gestor de identidade híbridos

Este artigo descreve como combinar no local e da nuvem em relatórios híbridos no Azure e como gerir e ver estes relatórios.

## <a name="available-hybrid-reports"></a>Relatórios híbridos disponíveis
Os primeiros três relatórios do Microsoft Identity Manager disponíveis no Azure Active Directory (Azure AD) são os seguintes:

- **Atividade de reposição de palavra-passe**: apresenta todas as instâncias em que um utilizador efetuou a utilizar a reposição de palavra-passe self-service (SSPR) de reposição de palavra-passe e fornece as portas ou métodos utilizados para autenticação.

- **Registo de reposição de palavra-passe**: apresenta sempre que um utilizador se regista SSPR e os métodos utilizados para autenticar. Exemplos de métodos poderão ser um número de telemóvel ou perguntas e respostas.
   > [!NOTE]
   > Para *registo de reposição de palavra-passe* relatórios, é efetuada qualquer diferenciação entre a porta de SMS e a porta de MFA. Ambas são consideradas métodos de telemóvel.

- **Atividade de grupos Self-Service**: apresenta cada tentativa de vezes que uma pessoa para adicionar ou eliminá-lo ou herself de um grupo e criação de grupo.

    ![Imagem da criação de relatórios híbridos do Azure – atividade de reposição de palavra-passe](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * Os relatórios atualmente apresentam dados até um mês de atividade.
> * O agente de relatórios de híbridos anterior tem de ser desinstalado.
> * Para desinstalar a criação de relatórios híbridos, desinstale o agente MIMreportingAgent.msi.

## <a name="prerequisites"></a>Pré-requisitos

* Serviço do Identity Manager 2016 SP1 Identity Manager, compilação recomendado [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager) .

* Um inquilino do Azure AD Premium com um administrador licenciado no diretório.

* Ligação de internet do servidor do Identity Manager para o Azure de saída.

## <a name="requirements"></a>Requisitos
Os requisitos para utilizar a criação de relatórios de híbridos do Identity Manager estão listados na seguinte tabela:

| Requisito | Descrição |
| --- | --- |
| Azure AD Premium | Criação de relatórios híbridos é uma funcionalidade do Azure AD Premium e requer o Azure AD Premium. </br>Para obter mais informações, consulte [introdução ao Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium). </br>Obter um [avaliação gratuita de 30 dias do Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/). |
| Tem de ser um administrador global do seu Azure AD |Por predefinição, apenas os administradores globais podem instalar e configurar os agentes, começar a utilizar, aceder ao portal e executar operações no Azure. </br>**Importante**: A conta que utiliza ao instalar os agentes deve ser uma conta escolar ou profissional. Não pode ser uma conta Microsoft. Para obter mais informações, consulte [inscrever-se no Azure como uma organização](https://docs.microsoft.com/azure/active-directory/sign-up-organization). |
| Agente de híbridos do Identity Manager está instalado em cada servidor do Identity Manager Service direcionado | Para receber os dados e fornecer capacidades de monitorização e análise, a criação de relatórios híbridos requer os agentes para ser instalada e configurada nos servidores de destino.  </br>|
| Conectividade de saída para os pontos finais de serviço do Azure | Durante a instalação e a execução, o agente precisa de conectividade aos pontos finais de serviço do Azure. Se a conectividade de saída está bloqueada por firewalls, certifique-se de que os seguintes pontos finais são adicionados à lista de permitidos:<ul><li>\*.blob.core.windows.net </li><li>\*.servicebus.windows.net - Port: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li></ul> |
|Conectividade de saída com base em endereços IP | Endereços IP filtragem com base nas firewalls, consulte ao [intervalos de IP do Azure](https://www.microsoft.com/download/details.aspx?id=41653).|
| Inspeção de SSL para o tráfego de saída está filtrada ou desativada | Os registo passo ou dados de carregamento de operações do agente poderão falhar se não houver inspeção de SSL ou terminação para tráfego de saída na camada de rede. |
| Portas de firewall num servidor que executa o agente | Para comunicar com os pontos finais de serviço do Azure, o agente requer as seguintes portas de firewall estejam abertas:<ul><li>Porta TCP 443</li><li>Porta TCP 5671</li></ul> |
| Permitir que determinados de Web sites se a segurança avançada do Internet Explorer está ativado |Se avançada do Internet Explorer é com segurança ativada, os seguintes Web sites tem de ser permitidos no servidor que tenha o agente instalado:<ul><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>O servidor de Federação para a sua organização considerado fidedigno pelo Azure Active Directory (por exemplo, https://sts.contoso.com).</li></ul> |
</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Instalar o agente de relatórios do Identity Manager no Azure AD
Depois de instalar o agente de relatórios, os dados da atividade do Identity Manager são exportados do Identity Manager para o registo de eventos do Windows. Agente de relatórios do Identity Manager processa os eventos e, em seguida, carrega-os para o Azure. No Azure, os eventos são analisados, desencriptados e filtrados para os relatórios necessários.

1.  Instale Identity Manager 2016.

2.  Transferir o agente de relatórios do Identity Manager e, em seguida, efetue o seguinte:

    a. Inicie sessão no portal de gestão do Azure AD e, em seguida, selecione **do Active Directory**.

    b. Faça duplo clique em diretório para os quais um administrador global e tem uma subscrição do Azure AD Premium.

    c. Selecione **configuração**e, em seguida, transfira o agente de relatórios.

3.  Instale o agente de relatórios da seguinte forma:

    a.  Transferir o [MIMHReportingAgentSetup.exe ficheiro](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) para o servidor do Identity Manager Service.

    b.  Executar `MIMHReportingAgentSetup.exe`. 

    c.  Execute o instalador do agente.

    d.  Certifique-se de que o agente de relatórios do Identity Manager está em execução.

    e.  Reinicie o serviço do Identity Manager.

4.  Certifique-se de que o agente de relatórios do Identity Manager está a funcionar no Azure.

    Pode criar relatórios portal para repor a palavra-passe de um utilizador de reposição de dados utilizando a passe self-service do Identity Manager. Certifique-se de que a reposição de palavra-passe foi concluída com êxito e, em seguida, verificar para garantir que os dados são apresentados no portal de gestão do Azure AD.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Ver relatórios híbridos no portal do Azure

1.  Iniciar sessão para o [portal do Azure](https://portal.azure.com/) com a sua conta de administrador global do inquilino.

2.  Selecione **do Azure Active Directory**.

3.  Na lista de diretórios disponíveis da sua subscrição, selecione o diretório do inquilino.

4.  Selecione **registos de auditoria**.

5.  No **categoria** pendente lista, certifique-se de que **serviço MIM** está selecionada.

> [!IMPORTANT]
> Pode demorar algum tempo para Identity Manager dados de auditoria a aparecer no portal do Azure.

## <a name="stop-creating-hybrid-reports"></a>Parar a criação de relatórios híbridos
Se pretender parar de carregar dados de auditoria relatórios do Identity Manager para o Azure AD, desinstale o agente de relatórios híbridos. Utilize a ferramenta Windows adicionar ou remover programas para desinstalar a criação de relatórios de híbridos do Identity Manager.

## <a name="windows-events-used-for-hybrid-reporting"></a>Eventos do Windows utilizados para a criação de relatórios híbridos
Os eventos gerados pelo Identity Manager são armazenados no registo de eventos do Windows. Pode ver eventos de **Visualizador de eventos** selecionando **registos de serviços e aplicações** > **registo de pedido do Identity Manager**. Cada pedido do Identity Manager é exportado como eventos no registo de eventos do Windows na estrutura de JSON. Pode exportar o resultado ao sistema de gestão (SIEM) de segurança as informações e eventos.

|Tipo de evento|ID|Detalhes do evento|
|--------------|------|-----------------|
|Informações|4121|Dados do evento do Identity Manager incluem todos os dados de pedidos.|
|Informações|4137|O Gestor de identidade extensão 4121 do evento, se não houver demasiados dados para um único evento. O cabeçalho neste evento é apresentado no seguinte formato: `"Request: <GUID> , message <xxx> out of <xxx>`.|
