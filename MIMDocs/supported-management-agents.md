---
title: Conectores suportados | Documentos da Microsoft
description: Use conectores para gerenciar a transferência de dados entre o MIM e suas fontes de dados conectadas.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 1/24/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: ''
ms.suite: ems
ms.openlocfilehash: 1f7d4150ce7012cd4726126ba50b1ab0f94f474c
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "67690674"
---
# <a name="connect-to-your-directories"></a>Ligar aos diretórios

Conectores vinculam fontes de dados conectadas específicas a Microsoft Identity Manager SP1 (MIM). Um conector move dados de uma origem de dados ligada para o MIM. Quando os dados no MIM são modificados, o conector também pode exportar os dados para a origem de dados ligada para que se mantenha sincronizada com o MIM. Geralmente, há, pelo menos, um conector para cada diretório ligado.

No Forefront Identity Manager, os conectores eram conhecidos como agentes de gestão. Esse termo continua a ser utilizado em alguns artigos ou partes do produto, mas ambos os termos são alusivos ao mesmo conceito.

Este artigo aborda os conectores que estão incluídos & com suporte no MIM, mas o conector para conectividade extensível 2,0 torna possível conectar-se com ainda mais fontes de dados. Alguns parceiros criaram assim os seus próprios conectores e uma lista completa está disponível na wiki [FIM 2010: agentes de gestão de parceiros](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx).

## <a name="supported-connectors-in-mim-2016-sp1"></a>Conectores com suporte no MIM 2016 SP1

| Nome | Versões com suporte da fonte de dados conectada & links técnicos |
| ---- | ----------------------------------------------- |
| Serviços de Domínio do Active Directory | Active Directory no Windows Server 2012 e no Windows Server 2016 |
| Serviços LDS do Active Directory (ADLDS) | Serviços LDS do Active Directory (ADLDS) |
| Lista de Endereços Global (GAL) do Active Directory | GAL (lista de endereços global) Active Directory – Exchange 2013, Exchange 2016 |
| Extensible Connectivity 2.0 | Qualquer origem de dados baseada em chamadas ou baseada em ficheiros |
| Serviço FIM | Serviço do MIM. Observe que o serviço de sincronização do MIM e o serviço do MIM devem ter a mesma versão. |
| Base de Dados Universal IBM DB2 | IBM DB2 versão 9,5 ou 9,7; IBM DB2 OLEDB v 9.5 FP5 ou v 9.7 FP1 |
| Servidor de Diretório IBM | Servidor de Diretório IBM Tivoli 6. x |
| Novell eDirectory | Novell eDirectory versão 8.7.3, 8.8.5 e 8.8.6 |
| Base de Dados Oracle | Base de Dados Oracle 10 g ou 11g; cliente de 64 bits |
| Microsoft SQL Server | SQL Server 2012, 2014, 2016 |
| Servidor de Diretório Oracle (anteriormente Sun e Netscape) | Servidor de Diretório Sun 6.x, 7.x e Oracle 11 |
| [Conector do Windows PowerShell](https://msdn.microsoft.com/library/dn640417.aspx) | Windows PowerShell 2.0 ou superior |
| [Conector de Microsoft Azure Active Directory](https://msdn.microsoft.com/library/dn511001.aspx) | Microsoft Azure Active Directory (não recomendado para novas implantações) |
| [Conector LDAP genérico](https://msdn.microsoft.com/library/dn510997.aspx) | [Servidor LDAP v3 (compatível com RFC 4510)](reference/microsoft-identity-manager-2016-connector-genericldap.md#overview-of-the-generic-ldap-connector) |
| [Conector SQL Genérico](reference/microsoft-identity-manager-2016-connector-genericsql.md) | [Há suporte para o conector com todos os drivers ODBC de 64 bits](reference/microsoft-identity-manager-2016-connector-genericsql.md#overview-of-the-generic-sql-connector) |
| [Conector para Lotus Domino](https://msdn.microsoft.com/library/hh859750.aspx) | Lotus Notes versão v 8.5. x |
| [UPA do conector do SharePoint Services](https://msdn.microsoft.com/library/dn511003.aspx) | Servidor SharePoint 2013 ou 2016 com a aplicação de serviço de Perfil de Utilizador (UPA) |
| [Conector para Serviços Web](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5,0 ou 6,0; Oracle PeopleSoft 9,1; Oracle eBusiness 12,1 e outras APIs SOAP e REST](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [Arquivo de texto de par atributo-valor](https://technet.microsoft.com/library/cc708644(v=ws.10).aspx) | Ficheiros de texto de par atributo-valor |
| [Arquivo de texto delimitado](https://technet.microsoft.com/library/cc720612(v=ws.10).aspx) | Ficheiros de texto delimitado |
| [DSML (linguagem de marcação de serviços de diretório)](https://technet.microsoft.com/library/cc720660(v=ws.10).aspx) | Directory Services Markup Language (DSML) 2.0 |
| [Arquivo de texto de largura fixa](https://technet.microsoft.com/library/cc720633(v=ws.10).aspx) | Ficheiros de texto de largura fixa |
| [Formato de intercâmbio de dados LDAP (LDIF)](https://technet.microsoft.com/library/cc708662(v=ws.10).aspx) | LDAP Data Interchange Format (LDIF) |
| [Conector de Microsoft Graph](microsoft-identity-manager-2016-connector-graph.md) | Microsoft Graph |

## <a name="related-topics"></a>Tópicos relacionados

[Agentes de gestão no FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
