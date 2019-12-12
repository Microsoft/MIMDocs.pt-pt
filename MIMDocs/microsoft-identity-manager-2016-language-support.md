---
title: Idiomas com suporte do Microsoft Identity Manager 2016 SP1 | Microsoft Docs
description: Uma lista de idiomas com suporte do Microsoft Identity Manager 2016 SP1.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/23/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.openlocfilehash: 5704e978734bea13f1a362aeb203810f3864205a
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "68701480"
---
# <a name="supported-languages"></a>Idiomas suportados

Este artigo descreve os idiomas com suporte e o mapeamento de atualizações do Microsoft Identity Manager 2016 SP1 versão 4.5. x ou superior.

## <a name="mim-service-and-portal-and-add-ins-and-extensions-language-pack"></a>Pacote de idiomas do serviço e portal e suplementos e extensões do MIM 

O pacote de idiomas do serviço do Microsoft e do portal oferece suporte aos seguintes idiomas 33 idiomas.  

> [!NOTE]
> No [4.4.1642.0](https://support.microsoft.com/en-us/help/4021562/hotfix-rollup-package-build-4-4-1642-0-is-available-for-microsoft) , uma chave do registro foi adicionada chamada "OverrideDefaultUILocale" ao pacote de idiomas de suplementos e extensões do mim tentará mapear todos os idiomas semelhantes para aquele com suporte. Por exemplo, se o idioma de exibição do Windows for ES-CL (espanhol Chile) ou qualquer\*ES, ele tentará mapear isso para ES-ES (espanhol da Espanha).

> [!IMPORTANT]
> O texto no suplemento SSPR e no Portal serão localizados, mas as perguntas não terão nenhum trabalho adicional. Você precisará criar fluxos de trabalho Authn (e conjuntos complementares e MPRs para direcioná-los) para direcionar perguntas em cada idioma para o local de destino.

|       Idioma        | FIM (4.3. x. x)/MIM (4.4. XX) | MIM (4,5. x. x) |
|-----------------------|--------------------------|--------------|
|       Búlgaro       |          bg-BG           |      BG      |
| Chinês (Simplificado)  |          zh-CN           |   zh-Hans    |
|   Chinês (Taiwan)    |          zh-TW           |   zh-Hant    |
|       Croata        |          RH de RH           |      hr      |
|         Checo         |          cs-CZ           |      cs      |
|        Dinamarquês         |          da-DK           |      da      |
|         Neerlandês         |          NL-NL           |      nl      |
|       Estónio        |          et-EE           |      os/390      |
|        Francês         |          FR-FR           |      fr      |
|        Finlandês        |          fi-FI           |      fi      |
|        Alemão         |          de-DE           |      de      |
|         Grego         |          el GR           |      el      |
|         Hindi         |          IN           |      hi      |
|       Húngaro       |          hu-HU           |      hu      |
|        Italiano        |          it-IT           |      it      |
|       Japonês        |          ja-JP           |      ja      |
|        Coreano         |          ko-KR           |      ko      |
|      Lituano       |          lt-LT           |      lt      |
|        Letão        |          lv-LV           |      lv      |
|       Norueguês       |          nb-NO           |    nb-NO     |
|        Polaco         |          pl-PL           |      pl      |
| Português (Portugal) |          pt-PT           |      pt      |
|  Português (Brasil)  |          pt-BR           |    pt-BR     |
|        Russo        |          ru-RU           |      ru      |
|       Romeno        |          RO-RO           |      ro      |
|        Espanhol        |          es-ES           |      es      |
|        Eslovaco         |          sk SK           |      SK      |
|        Sueco        |          SV-SE           |      sv      |
|       Esloveno       |          IS SL           |      sl      |
|   Sérvio-Sérvia    |  Sr-LATN-CS (preterida)  |  Sr-Latn-RS  |
|         Tailandês          |          th-TH           |      º      |
|        Turco        |          tr-TR           |      tr      |
|       Ucraniano       |          uk-UA           |      uk      |

## <a name="certificate-management"></a>Gestão de Certificados 
O Microsoft Certificate Management dá suporte aos nove idiomas a seguir. 

|Idioma|FIM (4.3. x. x)/MIM (4.4. XX)|Novo MIM (4,5. x. x)
|-----|-----|-----|-----|
|Chinês (Simplificado)|zh-CN|zh-Hans|
|Chinês (Taiwan)|zh-TW|zh-Hant|
|Neerlandês|NL-NL|nl|
|Francês|FR-FR|fr|
|Alemão|de-DE|de|
|Italiano|it-IT|it|
|Japonês|ja-JP|ja|
|Português (Portugal)|pt-PT|pt-PT|
|Espanhol|es-ES|es|

## <a name="certificate-management-modern-application"></a>Aplicativo moderno de gerenciamento de certificados  
O aplicativo moderno de gerenciamento de certificados da Microsoft dá suporte aos seguintes idiomas 33. 

|Idioma | [1.0.225.104](https://www.microsoft.com/en-us/download/details.aspx?id=54954) | |
|-----|-----|-----|-----|
|Neerlandês|NL-NL|nl|
|Chinês (Simplificado)|zh-CN|zh-Hans|
|Chinês (Taiwan)|zh-TW|zh-Hant|
|Checo|cs-CZ|cs|
|Dinamarquês|da-DK|da|
|Francês|FR-FR|fr|
|Finlandês|fi-FI|fi|
|Alemão|de-DE|de|
|Grego|el GR|el|
|Húngaro|hu-HU|hu|
|Italiano|it-IT|it|
|Japonês|ja-JP|ja|
|Coreano|ko-KR|ko|
|Norueguês|nb-NO|nb-NO|
|Polaco|pl-PL|pl|
|Português (Portugal)|pt-PT|pt|
|Português (Brasil)|pt-BR|pt-BR|
|Russo|ru-RU|ru|
|Romeno|RO-RO|ro|
|Espanhol|es-ES|es|
|Sueco|SV-SE|sv|
|Turco|tr-TR|tr|

## <a name="next-steps"></a>Próximos passos

- [Primeira implementação](microsoft-identity-manager-deploy.md)
- [Histórico de Versões](reference/version-history.md)
