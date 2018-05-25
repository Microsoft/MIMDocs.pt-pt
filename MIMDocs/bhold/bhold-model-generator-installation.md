---
title: Instalação de gerador de modelo do BHOLD | Microsoft Docs
description: Modelo do BHOLD permite-lhe aos dados da estrutura de várias origens
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 3d45d18042ccda83873aa929101222c15f36246a
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/25/2018
---
# <a name="bhold-model-generator-installation"></a>Instalação de gerador de modelo do BHOLD

Ao utilizar o módulo de gerador de modelos do BHOLD, pode estrutura dados a partir de origens autoritativos que contém informações organizacionais, juntamente com listas de controlo de acesso (ACL) para um modelo que pode ser utilizado na administração do BHOLD e do utilizador.

## <a name="bhold-model-generator-installation-requirements"></a>Requisitos de instalação do gerador de modelos do BHOLD 

Antes de instalar o módulo de gerador de modelos do BHOLD, tem de instalar o seguinte:

1. Módulo de núcleo do BHOLD no servidor no qual planeia instalar o módulo de gerador de modelos do BHOLD. Para obter informações sobre como instalar o módulo do BHOLD Core, consulte [BHOLD Core instalação](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).

2. O Microsoft OLE DB Provider para Microsoft Jet tem de estar instalado. Para obter mais informações consulte [neste artigo](http://support.microsoft.com/kb/271908).

>[!WARNING]
Não instale o gerador de modelos do BHOLD na sua rede de produção. Gerador de modelos do BHOLD destina-se a ser utilizado offline num ambiente de teste, para criar um modelo normalizado de função que pode importar para o modelo de função da empresa. Executa o gerador de modelos do BHOLD na sua rede de produção pode resultar na perda do seu modelo de função existente.

## <a name="before-you-begin"></a>Antes de começar

Antes de instalar o módulo de gerador de modelos do BHOLD, tem de estar preparado para fornecer as informações que o Assistente de configuração de gerador de modelo do BHOLD necessita para concluir a instalação. A folha de cálculo seguinte irão ajudá-lo, registe essa informação para estará pronto para fornecê-lo quando for necessário. Também terá de certificar-se

Base de dados do Microsoft Access motor 2010 Redistributable

 

*Do \<*<http://daipvstf:8080/tfs/ActiveDirectory/IAM/_workitems>*\>*

 

<https://www.microsoft.com/en-us/download/confirmation.aspx?id=13255>

**Definições da conta**

| **Item**                                    | **Descrição**                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilize um fornecedor de segurança no computador/domínio** | Quando selecionada, especifica que a segurança dos serviços de domínio do Active Directory controlará o acesso para BHOLD núcleo.                                                                                                                | Selecione a caixa de verificação. **Importante:** a instalação falhará se esta caixa de verificação não estiver selecionada.                                                                                                                                                                                                                   |
| **Domínio**                                  | Especifica o domínio que contém a conta de serviço que criou ao instalar BHOLD Core. Para obter mais informações, consulte [BHOLD Core instalação](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome apenas se estiver incorreto. **Importante:** especifique o nome de domínio com o nome de NetBIOS (abreviado), não o nome de domínio completamente qualificado (FQDN). Por exemplo, se o FQDN do domínio for fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **User**                                    | Especifica o nome de início de sessão da conta de utilizador do serviço de núcleo do BHOLD.                                                                                                                                                          | Escreva o nome da conta de utilizador aqui:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                                | Especifica a palavra-passe da conta de utilizador de serviço.                                                                                                                                                                       | Escrever a palavra-passe aqui: **importante:** não se esqueça de manter esta palavra-passe numa localização segura, oculta.                                                                                                                                                                                                                  |

**Definições de cópia de segurança da base de dados**

| Item                                        | Descrição                                                                                                                                                                                                                                                                                                                                                                                                                  | Valor                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilizar a segurança integrada**                 | Especifica que a autenticação Windows é utilizada para aceder à base de dados.                                                                                                                                                                                                                                                                                                                                                        | Selecione a caixa de verificação se a autenticação Windows é utilizada para ligar ao SQL Server. Desmarque a caixa de verificação se for utilizada a autenticação do SQL Server. A base de dados deve ter sido criado antes de executar BHOLD Core configuração se autenticação do SQL Server é utilizada. **Nota:** se for utilizada a autenticação do Windows, deve ter sessão iniciada com uma conta que tenha a função de servidor sysadmin no servidor de base de dados. **Importante:** utilizar autenticação do SQL Server apenas em ambientes de teste. A Microsoft recomenda vivamente através da autenticação do Windows em implementações de produção. |
| **Base de dados de utilizador** e **palavra-passe de base de dados** | Especifica o nome de utilizador e palavra-passe de um utilizador com a função de servidor sysadmin no servidor de base de dados. Estes valores são fornecidos apenas quando é utilizada a autenticação do SQL Server.                                                                                                                                                                                                                                                  | Escrever o nome de utilizador do SQL Server: escrever o SQL Server utilizador palavra-passe aqui: </br></br> **Importante:** não se esqueça de manter esta palavra-passe numa localização segura, oculta.                                                                                                                                                                                                                                                                                                                                                                                                           |
| **Servidor de base de dados** e **nome de base de dados**   | Especifica o nome NetBIOS do servidor de base de dados e o nome da cópia de segurança base de dados que a configuração de gerador do BHOLD modelo irá criar. Se não estiver a utilizar a instância de servidor de base de dados predefinida, especifique a instância de servidor de base de dados no formato  *\<servidor\>*\\*\<instância\>* .  A Microsoft recomenda que atribua um nome da base de dados de cópia de segurança utilizando o nome da base de dados principal do BHOLD seguido \_cópia de segurança, por exemplo B1_BACKUP. | Escrita de nome de servidor (ou servidor e instância) aqui: </br> Escreva o nome de base de dados aqui:

## <a name="bhold-model-generator-setup"></a>Configuração do gerador de modelos do BHOLD

Para instalar o módulo de gerador de modelos do BHOLD, inicie sessão como membro do grupo Admins do domínio, transfira o ficheiro seguinte e executá-la como administrador no servidor que tenciona instalar o módulo de núcleo do BHOLD em:

- BholdModelGenerator  *\<versão\>*\_Release.msi

Substitua *\<versão\>* com o número de versão da versão do gerador de modelos do BHOLD que está a instalar.

Para executar o ficheiro de programa como administrador, clique com o botão direito do ficheiro e, em seguida, clique em **executar como administrador**.

## <a name="next-steps"></a>Próximos passos

- Para obter informações sobre como criar ficheiros de entrada [referência técnica do Microsoft BHOLD Suite](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx)
- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)