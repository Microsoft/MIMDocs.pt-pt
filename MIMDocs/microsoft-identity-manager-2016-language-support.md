---
title: Idiomas Suportados do Microsoft Identity Manager 2016 SP1 [ Microsoft Docs
description: Uma lista de idiomas que são suportados pelo Microsoft Identity Manager 2016 SP1.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 05/23/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.openlocfilehash: 2caf9f06067c229d585019f912a7ff4e00fad3e6
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044128"
---
# <a name="supported-languages"></a>Linguagens suportadas

Este artigo descreve os idiomas suportados e o mapeamento de atualizações do Microsoft Identity Manager 2016 sP1 versão 4.5.x ou superior.

## <a name="mim-service-and-portal-and-add-ins-and-extensions-language-pack"></a>Pacote de linguagem de serviço mim e portal e add-ins e extensões 

O Microsoft MIM Service e portal Language Pack suportam os seguintes idiomas 33 idiomas.  

> [!NOTE]
> Em [4.4.1642.0](https://support.microsoft.com/en-us/help/4021562/hotfix-rollup-package-build-4-4-1642-0-is-available-for-microsoft) foi adicionada uma chave de registo chamada "OverrideDefaultUILocale" ao pacote de idiomas MIM Add-ins e Extensions tentará mapear todas as línguas semelhantes às que são suportadas. Por exemplo, se a Linguagem de Exibição do Windows\*for ES-CL (Chile espanhol), ou qualquer ES- tentará mapear isso para ES-ES (Espanha espanhola).

> [!IMPORTANT]
> O texto no add-in e portal sspr será localizado, mas as questões não serão sem trabalho adicional. Você precisará criar fluxos de trabalho AuthN (e conjuntos e MPRs que acompanham para direcioná-los) para direcionar as questões em cada idioma para a localização alvo.

|       Idioma        | FIM (4.3.x.x)/MIM (4.4.xx) | MIM (4.5.x.x) |
|-----------------------|--------------------------|--------------|
|       Búlgaro       |          bg-BG           |      bg      |
| Chinês (Simplificado)  |          zh-CN           |   zh-hans    |
|   Chinês (Taiwan)    |          zh-TW           |   zh-hant    |
|       Croata        |          hr-HR           |      hr      |
|         Checo         |          cs-CZ           |      cs      |
|        Dinamarquês         |          da-DK           |      da      |
|         Neerlandês         |          nl-NL           |      nl      |
|       Estónio        |          et-EE           |      et      |
|        Francês         |          fr-FR           |      fr      |
|        Finlandês        |          fi-FI           |      fi      |
|        Alemão         |          de-DE           |      de      |
|         Grego         |          el-GR           |      el      |
|         Hindi         |          hi-IN           |      Olá      |
|       Húngaro       |          hu-HU           |      hu      |
|        Italiano        |          it-IT           |      lo      |
|       Japonês        |          ja-JP           |      ja      |
|        Coreano         |          ko-KR           |      ko      |
|      Lituano       |          lt-LT           |      lt      |
|        Letão        |          lv-LV           |      lv      |
|       Norueguês       |          nb-NO           |    nb-NO     |
|        Polaco         |          pl-PL           |      pl      |
| Português (Portugal) |          pt-PT           |      pt      |
|  Português (Brasil)  |          pt-BR           |    pt-BR     |
|        Russo        |          ru-RU           |      ru      |
|       Romeno        |          ro-RO           |      ro      |
|        Espanhol        |          es-ES           |      es      |
|        Eslovaco         |          sk-SK           |      sk      |
|        Sueco        |          sv-SE           |      sv      |
|       Esloveno       |          sl-SI           |      sl      |
|   Sérvio - Sérvia    |  sr-latn-CS (depricado)  |  sr-Latn-RS  |
|         Tailandês          |          th-TH           |      th      |
|        Turco        |          tr-TR           |      tr      |
|       Ucraniano       |          uk-UA           |      Reino Unido      |

## <a name="certificate-management"></a>Gestão de Certificados 
A Microsoft Certificate Management suporta os seguintes 9 idiomas. 

|Idioma|FIM (4.3.x.x)/MIM (4.4.xx)|Novo MIM (4.5.x.x)
|-----|-----|-----|-----|
|Chinês (Simplificado)|zh-CN|zh-hans|
|Chinês (Taiwan)|zh-TW|zh-hant|
|Neerlandês|nl-NL|nl|
|Francês|fr-FR|fr|
|Alemão|de-DE|de|
|Italiano|it-IT|lo|
|Japonês|ja-JP|ja|
|Português (Portugal)|pt-PT|pt-PT|
|Espanhol|es-ES|es|

## <a name="certificate-management-modern-application"></a>Aplicação moderna de Gestão de Certificados  
A Aplicação Moderna de Gestão de Certificados da Microsoft suporta os seguintes 33 idiomas. 

|Idioma | [1.0.225.104](https://www.microsoft.com/en-us/download/details.aspx?id=54954) | |
|-----|-----|-----|-----|
|Neerlandês|nl-NL|nl|
|Chinês (Simplificado)|zh-CN|zh-hans|
|Chinês (Taiwan)|zh-TW|zh-hant|
|Checo|cs-CZ|cs|
|Dinamarquês|da-DK|da|
|Francês|fr-FR|fr|
|Finlandês|fi-FI|fi|
|Alemão|de-DE|de|
|Grego|el-GR|el|
|Húngaro|hu-HU|hu|
|Italiano|it-IT|lo|
|Japonês|ja-JP|ja|
|Coreano|ko-KR|ko|
|Norueguês|nb-NO|nb-NO|
|Polaco|pl-PL|pl|
|Português (Portugal)|pt-PT|pt|
|Português (Brasil)|pt-BR|pt-BR|
|Russo|ru-RU|ru|
|Romeno|ro-RO|ro|
|Espanhol|es-ES|es|
|Sueco|sv-SE|sv|
|Turco|tr-TR|tr|

## <a name="next-steps"></a>Passos seguintes

- [Primeira implantação](microsoft-identity-manager-deploy.md)
- [História da versão](reference/version-history.md)
