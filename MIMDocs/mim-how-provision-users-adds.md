---
title: Microsoft Identity Manager 2016 prestação de utilizadores à AD  Microsoft Docs
description: Reveja o processo de criação de utilizadores no AD DS com o Microsoft Identity Manager 2016
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 149339a6e1029f01378a518a98029c1d588de6f9
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044179"
---
# <a name="how-do-i-provision-users-to-ad-ds"></a>Como Aprovisiono Utilizadores para o AD DS?

Aplica-se a: Microsoft Identity Manager 2016 SP1 (MIM)

Um requisito básico de um sistema de gestão de identidades é a capacidade de aprovisionar recursos para um sistema externo.

Este guia contém orientações acerca dos principais blocos modulares envolvidos no processo de aprovisionamento de utilizadores a partir do Microsoft® Identity Manager (MIM) 2016 para o Active Directory® Domain Services (AD DS), descreve como pode verificar se o seu cenário está a funcionar conforme esperado, oferece sugestões para a gestão de utilizadores do Active Directory com o MIM 2016 e indica fontes de informação adicionais.

## <a name="before-you-begin"></a>Antes de Começar


Nesta secção, irá encontrar informações sobre o âmbito deste documento. Em geral, os guias de instruções são direcionados para os leitores que já têm experiência com o processo de sincronização de objetos com o MIM, conforme descrito nos [Guias de Introdução](https://go.microsoft.com/FWLink/p/?LinkId=190486) relacionados.

### <a name="audience"></a>Audiência


Este guia destina-se a profissionais de tecnologias de informação (TI) que já possuem conhecimentos básicos sobre a forma como o processo de sincronização do MIM funciona e que têm interesse em obter experiência prática e informações de tipo conceptual sobre cenários específicos.

### <a name="prerequisite-knowledge"></a>Conhecimento dos pré-requisitos


Este documento parte do princípio que tem acesso a uma instância em execução do MIM e que tem experiência com a configuração de cenários de sincronização simples, conforme descrito nos seguintes documentos:

-   [Introdução à Sincronização de Entrada](https://go.microsoft.com/FWLink/p/?LinkId=189652)

-   [Introdução à Sincronização de Saída](https://go.microsoft.com/FWLink/p/?LinkId=189653)

Os conteúdos neste documento funcionam como uma extensão destes documentos de introdução.

### <a name="scope"></a>Âmbito


O cenário descrito neste documento foi simplificado com o intuito de abordar os requisitos de um ambiente básico de laboratório. O objetivo é que fique a compreender as tecnologias e os conceitos abordados.

Este documento ajuda-o a desenvolver uma solução que envolve a gestão de grupos no AD DS através do MIM.

### <a name="time-requirements"></a>Requisitos de tempo


Os procedimentos descritos neste documento levam entre 90 a 120 minutos a concluir.

Estas estimativas de tempo pressupõem que o ambiente de teste já foi configurado e não incluem o tempo necessário para o configurar.

### <a name="getting-support"></a>Obter suporte


Se tiver alguma pergunta em relação aos conteúdos deste documento ou feedback geral que gostaria de abordar, pode publicar uma mensagem no [fórum do Forefront Identity Manager 2010](https://go.microsoft.com/FWLink/p/?LinkId=189654).

## <a name="scenario-description"></a>Descrição do Cenário


A Fabrikam, uma empresa fictícia, pretende utilizar o MIM para gerir as contas de utilizador existentes no AD DS da empresa através do MIM. Como parte deste processo, a Fabrikam tem de aprovisionar utilizadores para o AD DS. Para começar o teste inicial, a Fabrikam instalou um ambiente de laboratório básico composto pelo MIM e pelo AD DS.
Neste ambiente de laboratório, a Fabrikam está a testar um cenário que consiste num utilizador criado manualmente no Portal do MIM. O objetivo deste cenário é aprovisionar o utilizador como um utilizador ativo e com uma palavra-passe predefinida para o AD DS.

## <a name="scenario-design"></a>Estrutura do Cenário


Para utilizar este guia, precisa de três componentes de arquitetura:

-   Controlador de domínio do Active Directory

-   Computador com o Serviço de Sincronização do FIM em execução
-   Computador com o Portal do FIM em execução

A seguinte ilustração descreve o ambiente necessário.

![Ambiente necessário](media/how-provision-users-adds/image001.png)


Pode executar todos os componentes num computador.

> [!NOTE]
> Para obter mais informações sobre como configurar o MIM, veja o [Guia de Instalação do FIM](https://go.microsoft.com/FWLink/p/?LinkId=165845).

## <a name="scenario-components-list"></a>Lista de Componentes do Cenário


A seguinte tabela indica os componentes que fazem parte do cenário contido neste guia.

| ![Unidade Organizacional](media/how-provision-users-adds/image005.jpg)   | Unidade organizacional                | Objetos FIM – unidade organizacional (UO) que é utilizada como destino para os utilizadores aprovisionados.                                                       |
|----------------------------------------|------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| ![Contas de utilizador](media/how-provision-users-adds/image006.jpg)   | Contas de utilizador                      | &#183; **ADMA** – conta de utilizador do Active Directory com um número de direitos suficiente para ligar ao AD DS.<br/> &#183; **FIMMA** – conta de utilizador do Active Directory com direitos suficientes para ligar ao MIM.
                                                                 |
| ![Agentes de gestão e perfis de execução](media/how-provision-users-adds/image007.jpg)  | Agentes de gestão e perfis de execução | &#183; **ADMA da Fabrikam** – agente de gestão que troca dados com o AD DS. <br/> &#183; FIMMA da Fabrikam – agente de gestão que troca dados com o MIM.                                                                                 |
| ![Regras de sincronização](media/how-provision-users-adds/image008.jpg)  | Regras de sincronização              | Regra de Sincronização de Saída de Grupo da Fabrikam – regra de sincronização de saída que aprovisiona utilizadores para o AD DS.                                     |
| ![Conjuntos](media/how-provision-users-adds/image009.jpg)   | Conjuntos                               | Todos os Contratantes – conjunto com associação de grupo dinâmica de todos os objetos com o valor de atributo EmployeeType do Contratante.                                |
| ![Fluxos de Trabalho](media/how-provision-users-adds/image010.jpg)  | Fluxos de Trabalho                          | Fluxo de Trabalho de Aprovisionamento do AD – fluxo de trabalho para incluir o utilizador do MIM no âmbito da Regra de Sincronização de Saída do AD.                                |
| ![Regras de política de gestão](media/how-provision-users-adds/image011.jpg)   | Regras de política de gestão            | Regra de Política de Gestão de Aprovisionamento do AD – a regra de política de gestão (MPR) que é acionada quando um recurso se torna membro do conjunto Todos os Contratantes. |
| ![Utilizadores do MIM](media/how-provision-users-adds/image012.jpg) | Utilizadores do MIM                          | Eduarda Almeida – utilizadora do MIM a aprovisionar ao AD DS.                                                                                             |



## <a name="scenario-steps"></a>Passos do Cenário


O cenário descrito neste guia consiste nos blocos modulares apresentados na seguinte imagem.

![Passos do cenário](media/how-provision-users-adds/image013.png)


## <a name="configuring-the-external-systems"></a>Configurar os Sistemas Externos


Nesta secção, encontrará instruções para os recursos que precisa de criar e que se encontram fora do seu ambiente do MIM.

### <a name="step-1-create-the-ou"></a>Passo 1: criar a UO


Tem de criar a UO como um contentor para o utilizador de exemplo aprovisionado. Para obter mais informações sobre como criar UOs, veja [Criar uma Nova Unidade Organizacional](https://go.microsoft.com/FWLink/p/?LinkId=189655).

Crie uma UO com o nome MIMObjects no AD DS.

### <a name="step-2-create-the-active-directory-user-accounts"></a>Passo 2: crie as contas de utilizador do Active Directory

Para o cenário descrito neste guia, irá precisar de duas contas de utilizador do Active Directory:

- **ADMA** – utilizada pelo agente de gestão do Active Directory.

- **FIMMA** – utilizada pelo agente de gestão do Serviço FIM.

Em ambos os casos, basta criar contas de utilizador normais. Poderá encontrar informações adicionais sobre os requisitos específicos de ambas as contas mais adiante neste documento. Para obter mais informações sobre como criar utilizadores, veja [Criar uma Nova Conta de Utilizador](https://go.microsoft.com/FWLink/p/?LinkId=189656).


## <a name="configuring-the-fim-synchronization-service"></a>Configurar o Serviço de Sincronização do FIM


Para os passos de configuração descritos nesta secção, terá de iniciar o Synchronization Service Manager do FIM.

### <a name="creating-the-management-agents"></a>Criar os agentes de gestão

Para o cenário descrito neste guia, terá de criar dois agentes de gestão:

-   **ADMA da Fabrikam** – o agente de gestão do AD DS.

-   **FIMMA da Fabrikam** – o agente de gestão do serviço FIM.

### <a name="step-3-create-the-fabrikam-adma-management-agent"></a>Passo 3: criar o agente de gestão do ADMA da Fabrikam

Ao configurar um agente de gestão do AD DS, tem de especificar uma conta que seja utilizada pelo agente de gestão na troca de dados com o AD DS. Deve utilizar uma conta de utilizador normal. No entanto, para importar os dados do AD DS, a conta tem de ter o direito para consultar as alterações feitas pelo controlo DirSync. Se pretende que o seu agente de gestão exporte dados para o AD DS, tem de conceder direitos suficientes à conta na UO de destino. Para obter mais informações sobre este tópico, veja [Configuring the ADMA Account (Configurar a Conta ADMA)](https://go.microsoft.com/FWLink/p/?LinkId=189657).

Para criar um utilizador no AD DS, terá de transitar o DN do objeto. Para além disso, é uma boa prática transitar o nome próprio, apelido e o nome a apresentar para garantir que os seus objetos podem ser detetados.

No AD DS, ainda é comum os utilizadores utilizarem o atributo sAMAccountName para iniciar sessão no serviço de diretório. Se não especificar um valor para este atributo, o serviço de diretório irá gerar um valor aleatório para o mesmo. No entanto, estes valores aleatórios não são fáceis de utilizar e é por este motivo que as exportações para o AD DS costumam incluir uma versão deste atributo que seja fácil de utilizar. Para permitir que um utilizador inicie sessão no AD DS, terá também de incluir uma palavra-passe criada através do atributo unicodePwd na lógica de exportação.

> [!Note]
> Certifique-se de que o valor que especificar com o atributo unicodePwd está em conformidade com as políticas de palavra-passe do AD DS de destino.

Ao definir uma palavra-passe para contas AD DS, também terá de criar uma conta ativada. Poderá fazê-lo ao definir o atributo userAccountControl. Para obter mais informações sobre o atributo userAccountControl, veja [Using FIM to Enable or Disable Accounts in Active Directory (Utilizar o FIM para Ativar ou Desativar Contas no Active Directory)](https://go.microsoft.com/FWLink/p/?LinkId=189658).

A seguinte tabela indica as definições específicas do cenário mais importantes que terá de configurar.

| Página de estruturador do agente de gestão                          | Configuração                                                  |
|---------------------------------------------------------|----------------------------------------------------------------|
| Criar agente de gestão                                 | 1. **Agente de gestão para:** AD DS  <br/> 2. **Nome:** Fabrikam ADMA |
| Ligar à floresta do Active Directory                      | 1. **Selecione divisórias de diretório:** "DC=Fabrikam,DC=com"   <br/>   2. Clique em **recipientes** para abrir a caixa de diálogo **Select Containers** e **certifique-se** de que mimObjects é o único OU que é selecionado.        |
| Selecionar Tipos de objeto                                     | Para além dos Tipos de objeto já selecionados, selecione **utilizador.** |
| Selecionar atributos                                       | 1. Clique **em mostrar tudo.** <br/>   2. Selecione os seguintes atributos: <br/> &nbsp;&nbsp;&nbsp;&#176; **displayName** <br/> &nbsp;&nbsp;&nbsp;&#176; **givenName** <br/> &nbsp;&nbsp;&nbsp;&#176;  **sn** <br/> &nbsp;&nbsp;&nbsp;&#176;  **SamAccountName** <br/> &nbsp;&nbsp;&nbsp;&#176;  **unicodePwd** <br/> &nbsp;&nbsp;&nbsp;&#176;  **userAccountControl**     

Para obter mais informações, veja os seguintes tópicos no menu Ajuda:
- Criar um Agente de Gestão
- Ligar a uma Floresta do Active Directory
- Utilizar o Agente de Gestão do Active Directory
- Configurar Partições de Diretório

> [!Note]
> Certifique-se de que configurou uma regra de importação de fluxos de atributos para o atributo ExpectedRulesList.

### <a name="step-4-create-the-fabrikam-fimma-management-agent"></a>Passo 4: criar o agente de gestão do FIMMA da Fabrikam

Ao configurar um agente de gestão do Serviço FIM, tem de especificar uma conta que seja utilizada pelo agente de gestão na troca de dados com o Serviço FIM.

Deve utilizar uma conta de utilizador normal. A conta tem de ser a mesma que especificou durante a instalação do MIM. Para obter um script que pode utilizar para determinar o nome da conta FIMMA que especificou durante a configuração e para testar se esta conta ainda é válida, veja [Using Windows PowerShell to Do a FIMMA Account Configuration Quick Test (Utilizar o Windows PowerShell para Realizar um Teste Rápido à Configuração de Contas FIMMA)](https://go.microsoft.com/FWLink/p/?LinkId=189659).

A seguinte tabela indica as definições específicas do cenário mais importantes que terá de configurar. Crie o agente de gestão com base nas informações fornecidas na tabela abaixo.  

| Página de estruturador do agente de gestão | Configuração |
|------------|------------------------------------|
| Criar agente de gestão | 1. **Agente de gestão para:** Agente de Gestão de Serviços FIM <br/> 2. **Nome** Fabrikam FIMMA |
| Ligar à base de dados     | Utilize as seguintes definições: <br/> &#183; **Servidor:** localhost <br/> &#183; **Base de dados:** FIMService <br/> &#183;**Endereço de base do serviço FIM:** http://localhost:5725 <br/> <br/> Forneça as informações relativas à conta que criou para este agente de gestão |
| Selecionar Tipos de objeto                                     | Para além dos Tipos de objeto selecionados, selecione **Pessoa.**   |
| Configurar mapeamentos de Tipos de objetos                          | Para além dos mapeamentos de tipos de objetos existentes, adicione um mapeamento da pessoa **Tipo de Objeto Origem de Dados** à pessoa **Tipo de Objeto Metaverso**. |
| Configurar fluxo de atributos                                | Para além dos mapeamentos de fluxos de atributos existentes, adicione os seguintes mapeamentos de fluxos de atributos: <br/><br/> ![Fluxo de atributos](media/how-provision-users-adds/image018.jpg) |




Para obter mais informações, consulte os seguintes tópicos do menu de ajuda:
-   Criar um Agente de Gestão

-   Ligar a uma Base de Dados do Active Directory

-   Utilizar o Agente de Gestão do Active Directory

-   Configurar Partições de Diretório

> [!NOTE]
>  Certifique-se de que configurou uma regra de importação de fluxos de atributos para o atributo ExpectedRulesList.

### <a name="step-5-create-the-run-profiles"></a>Passo 5: criar os perfis de execução

A seguinte tabela indica os perfis de execução que tem de criar para o cenário descrito neste guia.

| Agente de gestão  | Perfil de execução     |
|-------------------|--------------------------------------|
| ADMA da Fabrikam     | 1. Importação integral  <br/> 2. Sincronização completa <br/> 3. Importação delta <br/> 4. Sincronização delta <br/> 5. Exportação                                                                    |
| FIMMA da Fabrikam   | 1. Importação integral <br> 2. Sincronização completa <br/> 3. Importação Delta <br/> 4. Sincronização delta <br/> 5. Exportação|                                                                                                                                                                                   

Crie perfis de execução para cada agente de gestão conforme a tabela anterior.


> [!Note]
> Para obter mais informações, veja o tópico Criar um Perfil de Execução do Agente de Gestão na Ajuda do MIM.                                                                                                                  
> 
> 
> [!Important]
>  Verifique se o aprovisionamento se encontra ativado no seu ambiente. Pode fazê-lo executando o script, utilizando o Windows PowerShell para ativar o provisionamento (https://go.microsoft.com/FWLink/p/?LinkId=189660).


## <a name="configuring-the-fim-service"></a>Configurar o Serviço FIM


Para o cenário descrito neste guia, tem de configurar uma política de aprovisionamento conforme apresentado na seguinte imagem.

![Política de Aprovisionamento](media/how-provision-users-adds/image019.png)

O objetivo desta política de aprovisionamento é incluir grupos no âmbito da Regra de Sincronização de Saída do AD. Ao incluir o seu recurso no âmbito da regra de sincronização, está a permitir que o motor de sincronização aprovisione o recurso no AD DS de acordo com a sua configuração.

Para configurar o serviço FIM, navegue no Windows Internet Explorer® para http://localhost/identitymanagement. Na página do Portal do MIM Portal, para criar a política de aprovisionamento, aceda às páginas relacionadas da secção Administração. Para verificar a sua configuração, deve executar o script [Using Windows PowerShell to document your provisioning policy configuration (Utilizar o Windows PowerShell para documentar a configuração da sua política de aprovisionamento)](https://go.microsoft.com/FWLink/p/?LinkId=189661).

### <a name="step-6-create-the-synchronization-rule"></a>Passo 6: criar a regra de sincronização

As seguintes tabelas mostram a configuração da regra de sincronização de aprovisionamento da Fabrikam. Crie a regra de sincronização de acordo com os dados apresentados nas seguintes tabelas.

| Configuração de regra de sincronização                                                                         |                                                                             |                                                           
|------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------|
| Nome                                                                                                       | Regra de Sincronização de Saída de Utilizadores do Active Directory                         |                                                          
| Description                                                                                               |                                                                             |                                                           
| Precedência                                                                                                | 2                                                                           |                                                           
| Direção do Fluxo de Dados   | Saída             |       
| Dependência       |         |                                         


| Âmbito |                                                                             |                                                           
|--------|-------|
| Tipo de Recurso Metaverso | pessoa |                                                         
| Sistema Externo                   |ADMA da Fabrikam                                                               |                                                       
| Tipo de Recurso Sistema Externo                                                                              | utilizador      



| Relationship ||
|------------|---------|
| Criar Recurso no Sistema Externo                                                                         | Verdadeiro                                                                        |                                                           
| Ativar Desaprovisionamento                                                                                      | Falso                                                                       |                                                           

| Critérios de relação                                                                                      | |
|------------|----------|
| Atributo ILM     | Atributo de Origem de Dados                                                       |
| Atributo de Origem de Dados         | sAMAccountName    |

| Fluxos do atributo de saída iniciais        | |                                                             |
|-------------------|---------------------- |---------------|
| Permitir valores nulos                 | Destino                                                                 | Origem                                                    |
| falso                       | dn                                                                          | \+("CN=",displayName,",OU=MIMObjects,DC=fabrikam,DC=com") |
| falso                       | userAccountControl                                                          | **Constante:** 512                                         |
| falso                                                                     | pwdUnicode                    | Constante: P\@\$\$W0rd                                    |

| Fluxos do atributo de saída persistentes  |                                                                     |                                                           |
|--------------------------------------|---------------------------------------------------------------------|-----------------------------------------------------------|
| Permitir valores nulos                                                                                                | Destino                                                                 | Origem                                                    |
| falso                                                                                                      | sAMAccountName                                                              | accountName                                               |
| falso                                                                                                      | displayName                                                                 | displayName                                               |
| falso                                                                                                      | givenName                                                                   | firstName                                                 |
| falso                                                                                                      | sn                                                                          | lastName                                                  |



> [!NOTE]
>  Importante Certifique-se de que selecionou a opção Apenas Fluxo Inicial para o fluxo do atributo que contém o DN como destino.                                                                          

### <a name="step-7-create-the-workflow"></a>Passo 7: criar o fluxo de trabalho

O Fluxo de Trabalho de Aprovisionamento do AD tem como objetivo adicionar a regra de sincronização de aprovisionamento da Fabrikam a um recurso. As seguintes tabelas mostram a configuração.  Crie um fluxo de trabalho de acordo com os dados apresentados nas tabelas abaixo.

| Configuração de fluxo de trabalho               |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Nome                                 | Fluxo de Trabalho de Aprovisionamento de Utilizadores do Active Directory                     |
| Description                          |                                                                 |
| Tipo de Fluxo de Trabalho                        | Action                                                          |
| Executar Na Atualização da Política                 | Falso                                                           |

| Regra de sincronização                 |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Nome                                 | Regra de Sincronização de Saída de Utilizadores do Active Directory             |
| Action                               | Adicionar                                                             |




### <a name="step-8-create-the-mpr"></a>Passo 8: criar o MPR

O tipo de MPR necessário é Transição de Conjunto e este é acionado quando um recurso se torna membro do conjunto Todos os Contratantes. As seguintes tabelas mostram a configuração.  Crie um MPR de acordo com os dados nas tabelas abaixo.

| Configuração de MPR                    |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Nome                                 | Regra de Política de Gestão do Aprovisionamento de Utilizadores do AD                 |
| Description                          |                                                             |
| Tipo                                 | Transição de Conjunto                                              |
| Concede Permissões                   | Falso                                                       |
| Desativado                             | Falso                                                       |

| Definição da transição                |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Tipo de Transição                      | Transição de Entrada                                               |
| Conjunto de Transições                       | Todos os Contratantes                                             |

| Fluxos de trabalho de políticas                     |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Tipo                                 | Action                                                      |
| Nome a Apresentar                         | Fluxo de Trabalho de Aprovisionamento de Utilizadores do Active Directory                 |




## <a name="initializing-your-environment"></a>Iniciar o seu Ambiente


Os objetivos da fase de inicialização são os seguintes:

-   Incluir a sua regra de sincronização no metaverso.

-   Incluir a sua estrutura do Active Directory no espaço conector do Active Directory.

### <a name="step-9-run-the-run-profiles"></a>Passo 9: executar os perfis de execução

A seguinte tabela indica os perfis de execução que fazem parte da fase de inicialização.  Execute os perfis de execução de acordo com a tabela abaixo.

| Executar                                                                                                           | Agente de gestão                                      | Perfil de execução          |
|---------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|----------------------|
| 1                                                                                                             | FIMMA da Fabrikam                                        | Importação completa          |
| 2                                                                                                             |                                                       | Sincronização completa |
| 3                                                                                                             |                                                       | Exportar               |
| 4                                                                                                             |                                                       | Importação delta         |
|                                                                                                               |                                                       |                      |
| 5                                                                                                             | ADMA da Fabrikam                                         | Importação completa          |
| 6                                                                                                             |                                                       | Sincronização completa |



> [!NOTE]
> Deve verificar se a sua regra de sincronização de saída foi projetada no metaverso com êxito.

## <a name="testing-the-configuration"></a>Testar a Configuração


Esta secção tem como objetivo testar a sua configuração atual. Para testar a configuração, terá de:

1.  Criar um utilizador de exemplo no Portal do FIM.

2.  Verificar os requisitos de aprovisionamento do utilizador de exemplo.

3.  Aprovisionar o utilizador de exemplo no AD DS.

4.  Verificar se o utilizador existe no AD DS.

### <a name="step-10-create-a-sample-user-in-mim"></a>Passo 10: criar um utilizador de exemplo no MIM


A seguinte tabela indica as propriedades do utilizador de exemplo. Crie um utilizador de amostra de acordo com os dados na tabela abaixo.

| Atributo                              | Valor                                                          |
|----------------------------------------|----------------------------------------------------------------|
| Nome Próprio                             | Eduarda                                                         |
| Apelido                              | Almeida                                                          |
| Nome a Apresentar                           | Eduarda Almeida                                                   |
| Nome da Conta                           | EAlmeida                                                         |
| Domain                                 | Fabrikam                                                       |
| Tipo de Funcionário                          | Contratante                                                     |



### <a name="verify-the-provisioning-requisites-of-the-sample-user"></a>Verificar os requisitos de aprovisionamento do utilizador de exemplo


Para aprovisionar o utilizador de exemplo no AD DS, é necessário cumprir dois pré-requisitos:

1.  O utilizador tem de ser membro do conjunto Todos os Contratantes.

2.  O utilizador definido tem de estar no âmbito da regra de sincronização de saída.

### <a name="step-11-verify-the-user-is-a-member-of-all-contractors"></a>Passo 11: verificar se o utilizador é membro do conjunto Todos os Contratantes

Se verificar se o utilizador é membro do conjunto Todos os Contratantes, abra o conjunto e, em seguida, clique em Ver Membros.

![Verifique se o utilizador é membro do conjunto Todos os Contratantes](media/how-provision-users-adds/image022.jpg)


### <a name="step-12-verify-the-user-is-in-the-scope-of-the-outbound-synchronization-rule"></a>Passo 12: verifique se o utilizador está no âmbito da regra de sincronização de saída

Para verificar se o utilizador está no âmbito da regra de sincronização, abra a página de propriedade do utilizador e reveja o atributo da Lista de Regras Esperadas no separador Provisioning. O atributo da Lista de Regras Esperadadeve listar o Utilizador AD

Regra de Sincronização de Saída. A imagem que se segue mostra um exemplo do atributo da Lista de Regras Esperadas.

![Estado da regra de sincronização](media/how-provision-users-adds/image023.jpg)

Nesta fase do processo, o Estado da Regra de Sincronização será Pendente. Isto significa que a regra de sincronização ainda não foi aplicada ao utilizador.



### <a name="step-13-synchronize-the-sample-group"></a>Passo 13: sincronizar o grupo de exemplo


Antes de iniciar o primeiro ciclo de sincronização de um objeto de teste, deve controlar o estado esperado do objeto após executar todos os perfis de execução necessários num plano de teste. O seu plano de teste também deve incluir os valores do atributo esperados junto do estado geral do seu objeto (criado, atualizado ou eliminado).
Utilize o seu plano de teste para comprovar as suas expectativas em relação ao mesmo. Se um passo não devolver os resultados esperados, não avance para o passo seguinte até ter resolvido as discrepâncias entre o resultado esperado e o resultado real.

Para comprovar as suas expectativas, pode utilizar as estatísticas de sincronização como um primeiro indicador. Por exemplo, se estiver à espera que os novos objetos sejam transitados por um espaço conector, mas as estatísticas de importação não devolverem "Adições", existe algo no seu ambiente que não está a funcionar conforme esperado.

![Estatísticas de sincronização](media/how-provision-users-adds/image024.jpg)

Embora as estatísticas de sincronização possam dar-lhe uma primeira indicação se o seu cenário funciona conforme o esperado, deve utilizas as funcionalidades Procurar Espaço Conector e Procurar Metaverso do Gestor do Serviço de Sincronização para verificar os valores do atributo esperados.

Para sincronizar o utilizador no AD DS:

1.  Importe o utilizador para o espaço conector do FIMMA.

2.  Projete o utilizador no metaverso.

3.  Aprovisione o utilizador no espaço conector do Active Directory.

4.  Exporte as informações de estado para o FIM.

5.  Exporte o utilizador para o AD DS.

6.  Confirme a criação do utilizador.

Para efetuar estas tarefas, deve executar os seguintes perfis de execução.

| Agente de gestão | Perfil de execução  |
|------------------|--------------|
| FIMMA da Fabrikam   | 1. Importação delta <br/> 2. Sincronização Delta <br/> 3. Exportação <br/> 4. Importação delta |
| FIMMA da Fabrikam   | 1. Exportação <br/> 2. Importação Delta       |


Após a importação da base de dados do Serviço FIM, Britta Simon e o objeto ExpectedRuleEntry que liga Britta à Regra de Sincronização de Saída do Utilizador AD são encenados no espaço do conector Fabrikam FIMMA. Ao rever as propriedades da Britta no espaço do conector, junto aos valores de atributo que configurado no Portal FIM, também encontra uma referência válida ao objeto de entrada de regras esperada. A imagem que se segue mostra um exemplo disso.

![Propriedades dos objetos de espaço conector](media/how-provision-users-adds/image025.jpg)

A execução da sincronização delta no FIMMA da Fabrikam tem como objetivo efetuar várias operações:

-   Projeção – o objeto Novo Utilizador e o objeto relacionado Entrada de Regra Esperada são projetados no metaverso.

-   Aprovisionamento – O objeto Eduarda Almeida recentemente projetado é aprovisionado no espaço conector do ADMA da Fabrikam.

-   Exportar Fluxos de Atributos – a exportação de fluxos de atributos ocorre em ambos os agentes de gestão. No ADMA da Fabrikam, o objeto Eduarda Almeida recentemente aprovisionado é preenchido com novos valores de atributo. No FIMMA da Fabrikam, o objeto Eduarda Almeida e o objeto relacionado ExpectedRuleEntry existentes são atualizados com os valores de atributo resultantes da projeção.

![Estatísticas de sincronização](media/how-provision-users-adds/image026.jpg)

Conforme indicado pelas estatísticas de sincronização, ocorreu uma atividade de aprovisionamento no espaço conector do ADMA da Fabrikam. Ao rever as propriedades dos objetos de metaverso da Eduarda Almeida, irá perceber que esta atividade é resultado do atributo ExpectedRulesList que foi preenchido com uma referência válida.

![Propriedades dos objetos de metaverso](media/how-provision-users-adds/image027.jpg)

Durante a seguinte exportação para o FIMMA da Fabrikam, o estado da regra de sincronização da Eduarda Almeida passa de Pendente a Aplicado, o que indica que a regra de sincronização de saída já se encontra ativa no objeto contido no metaverso.

![Regra de sincronização aplicada](media/how-provision-users-adds/image028.jpg)

Devido ao aprovisionamento de um novo objeto ao espaço conector do ADMA, deverá ter uma exportação Adicionar pendente neste agente de gestão. 

![Exportações pendentes do agente de gestão](media/how-provision-users-adds/image029.jpg)

No FIM, todas as execuções de exportação requerem uma importação delta para concluir a operação de exportação. A importação delta que efetuar após a execução de uma exportação anterior é denominada como uma importação de confirmação. As importações de confirmação são necessárias para permitir que o Serviço de Sincronização do FIM aplique os requisitos de atualização adequados durante execuções de sincronização sucessivas.


Execute os perfis de execução de acordo com as instruções indicadas nesta secção.

> [!IMPORTANT]
> Cada perfil de execução tem de ser executado sem erros.

### <a name="step-14-verify-the-provisioned-user-in-ad-ds"></a>Passo 14: verificar o utilizador aprovisionado no AD DS

Para verificar se o seu utilizador de exemplo foi aprovisionado no AD DS, tem de abrir a UO FIMObjects. Deverá encontrar a utilizadora Eduarda Almeida na UO FIMObjects.

![Verifique se o seu utilizador de exemplo se encontra na UO FIMObjects.](media/how-provision-users-adds/image033.jpg)

## <a name="summary"></a>Resumo


O objetivo deste documento é apresentar-lhe os principais blocos modulares para sincronizar utilizadores no MIM com o AD DS. No seu teste inicial, deve começar por especificar o número mínimo de atributos necessários para concluir uma tarefa e adicionar mais atributos ao seu cenário quando os passos gerais funcionarem conforme o esperado. Reduzir a complexidade dos atributos ao mínimo possível simplifica o processo de resolução de problemas.

Quando testar a sua configuração, é muito provável que tenha de eliminar e voltar a criar novos objetos de teste. Para objetos com um

atributo esperado De Regras Lista, isto pode resultar em objetos ERE órfãos.
Para obter uma descrição sobre como pode remover estes objetos do seu ambiente de teste, veja [A Method to Remove Orphaned ExpectedRuleEntry Objects from Your Environment (Um Método para Remover Objetos ExpectedRuleEntry Isolados do seu Ambiente)](https://go.microsoft.com/FWLink/p/?LinkId=189667).

Num cenário de sincronização típico que inclua o AD DS como destino da sincronização, o MIM não é obrigatório para todos os atributos de um objeto. Por exemplo, quando gerir objetos de utilizador no AD DS através do FIM, no mínimo é necessário que o domínio e os atributos objectSID sejam gerados pelo agente de gestão do AD DS.
O nome, o domínio e os atributos objectSID da conta são necessários se quiser permitir que um utilizador possa iniciar sessão no Portal do FIM. Para preencher estes atributos do AD DS, é necessária uma regra de sincronização de entrada adicional para o espaço conector do AD DS. Quando gerir objetos com múltiplas origens de valores de atributos, certifique-se de que configura a precedência do fluxo de atributos corretamente. Se a precedência do fluxo de atributos não for configurada corretamente, o motor de sincronização impedirá o preenchimento dos valores de atributos. Encontrará mais informações sobre a precedência do fluxo de atributos no artigo [About Attribute Flow Precedence (Acerca da Precedência do Fluxo de Atributos)](https://go.microsoft.com/FWLink/p/?LinkId=189675).

## <a name="next-steps"></a>Passos Seguintes

[Using FIM to Enable or Disable Accounts in Active Directory (Utilizar o FIM para Ativar ou Desativar Contas no Active Directory)](https://go.microsoft.com/FWLink/p/?LinkId=189670)

[Compreender o processamento de atributos de referência](https://go.microsoft.com/FWLink/p/?LinkId=189671)

[Como Gerir a Conta MA FIM](https://go.microsoft.com/FWLink/p/?LinkId=189672)

[Deteção de Contas Não Autorizadas – Parte 1: Visualização](https://go.microsoft.com/FWLink/p/?LinkId=189673)

[Como Detetar Conectores](https://go.microsoft.com/FWLink/p/?LinkId=189674)

[Como configurar a Conta ADMA](https://go.microsoft.com/FWLink/p/?LinkId=189657)

[About Attribute Flow Precedence (Acerca da Precedência do Fluxo de Atributos)](https://go.microsoft.com/FWLink/p/?LinkId=189675)

[Compreensão das Exportações](https://social.technet.microsoft.com/wiki/contents/articles/1861.understanding-exports-in-ilm-2007.aspx)
