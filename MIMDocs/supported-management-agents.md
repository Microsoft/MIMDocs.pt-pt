---
title: Conectores suportados | Documentos da Microsoft
description: Utilize conectores para gerir a transferência de dados entre mim e as suas fontes de dados conectadas.
keywords: ''
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
author: EugeneSergeev
manager: aashiman
editor: ''
reviewer: markwahl-msft
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/23/2019
ms.author: esergeev
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b189d80053dd357874c57a461553205d3394ab04
ms.sourcegitcommit: 32c7a46b2f8ed3f2f9ebc6f79a4ecb0019fe62e0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2020
ms.locfileid: "77527910"
---
# <a name="connect-to-your-directories"></a>Ligar aos diretórios

Os conectores ligam fontes de dados específicas ligadas ao Microsoft Identity Manager SP1 (MIM). Um conector move dados de uma origem de dados ligada para o MIM. Quando os dados no MIM são modificados, o conector também pode exportar os dados para a origem de dados ligada para que se mantenha sincronizada com o MIM. Geralmente, há, pelo menos, um conector para cada diretório ligado.

No Forefront Identity Manager, os conectores eram conhecidos como agentes de gestão. Esse termo continua a ser utilizado em alguns artigos ou partes do produto, mas ambos os termos são alusivos ao mesmo conceito.

Este artigo abrange os conectores que estão incluídos e suportados em MIM, mas o conector para conectividade extensível 2.0 permite a ligação com ainda mais fontes de dados. Alguns parceiros criaram assim os seus próprios conectores e uma lista completa está disponível na wiki [FIM 2010: agentes de gestão de parceiros](https://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx).

## <a name="supported-connectors-in-mim-2016-sp1"></a>Conectores suportados em MIM 2016 SP1

| Nome | Versões suportadas da fonte de dados conectadas e ligações técnicas |
| ---- | ----------------------------------------------- |
| Serviços de Domínio do Active Directory | Diretório Ativo no Windows Server 2012 - 2019 |
| Serviços LDS do Active Directory (ADLDS) | Serviços LDS do Active Directory (ADLDS) |
| Lista de Endereços Global (GAL) do Active Directory | Lista de endereços globais do Diretório Ativo (GAL) – Bolsa 2013 - 2019 |
| Extensible Connectivity 2.0 | Qualquer origem de dados baseada em chamadas ou baseada em ficheiros |
| Serviço FIM | Serviço MIM. Note que o Serviço de Sincronização MIM e o Serviço MIM devem ser a mesma versão. |
| Base de Dados Universal IBM DB2 | IBM DB2 versão 9.5 ou 9.7; IBM DB2 OLEDB v9.5 FP5 ou v9.7 FP1 |
| Servidor de Diretório IBM | Servidor de Diretório IBM Tivoli 6. x |
| Novell eDirectory | Novell eDirectory versão 8.7.3, 8.8.5 e 8.8.6 |
| Base de Dados Oracle | Base de Dados Oracle 10 g ou 11g; cliente de 64 bits |
| Microsoft SQL Server | SQL Server 2012 - 2017 |
| Servidor de Diretório Oracle (anteriormente Sun e Netscape) | Servidor de Diretório Sun 6.x, 7.x e Oracle 11 |
| [Conector PowerShell do Windows PowerShell](https://msdn.microsoft.com/library/dn640417.aspx) | Windows PowerShell 2.0 ou superior |
| [Conector de diretório ativo microsoft Azure](https://msdn.microsoft.com/library/dn511001.aspx) | Microsoft Azure Ative Directory (não recomendado para novas implementações) |
| [Conector Genérico LDAP](https://msdn.microsoft.com/library/dn510997.aspx) | [Servidor LDAP v3 (rFC 4510 conforme)](reference/microsoft-identity-manager-2016-connector-genericldap.md#overview-of-the-generic-ldap-connector) |
| [Conector SQL Genérico](reference/microsoft-identity-manager-2016-connector-genericsql.md) | [O Connector é suportado com todos os condutores de ODBC de 64 bits](reference/microsoft-identity-manager-2016-connector-genericsql.md#overview-of-the-generic-sql-connector) |
| [Conector para Lotus Domino](https://msdn.microsoft.com/library/hh859750.aspx) | Lotus Notes Lançamento v8.5.x, v9.0.x |
| [Conector de serviços SharePoint UPA](https://msdn.microsoft.com/library/dn511003.aspx) | SharePoint servidor 2013 - 2019 com aplicação de serviço de perfil de utilizador (UPA) |
| [Conector para Serviços Web](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5.0 ou 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 e outras APIs soap e REST](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [Ficheiro de texto de par de atributo-valor](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | Ficheiros de texto de par atributo-valor |
| [Ficheiro de texto delimitado](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | Ficheiros de texto delimitado |
| [Linguagem de marcação de serviços de diretório (DSML)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | Directory Services Markup Language (DSML) 2.0 |
| [Arquivo de texto de largura fixa](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | Ficheiros de texto de largura fixa |
| [Formato de intercâmbio de dados LDAP (LDIF)](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | LDAP Data Interchange Format (LDIF) |
| [Conector de gráficos da Microsoft](microsoft-identity-manager-2016-connector-graph.md) | Microsoft Graph |

## <a name="related-topics"></a>Tópicos relacionados

[Agentes de gestão no FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
