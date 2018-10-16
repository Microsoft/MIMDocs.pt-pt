---
title: BHOLD reporting instalação | Documentos da Microsoft
description: Módulo de relatórios do BHOLD permite-lhe gerar relatórios sobre as funções e políticas de autorização
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 7c95529bf1cd227bee8f1dde11c94a810b44fc8c
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332260"
---
# <a name="bhold-reporting-installation"></a>Relatórios de instalação do BHOLD

O módulo de relatórios do BHOLD dá-lhe a capacidade de gerar relatórios sobre as funções e de outras políticas de autorização no BHOLD. Estes relatórios são muitas vezes útil para auditoria ou para demonstrar a conformidade com os requisitos de regulamentação. Além disso, este módulo expande a sua capacidade para gerir a autorização da sua organização, concedendo aos utilizadores as informações necessárias analisar a associação de suas funções. Os relatórios podem ter restringido a vistas que certifique-se de que os utilizadores que criam relatórios são apresentados apenas as informações estão autorizados a ver.

## <a name="bhold-reporting-installation-requirements"></a>Requisitos de instalação do BHOLD Reporting

Antes de instalar o módulo do BHOLD Reporting, tem de instalar o módulo do BHOLD Core no servidor no qual planeia instalar o módulo do BHOLD Reporting. Para obter informações sobre como instalar o módulo do BHOLD Core, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

> [!IMPORTANT]
> Se estiver a instalar o BHOLD Reporting e BHOLD Attestation, tem de instalar BHOLD Reporting antes da instalação do BHOLD Attestation.

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a instalar o módulo do BHOLD Reporting, precisa estar preparado para fornecer as informações que o Assistente de configuração do BHOLD Reporting necessita para concluir a instalação. A seguinte planilha ajudará a gravar essa informação para que estará pronto para fornecer quando for necessário.

| **Item**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilize um fornecedor de segurança num computador/domínio** | Quando selecionada, especifica que a segurança de serviços de domínio do Active Directory irá controlar o acesso a BHOLD Core.                                                                                                                | Selecione a caixa de verificação. </br>**Importante:** a instalação falhará se esta caixa de verificação não estiver selecionada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que criou durante a instalação do BHOLD Core. Para obter mais informações, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome apenas se estiver incorreto. **Importante:** especifique o nome de domínio com o nome NetBIOS do (curto), não o nome de domínio completamente qualificado (FQDN). Por exemplo, se o FQDN do domínio fabrikam.com, especifique o nome de domínio como a FABRIKAM. |
| **User**                                    | Especifica o nome de início de sessão da conta de utilizador de serviço do BHOLD Core.                                                                                                                                                          | Escreva o nome da conta de utilizador aqui:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                                | Especifica a palavra-passe da conta de utilizador de serviço.                                                                                                                                                                       | Escreva a palavra-passe aqui: </br>**Importante:** Certifique-se de que tenha esta palavra-passe num local seguro oculto.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>Instalação do BHOLD Reporting

Para instalar o módulo do BHOLD Reporting, inicie sessão como membro do grupo Admins do domínio, transfira o ficheiro seguinte e executá-lo como administrador no servidor que pretende instalar o módulo de relatórios do BHOLD em:

- BholdReporting<em>\<versão\></em>\_Release.msi

Substitua *\<versão\>* com o número de versão da versão do BHOLD Reporting que está a instalar.

Para executar o ficheiro de programa como administrador, o ficheiro com o botão direito e, em seguida, clique em **executar como administrador**.

## <a name="next-steps"></a>Passos Seguintes

- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)