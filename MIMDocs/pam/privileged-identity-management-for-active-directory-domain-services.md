---
title: Privileged Access Management para os Active Directory Domain Services | Documentos da Microsoft
description: Saiba mais sobre Privileged Access Management e como pode ajudar a gerir e proteger o ambiente do Active Directory.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/05/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: cf3796f7-bc68-4cf7-b887-c5b14e855297
ms.reviewer: mwahl
ms.suite: ems
experimental: true
experiment_id: kgremban_images
ms.openlocfilehash: 351a516ccb6a529ca27b157508b06af46f3d243a
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010732"
---
# <a name="privileged-access-management-for-active-directory-domain-services"></a>Privileged Access Management para Serviços de Domínio do Active Directory

A MIM Privileged Access Management (PAM) é uma solução que ajuda as organizações a restringir o acesso privilegiado dentro de um ambiente de Diretório Ativo existente e isolado.

O Privileged Access Management concretiza dois objetivos:

- Restabelecer o controlo sobre um ambiente do Active Directory comprometido, mantendo um ambiente bastion separado, que é conhecido por não ser afetado por ataques maliciosos.
- Isolar a utilização de contas com privilégios para reduzir o risco de roubo dessas credenciais.

> [!NOTE]
> A MIM PAM distingue-se da [Azure Ative Directory Privileged Identity Management](https://azure.microsoft.com/documentation/articles/active-directory-privileged-identity-management-configure/) (PIM). A MIM PAM destina-se a ambientes AD isolados no local. Azure AD PIM é um serviço em Azure AD que lhe permite gerir, controlar e monitorizar o acesso a recursos em Azure AD, Azure e outros Serviços Online da Microsoft, como o Microsoft 365 ou o Microsoft Intune. Para obter orientações sobre ambientes ligados à Internet e ambientes híbridos, consulte [a garantia de acesso privilegiado](/security/compass/overview) para mais informações.

## <a name="what-problems-does-mim-pam-help-solve"></a>Que problemas a MIM PAM ajuda a resolver?

Hoje em dia, é muito fácil para os atacantes obterem credenciais de conta de Domain Admins, e é muito difícil descobrir estes ataques após o facto. O objetivo do PAM é reduzir as oportunidades de utilizadores mal intencionados obterem acesso, aumentando simultaneamente o seu controlo e o conhecimento do ambiente.

O PAM dificulta a capacidade dos atacantes de penetrarem numa rede e obterem acesso a contas com privilégios. O PAM adiciona proteção aos grupos com privilégios que controlam o acesso numa série de computadores associados a um domínio e de aplicações nesses computadores. Além disso, adiciona mais monitorização, mais visibilidade e mais controlos de grãos finos. Isto permite que as organizações vejam quem são os seus administradores privilegiados e o que estão a fazer. O PAM dá às organizações uma visão mais ampla de como as contas administrativas são utilizadas no ambiente.

A abordagem PAM fornecida pela MIM destina-se a ser utilizada numa arquitetura personalizada para ambientes isolados onde o acesso à Internet não esteja disponível, onde esta configuração é exigida por regulamentação, ou em ambientes isolados de alto impacto, como laboratórios de investigação offline e tecnologia operacional desligada ou ambientes de controlo de supervisão e aquisição de dados. Se o seu Ative Directory faz parte de um ambiente ligado à Internet, consulte [a garantia de acesso privilegiado](/security/compass/overview) para obter mais informações sobre por onde começar.

## <a name="setting-up-mim-pam"></a>Criação mim PAM

O PAM baseia-se no princípio de administração just-in-time, que está relacionada com a [administração just enough (JEA)](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DCIM-B362). JEA é um conjunto de ferramentas Windows PowerShell que define um conjunto de comandos para a realização de atividades privilegiadas. É um ponto final onde os administradores podem obter autorização para executar comandos. No JEA, um administrador decide que os utilizadores com um determinado privilégio podem executar uma determinada tarefa. Sempre que um utilizador elegível precisar de realizar essa tarefa, o administrador ativa essa permissão. As permissões expiram após um período de tempo especificado, para que um utilizador mal intencionado não possa roubar o acesso.

A configuração e a operação do PAM tem quatro passos.

![Passos do PAM: preparar, proteger, operar, monitorizar - diagrama](media/MIM_PIM_SetupProcess.png)

1. **Preparar**: identifique quais os grupos na sua floresta existente têm privilégios significativos. Volte a criar estes grupos sem membros na floresta bastion.
2. **Proteger:** Configurar o ciclo de vida e a proteção de autenticação para quando os utilizadores solicitarem a administração just-in-time. 
3. **Operar**: depois de cumpridos os requisitos de autenticação e de um pedido ser aprovado, uma conta de utilizador é adicionada temporariamente a um grupo com privilégios na floresta bastion. Durante um período de tempo predefinido, o administrador tem todos os privilégios e permissões de acesso que são atribuídos a esse grupo. Após esse tempo, a conta é removida do grupo.
4. **Monitorizar**: o PAM adiciona auditoria, alertas e relatórios de pedidos de acesso privilegiado. Pode rever o histórico de acesso privilegiado e ver quem realizou uma atividade. Pode decidir se a atividade é válida ou não e identificar facilmente uma atividade não autorizada, tal como uma tentativa de adicionar um utilizador diretamente a um grupo com privilégios na floresta original. Este passo é importante não só para identificar software malicioso, mas também para controlar atacantes "internos".

## <a name="how-does-mim-pam-work"></a>Como funciona a MIM PAM?

O PAM baseia-se em novas capacidades do AD DS, particularmente para autorização e autenticação de contas de domínio, e em novas capacidades do Microsoft Identity Manager. O PAM separa as contas com privilégios de um ambiente do Active Directory existente. Quando é preciso utilizar uma conta com privilégios, primeiro tem de ser pedida e, em seguida, aprovada. Após a aprovação, a conta com privilégios obtém permissão através de um grupo principal externo numa nova floresta bastion, em vez de na floresta atual do utilizador ou aplicação. A utilização de uma floresta bastion dá à organização um maior controlo, como, por exemplo, quando um utilizador pode ser membro de um grupo com privilégios, e como o utilizador precisa de fazer a autenticação.

O Active Directory, o Serviço MIM e outras partes desta solução também podem ser implementados numa configuração de elevada disponibilidade.

O exemplo seguinte mostra como o PIM funciona de forma mais detalhada.

![Processo do PIM e participantes - diagrama](media/MIM_PIM_howitworks.png)

A floresta bastion emite associações a um grupo de tempo limitado, que, por sua vez, produzem permissões de concessão de permissões (TGTs) de tempo limitado. Os serviços ou as aplicações baseados em Kerberos podem honrar e impor estas TGTs se as aplicações e serviços existirem em florestas que confiam na floresta bastion.

As contas de utilizador diárias não precisam de se deslocar para uma nova floresta. O mesmo acontece com os computadores, aplicações e os respetivos grupos. Estes permanecem onde estão atualmente numa floresta existente. Considere o exemplo de uma organização que está preocupada com estes problemas atuais de cibersegurança, mas não tem planos imediatos para atualizar a infraestrutura de servidor para a próxima versão do Windows Server. Essa organização pode continuar a tirar partido desta solução combinada com o MIM e uma nova floresta bastion, e pode controlar melhor o acesso aos recursos existentes.

O PAM oferece as seguintes vantagens:

- **Isolamento/controlo do âmbito de privilégios**: os utilizadores não retêm privilégios em contas que também são utilizadas para tarefas sem privilégios, como a verificação do e-mail ou a navegação na Internet. Os utilizadores precisam de pedir privilégios. Os pedidos são aprovados ou negados com base nas políticas MIM definidas por um administrador do PAM. Até um pedido for aprovado, o acesso privilegiado não está disponível.

- **Atualização e prova**: estes são novos desafios de autenticação e autorização para ajudar a gerir o ciclo de vida de diferentes contas administrativas. O utilizador pode pedir a elevação de uma conta administrativa e esse pedido passa por fluxos de trabalho do MIM.

- **Registo adicional**: juntamente com os fluxos de trabalho do MIM incorporados, existe um registo adicional do PAM que identifica o pedido, o modo como foi autorizado e quaisquer eventos que ocorram após a aprovação.

- **Fluxo de trabalho personalizável**: os fluxos de trabalho do MIM podem ser configurados para diferentes cenários e podem ser utilizados vários fluxos de trabalho, com base nos parâmetros do utilizador requerente ou das funções pedidas.

## <a name="how-do-users-request-privileged-access"></a>Como é que os utilizadores pedem acesso privilegiado?

Existem várias formas de um utilizador poder submeter um pedido, incluindo:

- A API dos Serviços Web dos Serviços MIM
- Um ponto final de REST
- Windows PowerShell (`New-PAMRequest`)

Obter detalhes acerca dos [cmdlets de Gestão de Acesso Privilegiado](https://docs.microsoft.com/powershell/identitymanager/mimpam/vlatest/mimpam).

## <a name="what-workflows-and-monitoring-options-are-available"></a>Que fluxos de trabalho e opções de monitorização estão disponíveis?

Como exemplo, digamos que um utilizador era membro de um grupo administrativo antes da INSTALAÇÃO DAP. Como parte da configuração do PAM, o utilizador é removido do grupo administrativo, e uma política é criada na MIM. A política especifica que, se esse utilizador solicitar privilégios administrativos, o pedido é aprovado e uma conta separada para o utilizador será adicionada ao grupo privilegiado na floresta de bastiões.

Partindo do princípio de que o pedido é aprovado, o fluxo de trabalho da ação comunica diretamente com a floresta bastion do Active Directory para colocar um utilizador num grupo. Por exemplo, quando a Joana pede para administrar a base de dados de RH, a conta administrativa da Joana é adicionada ao grupo com privilégios na floresta bastion dentro de segundos. A sua conta administrativa nesse grupo expirará após um prazo. Com o Windows Server 2016 ou mais tarde, essa adesão está associada no Ative Directory com um prazo limite.

> [!NOTE]
> Quando adicionar um novo membro a um grupo, a alteração precisa de ser replicada para outros controladores de domínio (DCs) na floresta bastion. A latência de replicação pode afetar a capacidade dos utilizadores de acederem a recursos. Para obter mais informações sobre a latência de replicação, veja [Como Funciona a Topologia de Replicação do Active Directory](https://technet.microsoft.com/library/cc755994.aspx).
>
> Em contrapartida, uma ligação expirada é avaliada em tempo real pelo Gestor de contas de segurança (SAM). Embora a adição de um membro do grupo precise de ser replicada pelo DC que recebe o pedido de acesso, a remoção de um membro do grupo é avaliada instantaneamente em qualquer DC.

Este fluxo de trabalho destina-se especificamente a estas contas administrativas. Os administradores (ou até mesmo os scripts) que precisem apenas de acesso ocasional a grupos com privilégios, podem pedir precisamente esse acesso. O MIM regista o pedido e as alterações no Active Directory, e pode visualizá-los no Visualizador de Eventos ou enviar os dados para soluções de monitorização empresariais, como o System Center 2012 - Serviços de Recolha de Auditorias (ACS) do Operations Manager, ou outras ferramentas de terceiros.

## <a name="next-steps"></a>Próximos passos

- [Estratégia de acesso privilegiado](https://docs.microsoft.com/security/compass/privileged-access-strategy)
- [Cmdlets de Gestão de Acesso Privilegiado](https://docs.microsoft.com/powershell/identitymanager/mimpam/vlatest/mimpam)
