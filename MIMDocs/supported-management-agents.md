---
title: Conectores suportados | Documentos da Microsoft
description: Utilize conectores para gerir a transferência de dados entre a MIM e as suas fontes de dados ligadas.
keywords: ''
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
author: EugeneSergeev
manager: daveba
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
ms.openlocfilehash: 157fd8d2a6b4296899f90c661e12ba6e19743d0f
ms.sourcegitcommit: 22fa4dac943a0c6b0815b711bd1996f77a390e7c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2020
ms.locfileid: "92174532"
---
# <a name="connect-to-your-directories"></a>Ligar aos diretórios

Os conectores ligam fontes de dados conectadas específicas ao Microsoft Identity Manager SP1 (MIM). Um conector move dados de uma origem de dados ligada para o MIM. Quando os dados no MIM são modificados, o conector também pode exportar os dados para a origem de dados ligada para que se mantenha sincronizada com o MIM. Geralmente, há, pelo menos, um conector para cada diretório ligado.

No Forefront Identity Manager, os conectores eram conhecidos como agentes de gestão. Esse termo continua a ser utilizado em alguns artigos ou partes do produto, mas ambos os termos são alusivos ao mesmo conceito.

Este artigo abrange os conectores incluídos & suportados no MIM, mas o conector da Conectividade Extensível 2.0 permite ligar-se a mais fontes de dados. Alguns parceiros criaram assim os seus próprios conectores e uma lista completa está disponível na wiki [FIM 2010: agentes de gestão de parceiros](https://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx).

## <a name="supported-connectors-in-mim-2016-sp1"></a>Conectores suportados no MIM 2016 SP1

| Nome do conector | Versões suportadas da fonte de dados ligadas & ligações técnicas |
| ---- | ----------------------------------------------- |
| Active Directory Domain Services | Diretório Ativo no Windows Server 2012 - 2019 |
| Serviços LDS do Active Directory (ADLDS) | Serviços LDS do Active Directory (ADLDS) |
| Lista de Endereços Global (GAL) do Active Directory | Ative Directory Global Address List (GAL) in Exchange 2013 - 2019 |
| Extensible Connectivity 2.0 | Qualquer origem de dados baseada em chamadas ou baseada em ficheiros |
| Serviço FIM | Serviço MIM. Note que o Serviço de Sincronização MIM e o Serviço MIM devem ser a mesma versão. |
| Base de Dados Universal IBM DB2 | Ibm DB2 versão 9.5 ou 9.7; IBM DB2 OLEDB v9.5 FP5 ou v9.7 FP1 <br/> Utilize o conector GENÉRICO SQL para versões posteriores|
| Servidor de Diretório IBM | Servidor de Diretório IBM Tivoli 6. x <br/> Utilize o conector LDAP genérico para versões posteriores|
| Novell eDirectory | Novell eDirectory versão 8.7.3, 8.8.5 e 8.8.6 <br/> Utilize o conector LDAP genérico para versões posteriores|
| Base de dados Oracle | Base de Dados Oracle 10 g ou 11g; cliente de 64 bits <br/> Utilize o conector GENÉRICO SQL para versões posteriores|
| Microsoft SQL Server | SQL Server 2012 - 2017 <br/> Utilize o conector GENÉRICO SQL para versões posteriores ou SQL Azure|
| Servidor de Diretório Oracle (anteriormente Sun e Netscape) | Servidor de Diretório Sun 6.x, 7.x e Oracle 11<br/> Utilize o conector LDAP genérico para versões posteriores |
| [Conector do Windows PowerShell](https://msdn.microsoft.com/library/dn640417.aspx) | Windows PowerShell 2.0 ou superior |
| [Conector de Diretório Ativo Microsoft Azure](https://msdn.microsoft.com/library/dn511001.aspx) | Microsoft Azure Ative Directy (não recomendado para novas implementações) |
| [Conector LDAP genérico](https://msdn.microsoft.com/library/dn510997.aspx) | [Servidor LDAP v3 (compatível com RFC 4510)](reference/microsoft-identity-manager-2016-connector-genericldap.md#overview-of-the-generic-ldap-connector) |
| [Conector do SQL genérico](reference/microsoft-identity-manager-2016-connector-genericsql.md) | [O Conector é suportado com todos os controladores ODBC de 64 bits](reference/microsoft-identity-manager-2016-connector-genericsql.md#overview-of-the-generic-sql-connector) |
| [Conector para Lotus Domino](https://msdn.microsoft.com/library/hh859750.aspx) | Notas de Lótus Lançamento v8.5.x, v9.0.x |
| [Conector de serviços sharePoint UPA](https://msdn.microsoft.com/library/dn511003.aspx) | Servidor SharePoint 2013 - 2019 com aplicação de serviço de perfil de utilizador (UPA) |
| [Conector para Serviços Web](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5.0 ou 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 e outros APIs SOAP e REST](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [Ficheiro de texto de Par Atributo-Valor](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | Ficheiros de texto de par atributo-valor |
| [Ficheiro de texto delimitado](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | Ficheiros de texto delimitado |
| [Serviços de diretório marcar o idioma (DSML)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | Directory Services Markup Language (DSML) 2.0 |
| [Ficheiro de texto de largura fixa](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | Ficheiros de texto de largura fixa |
| [LDAP Data Interchange Format (LDIF)](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | LDAP Data Interchange Format (LDIF) |
| [Conector de gráficos da Microsoft](microsoft-identity-manager-2016-connector-graph.md) | Microsoft Graph |

## <a name="related-topics"></a>Tópicos relacionados

[Agentes de gestão no FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
