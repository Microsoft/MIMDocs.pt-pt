---
title: Trabalhar com os relatórios híbridos no Azure utilizando o Identity Manager 2016 | Documentos da Microsoft
description: Saiba como combinar dados no local e na cloud em relatórios híbridos no Azure e como gerir e ver estes relatórios.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 2/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: 18e4127b1d854a53734142bb58442627619491ef
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358538"
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Trabalhar com relatórios no Identity Manager híbridos

Este artigo discute como combinar no local e na cloud dados em relatórios híbridos no Azure e como gerir e ver estes relatórios.

## <a name="available-hybrid-reports"></a>Relatórios híbridos disponíveis
Os primeiros três relatórios do Microsoft Identity Manager disponíveis no Azure Active Directory (Azure AD) são os seguintes:

- **Atividade de reposição de palavra-passe**: apresenta todas as instâncias, quando um utilizador efetuou a reposição de reposição de palavra-passe self-service (SSPR) a utilizar palavra-passe e fornece as portas ou métodos usados para autenticação.

- **Registo de reposição de palavra-passe**: apresenta sempre que um utilizador regista para SSPR e os métodos utilizados para autenticar. Exemplos de métodos podem ser um número de telemóvel ou perguntas e respostas.
   > [!NOTE]
   > Para *registo de reposição de palavra-passe* de relatórios, é efetuada sem qualquer diferenciação entre a porta de SMS e a porta MFA. Ambas são consideradas métodos de telefonia móvel.

