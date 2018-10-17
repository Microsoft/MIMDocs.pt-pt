---
title: Instalação do BHOLD attestation | Documentos da Microsoft
description: Módulo do BHOLD attestation permite-lhe designar os revisores e executam revisões
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: e4c3a6248585d55fddbbca3153f33734d7c5c429
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358113"
---
# <a name="bhold-attestation-installation"></a>Instalação do BHOLD attestation

O módulo do BHOLD Attestation permite-lhe designar os revisores e executam revisões periódicas das relações entre utilizadores e permissões por aplicação e contas.

## <a name="bhold-attestation-installation-requirements"></a>Requisitos de instalação do BHOLD Attestation

Antes de instalar o módulo do BHOLD Attestation, tem de instalar o módulo do BHOLD Core no servidor no qual planeia instalar o módulo de atestado do BHOLD. Para obter informações sobre como instalar o módulo do BHOLD Core, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Uma vez que a mensagem de e-mail de envios de contactos de módulo do BHOLD Attestation mensagens aos utilizadores, seu ambiente tem de ter um servidor de e-mail de Simple Mail Transfer Protocol (SMTP), como o Microsoft Exchange Server.

> [!IMPORTANT]
> Se estiver a instalar o BHOLD Reporting e BHOLD Attestation, tem de instalar BHOLD Reporting antes da instalação do BHOLD Attestation.

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a instalar o módulo do BHOLD Attestation, precisa estar preparado para fornecer as informações que o Assistente de configuração do BHOLD Attestation necessita para concluir a instalação. A seguinte planilha ajudará a gravar essa informação para que estará pronto para fornecer quando for necessário.

| **Item**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilize um fornecedor de segurança num computador/domínio** | Quando selecionada, especifica que a segurança de serviços de domínio do Active Directory irá controlar o acesso a BHOLD Core.                                                                                                                | Selecione a caixa de verificação. **Importante:** a instalação falhará se esta caixa de verificação não estiver selecionada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que criou durante a instalação do BHOLD Core. Para obter mais informações, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome apenas se estiver incorreto. **Importante:** especifique o nome de domínio com o nome NetBIOS do (curto), não o nome de domínio completamente qualificado (FQDN). Por exemplo, se o FQDN do domínio fabrikam.com, especifique o nome de domínio como a FABRIKAM. |
| **User**                                    | Especifica o nome de início de sessão da conta de utilizador de serviço do BHOLD Core.                                                                                                                                                          | Escreva o nome da conta de utilizador aqui:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                                | Especifica a palavra-passe da conta de utilizador de serviço.                                                                                                                                                                       | Escrever a palavra-passe aqui: **importante:** Certifique-se de que tenha esta palavra-passe num local seguro oculto.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>Instalação do BHOLD Attestation

Para instalar o módulo do BHOLD Attestation, inicie sessão como membro do grupo Admins do domínio, transfira o ficheiro seguinte e executá-lo como administrador no servidor que pretende instalar o módulo do BHOLD Attestation em:

- BholdAttestation<em>\<versão\></em>\_Release.msi

Substitua *\<versão\>* com o número de versão da versão do BHOLD Attestation que está a instalar.

Para executar o ficheiro de programa como administrador, o ficheiro com o botão direito e, em seguida, clique em **executar como administrador**.

## <a name="next-steps"></a>Passos Seguintes

- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
