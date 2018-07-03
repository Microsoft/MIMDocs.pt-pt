---
title: MIM preterido recursos e planejar o futuro | Documentos da Microsoft
description: Documentos neste artigo preterido funcionalidades do MIM Identity Manager 2016 SP1.
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
ms.openlocfilehash: a4239f1d69d8a43d70dd38af16e9ef8be62bd33c
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36288916"
---
# <a name="deprecated-features"></a>Funcionalidades preteridas

Este artigo descreve as funcionalidades preteridas do Microsoft Identity Manager 2016 SP1. Quando a funcionalidade ainda está presente no Microsoft Identity Manager, ainda é suportado. Funcionalidades não são recomendadas para novas Implantações, como pode ser removidos numa versão de funcionalidade.  Para os desenvolvedores, recomendamos que não utilizar funcionalidades em qualquer novos aplicativos ou soluções de preteridas.

> [!NOTE]
> Funções e funcionalidades removidas no SP1 do Microsoft Identity Manager são identificadas com * *. <br>
> Para obter mais informações sobre o suporte [ciclo de vida do Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)


## <a name="bhold"></a>BHOLD 

A Microsoft não recomenda a clientes começam novas implementações dos componentes do Microsoft BHOLD Suite. As implementações existentes do BHOLD irão continuar a ser suportado. Agora o Azure AD proporciona [as revisões de acesso](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview) que substituir alguns dos recursos BHOLD attestation campanha.

## <a name="certificate-management"></a>Gestão de Certificados 

| **Categoria**                | **Funcionalidade preterida**              | **Substituição e comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Agentes de gestão | * * FIM Certificate Management | Agente de gestão de certificado do FIM foi removido em MIM 2016.                                                             |

## <a name="service-and-portal"></a>Serviço e Portal

| **Categoria**                | **Funcionalidade preterida**              | **Substituição e comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Configuração programática | Interface de configuração de serviço Web (dados de ma e dados de mv) | A capacidade de configurar o serviço de sincronização do FIM através do serviço de web do serviço FIM será removida numa versão seguinte.
|

## <a name="synchronization-service"></a>Serviço de Sincronização 

| **Categoria**                | **Funcionalidade preterida**              | **Substituição e comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Configuração programática | Interface de configuração do serviço Web | A capacidade de configurar o serviço de sincronização do FIM através do serviço FIM será removida numa versão seguinte.                                                          |
| Agentes de gestão           | MAs incorporadas                        | As MAs seguintes foram removidas em MIM 2016: </br> 1. * * MA para gestão de certificados do FIM </br>2. * * MA para Lotus Notes</br> 3. * * MA para SAP R/3 </br> O Lotus Notes e a SAP R/3 MAs foram substituídas por novas versões. Para obter mais informações, consulte [mais recente histórico de versões do conector & Download](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)                                                                                                                                                                                                                                              |
| Agentes de gestão           | ECMA1                               | A estrutura de extensibilidade ECMA1/XMA foi substituída por do ECMA 2.0. A atualizar agentes de gestão de ECMA1 existentes com conectores ECMA2.0 é necessária.                                                                                                                                          |
| Agentes de gestão           | Executar conectores out-of-proc      | Esta funcionalidade não será substituída. O serviço de sincronização será sempre chamar o conector no mesmo processo. É da responsabilidade do conector para iniciar e gerir o outro processo. |
| Agentes de gestão           | Configurar o nome de exibição de partição    | Esta funcionalidade não será substituída. Esta opção só foi utilizada para fornecer um nome alternativo para uma partição as interfaces de WMI.                                                                                                                                                                       |
| Perfis de execução                | Perfis combinados                   | A importação/sincronização delta combinado de perfis, a sincronização delta/importação completa e importação/sincronização completa serão removidos. Em vez disso, deve usar perfis de execução com dois passos. 

> [!NOTE]
> Deve manter perfis de execução combinados apenas em ambientes onde o desempenho poderia ser afetado por um grande número de disconnectors existentes.


| **Categoria**                | **Funcionalidade preterida**              | **Substituição e comentário**           |
|--------|-------|---|    
| Precedência de atributos | Precedência de multi-maestria/igual                       | Precedência igual será removida. Não há nenhuma substituição para esta funcionalidade. Em vez disso, deve configurar precedência manual. Pode continuar a utilizar esta funcionalidade, se o ambiente tiver um agente de gestão do serviço FIM implementado. Este agente de gestão não fornece manual precedência para evitar a exportação não precedent para aprovisionamento declarativo. |
| Regras de associação           | Junte-se no "Qualquer" tipo de objeto                             | Esta funcionalidade não será substituída. Todas as regras de associação devem definir explicitamente o tipo de objeto de metaverso que estão a tentar associar a.       |
| Fluxos de atributos      | Desmarque a opção "permitir valores nulos" para valores exportados            | Esta funcionalidade não será substituída. "Permitir valores nulos" sempre será selecionado. Deve certificar-se de que tem a opção "Permitir valores NULL" selecionado no seu ambiente atual.  |
| Fluxos de atributos      | "Não se lembrar atributos"                            | Esta funcionalidade não será substituída. Atributos serão sempre que possa ser recuperados, que é a melhor prática.  |
| Extensão de regras      | Executar o metaverso e ma regras de extensão out-of-proc | Esta funcionalidade não será substituída. As regras de fluxo de metaverso e atributo serão executado no mesmo processo, como o motor de sincronização.       |
| Extensão de regras      | Propriedades da transação                                | Esta funcionalidade não será substituída. Deve evitar transmitindo dados entre a sincronização de entrada, aprovisionamento e de saída usando essa classe de utilitário.  |
| Extensão de regras      | ExchangeUtils: Create55\* métodos                     | Os métodos para criar objetos para servidores Exchange 5.5 serão removidos.        |
| Interface            | Mms_Metaverse                                        | Todos os membros de classe ClmUtils serão removidos numa versão seguinte.   |

## <a name="next-steps"></a>Próximos passos
Saiba mais sobre:

- Microsoft Identity Manager está ainda intimamente relacionado com o respetivo predecessor, o Forefront Identity Manager. Se ainda utilizar FIM ou desejar referir-se a documentação adicional, observe o [Mapa de Documentação do FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Considerações sobre a topologia da implementação de MIM](topology-considerations.md) este artigo apresenta várias topologias de implementação que pode considerar implementar.
- [Guia de planeamento de capacidade](capacity-planning-guide.md) pode utilizar este guia, juntamente com ambientes de teste, para compreender o âmbito adequado para a sua implementação.
