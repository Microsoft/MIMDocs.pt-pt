---
title: Instalação do BHOLD Analytics | Documentos da Microsoft
description: Módulo do BHOLD Analytics fornece baseada em regras de teste de acesso a dados
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 24e43d8ad886fcd7bfa7a9e694754b956f1659cd
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290276"
---
# <a name="bhold-analytics-installation"></a>Instalação do BHOLD Analytics

O módulo do BHOLD Analytics fornece baseada em regras de teste de acesso a dados para garantir que sua organização com eficiência pode controlar o acesso aos seus dados e está em conformidade com requisitos de acesso interno e externo. A análise de impacto automatizada gerada pelo módulo do BHOLD Analytics dá-lhe uma descrição geral que mostra o número de utilizadores que seriam afetados pela imposição de uma regra de proposto, tanto aqueles que estivesse de acordo com a regra e aqueles que violaria a regra. O módulo do BHOLD Analytics também pode fornecer uma lista detalhada dos utilizadores que estivesse de acordo com a regra e aqueles que violaria a regra.

## <a name="bhold-analytics-installation-requirements"></a>Requisitos de instalação do BHOLD Analytics

Antes de instalar o módulo do BHOLD Analytics, tem de instalar o módulo do BHOLD Core no servidor no qual planeia instalar o módulo do BHOLD Analytics. Para obter informações sobre como instalar o módulo do BHOLD Core, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a instalar o módulo do BHOLD Analytics, precisa estar preparado para fornecer as informações que o Assistente de configuração do BHOLD Analytics necessita para concluir a instalação. A seguinte planilha ajudará a gravar essa informação para que estará pronto para fornecer quando for necessário.

| **Item**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilize um fornecedor de segurança num computador/domínio** | Quando selecionada, especifica que a segurança de serviços de domínio do Active Directory irá controlar o acesso a BHOLD Core.                                                                                                                | Selecione a caixa de verificação. **Importante:** a instalação falhará se esta caixa de verificação não estiver selecionada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que criou durante a instalação do BHOLD Core. Para obter mais informações, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome apenas se estiver incorreto. **Importante:** especifique o nome de domínio com o nome NetBIOS do (curto), não o nome de domínio completamente qualificado (FQDN). Por exemplo, se o FQDN do domínio fabrikam.com, especifique o nome de domínio como a FABRIKAM. |
| **User**                                    | Especifica o nome de início de sessão da conta de utilizador de serviço do BHOLD Core.                                                                                                                                                          | Escreva o nome da conta de utilizador aqui:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                                | Especifica a palavra-passe da conta de utilizador de serviço.                                                                                                                                                                       | Escrever a palavra-passe aqui: **importante:** Certifique-se de que tenha esta palavra-passe num local seguro oculto.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>Instalação do BHOLD Analytics

Para instalar o módulo do BHOLD Analytics, inicie sessão como membro do grupo Admins do domínio, transfira o ficheiro seguinte e executá-lo como administrador no servidor que pretende instalar o módulo do BHOLD Analytics num:

- BholdAnalytics<em>\<versão\></em>\_Release.msi

Substitua *\<versão\>* com o número de versão da versão do BHOLD Analytics que está a instalar.

Para executar o ficheiro de programa como administrador, o ficheiro com o botão direito e, em seguida, clique em **executar como administrador**.

# <a name="next-steps"></a>Próximos passos

- [Instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)
- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)