---
title: Atualização de suporte para clientes Azure AD Premium usando o Microsoft Identity Manager ; Microsoft Docs
description: Este artigo descreve como os clientes Azure AD Premium podem obter apoio após 21 de janeiro de 2021.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 6/9/2020
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
reviewer: markwahl-msft
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 26dcaf121c4fd980d296ffee893af3ca66249c6c
ms.sourcegitcommit: dae61d97c9db5402d35e2757a1ce844d16236032
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/25/2020
ms.locfileid: "96035621"
---
# <a name="support-update-for-azure-ad-premium-customers-using-microsoft-identity-manager"></a>Atualização de suporte para clientes Azure AD Premium usando o Microsoft Identity Manager

Aplica-se a: Azure AD Premium, Microsoft Identity Manager (MIM)

Para os clientes Azure AD Premium, o suporte standard está disponível a partir de junho de 2020, continuando depois de janeiro de 2021, para componentes específicos do [Microsoft Identity Manager 2016 Service Pack 2](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016), ou posteriores pacotes de serviços, que permitem a integração do Azure AD. Isto para além do suporte existente para o Microsoft Identity Manager já fornecido através da política fixa do [ciclo de vida](https://docs.microsoft.com//lifecycle/policies/fixed) e dos planos de apoio às [empresas.](https://support.microsoft.com/help/4341255)

Os componentes MIM para os quais existe suporte padrão incluem:
- Serviço de Sincronização MIM e Serviço de Notificação de Alteração de Password (PCNS)
- Serviço MIM e Portal, Add-ins e extensões, Scripts de Suporte ao Armazém de Dados e pacotes de idiomas
- Conectores MIM

Estes componentes MIM povoam o Ative Directory, e por extensão, Azure AD através do Azure AD Connect, com os utilizadores e grupos a provisionados a partir de um sistema de RH no local ou outro sistema de fontes de registo. Isto garante que os clientes que utilizam o Azure AD Premium com sistemas no local podem continuar a ser apoiados durante a migração dos seus cenários de gestão de identidade de sistemas de acesso ao Azure AD. 

## <a name="opening-a-support-request-in-the-azure-portal"></a>Abertura de um pedido de apoio no portal Azure

Como opção de suporte adicional para o Microsoft Identity Manager, os clientes Azure AD Premium podem solicitar suporte para os componentes acima mencionados do Microsoft Identity Manager 2016 Service Pack 2, ou um hotfix ou atualização posterior, através do portal Azure.

Um cliente pode criar um pedido de suporte Azure, utilizando as instruções de Como criar um pedido de [suporte Azure:](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)
1. selecionar *Tipo de emissão: Técnico*
1. mudar para mostrar *Todos os Serviços*
1. na lista de serviços no Azure Ative Directy seleciona  *o Provisionamento do Utilizador e a Sincronização*
1. selecionar *tipo de problema: Microsoft Identity Manager (MIM)*
1. selecionar *subtipo de problemas*: *Conectores,* *Serviço e Portal* ou motor de *sincronização*

![Criar pedido de suporte MIM](media/azure-active-directory-new-support-request.png)

Os componentes MIM estão listados como tipos de problemas dentro *do Azure Ative Directory User Provisioning and Synchronization* no portal Azure.

Para pedidos abertos através do portal Azure, o suporte padrão está disponível para os clientes Azure AD Premium para os seguintes componentes do Microsoft Identity Manager 2016 Service Pack 2, ou um [hotfix ou atualização posterior](reference/version-history.md): Serviço de Sincronização, Serviço de Notificação de Mudança de Palavras (PCNS), Conectores, Serviço e Portal, Add-ins e extensões, Scripts de Suporte ao Armazém de Dados e pacotes de linguagem.

## <a name="other-support-options"></a>Outras opções de suporte

MIM 2016 SP2, construído 4.6.34.0, foi lançado em outubro de 2019. Os clientes são altamente encorajados a permanecer em um pacote de serviço totalmente suportado para garantir que eles estão na versão mais recente e mais segura do seu produto. Para mais informações, consulte a política do [ciclo de vida do pacote de serviços.](https://support.microsoft.com/help/17138)

Para clientes que ainda utilizam uma construção mais antiga de MIM, ou clientes que não tenham suporte ou subscrição do Azure para um suite que inclua O Azure Ative Directory Premium, ou para problemas com outros componentes da MIM não listados acima, o suporte continua disponível. A política de suporte é descrita na [política de ciclo de vida fixo](https://docs.microsoft.com/lifecycle/policies/fixed) com as datas específicas no ciclo de vida de suporte para o Microsoft Identity Manager [2016](https://support.microsoft.com/lifecycle/search?alpha=microsoft%20identity%20manager%202016).

Além do apoio do Azure, existem várias outras opções de apoio que as organizações podem usar para obter apoio. Por exemplo, se tiver o Microsoft Professional Support, pode [criar um novo pedido de suporte.](https://support.microsoft.com/supportforbusiness/productselection) Para selecionar o componente MIM relevante:
1. selecionar família de produtos *Segurança*
1. selecionar gestor de identidade de produto *2016*
