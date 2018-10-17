---
title: Plataformas de software suportadas| Documentos da Microsoft
description: Localizar os produtos e versões que são compatíveis com cada um dos componentes do MIM 2016
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/11/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: ''
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: 0501dbeb279dc37655d1d9a5e99545b07eea0623
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358691"
---
# <a name="supported-platforms-for-mim-2016"></a>Plataformas suportadas para o MIM 2016

A tabela seguinte descreve as plataformas suportadas e a versão de cada componente do Microsoft Identity Manager 2016. As versões marcadas com um * só são suportadas em MIM 2016 service pack 1 com a correção mais recente.


| **Componente do MIM** | **Plataforma** | **Versão** |
|-------------------|--------------|--------------|
| **Sincronização do MIM** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
| | Nível funcional de diretório Active Directory para aprovisionamento de utilizadores, PCNS e sincronização da GAL | Windows 2000 <br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 *
| | Base de dados da Sincronização do MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| | Active Directory para aprovisionamento de utilizadores, PCNS e sincronização da GAL (opcional)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Exchange para o aprovisionamento de caixas de correio e Sincronização da GAL (opcional)|Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016* |
| | Ambiente de desenvolvimento (opcional) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017* |
| | Sistema ligado adicional (opcional) | Serviços de Domínio do Active Directory<br/>Active Directory<br/>Serviços LDS<br/>SQL Server 2008 ou posterior<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> Outros produtos de terceiros |
| **Serviço e Portal do MIM** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |Cenário de PAM: Windows Server | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |Cenário de PAM: Active Directory para o ambiente de bastion floresta de PAM | Windows Server 2012 R2 <br/> Windows Server 2016 * |
| |Cenário de PAM: Active Directory para florestas do PAM cenário existentes (CORP) | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
| | Base de dados do Serviço MIM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 |
| | SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
| | Servidor de correio para a aprovação do Serviço MIM e e-mails de gestão de grupos (opcional) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * (só de notificação antes da compilação [4.4.1749.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history#version-4417490) |
| | Browser | Todos os browsers principais suportadas * (limitado de dispositivos móveis)|
| **Relatórios do Serviço MIM** | Windows Server |  Windows Server 2008 R2 SP1<br/>Windows Server 2012 <br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Armazém de dados | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager </br> System Center 2016 Service Manager * (com 4.4.1459)<br/> [Compatibilidade de Versões do SQL Server para o System Center 2016](https://docs.microsoft.com/system-center/scsm/upgrade-to-sm-2016)
 |
| **Portais de Registo e Reposição de Palavras-passe do MIM** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | browser | Todos os principais browsers suportados |
| **Extensões e Suplementos do MIM** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Integração com o Outlook (opcional) | Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016 (no Windows 10) * |
| | Cmdlets de requerente do PowerShell de PAM (opcionais) | Windows 8.1<br/>Windows 10 |
| **Gestão de Certificados do MIM** (integração com o Servidor e a AC) | Windows server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Autoridade de certificação | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Base de dados do MIM CM | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
| **Gestão de Certificados do MIM** (Aplicação) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **Gestão de certificados de MIM** (em massa do cliente) | Windows | Windows 7 |
| **Gestão de certificados de MIM** (ActiveX do cliente com base em cartão inteligente) | Windows | Windows 7 </br> Windows 8 </br> Windows 8.1 </br> Windows 10 |
| **MIM BHOLD Suite** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Base de dados do BHOLD | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * <br/> SQL Server 2016 * |
| | Servidor de correio (opcional) | Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | browser | Internet Explorer suportado navegadores com o Silverlight |
