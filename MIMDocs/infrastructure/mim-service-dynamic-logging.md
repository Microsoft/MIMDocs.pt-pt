---
title: Registo Dinâmico do Serviço MIM | Documentos da Microsoft
description: Ative o registo dinâmico do serviço MIM sem ser necessário reiniciar o serviço de gestão
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 90ef2ab63be3914d1d48c7319821177e7e62f9e0
ms.sourcegitcommit: 65e11fd639464ed383219ef61632decb69859065
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2019
ms.locfileid: "68701305"
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

Configuração de log dinâmico localizada na linha 266: Microsoft. ResourceManagement. Service. exe. config

![Secções realçadas a mostrar linhas com as várias áreas de registo disponíveis](media/mim-service-dynamic-logging/screen02.png)

Por padrão, o local de log estará em * * C:\Arquivos de Programas\microsoft Forefront Identity Manager\2010\Service, a conta de serviço do FIM precisará de permissão de gravação para esse local para gerar o log dinâmico.

![Localização da pasta dos registos](media/mim-service-dynamic-logging/screen03.png)

> [!NOTE]
>  Se ocorrerem erros inesperados (erros de sintaxe no ficheiro de configuração Microsoft.ResourceManagement.Service.exe.config ou outros problemas), será escrita uma mensagem de erro correspondente no ficheiro Microsoft.ResourceManagement.Service.exe_Emergency.log no caminho %TMP%, %TEMP% ou %USERPROFILE% (o primeiro que existir).  
> 1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

Para exibir o rastreamento, você pode usar a [ferramenta Visualizador de rastreamento de serviço](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![Captura de ecrã do Visualizador de Rastreio de Serviços](media/mim-service-dynamic-logging/screen04.png)

# <a name="updates-build-45xx-or-greater"></a>Actualiza Build 4.5. x. x ou superior

Na Build 4.5. x. x, revisamos o recurso de log para especificar o nível de log padrão é **"Warning"** . O serviço grava mensagens em dois arquivos (os índices "00" e "01" são adicionados antes da extensão). Os arquivos estão localizados no diretório "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service". Quando o arquivo excede o tamanho máximo, o serviço começa a gravar em outro arquivo. Se outro arquivo existir, ele será substituído. O tamanho máximo padrão do arquivo é 1 GB. Para alterar o tamanho máximo padrão, é necessário adicionar o parâmetro **"maxOutputFileSizeKB"** com o valor de tamanho de arquivo máximo em KB no ouvinte (Veja o exemplo abaixo) e reiniciar o serviço mim. Quando o serviço é iniciado, ele acrescenta logs em um arquivo mais recente (se o limite de espaço for excedido, ele substituirá o arquivo mais antigo). 

> [!NOTE] 
> Como o tamanho do arquivo de verificação de serviço antes da mensagem ser gravada, o tamanho do arquivo pode ser maior que o tamanho máximo para o tamanho de uma mensagem. Por padrão, o tamanho dos logs pode ser de aproximadamente 6 GB (três > ouvintes com dois arquivos para um tamanho de GB).

> [!NOTE] 
> A conta de serviço deve ter permissões para gravar no diretório > "C:\Arquivos de Programas\microsoft Forefront Identity Manager\2010\Service" >. Caso a conta de serviço não tenha tais direitos, os arquivos de > não serão criados.

Exemplo de como definir o tamanho máximo do arquivo para 200 MB (200 * 1024 KB) para arquivos svclog Connector e 100 MB * (100 * 1024 KB) para arquivos txt

`<add initializeData="Microsoft.ResourceManagement.Service_tracelog.svclog" type="Microsoft.IdentityManagement.CircularTraceListener.CircularXmlTraceListener, Microsoft.IdentityManagement.CircularTraceListener, PublicKeyToken=31bf3856ad364e35" name="ServiceModelTraceListener" traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" maxOutputFileSizeKB="204800">`
