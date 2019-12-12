---
title: Instalação do atestado do BHOLD | Microsoft Docs
description: O módulo de atestado BHOLD permite designar revisores e realizar revisões
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: e4c3a6248585d55fddbbca3153f33734d7c5c429
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "64519175"
---
# <a name="bhold-attestation-installation"></a>Instalação do atestado BHOLD

O módulo de atestado BHOLD permite designar revisores e executar revisões recorrentes das relações entre usuários e contas e permissões por aplicativo.

## <a name="bhold-attestation-installation-requirements"></a>Requisitos de instalação do atestado BHOLD

Antes de instalar o módulo de atestado do BHOLD, você deve instalar o módulo BHOLD Core no servidor no qual planeja instalar o módulo de atestado do BHOLD. Para obter informações sobre como instalar o módulo BHOLD Core, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Como os contatos do módulo de atestado do BHOLD enviam mensagens de email aos usuários, seu ambiente deve ter um servidor de email SMTP, como o Microsoft Exchange Server.

> [!IMPORTANT]
> Se você estiver instalando o BHOLD Reporting e o atestado BHOLD, deverá instalar o BHOLD Reporting antes de instalar o atestado de BHOLD.

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a instalar o módulo de atestado do BHOLD, você precisa estar preparado para fornecer as informações que o assistente de configuração do atestado do BHOLD exige para concluir a instalação. A planilha a seguir o ajudará a registrar essas informações para que você fique pronto para fornecê-las quando necessário.

| **Item**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Usar o provedor de segurança em domínio/computador** | Quando selecionado, especifica que Active Directory Domain Services segurança controlará o acesso ao BHOLD Core.                                                                                                                | Selecione a caixa de verificação. **Importante:** A instalação falhará se essa caixa de seleção não estiver marcada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que você criou ao instalar o BHOLD Core. Para obter mais informações, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome somente se ele estiver incorreto. **Importante:** Especifique o nome de domínio usando o nome NetBIOS (curto), não o FQDN (nome de domínio totalmente qualificado). Por exemplo, se o FQDN do domínio for fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **User**                                    | Especifica o nome de logon da conta de usuário do serviço do BHOLD Core.                                                                                                                                                          | Escreva o nome da conta de usuário aqui:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                                | Especifica a senha da conta de usuário do serviço.                                                                                                                                                                       | Escreva a senha aqui: **importante:** Lembre-se de manter essa senha em um local oculto e seguro.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>Instalação do atestado BHOLD

Para instalar o módulo de atestado do BHOLD, faça logon como um membro do grupo Admins. do domínio, baixe o arquivo a seguir e execute-o como administrador no servidor no qual você pretende instalar o módulo de atestado do BHOLD:

- BholdAttestation<em>\<versão\></em>\_Release. msi

Substitua *\<versão\>* pelo número de versão da versão de atestado BHOLD que você está instalando.

Para executar o arquivo de programa como administrador, clique com o botão direito do mouse no arquivo e clique em **Executar como administrador**.

## <a name="next-steps"></a>Próximos passos

- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
