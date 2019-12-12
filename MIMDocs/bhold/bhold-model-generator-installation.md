---
title: Instalação do gerador de modelos BHOLD | Microsoft Docs
description: O modelo BHOLD permite estruturar dados de várias fontes
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 3d2510d6ea604dd88e56436812ed8bc975bc5c2b
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "64516738"
---
# <a name="bhold-model-generator-installation"></a>Instalação do gerador de modelos BHOLD

Usando o módulo gerador de modelos BHOLD, você pode estruturar dados de fontes autoritativas que contenham informações de usuário e organizacionais junto com ACLs (listas de controle de acesso) em um modelo que possa ser usado na administração de BHOLD.

## <a name="bhold-model-generator-installation-requirements"></a>Requisitos de instalação do gerador de modelos BHOLD 

Antes de instalar o módulo gerador de modelos do BHOLD, você deve instalar o seguinte:

1. Módulo BHOLD Core no servidor no qual você planeja instalar o módulo gerador de modelos do BHOLD. Para obter informações sobre como instalar o módulo BHOLD Core, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

2. O Microsoft OLE DB Provider para Microsoft Jet deve estar instalado. Para obter mais informações, consulte [Este artigo](http://support.microsoft.com/kb/271908).

> [!WARNING]
> Não instale o gerador de modelo BHOLD em sua rede de produção. O gerador de modelo BHOLD destina-se a ser usado offline em um ambiente de preparo para criar um modelo de função normalizado que você pode importar para o modelo de função corporativa. Executar o gerador de modelo BHOLD em sua rede de produção pode resultar em perda de seu modelo de função existente.

## <a name="before-you-begin"></a>Antes de começar

Antes de instalar o módulo gerador de modelos do BHOLD, você precisa estar preparado para fornecer as informações que o assistente de configuração do gerador de modelos do BHOLD exige para concluir a instalação. A planilha a seguir o ajudará a registrar essas informações para que você fique pronto para fornecê-las quando necessário. Você também precisará garantir

Microsoft Access Mecanismo de Banco de Dados 2010 redistribuível

 

*De \<* <http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems> *\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Configurações da conta**

| **Item**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Usar o provedor de segurança em domínio/computador** | Quando selecionado, especifica que Active Directory Domain Services segurança controlará o acesso ao BHOLD Core.                                                                                                                | Selecione a caixa de verificação. **Importante:** A instalação falhará se essa caixa de seleção não estiver marcada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que você criou ao instalar o BHOLD Core. Para obter mais informações, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome somente se ele estiver incorreto. **Importante:** Especifique o nome de domínio usando o nome NetBIOS (curto), não o FQDN (nome de domínio totalmente qualificado). Por exemplo, se o FQDN do domínio for fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **User**                                    | Especifica o nome de logon da conta de usuário do serviço do BHOLD Core.                                                                                                                                                          | Escreva o nome da conta de usuário aqui:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                                | Especifica a senha da conta de usuário do serviço.                                                                                                                                                                       | Escreva a senha aqui: **importante:** Lembre-se de manter essa senha em um local oculto e seguro.                                                                                                                                                                                                                  |

**Configurações do banco de dados de backup**

| Item                                        | Description                                                                                                                                                                                                                                                                                                                                                                                                                  | Valor                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Usar segurança integrada**                 | Especifica que a autenticação do Windows é usada para acessar o banco de dados.                                                                                                                                                                                                                                                                                                                                                        | Marque a caixa de seleção se a autenticação do Windows for usada para se conectar ao SQL Server. Desmarque a caixa de seleção se SQL Server autenticação for usada. O banco de dados deve ter sido criado antes da execução da instalação do BHOLD Core se SQL Server autenticação for usada. **Observação:** Se a autenticação do Windows for usada, você deverá estar conectado com uma conta que tenha a função de servidor sysadmin no servidor de banco de dados. **Importante:** Use SQL Server autenticação somente em ambientes de teste. A Microsoft recomenda enfaticamente o uso da autenticação do Windows em implantações de produção. |
| **Usuário do banco de dados** e **senha do banco de dados** | Especifica o nome de usuário e a senha de um usuário com a função de servidor sysadmin no servidor de banco de dados. Esses valores são fornecidos somente quando SQL Server autenticação é usada.                                                                                                                                                                                                                                                  | Escreva o nome de usuário do SQL Server aqui: escreva a senha do usuário do SQL Server aqui: </br></br> **Importante:** Certifique-se de manter essa senha em um local oculto e seguro.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Servidor de banco de dados** e **nome do banco de dados**   | Especifica o nome NetBIOS do servidor de banco de dados e o nome do banco de dados de backup que será criado pela instalação do gerador de modelos BHOLD. Se você não estiver usando a instância padrão do servidor de banco de dados, especifique a instância do servidor de banco de dados no formulário *\<server\>* \\*instância\<\>* .  A Microsoft recomenda que você nomeie o banco de dados de backup usando o nome do banco de dados do BHOLD Core seguido por \_BACKUP, por exemplo B1_BACKUP. | Escreva o nome do servidor (ou servidor e instância) aqui: </br> Escreva o nome do banco de dados aqui:

## <a name="bhold-model-generator-setup"></a>Configuração do gerador de modelos BHOLD

Para instalar o módulo gerador de modelos do BHOLD, faça logon como um membro do grupo Admins. do domínio, baixe o seguinte arquivo e execute-o como administrador no servidor em que você pretende instalar o módulo BHOLD Core:

- BholdModelGenerator *\<versão\>* \_Release. msi

Substitua *\<versão\>* pelo número de versão da versão do gerador do modelo BHOLD que você está instalando.

Para executar o arquivo de programa como administrador, clique com o botão direito do mouse no arquivo e clique em **Executar como administrador**.

## <a name="next-steps"></a>Próximos passos

- Para obter informações sobre como criar arquivos de entrada, [referência técnica do Microsoft BHOLD Suite](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx)
- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
