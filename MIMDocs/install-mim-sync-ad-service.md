---
title: Utilizar a Sincronização do Microsoft Identity Manager com o AD | Documentos da Microsoft
description: Utilize agentes de gestão e o Serviço de Sincronização do MIM para sincronizar as bases de dados do Active Directory e do MIM.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/12/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 5e532b67-64a6-4af6-a806-980a6c11a82d
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 81cf34959ccdea5ad9eb463f85a25d26bc1d8ede
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042428"
---
# <a name="install-mim-2016-synchronize-active-directory-and-mim-service"></a>Instalar o MIM 2016: Sincronizar o Active Directory e o Serviço MIM

> [!div class="step-by-step"]
> [« Portal e Serviço do MIM](install-mim-service-portal.md)
> 
> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio – **nomedoservidormim**
> - Nome de domínio – **contoso**
> - Senha -<strong>Pass@word1</strong>

Por predefinição, o Serviço de Sincronização do MIM (Sincronizar) não tem conetores configurados.  O primeiro passo típico consiste em utilizar a Sincronização do MIM para povoar a base de dados do Serviço MIM com contas do Active Directory existentes. Para tal, deverá utilizar a aplicação Serviço de Sincronização do MIM.

## <a name="create-the-mim-management-agent"></a>Criar o agente de gestão do MIM
O agente de gestão (MA) do MIM é um conetor da Sincronização do MIM para o Serviço MIM. Para criar este conetor, utilize o assistente Criar Agente de Gestão.

Quando configura um agente de gestão do MIM, tem de especificar uma conta de utilizador. Este documento utiliza **MIMMA** como o nome desta conta.

> [!NOTE]
> A conta que utiliza para o agente de gestão do MIM tem de ser a mesma conta que especificou durante a instalação do Serviço MIM. Se planeia ativar a funcionalidade 'Use mimSync', o Serviço de Sincronização MIM deve ser instalado utilizando a Conta de Serviço Gerida pelo Grupo.

### <a name="to-create-the-mim-ma"></a>Para criar o MA do MIM

1.  Abra o Synchronization Service Manager.

2.  Para abrir o assistente do Agente de Gestão Criar, mude para a página agentes de **gestão** e, em seguida, no menu **Ações,** clique em **Criar**.

3.  Na página Do Agente de **Gestão Criar,** forneça as seguintes definições e, em seguida, clique em **Seguinte**.

    -   Agente de gestão para: agente de gestão do Serviço FIM

    -   Nome: MIMMA

4.  Na página **Ligar à Base de Dados**, forneça as seguintes definições e, em seguida, clique em **Seguinte**
    -   Servidor: localhost

    -   Base de dados: FIMService

    -   Endereço de base do serviço MIM:http://localhost:5725

    -   Modo de autenticação: autenticação integrada do Windows

    -   Nome de utilizador: mimma

    -   Palavra-passe: Pass@word

    -   Domínio: contoso

5.  Na página **Tipos de Objetos Selecionados**, verifique se os tipos de objetos que estão listados abaixo estão selecionados e, em seguida, clique em **Seguinte**

    -   EntradaDeRegraDetetada

    -   EntradaDeRegraEsperada

    -   Grupo

    -   Pessoa

    -   RegraDeSincronização

6.  Na página **Atributos Selecionados**, selecione **Mostrar Tudo**, confirme que todos os atributos listados estão selecionados e, em seguida, clique em **Seguinte**.

7.  Na página **Configurar Filtro de Conetor**, clique em **Seguinte**.

8.  Na página **Configurar Mapeamentos de Tipos de Objeto**, adicione o seguinte mapeamento e, em seguida, clique em **Seguinte**

    - Selecione **Pessoa** na lista **Tipo do Objeto de Origem de Dados**.
    - Clique em **Adicionar Mapeamento** para abrir a caixa de diálogo Mapeamento.
    - Selecione **pessoa** na lista **Tipo de objeto metaverso**.
    - Clique em **OK** para fechar a caixa de diálogo Mapeamento.
    - Selecione **Grupo** na lista **Tipo de Objeto da Origem de Dados**.
    - Clique em **Adicionar Mapeamento** para abrir a caixa de diálogo Mapeamento.
    - Selecione **grupo** na lista **Tipo de objeto metaverso**.
    - Clique em **OK** para fechar a caixa de diálogo Mapeamento.

