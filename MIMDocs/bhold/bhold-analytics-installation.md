---
title: Instalação da análise do BHOLD | Microsoft Docs
description: O módulo do BHOLD Analytics fornece testes baseados em regras de acesso a dados
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: ec7069156aa033b33a139ae83e26abcdea7b482a
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "64516879"
---
# <a name="bhold-analytics-installation"></a>Instalação do BHOLD Analytics

O módulo análise do BHOLD fornece testes baseados em regras de acesso a dados para garantir que sua organização possa controlar efetivamente o acesso a seus dados e esteja em conformidade com os requisitos de acesso interno e externo. A análise de impacto automatizada gerada pelo módulo análise de BHOLD fornece uma visão geral que mostra o número de usuários que seriam afetados pela imposição de uma regra proposta, tanto aqueles que atenderam à regra quanto aqueles que violariam a regra. O módulo análise do BHOLD também pode fornecer uma lista detalhada dos usuários que obedecerão à regra e aqueles que violariam a regra.

## <a name="bhold-analytics-installation-requirements"></a>Requisitos de instalação do BHOLD Analytics

Antes de instalar o módulo análise do BHOLD, você deve instalar o módulo BHOLD Core no servidor no qual você planeja instalar o módulo análise do BHOLD. Para obter informações sobre como instalar o módulo BHOLD Core, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a instalar o módulo do BHOLD Analytics, você precisa estar preparado para fornecer as informações que o assistente de instalação do BHOLD Analytics exige para concluir a instalação. A planilha a seguir o ajudará a registrar essas informações para que você fique pronto para fornecê-las quando necessário.

| **Item**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Usar o provedor de segurança em domínio/computador** | Quando selecionado, especifica que Active Directory Domain Services segurança controlará o acesso ao BHOLD Core.                                                                                                                | Selecione a caixa de verificação. **Importante:** A instalação falhará se essa caixa de seleção não estiver marcada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que você criou ao instalar o BHOLD Core. Para obter mais informações, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome somente se ele estiver incorreto. **Importante:** Especifique o nome de domínio usando o nome NetBIOS (curto), não o FQDN (nome de domínio totalmente qualificado). Por exemplo, se o FQDN do domínio for fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **User**                                    | Especifica o nome de logon da conta de usuário do serviço do BHOLD Core.                                                                                                                                                          | Escreva o nome da conta de usuário aqui:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                                | Especifica a senha da conta de usuário do serviço.                                                                                                                                                                       | Escreva a senha aqui: **importante:** Lembre-se de manter essa senha em um local oculto e seguro.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>Instalação do BHOLD Analytics

Para instalar o módulo do BHOLD Analytics, faça logon como um membro do grupo Admins. do domínio, baixe o arquivo a seguir e execute-o como administrador no servidor no qual você pretende instalar o módulo análise do BHOLD:

- BholdAnalytics<em>\<versão\></em>\_Release. msi

Substitua *\<versão\>* pelo número de versão da versão de análise do BHOLD que você está instalando.

Para executar o arquivo de programa como administrador, clique com o botão direito do mouse no arquivo e clique em **Executar como administrador**.

# <a name="next-steps"></a>Próximos passos

- [Instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)
- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
