---
title: MIM preterido funcionalidades e o planeamento para o futuro | Microsoft Docs
description: Documentos neste artigo preterido funcionalidades do SP1 de 2016 MIM Identity Manager.
keywords: ''
author: barclayn
ms.author: davidste
manager: mbaldwin
ms.date: 2/28/2018
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 50f7b135ce0d5a46ea08068a7658b229759d2b50
ms.sourcegitcommit: 24bb3e82f55971696bdefa6c240f1a27f856e110
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="deprecated-features"></a>Funcionalidades preteridas

Este artigo descreve as funcionalidades preteridas do Microsoft Identity Manager 2016 SP1. Em que a funcionalidade ainda está presente no Microsoft Identity Manager, ainda é suportada. Funcionalidades não são recomendadas para novas implementações, que podem ser removidos numa versão de funcionalidade.  Para programadores, recomendamos que não utilizar as funcionalidades em qualquer novas aplicações ou soluções preteridas.

>[!NOTE]
Funções e funcionalidades removidas no SP1 do Microsoft Identity Manager são identificadas com o * *. <br>
Para mais informações sobre o suporte [ciclo de vida para o Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)


## <a name="bhold"></a>BHOLD 

A Microsoft não recomenda a clientes começam novas implementações dos componentes do Microsoft BHOLD Suite. As implementações existentes do BHOLD irão continuar a ser suportado. Agora o Azure AD fornece [aceder revisões](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview) que substitua algumas das funcionalidades BHOLD atestado campanha.

## <a name="certificate-management"></a>Gestão de Certificados 
| **Categoria**                | **Funcionalidade preterida**              | **Substituição e comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Agentes de gestão | * * FIM Certificate Management | Agente de gestão de certificado do FIM foi removido em MIM 2016.                                                             |

## <a name="service-and-portal"></a>Serviço e Portal

| **Categoria**                | **Funcionalidade preterida**              | **Substituição e comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Configuração programática | Interface de configuração de serviço Web (ma dados e os dados de mv) | A capacidade de configurar o serviço de sincronização do FIM através do serviço web do FIM service será removida numa versão seguinte.
|

## <a name="synchronization-service"></a>Serviço de Sincronização 

| **Categoria**                | **Funcionalidade preterida**              | **Substituição e comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Configuração programática | Interface de configuração do serviço Web | A capacidade de configurar o serviço de sincronização do FIM através do serviço FIM será removida numa versão seguinte.                                                          |
| Agentes de gestão           | MAs incorporadas                        | As seguintes MAs tem sido removidas em MIM 2016: </br> 1. * * MA para gestão de certificados do FIM </br>2. * * MA para Lotus notas</br> 3. * * MA para SAP R/3 </br> As notas de Lotus e SAP R/3 MAs foram substituídas por novas versões. Para obter mais informações, consulte [histórico de lançamento do conector versão mais recente & de transferência](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)                                                                                                                                                                                                                                              |
| Agentes de gestão           | ECMA1                               | A estrutura de extensibilidade ECMA1/XMA foi substituída pelo 2.0 ECMA. É necessário atualizar agentes de gestão existentes ECMA1 com ECMA2.0 conectores.                                                                                                                                          |
| Agentes de gestão           | Em execução conectores out-of-proc      | Esta funcionalidade não será substituída. O serviço de sincronização sempre ligará para o conector no mesmo processo. É da responsabilidade do conector para iniciar e gerir o outro processo. |
| Agentes de gestão           | Configurar nome a apresentar partição    | Esta funcionalidade não será substituída. Esta opção apenas foi utilizada para fornecer um nome alternativo para uma partição nas interfaces WMI.                                                                                                                                                                       |
| Perfis de execução                | Perfis combinadas                   | A importação/sincronização delta perfis combinada, sincronização delta/importação completa e sincronização/importação completa serão removidos. Em vez disso, deve utilizar perfis de execução com dois passos. 

>[!NOTE]
Deve manter perfis de execução combinado apenas em ambientes onde o desempenho será afetado por um grande número de disconnectors existentes.


| **Categoria**                | **Funcionalidade preterida**              | **Substituição e comentário**           |
|--------|-------|---|    
| Precedência de atributos | Precedência de várias-mastery/igual                       | Precedência igual será removida. Não há nenhuma substituição para esta funcionalidade. Deve configurar a precedência do manual em vez disso. Pode continuar a utilizar esta funcionalidade, se o seu ambiente tem um agente de gestão do serviço FIM implementado. Este agente de gestão não fornece precedência manual para evitar a exportação não precedent para aprovisionamento declarativo. |
| Regras de associação           | Associar "Qualquer" tipo de objeto                             | Esta funcionalidade não será substituída. Todas as regras de associação devem definir explicitamente o tipo de objeto de metaverso que estão a tentar associar ao.       |
| Fluxos de atributos      | Desmarcar "permitir valores NULL" para valores exportados            | Esta funcionalidade não será substituída. "Permitir valores nulos" sempre será selecionado. Certifique-se de que tem a opção "Permitir valores nulos" selecionada no seu ambiente atual.  |
| Fluxos de atributos      | "Não recuperar os atributos"                            | Esta funcionalidade não será substituída. Atributos serão sempre possível resgatar os, que é a melhor prática.  |
| Extensão de regras      | Executar o metaverso e ma regras extensão out-of-proc | Esta funcionalidade não será substituída. As regras de fluxo de metaverso e atributo serão executado no mesmo processo, como o motor de sincronização.       |
| Extensão de regras      | Propriedades da transação                                | Esta funcionalidade não será substituída. Deve evitar passar dados entre utilizar esta classe do utilitário de sincronização de entrada, aprovisionamento e de saída.  |
| Extensão de regras      | ExchangeUtils: Create55\* métodos                     | Os métodos para criar objetos para os servidores Exchange 5.5 serão removidos.        |
| Interface            | Mms_Metaverse                                        | Todos os membros de classe ClmUtils serão removidos numa versão seguinte.   |

## <a name="next-steps"></a>Próximos passos
Saiba mais sobre:

- Microsoft Identity Manager está ainda intimamente relacionado com o respetivo predecessor, o Forefront Identity Manager. Se ainda utilizar FIM ou desejar referir-se a documentação adicional, observe o [Mapa de Documentação do FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Considerações sobre a topologia de implementação de MIM](topology-considerations.md) este artigo apresenta várias topologias de implementação que pode considerar implementar.
- [Guia de planeamento de capacidade](capacity-planning-guide.md) pode utilizar este guia, juntamente com ambientes de teste, para compreender o âmbito adequado para a sua implementação.
