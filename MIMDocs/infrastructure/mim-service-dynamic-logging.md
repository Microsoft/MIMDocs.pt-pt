---
title: Registo Dinâmico do Serviço MIM | Documentos da Microsoft
description: Ative o registo dinâmico do serviço MIM sem ser necessário reiniciar o serviço de gestão
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/29/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 69ebe774ddea0176fb26ef74b8f4352e4bb5d039
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042173"
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

Config de exploração madeireira dinâmica localizada na linha 266: Microsoft.ResourceManagement.Service.exe.config

![Secções realçadas a mostrar linhas com as várias áreas de registo disponíveis](media/mim-service-dynamic-logging/screen02.png)

Por defeito, a localização de registo será no **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service, a conta do Serviço FIM precisará de uma permissão de escrita para este local para gerar o registo dinâmico.

![Localização da pasta dos registos](media/mim-service-dynamic-logging/screen03.png)

> [!NOTE]
>  Se ocorrerem erros inesperados (erros de sintaxe no ficheiro de configuração Microsoft.ResourceManagement.Service.exe.config ou outros problemas), será escrita uma mensagem de erro correspondente no ficheiro Microsoft.ResourceManagement.Service.exe_Emergency.log no caminho %TMP%, %TEMP% ou %USERPROFILE% (o primeiro que existir).  
> 1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

Para ver o vestígio, pode utilizar a ferramenta do [visualizador Do Rastreio](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx) de Serviço

 ![Captura de ecrã do Visualizador de Rastreio de Serviços](media/mim-service-dynamic-logging/screen04.png)

## <a name="updates-build-45xx-or-greater"></a>Atualizações: Construa 4.5.x.x ou maior

Na construção 4.5.x.x Revimos a função de registo para especificar que o nível de registo predefinido é **"Aviso".** O serviço escreve mensagens em dois ficheiros (os índices "00" e "01" são adicionados antes da extensão). Os ficheiros estão localizados no diretório "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service". Quando o ficheiro excede o tamanho máximo, o serviço começa a escrever noutro ficheiro. Se outro ficheiro existir, será substituído. O tamanho máximo padrão do ficheiro é de 1 GB. Para alterar o tamanho máximo predefinido, é necessário adicionar o parâmetro **"maxOutputFileSizeKB"** com valor do tamanho máximo do ficheiro kB no ouvinte (ver exemplo abaixo) e reiniciar o Serviço MIM. Quando o serviço é iniciado, apresta registos em ficheiros mais recentes (se o limite de espaço for ultrapassado, substitui o ficheiro mais antigo). 

> [!NOTE] 
> Como o tamanho do ficheiro de verificação de serviço antes da mensagem ser escrita, o tamanho do ficheiro pode ser maior do que o tamanho máximo para o tamanho de uma mensagem. por padrão, o tamanho dos registos pode ser de aproximadamente 6 GB (três ouvintes >com dois ficheiros para um tamanho GB).

> [!NOTE] 
> A conta de serviço deve ter permissões para escrever em > "C:\Program Files\Microsoft Forefront Identity Manager\2010\Service" >diretório. Caso a conta de serviço não tenha esses direitos, os ficheiros >não serão criados.

Exemplo como definir o tamanho máximo do ficheiro para 200 MB (200 * 1024 KB) para ficheiros de svclog e 100 MB *(100 * 1024 KB) para ficheiros txt

`<add initializeData="Microsoft.ResourceManagement.Service_tracelog.svclog" type="Microsoft.IdentityManagement.CircularTraceListener.CircularXmlTraceListener, Microsoft.IdentityManagement.CircularTraceListener, PublicKeyToken=31bf3856ad364e35" name="ServiceModelTraceListener" traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" maxOutputFileSizeKB="204800">`
