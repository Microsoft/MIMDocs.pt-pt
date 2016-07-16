---
title: "Instalar o MIM 2016&#58; Sincronizar o Active Directory e o Serviço MIM | Microsoft Identity Manager"
description: "Utilize agentes de gestão e o Serviço de Sincronização do MIM para sincronizar as bases de dados do Active Directory e do MIM."
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 5e532b67-64a6-4af6-a806-980a6c11a82d
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 8fd779bcc4d41b6e67d0fa31aa0f37c4ea2b410a
ms.openlocfilehash: f4e94980c6a03b08221fd46e19c421cce226086d


---

# Instalar o MIM 2016: Sincronizar o Active Directory e o Serviço MIM

>[!div class="step-by-step"]
[« Portal e Serviço MIM](install-mim-service-portal.md)

> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio – **nomedoservidormim**
> - Nome de domínio – **contoso**
> - Palavra-passe – **Palavra@passe1**

Por predefinição, o Serviço de Sincronização do MIM (Sincronizar) não tem conetores configurados.  O primeiro passo típico consiste em utilizar a Sincronização do MIM para povoar a base de dados do Serviço MIM com contas do Active Directory existentes. Para tal, deverá utilizar a aplicação Serviço de Sincronização do MIM.

## Criar o agente de gestão do MIM
O agente de gestão (MA) do MIM é um conetor da Sincronização do MIM para o Serviço MIM. Para criar este conetor, utilize o assistente Criar Agente de Gestão.

Quando configura um agente de gestão do MIM, tem de especificar uma conta de utilizador. Este documento utiliza **MIMMA** como o nome desta conta.

> [!NOTE]
> A conta que utiliza para o agente de gestão do MIM tem de ser a mesma conta que especificou durante a instalação do Serviço MIM.

###Para criar o MA do MIM

1.  Abra o Synchronization Service Manager.

2.  Para abrir o assistente Criar Agente de Gestão, no menu **Ações**, clique em **Criar**.

3.  Na página **Criar Agente de Gestão**, forneça as seguintes definições e, em seguida, clique em **Seguinte**.

    -   Agente de gestão para: agente de gestão do Serviço MIM

    -   Nome: MIMMA

4.  Na página **Ligar à Base de Dados**, forneça as seguintes definições e, em seguida, clique em **Seguinte**

    -   Servidor: localhost

    -   Base de dados: ServiçoMIM

    -   Endereço base do Serviço MIM: http://localhost:5725

    -   Modo de autenticação: autenticação integrada do Windows

    -   Nome de utilizador: mimma

    -   Palavra-passe: Palavra@passe

    -   Domínio: contoso

5.  Na página **Tipos de Objetos Selecionados**, verifique se os tipos de objetos que estão listados abaixo estão selecionados e, em seguida, clique em **Seguinte**

    -   EntradaDeRegraEsperada

    -   EntradaDeRegraDetetada

    -   RegraDeSincronização

    -   Pessoa

    -   Grupo

6.  Na página **Atributos Selecionados**, verifique se todos os atributos indicados estão selecionados e, em seguida, clique em **Seguinte**.

7.  Na página **Configurar Filtro de Conetor**, clique em **Seguinte**.

8.  Na página **Configurar Mapeamentos de Tipos de Objeto**, adicione o seguinte mapeamento e, em seguida, clique em **Seguinte**

    - Selecione **Pessoa** na lista **Tipo do Objeto de Origem de Dados**.
    - Clique em **Adicionar Mapeamento** para abrir a caixa de diálogo Mapeamento.
    - Selecione **pessoa** na lista **Tipo de objeto metaverso**.
    - Clique em **OK** para fechar a caixa de diálogo Mapeamento.

9.  Na página **Configurar Fluxo de Atributos**, aplique os seguintes mapeamentos de fluxo de atributos e, em seguida, clique em **Seguinte**

    | **Atributo de Origem de Dados** | **Direção do Fluxo** | **Atributo Metaverso** |
    |-|-|-|
    | NomedaConta | Exportar | nomedaConta |
    | Nome a Apresentar | Exportar | nomeaApresentar |
    | Domain | Exportar | domínio |
    | EmployeeID | Exportar | IDdefuncionário |
    | TipoDeFuncionário | Exportar | tipodeFuncionário |
    | E-mail | Exportar | correio |
    | NomePróprio | Exportar | nomePróprio |
    | Apelido | Exportar | apelido |
    | SIDobjeto | Exportar | sidObjeto |

