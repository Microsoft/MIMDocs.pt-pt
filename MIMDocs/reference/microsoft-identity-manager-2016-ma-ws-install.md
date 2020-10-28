---
title: MIM Instalar a Ferramenta de Cofiguração de Serviço Web ! Microsoft Docs
description: Este artigo cobre os passos para instalar a Ferramenta de Configuração do Serviço Web.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 4b9d2463ea30839c2ea4e2a3427d057c925183e8
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762147"
---
# <a name="install-the-web-service-configuration-tool"></a>Instale a Ferramenta de Configuração do Serviço Web

O Conector de Serviço Web e os projetos predefinidos estão disponíveis no [Microsoft Download Center.](https://www.microsoft.com/en-us/download/details.aspx?id=51495)

O **Web Service Connector MSI** expõe duas funcionalidades:

- Tempo de funcionação do conector do serviço web: Instala o conector central, as dependências do Conector e o Conector embalado.
- Ferramenta de configuração do serviço web: Instala a ferramenta de configuração do serviço web.

![Opções de conector de assistente de instalação](media/microsoft-identity-manager-2016-ma-ws-install/connector-installation-options.png)

A ferramenta de configuração pode ser instalada sem ter o Serviço de Sincronização instalado. Isto permite a configuração num computador separado.

## <a name="default-projects"></a>Projetos padrão

Projetos predefinidos adicionais são enviados com o Conector de Serviços Web. Estes estão disponíveis como ficheiros EXE auto-extrato. Você pode baixar o projeto de connector de serviço web conforme apropriado ao seu requisito.

Após a conclusão da instalação, os diferentes componentes com os seus binários são instalados na localização abaixo da pasta no seu sistema.

| Conteúdos | Localização |
|---|---|
| Tempo de execução do conector do serviço web           | %Ficheiros de programa% \\ Microsoft Forefront Gestão de Identidade \\ 2010 \\ &nbsp; Extensões de serviço de sincronização \\ |
| Projeto de conector de serviço web           | %Ficheiros de programa% \\ Microsoft Forefront Gestão de Identidade \\ 2010 \\ &nbsp; Extensões de serviço de sincronização \\ |
| Conector Embalado                      | %Ficheiros de programa% \\ Microsoft Forefront Gestão de Identidade \\ 2010 \\ Serviço de Sincronização &nbsp; \\ UIShell \\ XMLs \\ Embalados |
| Ferramenta de configuração de serviço web          | %Ficheiros de programa% \\ Microsoft Forefront Gestão de Identidade \\ 2010 \\ Serviço de Sincronização &nbsp; \\ UIShell \\ Web Service &nbsp; &nbsp; Configuração <br/>**Nota:** Este é o local de instalação predefinido. Pode alterar esta localização durante a instalação. |
| Ficheiro do Projeto de Serviço Web                | O utilizador pode selecionar qualquer pasta-alvo para extrair este ficheiro, mas o ficheiro de projeto extraído (. WsConfig) só é visível para o FIM Sync UI quando o ficheiro do projeto é extraído para a pasta **extensões** FIM. O ficheiro de projeto extraído é visível para a ferramenta de Configuração do Serviço Web em qualquer local. |


## <a name="additional-permissions"></a>Permissões adicionais

O ficheiro do projeto pode ser guardado e aberto a partir de qualquer local (com os privilégios de acesso adequados do seu executor); no entanto, apenas os ficheiros de projeto que são `Synchronization Service\Extension` guardados na pasta podem ser selecionados no assistente de conector do Serviço Web acedido através do FIM Sync UI.

O utilizador que executa a Ferramenta de Configuração do Serviço Web requer os seguintes privilégios:

- Leia/Escreva permissões para a pasta de extensão do serviço de sincronização.
- Leia o acesso à chave de registo **HKLM \\ System \\ CurrentControlSet \\ Services \\ FINSSynchronizationService \\ Parâmetros** .
