---
title: 'Passo 4: Configurar o SharePoint'
description: "Este é o passo 4 da configuração do PAM através de scripts. Neste passo, vai configurar o SharePoint para o utilizar como parte da implementação do PAM."
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
translationtype: Human Translation
ms.sourcegitcommit: f08b0197341351bd5f33552f26b96132b1356239
ms.openlocfilehash: 12eb9a00584f72b9c628e870562a743fb603d4a3
ms.lasthandoff: 01/10/2017


---

# <a name="step-4-configuring-sharepoint"></a>Passo 4: Configurar o SharePoint

>[!div class="step-by-step"]
[« Passo 3](sp1-step3-installing-configuring-sql.md)
[Passo 5»](sp1-step5-configuring-pam.md)

O SharePoint tem de ser o SharePoint Foundation 2013 com o SP1.

Para domínios associados a servidores, inicie sessão como MIMAdmin

1. Execute o PowerShell como Administrador
2.  .\PAMDeployment.ps1
3.  Selecione a Opção 4 do Menu (Configuração do SharePoint)


Para servidores do grupo de trabalho

1. Execute o PowerShell como Administrador
2.  cd $env:SYSTEMDRIVE\PAM
3.  .\PAMDeployment.ps1
4. Selecione a Opção 4 do Menu (Configuração do SharePoint)

A máquina será reiniciada diversas vezes enquanto instala o SharePoint. De cada vez que é reiniciada, a configuração do SharePoint tem de ser novamente executada e deverá confirmar que inicia sessão com a conta de MIMAdmin.
Se a máquina que instala o SharePoint não tiver ligação à Internet para transferir os pré-requisitos, poderá transferi-los de forma independente e colocá-los numa pasta local. **Este caminho da pasta local tem de ser atualizado no ficheiro PAMConfiguration.xml em <PrerequisitesBinaryLocation/>.** Veja a Adenda 5 para obter as ligações para transferir os ficheiros.
Após a instalação, o GUI de Configuração do SharePoint será apresentado. Siga os passos para concluir a instalação do SharePoint. Selecione o Servidor Completo e percorra o resto da IU. Após a instalação, pedir-lhe-á que execute o Assistente de Configuração. Conclua os passos como indicado abaixo.

1. No separador **Ligar a um farm de servidores**, mude para **criar um novo farm de servidores**.
2. Especifique o **SQLServer** como o servidor da base de dados de configuração e a **ServiceAccount do SharePoint** como a conta de acesso à base de dados para o SharePoint utilizar.
3. Especifique uma palavra-passe como frase de acesso de segurança do farm **(não será utilizada mais tarde)**
4. Aceite as restantes predefinições do assistente de configuração do SharePoint para criar um farm de servidor único

Para obter os detalhes, veja a secção **Configurar SharePoint** no [Passo 3 – Preparar um servidor de PAM](/microsoft-identity-manager/pam/step-3-prepare-pam-server). Quando tiver concluído, execute o script “\PAMDeployment.ps1” novamente. Para tal, selecione a Opção 4 (Configuração do SharePoint) para concluir este passo.

>[!div class="step-by-step"]
[« Passo 3](sp1-step3-installing-configuring-sql.md)
[Passo 5»](sp1-step5-configuring-pam.md)