9.  Na página **Configurar Fluxo de Atributos**, crie mapeamentos de fluxo de atributos, tal como mostrado abaixo e, em seguida, clique em **Seguinte**

    -   Selecione **Pessoa** como o tipo de objeto de Origem de dados e de Metaverso.

    -   Selecione **Direto** como o Tipo de Mapeamento.

    -   Para cada linha na tabela seguinte, conclua estes passos:

        -   Selecione a **direção do Fluxo** mostrado para essa linha na tabela.

        -   Selecione o **atributo de Origem de dados** mostrado para essa linha na tabela.

        -   Selecione o **atributo do Metaverso** mostrado para essa linha na tabela.

        -   Para aplicar o mapeamento de fluxo, clique em **Novo**.

    | **Atributo de Origem de Dados** | **Direção do Fluxo** | **Atributo Metaverso** |
    |-|-|-|
    | AccountName | Exportar | accountName |
    | DisplayName | Exportar | displayName |
    | Domain | Exportar | domínio |
    | Email | Exportar | correio |
    | EmployeeID | Exportar | IDdefuncionário |
    | TipoDeFuncionário | Exportar | tipodeFuncionário |
    | FirstName | Exportar | nomePróprio |
    | LastName | Exportar | apelido |
    | SIDobjeto | Exportar | sidObjeto |

    -   Selecione **Grupo** como o tipo de objeto de Origem de dados e de Metaverso.

    -   Selecione **Direto** como o Tipo de Mapeamento.

    -   Para cada linha na tabela seguinte, conclua estes passos:

        -   Selecione a **direção do Fluxo** mostrado para essa linha na tabela.

        -   Selecione o **atributo de Origem de dados** mostrado para essa linha na tabela.

        -   Selecione o **atributo do Metaverso** mostrado para essa linha na tabela.

        -   Para aplicar o mapeamento de fluxo, clique em **Novo**.

    | **Atributo de Origem de Dados** | **Direção do Fluxo** | **Atributo Metaverso** |
    |-|-|-|
    | AccountName | Exportar | accountName |
    | DisplayName | Exportar | displayName |
    | Domain | Exportar | domínio |
    | Email | Exportar | correio |
    | MailNickName | Exportar | mailNickName |
    | Membro | Exportar | membro |
    | SIDobjeto | Exportar | sidObjeto |
    | Âmbito | Exportar | scope |
    | Tipo | Exportar | tipo |
    | MembershipAddWorkflow | Exportar | membershipAddWorkflow |
    | MembershipLocked | Exportar | membershipLocked |
    | AccountName | Importar | accountName |
    | DisplayedOwner | Importar | displayedOwner |
    | DisplayName | Importar | displayName |
    | MailNickName | Importar | mailNickName |
    | Membro | Importar | membro |
    | Âmbito | Importar | scope |
    | Tipo | Importar | tipo |

10.  Na página **Configurar Desaprovisionamento**, clique em **Seguinte**

11.  Para criar o agente de gestão, na página **Configurar Extensões**, clique em **Concluir**.

## <a name="create-the-ad-management-agent"></a>Criar o agente de gestão do AD
O agente de gestão do Active Directory é um conetor para os Serviços de Domínio do AD. Para criar este conetor, utilize o assistente Criar Agente de Gestão.

1. Para abrir o assistente Criar Agente de Gestão, no menu **Ações**, clique em **Criar**.

2. Na página **Criar Agente de Gestão**, forneça as seguintes definições e, em seguida, clique em **Seguinte**:

    - Agente de gestão para: Serviços de Domínio do Active Directory
    - Nome: ADMA

