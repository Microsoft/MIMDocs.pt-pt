---
title: Instalação De Análise BHOLD [ Instalação Análise BHOLD ] Microsoft Docs
description: Módulo BHOLD Analytics fornece testes baseados em regras de acesso a dados
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 7d26abb58fa976d40638b9512d5684d86f483378
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79041935"
---
# <a name="bhold-analytics-installation"></a>Instalação analítica BHOLD

O módulo BHOLD Analytics fornece testes baseados em regras de acesso a dados para garantir que a sua organização pode controlar eficazmente o acesso aos seus dados e cumpre os requisitos de acesso interno e externo. A análise automatizada de impacto gerada pelo módulo BHOLD Analytics dá-lhe uma visão geral que mostra o número de utilizadores que seriam afetados pela aplicação de uma regra proposta, tanto aqueles que cumpririam a regra como aqueles que violariam a regra. O módulo BHOLD Analytics também pode fornecer uma lista detalhada dos utilizadores que cumpririam a regra e aqueles que violariam a regra.

## <a name="bhold-analytics-installation-requirements"></a>Requisitos de instalação BHOLD Analytics

Antes de instalar o módulo BHOLD Analytics, tem de instalar o módulo BHOLD Core no servidor no qual planeia instalar o módulo BHOLD Analytics. Para obter informações sobre a instalação do módulo BHOLD Core, consulte a [Instalação Core BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a instalar o módulo BHOLD Analytics, tem de estar preparado para fornecer a informação que o assistente de configuração bhold Analytics necessita para completar a instalação. A seguinte folha de cálculo irá ajudá-lo a registar essa informação para que esteja pronto para fornecê-la quando for necessária.

| **Artigo**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilize o Fornecedor de Segurança no Domínio/Máquina** | Quando selecionado, especifica que a segurança dos Serviços de Domínio do Diretório Ativo controlará o acesso ao BHOLD Core.                                                                                                                | Selecione a caixa de verificação. **Importante:** A instalação falhará se esta caixa de verificação não for selecionada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que criou ao instalar o BHOLD Core. Para mais informações, consulte [a Instalação BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Mude o nome apenas se estiver incorreto. **Importante:** Especifique o nome de domínio utilizando o nome NetBIOS (curto) e não o nome de domínio totalmente qualificado (FQDN). Por exemplo, se o FQDN do domínio for fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **User**                                    | Especifica o nome de início de sessão da conta de utilizador do serviço BHOLD Core.                                                                                                                                                          | Escreva aqui o nome da conta de utilizador:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                                | Especifica a palavra-passe da conta de utilizador do serviço.                                                                                                                                                                       | Escreva a palavra-passe aqui: **Importante:** Certifique-se de manter esta palavra-passe num local escondido e seguro.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>Instalação BHOLD Analytics

Para instalar o módulo BHOLD Analytics, inicie sessão como membro do grupo Domain Admins, descarregue o seguinte ficheiro e execute-o como administrador no servidor que pretende instalar o módulo BHOLD Analytics em:

- Versão\<BholdAnalytics<em>\></em>\_Release.msi

Substitua *\<Versão\>* com o número de versão da versão do lançamento do BHOLD Analytics que está a instalar.

Para executar o ficheiro do programa como administrador, clique no ficheiro à direita e, em seguida, clique em **Executar como administrador**.

## <a name="next-steps"></a>Próximos passos

- [Instalação bhold core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)
- [Guia de instalação BHOLD](bhold-installation-guide.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
