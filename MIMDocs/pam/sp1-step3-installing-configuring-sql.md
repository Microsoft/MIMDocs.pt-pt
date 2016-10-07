---
title: 'Passo 3: Configurar o SQL'
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
ms.openlocfilehash: a7d456b1c2baf31ef2d7ca801a567cf42eaef52e


---
# Passo 3: Configurar o SQL

Antes de avançar com os passos abaixo, verifique se está a utilizar o SQL Server 2012 SP1 ou posterior ou o SQL server 2014. Para servidores associados a um domínio, inicie sessão com a conta de MIMAdmin; caso contrário, inicie sessão como administrador local
1. Execute o PowerShell como Administrador
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Selecione a Opção 3 do Menu (Configuração do SQL Server)

  Se o servidor ainda não estiver associado ao domínio PRIV, terá de fornecer as credenciais e associar o servidor ao domínio.
  Após a associação a um domínio, o computador será reiniciado. Após a reinicialização bem sucedida, inicie sessão no servidor com a conta de MIMAdmin.

1. Execute o PowerShell como administrador
2. cd $env:SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. Selecione a Opção 3 do Menu (Configuração do SQL Server)

Quando lhe for pedido, forneça a palavra-passe da conta de serviço de MIMAdmin e deixe a instalação continuar. Depois de concluído, avance para o Passo 4.



<!--HONumber=Sep16_HO4-->


