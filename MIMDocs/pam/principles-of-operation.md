---
title: Compreender os componentes do PAM | Documentos da Microsoft
description: A Gestão de Acesso Privilegiado partilha alguns componentes com o MIM e tem de alguns próprios. Saiba como estes funcionam em conjunto.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 6498f68f-36d3-448c-8fe6-649ad5a7f97d
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e3e248092f674a031e255eb368681ab1b6dc21df
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010698"
---
# <a name="understand-the-components-of-mim-pam"></a>Compreender os componentes da MIM PAM

A Gestão privilegiada de acesso mantém o acesso administrativo separado das contas do dia-a-dia dos utilizadores utilizando uma floresta separada.

> [!NOTE]
> A abordagem PAM fornecida pela MIM destina-se a ser utilizada numa arquitetura personalizada para ambientes isolados onde o acesso à Internet não esteja disponível, onde esta configuração é exigida por regulamentação, ou em ambientes isolados de alto impacto, como laboratórios de investigação offline e tecnologia operacional desligada ou ambientes de controlo de supervisão e aquisição de dados. Se o seu Ative Directory faz parte de um ambiente ligado à Internet, consulte [a garantia de acesso privilegiado](/security/compass/overview) para obter mais informações sobre por onde começar.

 Esta solução depende de florestas paralelas:

- *CORP*: floresta empresarial para fins gerais que inclui um ou mais domínios. Embora possa ter várias florestas CORP, os exemplos nestes artigos assumem uma única floresta com um único domínio de simplicidade.  
- *PRIV*: floresta dedicada criada especialmente para este cenário de PAM. Esta floresta inclui um domínio para acomodar contas e grupos privilegiados com um ou mais domínios CORP.

A solução MIM, conforme foi configurada para PAM, inclui os seguintes componentes:  

- **Serviço MIM**: implementa lógica de negócio para realizar operações de gestão de identidades e acessos, incluindo o processamento de pedidos de gestão e elevação de contas privilegiadas.
- **Portal MIM**: um portal baseado no SharePoint, hospedado pelo SharePoint 2013 ou posteriormente, que fornece uma UI de gestão e configuração de administrador.
- **Base de Dados de Serviço MIM**: armazenada no SQL Server 2012 ou posteriormente, e detém dados de identidade e meta-dados necessários para o Serviço MIM.
- **Serviço de Monitorização de PAM** e **Serviço de Componentes de PAM**: dois serviços que gerem o ciclo de vida de contas privilegiadas e ajudam o PRIV AD no ciclo de vida de associação a grupos.
- **Cmdlets do PowerShell**: para povoar o Serviço MIM e o PRIV AD com utilizadores e grupos que correspondem aos utilizadores e grupos na floresta CORP para os administradores de PAM e para os utilizadores finais pedirem apenas a utilização de privilégios just-in-time (JIT) numa conta administrativa.
- **API REST e portal de exemplo de PAM**: para programadores que integram o MIM no cenário de PAM com clientes personalizados para elevação, sem necessitar de utilizar o PowerShell ou SOAP. A utilização da API REST é demonstrada através de uma aplicação Web de exemplo.

Uma vez instalado e configurado, cada grupo criado pelo procedimento de migração na floresta PRIVADA é um grupo principal estrangeiro que espelha o grupo na floresta corp original. O grupo principal estrangeiro fornece aos utilizadores que são membros desse grupo com o mesmo SID no seu token Kerberos como o SID do grupo na floresta CORP. Além disso, quando o Serviço MIM adiciona membros a estes grupos na floresta PRIV, essas associações terão tempo limitado.

Consequentemente, quando um utilizador pede a elevação utilizando os cmdlets do PowerShell e o pedido é aprovado, o Serviço MIM irá adicionar a conta na floresta PRIV a um grupo na floresta PRIV. Quando o utilizador inicia sessão com a respetiva conta privilegiada, o token Kerberos irá conter um Identificador de Segurança (SID) idêntico ao SID do grupo na floresta CORP. Uma vez que a floresta CORP está configurada para confiar na floresta PRIV, a conta elevada a ser utilizada para aceder a um recurso na floresta CORP, num recurso que verifica as associações a grupos Kerberos, é membro dos grupos de segurança desse recurso. Isto é fornecido através da autenticação Kerberos entre florestas.

Além disso, estas associações têm tempo limitado para que, após um intervalo de tempo pré-configurado, a conta administrativa do utilizador já não faça parte do grupo na floresta PRIV. Como resultado, essa conta já não poderá ser utilizada para aceder a recursos adicionais.
