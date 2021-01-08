---
title: Descrição geral do ambiente do PAM | Documentos da Microsoft
description: Localizar o número e a configuração necessários de máquinas virtuais a implementar Privileged Access Management com êxito
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/05/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: ca306e7521bbdc2200663889becd91b1ae952d5a
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010664"
---
# <a name="mim-pam-test-lab-environment-overview"></a>Mim PAM test lab ambiente

Para configurar um laboratório de teste da MIM PAM, pode instalar o software em máquinas virtuais.
Privileged Access Management trabalha com as máquinas virtuais (VMs) com unidades separadas que estão ligadas entre si numa rede partilhada. Estas máquinas virtuais podem ser alojadas pelo Windows 8.1, pelo Windows Server 2012 R2 ou por outras plataformas de sistema operativo.

![Servidores de PAM: relações e plataformas suportadas - diagrama](media/pam-test-lab-architecture.png)

Precisa de um mínimo de três máquinas virtuais.  Se ainda não tem um domínio AD para a PAM gerir, precisa de um VM adicional para funcionar como controlador de domínio CORP.  Se desejar configurar o software PRIV para uma elevada disponibilidade, precisa de mais dois VMs adicionais.

As unidades onde as imagens do disco VM serão armazenadas precisam de pelo menos 120 GB de espaço em disco livre.  Se planeia implementar para elevada disponibilidade, certifique-se de que o subsistema de disco cumpre os requisitos do armazenamento partilhado SQL.  O armazenamento partilhado pode estar sob a forma de discos de cluster de Clustering de Ativação Pós-falha do Windows Server, de discos numa Rede de Área de Armazenamento (SAN) ou partilhas de ficheiros num servidor do SMB.

> [!IMPORTANT]
> O armazenamento deve ser dedicado ao ambiente de bastião. A partilha de armazenamento com outras cargas de trabalho fora do ambiente de bastião não é recomendada, uma vez que pode comprometer a integridade do ambiente de bastião.

## <a name="next-steps"></a>Próximos passos

- [Gestão privilegiada de acesso para serviços de domínio de diretório ativo](privileged-identity-management-for-active-directory-domain-services.md) é uma visão geral da PAM e como funciona.
- [Compreender os componentes da PAM](principles-of-operation.md) é uma visão geral dos vários componentes da PAM.
