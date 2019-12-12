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
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "64518548"
---
# <a name="environment-overview"></a>Descrição geral do ambiente

Privileged Access Management trabalha com as máquinas virtuais (VMs) com unidades separadas que estão ligadas entre si numa rede partilhada. Estas máquinas virtuais podem ser alojadas pelo Windows 8.1, pelo Windows Server 2012 R2 ou por outras plataformas de sistema operativo.

![Servidores de PAM: relações e plataformas suportadas - diagrama](media/pam-test-lab-architecture.png)

Você precisa de um mínimo de três máquinas virtuais.  Se você ainda não tiver um domínio do AD para o PAM gerenciar, precisará de uma VM adicional para atuar como um controlador de domínio CORP.  Se você quiser configurar o software PRIV para alta disponibilidade, precisará de duas VMs adicionais.

As unidades em que as imagens de disco da VM serão armazenadas precisam de pelo menos 120 GB de espaço livre em disco.  Se planeia implementar para elevada disponibilidade, certifique-se de que o subsistema de disco cumpre os requisitos do armazenamento partilhado SQL.  O armazenamento partilhado pode estar sob a forma de discos de cluster de Clustering de Ativação Pós-falha do Windows Server, de discos numa Rede de Área de Armazenamento (SAN) ou partilhas de ficheiros num servidor do SMB.

> [!IMPORTANT]
> O armazenamento deve ser dedicado ao ambiente de bastiões. O compartilhamento de armazenamento com outras cargas de trabalho fora do ambiente de bastiões não é recomendado, pois pode prejudicar a integridade do ambiente de bastiões.

## <a name="next-steps"></a>Próximos passos

- [Privileged Access Management para Active Directory Domain Services](privileged-identity-management-for-active-directory-domain-services.md) é uma visão geral do Pam e como ele funciona.
- [Entender os componentes do Pam](principles-of-operation.md) é uma visão geral dos vários componentes do Pam.
