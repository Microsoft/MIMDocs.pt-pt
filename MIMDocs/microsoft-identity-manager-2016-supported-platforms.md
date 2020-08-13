---
title: Plataformas de software suportadas| Documentos da Microsoft
description: Localizar os produtos e versões que são compatíveis com cada um dos componentes do MIM 2016
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/19/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: ''
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: 04e96f3c8eb71041dccb173f2a1dc7dbba46ef01
ms.sourcegitcommit: cd503e8e9933d39d6fbf894c7d27bf9566301ac8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/13/2020
ms.locfileid: "88168342"
---
# <a name="supported-platforms-for-mim-2016"></a>Plataformas suportadas para o MIM 2016

A tabela seguinte descreve as plataformas suportadas e a versão de cada componente do Microsoft Identity Manager 2016. As versões marcadas com um * só são suportadas no MIM 2016 service pack 1. As versões marcadas com ** só são suportadas no pacote de serviços MIM 2016 ou num hotfix posterior. As versões marcadas com "NR", para não recomendado, são suportadas, mas não são recomendadas se iniciar uma nova implantação daquela plataforma para MIM.


| **Componente do MIM** | **Plataforma** | **Versão** |
|-------------------|--------------|--------------|
| **Sincronização do MIM** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2<br/>Windows Server 2016 *<br/>Windows Server 2019 ** |
| | Nível funcional do Diretório Ativo para o fornecimento de utilizadores, PCNS e GAL Sync | Windows 2000 (NR)<br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 *
| | Base de dados da Sincronização do MIM | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/>SQL Server 2016 SP2 * <br/>SQL Server 2017 ** <br/> SQL Server 2019 ** |
| | Diretório ativo para provisionamento de utilizadores, PCNS e GAL Sync (opcional)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | Exchange para o aprovisionamento de caixas de correio e Sincronização da GAL (opcional)|Servidor de Troca 2010 SP3 (NR)<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016 *<br/>Servidor de Troca 2019 ** |
| | Ambiente de desenvolvimento (opcional) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Estúdio Visual 2017 * |
| | Sistema ligado adicional (opcional) | Active Directory Domain Services<br/>Active Directory<br/>Serviços LDS<br/>SQL Server 2008 ou posterior<br/>SharePoint Server 2013<br/> SharePoint Server 2016 *<br/> SharePoint Server 2019 ** <br/> Outros produtos de terceiros |
| **Portal e Serviço do MIM** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| |Cenário PAM: Servidor do Windows | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * <br/> Windows Server 2019 **|
| |Cenário PAM: Diretório ativo para o ambiente de bastião floresta PAM | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * <br/> Windows Server 2019 ** |
| |Cenário PAM: Diretório ativo para as florestas existentes (CORP) | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * <br/> Windows Server 2019 ** |
| | Base de dados do Serviço MIM | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 *<br/> SQL Server 2017 ** <br/> SQL Server 2019 ** |
| | SharePoint | SharePoint Foundation 2010 (NR)<br/>SharePoint Foundation 2013 SP1 (NR) <br/> SharePoint 2016 *<br/> SharePoint 2019 ** |
| | Servidor de correio para a aprovação do Serviço MIM e e-mails de gestão de grupos (opcional) | Servidor de Troca 2010 SP3 (NR)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 *<br/> Servidor de Troca 2019 ** <br/> Exchange Online * (Notificação apenas antes de construir [4.4.1749.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history#version-4417490)) |
| | Browser | Todos os principais navegadores suportados * (dispositivos móveis limitados)|
| **Relatórios do Serviço MIM** | Windows Server |  Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR) <br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | Armazém de dados | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager <br/> System Center 2016 Service Manager * (com 4.4.1459)<br/> Gestor de serviços do System Center 2019 ** |
| **Portais de Registo e Reposição de Palavras-passe do MIM** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | Browser | Todos os principais navegadores suportados |
| **Extensões e Suplementos do MIM** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
| | Integração com o Outlook (opcional) | Outlook 2010 (no Windows, exceto Click-To-Run)<br/>Outlook 2013 (no Windows, exceto Click-To-Run) <br/> Outlook 2016 (no Windows 10, exceto Click-To-Run) *<br/>Outlook do Office 365 (no Windows 10, incluindo Click-To-Run) ** |
| | Cmdlets de requerente do PowerShell de PAM (opcionais) | Windows 8.1<br/>Windows 10 |
| **Gestão de Certificados do MIM** (integração com o Servidor e a AC) | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | Autoridade de certificação | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 ** |
| | Base de dados do MIM CM | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 *<br/> SQL Server 2017 ** |
| **Gestão de Certificados do MIM** (Aplicação) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **Gestão de Certificados MIM** (Cliente a granel) | Windows | Windows 7 |
| **Gestão de Certificados MIM** (cartão inteligente baseado em Cliente ActiveX) | Windows | Windows 7 <br/> Windows 8 <br/> Windows 8.1 <br/> Windows 10 |
| **MIM BHOLD Suite** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Base de dados do BHOLD | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4  <br/> SQL Server 2014 SP3 * <br/> SQL Server 2016 SP2 * |
| | Servidor de correio (opcional) | Servidor de Troca 2010 SP3 (NR)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | Browser | Navegadores suportados pelo Internet Explorer com Silverlight |
