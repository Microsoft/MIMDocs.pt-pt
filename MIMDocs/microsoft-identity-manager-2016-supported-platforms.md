---
title: Plataformas de software suportadas| Documentos da Microsoft
description: "Localizar os produtos e versões que são compatíveis com cada um dos componentes do MIM 2016"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: f1faed3a09023e288a68a8950dae43725f19eb3e
ms.openlocfilehash: 33c84afa4d6fd2ed7bde33de39dd151f83f07fd4
ms.lasthandoff: 04/12/2017


---

# <a name="supported-platforms-for-mim-2016"></a>Plataformas suportadas para o MIM 2016

A tabela seguinte descreve as plataformas suportadas e a versão de cada componente do Microsoft Identity Manager 2016. As versões marcadas com um * só são suportadas no MIM 2016 service pack 1.


| **Componente do MIM** | **Plataforma** | **Versão** |
|-------------------|--------------|-------------|
| **Sincronização do MIM** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
| | Base de dados da Sincronização do MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Active Directory para o aprovisionamento de utilizadores, PCNS e Sincronização da GAL (opcional)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Exchange para o aprovisionamento de caixas de correio e Sincronização da GAL (opcional)|Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016* |
| | Ambiente de desenvolvimento (opcional) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017* |
| | Sistema ligado adicional (opcional) | Serviços de Domínio do Active Directory<br/>Active Directory<br/>Serviços LDS<br/>SQL Server 2000 ou posterior<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> Outros produtos de terceiros |
| **Serviço MIM** (exceto o cenário de PAM) | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Base de dados do Serviço MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Exchange para a aprovação do Serviço MIM e e-mails de gestão de grupos (opcional) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (Apenas Notificações) |
| **Portal e Serviço MIM** (apenas para cenário de PAM)| Windows Server | Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Active Directory para a floresta de PAM do ambiente bastion | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Active Directory para florestas existentes | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
| | Base de dados do Serviço MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
| | Servidor de correio para a aprovação do Serviço MIM e e-mails de gestão de grupos (opcional) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (Apenas Notificações) |
| | Browser | Todos os browsers principais |
| **Relatórios do Serviço MIM** | Windows Server | Windows Server 2012 <br/> Windows Server 2016 * |
| | Armazém de dados | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager </br> System Center 2016 Service Manager * (com 4.4.1459)<br/> [Compatibilidade de Versões do SQL Server para o System Center 2016](https://technet.microsoft.com/en-us/system-center-docs/system-requirements/sql-server-version-compatibility)
 |
| **Portais de Registo e Reposição de Palavras-passe do MIM** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | browser | Todos os browsers principais |
| **Extensões e Suplementos do MIM** | Windows | Windows 7<br/>Windows 8<br/>Windows 8,1<br/>Windows 10 |
| | Integração com o Outlook (opcional) | Outlook 2007 SP2<br/>Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016 (no Windows 10) * |
| | Cmdlets de requerente do PowerShell de PAM (opcionais) | Windows 8.1<br/>Windows 10 |
| **Gestão de Certificados do MIM** (integração com o Servidor e a AC) | Windows server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Autoridade de certificação | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Base de dados do MIM CM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| **Gestão de Certificados do MIM** (Aplicação) | Windows | Windows 8<br/>Windows 8,1<br/>Windows 10 |
| **Gestão de Certificados do MIM** (Cliente e Cliente em Massa) | Windows | Windows 7 |
| **MIM BHOLD Suite** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Base de dados do BHOLD | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * <br/> SQL Server 2016 * |
| | Servidor de correio (opcional) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | browser | Internet Explorer 7, 8, 9, 10 ou 11 com o Silverlight |
