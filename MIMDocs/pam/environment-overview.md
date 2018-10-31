---
title: Descrição geral do ambiente do PAM | Documentos da Microsoft
description: Localizar o número e a configuração necessários de máquinas virtuais a implementar Privileged Access Management com êxito
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6f4b6e224b6b50bf2190688a994f35159d273713
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379503"
---
# <a name="environment-overview"></a>Descrição geral do ambiente

Privileged Access Management trabalha com as máquinas virtuais (VMs) com unidades separadas que estão ligadas entre si numa rede partilhada. Estas máquinas virtuais podem ser alojadas pelo Windows 8.1, pelo Windows Server 2012 R2 ou por outras plataformas de sistema operativo.

![Servidores de PAM: relações e plataformas suportadas - diagrama](media/pam-test-lab-architecture.png)

É necessário um mínimo de três máquinas virtuais.  Se ainda não tiver um domínio do AD para o PAM gerir, terá uma VM adicional para agir como um controlador de domínio CORP.  Se quiser configurar o software PRIV para elevada disponibilidade, terá duas VMs adicionais.

As unidades onde serão armazenadas as imagens de disco da VM tem de, pelo menos, 120 GB de espaço livre em disco.  Se planeia implementar para elevada disponibilidade, certifique-se de que o subsistema de disco cumpre os requisitos do armazenamento partilhado SQL.  O armazenamento partilhado pode estar sob a forma de discos de cluster de Clustering de Ativação Pós-falha do Windows Server, de discos numa Rede de Área de Armazenamento (SAN) ou partilhas de ficheiros num servidor do SMB.

> [!IMPORTANT]
> Armazenamento tem de estar dedicado para o ambiente bastion. Armazenamento de partilha com outras cargas de trabalho fora do ambiente bastion não é recomendado, que pode comprometer a integridade do ambiente bastion.

## <a name="next-steps"></a>Passos Seguintes

- [Gestão de acesso privilegiado para serviços de domínio do Active Directory](privileged-identity-management-for-active-directory-domain-services.md) uma visão geral de PAM e como ela funciona.
- [Compreender os componentes de PAM](principles-of-operation.md) é uma visão geral dos vários componentes do PAM.
