---
title: Upgrade de FIM 2010 R2 e MIM 2016 SP2 para Microsoft Identity Manager 2016 Service Pack 2  ) Microsoft Docs
description: Saiba como atualizar os seus componentes FIM 2010 R2 ou MIM 2016 SP2 e, em seguida, instale os componentes que são novos em MIM 2016.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: daveba
ms.date: 09/16/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: bfe604795d44cdea61fe0d10bc2943f9b8433784
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044157"
---
# <a name="mim-2016-sp2-upgrade--from-forefront-identity--or-microsoft-identity-manager"></a>Mim 2016 SP2 upgrade da Front Front Identity ou Microsoft Identity Manager

As organizações podem fazer upgrade para o Microsoft Identity Manager 2016 SP2 a partir de versões anteriores do Microsoft Identity Manager ou do Forefront Identity Manager.  Cada secção deste artigo cobre um caminho de atualização suportado.

Existem várias opções de upgrade disponíveis. Se já estiver a executar mim 2016 e não precisar de atualizar a plataforma subjacente (Windows Server, SQL, SharePoint, SCSM DW) ou executar serviços MIM usando contas de serviço geridas pelo grupo, e não utilizar pacotes de idiomas MIM, então a opção mais fácil será a instalação de upgrade /hotfix (.msp). Caso contrário, recomenda-se a instalação completa.

> [!IMPORTANT]
> Verifique a secção de [pré-requisitos](prepare-server-ws2016.md#software-prerequisites) do Software atualizado antes de instalar o MIM 2016 SP2

## <a name="upgrade-from-fim-2010-r2-sp1-or-later-fim-builds"></a>Upgrade a partir de FIM 2010 R2 SP1 ou posteriormente fim construções

> [!NOTE]
> A versão minimalista suportada do Gestor de Identidade da Vanguarda que pode ser diretamente atualizado para MIM 2016 SP2 é FIM 2010 R2 SP1 (construção 4.1.3419.0). Não é suportado o upgrade direto para MIM 2016 SP2 a partir de versões anteriores do FIM. Se estiver a executar as construções DO FIM mais cedo do que 4.1.3419.0 então tem de fazer upgrade para FIM 2010 R2 SP1 antes de fazer upgrade para MIM 2016 SP2.

1. **Opção 1: Instalação completa utilizando bases de dados existentes**
    1. Faça uma cópia de cópia de cópia de cópia de segurança das suas bases de dados FIMSynchronizationService e FIMService.
    1. Exportar todos os objetos RCDC do Serviço FIM e cordas de recursos RCDC a que fez alterações.
    1. Chaves de encriptação do Serviço de Sincronização de Exportação.
    1. A pasta 'Extensões' do Servidor de Sincronização, Microsoft.ResourceManagement.exe.config as Afumterer MIM pode substituir alterações personalizadas feitas em ficheiros config.
    1. Desinstale **todos os** componentes DO, incluindo pacotes de idiomas (para desinstalar primeiro).
    1. *Opcional:* Mova as bases de dados DO FIM para outro servidor SQL. Recomenda-se criar pseudónimos SQL nos servidores MIM e utilizar estes pseudónimos em vez de um nome real de servidor SQL para facilitar a migração de DBs MIM no futuro.
    1. Instale mim 2016 SP2 a partir dos meios de instalação .iso no mesmo ou em outro servidor seguindo o guia de [implementação MIM](microsoft-identity-manager-deploy.md), opte por utilizar bases de dados existentes quando solicitado, fornecer chaves de encriptação do Serviço de Sincronização previamente exportadas.
    1. Reveja os ficheiros miisserver.exe.config e Microsoft.ResourceManagement.Service.exe.config para os redirecionamentos de .net em falta ou quaisquer secções personalizadas que tenha adicionado.
    1. Instale pacotes de idiomas MIM 2016 SP2, se necessário.
    1. Reenvie as personalizações para objetos RCDC e cadeias de recursos RCDC e locais, se necessário.
    1. Atualize os addons DO FIM e os clientes de Reset de Palavras-Passe, forneça um novo nome de servidor do Serviço MIM se o nome do servidor mim service for alterado.
    
## <a name="upgrade-from-previous-mim-2016-builds"></a>Upgrade das construções anteriores do MIM 2016
1. Faça uma cópia de cópia de cópia de cópia de segurança das suas bases de dados FIMSynchronizationService e FIMService.
1. Exportar todas as localidades personalizadas do Serviço FIM feitas para objetos RCDC e cadeias de recursos RCDC.
1. Chaves de encriptação do Serviço de Sincronização de Exportação.
1. A pasta 'Extensões' do Servidor de Sincronização, Microsoft.ResourceManagement.exe.config as Afumterer MIM pode substituir alterações personalizadas feitas em ficheiros config.
1. Desinstale pacotes de idiomas MIM se utilizados.
1. Pare os serviços mim.
1. **Opção 1: Atualização no local - instalação hotfix**
    1. Aplicar [hotfix](https://www.microsoft.com/download/details.aspx?id=100412) do Serviço de Sincronização MIM 2016 SP2
    1. Aplicar [hotfix](https://www.microsoft.com/download/details.aspx?id=100412) de serviço MIM 2016 SP2
    1. Revê miisserver.exe.config e Microsoft.ResourceManagement.Service.exe.config para ficheiros de falta de .net ou quaisquer secções personalizadas que devem ser adicionadas.
    1. Instale pacotes de idiomas MIM 2016 SP2, se necessário.
    1. Reenvie as personalizações para objetos RCDC e cadeias de recursos RCDC e locais, se necessário.
    1. Atualize os clientes de Add-ons mim 2016 e reset de palavra-passe.
1. **Opção 2: Instalação completa utilizando bases de dados existentes**
    1. Desinstale **todos os** componentes MIM.
    1. *Opcional:* Mova as bases de dados DO FIM para outro servidor SQL. Recomenda-se criar pseudónimos SQL nos servidores MIM e utilizar estes pseudónimos em vez de um nome real de servidor SQL para facilitar a migração de DBs MIM no futuro.
    1. Instale mim 2016 SP2 a partir dos meios de instalação .iso no mesmo ou em outro servidor seguindo o guia de [implementação MIM](microsoft-identity-manager-deploy.md), opte por utilizar bases de dados existentes quando solicitado, fornecer chaves de encriptação do Serviço de Sincronização previamente exportadas.
    1. Revê miisserver.exe.config e Microsoft.ResourceManagement.Service.exe.config para ficheiros de falta de .net ou quaisquer secções personalizadas que devem ser adicionadas.
    1. Instale pacotes de idiomas MIM 2016 SP2, se necessário.
    1. Reenvie as personalizações para objetos RCDC e cadeias de recursos RCDC e locais, se necessário.
    1. Atualize os clientes Add-ons e Password Reset da MIM 2016, forneça um novo nome de servidor do Serviço MIM se o nome do servidor do Serviço MIM for alterado.

> [!NOTE]
> As atualizações do Idiom Packs após o MIM 2016 SP2 serão distribuídas como hotfixes (.ficheiros msp), eliminando a necessidade de desinstalar/reinstalar pacotes de idiomas.

Informações mais detalhadas sobre os procedimentos de backup de upgrade e bases de dados podem ser encontradas no artigo de [Upgrade para FIM 2010 R2,](https://docs.microsoft.com/previous-versions/mim/jj134291%28v%3dws.10%29) que é aplicável a qualquer processo de upgrade FIM ou MIM.
