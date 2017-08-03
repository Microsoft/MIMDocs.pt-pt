---
title: "Registo Dinâmico do Serviço MIM | Documentos da Microsoft"
description: "Ative o registo dinâmico do serviço MIM sem ser necessário reiniciar o serviço de gestão"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 03/24/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 
ms.openlocfilehash: 1e2fb9a9ae508ab601ebad1dec7acc21dc44d13e
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="mim-sp1-4414360--service-dynamic-logging"></a>Registo Dinâmico do Serviço MIM SP1 (4.4.1436.0)
Na versão 4.4.1436.0 adicionámos uma nova funcionalidade de registo. Esta funcionalidade permite aos engenheiros de suporte e aos administradores ativar o registo sem ser necessário reiniciar o serviço de gestão.

Após a instalação, verá uma nova linha em Microsoft.ResourceManagement.Service.exe.config denominada

*   Linha 6: ``<section name="dynamicLogging" type="Microsoft.ResourceManagement.Utilities.DynamicLoggingSection, Microsoft.ResourceManagement.Service" />``
*   Linha 8: ``<dynamicLogging mode="true" loggingLevel="Verbose" />``
*   Linha 266: ``</system.diagnostics> ``

![Secções realçadas a mostrar as novas entradas do registo dinâmico](media/mim-service-dynamic-logging/screen01.png)

Pode consultar os níveis do registo dinâmico [aqui](https://msdn.microsoft.com/library/ms733025(v=vs.110).aspx#Anchor_3)

- Critical (crítico) = o serviço de nível predefinido só irá escrever eventos críticos
- Atualize a Linha 8 (dynamicLogging mode="true" loggingLevel="Critical") com o valor de registo preferencial

Configuração do registo dinâmico na linha 266: Microsoft.ResourceManagement.Service.exe.config

![Secções realçadas a mostrar linhas com as várias áreas de registo disponíveis](media/mim-service-dynamic-logging/screen02.png)

Por predefinição, a localização do registo será **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service**. Para o registo dinâmico ser gerado, a conta do Serviço FIM precisará de permissão de escrita nesta localização.

![Localização da pasta dos registos](media/mim-service-dynamic-logging/screen03.png)

 >[!NOTE]
 Se ocorrerem erros inesperados (erros de sintaxe no ficheiro de configuração Microsoft.ResourceManagement.Service.exe.config ou outros problemas), será escrita uma mensagem de erro correspondente no ficheiro Microsoft.ResourceManagement.Service.exe_Emergency.log no caminho %TMP%, %TEMP% ou %USERPROFILE% (o primeiro que existir).  
1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

Para ver o rastreio, pode utilizar a [ferramenta Visualizador de Rastreio de Serviços](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![Captura de ecrã do Visualizador de Rastreio de Serviços](media/mim-service-dynamic-logging/screen04.png)
