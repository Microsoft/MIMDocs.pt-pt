---
title: Conectores suportados | Documentos da Microsoft
description: "Utilizar conectores para gerir a transferência de dados entre MIM e as origens de dados ligada."
keywords: 
author: fimguy
ms.author: fimguy
manager: bhu
ms.date: 1/24/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 1e100a686f009d1a2290d7965fe36eea819148be
ms.sourcegitcommit: fab9f21eea15d2024f11a59fc9e43db15bd215c7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="connect-to-your-directories"></a>Ligar aos diretórios

Os conectores ligam origens de dados específicas para o Microsoft Identity Manager SP1 (MIM). Um conector move dados de uma origem de dados ligada para o MIM. Quando os dados no MIM são modificados, o conector também pode exportar os dados para a origem de dados ligada para que se mantenha sincronizada com o MIM. Geralmente, há, pelo menos, um conector para cada diretório ligado.

No Forefront Identity Manager, os conectores eram conhecidos como agentes de gestão. Esse termo continua a ser utilizado em alguns artigos ou partes do produto, mas ambos os termos são alusivos ao mesmo conceito.

Este artigo abrange os conectores que estão incluídos & suportados em MIM, mas o conetor para o Extensible Connectivity 2.0 possibilita a estabelecer ligação com o mesmo mais origens de dados. Alguns parceiros criaram assim os seus próprios conectores e uma lista completa está disponível na wiki [FIM 2010: agentes de gestão de parceiros](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx).

## <a name="supported-connectors-in-mim-2016-sp1"></a>Conectores suportados em MIM 2016 SP1

| Nome | Versões suportadas da origem de dados ligada & ligações técnicas |
| ---- | ----------------------------------------------- |
| Serviços de Domínio do Active Directory | Active Directory de 2012, 2016 |
| Serviços LDS do Active Directory (ADLDS) | Serviços LDS do Active Directory (ADLDS) |
| Lista de Endereços Global (GAL) do Active Directory | Do Active Directory lista de endereços Global (GAL) – Exchange 2013, 2016 |
| Extensible Connectivity 2.0 | Qualquer origem de dados baseada em chamadas ou baseada em ficheiros |
| Serviço FIM | Agente de gestão do FIM Service (serviço de sincronização) tem de ser, a mesma versão do "Forefront Identity Manager instalado o serviço" |
| Base de Dados Universal IBM DB2 | IBM DB2 versão 9.5 ou 9.7; IBM DB2 OLEDB v 9.5 FP5 ou v 9.7 FP1 |
| Servidor de Diretório IBM | Servidor de Diretório IBM Tivoli 6. x |
| Novell eDirectory | Novell eDirectory versão 8.7.3, 8.8.5 e 8.8.6 |
| Base de Dados Oracle | Base de Dados Oracle 10 g ou 11g; cliente de 64 bits |
| Microsoft SQL Server | SQL Server 2012, 2014, 2016 |
| Servidor de Diretório Oracle (anteriormente Sun e Netscape) | Servidor de Diretório Sun 6.x, 7.x e Oracle 11 |
| [Conector Windows PowerShell para FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn640417.aspx) | Windows PowerShell 2.0 ou superior |
| [Conector Microsoft Azure Active Directory para FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511001.aspx) | Microsoft Azure Active Directory |
| [Conector LDAP Genérico para FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn510997.aspx) | [Servidor LDAP v3 (com RFC 4510)](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-genericldap) |
| [Conector do SQL Server genérico para FIM 2010 R2 / MIM](https://msdn.microsoft.com/en-us/library/dn510997.aspx) | [O conector é suportado com todos os controladores ODBC de 64 bits](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-genericsql) |
| [Conector para Lotus Domino](https://msdn.microsoft.com/en-us/library/hh859750.aspx) | Lotus Notes v 8.5. x de versão |
| [Conector de serviços do SharePoint UPA](https://msdn.microsoft.com/en-us/library/dn511003.aspx) | Servidor SharePoint 2013 ou 2016 com a aplicação de serviço de Perfil de Utilizador (UPA) |
| [Conector para Serviços Web](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | [SAP ECC 5.0 ou 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1](https://docs.microsoft.com/en-us/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) |
| [Ficheiro de texto de par atributo-valor](https://technet.microsoft.com/en-us/library/cc708644(v=ws.10).aspx) | Ficheiros de texto de par atributo-valor |
| [Ficheiro de texto delimitado](https://technet.microsoft.com/en-us/library/cc720612(v=ws.10).aspx) | Ficheiros de texto delimitado |
| [Serviços de diretório marcar o idioma (DSML)](https://technet.microsoft.com/en-us/library/cc720660(v=ws.10).aspx) | Directory Services Markup Language (DSML) 2.0 |
| [Ficheiro de texto de largura fixa](https://technet.microsoft.com/en-us/library/cc720633(v=ws.10).aspx) | Ficheiros de texto de largura fixa |
| [Formato (LDIF) de intercâmbio de dados de LDAP](https://technet.microsoft.com/en-us/library/cc708662(v=ws.10).aspx) | LDAP Data Interchange Format (LDIF) |

## <a name="related-topics"></a>Tópicos relacionados

[Agentes de gestão no FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)
