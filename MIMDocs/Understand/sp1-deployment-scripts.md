---
title: "Scripts de Implementação da PAM do MIM2016 SP1"
description: "Preparar o domínio CORP com identidades novas ou existentes para ser gerido pelo Privileged Identity Manager através de scripts"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: c7c5266f3d1c51e933855031f4128cbcb967d6e2
ms.openlocfilehash: 43a176ed2f1375eb98851064c460515ec09de132


---

# Scripts de implementação da PAM do MIM2016 SP1

Neste service pack, introduzimos um conjunto de scripts de implementação para facilitar a implementação da PAM. Estes scripts estão disponíveis no centro de transferências. Antes de tentar utilizar os scripts, é importante verificar se os pressupostos abaixo se aplicam ao seu ambiente.

Pressupostos Importantes:
1. O sistema operativo em todas os computadores é, pelo menos, o Windows Server 2012 R2. Se estiver a experimentar Windows Server 2016 Technical Preview 5, o controlador de domínio do PRIV tem de ser instalado com a compilação TP5.
2. Tem de configurar o DNS para que a resolução de nomes entre os Controladores de Domínio e os servidores de componentes seja automática.
3. Os binários de instalação têm de estar disponíveis localmente nos servidores designados para a instalação do SQL, SharePoint e MIM.
4. O ambiente tem três máquinas dedicadas (físicas ou virtuais) que executam, de forma independente, o CORPDC, o PRIVDC e o PAMSERVER.
5. Para a opção de validação, assume-se que existe um computador cliente dedicado para executar este passo.

>[!NOTE] Se ocorrer algum problema durante a execução dos scripts, poderá ter de ver os registos. Todos os registos de script são guardados em %AppData%\MIMPAMInstall. Comprima a pasta para um ficheiro Zip e envie por e-mail para mim2016@microsoft.com, juntamente com os detalhes da operação e do erro.



<!--HONumber=Sep16_HO4-->


