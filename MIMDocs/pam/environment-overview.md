---
title: "Descrição geral do ambiente do PAM | Documentos da Microsoft"
description: "Localizar o número e a configuração necessários de máquinas virtuais a implementar Privileged Access Management com êxito"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3e6c5a70c6b9ed140a56135676bbd14a84504317
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="environment-overview"></a>Descrição geral do ambiente

Privileged Access Management trabalha com as máquinas virtuais (VMs) com unidades separadas que estão ligadas entre si numa rede partilhada. Estas máquinas virtuais podem ser alojadas pelo Windows 8.1, pelo Windows Server 2012 R2 ou por outras plataformas de sistema operativo.

![Servidores de PAM: relações e plataformas suportadas - diagrama](media/pam-test-lab-architecture.png)

É necessário um mínimo de três máquinas virtuais.  Se ainda não tiver um domínio do AD para o PAM gerir, necessitará uma VM adicional para agir como um controlador de domínio CORP.  Se pretender configurar o software PRIV para elevada disponibilidade, também necessitará duas VMs adicionais.

As unidades onde as imagens de disco da máquina virtual serão armazenadas necessita de, pelo menos, 120 GB de espaço livre em disco para armazenar todas as VMs.  Se planeia implementar para elevada disponibilidade, certifique-se de que o subsistema de disco cumpre os requisitos do armazenamento partilhado SQL.  O armazenamento partilhado pode estar sob a forma de discos de cluster de Clustering de Ativação Pós-falha do Windows Server, de discos numa Rede de Área de Armazenamento (SAN) ou partilhas de ficheiros num servidor do SMB. Tenha em atenção que estas têm de estar dedicadas ao ambiente bastion; o armazenamento de partilha com outras cargas de trabalho fora do ambiente de bastion não é recomendado, uma vez que pode comprometer a integridade do ambiente bastion.

> [!NOTE]
> A pré-visualização técnica (CTP) do cliente do MIM atual não é compatível com os conteúdos da base de dados ou do diretório do CTP anterior. Se tiverem avaliado anteriormente o MIM para o PAM ou outros cenários, efetue uma cópia de segurança e arquive as máquinas virtuais utilizadas para esse teste, e inicie a implementação com novas imagens de máquina virtual que não o tenham sido utilizadas anteriormente para cenários MIM.
