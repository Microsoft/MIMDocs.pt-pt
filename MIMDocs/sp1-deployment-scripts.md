---
title: "Scripts de Implementação da PAM do MIM2016 SP1"
description: "Esta página faz parte da série de artigos sobre a configuração do Privileged Identity Manager através de scripts. Inclui uma lista dos pressupostos sobre o ambiente."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 07/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: ae8f6a87f57c95e073b40d3cda944c71f1bf7247
ms.sourcegitcommit: 0cb8269f07a5f419d2d1cd760d9cc78b8a1c8aa9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2017
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>Scripts de implementação da PAM do MIM2016 SP1

Neste service pack, introduzimos um conjunto de scripts de implementação para facilitar a implementação da PAM. Estes scripts estão disponíveis no centro de transferências. Antes de tentar utilizar os scripts, é importante verificar se os pressupostos abaixo se aplicam ao seu ambiente.

Pressupostos Importantes:
1. O sistema operativo em todas os computadores é, pelo menos, o Windows Server 2012 R2. Se estiver a experimentar Windows Server 2016 Technical Preview 5, o controlador de domínio do PRIV tem de ser instalado com a compilação TP5.
2. Tem de configurar o DNS para que a resolução de nomes entre os Controladores de Domínio e os servidores de componentes seja automática.
3. Os binários de instalação têm de estar disponíveis localmente nos servidores designados para a instalação do SQL, SharePoint e MIM.
4. O ambiente tem três máquinas dedicadas (físicas ou virtuais) que executam, de forma independente, o CORPDC, o PRIVDC e o PAMSERVER.
5. Para a opção de validação, assume-se que existe um computador cliente dedicado para executar este passo.

>[!NOTE]
>Se ocorrer algum problema durante a execução dos scripts, poderá ter de ver os registos. Todos os registos de script são guardados em %AppData%\MIMPAMInstall. Comprima a pasta para um ficheiro Zip e inclua estes conteúdos, juntamente com os detalhes da operação e do erro, no incidente do suporte.

Pronto para começar com os scripts de implementação da PAM? Comece em [Configurar a PAM através de scripts](./pam/sp1-pam-configure-using-scripts.md).