3. Na página **Ligar à Floresta do Active Directory**, forneça as seguintes definições e, em seguida, clique em **Seguinte**:

    - Nome da floresta: contoso.local
    - Nome de utilizador: administrador
    - Palavra-passe: &lt;a palavra-passe da conta&gt;
    - Domínio: contoso

4. Na página **Configurar Partições de Diretório**, forneça as seguintes definições e, em seguida, clique em **Seguinte**:

    - Na lista **Selecionar partições de diretório**, selecione **DC=CONTOSO, DC=local**.

    - Para abrir a caixa de diálogo Selecionar Contentores, clique em **Contentores**.

    - Se pretender alterar o contentor para que apenas o MIM efetue a gestão de objetos num contentor específico, clique no nó **DC=CONTOSO,DC=local** e, em seguida, clique no nó do contentor pretendido.

    - Para fechar a caixa de diálogo Selecionar Contentores, clique em **OK**.

5. Na página **Configurar Hierarquia de Aprovisionamento**, clique em **Seguinte**.

6. Na página **Selecionar Tipos de Objeto**, forneça as seguintes definições e, em seguida, clique em **Seguinte**:

    - Na lista **Tipos de objeto**, selecione **utilizador** e **grupo**.

7. Na página **Selecionar Atributos**, selecione **Mostrar TUDO**, selecione os atributos seguintes e, em seguida, clique em **Seguinte**:

    -   empresa
    -   displayName
    -   IDdefuncionário
    -   tipodeFuncionário
    -   nomeDado
    -   tipodeGrupo
    -   managedBy
    -   gestor
    -   membro
    -   sidObjeto
    -   sAMAccountName
    -   sAMAccountType
    -   sn
    -   pwdUnicode
    -   controloContaUtilizador

8. Na página **Configurar Filtro de Conetor**, clique em **Seguinte**.

9. Na página **Configurar Regras de Associação e Projeção**, clique em **Seguinte**.

10. Na página **Configurar Fluxo de Atributos**, clique em **Seguinte**.

11. Na página de **Dprovisionamento configurar,** clique em **Seguinte**.

12. Na página **Configurar Extensões**, clique em **Concluir**.


## <a name="create-run-profiles"></a>Criar Perfis de Execução

Crie perfis de execução para os Conectores ADMA e MIMMA.

### <a name="create-run-profiles-for-the-adma-connector"></a>Criar perfis de execução para o conetor do ADMA

Esta tabela mostra os cinco perfis de execução que criará para o conetor do ADMA:

| Nome | Tipo |
| ---- | ---- |
| Perfil1 | Importação Completa (Apenas Fase) |
| Perfil2 | Sincronização Completa |
| Perfil3 | Importação Delta (Apenas Fase) |
| Perfil4 | Sincronização Delta |
| Perfil5 | Exportar |

Para criar perfis de execução para o conetor do ADMA:

1. Abra o Gestor de Serviços de Sincronização e no menu **Ferramentas,** clique em **Agentes de Gestão.**

2. Na lista **Agentes de Gestão**, selecione **ADMA**.

3. Para abrir a caixa de diálogo Configurar Perfis de Execução para, no menu **Ações**, clique em **Configurar Perfis de Execução**.

4. Para cada perfil de execução na tabela, conclua os seguintes passos:

    - Para abrir o assistente Configurar Perfil de Execução, clique em **Novo Perfil**.

    - Na caixa **Nome**, escreva o nome de perfil da tabela e clique em **Seguinte**.

    - Na lista **Tipo**, selecione o tipo de passo da tabela e, em seguida, clique em **Seguinte**.

    - Clique em **Concluir** para criar o perfil de execução.

5. Para fechar a caixa de diálogo Configurar Perfis de Execução, clique em **OK**.

### <a name="create-run-profiles-for-the-mimma-connector"></a>Criar perfis de execução para o conetor do MIMMA

Esta tabela mostra os cinco perfis de execução correspondentes para o conetor do MIMMA:

