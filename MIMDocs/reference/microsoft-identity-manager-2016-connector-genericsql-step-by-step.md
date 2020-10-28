---
title: Conector SQL genérico passo a passo ! Microsoft Docs
description: Este artigo está a passar por um sistema de RH simples passo a passo usando o Conector SQL Genérico.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 28c1cc60-24fd-4d0d-a36d-b4aba6de86e7
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 04/02/2018
ms.author: billmath
ms.openlocfilehash: 1343fc7604789985c219f6bbae526df72a0baa5b
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762186"
---
# <a name="generic-sql-connector-step-by-step"></a>Conector do SQL genérico passo a passo
Este tópico é um guia passo a passo. Cria uma base de dados de RH de amostra simples e utiliza-a para importar alguns utilizadores e a sua filiação no grupo.

## <a name="prepare-the-sample-database"></a>Preparar a base de dados de amostras
Num servidor que executa o SQL Server, executa o script SQL encontrado no [apêndice A](#appendix-a). Este script cria uma base de dados de amostras com o nome GSQLDEMO. O modelo de objeto para a base de dados criada parece esta imagem:  
![Modelo de objeto](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/objectmodel.png)

Crie também um utilizador que pretende utilizar para ligar à base de dados. Nesta passagem, o utilizador chama-se FABRIKAM\SQLUser e está localizado no domínio.

## <a name="create-the-odbc-connection-file"></a>Criar o ficheiro de ligação ODBC
O Conector SQL Genérico está a utilizar o ODBC para se ligar ao servidor remoto. Primeiro, temos de criar um ficheiro com a informação de ligação da ODBC.

1. Inicie o utilitário de gestão ODBC no seu servidor:  
   ![ODBC](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc.png)
2. Selecione o separador **Ficheiro DSN** . Clique **em Adicionar...** .  
   ![ODBC1](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc1.png)
3. O condutor fora de caixa funciona bem, por isso selecione-o e clique **em Next>** .  
   ![ODBC2](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc2.png)
4. Dê ao ficheiro um nome, como **GenéricoSQL.**  
   ![ODBC3](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc3.png)
5. Clique em **Concluir** .  
   ![ODBC4](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc4.png)
6. Hora de configurar a ligação. Dê à fonte de dados uma boa descrição e forneça o nome do servidor que executa o SQL Server.  
   ![ODBC5](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc5.png)
7. Selecione como autenticar com SQL. Neste caso, utilizamos a Autenticação do Windows.  
   ![ODBC6](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc6.png)
8. Fornecer o nome da base de dados da amostra, **GSQLDEMO** .  
   ![ODBC7](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc7.png)
9. Mantenha tudo padrão neste ecrã. Clique em **Concluir** .  
   ![ODBC8](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc8.png)
10. Para verificar se tudo está a funcionar como esperado, clique em **Test Data Source** .  
    ![ODBC9](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc9.png)
11. Certifique-se de que o teste é bem sucedido.  
    ![ODBC10](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc10.png)
12. O ficheiro de configuração ODBC deve agora ser visível no Ficheiro DSN.  
    ![ODBC11](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/odbc11.png)

Agora temos o ficheiro que precisamos e podemos começar a criar o Conector.

## <a name="create-the-generic-sql-connector"></a>Criar o Conector SQL Genérico
1. Na UI do Gestor de Serviço de Sincronização, selecione **Conectors** e **Crie** . Selecione **Generic SQL (Microsoft)** e dê-lhe um nome descritivo.  
   ![Conector1](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector1.png)
2. Encontre o ficheiro DSN que criou na secção anterior e faça o upload para o servidor. Forneça as credenciais para ligar à base de dados.  
   ![Conector2](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector2.png)
3. Nesta passagem, estamos a facilitar-nos a vida e a dizer que existem dois tipos de objetos, **Utilizador** e **Grupo.**
   ![Conector3](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector3.png)
4. Para encontrar os atributos, queremos que o Conector detete esses atributos olhando para a própria tabela. Uma **vez que os Utilizadores** são uma palavra reservada em SQL, precisamos fornenciá-la em parênteses quadrados [ ].  
   ![Conector4](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector4.png)
5. Hora de definir o atributo âncora e o atributo DN. Para **Utilizadores,** utilizamos a combinação dos dois atributos username e EmployeeID. Para **o grupo,** utilizamos o GroupName (não é realista na vida real, mas para este passo em diante funciona).
   ![Conector5](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector5.png)
6. Nem todos os tipos de atributos podem ser detetados numa base de dados SQL. O tipo de atributo de referência, em particular, não pode. Para o tipo de objeto de grupo, precisamos alterar o ProprietárioID e o MemberID para referência.  
   ![Conector6](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector6.png)
7. Os atributos que selecionamos como atributos de referência no passo anterior requerem o tipo de objeto a que estes valores são uma referência. No nosso caso, o tipo de objeto utilizador.  
   ![Conector7](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector7.png)
8. Na página De Parâmetros Globais, selecione **Watermark** como a estratégia delta. Também digite no formato data/hora **yyy-MM-dd HH:mm:mm:ss** .
   ![Conector8](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector8.png)
9. Na página **Configure Partitions and Hierarquias,** selecione ambos os tipos de objetos.
   ![Conector9](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/connector9.png)
10. Nos **tipos de objetos selecionados** e **atributos selecionados,** selecione ambos os tipos de objetos e todos os atributos. Na página **Configure Anchors,** clique em **Terminar** .

## <a name="create-run-profiles"></a>Criar Perfis de Execução
1. No UI do Gestor de Serviço de Sincronização, selecione **Conectors** e **Configure Run Profiles** . Clique em **Novo Perfil** . Começamos com **a Full Import.**  
   ![Runprofile1](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile1.png)
2. Selecione o tipo **de Importação Completa (Apenas em Palco)** .  
   ![Runprofile2](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile2.png)
3. Selecione a partição **OBJECT=Utilizador** .  
   ![Runprofile3](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile3.png)
4. Selecione **Tabela** e escreva **[UTILIZADORES]** . Desloque-se para a secção do tipo de objeto multi-valor e introduza os dados como na imagem seguinte. **Selecione Acabamento** para guardar o passo.  
   ![Runprofile4a](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile4a.png)  
   ![Runprofile4b](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile4b.png)  
5. Selecione **Novo Passo** . Desta vez, selecione **OBJECT=Group** . Na última página, utilize a configuração como na seguinte imagem. Clique em **Concluir** .  
   ![Runprofile5a](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile5a.png)  
   ![Runprofile5b](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/runprofile5b.png)  
6. Opcional: Se quiser, pode configurar perfis de execução adicionais. Para esta passagem, apenas é utilizada a Importação Completa.
7. Clique **em OK** para terminar a alteração de perfis de execução.

## <a name="add-some-test-data-and-test-the-import"></a>Adicione alguns dados de teste e teste a importação
Preencha alguns dados de teste na sua base de dados de amostras. Quando estiver pronto, selecione **Run** e **Full import** .

Aqui está um utilizador com dois números de telefone e um grupo com alguns membros.  
![cs1](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/cs1.png)  
![cs2](./media/microsoft-identity-manager-2016-connector-genericsql-step-by-step/cs2.png)  

## <a name="appendix-a"></a>Anexo A
**Script SQL para criar a base de dados de amostras**

```SQL
---Creating the Database---------
Create Database GSQLDEMO
Go
-------Using the Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
