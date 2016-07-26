---
title: "Implementar o Serviço de Notificação de Alteração de Palavra-passe | Microsoft Identity Manager"
description: "Obtenha os passos para instalar e configurar o Serviço de Notificação de Alteração de Palavra-passe do MIM no controlador de domínio."
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 97edae12-6f86-4f9f-8620-a95a096e482a
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 875ed6b96929822ac166a4a262cc8547a4ea3b2a
ms.openlocfilehash: 85e83b85f047ca2c2648b42ec68b832caae645ee


---

# Implementar o Serviço de Notificação de Alteração de Palavra-passe do MIM num controlador de domínio

## Instalar o Serviço de Notificação de Alteração de Palavra-passe
O Serviço de Notificação de Alteração de Palavra-passe (PCNS) é um serviço instalado nos controladores de domínio que permite a sincronização de palavras-passe pelo MIM para outros sistemas, tais como o servidor de diretório de outro fornecedor. Para a sincronização de palavras-passe, instale o PCNS em cada servidor de controlador de domínio.

1.  Inicie sessão como administrador de domínio num Servidor em execução no Windows Server com a função dos Serviços de Domínio do Active Directory.

2.  Copie a pasta de configuração do Serviço de Notificação de Alteração de Palavra-passe para o computador.

3.  Localize o ficheiro *Serviço de Notificação de Alteração de Palavra-passe.msi*, clique com o botão direito do rato no ficheiro e crie um atalho.

4.  Localize o ficheiro de atalho e clique com o botão direito do rato no ficheiro para ver as **Propriedades**.

5.  No campo Destino, adicione o preâmbulo *msiexec.exe /i* antes do caminho para o ficheiro msi e o sufixo *SCHEMAONLY=TRUE* depois do caminho de msi. Por exemplo, se a pasta de configuração for *C:\PCNS*, o comando a executar terá o seguinte aspeto: (tudo numa só linha).

    ```
    msiexec.exe /i "C:\PCNS\x64\Password Change Notification Service.msi" SCHEMAONLY=TRUE
    ```

6.  Guarde as alterações do ficheiro de atalho.

7.  Execute o ficheiro de atalho para iniciar a instalação do PCNS no modo de extensão do esquema. Quando for apresentado o ecrã seguinte, clique em **Seguinte**.

8.  Será notificado de que a Configuração irá atualizar o esquema do Active Directory para o Serviço de Notificação de Alteração de Palavra-passe. Clique em **OK** para prosseguir com a atualização do esquema.

9. Quando tiver concluído o processo de extensão do Esquema e for apresentado o ecrã seguinte, clique em **Concluir**.

10. Execute o ficheiro *Serviço de Notificação de Alteração de Palavra-passe.msi* novamente – desta vez, diretamente (não é necessária qualquer cadeia de execução).  Quando for apresentado o ecrã seguinte, clique em **Seguinte**.

11. Aceite o contrato de licença e clique em **Seguinte**.

12. Clique para iniciar a instalação.

13. Quando o ecrã de conclusão da instalação com êxito for apresentado, clique em **Concluir**.

14. Reinicie o computador para que as alterações de configuração efetuadas ao Serviço de Notificação de Alteração de Palavra-passe do MIM sejam aplicadas. Pode fazê-lo ao clicar em **Sim** na janela de pop-up apresentada ou pode reiniciar mais tarde.

## Configurar o Serviço de Notificação de Alteração de Palavra-passe
Quando estiver novamente ligado ao servidor DC como um administrador de domínio, aceda a *C:\Programas\Serviço de Notificação de Alteração de Palavra-passe.* Execute o ficheiro *pcnscfg.exe*.



<!--HONumber=Jun16_HO4-->