| Nome | Tipo |
| -------- | -------- |
| Perfil1 | Importação Completa (Apenas Fase) |
| Perfil2 | Sincronização Completa |
| Perfil3 | Importação Delta (Apenas Fase) |
| Perfil4 | Sincronização Delta |
| Perfil5 | Exportar |

Para criar perfis de execução para o conetor do MIMMA:

1. Abra o Gestor de Serviços de Sincronização e no menu **Ferramentas,** clique em **Agentes de Gestão.**

2. Na lista **Agentes de Gestão**, selecione **MIMMA**.

3. Para abrir a caixa de diálogo Configurar Perfis de Execução para, no menu **Ações**, clique em **Configurar Perfis de Execução**.

4. Para cada perfil de execução na tabela, conclua os seguintes passos:

    - Para abrir o assistente Configurar Perfil de Execução, clique em **Novo Perfil**.

    - Na caixa **Nome**, escreva o nome de perfil da tabela e clique em **Seguinte**.

    - Na lista **Tipo**, selecione o tipo de passo da tabela e, em seguida, clique em **Seguinte**.

    - Clique em **Concluir** para criar o perfil de execução.

5. Para fechar a caixa de diálogo Configurar Perfis de Execução, clique em **OK**.

## <a name="configure-the-mim-service"></a>Configurar o Serviço MIM

Através do Portal do MIM, irá criar a regra de sincronização de entrada de utilizadores do AD para o Serviço MIM.

Para criar a regra de sincronização de entrada de utilizadores do AD:

1. Na página inicial do Portal do MIM, na barra de navegação, clique em **Administração**.

2. Para abrir a página Regras de Sincronização, clique em **Regras de Sincronização**.

3. Para abrir o assistente Criar Regra de Sincronização, na barra de ferramentas, clique em **Novo**.

4. No separador **Geral**, forneça as seguintes informações e, em seguida, clique em **Seguinte**:

    -   Nome a Apresentar: Regra de Sincronização de Entrada de Utilizadores do AD
    -   Direção do Fluxo de Dados: Entrada

5. No separador **Âmbito**, forneça as seguintes informações e, em seguida, clique em **Seguinte**:

    -   Tipo de Recurso Metaverso: pessoa
    -   Sistema Externo: ADMA
    -   Tipo de Recurso de Sistema Externo: utilizador

6. No separador **Relação**, forneça as seguintes informações e, em seguida, clique em **Seguinte**:

    -   Para configurar os Critérios de Relação, selecione **SIDobjeto** na lista ObjetoMetaverso:pessoa(Atributo) e na lista ObjetoDeSistemaLigado:pessoa (Atributo).

    -   Selecione **Criar Recurso no FIM**.

7. Na página **Fluxo de Atributos de Entrada**, forneça as seguintes informações e, em seguida, clique em **Seguinte**:

    | Regra de Fluxo | Origem | Destino |
    |-|-|-|
    |Regra 1|NomeContaSam|accountName|
    |Regra 2|displayName|displayName|
    |Regra 3|TipoDeFuncionário|tipodeFuncionário|
    |Regra 4|nomeDado|nomePróprio|
    |Regra 5|sn|apelido|
    |Regra 6|Gestor|gestor|
    |Regra 7|SIDobjeto|SIDobjeto|
    |Regra 8|"Contoso"|domínio|

    Para cada linha nesta tabela, efetue os seguintes passos:

    - Para abrir a caixa de diálogo Definição de Fluxo, clique em **Novo Fluxo de Atributos**.

    - No separador **Origem**, selecione o atributo apresentado para essa linha na tabela.

    - No separador **Destino**, selecione o atributo apresentado para essa linha na tabela.

    - Para aplicar a configuração do fluxo de atributos, clique em **OK**.

8. No separador **Resumo**, clique em **Submeter**.

## <a name="initialize-the-testing-environment"></a>Inicializar o ambiente de teste
Existem quatro passos que precisa de realizar antes de testar a configuração do MIM com dados do AD:

