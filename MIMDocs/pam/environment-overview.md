---
title: "Descrição geral do ambiente do PAM | Documentos da Microsoft"
description: "Localizar o número e a configuração necessários de máquinas virtuais a implementar Privileged Access Management com êxito"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/31/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3be2e19673a863098739e830d9c83ce264abf412
ms.sourcegitcommit: 210195369d2ecd610569d57d0f519d683ea6a13b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/01/2017
---
# <a name="environment-overview"></a>Descrição geral do ambiente

Privileged Access Management trabalha com as máquinas virtuais (VMs) com unidades separadas que estão ligadas entre si numa rede partilhada. Estas máquinas virtuais podem ser alojadas pelo Windows 8.1, pelo Windows Server 2012 R2 ou por outras plataformas de sistema operativo.

![Servidores de PAM: relações e plataformas suportadas - diagrama](media/pam-test-lab-architecture.png)

É necessário um mínimo de três máquinas virtuais.  Se ainda não tiver um domínio do AD para o PAM gerir, precisa de uma VM adicional para agir como um controlador de domínio CORP.  Se pretender configurar o software PRIV para elevada disponibilidade, tem duas VMs adicionais.

As unidades onde serão armazenadas as imagens de disco da VM tem de, pelo menos, 120 GB de espaço livre em disco.  Se planeia implementar para elevada disponibilidade, certifique-se de que o subsistema de disco cumpre os requisitos do armazenamento partilhado SQL.  O armazenamento partilhado pode estar sob a forma de discos de cluster de Clustering de Ativação Pós-falha do Windows Server, de discos numa Rede de Área de Armazenamento (SAN) ou partilhas de ficheiros num servidor do SMB.

>[!IMPORTANT]
Armazenamento têm de estar dedicado ao ambiente bastion. Armazenamento de partilha com outras cargas de trabalho fora do ambiente bastion não é recomendado, mesmo que pode comprometer a integridade do ambiente bastion.

## <a name="next-steps"></a>Próximos passos

- [Privileged Access Management para serviços de domínio do Active Directory](privileged-identity-management-for-active-directory-domain-services.md) é uma descrição geral do PAM e como funciona.
- [Compreender os componentes de PAM](principles-of-operation.md) é uma descrição geral dos vários componentes de PAM.