---
title: Trabalhar com relatórios híbridos no Azure usando o Identity Manager 2016 | Microsoft Docs
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
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "64517503"
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Trabalhar com relatórios híbridos no Identity Manager

Este artigo discute como combinar dados locais e na nuvem em relatórios híbridos no Azure e como gerenciar e exibir esses relatórios.

## <a name="available-hybrid-reports"></a>Relatórios híbridos disponíveis
Os três primeiros Microsoft Identity Manager relatórios disponíveis no Azure Active Directory (AD do Azure) são os seguintes:

- **Atividade de redefinição de senha**: exibe cada instância quando um usuário realizou a redefinição de senha usando a redefinição de senha de autoatendimento (SSPR) e fornece os Gates ou métodos usados para autenticação.

- **Registro de redefinição de senha**: exibe cada vez que um usuário se registra para SSPR e os métodos usados para autenticação. Exemplos de métodos podem ser um número de telefone celular ou perguntas e respostas.
   > [!NOTE]
   > Para relatórios de *registro de redefinição de senha* , nenhuma diferenciação é feita entre a porta do SMS e a porta MFA. Ambos são considerados métodos de telefone celular.

- **Atividade de grupos de autoatendimento**: exibe cada tentativa feita por alguém de adicioná-la ou excluí-la de uma criação de grupo e grupo.

    ![Imagem da criação de relatórios híbridos do Azure – atividade de reposição de palavra-passe](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * Os relatórios atualmente apresentam dados de até um mês de atividade.
> * O agente de relatório híbrido anterior deve ser desinstalado.
> * Para desinstalar os relatórios híbridos, desinstale o agente agente mimreportingagent. msi.

## <a name="prerequisites"></a>Pré-requisitos

* Serviço Gerenciador de identidade do Identity Manager 2016 SP1, compilação recomendada [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager) .

* Um locatário Azure AD Premium com um administrador licenciado em seu diretório.

* Conectividade de Internet de saída do servidor do Identity Manager para o Azure.

## <a name="requirements"></a>Requisitos
Os requisitos para usar o relatório híbrido do Identity Manager estão listados na tabela a seguir:


|                                         Requisito                                         |                                                                                                                                                                                                                                                                                    Description                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                      Azure AD Premium                                       |                                                                                                        O relatório híbrido é um recurso Azure AD Premium e requer Azure AD Premium. </br>Para obter mais informações, consulte [introdução ao Azure ad Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium). </br>Obtenha uma [avaliação gratuita de 30 dias do Azure ad Premium](https://azure.microsoft.com/trial/get-started-active-directory/).                                                                                                         |
|                     Você deve ser um administrador global do Azure AD                     |                                                                   Por padrão, somente os administradores globais podem instalar e configurar os agentes para começar, acessar o portal e executar qualquer operação no Azure. </br>**Importante**: a conta que você usa ao instalar os agentes deve ser uma conta corporativa ou de estudante. Não pode ser uma conta Microsoft. Para obter mais informações, consulte [inscrever-se no Azure como uma organização](https://docs.microsoft.com/azure/active-directory/sign-up-organization).                                                                   |
| O agente híbrido do Identity Manager está instalado em cada servidor de serviço do Identity Manager de destino |                                                                                                                                                                                                       Para receber os dados e fornecer recursos de monitoramento e análise, os relatórios híbridos exigem que os agentes sejam instalados e configurados nos servidores de destino.  </br>                                                                                                                                                                                                       |
|                    Conectividade de saída para os pontos finais de serviço do Azure                     | Durante a instalação e a execução, o agente precisa de conectividade aos pontos finais de serviço do Azure. Se a conectividade de saída estiver bloqueada por firewalls, verifique se os seguintes pontos de extremidade foram adicionados à lista de permissões:<ul><li>\*.blob.core.windows.net </li><li>\*. servicebus.windows.net-porta: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li><https://management.azure.com> </li><li><https://policykeyservice.dc.ad.msft.net/></li><li><https://login.windows.net></li><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li></ul> |
|                         Conectividade de saída com base em endereços IP                         |                                                                                                                                                                                                                      Para a filtragem baseada em endereços IP em firewalls, consulte os [intervalos de IP do Azure](https://www.microsoft.com/download/details.aspx?id=41653).                                                                                                                                                                                                                      |
|                 A inspeção SSL para o tráfego de saída é filtrada ou desabilitada                 |                                                                                                                                                                                                               A etapa de registro do agente ou as operações de carregamento de dados poderão falhar se houver inspeção ou término de SSL para o tráfego de saída na camada de rede.                                                                                                                                                                                                                |
|                      Portas de firewall no servidor que executa o agente                       |                                                                                                                                                                                                          Para se comunicar com os pontos de extremidade de serviço do Azure, o agente requer que as seguintes portas de firewall sejam abertas:<ul><li>Porta TCP 443</li><li>Porta TCP 5671</li></ul>                                                                                                                                                                                                          |
|          Permitir determinados sites se a segurança aprimorada do Internet Explorer estiver habilitada           |                                                                                Se a segurança aprimorada do Internet Explorer estiver habilitada, os sites a seguir deverão ser permitidos no servidor que tem o agente instalado:<ul><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li><li><https://login.windows.net></li><li>O servidor de Federação da sua organização confiável por Azure Active Directory (por exemplo, <https://sts.contoso.com>).</li></ul>                                                                                |

</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Instalar o agente de relatório do Identity Manager no Azure AD
Após a instalação do agente de relatório, os dados da atividade do Identity Manager são exportados do Identity Manager para o log de eventos do Windows. O agente de relatório do Identity Manager processa os eventos e os carrega no Azure. No Azure, os eventos são analisados, desencriptados e filtrados para os relatórios necessários.

1.  Instale o Identity Manager 2016.

2.  Baixe o agente de relatório do Identity Manager e, em seguida, faça o seguinte:

    a. Entre no portal de gerenciamento do Azure AD e selecione **Active Directory**.

    b. Clique duas vezes no diretório para o qual você é um administrador global e tem uma assinatura Azure AD Premium.

    c. Selecione **configuração**e baixe agente de relatório.

3.  Instale o agente de relatório fazendo o seguinte:

    a.  Baixe o [arquivo MIMHReportingAgentSetup. exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) para o servidor de serviço do Identity Manager.

    b.  Executar `MIMHReportingAgentSetup.exe`. 

    c.  Execute o instalador do agente.

    d.  Verifique se o serviço agente de relatório do Identity Manager está em execução.

    e.  Reinicie o serviço Identity Manager.

4.  Verifique se o agente de relatório do Identity Manager está funcionando no Azure.

    Você pode criar dados de relatório usando o portal de redefinição de senha de autoatendimento do Identity Manager para redefinir a senha de um usuário. Verifique se a redefinição de senha foi concluída com êxito e, em seguida, verifique se os dados são exibidos no portal de gerenciamento do Azure AD.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Exibir relatórios híbridos no portal do Azure

1.  Entre no [portal do Azure](https://portal.azure.com/) com sua conta de administrador global para o locatário.

2.  Selecione **Azure Active Directory**.

3.  Na lista de diretórios disponíveis para sua assinatura, selecione o diretório do locatário.

4.  Selecione **Registos de Auditoria**.

5.  Na lista suspensa **categoria** , verifique se o **serviço mim** está selecionado.

> [!IMPORTANT]
> Pode levar algum tempo para que os dados de auditoria do Identity Manager apareçam no portal do Azure.

## <a name="stop-creating-hybrid-reports"></a>Parar a criação de relatórios híbridos
Se você quiser parar de carregar dados de auditoria de relatórios do Identity Manager para o Azure AD, desinstale o agente de relatório híbrido. Use a ferramenta Adicionar ou remover programas do Windows para desinstalar o relatório híbrido do Identity Manager.

## <a name="windows-events-used-for-hybrid-reporting"></a>Eventos do Windows utilizados para a criação de relatórios híbridos
Os eventos gerados pelo Identity Manager são armazenados no log de eventos do Windows. Você pode exibir os eventos na **Visualizador de eventos** selecionando **logs de aplicativos e serviços** > **log de solicitações do Identity Manager**. Cada solicitação do Identity Manager é exportada como um evento no log de eventos do Windows na estrutura JSON. Você pode exportar o resultado para o sistema SIEM (gerenciamento de eventos e informações de segurança).

|Tipo de evento|ID|Detalhes do evento|
|--------------|------|-----------------|
|Informações|4121|Os dados de evento do Identity Manager que incluem todos os dados de solicitação.|
|Informações|4137|A extensão do evento 4121 do Identity Manager, se houver muitos dados para um único evento. O cabeçalho nesse evento é exibido no seguinte formato: `"Request: <GUID> , message <xxx> out of <xxx>`.|