### <a name="enable-provisioning"></a>Ativar Aprovisionamento

1. Abra o Synchronization Service Manager.

2. Para abrir a caixa de diálogo Opções, no menu **Ferramentas**, clique em **Opções**

3. Selecione **Ativar Aprovisionamento da Regra de Sincronização**.

4. Para fechar a caixa de diálogo Opções, clique em **OK**.

### <a name="initialize-the-mimma"></a>Inicializar o MIMMA

Execute um ciclo de sincronização completo neste conetor. O ciclo completo inclui os seguintes perfis de execução:

- Importação Completa
- Sincronização Completa
- Exportar
- Importação Delta

Siga estes passos para executar cada um dos quatro perfis de execução.

1. Abra o Synchronization Service Manager e, no menu **Ferramentas**, clique em **Agentes de Gestão**.

2. Na lista **Agentes de Gestão**, selecione **MIMMA**.

3. Para abrir a caixa de diálogo Executar Agente de Gestão, no menu **Ações**, clique em **Executar**.

4. Para cada perfil de execução indicado na tabela, conclua os seguintes passos:

    - Para abrir a caixa de diálogo Executar Agente de Gestão, no menu **Ações**, clique em **Executar**.

    - Na lista **Perfis de execução**, selecione o perfil de execução que pretende executar.

    - Para iniciar o perfil de execução, clique em **OK**.

#### <a name="configure-attribute-flow-precedence"></a>Configurar a precedência do fluxo de atributos

Durante a inicialização do conetor do MIM, as regras de sincronização configuradas foram importadas para o metaverso.

Ajuste a precedência do fluxo de atributos para os atributos contribuídos por este conetor para se certificar de que os atributos já no AD possam circular para o metaverso e, posteriormente, para a base de dados do Serviço MIM.

### <a name="initialize-the-adma"></a>Inicializar o ADMA

Para inicializar o conetor do Active Directory, terá de executar uma importação completa e uma sincronização completa no mesmo. A importação completa transfere os objetos existentes do AD para o espaço conetor. A sincronização completa atualiza as regras de sincronização para corresponderem às do conetor do MIM.

1. Abra o Synchronization Service Manager e, no menu **Ferramentas**, clique em **Agentes de Gestão**.

2. Na lista **Agentes de Gestão**, selecione **ADMA**.

3. Para abrir a caixa de diálogo Executar Agente de Gestão, no menu **Ações**, clique em **Executar**.

4. Para cada perfil de execução indicado na tabela, conclua os seguintes passos:

    - Para abrir a caixa de diálogo Executar Agente de Gestão, no menu **Ações**, clique em **Executar**.

    - Na lista **Perfis de execução**, selecione o perfil de execução que pretende executar.

    - Para iniciar o perfil de execução, clique em **OK**.

### <a name="populate-the-mim-service-database"></a>Povoar a base de dados do Serviço MIM

Para povoar a base de dados do Serviço MIM com os objetos, tem de executar um ciclo de sincronização no conetor do MIMMA. O ciclo de consiste em:

- Exportar
- Importação Completa
- Sincronização Completa

Siga estes passos para executar cada um dos três perfis de execução.

1. Abra o Synchronization Service Manager e clique em **Agentes de Gestão** no menu **Ferramentas**.

2. Selecione **MIMMA** na lista **Agentes de Gestão**.

3. Clique em **Executar** no menu **Ações** para abrir a caixa de diálogo Executar Agente de Gestão.

4. Para cada perfil de execução indicado na tabela, conclua os seguintes passos:

    - Clique em **Executar** no menu **Ações** para abrir a caixa de diálogo Executar Agente de Gestão.
    - Selecione o perfil de execução que pretende executar na lista **Perfis de execução**.
    - Clique em **OK** para iniciar o perfil de execução.

> [!div class="step-by-step"]
> [« Portal e Serviço do MIM](install-mim-service-portal.md)
