---
title: Instalação de atesta BHOLD Microsoft Docs
description: Módulo de atesta BHOLD permite-lhe designar revisores e executar avaliações
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 3d7e136ee86272429ff16a49b3c1319a4543648f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79042326"
---
# <a name="bhold-attestation-installation"></a>Instalação de atesta BHOLD

O módulo BHOLD Attestation permite-lhe designar revisores e realizar revisões recorrentes das relações entre utilizadores e permissões e contas por aplicação.

## <a name="bhold-attestation-installation-requirements"></a>Requisitos de instalação de atestado bHOLD

Antes de instalar o módulo BHOLD Attestation, tem de instalar o módulo BHOLD Core no servidor no qual planeia instalar o módulo BHOLD Attestation. Para obter informações sobre a instalação do módulo BHOLD Core, consulte a [Instalação Core BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Uma vez que os contactos do módulo BHOLD Attestation enviam mensagens de correio eletrónico aos utilizadores, o seu ambiente deve ter um servidor de e-mail simples (SMTP) de email, como o Microsoft Exchange Server.

> [!IMPORTANT]
> Se estiver a instalar tanto o BHOLD Reporting como o BHOLD Attestation, tem de instalar o BHOLD Reporting antes de instalar a Estação BHOLD.

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a instalar o módulo BHOLD Attestation, tem de estar preparado para fornecer a informação que o assistente de configuração da Estação DeAtatoa BHOLD necessita para completar a instalação. A seguinte folha de cálculo irá ajudá-lo a registar essa informação para que esteja pronto para fornecê-la quando for necessária.

| **Artigo**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilize o Fornecedor de Segurança no Domínio/Máquina** | Quando selecionado, especifica que a segurança dos Serviços de Domínio do Diretório Ativo controlará o acesso ao BHOLD Core.                                                                                                                | Selecione a caixa de verificação. **Importante:** A instalação falhará se esta caixa de verificação não for selecionada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que criou ao instalar o BHOLD Core. Para mais informações, consulte [a Instalação BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Mude o nome apenas se estiver incorreto. **Importante:** Especifique o nome de domínio utilizando o nome NetBIOS (curto) e não o nome de domínio totalmente qualificado (FQDN). Por exemplo, se o FQDN do domínio for fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **User**                                    | Especifica o nome de início de sessão da conta de utilizador do serviço BHOLD Core.                                                                                                                                                          | Escreva aqui o nome da conta de utilizador:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                                | Especifica a palavra-passe da conta de utilizador do serviço.                                                                                                                                                                       | Escreva a palavra-passe aqui: **Importante:** Certifique-se de manter esta palavra-passe num local escondido e seguro.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>Instalação de Attestation BHOLD

Para instalar o módulo BHOLD Attestation, inicie sessão como membro do grupo Domain Admins, descarregue o seguinte ficheiro e execute-o como administrador no servidor que pretende instalar o módulo BHOLD Attestation em:

- Versão\<de BholdAttestation<em>\></em>\_Release.msi

Substitua *\<Versão\>* com o número de versão da versão do lançamento da Estação BHOLD que está a instalar.

Para executar o ficheiro do programa como administrador, clique no ficheiro à direita e, em seguida, clique em **Executar como administrador**.

## <a name="next-steps"></a>Próximos passos

- [Guia de instalação BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
