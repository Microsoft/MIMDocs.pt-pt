---
title: Planejando o Microsoft Identity Manager 2016 no ambiente TLS 1,2 | Microsoft Docs
description: Planejando o Microsoft Identity Manager 2016 no ambiente TLS 1,2
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/26/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b574a9c8536a460cdddf57131a821775672c082a
ms.sourcegitcommit: b09a8c93983d9d92ca4871054650b994e9996ecf
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2019
ms.locfileid: "73383955"
---
# <a name="planning-mim-2016-sp2-in-tls-12-or-fips-mode-environments"></a>Planejando o MIM 2016 SP2 em ambientes de modo TLS 1,2 ou FIPS


> [!IMPORTANT]
> Este artigo se aplica somente ao MIM 2016 SP2

Ao instalar o MIM 2016 SP2 no ambiente bloqueado que tem todos os protocolos de criptografia, mas TLS 1,2 desabilitado, os seguintes requisitos se aplicam:
- Para estabelecer uma conexão TLS 1,2, os componentes do MIM exigem as atualizações mais recentes do Windows Server e .NET Framework que habilitam o suporte a TLS 1,2 no .NET 3,5 Framework a ser instalado. Dependendo da configuração do servidor, *talvez* seja necessário [habilitar o SystemDefaultTlsVersions para .NET Framework](https://support.microsoft.com/help/3154520/support-for-tls-system-default-versions-included-in-the-net-framework) e/ou [desabilitar todos os protocolos SCHANNEL, exceto o TLS 1,2](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) no registro nas subchaves do *cliente* e do *servidor* .

## <a name="mim-synchronization-service-sql-ma"></a>Serviço de sincronização do MIM, SQL MA

- Para estabelecer uma conexão TLS 1,2 segura com o SQL Server, o serviço de sincronização do MIM e o agente de gerenciamento do SQL interno exigem [SQL Native Client 11.0.7001.0](https://www.microsoft.com/download/details.aspx?id=50402) ou posterior.

## <a name="mim-service"></a>Serviço MIM
- Os certificados autoassinados não podem ser usados pelo serviço do MIM no ambiente TLS 1,2 somente. Escolha um certificado compatível com criptografia forte emitido pela autoridade de certificação confiável ao instalar o serviço do MIM.
- O instalador do serviço do MIM adicionalmente requer [OLE DB driver para SQL Server versão 18,2](https://www.microsoft.com/download/details.aspx?id=56730) ou posterior.

## <a name="fips-mode-considerations"></a>Considerações sobre o modo FIPS

Se você instalar o serviço do MIM em um servidor com o modo FIPS habilitado, será necessário desabilitar a validação da política FIPS para permitir que os fluxos de trabalho do serviço do MIM sejam executados. Para fazer isso, adicione o elemento *enforceFIPSPolicy Enabled = False* à seção *Runtime* do arquivo *Microsoft. ResourceManagement. Service. exe. config* entre as seções *Runtime* e *assemblyBinding* , conforme descrito abaixo:

```XML
<runtime>
<enforceFIPSPolicy enabled="false"/>
<assemblyBinding ...>
```    