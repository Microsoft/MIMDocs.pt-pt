---
title: Compreender os componentes do PAM | Documentos da Microsoft
description: "A Gestão de Acesso Privilegiado partilha alguns componentes com o MIM e tem de alguns próprios. Saiba como estes funcionam em conjunto."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 6498f68f-36d3-448c-8fe6-649ad5a7f97d
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 53fe79f251c3b18426f16b4007cda49e67d7b028
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# Compreender os componentes de PAM
<a id="understand-the-components-of-pam" class="xliff"></a>

A Gestão de Acesso Privilegiado mantém o acesso administrativo separado das contas de utilizador quotidianas. Esta solução depende de florestas paralelas:

- *CORP*: floresta empresarial para fins gerais que inclui um ou mais domínios. Embora possa ter várias florestas CORP, os exemplos nestes artigos assumem uma única floresta com um único domínio de simplicidade.  
- *PRIV*: floresta dedicada criada especialmente para este cenário de PAM. Esta floresta inclui um domínio para acomodar contas e grupos privilegiados com um ou mais domínios CORP.

A solução MIM, conforme foi configurada para PAM, inclui os seguintes componentes:  

- **Serviço MIM**: implementa lógica de negócio para realizar operações de gestão de identidades e acessos, incluindo o processamento de pedidos de gestão e elevação de contas privilegiadas.   
- **Portal do MIM**: portal baseado no SharePoint, alojado pelo SharePoint 2013, que fornece um administrador de gestão e IU de configuração.
- **Base de Dados do Serviço MIM**: armazenada no SQL Server 2012 ou 2014 e contém metadados e dados de identidade necessários para o Serviço MIM.
- **Serviço de Monitorização de PAM** e **Serviço de Componentes de PAM**: dois serviços que gerem o ciclo de vida de contas privilegiadas e ajudam o PRIV AD no ciclo de vida de associação a grupos.
- **Cmdlets do PowerShell**: para povoar o Serviço MIM e o PRIV AD com utilizadores e grupos que correspondem aos utilizadores e grupos na floresta CORP para os administradores de PAM e para os utilizadores finais pedirem apenas a utilização de privilégios just-in-time (JIT) numa conta administrativa.
- **API REST e portal de exemplo de PAM**: para programadores que integram o MIM no cenário de PAM com clientes personalizados para elevação, sem necessitar de utilizar o PowerShell ou SOAP. A utilização da API REST é demonstrada através de uma aplicação Web de exemplo.

Depois de instalado e configurado, cada grupo criado pelo procedimento de migração na floresta PRIV é um grupo de segurança baseado em SIDHistory sombra (ou, numa atualização posterior, no Windows Server vNext, um grupo principal externo) que espelha o grupo SID na floresta CORP original. Além disso, quando o Serviço MIM adiciona membros a estes grupos na floresta PRIV, essas associações terão tempo limitado.

Consequentemente, quando um utilizador pede a elevação utilizando os cmdlets do PowerShell e o pedido é aprovado, o Serviço MIM irá adicionar a conta na floresta PRIV a um grupo na floresta PRIV. Quando o utilizador inicia sessão com a respetiva conta privilegiada, o token Kerberos irá conter um Identificador de Segurança (SID) idêntico ao SID do grupo na floresta CORP. Uma vez que a floresta CORP está configurada para confiar na floresta PRIV, a conta elevada a ser utilizada para aceder a um recurso na floresta CORP, num recurso que verifica as associações a grupos Kerberos, é membro dos grupos de segurança desse recurso. Isto é fornecido através da autenticação Kerberos entre florestas.

Além disso, estas associações têm tempo limitado para que, após um intervalo de tempo pré-configurado, a conta administrativa do utilizador já não faça parte do grupo na floresta PRIV. Como resultado, essa conta já não poderá ser utilizada para aceder a recursos adicionais.
