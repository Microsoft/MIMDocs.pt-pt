---
title: Trabalhe com relatórios híbridos em Azure utilizando o Gestor de Identidade 2016 | Microsoft Docs
description: Saiba como combinar dados no local e na nuvem em relatórios híbridos no Azure e como gerir e ver estes relatórios.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 2/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: 0b0a3adcf1651f6a431eec02488e0ab75347b3ca
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835579"
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Trabalhar com relatórios híbridos em Gestor de Identidade

Este artigo discute como combinar dados no local e em nuvem em relatórios híbridos em Azure, e como gerir e ver estes relatórios.

## <a name="available-hybrid-reports"></a>Relatórios híbridos disponíveis
Os três primeiros relatórios do Microsoft Identity Manager disponíveis no Azure Ative Directory (Azure AD) são os seguintes:

- **Atividade de reset da palavra-passe**: Exibe cada instância quando um utilizador executou a palavra-passe reiniciada usando o reset da palavra-passe de autosserviço (SSPR) e fornece os portões ou métodos utilizados para a autenticação.

- **Registo de reset da palavra-passe**: Apresenta cada vez que um utilizador se regista para SSPR e os métodos utilizados para autenticar. Exemplos de métodos podem ser um número de telemóvel ou perguntas e respostas.
   > [!NOTE]
   > Para os relatórios *de registo de redefinição de palavra-passe,* não é feita qualquer diferenciação entre o portão SMS e o portão MFA. Ambos são considerados métodos de telemóvel.

