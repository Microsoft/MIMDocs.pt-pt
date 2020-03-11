---
title: Configurar a PAM através de Scripts
description: Este artigo faz parte da série para a configuração do PAM através de scripts. Inclui a modificação do ficheiro XML que será usado pelos scripts de implementação do PAM.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 07/20/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 102754fc88af32cb9abed40716ba9168a041d58e
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043890"
---
# <a name="configure-pam-using-scripts"></a>Configurar a PAM através de scripts

Se optar por instalar o SQL e o SharePoint em servidores separados, estes deveverão ser configurados com as seguintes instruções. Se o SQL, o SharePoint e os componentes de PAM estiverem instalados no mesmo computador, os passos abaixo têm de ser executados nesse computador.

Os passos abaixo assumem que o domínio PRIV já está configurado. Para obter instruções para configurar um domínio PRIV, veja a adenda no final do documento.

Passos:

1. Transferir [Scripts de implementação da PAM](https://www.microsoft.com/download/details.aspx?id=53941)
2. Descomprima o ficheiro comprimido “PAMDeploymentScripts.zip” para a pasta %SYSTEMDRIVE%\PAM em todos os computadores.
3. Em qualquer um dos computadores, abra o ficheiro **PAMDeploymentConfig.xml** e atualize os detalhes com o gráfico abaixo ou as instruções dentro do próprio ficheiro XML. Se as florestas CORP e PRIV já estiverem configuradas, só precisará atualizar o **DNSName** e o **NetbiosName**.
4. Na secção Funções, atualize a **conta de serviço**, os **detalhes do computador** e a **localização dos binários de instalação** das funções SQL, SharePoint e MIM.
    1. A localização do binário MIM tem de apontar para o diretório que contém a pasta “Serviço e Portal”. A localização do binário de Cliente tem de apontar para o diretório que contém a pasta “Add-ins and Extensions.msi”.

5. Se se tratar de um ambiente PRIVOnly, a etiqueta PRIVOnly tem de ser definida como Verdadeiro.
    1. Para ambientes PRIVOnly, atualize o **DNSName** e o **NetbiosName** do Domínio PRIV para corresponder ao domínio CORP. Verifique se os sufixos dos computadores onde serão instalados o SQL, o SharePoint e o MIM estão corretos, dado que o ficheiro de modelo predefinido assume uma configuração CORP e PRIV.
    2. Clique aqui para obter mais detalhes sobre os ambientes PRIVOnly.

6. Copie o mesmo PAMDeploymentConfig.xml para a pasta %SYSTEMDRIVE%\PAM em todos os computadores, CORPDC, PRIVDC, Servidor PAM, SQL Server e servidores do SharePoint.


## <a name="deployment-worksheet"></a>Folha de cálculo de implementação

Antes de proceder à atualização do PAMDeploymentConfig.xml e coloque a cópia atualizada em todas as máquinas.

### <a name="setup"></a>Setup

|Machine   | Quem executar como   |Comandos   |
|---|---|---|
|  PRIVDC |Administrador de Domínio PRIV   | .\PAMDeployment.ps1 Selecione a opção 1 do menu (Configuração da Floresta PRIV)   |
|   |   |  O passo acima gera um SIDs.txt. Este ficheiro tem de ser copiado para $envDrive:PAM do CORPDC antes de executar o passo seguinte. |
| CORPDC  |Administrador de Domínio CORP   | .\PAMDeployment.ps1 Selecione a opção 2 do menu (Configuração da Floresta CORP)   |
| PAMServer (ou SQL Server)   |Administrador de Domínio CORP   |  .\PAMDeployment.ps1 Selecione a opção 2 do menu (Configuração da Floresta CORP)  |
|  PAMServer |  Administrador Local (Administrador MIM após a associação a um domínio) |  .\PAMDeployment.ps1 Selecione a opção 4 do menu (Configuração do SharePoint)  |
| PAMServer  | Administrador Local (Administrador MIM após a associação a um domínio)  | .\PAMDeployment.ps1 Selecione a opção 5 do menu (Configuração da PAM do MIM)   |
|  PAMServer |MIMAdmin   | .\PAMDeployment.ps1 Selecione a opção 6 do menu (Configuração da Confiança de PAM) .\PAMDeployment.ps1 Selecione a opção 6 do menu (Configuração da Confiança de PAM) |

### <a name="validation"></a>Validação

|  Machine | Quem executar como   | Comandos   |
|---|---|---|
| CORPClient  | Utilizador CORP (administrador local)  |   .\PAMDeployment.ps1 Selecione a opção 7 do menu (Configuração do Cliente de PAM do MIM)  |
| CORPDC  | Administrador de Domínio CORP   | Import-module .\PAMValidation.psm1 ; Create-PAMValidationCORPDCConfig   |
| PAMServer   | MIMAdmin  | Import-module .\PAMValidation.psm1 ; Move-PAMValidationUsersToPAM  |
| CORPClient  | Utilizador CORP (administrador local)   |   Import-module .\PAMValidation.psm1 ; Enable-PAMUsersCORPClientRemote |
|  CORPClient | <PRIV>Utilizador \PRIV.pamRequestor e no caso de PRIVOnly: <CORP>\pamrequestor   | Import-module .\PAMValidation.psm1 ; Test-PAMValidationScenarioNoApprovalRequest  |


> [!div class="step-by-step"]
> [Iniciar »](sp1-step1-configuring-priv-domain.md)
