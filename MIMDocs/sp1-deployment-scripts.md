---
title: Scripts de implantação MIM PAM
description: Esta página faz parte da série de artigos sobre configurar o Microsoft Identity Manager usando scripts. Inclui uma lista dos pressupostos sobre o ambiente.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/17/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: e7eea2c72df3ca9893acc5f6989afa9c488f384e
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010460"
---
# <a name="mim-pam-deployment-scripts"></a>Scripts de implantação MIM PAM

O Pacote de Serviços MIM 2016 introduziu um conjunto de scripts de implementação para facilitar a implementação do PAM. Estes scripts estão disponíveis no centro de descarregamento. Antes de tentar utilizar os scripts, é importante que certifique-se de que cumpre os requisitos abaixo:

1. O sistema operativo em todos os servidores é pelo menos o Windows Server 2012 R2.
2. O DNS deve ser configurado para permitir a resolução de nomes entre os controladores de domínio e os servidores de componentes.
3. Os binários de instalação têm de estar disponíveis localmente nos servidores designados para a instalação do SQL, SharePoint e MIM.
4. O ambiente tem três máquinas dedicadas (físicas ou virtuais) que executam, de forma independente, o CORPDC, o PRIVDC e o PAMSERVER.
5. Para a opção de validação, você precisa ter uma estação de trabalho dedicada.

>[!NOTE]
>Se ocorrer algum problema durante a execução dos scripts, poderá ter de ver os registos. Todos os registos de script são guardados em %AppData%\MIMPAMInstall. Comprima a pasta para um ficheiro Zip e inclua estes conteúdos, juntamente com os detalhes da operação e do erro, no incidente do suporte.

Pronto para começar com os scripts de implementação da PAM? Comece em [Configurar a PAM através de scripts](./pam/sp1-pam-configure-using-scripts.md).
