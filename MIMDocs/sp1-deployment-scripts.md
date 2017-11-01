---
title: "Scripts de Implementação da PAM do MIM2016 SP1"
description: "Esta página faz parte da série de artigos sobre a configuração do Privileged Identity Manager através de scripts. Inclui uma lista dos pressupostos sobre o ambiente."
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/17/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 77a222c0a36f4e244a5114eddfc0edadb168d1cd
ms.sourcegitcommit: 06add1a636720f74bc0c0f25b4100b19f1bd31da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2017
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>Scripts de implementação da PAM do MIM2016 SP1

Neste service pack, introduzimos um conjunto de scripts de implementação para facilitar a implementação da PAM. Estes scripts estão disponíveis no centro de transferências. Antes de tentar a utilizar os scripts é importante que, certificar-se de que cumpre os requisitos abaixo:

1. O sistema operativo em todos os servidores é, pelo menos, Windows Server 2012 R2.
2. DNS deve ser configurado para permitir a resolução de nome entre os controladores de domínio e os servidores de componente.
3. Os binários de instalação têm de estar disponíveis localmente nos servidores designados para a instalação do SQL, SharePoint e MIM.
4. O ambiente tem três máquinas dedicadas (físicas ou virtuais) que executam, de forma independente, o CORPDC, o PRIVDC e o PAMSERVER.
5. Para a opção de validação tem de ter uma estação de trabalho dedicada.

>[!NOTE]
>Se ocorrer algum problema durante a execução dos scripts, poderá ter de ver os registos. Todos os registos de script são guardados em %AppData%\MIMPAMInstall. Comprima a pasta para um ficheiro Zip e inclua estes conteúdos, juntamente com os detalhes da operação e do erro, no incidente do suporte.

Pronto para começar com os scripts de implementação da PAM? Comece em [Configurar a PAM através de scripts](./pam/sp1-pam-configure-using-scripts.md).
