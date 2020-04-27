---
title: Planeamento do Microsoft Identity Manager 2016 em ambiente TLS 1.2 [ Microsoft Docs
description: Planeamento do Microsoft Identity Manager 2016 em ambiente TLS 1.2
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
ms.openlocfilehash: f8d0be0cb9ffa0f32415f11b407954cb0c985024
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043448"
---
# <a name="planning-mim-2016-sp2-in-tls-12-or-fips-mode-environments"></a>Planeamento MIM 2016 SP2 em ambientes tLS 1.2 ou modo FIPS


> [!IMPORTANT]
> Este artigo aplica-se apenas a MIM 2016 SP2

Ao instalar o MIM 2016 SP2 no ambiente bloqueado que tem todos os protocolos de encriptação, mas TLS 1.2 desativado, aplicam-se os seguintes requisitos:
- Para estabelecer componentes mim de ligação TLS 1.2 seguros, os componentes MIM de ligação TLS 1.2 requerem as mais recentes atualizações para o Windows Server e .NET Framework que permitem a instalação do suporte TLS 1.2 em .NET 3.5 Framework. Dependendo da configuração do *servidor, poderá* ser necessário ativar o [SystemDefaultTlsVersions para .NET Framework](https://support.microsoft.com/help/3154520/support-for-tls-system-default-versions-included-in-the-net-framework) e/ou [desativar todos os protocolos SCHANNEL, exceto TLS 1.2](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) no registo em subchaves *de Cliente* e *Servidor.*

## <a name="mim-synchronization-service-sql-ma"></a>Serviço de Sincronização MIM, SQL MA

- Para estabelecer uma ligação TLS 1.2 segura com o servidor SQL, o Serviço de Sincronização MIM e o agente de gestão SQL incorporado requerem o [Cliente Nativo SQL 11.0.7001.0](https://www.microsoft.com/download/details.aspx?id=50402) ou mais tarde.

## <a name="mim-service"></a>Serviço MIM
- Os certificados auto-assinados não podem ser utilizados pelo Serviço MIM apenas em ambiente TLS 1.2. Escolha um certificado compatível com encriptação forte emitido pela Autoridade de Certificação fidedigna ao instalar o Serviço MIM.
- O instalador de serviço mim requer ainda o Controlador Db OLE para a [versão 18.2 do Servidor SQL](https://www.microsoft.com/download/details.aspx?id=56730) ou posterior.

## <a name="fips-mode-considerations"></a>Considerações de modo FIPS

Se instalar o Serviço MIM num servidor com o modo FIPS ativado, é necessário desativar a validação da política FIPS para permitir a execução dos fluxos de trabalho de serviço mim. Para tal, adicione o *enforceFIPSPolicy habilitado=elemento falso* na secção de tempo de *execução* da *Microsoft.ResourceManagement.exe.config* entre as secções de *execução* e montagemAs secções *de ligação* descritas abaixo:

```XML
<runtime>
<enforceFIPSPolicy enabled="false"/>
<assemblyBinding ...>
```    
