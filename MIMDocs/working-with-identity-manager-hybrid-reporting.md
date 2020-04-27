---
title: Trabalhar com reportagem híbrida em Azure através do Identity Manager 2016 [ Gestor de Identidade 2016] Microsoft Docs
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
ms.openlocfilehash: fd0efd3e3d5c42f4b67d0abd42f6dab8254573e5
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044349"
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Trabalhar com reporte híbrido em Gestor de Identidade

Este artigo discute como combinar no local e dados em nuvem em relatórios híbridos em Azure, e como gerir e ver estes relatórios.

## <a name="available-hybrid-reports"></a>Relatórios híbridos disponíveis
Os três primeiros relatórios do Microsoft Identity Manager disponíveis no Azure Ative Directory (Azure AD) são os seguintes:

- Atividade de reposição de **palavra-passe**: Mostra cada instância quando um utilizador executou o reset da palavra-passe utilizando o reset de palavra-passe self-service (SSPR) e fornece os portões ou métodos utilizados para a autenticação.

- **Registo de redefinição de palavra-passe**: Mostra cada vez que um utilizador se regista para SSPR e os métodos utilizados para autenticar. Exemplos de métodos podem ser um número de telemóvel ou perguntas e respostas.
   > [!NOTE]
   > Para *redefinir* relatórios de registo de passwords, não é feita diferenciação entre o portão SMS e o portão MFA. Ambos são considerados métodos de telemóvel.

