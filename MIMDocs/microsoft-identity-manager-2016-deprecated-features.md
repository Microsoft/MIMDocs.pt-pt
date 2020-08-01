---
title: MIM deprecou funcionalidades e planeamento para o futuro Microsoft Docs
description: Este artigo documenta características preifríadas do Gestor de Identidade mim 2016 SP2.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 7/28/2020
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: cb0159ec70e161dc47140712bd2ac039786e034d
ms.sourcegitcommit: 50cee02a7146445bd6fa361349099c7b294792d8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/30/2020
ms.locfileid: "87443549"
---
# <a name="deprecated-features"></a>Características precotados

Este artigo descreve as funcionalidades preprecadas do Microsoft Identity Manager 2016 SP2. Onde a funcionalidade ainda se encontra presente no Microsoft Identity Manager, ainda é suportada. As funcionalidades não são recomendadas para novas implementações, uma vez que podem ser removidas num futuro hotfix ou desbloqueio do pack de serviço.  Para os desenvolvedores, recomendamos não utilizar funcionalidades pretiladas em quaisquer novas aplicações ou soluções.

> [!NOTE]
>
> Para obter mais informações sobre as novas opções de suporte para MIM, consulte [as opções de suporte para clientes Azure AD Premium.](support-update-for-azure-active-directory-premium-customers.md)

## <a name="bhold"></a>BHOLD

A Microsoft não recomenda que os clientes iniciem novas implementações dos componentes da Microsoft BHOLD Suite. As implantações existentes da BHOLD continuarão a ser apoiadas. O Azure AD agora fornece [revisões de acesso](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview), que substitui as funcionalidades da campanha de atestado BHOLD, e a gestão de direitos, que substitui as funcionalidades de atribuição de acesso.

## <a name="service-and-portal"></a>Serviço e Portal

| **Categoria**                | **Recurso precotado**              | **Comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Configuração programática de sincronização | Interface de configuração do Serviço Web (ma-data e mv-data) | A capacidade de configurar o serviço de sincronização MIM, através do serviço mim do serviço WEB, pode ser removida num futuro hotfix ou pacote de serviço.
|

## <a name="synchronization-service"></a>Serviço de Sincronização 

Os seguintes MAs foram removidos em MIM 2016: </br> 1. MA para gestão de certificados FIM </br>2. MA para notas de lótus</br> 3. MA para SAP R/3 </br> As Notas de Lótus e OS SAP R/3 MAs foram substituídas por novos conectores. Para mais informações, consulte [o histórico de versão do conector mais recente & descarregar](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history).

O quadro de extensibilidade ECMA1/XMA foi substituído por ECMA 2.0. É necessária atualização dos agentes de gestão ECMA1 existentes com conectores ECMA2.0.

| **Categoria**                | **Recurso precotado**              | **Comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Agentes de Gestão           | Conectores fora de proc      | O serviço de sincronização ligará sempre o conector no mesmo processo. É da responsabilidade do conector iniciar e gerir o outro processo. |
| Agentes de Gestão           | Nome do visor de divisória configurar    | Esta opção foi utilizada apenas para fornecer um nome alternativo para uma partição nas interfaces do WMI.                                                                                                                                                                       |
| Executar perfis                | Perfis combinados                   | Os perfis combinados delta import/sync, total import/delta sync e importação/sincronização completa podem ser removidos. Utilize perfis de execução com dois passos em vez disso.

> [!NOTE]
> Deve manter os perfis de execução combinados apenas em ambientes onde o desempenho seria impactado por um grande número de desconexões existentes.

| **Categoria**                | **Recurso precotado**              | **Comentário**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| Precedência de Atributos | Precedência multi-mestria/igual                       | A precedência igual pode ser removida. Em vez disso, deve configurar a precedência manual. Pode continuar a utilizar esta funcionalidade se o seu ambiente tiver um agente de gestão do Serviço FIM implantado. Este agente de gestão não proporciona precedência manual para evitar a exportação-não precedente para o provisionamento declarativo. |
| Aderir regras           | Junte-se ao tipo de objeto "Qualquer"                             | Todas as regras de junção devem definir explicitamente o tipo de objeto metaverso a que estão a tentar aderir.       |
| Fluxos de atributos      | Desmarcar "permitir nulos" para valores exportados            | Será sempre selecionado "Permitir nulos", por isso certifique-se de que tem "Permitir nulos" selecionado no seu ambiente atual.  |
| Fluxos de atributos      | "Não me lembro de atributos"                            | Os atributos serão sempre recordados, que é a melhor prática.  |
| Extensão de regras      | Executar metaverso e ma regras extensão fora de proc | As regras de metaverso e de fluxo de atributos funcionarão no mesmo processo que o motor de sincronização.       |
| Extensão de regras      | Propriedades de transação                                | Evite passar dados entre a sincronização de entrada, provisão e sincronização de saída utilizando esta classe de utilidade.  |
| Extensão de regras      | ExchangeUtils: Criar métodos 55 \*                     | Os métodos para criar objetos para os servidores Exchange 5.5 podem ser removidos.        |
| Interface            | Mms_Metaverse                                        | Todos os membros da classe ClmUtils podem ser removidos num futuro hotfix ou pacote de serviço.   |

## <a name="next-steps"></a>Passos seguintes
Saiba mais sobre:

- Microsoft Identity Manager está ainda intimamente relacionado com o respetivo predecessor, o Forefront Identity Manager. Se ainda utilizar FIM ou desejar referir-se a documentação adicional, observe o [Mapa de Documentação do FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx).
- [Considerações de topologia para implantação de MIM](topology-considerations.md) Este artigo introduz várias topologias de implantação que poderá considerar implementar.
- [Guia de planeamento de capacidades](capacity-planning-guide.md) Pode utilizar este guia, juntamente com ambientes de teste, para compreender o âmbito adequado para a sua implantação.

