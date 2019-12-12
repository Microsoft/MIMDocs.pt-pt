---
title: 'Passo 8: Verificação de implementação da PAM'
description: A implementação do PAM com scripts inclui scripts de verificação que podem executar um cenário do PAM para validar se a implementação do PAM está a funcionar conforme esperado.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: d6b0327b39a76799b2943565dd0c3e00f55f745f
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "64518122"
---
# <a name="step-8-pam-deployment-verification"></a>Passo 8: Verificação de implementação da PAM

> [!div class="step-by-step"]
> [« Passo 7](sp1-step7-setup-sidhistory-sidfiltering.md)
> [Adenda »](sp1-pam-deployment-addendum.md)

O Pacote de implementação inclui scripts de verificação que podem executar um cenário da PAM para validar se a implementação da PAM está a funcionar conforme esperado.
Para utilizar a Verificação de Implementação, modifique a secção PAMDeploymentConfig.xml denominada <PamValidation/>.

>[!NOTE]
>A validação exige um computador Cliente associado ao Domínio CORP com os componentes do lado do cliente da PAM instalados. Veja a Adenda para obter os scripts sobre como instalar um cliente.

O nome do computador cliente tem de ser atualizado na etiqueta <PAMValidationClient/> do PAMDeploymentConfig.xml. Os restantes dados no nó <PAMValidation/> terão de ser editados apenas se estiverem em conflito com os utilizadores/grupos existentes, dado que esta validação tentará criá-los.
Utilize os seguintes passos para realizar a validação:

Passo 1:

1. Inicie sessão no CORPDC como um Administrador de Domínio do CORP
2. Execute o PowerShell como Administrador
3. cd $env:SYSTEMDRIVE\PAM
4. Import-module .\PAMValidation.psm1
5. Create-PAMValidationonCORPDCConfig

Este passo permitirá criar os grupos e os utilizadores necessários para a validação

Passo 2:

1. Inicie sessão no servidor PAM como MIMAdmin
2. Execute o PowerShell como Administrador
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. move-PAMVAlidationUsersToPAM

Este passo migra os utilizadores e os grupos para o ambiente da PAM

Passo 3:

1. Inicie sessão no cliente CORP como um administrador local
2. Execute o PowerShell como Administrador
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. Enable-PAMUsersCORPClientRemote


Neste passo, ser-lhe-á pedida a credencial de CORPAdmin. Depois de fornecida, adicionará os utilizadores necessários aos grupos “Utilizadores do Ambiente de Trabalho Remoto” e “Utilizadores de Gestão Remota”.
No Cliente CORP, utilize o seguinte comando para abrir o PowerShell como o utilizador do PRIV que está a validar. </br></br>
**Runas/u:<PRIV domain>\PRIV.pamRequestor PowerShell. exe**  </br></br>
Na janela do PowerShell, escreva:

1. cd $env:SYSTEMDRIVE\PAM
2. import-module .\PAMValidation.psm1
3. test-PAMValidationScenarioNoApprovalRequest


  Este procedimento mostra o estado do pedido.
  Inicialmente, o utilizador não terá acesso ao recurso. Depois de o utilizador ser adicionado Just-In-Time à função, é-lhe concedido acesso. Depois de expirar a duração do pedido, o utilizador deixará de ter novamente acesso.
  O script utiliza a predefinição (11 minutos) para o pedido expirar.

> [!div class="step-by-step"]
> [« Passo 7](sp1-step7-setup-sidhistory-sidfiltering.md)
> [Adenda »](sp1-pam-deployment-addendum.md)
