---
title: Instalação do BHOLD model generator | Documentos da Microsoft
description: BHOLD modelo permite aos dados da estrutura de várias origens
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: ddb49219b5f68ff060f9b15a9ab64cb85a035d98
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333484"
---
# <a name="bhold-model-generator-installation"></a>Instalação do BHOLD Model Generator

Ao utilizar o módulo do BHOLD Model Generator, é possível estruturar dados de origens autoritativos que contém informações organizacionais, juntamente com listas de controlo de acesso (ACLs) num modelo que pode ser utilizado na administração BHOLD e do utilizador.

## <a name="bhold-model-generator-installation-requirements"></a>Requisitos de instalação do BHOLD Model Generator 

Antes de instalar o módulo do BHOLD Model Generator, tem de instalar o seguinte:

1. Módulo do BHOLD Core no servidor no qual planeia instalar o módulo do BHOLD Model Generator. Para obter informações sobre como instalar o módulo do BHOLD Core, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

2. O Microsoft OLE DB Provider para Microsoft Jet tem de ser instalado. Para obter mais informações, consulte [este artigo](http://support.microsoft.com/kb/271908).

> [!WARNING]
> Não instale o gerador de modelos do BHOLD na sua rede de produção. Gerador de modelos do BHOLD destina-se para uso offline num ambiente de teste para criar um modelo normalizado de função que pode ser importado para o modelo de função de empresa. Executar o gerador de modelo do BHOLD na sua rede de produção pode resultar na perda de seu modelo de função existente.

## <a name="before-you-begin"></a>Antes de começar

Antes de instalar o módulo do BHOLD Model Generator, precisa estar preparado para fornecer as informações que o Assistente de configuração do BHOLD Model Generator necessita para concluir a instalação. A seguinte planilha ajudará a gravar essa informação para que estará pronto para fornecer quando for necessário. Também precisará certificar-se

Motor de 2010 redistribuível de base de dados do Microsoft Access

 

*De \<*<http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems>*\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Definições da conta**

| **Item**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilize um fornecedor de segurança num computador/domínio** | Quando selecionada, especifica que a segurança de serviços de domínio do Active Directory irá controlar o acesso a BHOLD Core.                                                                                                                | Selecione a caixa de verificação. **Importante:** a instalação falhará se esta caixa de verificação não estiver selecionada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que criou durante a instalação do BHOLD Core. Para obter mais informações, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome apenas se estiver incorreto. **Importante:** especifique o nome de domínio com o nome NetBIOS do (curto), não o nome de domínio completamente qualificado (FQDN). Por exemplo, se o FQDN do domínio fabrikam.com, especifique o nome de domínio como a FABRIKAM. |
| **User**                                    | Especifica o nome de início de sessão da conta de utilizador de serviço do BHOLD Core.                                                                                                                                                          | Escreva o nome da conta de utilizador aqui:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                                | Especifica a palavra-passe da conta de utilizador de serviço.                                                                                                                                                                       | Escrever a palavra-passe aqui: **importante:** Certifique-se de que tenha esta palavra-passe num local seguro oculto.                                                                                                                                                                                                                  |

**Definições de cópia de segurança da base de dados**

| Item                                        | Descrição                                                                                                                                                                                                                                                                                                                                                                                                                  | Valor                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilizar a segurança integrada**                 | Especifica que a autenticação do Windows é usada para acessar o banco de dados.                                                                                                                                                                                                                                                                                                                                                        | Selecione a caixa de verificação se a autenticação do Windows é utilizada para ligar ao SQL Server. Desmarque a caixa de verificação se for utilizada a autenticação do SQL Server. A base de dados deve ter sido criada antes de executar o BHOLD Core configuração se autenticação do SQL Server é utilizada. **Nota:** se é utilizada a autenticação do Windows, deve fazer logon com uma conta que tenha a função de servidor sysadmin no servidor de base de dados. **Importante:** utilize autenticação do SQL Server apenas em ambientes de teste. A Microsoft recomenda utilizar a autenticação do Windows em implementações de produção. |
| **Utilizador de base de dados** e **palavra-passe de base de dados** | Especifica o nome de utilizador e palavra-passe de um utilizador com a função de servidor sysadmin no servidor de base de dados. Estes valores são fornecidos apenas quando é utilizada a autenticação do SQL Server.                                                                                                                                                                                                                                                  | Escrever o nome de utilizador do SQL Server: escrever a senha de usuário do SQL Server: </br></br> **Importante:** Certifique-se de que tenha esta palavra-passe num local seguro oculto.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Servidor de base de dados** e **nome de base de dados**   | Especifica o nome NetBIOS do servidor de base de dados e o nome da base de dados cópia de segurança que criará o programa de configuração do BHOLD Model Generator. Se não estiver a utilizar a instância de servidor de base de dados predefinida, especifique a instância de servidor de base de dados no formato  *\<server\>*\\*\<instância\>* .  A Microsoft recomenda que atribua um nome da base de dados de cópia de segurança usando o nome da base de dados do BHOLD Core seguido \_cópia de segurança, por exemplo B1_BACKUP. | Escrita do servidor (ou servidor e instância) nome aqui: </br> Escreva aqui o nome da base de dados:

## <a name="bhold-model-generator-setup"></a>Instalação do BHOLD Model Generator

Para instalar o módulo do BHOLD Model Generator, inicie sessão como membro do grupo Admins do domínio, transfira o ficheiro seguinte e executá-lo como administrador no servidor que pretende instalar o módulo do BHOLD Core num:

- BholdModelGenerator  *\<versão\>*\_Release.msi

Substitua *\<versão\>* com o número de versão da versão do BHOLD Model Generator que está a instalar.

Para executar o ficheiro de programa como administrador, o ficheiro com o botão direito e, em seguida, clique em **executar como administrador**.

## <a name="next-steps"></a>Passos Seguintes

- Para obter informações sobre como criar ficheiros de entrada [referência técnica do Microsoft BHOLD Suite](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx)
- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)