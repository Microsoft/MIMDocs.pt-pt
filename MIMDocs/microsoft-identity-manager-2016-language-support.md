---
title: Idiomas do Microsoft Identity Manager 2016 SP1 suportados | Documentos da Microsoft
description: Uma lista de idiomas que são suportadas pelo Microsoft Identity Manager 2016 SP1.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 05/23/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.openlocfilehash: bb0287b894786d13398819b04bdb089f0f36b33e
ms.sourcegitcommit: acb2c61831cb634278acc439d6d9496ff51a6a54
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/04/2018
ms.locfileid: "43694644"
---
# <a name="supported-languages"></a>Idiomas suportados

Este artigo descreve os idiomas suportados e o mapeamento de atualizações de versão do Microsoft Identity Manager 2016 SP1 4.5.x ou superior.

## <a name="mim-service-and-portal-and-add-ins-and-extensions-language-pack"></a>Portal e serviço MIM e o pacote de idiomas de extensões e suplementos 

O pacote de idiomas do Portal e do serviço de MIM de Microsoft suportam os seguintes idiomas 33 idiomas.  

> [!NOTE]
> Na [4.4.1642.0](https://support.microsoft.com/en-us/help/4021562/hotfix-rollup-package-build-4-4-1642-0-is-available-for-microsoft) uma chave de registo foi adicionada o chamado "OverrideDefaultUILocale" para suplementos de MIM e o pacote de idiomas de extensões irá tentar mapear todos os idiomas semelhante para aquele que é suportado. Por exemplo, se o idioma de apresentação do Windows for ES-CL (Espanhol Chile), ou qualquer ES -\*, ele tentará mapeá-la para ES-ES (espanhol de Espanha).

> [!IMPORTANT]
> O texto no suplemento do SSPR e o portal será localizado, mas as perguntas serão não sem trabalho adicional. Terá de criar AuthN fluxos de trabalho (e que acompanha este artigo e conjuntos de MPRs abordadas) para as perguntas de destino em cada linguagem para a localização de destino.

|       Idioma        | FIM(4.3.x.x)/MIM(4.4.XX) | MIM(4.5.x.x) |
|-----------------------|--------------------------|--------------|
|       Búlgaro       |          bg-BG           |      BG      |
| Chinês (simplificado)  |          zh-CN           |   zh hans    |
|   Chinês (Taiwan)    |          zh-TW           |   zh-hant    |
|       Croata        |          RH de RH           |      RH      |
|         Checo         |          cs-CZ           |      CS      |
|        Dinamarquês         |          da-DK           |      – pacote de iniciação      |
|         Holandês         |          NL-NL           |      NL      |
|       Estónio        |          et EE           |      et      |
|        Francês         |          FR-FR           |      FR      |
|        Finlandês        |          fi-FI           |      Fi      |
|        Alemão         |          de-DE           |      Alemanha      |
|         Grego         |          el GR           |      EL      |
|         Híndi         |          IN           |      Olá!      |
|       Húngaro       |          hu-HU           |      HU      |
|        Italiano        |          it-IT           |      ele      |
|       Japonês        |          ja-JP           |      ja      |
|        Coreano         |          ko-KR           |      ko      |
|      Lituano       |          lt-LT           |      lt      |
|        Letão        |          lv-LV           |      LV      |
|       Norueguês       |          nb-NO           |    nb-NO     |
|        Polaco         |          pl-PL           |      PL      |
| Português (Portugal) |          pt-PT           |      Hora do Pacífico      |
|  Português (Brasil)  |          pt-BR           |    pt-BR     |
|        Russo        |          ru-RU           |      RU      |
|       Romeno        |          RO-RO           |      ro      |
|        Espanhol        |          es-ES           |      es      |
|        Eslovaco         |          sk SK           |      SK      |
|        Sueco        |          SV-SE           |      SV      |
|       Esloveno       |          IS SL           |      SL      |
|   Sérvio – Sérvia    |  sr-latn-CS(Depricated)  |  SR-Latn-RS  |
|         Tailandês          |          th-TH           |      TH      |
|        Turco        |          tr-TR           |      TR      |
|       Ucraniano       |          uk-UA           |      Reino Unido      |

## <a name="certificate-management"></a>Gestão de Certificados 
A gestão de certificados da Microsoft suporta os seguintes 9 idiomas. 

|Idioma|FIM(4.3.x.x)/MIM(4.4.XX)|Novo MIM(4.5.x.x)
|-----|-----|-----|-----|
|Chinês (simplificado)|zh-CN|zh hans|
|Chinês (Taiwan)|zh-TW|zh-hant|
|Holandês|NL-NL|NL|
|Francês|FR-FR|FR|
|Alemão|de-DE|Alemanha|
|Italiano|it-IT|ele|
|Japonês|ja-JP|ja|
|Português (Portugal)|pt-PT|pt-PT|
|Espanhol|es-ES|es|

## <a name="certificate-management-modern-application"></a>Aplicação moderna de gestão de certificado  
A aplicação moderna do certificado de gestão de Microsoft suporta os seguintes 33 idiomas. 

|Idioma | [1.0.225.104](https://www.microsoft.com/en-us/download/details.aspx?id=54954) | |
|-----|-----|-----|-----|
|Holandês|NL-NL|NL|
|Chinês (simplificado)|zh-CN|zh hans|
|Chinês (Taiwan)|zh-TW|zh-hant|
|Checo|cs-CZ|CS|
|Dinamarquês|da-DK|– pacote de iniciação|
|Francês|FR-FR|FR|
|Finlandês|fi-FI|Fi|
|Alemão|de-DE|Alemanha|
|Grego|el GR|EL|
|Húngaro|hu-HU|HU|
|Italiano|it-IT|ele|
|Japonês|ja-JP|ja|
|Coreano|ko-KR|ko|
|Norueguês|nb-NO|nb-NO|
|Polaco|pl-PL|PL|
|Português (Portugal)|pt-PT|Hora do Pacífico|
|Português (Brasil)|pt-BR|pt-BR|
|Russo|ru-RU|RU|
|Romeno|RO-RO|ro|
|Espanhol|es-ES|es|
|Sueco|SV-SE|SV|
|Turco|tr-TR|TR|

## <a name="next-steps"></a>Passos Seguintes

- [Primeira implementação](microsoft-identity-manager-deploy.md)
- [Histórico de versões](/reference/version-history.md)
