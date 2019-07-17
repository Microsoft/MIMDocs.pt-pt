---
title: Plataformas de software suportadas| Documentos da Microsoft
description: Localizar os produtos e versões que são compatíveis com cada um dos componentes do MIM 2016
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: ''
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: d4cb70ae60d23049251caa121a1834eeacb05677
ms.sourcegitcommit: 4c4bc7aa42cd5984c838abdd302490355ddcb4ea
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/16/2019
ms.locfileid: "68238927"
---
# <a name="supported-platforms-for-mim-2016"></a>Plataformas suportadas para o MIM 2016

A tabela seguinte descreve as plataformas suportadas e a versão de cada componente do Microsoft Identity Manager 2016. As versões marcadas com um * só têm suporte no MIM 2016 service pack 1 com o hotfix mais recente.  As versões marcadas com "NR", para não recomendado, têm suporte, mas não são recomendadas se iniciar uma implantação nova da plataforma para MIM.


| **Componente do MIM** | **Plataforma** | **Versão** |
|-------------------|--------------|--------------|
| **Sincronização do MIM** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
| | Active Directory nível funcional para provisionamento de usuário, PCNS e sincronização de GAL | Windows 2000 (NR)<br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 *
| | Base de dados da Sincronização do MIM | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 * |
| | Active Directory para provisionamento de usuário, PCNS e sincronização de GAL (opcional)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Exchange para o aprovisionamento de caixas de correio e Sincronização da GAL (opcional)|Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016 * |
| | Ambiente de desenvolvimento (opcional) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017 * |
| | Sistema ligado adicional (opcional) | Serviços de Domínio do Active Directory<br/>Active Directory<br/>Serviços LDS<br/>SQL Server 2008 ou posterior<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> Outros produtos de terceiros |
| **Serviço e Portal do MIM** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |Cenário do PAM:  Windows Server | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * |
| |Cenário do PAM: Active Directory para a floresta de PAM do ambiente bastion | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * |
| |Cenário do PAM: Active Directory para florestas do cenário do PAM (CORP) existentes | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
| | Base de dados do Serviço MIM | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 * |
| | SharePoint | SharePoint Foundation 2010 (NR)<br/>SharePoint Foundation 2013 SP1 (NR) <br/> SharePoint 2016 * |
| | Servidor de correio para a aprovação do Serviço MIM e e-mails de gestão de grupos (opcional) | Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (somente notificação antes de Compilar [4.4.1749.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history#version-4417490)) |
| | Browser | Todos os principais navegadores com suporte * (dispositivos móveis limitados)|
| **Relatórios do Serviço MIM** | Windows Server |  Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR) <br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Armazém de dados | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager <br/> System Center 2016 Service Manager * (com 4.4.1459)<br/> [Compatibilidade de Versões do SQL Server para o System Center 2016](https://docs.microsoft.com/system-center/scsm/upgrade-to-sm-2016) |
| **Portais de Registo e Reposição de Palavras-passe do MIM** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Browser | Todos os principais navegadores com suporte |
| **Extensões e Suplementos do MIM** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Integração com o Outlook (opcional) | Outlook 2010 (no Windows, exceto clique para executar)<br/>Outlook 2013 (no Windows, exceto clique para executar) <br/> Outlook 2016 (no Windows 10, exceto clique para executar) * |
| | Cmdlets de requerente do PowerShell de PAM (opcionais) | Windows 8.1<br/>Windows 10 |
| **Gestão de Certificados do MIM** (integração com o Servidor e a AC) | Servidor do Windows | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Autoridade de certificação | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Base de dados do MIM CM | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 * |
| **Gestão de Certificados do MIM** (Aplicação) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **Gerenciamento de certificados do mim** (Cliente em massa) | Windows | Windows 7 |
| **Gerenciamento de certificados do mim** (Cartão inteligente baseado no ActiveX do cliente) | Windows | Windows 7 <br/> Windows 8 <br/> Windows 8.1 <br/> Windows 10 |
| **MIM BHOLD Suite** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Base de dados do BHOLD | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4  <br/> SQL Server 2014 SP3 * <br/> SQL Server 2016 SP2 * |
| | Servidor de correio (opcional) | Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | Browser | Navegadores com suporte do Internet Explorer com Silverlight |
