---
title: Descrição geral do ambiente do PAM | Documentos da Microsoft
description: Localizar o número e a configuração necessários de máquinas virtuais a implementar Privileged Access Management com êxito
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: c758bd924c6f7272a44567c2e9dc919215cdd273
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044009"
---
# <a name="environment-overview"></a>Descrição geral do ambiente

Privileged Access Management trabalha com as máquinas virtuais (VMs) com unidades separadas que estão ligadas entre si numa rede partilhada. Estas máquinas virtuais podem ser alojadas pelo Windows 8.1, pelo Windows Server 2012 R2 ou por outras plataformas de sistema operativo.

![Servidores de PAM: relações e plataformas suportadas - diagrama](media/pam-test-lab-architecture.png)

Precisa de um mínimo de três máquinas virtuais.  Se ainda não tem um domínio ad para a PAM gerir, precisa de um VM adicional para funcionar como controlador de domínio CORP.  Se desejar configurar o software PRIV para uma alta disponibilidade, necessita de mais dois VMs adicionais.

As unidades onde as imagens do disco VM serão armazenadas precisam de pelo menos 120 GB de espaço em disco livre.  Se planeia implementar para elevada disponibilidade, certifique-se de que o subsistema de disco cumpre os requisitos do armazenamento partilhado SQL.  O armazenamento partilhado pode estar sob a forma de discos de cluster de Clustering de Ativação Pós-falha do Windows Server, de discos numa Rede de Área de Armazenamento (SAN) ou partilhas de ficheiros num servidor do SMB.

> [!IMPORTANT]
> O armazenamento deve ser dedicado ao ambiente do bastião. Partilhar o armazenamento com outras cargas de trabalho fora do ambiente do bastião não é recomendado, pois poderia comprometer a integridade do ambiente do bastião.

## <a name="next-steps"></a>Passos seguintes

- [A Gestão de Acesso Privilegiado para Serviços](privileged-identity-management-for-active-directory-domain-services.md) de Domínio de Diretório Ativo é uma visão geral da PAM e como funciona.
- [Compreender os componentes da PAM](principles-of-operation.md) é uma visão geral dos vários componentes da PAM.
