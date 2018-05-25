---
title: Idiomas do Microsoft Identity Manager 2016 SP1 suportados | Microsoft Docs
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
ms.openlocfilehash: 101f4110d6439cfc17f25c8880531e18f51920da
ms.sourcegitcommit: 19ed53fa6af61086774a0429fd1520067caf4e93
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/23/2018
---
# <a name="supported-languages"></a>Idiomas suportados

Este artigo descreve os idiomas suportados e o mapeamento das atualizações da versão SP1 do Microsoft Identity Manager 2016 4.5.x ou superior.

## <a name="mim-service-and-portal-and-add-ins-and-extensions-language-pack"></a>Portal e serviço MIM e o pacote de idiomas de extensões e suplementos 

Pacote de idiomas do Portal e do serviço de MIM do Microsoft suportam os seguintes 33 idiomas idiomas.  

> [!NOTE]
> No [4.4.1642.0](https://support.microsoft.com/en-us/help/4021562/hotfix-rollup-package-build-4-4-1642-0-is-available-for-microsoft) uma chave de registo foi adicionada chamado "OverrideDefaultUILocale" para suplementos de MIM e o pacote de idiomas de extensões irá tentar mapear todos os idiomas semelhantes ao que é suportado. Por exemplo, se o idioma de apresentação do Windows é ES-CL (Espanhol subordinado), ou qualquer ES-* *, irá tentar mapear isto para ES-ES (Espanhol (Espanha)).

> [!IMPORTANT]
> O texto do suplemento do SSPR e o portal irá ser localizado, mas as perguntas serão não sem fazer tarefas adicionais. Terá de criar AuthN fluxos de trabalho (e associada conjuntos e MPRs para segmentá-los) às perguntas de destino em cada idioma para a localização de destino.

|idioma|FIM(4.3.x.x)/MIM(4.4.XX)|MIM(4.5.x.x)
|-----|-----|-----|
|Búlgaro|bg-BG|BG|
|Chinês (simplificado)|zh-CN|zh-hans|
|Chinês (Taiwan)|zh-TW|zh-hant|
|Croata|HR de RH|hr|
|Checo|cs-CZ|CS|
|Dinamarquês|da-DK|da|
|Neerlandês|NL-NL|NL|
|Estónio|Definir EE|definir|
|Francês|FR-FR|FR|
|Finlandês|fi-FI|Fi|
|Alemão|Alemanha da Alemanha|Alemanha|
|Grego|el GR|EL|
|Hindi|IN|Olá|
|Húngaro|hu-HU|HU|
|Italiano|it-IT|-la|
|Japonês|ja-JP|ja|
|Coreano|ko-KR|ko|
|Lituano|lt-LT|lt|
|Letão|lv-LV|LV|
|Norueguês|nb-NO|nb-NO|
|Polaco|pl-PL|LP|
|Português (Portugal)|pt-PT|pt|
|Português (Brasil)|pt-BR|pt-BR|
|Russo|ru-RU|RU||SV|
|Romeno|RO RO|ro|
|Espanhol|es-ES|es|
|Eslovaco|sk SK|SK|
|Sueco|SV-SE|SV|
|Esloveno|TAMA SL|SL|
|Sérvio - Serbia |sr-latn-CS(Depricated)|SR-Latn-RS|
|Tailandês|ésimo-Ésimo|ésimo|
|Turco|tr-TR|TR|
|Ucraniano|uk-UA|RU|

## <a name="certificate-management"></a>Gestão de Certificados 
A gestão de certificados do Microsoft suporta os seguintes 9 idiomas. 

|idioma|FIM(4.3.x.x)/MIM(4.4.XX)|Novo MIM(4.5.x.x)
|-----|-----|-----|-----|
|Chinês (simplificado)|zh-CN|zh-hans|
|Chinês (Taiwan)|zh-TW|zh-hant|
|Neerlandês|NL-NL|NL|
|Francês|FR-FR|FR|
|Alemão|Alemanha da Alemanha|Alemanha|
|Italiano|it-IT|-la|
|Japonês|ja-JP|ja|
|Português (Portugal)|pt-PT|pt-PT|
|Espanhol|es-ES|es|

## <a name="certificate-management-modern-application"></a>Aplicações modernas de gestão de certificados  
Aplicações modernas de gestão de certificados do Microsoft suporta os seguintes 33 idiomas. 

|idioma | [1.0.225.104](https://www.microsoft.com/en-us/download/details.aspx?id=54954) | |
|-----|-----|-----|-----|
|Neerlandês|NL-NL|NL|
|Chinês (simplificado)|zh-CN|zh-hans|
|Chinês (Taiwan)|zh-TW|zh-hant|
|Checo|cs-CZ|CS|
|Dinamarquês|da-DK|da|
|Francês|FR-FR|FR|
|Finlandês|fi-FI|Fi|
|Alemão|Alemanha da Alemanha|Alemanha|
|Grego|el GR|EL|
|Húngaro|hu-HU|HU|
|Italiano|it-IT|-la|
|Japonês|ja-JP|ja|
|Coreano|ko-KR|ko|
|Norueguês|nb-NO|nb-NO|
|Polaco|pl-PL|LP|
|Português (Portugal)|pt-PT|pt|
|Português (Brasil)|pt-BR|pt-BR|
|Russo|ru-RU|RU|
|Romeno|RO RO|ro|
|Espanhol|es-ES|es|
|Sueco|SV-SE|SV|
|Turco|tr-TR|TR|

## <a name="next-steps"></a>Próximos passos

- [Primeira implementação](microsoft-identity-manager-deploy.md)
- [Histórico da versão](/reference/version-history.md)