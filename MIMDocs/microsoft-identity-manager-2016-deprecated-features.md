---
title: MIM Características depreciadas e planeamento para o futuro Microsoft Docs
description: Este artigo documenta características depreciadas do Gestor de Identidade MIM 2016 SP1.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 2/28/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 009d0e99e2da445d4df35dc9de81b297a65fe2a3
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044230"
---
# <a name="deprecated-features"></a>Características depreciadas

Este artigo descreve as funcionalidades depreciadas do Microsoft Identity Manager 2016 SP1. Onde a funcionalidade ainda se encontra presente no Microsoft Identity Manager, ainda é suportada. As funcionalidades não são recomendadas para novas implementações, uma vez que podem ser removidas numa versão de recurso.  Para os desenvolvedores, recomendamos que não utilize funcionalidades depreciadas em novas aplicações ou soluções.

> [!NOTE]
> As funcionalidades e funcionalidades removidas no Microsoft Identity Manager SP1 são identificadas com **. <br>
> Para mais informações sobre o ciclo de vida de suporte [para o Microsoft Identity Manager](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)


## <a name="bhold"></a>BHOLD 

A Microsoft não recomenda que os clientes iniciem novas implementações dos componentes da Microsoft BHOLD Suite. As atuais implantações do BHOLD continuarão a ser apoiadas. A Azure AD oferece agora [avaliações](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview) de acesso que substituem algumas das funcionalidades da campanha de atestado BHOLD.

## <a name="certificate-management"></a>Gestão de Certificados 

| **Categoria**                | **Recurso depreciado**              | **Substituição e Comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Agentes de Gestão | **GEStão de Certificados FIM | O Agente de Gestão de Certificados FIM foi removido em MIM 2016.                                                             |

## <a name="service-and-portal"></a>Serviço e Portal

| **Categoria**                | **Recurso depreciado**              | **Substituição e Comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Configuração Programática | Interface de configuração do Serviço Web (ma-data e mv-data) | A capacidade de configurar o serviço de sincronização FIM através do serviço web de serviço FIM será removida numa versão seguinte.
|

## <a name="synchronization-service"></a>Serviço de Sincronização 

| **Categoria**                | **Recurso depreciado**              | **Substituição e Comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Configuração Programática | Interface de configuração do Serviço Web | A capacidade de configurar o serviço de sincronização FIM através do serviço FIM será removida numa versão seguinte.                                                          |
| Agentes de Gestão           | MAs incorporados                        | Os seguintes Estados-Maiores foram removidos em MIM 2016: </br> 1. **MA para gestão de certificados FIM </br>2. **MA para notas de lóloto</br> 3. **MA para SAP R/3 </br> As Notas de Lógoo e os SAP R/3 MAs foram substituídos por novas versões. Para mais informações, consulte o histórico e o download da versão do [conector mais recente](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)                                                                                                                                                                                                                                              |
| Agentes de Gestão           | ECMA1                               | O quadro de extibilidade ECMA1/XMA foi substituído pelo ECMA 2.0. É necessário atualizar os agentes de gestão ecma1 existentes com conectores ECMA2.0.                                                                                                                                          |
| Agentes de Gestão           | Conectores de execução fora de proc      | Esta funcionalidade não será substituída. O serviço de sincronização irá sempre ligar para o conector no mesmo processo. É da responsabilidade do conector iniciar e gerir o outro processo. |
| Agentes de Gestão           | Configurar o nome do display da partição    | Esta funcionalidade não será substituída. Esta opção foi utilizada apenas para fornecer um nome alternativo para uma partilha nas interfaces WMI.                                                                                                                                                                       |
| Executar perfis                | Perfis combinados                   | Os perfis combinados delta import/sync, total importação/delta sincronização, e total importação/sincronização serão removidos. Em vez disso, devias usar perfis de execução com dois passos. 

> [!NOTE]
> Deve manter os perfis de execução combinados apenas em ambientes onde o desempenho seria impactado por um grande número de descodedores existentes.


| **Categoria**                | **Recurso depreciado**              | **Substituição e Comentário**           |
|--------|-------|---|    
| Precedência do atributo | Precedência multi-mestria/igual                       | A mesma precedência será removida. Não há substituto para esta funcionalidade. Em vez disso, deve configurar a precedência manual. Pode continuar a utilizar esta funcionalidade se o seu ambiente tiver um agente de gestão do Serviço FIM implantado. Este agente de gestão não fornece precedência manual para evitar a exportação não precedente para o provisionamento declarativo. |
| Aderir às Regras           | Junte-se ao tipo de objeto "Qualquer"                             | Esta funcionalidade não será substituída. Todas as regras de adesão devem definir explicitamente o tipo de objeto metaverso a que estão a tentar aderir.       |
| Fluxos de atributos      | Desseleccionar "permitir nulos" para valores exportados            | Esta funcionalidade não será substituída. "Permitir nulos" será sempre selecionado. Deve certificar-se de que tem "Allow Nulls" selecionado no seu ambiente atual.  |
| Fluxos de atributos      | "Não me lembro de atributos"                            | Esta funcionalidade não será substituída. Os atributos serão sempre recordados, que é a melhor prática.  |
| Extensão de Regras      | Executar metaverso e ma regras extensão fora-de-proc | Esta funcionalidade não será substituída. As regras de fluxo metaverso e atributo serão executadas no mesmo processo que o motor de sincronização.       |
| Extensão de Regras      | Propriedades de transações                                | Esta funcionalidade não será substituída. Deve evitar passar dados entre a entrada, o provisionamento e a sincronização de saída utilizando esta classe de utilidade.  |
| Extensão de Regras      | ExchangeUtils: Criar 55 métodos de\*                     | Os métodos para criar objetos para os servidores Do Exchange 5.5 serão removidos.        |
| Interface            | Mms_Metaverse                                        | Todos os membros da classe ClmUtils serão removidos numa próxima versão.   |

## <a name="next-steps"></a>Próximos passos
Saiba mais sobre:

- Microsoft Identity Manager está ainda intimamente relacionado com o respetivo predecessor, o Forefront Identity Manager. Se ainda utilizar FIM ou desejar referir-se a documentação adicional, observe o [Mapa de Documentação do FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Considerações de topologia para a implementação de MIM](topology-considerations.md) Este artigo introduz várias topoologias de implantação que pode considerar implementar.
- [Guia de planeamento de capacidades](capacity-planning-guide.md) Pode utilizar este guia, juntamente com ambientes de teste, para compreender a margem adequada para a sua implementação.
