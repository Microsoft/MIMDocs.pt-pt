---
title: Plataformas de software suportadas| Documentos da Microsoft
description: Localizar os produtos e versões que são compatíveis com cada um dos componentes do MIM 2016
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/19/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: ''
ms.suite: ems
ms.custom: mim
ms.openlocfilehash: f69566c9d6abc6b7c54cc875a958b66a112c3111
ms.sourcegitcommit: b09a8c93983d9d92ca4871054650b994e9996ecf
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2019
ms.locfileid: "73329357"
---
# <a name="supported-platforms-for-mim-2016"></a>Plataformas suportadas para o MIM 2016

A tabela seguinte descreve as plataformas suportadas e a versão de cada componente do Microsoft Identity Manager 2016. As versões marcadas com um * só são suportadas no MIM 2016 service pack 1. As versões marcadas com * * só têm suporte no MIM 2016 service pack 2 ou em um hotfix posterior. As versões marcadas com "NR", para não recomendado, têm suporte, mas não são recomendadas se iniciar uma implantação nova da plataforma para MIM.


| **Componente do MIM** | **Plataforma** | **Versão** |
|-------------------|--------------|--------------|
| **Sincronização do MIM** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2<br/>Windows Server 2016 *<br/>Windows Server 2019 * * |
| | Active Directory nível funcional para provisionamento de usuário, PCNS e sincronização de GAL | Windows 2000 (NR)<br/>Windows Server 2003<br/>Windows Server 2008<br/>Windows Server 2008 R2<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 *
| | Base de dados da Sincronização do MIM | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/>SQL Server 2016 SP2 *<br/>SQL Server 2017 * * |
| | Active Directory para provisionamento de usuário, PCNS e sincronização de GAL (opcional)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 * * |
| | Exchange para o aprovisionamento de caixas de correio e Sincronização da GAL (opcional)|Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1<br/>Exchange Server 2016 *<br/>Exchange Server 2019 * * |
| | Ambiente de desenvolvimento (opcional) | Visual Studio 2012<br/>Visual Studio 2013 <br/> Visual Studio 2015 <br/> Visual Studio 2017 * |
| | Sistema ligado adicional (opcional) | Serviços de Domínio do Active Directory<br/>Serviços de Domínio do<br/>Serviços LDS<br/>SQL Server 2008 ou posterior<br/>SharePoint Server 2013<br/> SharePoint Server 2016 *<br/> SharePoint Server 2019 * * <br/> Outros produtos de terceiros |
| **Serviço e Portal do MIM** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 * * |
| |Cenário do PAM: Windows Server | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * |
| |Cenário do PAM: Active Directory para floresta do PAM no ambiente de bastiões | Windows Server 2012 R2 (NR) <br/> Windows Server 2016 * |
| |Cenário do PAM: Active Directory para florestas do cenário do PAM (CORP) existentes | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
| | Base de dados do Serviço MIM | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 *<br/> SQL Server 2017 * * |
| | SharePoint | SharePoint Foundation 2010 (NR)<br/>SharePoint Foundation 2013 SP1 (NR) <br/> SharePoint 2016 *<br/> SharePoint 2019 * * |
| | Servidor de correio para a aprovação do Serviço MIM e e-mails de gestão de grupos (opcional) | Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 *<br/> Exchange Server 2019 * * <br/> Exchange Online * (somente notificação antes de Compilar [4.4.1749.0](https://docs.microsoft.com/microsoft-identity-manager/reference/version-history#version-4417490)) |
| | Browser | Todos os principais navegadores com suporte * (dispositivos móveis limitados)|
| **Relatórios do Serviço MIM** | Windows Server |  Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR) <br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 * * |
| | Armazém de dados | System Center 2012 Service Manager <br/> System Center 2012 R2 Service Manager <br/> System Center 2016 Service Manager * (com 4.4.1459)<br/> System Center 2019 Service Manager * * |
| **Portais de Registo e Reposição de Palavras-passe do MIM** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 * * |
| | browser | Todos os principais navegadores com suporte |
| **Extensões e Suplementos do MIM** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>10 do Windows |
| | Integração com o Outlook (opcional) | Outlook 2010 (no Windows, exceto clique para executar)<br/>Outlook 2013 (no Windows, exceto clique para executar) <br/> Outlook 2016 (no Windows 10, exceto clique para executar) *<br/>Office 365 Outlook (no Windows 10, incluindo o clique para executar) * * |
| | Cmdlets de requerente do PowerShell de PAM (opcionais) | Windows 8.1<br/>10 do Windows |
| **Gestão de Certificados do MIM** (integração com o Servidor e a AC) | Windows server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 * * |
| | Autoridade de certificação | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 *<br/> Windows Server 2019 * * |
| | Base de dados do MIM CM | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4 (NR)<br/>SQL Server 2014 SP3 (NR) <br/> SQL Server 2016 SP2 *<br/> SQL Server 2017 * * |
| **Gestão de Certificados do MIM** (Aplicação) | Windows | Windows 8<br/>Windows 8.1<br/>10 do Windows |
| **Gerenciamento de certificados do mim** (cliente em massa) | Windows | Windows 7 |
| **Gerenciamento de certificados do mim** (cartão inteligente baseado em ActiveX do cliente) | Windows | Windows 7 <br/> Windows 8 <br/> Windows 8.1 <br/> 10 do Windows |
| **MIM BHOLD Suite** | Windows Server | Windows Server 2008 R2 SP1 (NR)<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
| | Base de dados do BHOLD | SQL Server 2008 R2 SP3 (NR)<br/>SQL Server 2012 SP4  <br/> SQL Server 2014 SP3 * <br/> SQL Server 2016 SP2 * |
| | Servidor de correio (opcional) | Exchange Server 2010 SP3 (NR)<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * |
| | browser | Navegadores com suporte do Internet Explorer com Silverlight |
