---
title: Conectores suportados | Documentos da Microsoft
description: "Utilizar conectores para gerir a transferência de dados entre MIM e os diretórios."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: 7f16c3a054f0a2c59f118ba33bf64fca10034690
ms.openlocfilehash: b26fe7bc56ab8229054afb1409c3652e81464a3d
ms.contentlocale: pt-pt
ms.lasthandoff: 07/10/2017


---

# Ligar aos diretórios
<a id="connect-to-your-directories" class="xliff"></a>

Os conectores ligam origens de dados específicas ao Microsoft Identity Manager (MIM). Um conector move dados de uma origem de dados ligada para o MIM. Quando os dados no MIM são modificados, o conector também pode exportar os dados para a origem de dados ligada para que se mantenha sincronizada com o MIM. Geralmente, há, pelo menos, um conector para cada diretório ligado.

No Forefront Identity Manager, os conectores eram conhecidos como agentes de gestão. Esse termo continua a ser utilizado em alguns artigos ou partes do produto, mas ambos os termos são alusivos ao mesmo conceito.

Este artigo abrange os conectores que estão incluídos no MIM, mas o conetor para o Extensible Connectivity 2.0 possibilita a ligação a um número ainda maior de origens de dados. Alguns parceiros criaram assim os seus próprios conectores e uma lista completa está disponível na wiki [FIM 2010: agentes de gestão de parceiros](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx).

## Conectores suportados no MIM 2016
<a id="supported-connectors-in-mim-2016" class="xliff"></a>

| Nome | Versões suportadas da origem de dados ligada |
| ---- | ----------------------------------------------- |
| Serviços de Domínio do Active Directory | Active Directory 2000, 2003, 2003 R2, 2008, 2008 R2, 2012 |
| Serviços LDS do Active Directory (ADLDS) | Serviços LDS do Active Directory (ADLDS) |
| Lista de Endereços Global (GAL) do Active Directory | Lista de Endereços Global do Active Directory (GAL) – Exchange 2000, 2003, 2007, 2010, 2013 |
| Extensible Connectivity 2.0 | Qualquer origem de dados baseada em chamadas ou baseada em ficheiros |
| Serviço MIM | Documentos da Microsoft 2016 |
| Base de Dados Universal IBM DB2 | IBM DB2 versão 9.1, 9.5 ou 9.7; IBM DB2 OLEDB v9.5 FP5 ou v9.7 FP1 |
| Servidor de Diretório IBM | Servidor de Diretório IBM Tivoli 6. x |
| Novell eDirectory | Novell eDirectory versão 8.7.3, 8.8.5 e 8.8.6 |
| Base de Dados Oracle | Base de Dados Oracle 10 g ou 11g; cliente de 64 bits |
| Microsoft SQL Server | SQL Server 2000, 2005, 2008, 2008 R2, 2012 |
| Servidor de Diretório Oracle (anteriormente Sun e Netscape) | Servidor de Diretório Sun 6.x, 7.x e Oracle 11 |
| [Conector Windows PowerShell para FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn640417.aspx) | Windows PowerShell 2.0 ou superior |
| [Conector Microsoft Azure Active Directory para FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511001.aspx) | Microsoft Azure Active Directory |
| [Conector LDAP Genérico para FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn510997.aspx) | Servidor LDAP v3 (compatível com RFC 4510) |
| [Conector para Lotus Domino](https://msdn.microsoft.com/en-us/library/hh859750.aspx) | Lotus Notes Versão v8.0.x ou v8.5.x |
| [Conector SharePoint Service para FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511003.aspx) | Servidor SharePoint 2013 ou 2016 com a aplicação de serviço de Perfil de Utilizador (UPA) |
| [Conector para Serviços Web](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | SAP ECC 5.0 ou 6.0; Oracle PeopleSoft 9.1; Oracle eBusiness 12.1 |
| Ficheiro de texto de Par Atributo-Valor | Ficheiros de texto de par atributo-valor |
| Ficheiro de texto delimitado | Ficheiros de texto delimitado |
| Serviços de diretório marcar o idioma (DSML) | Directory Services Markup Language (DSML) 2.0 |
| Ficheiro de texto de largura fixa | Ficheiros de texto de largura fixa |
| LDAP Data Interchange Format (LDIF) | LDAP Data Interchange Format (LDIF) |

## Tópicos relacionados
<a id="related-topics" class="xliff"></a>

[Agentes de gestão no FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx)

