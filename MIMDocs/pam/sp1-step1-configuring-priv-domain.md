---
title: Passo 1 Configuração do domínio PRIV
description: Prepare o domínio PRIV com identidades existentes ou novas para ser gerido pelo Microsoft Identity Manager usando scripts
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: a4d7c8ed4f5d7a9d6b4653caf03de69f1d18e509
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010630"
---
# <a name="step-1-configuring-the-priv-domain"></a>Passo 1 Configuração do domínio PRIV

> [!div class="step-by-step"]
> [Passo 2 »](sp1-step2-configuring-corp-domain.md)

1. Inicie sessão no PRIVDC como Administrador
   * Se esta implantação de PAM é um ambiente PRIV-Only, inicie sessão no CORPDC
2. Execute o PowerShell como Administrador
3. cd $env:SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. Selecione a Opção 1 do Menu (Configuração da Floresta PRIV)


As Contas de Serviço necessárias para a gestão do SQL, SharePoint e MIM são criadas automaticamente, se ainda não estiverem presentes no domínio. Ser-lhe-á pedido que introduza as palavras-passe para a criação destas contas de serviço durante a execução do script.

Se o domínio PRIV for o Windows Server 2016, com o Nível Funcional definido para o Windows Server 2016, o script irá solicitar para permitir a funcionalidade de gestão de acesso privilegiado opcional do Diretório Ativo exigida pela PAM. Escolha “Sim” para continuar. Para níveis funcionais abaixo do Windows Server 2016, ignore o aviso que indica que a configuração adicional não será realizada. Terá de refazer a configuração PAMDeployment.ps1 e a PAM Forest, uma vez que o administrador eleva o nível funcional para o Windows Server 2016.

>[!NOTE]
>Os seguintes passos não são necessários para configurações PRIVADAOnly

7. Copie o SIDs.txt criado em $env:SYSTEMDRIVE\PAM para a pasta semelhante no CORPDC.
   * Você precisará desta lista de SIDs no CORPDC, para configurar permissões em um passo subsequente para que os utilizadores PRIVADO possam ler as propriedades do utilizador DA CORP.
8. Após a conclusão do script, ser-lhe-á pedido que reinicie o computador para que as alterações tenham efeito.

> [!div class="step-by-step"]
> [Passo 2 »](sp1-step2-configuring-corp-domain.md)