10.  Selecione **Pessoa** como o Tipo do objeto de origem de dados.

    -   Selecione **Pessoa** como o tipo de objeto do Metaverso.

    -   Selecione **Direto** como o Tipo de Mapeamento.

    -   Para cada linha na tabela anterior, conclua os seguintes passos:

        -   Selecione a **direção do Fluxo** mostrado para essa linha na tabela.

        -   Selecione o **atributo de Origem de dados** mostrado para essa linha na tabela.

        -   Selecione o **atributo do Metaverso** mostrado para essa linha na tabela.

        -   Para aplicar o mapeamento de fluxo, clique em **Novo**.

    -   Selecione **Grupo** como o tipo de origem de dados e como tipo de objeto do metaverso.

    -   Selecione **Direto** como o Tipo de Mapeamento.

    -   Para cada linha na tabela seguinte, conclua estes passos:

        -   Selecione a **direção do Fluxo** mostrado para essa linha na tabela.

        -   Selecione o **atributo de Origem de dados** mostrado para essa linha na tabela.

        -   Selecione o **atributo do Metaverso** mostrado para essa linha na tabela.

        -   Para aplicar o mapeamento de fluxo, clique em **Novo**.

    | **Atributo de Origem de Dados** | **Direção do Fluxo** | **Atributo Metaverso** |
    |-|-|-|
    | NomedaConta | Exportar | nomedaConta |
    | Nome a Apresentar | Exportar | nomeaApresentar |
    | Domain | Exportar | domínio |
    | E-mail | Exportar | correio |
    | MailNickName | Exportar | mailNickName |
    | Membro | Exportar | membro |
    | SIDobjeto | Exportar | sidObjeto |
    | Âmbito | Exportar | âmbito |
    | Tipo | Exportar | tipo |
    | MembershipAddWorkflow | Exportar | membershipAddWorkflow |
    | MembershipLocked | Exportar | membershipLocked |
    | Nome a Apresentar | Importar | nomeaApresentar |
    | Âmbito | Importar | âmbito |
    | Tipo | Importar | tipo |
    | Membro | Importar | membro |
    | NomedaConta | Importar | nomedaConta |
    | DisplayedOwner | Importar | displayedOwner |
    | MailNickName | Importar | mailNickName |


11.  Na página **Configurar Desaprovisionamento**, clique em **Seguinte**

12.  Para criar o agente de gestão, na página **Configurar Extensões**, clique em **Concluir**.

## Criar o agente de gestão do AD
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

7. Na página **Selecionar Atributos**, forneça as seguintes definições e, em seguida, clique em **Seguinte**:

    - Selecione **Mostrar Tudo**.

8. Na lista **Atributos**, selecione os seguintes atributos:

    -   empresa
    -   nomeaApresentar
    -   IDdefuncionário
    -   tipodeFuncionário
    -   nomeDado
    -   tipodeGrupo
    -   gestor
    -   geridoPor
    -   membro
    -   sidObjeto
    -   nomeContaSAM
    -   tipoContaSAM
    -   sn
    -   pwdUnicode
    -   controloContaUtilizador

9. Na página **Configurar Filtro de Conetor**, clique em **Seguinte**.

10. Na página **Configurar Regras de Associação e Projeção**, clique em **Seguinte**.

11. Na página **Configurar Fluxo de Atributos**, clique em **Seguinte**.

12. Na página **Configurar Desaprovisionamento**, clique em **Seguinte**.

13. Na página **Configurar Extensões**, clique em **Concluir**.


## Criar Perfis de Execução

Crie perfis de execução para os Conetores do MIMMA e do ADMA.

### Criar perfis de execução para o conetor do ADMA

Esta tabela mostra os cinco perfis de execução que criará para o conetor do ADMA:

| Nome | Tipo |
| ---- | ---- |
| Perfil1 | Importação Completa (Apenas Fase) |
| Perfil2 | Sincronização Completa |
| Perfil3 | Importação Delta (Apenas Fase) |
| Perfil4 | Sincronização Delta |
| Perfil5 | Exportar |

Para criar perfis de execução para o conetor do ADMA:

1. Abra o Synchronization Service Manager e, no menu **Ferramentas**, clique em **Agentes de Gestão**.

2. Na lista **Agentes de Gestão**, selecione **ADMA**.

3. Para abrir a caixa de diálogo Configurar Perfis de Execução para, no menu **Ações**, clique em **Configurar Perfis de Execução**.

4. Para cada perfil de execução na tabela, conclua os seguintes passos:

    - Para abrir o assistente Configurar Perfil de Execução, clique em **Novo Perfil**.

    - Na caixa **Nome**, escreva o nome de perfil da tabela e clique em **Seguinte**.

    - Na lista **Tipo**, selecione o tipo de passo da tabela e, em seguida, clique em **Seguinte**.

    - Clique em **Concluir** para criar o perfil de execução.

