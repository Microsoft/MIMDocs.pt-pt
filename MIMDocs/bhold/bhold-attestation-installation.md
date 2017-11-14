---
title: "Instalação de atestado do BHOLD | Microsoft Docs"
description: "Módulo de atestado do BHOLD permite-lhe designar revisores e efetuar as revisões"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 93d0b9a17d82911b71b1b220465b6d637687444b
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/15/2017
---
# <a name="bhold-attestation-installation"></a>Instalação de atestado do BHOLD

O módulo de atestado do BHOLD permite-lhe designar revisores e efetuar revisões periódicas das relações entre utilizadores e permissões por aplicação e contas.

## <a name="bhold-attestation-installation-requirements"></a>Requisitos de instalação de atestado do BHOLD

Antes de instalar o módulo de atestado do BHOLD, tem de instalar o módulo de núcleo do BHOLD no servidor no qual planeia instalar o módulo de atestado do BHOLD. Para obter informações sobre como instalar o módulo do BHOLD Core, consulte [BHOLD Core instalação](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). Porque o correio eletrónico contactos BHOLD atestado módulo envia mensagens aos utilizadores, o ambiente tem de ter um servidor de correio eletrónico de protocolo de transferência de correio simples (SMTP), como o Microsoft Exchange Server.

>[!IMPORTANT]
Se estiver a instalar o BHOLD Reporting e BHOLD atestado, tem de instalar relatórios BHOLD antes de instalar o atestado de BHOLD.

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a instalar o módulo de atestado do BHOLD, tem de estar preparado para fornecer as informações que o Assistente de configuração de atestado do BHOLD necessita para concluir a instalação. A folha de cálculo seguinte irão ajudá-lo, registe essa informação para estará pronto para fornecê-lo quando for necessário.

| **Item**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilize um fornecedor de segurança no computador/domínio** | Quando selecionada, especifica que a segurança dos serviços de domínio do Active Directory controlará o acesso para BHOLD núcleo.                                                                                                                | Selecione a caixa de verificação. **Importante:** a instalação falhará se esta caixa de verificação não estiver selecionada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que criou ao instalar BHOLD Core. Para obter mais informações, consulte [BHOLD Core instalação](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome apenas se estiver incorreto. **Importante:** especifique o nome de domínio com o nome de NetBIOS (abreviado), não o nome de domínio completamente qualificado (FQDN). Por exemplo, se o FQDN do domínio for fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **User**                                    | Especifica o nome de início de sessão da conta de utilizador do serviço de núcleo do BHOLD.                                                                                                                                                          | Escreva o nome da conta de utilizador aqui:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                                | Especifica a palavra-passe da conta de utilizador de serviço.                                                                                                                                                                       | Escrever a palavra-passe aqui: **importante:** não se esqueça de manter esta palavra-passe numa localização segura, oculta.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>Instalação de atestado do BHOLD

Para instalar o módulo de atestado do BHOLD, inicie sessão como membro do grupo Admins do domínio, transfira o ficheiro seguinte e executá-la como administrador no servidor que tenciona instalar o módulo de atestado do BHOLD em:

- BholdAttestation*\<versão\>*\_Release.msi

Substitua * \<versão\> * com o número de versão da versão do BHOLD atestado que está a instalar.

Para executar o ficheiro de programa como administrador, clique com o botão direito do ficheiro e, em seguida, clique em **executar como administrador**.

## <a name="next-steps"></a>Próximos passos

- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)