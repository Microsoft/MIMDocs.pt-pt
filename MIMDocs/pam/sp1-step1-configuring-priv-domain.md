---
title: "Passo 1: Configurar o domínio Priv"
description: "Preparar o domínio CORP com identidades novas ou existentes para ser gerido pelo Privileged Identity Manager através de scripts"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 24e91ed2f51206b03bec505fc0d28d25128d2c94
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="step-1-configuring-the-priv-domain"></a>Passo 1: Configurar o domínio Priv

>[!div class="step-by-step"]
[Passo 2 »](sp1-step2-configuring-corp-domain.md)

1. Inicie sessão no PRIVDC como Administrador
  * Se se tratar de um ambiente apenas PRIV, inicie sessão no CORPDC
2. Execute o PowerShell como Administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecione a Opção 1 do Menu (Configuração da Floresta PRIV)


As Contas de Serviço necessárias para gerir o SQL/SharePoint e o MIM serão criadas automaticamente se ainda não estiverem presentes no domínio. Ser-lhe-á pedido que introduza as palavras-passe para a criação destas contas de serviço durante a execução do script.
Se o domínio PRIV for o Windows Server 2016, com o Nível Funcional definido como Windows Server 2016 Technical Preview 5, o script pedirá para ativar a “Funcionalidade Privileged Access Management” opcional do Active Directory exigida pela PAM. Escolha “Sim” para continuar.
Para níveis funcionais abaixo do Windows Server 2016, ignore o aviso que indica que a configuração adicional não será realizada. Terá de voltar a executar a Configuração da Floresta de PAM e PAMDeployment.ps1, depois de o administrador elevar o nível funcional para o Windows Server 2016.

>[!NOTE]
>Os passos seguintes Não são necessários para configurações PRIVOnly

Copie o SIDs.txt criado em $env:SYSTEMDRIVE\PAM para a pasta semelhante no CORPDC. Este procedimento é exigido pelo CORPDC para configurar permissões para os utilizadores do PRIV poderem ler as propriedades do utilizador do CORP.
Após a conclusão do script, ser-lhe-á pedido que reinicie o computador para que as alterações tenham efeito.

>[!div class="step-by-step"]
[Passo 2 »](sp1-step2-configuring-corp-domain.md)
