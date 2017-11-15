---
title: "Relatórios de instalação do BHOLD | Microsoft Docs"
description: "Módulo de relatórios do BHOLD permite-lhe gerar relatórios sobre as funções e as políticas de autorização"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: aa6a263daadc4abdcad0eaaba554b6bc739fbd5f
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2017
---
# <a name="bhold-reporting-installation"></a>Relatórios de instalação do BHOLD

O módulo de relatórios do BHOLD dá-lhe a capacidade para gerar relatórios sobre as funções e de outras políticas de autorização de BHOLD. Estes relatórios, muitas vezes, são úteis para auditoria ou para demonstrar a conformidade para os requisitos de regulamentação. Além disso, este módulo expande a capacidade de gerir a autorização da sua organização, concedendo aos utilizadores as informações que precisam para analisar a associação das respetivas funções. Os relatórios podem ter restringido a vistas que certifique-se de que os utilizadores que criam relatórios são apresentados apenas as informações estão autorizados a ver.

## <a name="bhold-reporting-installation-requirements"></a>Requisitos de instalação de relatórios do BHOLD

Antes de instalar o módulo de relatórios do BHOLD, tem de instalar o módulo de núcleo do BHOLD no servidor no qual planeia instalar o módulo de relatórios do BHOLD. Para obter informações sobre como instalar o módulo do BHOLD Core, consulte [BHOLD Core instalação](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx).

>[!IMPORTANT]
Se estiver a instalar o BHOLD Reporting e BHOLD atestado, tem de instalar relatórios BHOLD antes de instalar o atestado de BHOLD.

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a instalar o módulo de relatórios do BHOLD, tem de estar preparado para fornecer as informações que o Assistente de configuração de relatórios do BHOLD necessita para concluir a instalação. A folha de cálculo seguinte irão ajudá-lo, registe essa informação para estará pronto para fornecê-lo quando for necessário.

| **Item**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilize um fornecedor de segurança no computador/domínio** | Quando selecionada, especifica que a segurança dos serviços de domínio do Active Directory controlará o acesso para BHOLD núcleo.                                                                                                                | Selecione a caixa de verificação. </br>**Importante:** a instalação falhará se esta caixa de verificação não estiver selecionada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que criou ao instalar BHOLD Core. Para obter mais informações, consulte [BHOLD Core instalação](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome apenas se estiver incorreto. **Importante:** especifique o nome de domínio com o nome de NetBIOS (abreviado), não o nome de domínio completamente qualificado (FQDN). Por exemplo, se o FQDN do domínio for fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **User**                                    | Especifica o nome de início de sessão da conta de utilizador do serviço de núcleo do BHOLD.                                                                                                                                                          | Escreva o nome da conta de utilizador aqui:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                                | Especifica a palavra-passe da conta de utilizador de serviço.                                                                                                                                                                       | Escreva a palavra-passe aqui: </br>**Importante:** não se esqueça de manter esta palavra-passe numa localização segura, oculta.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>Instalação do Reporting do BHOLD

Para instalar o módulo de relatórios do BHOLD, inicie sessão como membro do grupo Admins do domínio, transfira o ficheiro seguinte e executá-la como administrador no servidor que tenciona instalar o módulo de relatórios do BHOLD em:

- BholdReporting*\<versão\>*\_Release.msi

Substitua  *\<versão\>*  com o número de versão da versão do BHOLD Reporting Services que está a instalar.

Para executar o ficheiro de programa como administrador, clique com o botão direito do ficheiro e, em seguida, clique em **executar como administrador**.

## <a name="next-steps"></a>Próximos passos

- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)