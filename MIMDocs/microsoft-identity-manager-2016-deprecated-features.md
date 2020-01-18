---
title: Recursos preteridos do MIM e planejamento para o futuro | Microsoft Docs
description: Este artigo documenta os recursos preteridos do MIM Identity Manager 2016 SP1.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 2/28/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: dd023e2152693f56dd9a86dc66d9c1a8ef4ec64e
ms.sourcegitcommit: 1ca298d61f6020623f1936f86346b47ec5105d44
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76256585"
---
# <a name="deprecated-features"></a>Recursos preteridos

Este artigo descreve os recursos preteridos do Microsoft Identity Manager 2016 SP1. Quando o recurso ainda estiver presente no Microsoft Identity Manager, ainda terá suporte. Os recursos não são recomendados para novas implantações, pois podem ser removidos em uma versão de recurso.  Para os desenvolvedores, recomendamos não utilizar recursos preteridos em novos aplicativos ou soluções.

> [!NOTE]
> Os recursos e as funcionalidades removidas no Microsoft Identity Manager SP1 são identificados com * *. <br>
> Para obter mais informações sobre o [ciclo de vida do suporte para Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)


## <a name="bhold"></a>BHOLD 

A Microsoft não recomenda que os clientes iniciem novas implantações dos componentes do Microsoft BHOLD Suite. As implantações existentes do BHOLD continuarão a ter suporte. O Azure AD agora fornece [revisões de acesso](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview) que substituem alguns dos recursos de campanha de atestado BHOLD.

## <a name="certificate-management"></a>Gestão de Certificados 

| **Categoria**                | **Recurso preterido**              | **Substituição e comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Agentes de gerenciamento | \* * Gerenciamento de certificados do FIM | O agente de gerenciamento de certificados do FIM foi removido no MIM 2016.                                                             |

## <a name="service-and-portal"></a>Serviço e Portal

| **Categoria**                | **Recurso preterido**              | **Substituição e comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Configuração programática | Interface de configuração do serviço Web (ma-data e MV-Data) | A capacidade de configurar o serviço de sincronização do FIM por meio do serviço Web do FIM de serviço será removida em uma próxima versão.
|

## <a name="synchronization-service"></a>Serviço de Sincronização 

| **Categoria**                | **Recurso preterido**              | **Substituição e comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Configuração programática | Interface de configuração do serviço Web | A capacidade de configurar o serviço de sincronização do FIM por meio do serviço FIM será removida em uma próxima versão.                                                          |
| Agentes de gerenciamento           | MAs interno                        | O MAs a seguir foram removidos no MIM 2016: </br> 1. * * MA para gerenciamento de certificados do FIM </br>2. * * MA para Lotus Notes</br> 3. * * MA para SAP R/3 </br> As notas e o Lotus Notes e o SAP R/3 MAs foram substituídos por novas versões. Para obter mais informações, consulte [histórico de lançamento de versão mais recente do conector & baixar](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)                                                                                                                                                                                                                                              |
| Agentes de gerenciamento           | ECMA1                               | A estrutura de extensibilidade do ECMA1/XMA foi substituída pelo ECMA 2,0. É necessário atualizar os agentes de gerenciamento de ECMA1 existentes com conectores ECMA 2.0.                                                                                                                                          |
| Agentes de gerenciamento           | Executando conectores fora do processo      | Esse recurso não será substituído. O serviço de sincronização sempre chamará o conector no mesmo processo. É responsabilidade do conector iniciar e gerenciar o outro processo. |
| Agentes de gerenciamento           | Configurar nome de exibição da partição    | Esse recurso não será substituído. Essa opção foi usada apenas para fornecer um nome alternativo para uma partição nas interfaces WMI.                                                                                                                                                                       |
| Perfis de execução                | Perfis combinados                   | Os perfis combinados importação/sincronização Delta, importação completa/sincronização Delta e importação/sincronização completa serão removidos. Em vez disso, você deve usar perfis de execução com duas etapas. 

> [!NOTE]
> Você deve manter perfis de execução combinados somente em ambientes em que o desempenho seria afetado por um grande número de Desconectadores existentes.


| **Categoria**                | **Recurso preterido**              | **Substituição e comentário**           |
|--------|-------|---|    
| Precedência de Atributos | Multimestre/prioridade igual                       | A precedência igual será removida. Não há nenhuma substituição para esse recurso. Em vez disso, você deve configurar a precedência manual. Você pode continuar a usar esse recurso se o seu ambiente tiver um agente de gerenciamento de serviço do FIM implantado. Esse agente de gerenciamento não fornece precedência manual para evitar a exportação-não-precedente para provisionamento declarativo. |
| Regras de junção           | Ingressar no tipo de objeto "any"                             | Esse recurso não será substituído. Todas as regras de junção devem definir explicitamente o tipo de objeto do metaverso ao qual estão tentando ingressar.       |
| Fluxos de atributos      | Cancelar seleção de "permitir nulos" para valores exportados            | Esse recurso não será substituído. "Permitir nulos" sempre será selecionado. Certifique-se de que você tenha "permitir nulos" selecionado em seu ambiente atual.  |
| Fluxos de atributos      | "Não recuperar atributos"                            | Esse recurso não será substituído. Os atributos sempre serão recuperados, o que é a melhor prática.  |
| Extensão de regras      | Executando extensão de regras de metaverso e ma para fora do processo | Esse recurso não será substituído. As regras de fluxo de atributos e metaverso serão executadas no mesmo processo que o mecanismo de sincronização.       |
| Extensão de regras      | Propriedades da transação                                | Esse recurso não será substituído. Você deve evitar passar dados entre entrada, provisionamento e sincronização de saída usando essa classe de utilitário.  |
| Extensão de regras      | ExchangeUtils: métodos de\* Create55                     | Os métodos para criar objetos para servidores Exchange 5,5 serão removidos.        |
| Interface            | Mms_Metaverse                                        | Todos os membros da classe ClmUtils serão removidos em uma próxima versão.   |

## <a name="next-steps"></a>Próximos passos
Saiba mais sobre:

- Microsoft Identity Manager está ainda intimamente relacionado com o respetivo predecessor, o Forefront Identity Manager. Se ainda utilizar FIM ou desejar referir-se a documentação adicional, observe o [Mapa de Documentação do FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Considerações sobre topologia para implantação do mim](topology-considerations.md) Este artigo apresenta várias topologias de implantação que você pode considerar implementar.
- [Guia de planejamento de capacidade](capacity-planning-guide.md) Você pode usar este guia, juntamente com ambientes de teste, para entender o escopo apropriado para sua implantação.