5. Para fechar a caixa de diálogo Configurar Perfis de Execução, clique em **OK**.

### Criar perfis de execução para o conetor do MIMMA

Esta tabela mostra os cinco perfis de execução correspondentes para o conetor do MIMMA:

| Nome | Tipo |
| -------- | -------- |
| Perfil1 | Importação Completa (Apenas Fase) |
| Perfil2 | Sincronização Completa |
| Perfil3 | Importação Delta (Apenas Fase) |
| Perfil4 | Sincronização Delta |
| Perfil5 | Exportar |

Para criar perfis de execução para o conetor do MIMMA:

1. Abra o Synchronization Service Manager e, no menu **Ferramentas**, clique em **Agentes de Gestão**.

2. Na lista **Agentes de Gestão**, selecione **MIMMA**.

3. Para abrir a caixa de diálogo Configurar Perfis de Execução para, no menu **Ações**, clique em **Configurar Perfis de Execução**.

4. Para cada perfil de execução na tabela, conclua os seguintes passos:

    - Para abrir o assistente Configurar Perfil de Execução, clique em **Novo Perfil**.

    - Na caixa **Nome**, escreva o nome de perfil da tabela e clique em **Seguinte**.

    - Na lista **Tipo**, selecione o tipo de passo da tabela e, em seguida, clique em **Seguinte**.

    - Clique em **Concluir** para criar o perfil de execução.

5. Para fechar a caixa de diálogo Configurar Perfis de Execução, clique em **OK**.

## Configurar o Serviço MIM

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
    -   Tipo de Recurso de Sistema Externo: pessoa

6. No separador **Relação**, forneça as seguintes informações e, em seguida, clique em **Seguinte**:

    -   Para configurar os Critérios de Relação, selecione **SIDobjeto** na lista ObjetoMetaverso:pessoa(Atributo) e na lista ObjetoDeSistemaLigado:pessoa (Atributo).

    -   Selecione **Criar Recurso no MIM**.

7. Na página **Fluxo de Atributos de Entrada**, forneça as seguintes informações e, em seguida, clique em **Seguinte**:

    | Regra de Fluxo | Origem | Destino |
    |-|-|-|
    |Regra 1|NomeContaSam|f|
    |Regra 2|nomeaApresentar|nomeaApresentar|
    |Regra 3|TipoDeFuncionário|TipoDeFuncionário|
    |Regra 4|nomeDado|nomeDado|
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

## Inicializar o ambiente de teste
Existem quatro passos que precisa de realizar antes de testar a configuração do MIM com dados do AD:

### Ativar Aprovisionamento

1. Abra o Synchronization Service Manager.

2. Para abrir a caixa de diálogo Opções, no menu **Ferramentas**, clique em **Opções**

3. Selecione **Ativar Aprovisionamento da Regra de Sincronização**.

4. Para fechar a caixa de diálogo Opções, clique em **OK**.

### Inicializar o MIMMA

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

#### Configurar a precedência do fluxo de atributos

Durante a inicialização do conetor do MIM, as regras de sincronização configuradas foram importadas para o metaverso.

Ajuste a precedência do fluxo de atributos para os atributos contribuídos por este conetor para se certificar de que os atributos já no AD possam circular para o metaverso e, posteriormente, para a base de dados do Serviço MIM.

### Inicializar o ADMA

Para inicializar o conetor do Active Directory, terá de executar uma importação completa e uma sincronização completa no mesmo. A importação completa transfere os objetos existentes do AD para o espaço conetor. A sincronização completa atualiza as regras de sincronização para corresponderem às do conetor do MIM.

1. Abra o Synchronization Service Manager e, no menu **Ferramentas**, clique em **Agentes de Gestão**.

2. Na lista **Agentes de Gestão**, selecione **ADMA**.

3. Para abrir a caixa de diálogo Executar Agente de Gestão, no menu **Ações**, clique em **Executar**.

4. Para cada perfil de execução indicado na tabela, conclua os seguintes passos:

    - Para abrir a caixa de diálogo Executar Agente de Gestão, no menu **Ações**, clique em **Executar**.

    - Na lista **Perfis de execução**, selecione o perfil de execução que pretende executar.

    - Para iniciar o perfil de execução, clique em **OK**.

### Povoar a base de dados do Serviço MIM

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

>[!div class="step-by-step"]
[« Portal e Serviço MIM](install-mim-service-portal.md)



<!--HONumber=Jun16_HO4-->


