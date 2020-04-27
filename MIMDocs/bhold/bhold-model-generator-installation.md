---
title: Instalação do gerador de modelos BHOLD [ Microsoft Docs
description: O modelo BHOLD permite-lhe estruturar dados de várias fontes
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 6f7e0979246eb2124604f594c57b40ec11cc7140
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042021"
---
# <a name="bhold-model-generator-installation"></a>Instalação do gerador de modelobhold

Utilizando o módulo BHOLD Model Generator, pode estruturar dados de fontes autoritárias que contenham informações do utilizador e da organização, juntamente com as listas de controlo de acesso (ACLs) num modelo que pode ser utilizado na administração do BHOLD.

## <a name="bhold-model-generator-installation-requirements"></a>Requisitos de instalação do gerador de modelobhold 

Antes de instalar o módulo Gerador de Modelos BHOLD, tem de instalar o seguinte:

1. Módulo BHOLD Core no servidor no qual planeia instalar o módulo Gerador de Modelos BHOLD. Para obter informações sobre a instalação do módulo BHOLD Core, consulte a [Instalação Core BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

2. O Microsoft OLE DB Provider para o Microsoft Jet deve ser instalado. Para mais informações consulte [este artigo.](https://support.microsoft.com/kb/271908)

> [!WARNING]
> Não instale o Gerador de Modelos BHOLD na sua rede de produção. O BHOLD Model Generator destina-se a ser utilizado offline num ambiente de encenação para criar um modelo de função normalizado que pode importar para o seu modelo de papel empresarial. Executar o Gerador de Modelos BHOLD na sua rede de produção pode resultar na perda do seu modelo de papel existente.

## <a name="before-you-begin"></a>Antes de começar

Antes de instalar o módulo Gerador de Modelos BHOLD, tem de estar preparado para fornecer a informação de que o assistente de configuração do gerador do modelo BHOLD necessita para completar a instalação. A seguinte folha de cálculo irá ajudá-lo a registar essa informação para que esteja pronto para fornecê-la quando for necessária. Também terá de se certificar

Microsoft Access Database Engine 2010 Redistribuável

 

*De\<*<http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems>*\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Definições da conta**

| **Item**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilize o Fornecedor de Segurança no Domínio/Máquina** | Quando selecionado, especifica que a segurança dos Serviços de Domínio do Diretório Ativo controlará o acesso ao BHOLD Core.                                                                                                                | Selecione a caixa de verificação. **Importante:** A instalação falhará se esta caixa de verificação não for selecionada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que criou ao instalar o BHOLD Core. Para mais informações, consulte [a Instalação BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Mude o nome apenas se estiver incorreto. **Importante:** Especifique o nome de domínio utilizando o nome NetBIOS (curto) e não o nome de domínio totalmente qualificado (FQDN). Por exemplo, se o FQDN do domínio for fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **Utilizador**                                    | Especifica o nome de início de sessão da conta de utilizador do serviço BHOLD Core.                                                                                                                                                          | Escreva aqui o nome da conta de utilizador:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                                | Especifica a palavra-passe da conta de utilizador do serviço.                                                                                                                                                                       | Escreva a palavra-passe aqui: **Importante:** Certifique-se de manter esta palavra-passe num local escondido e seguro.                                                                                                                                                                                                                  |

**Definições de base de dados de backup**

| Item                                        | Descrição                                                                                                                                                                                                                                                                                                                                                                                                                  | Valor                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilizar segurança integrada**                 | Especifica que a Autenticação do Windows é utilizada para aceder à base de dados.                                                                                                                                                                                                                                                                                                                                                        | Selecione a caixa de verificação se a autenticação do Windows for utilizada para se ligar ao Servidor SQL. Limpe a caixa de verificação se for utilizada a autenticação do servidor SQL. A base de dados deve ter sido criada antes de executar a configuração do núcleo BHOLD se for utilizada a autenticação do servidor SQL. **Nota:** Se for utilizada a Autenticação do Windows, deve iniciar-se o seu login com uma conta que tenha a função de servidor de sisadmina no servidor de base de dados. **Importante:** Utilize a autenticação do servidor SQL apenas em ambientes de teste. A Microsoft recomenda vivamente a utilização da Autenticação do Windows em implementações de produção. |
| **Palavra-passe** **do utilizador** e da base de dados | Especifica o nome de utilizador e a palavra-passe de um utilizador com a função do servidor sysadmin no servidor de base de dados. Estes valores só são fornecidos quando for utilizada a autenticação do servidor SQL.                                                                                                                                                                                                                                                  | Escreva aqui o nome de utilizador do SQL Server: Escreva a palavra-passe do utilizador do Servidor SQL aqui: </br></br> **Importante:** Certifique-se de manter esta senha num local escondido e seguro.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Nome do servidor** de base de dados e **da base de dados**   | Especifica o nome NetBIOS do servidor de base de dados e o nome da base de dados de backup que a Configuração do Gerador do Modelo BHOLD criará. Se não estiver a utilizar a instância de servidor de base de dados predefinida, especifique a instância do servidor de base de dados na\\*\<instância\>* do * \<servidor\>* de formulários .  A Microsoft recomenda que nomeie a base de dados \_de backup utilizando o nome da base de dados BHOLD Core seguida de BACKUP, por exemplo, B1_BACKUP. | Escreva o nome do servidor (ou servidor e instância) aqui: </br> Escreva aqui o nome da base de dados:

## <a name="bhold-model-generator-setup"></a>Configuração do gerador de modelo BHOLD

Para instalar o módulo BHOLD Model Generator, inicie sessão como membro do grupo Domain Admins, descarregue o seguinte ficheiro e execute-o como administrador no servidor que pretende instalar o módulo BHOLD Core em:

- Versão BholdModelGenerator * \<Versão\>*\_Versão.msi

Substitua * \<\> * a Versão pelo número de versão do lançamento do Gerador do Modelo BHOLD que está a instalar.

Para executar o ficheiro do programa como administrador, clique no ficheiro à direita e, em seguida, clique em **Executar como administrador**.

## <a name="next-steps"></a>Passos seguintes

- Para obter informações sobre como criar ficheiros de entrada [Microsoft BHOLD Suite Referência Técnica](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx)
- [Guia de instalação BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
