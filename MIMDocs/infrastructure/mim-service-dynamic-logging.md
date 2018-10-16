---
title: Registo Dinâmico do Serviço MIM | Documentos da Microsoft
description: Ative o registo dinâmico do serviço MIM sem ser necessário reiniciar o serviço de gestão
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 06/25/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: ''
ms.openlocfilehash: ff82b2fce31abe417509347ce7b477dd1b4056f2
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332342"
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

Configuração do registo dinâmico localizado na linha 266: Microsoft

![Secções realçadas a mostrar linhas com as várias áreas de registo disponíveis](media/mim-service-dynamic-logging/screen02.png)

Por predefinição a localização do registo será a * * C:\Program Files\Microsoft Forefront Identity Manager\2010\Service, o serviço de FIM conta terá de permissão de escrita a esta localização para o registo dinâmico.

![Localização da pasta dos registos](media/mim-service-dynamic-logging/screen03.png)

> [!NOTE]
>  Se ocorrerem erros inesperados (erros de sintaxe no ficheiro de configuração Microsoft.ResourceManagement.Service.exe.config ou outros problemas), será escrita uma mensagem de erro correspondente no ficheiro Microsoft.ResourceManagement.Service.exe_Emergency.log no caminho %TMP%, %TEMP% ou %USERPROFILE% (o primeiro que existir).  
> 1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

Para ver o rastreio, pode usar o [ferramenta Visualizador de rastreio do serviço](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)

 ![Captura de ecrã do Visualizador de Rastreio de Serviços](media/mim-service-dynamic-logging/screen04.png)

# <a name="updates-build-45xx-or-greater"></a>Atualizações: Criar 4.5.x.x ou superior

Na compilação 4.5.x.x ter revimos a funcionalidade de registo para especificar o nível de registo predefinido é **"Aviso"**. O serviço escreve mensagens em dois arquivos ("00" e "01" índices são adicionados antes da extensão). Os ficheiros estão localizados no diretório de "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service". Quando o ficheiro excede o tamanho máximo é iniciado o serviço de escrita no outro ficheiro. Se existir outro ficheiro, esta será substituída. Tamanho máximo predefinido do arquivo é de 1 GB. Para alterar o tamanho máximo predefinido, é necessário adicionar **"maxOutputFileSizeKB"** parâmetro com o valor de tamanho de ficheiro máximo em KB para o serviço de escuta (veja o exemplo abaixo) e reinicie o serviço MIM. Quando o serviço é iniciado, ele anexa registos no ficheiro mais recente (se for excedido o limite de espaço substituir o ficheiro mais antigo). 

> [!NOTE] Como o tamanho de ficheiro de verificação de serviço antes da mensagem é gravada, o tamanho do ficheiro pode ser superior ao tamanho máximo para o tamanho de uma mensagem. Por predefinição, o tamanho dos logs de pode ser aproximadamente 6 GB (três > Serviços de escuta com dois ficheiros para o tamanho de um GB).

> [!NOTE] A conta de serviço deve ter permissões para escrever > "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service" > diretório. No caso da conta de serviço não tem esses direitos, o > não serão criados ficheiros.

Exemplo como definir o tamanho de ficheiro máximo como 200 MB (200 * 1024 KB) para ficheiros de svclog e 100 MB * (100 * 1024 KB) para ficheiros txt

`<add initializeData="Microsoft.ResourceManagement.Service_tracelog.svclog" type="Microsoft.IdentityManagement.CircularTraceListener.CircularXmlTraceListener, Microsoft.IdentityManagement.CircularTraceListener, PublicKeyToken=31bf3856ad364e35" name="ServiceModelTraceListener" traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" maxOutputFileSizeKB="204800">`
