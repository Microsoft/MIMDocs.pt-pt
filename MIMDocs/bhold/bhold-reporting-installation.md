---
title: Instalação de relatórios do BHOLD | Microsoft Docs
description: O módulo relatório do BHOLD permite gerar relatórios sobre funções e políticas de autorização
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 1f69f9f08cba24898509c771e4477b81c5ed272f
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "64519190"
---
# <a name="bhold-reporting-installation"></a>Instalação do BHOLD Reporting

O módulo relatório do BHOLD fornece a capacidade de gerar relatórios sobre funções e outras políticas de autorização no BHOLD. Esses relatórios costumam ser úteis para a auditoria ou para demonstrar a conformidade com os requisitos regulatórios. Além disso, esse módulo amplia sua capacidade de gerenciar a autorização em sua organização, fornecendo aos usuários as informações de que eles precisam para analisar a associação de suas funções. Os relatórios podem ter exibições restritas que asseguram que os usuários que criam relatórios sejam mostrados apenas as informações que eles têm permissão para ver.

## <a name="bhold-reporting-installation-requirements"></a>Requisitos de instalação do BHOLD Reporting

Antes de instalar o módulo relatório do BHOLD, você deve instalar o módulo BHOLD Core no servidor no qual planeja instalar o módulo relatório do BHOLD. Para obter informações sobre como instalar o módulo BHOLD Core, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

> [!IMPORTANT]
> Se você estiver instalando o BHOLD Reporting e o atestado BHOLD, deverá instalar o BHOLD Reporting antes de instalar o atestado de BHOLD.

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a instalar o módulo de relatório do BHOLD, você precisa estar preparado para fornecer as informações que o assistente de configuração de relatórios do BHOLD requer para concluir a instalação. A planilha a seguir o ajudará a registrar essas informações para que você fique pronto para fornecê-las quando necessário.

| **Item**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Usar o provedor de segurança em domínio/computador** | Quando selecionado, especifica que Active Directory Domain Services segurança controlará o acesso ao BHOLD Core.                                                                                                                | Selecione a caixa de verificação. </br>**Importante:** A instalação falhará se essa caixa de seleção não estiver marcada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que você criou ao instalar o BHOLD Core. Para obter mais informações, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome somente se ele estiver incorreto. **Importante:** Especifique o nome de domínio usando o nome NetBIOS (curto), não o FQDN (nome de domínio totalmente qualificado). Por exemplo, se o FQDN do domínio for fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **User**                                    | Especifica o nome de logon da conta de usuário do serviço do BHOLD Core.                                                                                                                                                          | Escreva o nome da conta de usuário aqui:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                                | Especifica a senha da conta de usuário do serviço.                                                                                                                                                                       | Escreva a senha aqui: </br>**Importante:** Certifique-se de manter essa senha em um local oculto e seguro.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>Instalação do BHOLD Reporting

Para instalar o módulo de relatório do BHOLD, faça logon como um membro do grupo Admins. do domínio, baixe o arquivo a seguir e execute-o como administrador no servidor no qual você pretende instalar o módulo relatório do BHOLD:

- BholdReporting<em>\<versão\></em>\_Release. msi

Substitua *\<versão\>* com o número de versão da versão de relatório do BHOLD que você está instalando.

Para executar o arquivo de programa como administrador, clique com o botão direito do mouse no arquivo e clique em **Executar como administrador**.

## <a name="next-steps"></a>Próximos passos

- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