- **Atividade de grupos Self-Service**: apresenta cada tentativa feita por alguém adicionar ou eliminar si próprio de um grupo e a criação do grupo.

    ![Imagem da criação de relatórios híbridos do Azure – atividade de reposição de palavra-passe](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * Os relatórios apresentam atualmente os dados de atividade até um mês.
> * O agente de relatórios de híbrido anterior. tem de ser desinstalado.
> * Para desinstalar os relatórios híbridos, desinstale o agente Mimreportingagent.

## <a name="prerequisites"></a>Pré-requisitos

* Serviço de Gestor de identidade do Identity Manager 2016 SP1, compilação recomendado [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager) .

* Um inquilino do Azure AD Premium com um administrador licenciado no seu diretório.

* Conectividade de internet do servidor do Identity Manager para o Azure de saída.

## <a name="requirements"></a>Requisitos
Os requisitos para utilizar a criação de relatórios de híbridos do Identity Manager estão listados na tabela a seguir:


|                                         Requisito                                         |                                                                                                                                                                                                                                                                                    Descrição                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                      Azure AD Premium                                       |                                                                                                        Criação de relatórios híbridos é uma funcionalidade do Azure AD Premium e requer o Azure AD Premium. </br>Para obter mais informações, consulte [introdução ao Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium). </br>Obter um [avaliação gratuita de 30 dias do Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/).                                                                                                         |
|                     Tem de ser um administrador global do seu Azure AD                     |                                                                   Por predefinição, apenas os administradores globais podem instalar e configurar os agentes para começar a utilizar, aceder ao portal e executar operações no Azure. </br>**Importante**: A conta que utiliza ao instalar os agentes tem de ser uma conta escolar ou profissional. Não pode ser uma conta Microsoft. Para obter mais informações, consulte [Inscreva-se para o Azure como uma organização](https://docs.microsoft.com/azure/active-directory/sign-up-organization).                                                                   |
| Agente de híbridos do Identity Manager está instalado em cada servidor de destino serviço Identity Manager |                                                                                                                                                                                                       Para receber os dados e fornecem capacidades de monitorização e análise, criação de relatórios híbridos requer que os agentes ser instalado e configurado nos servidores de destino.  </br>                                                                                                                                                                                                       |
|                    Conectividade de saída para os pontos finais de serviço do Azure                     | Durante a instalação e a execução, o agente precisa de conectividade aos pontos finais de serviço do Azure. Se a conectividade de saída está bloqueada por firewalls, certifique-se de que os pontos finais seguintes são adicionados à lista de permitidos:<ul><li>\*.blob.core.windows.net </li><li>\*. servicebus.windows.net - porta: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li><https://management.azure.com> </li><li><https://policykeyservice.dc.ad.msft.net/></li><li><https://login.windows.net></li><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li></ul> |
|                         Conectividade de saída com base em endereços IP                         |                                                                                                                                                                                                                      Para endereços IP a filtragem com base nos firewalls, consulte a [intervalos de IP do Azure](https://www.microsoft.com/download/details.aspx?id=41653).                                                                                                                                                                                                                      |
|                 Inspeção do SSL para tráfego de saída é filtrada ou está desativada                 |                                                                                                                                                                                                               Os agente passo ou dados de carregamento de operações de registo podem falhar se houver inspeção do SSL ou término para tráfego de saída na camada da rede.                                                                                                                                                                                                                |
|                      Portas de firewall no servidor que executa o agente                       |                                                                                                                                                                                                          Para comunicar com os pontos finais de serviço do Azure, o agente requer as seguintes portas de firewall estejam abertas:<ul><li>Porta TCP 443</li><li>Porta TCP 5671</li></ul>                                                                                                                                                                                                          |
|          Permitir que determinados de Web sites se segurança avançada do Internet Explorer está ativado           |                                                                                Se avançada do Internet Explorer segurança estiver ativada, os Web sites seguintes têm de ser permitidos no servidor que tem instalado o agente:<ul><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li><li><https://login.windows.net></li><li>O servidor de Federação para a sua organização considerado fidedigno pelo Azure Active Directory (por exemplo, <https://sts.contoso.com>).</li></ul>                                                                                |

</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Instalar o agente de relatórios do Identity Manager no Azure AD
Após a instalação do agente de relatórios, os dados da atividade do Identity Manager são exportados do Identity Manager para o registo de eventos do Windows. Agente de relatórios do Identity Manager processa os eventos e, em seguida, carrega-os para o Azure. No Azure, os eventos são analisados, desencriptados e filtrados para os relatórios necessários.

1.  Instale o Identity Manager 2016.

2.  Transferir o agente de relatórios do Identity Manager e, em seguida, faça o seguinte:

    a. Inicie sessão no portal de gestão do Azure AD e, em seguida, selecione **do Active Directory**.

    b. Faça duplo clique no diretório para o qual for um administrador global e tem uma subscrição do Azure AD Premium.

    c. Selecione **configuração**e, em seguida, transfira o agente de relatórios.

3.  Instale o agente de relatórios, fazendo o seguinte:

    a.  Transfira o [MIMHReportingAgentSetup.exe ficheiro](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) para o servidor do Identity Manager Service.

    b.  Executar `MIMHReportingAgentSetup.exe`. 

    c.  Execute o instalador do agente.

    d.  Certifique-se de que o serviço de agente de relatórios do Identity Manager está em execução.

    e.  Reinicie o serviço do Identity Manager.

4.  Certifique-se de que o agente de relatórios do Identity Manager está a funcionar no Azure.

    É possível criar o relatório de portal para repor a palavra-passe de um utilizador da reposição de dados com a senha de autoatendimento do Identity Manager. Certifique-se de que a reposição de palavra-passe foi concluída com êxito e, em seguida, verifique para se certificar de que os dados são apresentados no portal de gestão do Azure AD.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Ver relatórios híbridos no portal do Azure

1.  Inicie sessão para o [portal do Azure](https://portal.azure.com/) com a sua conta de administrador global do inquilino.

2.  Selecione **do Azure Active Directory**.

3.  Na lista de diretórios disponíveis para a sua subscrição, selecione o diretório do inquilino.

4.  Selecione **registos de auditoria**.

5.  Na **categoria** pendente lista, certifique-se de que **serviço MIM** está selecionada.

> [!IMPORTANT]
> Pode demorar algum tempo para o Identity Manager dados de auditoria para aparecer no portal do Azure.

## <a name="stop-creating-hybrid-reports"></a>Parar a criação de relatórios híbridos
Se pretender parar de carregar os relatórios de auditoria de dados do Identity Manager para o Azure AD, desinstale o agente de relatórios híbridos. Utilize a ferramenta Windows adicionar ou remover programas para desinstalar a criação de relatórios de híbridos do Identity Manager.

## <a name="windows-events-used-for-hybrid-reporting"></a>Eventos do Windows utilizados para a criação de relatórios híbridos
Eventos que são gerados pelo Gestor de identidade são armazenados no registo de eventos do Windows. Pode ver os eventos na **Visualizador de eventos** ao selecionar **registos de aplicações e serviços** > **registo de pedido do Identity Manager**. Cada pedido do Identity Manager é exportado como um evento no registo de eventos do Windows na estrutura JSON. Pode exportar o resultado para o seu sistema de gerenciamento (SIEM) de informações e eventos da segurança.

|Tipo de evento|ID|Detalhes do evento|
|--------------|------|-----------------|
|Informações|4121|Os dados de eventos do Identity Manager, que incluem todos os dados de pedido.|
|Informações|4137|A Identity Manager extensão 4121 do evento, se houver muitos dados para um único evento. O cabeçalho neste evento, é apresentado no seguinte formato: `"Request: <GUID> , message <xxx> out of <xxx>`.|