- **Atividade de grupos de self-service**: Exibe cada tentativa feita por alguém para adicioná-lo ou apagar-se de uma criação de grupo e grupo.

    ![Imagem da criação de relatórios híbridos do Azure – atividade de reposição de palavra-passe](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * Os relatórios apresentam atualmente dados para um mês de atividade.
> * O anterior Agente de Informação Híbrido deve ser desinstalado.
> * Para desinstalar relatórios híbridos, desinstale o agente MIMreportingAgent.msi.

## <a name="prerequisites"></a>Pré-requisitos

* Gestor de Identidade 2016 Serviço de Gestor de Identidade SP1, Recomendado construir [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager) .

* Um inquilino Azure AD Premium com um administrador licenciado no seu diretório.

* Conectividade de internet de saída do servidor Do Gestor de Identidade para o Azure.

## <a name="requirements"></a>Requisitos
Os requisitos para a utilização de relatórios híbridos do Gestor de Identidade estão listados na tabela seguinte:


|                                         Requisito                                         |                                                                                                                                                                                                                                                                                    Descrição                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                      Azure AD Premium                                       |                                                                                                        O reporte híbrido é uma funcionalidade Azure AD Premium e requer Azure AD Premium. </br>Para mais informações, consulte [Começar com o Azure AD Premium](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium). </br>Obtenha um [teste gratuito de 30 dias do Azure AD Premium.](https://azure.microsoft.com/trial/get-started-active-directory/)                                                                                                         |
|                     Deve ser administrador global do seu Anúncio Azure                     |                                                                   Por padrão, apenas os administradores globais podem instalar e configurar os agentes para começar, aceder ao portal e realizar quaisquer operações dentro do Azure. </br>**Importante**: A conta que utiliza quando instala os agentes deve ser uma conta de trabalho ou de escola. Não pode ser uma conta Microsoft. Para mais informações, consulte [o Signup para o Azure como uma organização](https://docs.microsoft.com/azure/active-directory/sign-up-organization).                                                                   |
| O Agente Híbrido do Gestor de Identidade está instalado em cada servidor de Serviço de Gestor de Identidade direcionado |                                                                                                                                                                                                       Para receber os dados e fornecer capacidades de monitorização e análise, o relatório híbrido requer que os agentes sejam instalados e configurados em servidores direcionados.  </br>                                                                                                                                                                                                       |
|                    Conectividade de saída para os pontos finais de serviço do Azure                     | Durante a instalação e a execução, o agente precisa de conectividade aos pontos finais de serviço do Azure. Se a conectividade de saída for bloqueada por firewalls, certifique-se de que os seguintes pontos finais são adicionados à lista permitida:<ul><li>\*.blob.core.windows.net </li><li>\*.servicebus.windows.net - Porto: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li><https://management.azure.com> </li><li><https://policykeyservice.dc.ad.msft.net/></li><li><https://login.windows.net></li><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li></ul> |
|                         Conectividade de saída com base em endereços IP                         |                                                                                                                                                                                                                      Para endereços IP filtrando em firewalls, consulte as [gamas IP Azure](https://www.microsoft.com/download/details.aspx?id=41653).                                                                                                                                                                                                                      |
|                 A inspeção SSL para tráfego de saída é filtrada ou desativada                 |                                                                                                                                                                                                               A etapa de registo do agente ou as operações de upload de dados podem falhar se houver inspeção ou rescisão ssl para tráfego de saída na camada de rede.                                                                                                                                                                                                                |
|                      Portas de firewall no servidor que executa o agente                       |                                                                                                                                                                                                          Para comunicar com os pontos finais do serviço Azure, o agente exige que as seguintes portas de firewall sejam abertas:<ul><li>Porta TCP 443</li><li>Porta TCP 5671</li></ul>                                                                                                                                                                                                          |
|          Permitir certos sites se a segurança reforçada do Internet Explorer estiver ativada           |                                                                                Se a segurança reforçada do Internet Explorer estiver ativada, os seguintes websites devem ser autorizados no servidor que tem o agente instalado:<ul><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li><li><https://login.windows.net></li><li>O servidor da federação para a sua organização confiado pelo Azure Ative Directory (por exemplo, <https://sts.contoso.com>).</li></ul>                                                                                |

</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Instale o Agente de Informação do Gestor de Identidade em Azure AD
Após a instalação do Agente de Informação, os dados da atividade do Gestor de Identidade são exportados do Gestor de Identidade para o Windows Event Log. O Agente de Relatórios do Gestor de Identidade processa os eventos e depois envia-os para o Azure. No Azure, os eventos são analisados, desencriptados e filtrados para os relatórios necessários.

1.  Instale o Gestor de Identidade 2016.

2.  Baixe o Agente de Informação do Gestor de Identidade e, em seguida, faça o seguinte:

    a. Inscreva-se no portal de gestão da AD Azure e, em seguida, **selecione Ative Directory**.

    b. Clique duas vezes no diretório para o qual é administrador global e tem uma subscrição Azure AD Premium.

    c. **Selecione Configuração**e, em seguida, baixe o Reporting Agent.

3.  Instale o Agente de Informação fazendo o seguinte:

    a.  Descarregue o [ficheiro MIMHReportingAgentSetup.exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) para o servidor do Serviço de Gestor de Identidade.

    b.  Execute `MIMHReportingAgentSetup.exe`. 

    c.  Execute o instalador do agente.

    d.  Certifique-se de que o serviço de agente de informação do Gestor de Identidade está em funcionamento.

    e.  Reinicie o serviço de Gestor de Identidade.

4.  Verifique se o Agente de Informação do Gestor de Identidade está a trabalhar no Azure.

    Pode criar dados de relatório utilizando o portal de redefinição de palavra-passe self-service Do Gestor de Identidade para redefinir a palavra-passe de um utilizador. Certifique-se de que o reset da palavra-passe foi concluído com sucesso e, em seguida, verifique para se certificar de que os dados são apresentados no portal de gestão da AD Azure.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Ver relatórios híbridos no portal Azure

1.  Inscreva-se no [portal Azure](https://portal.azure.com/) com a sua conta de administrador global para o inquilino.

2.  Selecione **Azure Active Directory**.

3.  Na lista de diretórios disponíveis para a sua subscrição, selecione o diretório de inquilinos.

4.  Selecione **Registos de Auditoria**.

5.  Na lista de abandono da **categoria,** certifique-se de que o **Serviço MIM** é selecionado.

> [!IMPORTANT]
> Pode levar algum tempo para que os dados de auditoria do Gestor de Identidade apareçam no portal Azure.

## <a name="stop-creating-hybrid-reports"></a>Parar a criação de relatórios híbridos
Se pretender parar de enviar dados de auditoria de reporte do Identity Manager para o Azure AD, desinstale o Agente de Informação Híbrido. Utilize a ferramenta Windows Add ou Remove Programs para desinstalar relatórios híbridos do Gestor de Identidade.

## <a name="windows-events-used-for-hybrid-reporting"></a>Eventos do Windows utilizados para a criação de relatórios híbridos
Os eventos gerados pelo Identity Manager são armazenados no Windows Event Log. Pode ver os eventos no Espectador de **Eventos** selecionando Registo de Pedido de**Pedido**de Pedido de Gestor de Identidade de **Aplicação e Serviços** > . Cada pedido do Gestor de Identidade é exportado como um evento no Windows Event Log na estrutura JSON. Pode exportar o resultado para o seu sistema de informação de segurança e gestão de eventos (SIEM).

|Tipo de evento|ID|Detalhes do evento|
|--------------|------|-----------------|
|Informações|4121|Os dados do evento Do Gestor de Identidade que incluem todos os dados do pedido.|
|Informações|4137|A extensão do Gestor de Identidade 4121, se houver demasiados dados para um único evento. O cabeçalho neste evento é apresentado `"Request: <GUID> , message <xxx> out of <xxx>`no seguinte formato: .|