- **Atividade de grupos de self-service**: Exibe cada tentativa feita por alguém para adicioná-lo ou apagar-se de um grupo e criação de grupo.

    ![Imagem da criação de relatórios híbridos do Azure – atividade de reposição de palavra-passe](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * Os relatórios apresentam atualmente dados até um mês de atividade.
> * O anterior agente de reporte híbrido deve ser desinstalado.
> * Para desinstalar relatórios híbridos, desinstale o agente MIMreportingAgent.msi.

## <a name="prerequisites"></a>Pré-requisitos

* Serviço de Gestor de Identidade 2016 SP1, Recomendado construir [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager) .

* Um inquilino Azure AD Premium com um administrador licenciado no seu diretório.

* A conectividade de internet de saída do servidor Identity Manager para O Azure.

## <a name="requirements"></a>Requisitos
Os requisitos para a utilização de relatórios híbridos do Gestor de Identidade estão listados no quadro seguinte:


|                                         Requisito                                         |                                                                                                                                                                                                                                                                                    Description                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                      Azure AD Premium                                       |                                                                                                        A reportagem híbrida é uma funcionalidade Azure AD Premium e requer Azure AD Premium. </br>Para mais informações, consulte [Começar com a Azure AD Premium.](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium) </br>Obtenha um [teste gratuito de 30 dias do Azure AD Premium](https://azure.microsoft.com/trial/get-started-active-directory/).                                                                                                         |
|                     Você deve ser um administrador global do seu AD Azure                     |                                                                   Por padrão, apenas os administradores globais podem instalar e configurar os agentes para iniciar, aceder ao portal e realizar quaisquer operações dentro do Azure. </br>**Importante**: A conta que utiliza quando instala os agentes deve ser uma conta de trabalho ou escola. Não pode ser uma conta Microsoft. Para mais informações, consulte [Inscrever-se no Azure como uma organização.](https://docs.microsoft.com/azure/active-directory/sign-up-organization)                                                                   |
| O Agente Híbrido gestor de identidade é instalado em cada servidor de Serviço de Gestor de Identidade direcionado |                                                                                                                                                                                                       Para receber os dados e fornecer capacidades de monitorização e análise, o relatório híbrido requer que os agentes sejam instalados e configurados em servidores direcionados.  </br>                                                                                                                                                                                                       |
|                    Conectividade de saída para os pontos finais de serviço do Azure                     | Durante a instalação e a execução, o agente precisa de conectividade aos pontos finais de serviço do Azure. Se a conectividade de saída for bloqueada por firewalls, certifique-se de que os seguintes pontos finais são adicionados à lista permitida:<ul><li>\*.blob.core.windows.net </li><li>\*.servicebus.windows.net - Porto: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li><https://management.azure.com> </li><li><https://policykeyservice.dc.ad.msft.net/></li><li><https://login.windows.net></li><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li></ul> |
|                         Conectividade de saída com base em endereços IP                         |                                                                                                                                                                                                                      Para obter endereços IP com base na filtragem em firewalls, consulte as [gamas Azure IP](https://www.microsoft.com/download/details.aspx?id=41653).                                                                                                                                                                                                                      |
|                 A inspeção SSL para tráfego de saída é filtrada ou desativada                 |                                                                                                                                                                                                               A etapa de registo do agente ou as operações de upload de dados podem falhar se houver inspeção SSL ou rescisão para tráfego de saída na camada de rede.                                                                                                                                                                                                                |
|                      Firewall portas no servidor que executa o agente                       |                                                                                                                                                                                                          Para comunicar com os pontos finais do serviço Azure, o agente exige que as seguintes portas de firewall estejam abertas:<ul><li>Porta TCP 443</li><li>Porta TCP 5671</li></ul>                                                                                                                                                                                                          |
|          Permitir certos sites se o Internet Explorer melhorar a segurança estiver ativado           |                                                                                Se a segurança reforçada do Internet Explorer estiver ativada, devem ser permitidos no servidor que o agente tenha instalado:<ul><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li><li><https://login.windows.net></li><li>O servidor da federação para a sua organização confiado pelo Azure Ative Directory (por exemplo, <https://sts.contoso.com> ).</li></ul>                                                                                |

</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Instalar agente de relatórios de gestor de identidade em Azure AD
Após a instalação do Agente de Relatório, os dados da atividade do Gestor de Identidade são exportados de Gestor de Identidade para Registo de Eventos do Windows. O Agente de Relatórios do Gestor de Identidade processa os eventos e depois envia-os para o Azure. No Azure, os eventos são analisados, desencriptados e filtrados para os relatórios necessários.

1.  Instalar Gestor de Identidade 2016.

2.  Baixar o Agente de Relatórios do Gestor de Identidade e, em seguida, fazer o seguinte:

    a. Inscreva-se no portal de gestão AD Azure e, em seguida, selecione **Ative Directory**.

    b. Clique duas vezes no diretório para o qual é administrador global e tenha uma subscrição AZure AD Premium.

    c. Selecione **Configuração** e, em seguida, baixe o Agente de Reportagem.

3.  Instalar o Agente de Relatórios fazendo o seguinte:

    a.  Descarregue o [ ficheiroMIMHReportingAgentSetup.exe](https://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) para o servidor Identity Manager Service.

    b.  Execute o `MIMHReportingAgentSetup.exe`. 

    c.  Execute o instalador do agente.

    d.  Certifique-se de que o serviço de agente de relatórios do Gestor de Identidade está a funcionar.

    e.  Reiniciar o serviço de Gestor de Identidade.

4.  Verifique se o Agente de Relatórios do Gestor de Identidade está a trabalhar no Azure.

    Pode criar dados de relatório utilizando o portal de redefinição da palavra-passe do Gestor de Identidade para redefinir a palavra-passe de um utilizador. Certifique-se de que o reset da palavra-passe foi concluído com sucesso e, em seguida, verifique se os dados são apresentados no portal de gestão AZure AD.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Ver relatórios híbridos no portal Azure

1.  Inscreva-se no [portal Azure](https://portal.azure.com/) com a sua conta de administrador global para o inquilino.

2.  Selecione **Azure Active Directory**.

3.  Na lista de diretórios disponíveis para a sua subscrição, selecione o diretório de inquilinos.

4.  Selecione **Registos de Auditoria**.

5.  Na  lista de categorias, certifique-se de que o **Serviço MIM** está selecionado.

> [!IMPORTANT]
> Pode levar algum tempo para que os dados de auditoria do Gestor de Identidade apareçam no portal Azure.

## <a name="stop-creating-hybrid-reports"></a>Parar a criação de relatórios híbridos
Se quiser parar de enviar dados de auditoria de relatórios do Gestor de Identidade para Azure AD, desinstale o Agente de Relatórios Híbridos. Utilize a ferramenta Programas de Adicionar ou Remover programas do Windows para desinstalar relatórios híbridos do Gestor de Identidade.

## <a name="windows-events-used-for-hybrid-reporting"></a>Eventos do Windows utilizados para a criação de relatórios híbridos
Os eventos gerados pelo Gestor de Identidade são armazenados no Registo de Eventos do Windows. Pode ver os eventos no Visualizador de **Eventos** selecionando registos de pedido de registo de pedido de registo de registos de **aplicações e serviços.**  >   Cada pedido de Gestor de Identidade é exportado como um evento no Windows Event Log na estrutura JSON. Pode exportar o resultado para o seu sistema de informação de segurança e gestão de eventos (SIEM).

|Tipo de evento|ID|Detalhes do evento|
|--------------|------|-----------------|
|Informações|4121|Os dados do evento do Gestor de Identidade que inclui todos os dados do pedido.|
|Informações|4137|A extensão do evento 4121 do Gestor de Identidade, se houver demasiados dados para um único evento. O cabeçalho deste evento é apresentado no seguinte formato: `"Request: <GUID> , message <xxx> out of <xxx>` .|
