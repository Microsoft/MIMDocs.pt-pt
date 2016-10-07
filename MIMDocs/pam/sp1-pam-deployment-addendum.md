---
title: Adenda
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
ms.openlocfilehash: 482cfbbac3ea668ca4bf9d8a4a45469e61634f98


---
# Adenda:

## Adenda 1: Configurar o domínio PRIV

Após descomprimir o ficheiro comprimido para a pasta $env:SYSTEMDRIVE\PAM, edite o PAMDeploymentConfig.xml para fornecer detalhes da floresta PRIV. Atualize o DNSName, o NetbiosName, o nome de DC, o Caminho da Base de Dados/do Registo e o Caminho do Sysvol. Atualize também o Domínio e o ForestMode. Se estiver a testar o Windows Server Technical Preview 5, defina o DomainMode e o ForestMode como WinThreshold.

1. Inicie sessão no DC do domínio PRIV como Administrador
2. Execute o PowerShell como Administrador
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Selecione a Opção 9 do Menu (configuração da Floresta Priv)


O DC reiniciará automaticamente após a conclusão. A palavra-passe de administrador do Modo de Restauro de Serviços de Diretório (DSRM) tem de cumprir os seguintes critérios:

  * O comprimento da palavra-passe tem um mínimo de 15 carateres
  * A palavra-passe contém, pelo menos, um caráter em minúsculas
  * A palavra-passe contém, pelo menos, um caráter em MAIÚSCULAS
  * A palavra-passe contém, pelo menos, um digito ou caráter especial

## Adenda 2: Configurar o domínio CORP

Se estiver a começar com o PAM e pretender configurar um ambiente de teste, o script também permitirá a configuração de um Domínio CORP. Após descomprimir o ficheiro comprimido para a pasta $env:SYSTEMDRIVE\PAM, edite o PAMDeploymentConfig.xml ao adicionar os detalhes da floresta CORP. Atualize o DNSName, o NetbiosName, o DC, o nome, o Caminho da Base de Dados/do Registo e o Caminho do Sysvol. O nível funcional deve ser, pelo menos, o Windows Server 2012 R2.

1. Inicie sessão no DC do domínio CORP como Administrador
2. Execute o PowerShell como Administrador
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Selecione a Opção 10 do Menu (configuração da floresta CORP)

O controlador de domínio será reiniciado automaticamente após a conclusão

## Adenda 3: Configurar um cliente CORP para fazer a validação

A ClientBinaryLocation no ficheiro de configuração tem de apontar para a localização onde se encontra o setup.exe.
Inicie sessão como um administrador local e execute os seguintes comandos numa janela do PowerShell elevada:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Selecione a Opção 7 do Menu (Configuração do Cliente de PAM do MIM)


Se a máquina não estiver associada a um domínio, solicitará as credenciais do administrador CORP para executar uma associação a um domínio. O computador tem de ser reiniciado após a associação a um domínio. Inicie sessão no cliente novamente como um administrador local e execute os seguintes comandos numa janela do PowerShell elevada:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Selecione a Opção 7 do Menu (Configuração do Cliente de PAM do MIM)

Prossiga com o Passo 8 indicado acima.

## Adenda 4: Se ocorrer um erro

Todos os registos de script são guardados em %AppData%\MIMPAMInstall. Comprima a pasta para um ficheiro Zip e envie por e-mail para [mim2016@microsoft.com](mim2016@microsoft.com), juntamente com os detalhes da operação e do erro.



<!--HONumber=Sep16_HO4-->

