---
title: Atualização do FIM 2010 R2 e do MIM 2016 SP2 para Microsoft Identity Manager 2016 Service Pack 2 | Microsoft Docs
description: Saiba como atualizar seus componentes do FIM 2010 R2 ou do MIM 2016 SP2 e, em seguida, instale os componentes que são novos no MIM 2016.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 09/16/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: bdf34be4841b1a911fdb61673e5a3855e66e7320
ms.sourcegitcommit: 323c2748dcc6b6991b1421dd8e3721588247bc17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/04/2019
ms.locfileid: "73568064"
---
# <a name="mim-2016-sp2-upgrade--from-forefront-identity--or-microsoft-identity-manager"></a>Atualização do MIM 2016 SP2 do Forefront Identity ou Microsoft Identity Manager

As organizações podem atualizar para Microsoft Identity Manager 2016 SP2 de versões anteriores do Microsoft Identity Manager ou do Forefront Identity Manager.  Cada seção deste artigo aborda um caminho de atualização com suporte.

Há várias opções de atualização disponíveis. Se você já estiver executando o MIM 2016 e não precisar atualizar a plataforma subjacente (Windows Server, SQL, SharePoint, SCSM DW) ou executar os serviços do MIM usando contas de serviço gerenciado de grupo e não usar os pacotes de idioma do MIM, a opção mais fácil estará no local atualização/instalação do hotfix (. msp). Caso contrário, a instalação completa é recomendada.

> [!IMPORTANT]
> Verifique a seção de [pré-requisitos de software](prepare-server-ws2016.md#software-prerequisites) atualizados antes de instalar o mim 2016 SP2

## <a name="upgrade-from-fim-2010-r2-sp1-or-later-fim-builds"></a>Atualização do FIM 2010 R2 SP1 ou compilações do FIM posterior

> [!NOTE]
> A versão mínima com suporte do Forefront Identity Manager que pode ser atualizada diretamente para o MIM 2016 SP2 é o FIM 2010 R2 SP1 (Build 4.1.3419.0). Não há suporte para a atualização direta para o MIM 2016 SP2 de versões anteriores do FIM. Se você estiver executando compilações do FIM anteriores a 4.1.3419.0, precisará atualizar para o FIM 2010 R2 SP1 antes de atualizar para o MIM 2016 SP2.

1. **Opção 1: instalação completa usando bancos de dados existentes**
    1. Faça uma cópia de backup dos bancos de dados FIMSynchronizationService e FIMService.
    1. Exporte todos os objetos RCDC do serviço FIM e as cadeias de caracteres de recurso RCDC nas quais você fez alterações.
    1. Exportar chaves de criptografia do serviço de sincronização.
    1. Backup miisserver. exe. config, pasta ' Extensions ' do servidor de sincronização, Microsoft. ResourceManagement. Service. exe. config como o instalador do MIM pode substituir alterações personalizadas feitas em arquivos de configuração.
    1. Desinstalar **todos os** componentes do fim, incluindo pacotes de idiomas (para serem desinstalados primeiro).
    1. *Opcional:* Mova os bancos de dados do FIM para outro SQL Server. É recomendável criar aliases do SQL em servidores MIM e usar esses aliases em vez do nome real do SQL Server para facilitar a migração de bancos de servidores do MIM no futuro.
    1. Instale o MIM 2016 SP2 da mídia de instalação do. ISO no mesmo ou em outro servidor após o [Guia de implantação do mim](microsoft-identity-manager-deploy.md), escolha usar bancos de dados existentes quando solicitado, forneça chaves de criptografia do serviço de sincronização exportadas anteriormente.
    1. Examine os arquivos miisserver. exe. config e Microsoft. ResourceManagement. Service. exe. config para obter os redirecionamentos do .net ausentes ou as seções personalizadas que você adicionou.
    1. Instale os pacotes de idiomas do MIM 2016 SP2, se necessário.
    1. Envie novamente as personalizações para objetos RCDC e cadeias de caracteres de recurso RCDC e localizações, se necessário.
    1. Atualize Complementos do FIM e clientes de redefinição de senha, forneça o novo nome do servidor do serviço do MIM se o nome do servidor do serviço do MIM for alterado.
    
## <a name="upgrade-from-previous-mim-2016-builds"></a>Atualização de builds anteriores do MIM 2016
1. Faça uma cópia de backup dos bancos de dados FIMSynchronizationService e FIMService.
1. Exporte todas as localizações de serviço do FIM personalizadas feitas para objetos RCDC e cadeias de caracteres de recurso RCDC.
1. Exportar chaves de criptografia do serviço de sincronização.
1. Backup miisserver. exe. config, pasta ' Extensions ' do servidor de sincronização, Microsoft. ResourceManagement. Service. exe. config como o instalador do MIM pode substituir alterações personalizadas feitas em arquivos de configuração.
1. Desinstale os pacotes de idiomas do MIM se forem usados.
1. Interrompa os serviços do MIM.
1. **Opção 1: atualização in-loco-instalação do hotfix**
    1. Aplicar o [hotfix](https://www.microsoft.com/download/details.aspx?id=100412) do serviço de sincronização do mim 2016 SP2
    1. Aplicar o [hotfix](https://www.microsoft.com/download/details.aspx?id=100412) do serviço mim 2016 SP2
    1. Examine os arquivos miisserver. exe. config e Microsoft. ResourceManagement. Service. exe. config para obter os redirecionamentos do .net ausentes ou as seções personalizadas que devem ser adicionadas.
    1. Instale os pacotes de idiomas do MIM 2016 SP2, se necessário.
    1. Envie novamente as personalizações para objetos RCDC e cadeias de caracteres de recurso RCDC e localizações, se necessário.
    1. Atualize os complementos e clientes de redefinição de senha do MIM 2016.
1. **Opção 2: instalação completa usando bancos de dados existentes**
    1. Desinstale **todos os** componentes do mim.
    1. *Opcional:* Mova os bancos de dados do FIM para outro SQL Server. É recomendável criar aliases do SQL em servidores MIM e usar esses aliases em vez do nome real do SQL Server para facilitar a migração de bancos de servidores do MIM no futuro.
    1. Instale o MIM 2016 SP2 da mídia de instalação do. ISO no mesmo ou em outro servidor após o [Guia de implantação do mim](microsoft-identity-manager-deploy.md), escolha usar bancos de dados existentes quando solicitado, forneça chaves de criptografia do serviço de sincronização exportadas anteriormente.
    1. Examine os arquivos miisserver. exe. config e Microsoft. ResourceManagement. Service. exe. config para obter os redirecionamentos do .net ausentes ou as seções personalizadas que devem ser adicionadas.
    1. Instale os pacotes de idiomas do MIM 2016 SP2, se necessário.
    1. Envie novamente as personalizações para objetos RCDC e cadeias de caracteres de recurso RCDC e localizações, se necessário.
    1. Atualize os complementos e clientes de redefinição de senha do MIM 2016, forneça o novo nome do servidor do serviço do MIM se o nome do servidor do serviço do MIM for alterado.

> [!NOTE]
> As atualizações de pacotes de idiomas após o MIM 2016 SP2 serão distribuídas como hotfixes (arquivos. msp), eliminando a necessidade de desinstalar/reinstalar pacotes de idiomas.

Informações mais detalhadas sobre os procedimentos de backup de atualização e de bancos de dados podem ser encontradas no artigo [atualizar para fim 2010 R2](https://docs.microsoft.com/previous-versions/mim/jj134291%28v%3dws.10%29) , aplicável a qualquer processo de atualização do fim ou do mim.
