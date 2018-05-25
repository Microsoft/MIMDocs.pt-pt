---
title: Instalação do BHOLD Analytics | Microsoft Docs
description: Módulo do BHOLD Analytics fornece baseadas em regras de teste de acesso a dados
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 2cff1f0b048a647198be6c05c658713750b41180
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/25/2018
---
# <a name="bhold-analytics-installation"></a>Instalação do BHOLD Analytics

O módulo do BHOLD Analytics fornece baseadas em regras de teste de acesso a dados para garantir que a sua organização eficazmente pode controlar o acesso aos seus dados e está em conformidade com os requisitos de acesso internos e externos. A análise de impacto automatizada gerada pelo módulo do BHOLD Analytics dá-lhe uma descrição geral que mostra o número de utilizadores que seriam afetados pela imposição de uma regra proposta, tanto a quem deverá estar em conformidade com a regra e quem seria violar a regra. O módulo do BHOLD Analytics também pode fornecer uma lista detalhada dos utilizadores que deverá estar em conformidade com a regra e quem seria violar a regra.

## <a name="bhold-analytics-installation-requirements"></a>Requisitos de instalação do BHOLD Analytics

Antes de instalar o módulo do BHOLD Analytics, tem de instalar o módulo de núcleo do BHOLD no servidor no qual planeia instalar o módulo do BHOLD Analytics. Para obter informações sobre como instalar o módulo do BHOLD Core, consulte [BHOLD Core instalação](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a instalar o módulo do BHOLD Analytics, tem de estar preparado para fornecer as informações que o Assistente de configuração do BHOLD Analytics necessita para concluir a instalação. A folha de cálculo seguinte irão ajudá-lo, registe essa informação para estará pronto para fornecê-lo quando for necessário.

| **Item**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilize um fornecedor de segurança no computador/domínio** | Quando selecionada, especifica que a segurança dos serviços de domínio do Active Directory controlará o acesso para BHOLD núcleo.                                                                                                                | Selecione a caixa de verificação. **Importante:** a instalação falhará se esta caixa de verificação não estiver selecionada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que criou ao instalar BHOLD Core. Para obter mais informações, consulte [BHOLD Core instalação](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome apenas se estiver incorreto. **Importante:** especifique o nome de domínio com o nome de NetBIOS (abreviado), não o nome de domínio completamente qualificado (FQDN). Por exemplo, se o FQDN do domínio for fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **User**                                    | Especifica o nome de início de sessão da conta de utilizador do serviço de núcleo do BHOLD.                                                                                                                                                          | Escreva o nome da conta de utilizador aqui:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                                | Especifica a palavra-passe da conta de utilizador de serviço.                                                                                                                                                                       | Escrever a palavra-passe aqui: **importante:** não se esqueça de manter esta palavra-passe numa localização segura, oculta.                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>Instalação do BHOLD Analytics

Para instalar o módulo do BHOLD Analytics, inicie sessão como membro do grupo Admins do domínio, transfira o ficheiro seguinte e executá-la como administrador no servidor que tenciona instalar o módulo do BHOLD Analytics no:

- BholdAnalytics*\<versão\>*\_Release.msi

Substitua *\<versão\>* com o número de versão da versão do BHOLD Analytics que está a instalar.

Para executar o ficheiro de programa como administrador, clique com o botão direito do ficheiro e, em seguida, clique em **executar como administrador**.

# <a name="next-steps"></a>Próximos passos

- [Instalação de núcleo do BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)
- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)