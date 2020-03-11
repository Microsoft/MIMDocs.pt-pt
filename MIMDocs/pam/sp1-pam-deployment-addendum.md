---
title: Adenda
description: Esta é a Adenda aos documentos que incluem a implementação com scripts do PAM. Inclui a configuração dos domínios PRIV e CORP, bem como a configuração de um cliente para fazer a validação e as informações sobre como pedir assistência.
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
ms.openlocfilehash: 34a4fbc2ada0c54cabb128af5ca90e2e89e06517
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043873"
---
# <a name="pam-deployment-scripts-addendum"></a>Adenda de scripts de implementação da PAM:

## <a name="addendum-1-setting-up-the-priv-domain"></a>Adenda 1: Configurar o domínio PRIV

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

## <a name="addendum-2-setting-up-the-corp-domain"></a>Adenda 2: Configurar o domínio CORP

Se estiver a começar com a PAM e pretender configurar um ambiente de teste, o script também permitirá a configuração de um Domínio CORP. Após descomprimir o ficheiro comprimido para a pasta $env:SYSTEMDRIVE\PAM, edite o PAMDeploymentConfig.xml ao adicionar os detalhes da floresta CORP. Atualize o DNSName, o NetbiosName, o DC, o nome, o Caminho da Base de Dados/do Registo e o Caminho do Sysvol. O nível funcional deve ser, pelo menos, o Windows Server 2012 R2.

1. Inicie sessão no DC do domínio CORP como Administrador
2. Execute o PowerShell como Administrador
3. cd $env:SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. Selecione a Opção 10 do Menu (configuração da floresta CORP)

O controlador de domínio será reiniciado automaticamente após a conclusão

## <a name="addendum-3-setting-up-a-corp-client-to-do-the-validation"></a>Adenda 3: Configurar um cliente CORP para fazer a validação

A ClientBinaryLocation no ficheiro de configuração tem de apontar para a localização onde se encontra o setup.exe.
Inicie sessão como um administrador local e execute os seguintes comandos numa janela do PowerShell elevada:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Selecione a Opção 7 do Menu (Configuração do Cliente da PAM do MIM)


Se a máquina não estiver associada a um domínio, solicitará as credenciais do administrador CORP para executar uma associação a um domínio. O computador tem de ser reiniciado após a associação a um domínio. Inicie sessão no cliente novamente como um administrador local e execute os seguintes comandos numa janela do PowerShell elevada:

1. cd $env:SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. Selecione a Opção 7 do Menu (Configuração do Cliente da PAM do MIM)

Prossiga com o Passo 8 indicado acima.

## <a name="addendum-4-if-something-goes-wrong"></a>Adenda 4: Se ocorrer um erro

Todos os registos de script são guardados em %AppData%\MIMPAMInstall. Comprima a pasta para um ficheiro Zip e envie por e-mail para [mim2016@microsoft.com](mailto:mim2016@microsoft.com), juntamente com os detalhes da operação e do erro.
