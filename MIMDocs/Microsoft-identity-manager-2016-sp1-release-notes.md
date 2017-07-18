---
title: "Microsoft Identity Manager 2016 Service Pack 1 | Documentos da Microsoft"
description: "Compreenda como funciona o MIM 2016 para criar uma experiência de gestão de identidades mais segura e conveniente na nuvem e no local."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 69d44af5eaef3665f3a55ea91f48d3658cd5e65c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# Novidades do Microsoft Identity Manager 2016 Service Pack 1
<a id="whats-new-for-microsoft-identity-manager-2016-service-pack-1" class="xliff"></a> #

Como parte do ciclo de lançamentos regulares para a manutenção e a atualização do Microsoft Identity Manager, é com satisfação que anunciamos o [Microsoft Identity Manager (MIM) 2016 Service Pack 1 (SP1)](https://msdn.microsoft.com/subscriptions/downloads/?fileid=70212#searchTerm=&Languages=en&PageSize=10&PageIndex=0&FileId=70212). Este documento descreve as atualizações, as funcionalidades, as alterações e os melhoramentos incluídos nesta versão.

Se ocorrerem problemas durante a implementação de produção do MIM SP1, contacte o suporte técnico da Microsoft.

Também queremos saber a sua opinião! Se tiver comentários ou preocupações que deseje ver esclarecidos pela equipa do produto, envie-nos um e-mail para [mim2016@microsoft.com.](mailto:mim2016@microsoft.com)



## Atualizações neste service pack
<a id="updates-in-this-service-pack" class="xliff"></a> #

### MIM
<a id="mim" class="xliff"></a>

- **Compatibilidade entre browsers do Portal do MIM para o utilizador final self-service:** neste service pack, introduzimos suporte para a maioria dos browsers principais. Os utilizadores podem agora aceder e interagir com o Portal do MIM no grupo self-service e na gestão de perfis a partir do Microsoft Edge, Chrome e Safari.

- **Suporte do Serviço MIM para Exchange Online:** o serviço MIM há muito tempo que suporta o envio e a receção de e-mails de aprovações e notificações. Antes do SP1, o MIM apenas suportava o Exchange Server ou SMTP. Com o service pack 1, o Serviço MIM pode enviar e receber pedidos, bem como notificações de e-mail através de uma conta online do Office 365 Exchange.

- **Validação do formato de ficheiros de imagem ao carregar:** o MIM agora é capaz de validar o formato de ficheiros de imagem quando são carregados para o portal.

### Privileged Access Management (PAM)
<a id="privileged-access-managementpam" class="xliff"></a>

- **Suporte da floresta (bastion) “PRIV” da PAM para o nível funcional do Windows Server 2016:** o Serviço PAM do MIM pode ser configurado num ambiente com controladores de domínio em execução no nível funcional da floresta dos Serviços de Domínio do Active Directory do Windows Server 2016. Quando configurado, a permissão Kerberos do utilizador terá um prazo limitado ao tempo restante da respetiva ativação da função.

    >[!Note]
    Se optar por manter o nível funcional da floresta do Windows Server 2012 R2 no domínio CORP, recomenda-se instalar [KB 2919442](https://support.microsoft.com/en-us/kb/2919442) e [ 2919355](https://support.microsoft.com/en-us/kb/2919355) no controlador de domínio CORP.

- **Elevação da conta com privilégios para grupos exclusivos da floresta (bastion) “PRIV”:** agora, os administradores podem informar o Serviço MIM relativamente a grupos e utilizadores exclusivos da floresta “PRIV”. Este procedimento permite a estes grupos e utilizadores serem incluídos nas funções de PAM.  Em seguida, podem ser ativados para uma função e associados a grupos na floresta “PRIV”.

- **Scripts de Implementação da PAM:** os Scripts de Implementação da PAM permitem aos administradores otimizarem a instalação do ambiente de PAM.

- **Cmdlets da PAM para a configuração Silo de Políticas de Autenticação:** o service pack 1 introduz Cmdlets novos para reforçar a segurança da floresta bastion. Estes Cmdlets criam automaticamente um Silo de Políticas de Autenticação, vinculado a um Modelo de Políticas de Autenticação.

    >[!Note]
    Estes Cmdlets são executados automaticamente como parte dos scripts de implementações.


## Suporte da Plataforma
<a id="platform-support" class="xliff"></a>
Pode encontrar informações atualizadas sobre as plataformas suportadas no documento denominado [Plataformas suportadas para o MIM 2016](microsoft-identity-manager-2016-supported-platforms.md).  As novas plataformas suportadas neste service pack incluem o SQL Server 2016 e o SharePoint 2016

## Problemas corrigidos nesta versão de Disponibilidade Geral do MIM 2016
<a id="issues-fixed-in-this-release-from-mim-2016-general-availability" class="xliff"></a>

### PAM
<a id="pam" class="xliff"></a>
- O cmdlet New-PAMGroup não criava objetos MIM para grupos locais de domínio na floresta PRIV
- O cmdlet New-PAMDomainConfiguration falhava com uma mensagem de erro “netdom”
- O Serviço de Monitorização de PAM registava avisos de grupos na floresta PRIV

## Como atualizar para o Service Pack 1
<a id="how-to-upgrade-to-service-pack-1" class="xliff"></a>

Os clientes que atualizarem para o Microsoft Identity Manager 2016 Service Pack 1 devem seguir as seguintes instruções em todos os serviços aplicáveis à sua implementação.

>[!Note]
>Os clientes com o Forefront Identity Manager 2010 R2 SP1 ou anterior têm primeiro de atualizar o respetivo ambiente para o Microsoft Identity Manager 2016, lançado em agosto de 2015, e seguir os passos abaixo.

Antes de começar

Tem de atualizar o motor de Sincronização do MIM antes de atualizar o portal e o serviço MIM.
Tem de criar uma cópia de segurança das bases de dados de Sincronização do MIM e MIMService.

  1. Desinstale o componente do Microsoft Identity Manager que está a atualizar
  2. Depois de concluída a desinstalação, abra a página inicial localizada no suporte de dados de instalação “FIMSplash.htm”
  3. Selecione o componente do MIM a atualizar
  4. Siga os avisos para continuar com a instalação
    * Instalação do Portal e do Serviço MIM: ao escolher o Exchange Online como a conta de correio, introduza o endereço de e-mail e as credenciais da conta do Exchange Online no ecrã seguinte.
