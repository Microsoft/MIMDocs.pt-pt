---
title: BHOLD reportando Instalação [ De informação BHOLD] Microsoft Docs
description: O módulo de relatórioBHOLD permite-lhe gerar relatórios sobre papéis e políticas de autorização
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 7bac40105aa53add914599c59e812587b6be9585
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79042038"
---
# <a name="bhold-reporting-installation"></a>Instalação de relatórios BHOLD

O módulo BHOLD Reporting dá-lhe a capacidade de gerar relatórios sobre papéis e outras políticas de autorização no BHOLD. Estes relatórios são muitas vezes úteis para a auditoria ou para demonstrar o cumprimento dos requisitos regulamentares. Além disso, este módulo alarga a sua capacidade de gerir a sua autorização dentro da sua organização, dando aos utilizadores a informação de que precisam para analisar a adesão das suas funções. Os relatórios podem ter opiniões restritas que garantam que os utilizadores que criam relatórios sejam mostrados apenas a informação que podem ver.

## <a name="bhold-reporting-installation-requirements"></a>Requisitos de instalação de relatórios BHOLD

Antes de instalar o módulo BHOLD Reporting, tem de instalar o módulo BHOLD Core no servidor no qual planeia instalar o módulo BHOLD Reporting. Para obter informações sobre a instalação do módulo BHOLD Core, consulte a [Instalação Core BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

> [!IMPORTANT]
> Se estiver a instalar tanto o BHOLD Reporting como o BHOLD Attestation, tem de instalar o BHOLD Reporting antes de instalar a Estação BHOLD.

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a instalar o módulo BHOLD Reporting, tem de estar preparado para fornecer as informações que o assistente de configuração de relatórios BHOLD necessita para completar a instalação. A seguinte folha de cálculo irá ajudá-lo a registar essa informação para que esteja pronto para fornecê-la quando for necessária.

| **Artigo**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilize o Fornecedor de Segurança no Domínio/Máquina** | Quando selecionado, especifica que a segurança dos Serviços de Domínio do Diretório Ativo controlará o acesso ao BHOLD Core.                                                                                                                | Selecione a caixa de verificação. </br>**Importante:** A instalação falhará se esta caixa de verificação não for selecionada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que criou ao instalar o BHOLD Core. Para mais informações, consulte [a Instalação BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Mude o nome apenas se estiver incorreto. **Importante:** Especifique o nome de domínio utilizando o nome NetBIOS (curto) e não o nome de domínio totalmente qualificado (FQDN). Por exemplo, se o FQDN do domínio for fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **User**                                    | Especifica o nome de início de sessão da conta de utilizador do serviço BHOLD Core.                                                                                                                                                          | Escreva aqui o nome da conta de utilizador:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                                | Especifica a palavra-passe da conta de utilizador do serviço.                                                                                                                                                                       | Escreva a senha aqui: </br>**Importante:** Certifique-se de manter esta senha num local escondido e seguro.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>Instalação de relatórios BHOLD

Para instalar o módulo BHOLD Reporting, inicie sessão como membro do grupo Domain Admins, descarregue o seguinte ficheiro e execute-o como administrador no servidor que pretende instalar o módulo BHOLD Reporting em:

- Versão\<DeBholdReporting<em>\></em>\_Release.msi

Substitua *\<Versão\>* com o número de versão da versão do lançamento do Relatório BHOLD que está a instalar.

Para executar o ficheiro do programa como administrador, clique no ficheiro à direita e, em seguida, clique em **Executar como administrador**.

## <a name="next-steps"></a>Próximos passos

- [Guia de instalação BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
