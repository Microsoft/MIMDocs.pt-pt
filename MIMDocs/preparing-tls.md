---
title: Planeamento microsoft Identity Manager 2016 em ambiente TLS 1.2 Microsoft Docs
description: Planeamento microsoft Identity Manager 2016 em ambiente TLS 1.2
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/26/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4e208e2f42c206ae50febefb6a8206dc3823e084
ms.sourcegitcommit: c9f5f960fd39745bf5b57161a2fd0238c88d035a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2020
ms.locfileid: "85133537"
---
# <a name="planning-mim-2016-sp2-in-tls-12-or-fips-mode-environments"></a>Planeamento MIM 2016 SP2 em ambientes TLS 1.2 ou modo FIPS


> [!IMPORTANT]
> Este artigo aplica-se apenas ao MIM 2016 SP2

Ao instalar o MIM 2016 SP2 no ambiente bloqueado que tenha todos os protocolos de encriptação, mas tLS 1.2 desativado, aplicam-se os seguintes requisitos:
- Para estabelecer componentes DE LIGAÇÃO TLS 1.2 seguros, as atualizações mais recentes para o Windows Server e o Quadro .NET permitem a instalação de suporte TLS 1.2 em .NET 3.5 Framework. Dependendo da configuração do servidor, *poderá* necessitar de [ativar as inversões SystemDefaultTls para .NET Framework](https://support.microsoft.com/help/3154520/support-for-tls-system-default-versions-included-in-the-net-framework) e/ou [desativar todos os protocolos SCHANNEL, exceto tls 1.2](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) no registo nas sub-tefes *cliente* e *servidor.*

## <a name="mim-synchronization-service-sql-ma"></a>Serviço de Sincronização MIM, SQL MA

- Para estabelecer uma ligação segura TLS 1.2 com o servidor SQL, o Serviço de Sincronização MIM e o agente de gestão SQL incorporado requerem [o SQL Native Client 11.0.7001.0](https://www.microsoft.com/download/details.aspx?id=50402) ou mais tarde.

## <a name="mim-service"></a>Serviço MIM
   >[!NOTE]
   >A instalação SEM vigilância DA MIM 2016 SP2 falha no ambiente TLS 1.2. Instale o Serviço MIM em modo interativo ou, se instalar sem vigilância, certifique-se de que o TLS 1.1 está ativado. Após a conclusão da instalação sem vigilância, aplique O TLS 1.2, se necessário.

- Os certificados auto-assinados não podem ser utilizados pelo Serviço MIM apenas em ambiente TLS 1.2. Escolha um certificado compatível com encriptação forte emitido pela Autoridade de Certificação Fidedigna ao instalar o Serviço MIM.
- O instalador de serviço MIM requer ainda [o controlador OLE DB para a versão 18.2](https://www.microsoft.com/download/details.aspx?id=56730) do SQL Server ou posterior.

## <a name="fips-mode-considerations"></a>Considerações no modo FIPS

Se instalar o Serviço MIM num servidor com o modo FIPS ativado, é necessário desativar a validação da política do FIPS para permitir a execução dos fluxos de trabalho do serviço MIM. Para tal, adicione *o elemento de aplicaçãoFIPSPolicy ativado=falso* elemento na secção de tempo de *funcionamento* do *ficheiroMicrosoft.ResourceManagement.Service.exe.config* entre o tempo de *execução* e a montagem As secções *de montagem,* conforme indicado abaixo:

```XML
<runtime>
<enforceFIPSPolicy enabled="false"/>
<assemblyBinding ...>
```    
