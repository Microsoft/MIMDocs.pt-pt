---
title: "Passo 8: Verificação de implementação da PAM"
description: "Preparar o domínio CORP com identidades novas ou existentes para ser gerido pelo Privileged Identity Manager através de scripts"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: 743ba586374ccc04e9ddafff759a00574e13f6ac


---

# Passo 8: Verificação de implementação da PAM

O Pacote de implementação inclui scripts de verificação que podem executar um cenário de PAM para validar se a implementação da PAM está a funcionar conforme esperado.
Para utilizar a Verificação de Implementação, modifique a secção PAMDeploymentConfig.xml denominada <PamValidation/>.

>[!Note] A validação exige um computador Cliente associado ao Domínio CORP com os componentes do lado do cliente da PAM instalados. Veja a Adenda para obter os scripts sobre como instalar um cliente.

O nome do computador cliente tem de ser atualizado na etiqueta <PAMValidationClient/> do PAMDeploymentConfig.xml. Os restantes dados no nó <PAMValidation/> terão de ser editados apenas se estiverem em conflito com os utilizadores/grupos existentes, dado que esta validação tentará criá-los.
Utilize os seguintes passos para realizar a validação:

Passo 1:

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

Este passo migra os utilizadores e os grupos para o ambiente de PAM

Passo 3:

1. Inicie sessão no cliente CORP como um administrador local
2. Execute o PowerShell como Administrador
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. Enable-PAMUsersCORPClientRemote


Neste passo, ser-lhe-á pedida a credencial de CORPAdmin. Depois de fornecida, adicionará os utilizadores necessários aos grupos “Utilizadores do Ambiente de Trabalho Remoto” e “Utilizadores de Gestão Remota”.
No Cliente CORP, utilize o seguinte comando para abrir o PowerShell como o utilizador do PRIV que está a validar. </br></br>
**Runas /u:<PRIV domain>\PRIV.pamRequestor powershell.exe**  </br></br>
Na janela do PowerShell, escreva:

1. cd $env:SYSTEMDRIVE\PAM
2. import-module .\PAMValidation.psm1
3. test-PAMValidationScenarioNoApprovalRequest


  Este procedimento mostra o estado do pedido.
  Inicialmente, o utilizador não terá acesso ao recurso. Depois de o utilizador ser adicionado Just-In-Time à função, é-lhe concedido acesso. Depois de expirar a duração do pedido, o utilizador deixará de ter novamente acesso.
  O script utiliza a predefinição (11 minutos) para o pedido expirar.



<!--HONumber=Sep16_HO4-->


