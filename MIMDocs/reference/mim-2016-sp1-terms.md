---
title: Terminologia SP1 do Microsoft Identity Manager 2016 Microsoft Docs
description: Lista completa de termos que são referenciados no Microsoft Identity Manager 2016 SP1.
keywords: Terminologia
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/28/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: fimguy
ms.suite: ems
ms.openlocfilehash: afe167cdcd6ca548ef34e802f5606bee6ba5b31e
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762451"
---
# <a name="microsoft-identity-manager-2016-sp1-terminology"></a>Terminologia SP1 do Microsoft Identity Manager 2016

Este documento é uma lista completa de termos que são referenciados no Microsoft Identity Manager 2016 SP1.

## <a name="a"></a>A

**Validação do grupo Ative Directory** : Um procedimento implementado no MIM 2016 que garante a singularidade do nome da conta de um grupo dentro de um domínio armazenado no Ative Directy.

**fluxo de trabalho de ação** : Um fluxo de trabalho que realiza uma ação. Isto inclui o envio de um e-mail de notificação e alterações na base de dados do Serviço MIM.

**atividade** : Uma atividade de fluxo de trabalho é o bloco básico de construção dos fluxos de trabalho do Windows Workflow Foundation (WF). Incorpora a lógica que é iniciada no tempo de conceção e tempo de execução ao construir e executar fluxos de trabalho.

**conjunto de atividades** : A . DLL ou um . Ficheiro EXE contendo um conjunto .NET que implementa a lógica para uma atividade de fluxo de trabalho.

**âncora** : Um ou mais atributos únicos de um tipo de objeto que não muda e representa um objeto na fonte de dados ligada à qual o objeto de espaço do conector está ligado (por exemplo, um número de funcionário ou um ID do utilizador).

**aprovação** : Uma aprovação é um ponto de decisão de fluxo de trabalho que pode ser usado para obter autorização de uma pessoa antes de continuar no fluxo de trabalho.

**e-mail de aprovação** : Se um pedido requer uma aprovação antes de ser comprometido, é enviado um e-mail de aprovação para os aprovadores identificados.

**pedido de aprovação** : Um pedido que requer aprovação. Por exemplo, uma mensagem de correio eletrónico enviada pela MIM 2016 a um aprovador como parte do processamento de uma atividade de aprovação.

**resposta de aprovação** : Resposta para um pedido de aprovação. Contém informações sobre se o pedido é aprovado ou não. Por exemplo, uma mensagem de correio eletrónico enviada do MIM Add-in for Outlook em resposta a um pedido de aprovação.

**pasta de pesquisa de aprovações** : As pastas de pesquisa criadas pelo ADD-in MIM para o Outlook que proporcionam ao utilizador uma forma de ver aprovações pendentes e concluídas, e atualizações de pedido de aprovação.

**limiar de aprovação** : O número de mensagens de resposta de aprovação positivas necessárias para permitir um pedido de continuação do processamento.

**aprovador** : A pessoa que dará aprovação para o pedido de proceder à fase seguinte. Recebem mensagens de pedido de aprovação se o ADD-in MIM for Outlook for utilizado. Consulte também a inscrição para "aprovação de escalada".

**fluxo de atributos** : Isto define a direção de que os valores atribuem o fluxo entre o serviço MIM e outros sistemas externos.

**atividade de autenticação** : Uma atividade de fluxo de trabalho que valida a identidade de um utilizador. Por exemplo, o portão de reset da palavra-passe e o portão de autenticação do Smart Card. Consulte também a entrada para "QA gate" e "gate de bloqueio".

**desafio de autenticação** : Um diálogo que exige que o utilizador forneça uma resposta à autenticação ao MIM 2016. Por exemplo, perguntas para o utilizador responder para redefinir as suas palavras-passe.

**atividade de desafio de autenticação** : Uma atividade da Fundação Windows Workflow que é usada para configurar um desafio que é emitido a um utilizador para autenticar para MIM 2016.

**fluxo de trabalho de autorização** : Um fluxo de trabalho com atividades que devem ser concluídas antes do Pedido ser comprometido na base de dados. Exemplos são validação e aprovação de dados.
<br/>

## <a name="c"></a>C

**atributo de registo claro** : Este atributo apaga o registo associado a um fluxo de trabalho de autenticação. Por exemplo, num Desafio de Perguntas e Respostas, as respostas são armazenadas no MIM 2016 sob a forma de dados de registo. Quando a caixa de registo clara é verificada e um fluxo de trabalho é guardado, os dados de registo são eliminados, exigindo que os utilizadores voltem a registar-se.

**membro computado (ou membro)** : Um conjunto de recursos apenas de leitura calculado a partir da combinação de membros geridos manualmente e um filtro.

**conector** : Um objeto no espaço do conector que representa um objeto numa fonte de dados ligada e está atualmente ligado a um objeto no metaverso através de regras predefinidas. O metadisorário utiliza objetos de conector para sincronizar valores de atributos entre uma fonte de dados conectada e o metaverso.

**filtro de conector** : Uma regra que utiliza para evitar que os objetos do espaço do conector se liguem a objetos metaversos.

**espaço de conector** : Uma área de preparação que contém representações de objetos e atributos selecionados numa fonte de dados conectada. Um objeto de espaço de conector é um objeto no espaço do conector que é criado por uma importação de dados a partir da fonte de dados conectada ou usando regras dentro da MIM para criar novos objetos em diferentes fontes de dados conectadas. Estes objetos possuem valores de atributos que podem ser importados ou exportados de objetos correspondentes na fonte de dados ligada.

**contagem XPath** : Expressão XPath que devolve um valor numérico a ser renderizado dentro dos parênteses após o nome de exibição do recurso.

**membro baseado em critérios** : um conjunto de recursos apenas de leitura calculado a partir da combinação de membros estáticos do grupo e um filtro.

**Membro baseado em critérios** : Um grupo no qual a adesão ao grupo é determinada por um filtro. Consulte também a inscrição para "adesão estática".

**membro da floresta transversal** : Membro de um grupo de segurança cuja conta de utilizador numa floresta diferente da conta do grupo.

**cálculo do grupo florestal transversal** : Uma atividade fora da caixa que coloca em todos os membros florestais de um grupo do Conjunto Principal de Segurança Externa (FSP) associado à floresta em que o grupo reside.


**expressão personalizada** : A linguagem descritiva utilizada na definição de funções ou nos fluxos de atributos em modo avançado.
<br/>

## <a name="d"></a>D

**conjunto de destino (ou definição de recurso-alvo após pedido)** : Um conjunto no qual um recurso se move devido a um pedido que altera os atributos desse recurso.

atividade de validação de **grupo padrão** : Uma atividade de fluxo de trabalho fora da caixa que determina se um pedido de gestão de grupo violaria a configuração ou política do Diretório Ativo MIM 2016 ou do Ative Directory.

**desconexão** : Um objeto no espaço do conector que representa um objeto numa fonte de dados ligada e não está atualmente ligado a um objeto no metaverso.

**objetos de desconexão** : Existem três tipos de objetos de desconexão: desconexões, desconexões explícitas e desconexões filtradas.

**Nome do visor** : Um atributo de um recurso que aparece numa interface de utilizador para identificar esse recurso. Um valor utilizado num nome de exposição deve ser inequívoco e legível pelo homem. É importante fornecer o nome do visor se quiser utilizar o recurso em vários controlos do Portal MIM, como o picker de recursos.

**grupo** de distribuição : Uma recolha de recursos que é mais comummente utilizadores e outros grupos que podem ser enviados em simultâneo enviando e-mail para a caixa de correio para o grupo.

**configuração de domínio** : Um recurso de configuração utilizado para modelar domínios do Ative Directory.

**grupo local de domínio** : Um grupo com âmbito local de domínio é um grupo de Diretório Ativo que assegura recursos dentro de um determinado domínio, e que pode conter membros dessa floresta ou qualquer floresta de confiança.

**ficheiro drop** : Um ficheiro drop é um ficheiro de registo XML que representa uma exportação ou importação potencial ou ocorrente.

**valor de atributo dinâmico** : O valor de um atributo que é calculado com base em outros atributos. Por exemplo, um atributo de nome é calculado concedendo o nome e o apelido.

**grupo dinâmico** : Um grupo cuja adesão é automaticamente determinada e mantida atualizada pelo MIM 2016, garantindo que o grupo contém todos os recursos (como pessoas, grupos, computadores) que se enquadram em condições expressas pela utilização do XPath.
<br/>

## <a name="e"></a>E

**enumeração** : Uma lista de recursos devolvidos pelo Serviço MIM 2016.

**escalada** : Se uma aprovação não for concluída dentro do prazo especificado, a aprovação é agravada e os aprovadores adicionais são adicionados à aprovação.

**aprovação de escalada** : O utilizador que recebe as mensagens de pedido de aprovação se os aprovadores não responderem. Consulte também a inscrição para "aprovador".

**conector explícito** : Um objeto no espaço do conector que está ligado a um objeto no metaverso e não pode ser desligado por um filtro de conector. Um conector explícito só pode ser criado manualmente com o Joiner, e só pode ser desligado através do provisionamento ou da utilização do Joiner.

**desconexão explícito** : Um objeto no espaço do conector que não esteja ligado a um objeto no metaverso e só pode ser acompanhado através da utilização do Joiner. Um objeto torna-se um desconexão explícito desligando manualmente o objeto utilizando o Joiner.

**exportação** : Exportação é o processo de impulsionar alterações nas fontes de dados ligadas. Uma exportação é sempre uma operação delta baseada na última importação bem sucedida. A MIM processa as mudanças na exportação, mas não processa as alterações no espaço do conector até que as alterações tenham sido confirmadas por uma nova importação. Dependendo do tipo de agente de gestão que utiliza, as alterações implementadas por uma exportação podem estar ao nível do atributo, objeto ou valor.

**Fluxo de Atributos de Exportação** : O processo de tradução dos atributos de um objeto do metaverso para o espaço do conector. Este processo pode envolver mapeamentos de um para um, aplicando extensões de regras para modificar atributos ou definir valores de atributos estáticos. Os atributos exportados são encenados no espaço do conector para a próxima exportação para a fonte de dados conectada.

**Linguagem de marcação de afirmação extensível (XAML)** : Uma língua baseada em XML na qual as definições de fluxo de trabalho estão representadas.

**filtro de deteção do sistema externo** : Determina os recursos que identifica e filtra a partir de um diretório de origem com base numa determinada condição.

**Tipo de recurso** do sistema externo : Este é o tipo de recurso no sistema externo ao qual os recursos MIM 2016 estão ligados.

**bandeira de criação** de recursos do sistema externo : Um parâmetro de uma regra de sincronização para indicar se um recurso deve ser criado no espaço do conector se com base nos critérios de relacionamento tal recurso não existir no sistema externo. Consulte a bandeira de criação de recursos MIM 2016. <!-- Not sure what this is, should this be a term? -->

**âmbito do sistema externo** : Um parâmetro de uma regra de sincronização contendo um filtro que apresenta recursos no sistema externo ao qual a regra se aplica.
<br/>

## <a name="f"></a>F

**filtro** : Uma expressão que contenha condições de filtro. Um filtro corresponde a um recurso se cada uma das condições do filtro contidas no filtro corresponder ao recurso. Em MIM 2016, o filtro utiliza sintaxe XPath.

**desconexão filtrado** : Um objeto no espaço do conector que é impedido de ser ligado ou projetado a um objeto no metaverso com base nas regras do filtro do conector no agente de gestão associado.

**Agente de gestão FIM** : Um agente de gestão que sincroniza entre o Serviço MIM 2016 e o Serviço de Sincronização MIM 2016.

**FIM / MIM redefiniu** o serviço de clientes : Isto refere-se ao serviço de procuração que reside no computador do utilizador final que comunica com o servidor MIM 2016.

**Extensões de redefinição** de password FIM /MIM : Isto refere-se ao código que reside no computador do utilizador final que alarga a funcionalidade do Início de Sítido do Windows para incluir o Reset de Palavra-Passe de Autosserviço.

Bandeira de criação de **recursos FIM /MIM** : Um parâmetro de uma regra de sincronização para indicar se um recurso deve ser criado na base de dados MIM 2016 se com base nos critérios de relacionamento os recursos não existirem. Consulte também a inscrição para "bandeira de criação de recursos do sistema externo".

**função** : Um componente que pode ser incluído numa regra de sincronização ou numa definição de fluxo de trabalho para processar valores de dados.
<br/>

## <a name="g"></a>G

**portão** : Uma atividade de fluxo de trabalho utilizada na fase de autenticação do processamento de pedidos. Consulte também a entrada para "QA gate" e "gate de bloqueio".

**nidificação em grupo** : Um campo de definição de grupo que especifica se o grupo contém outros grupos como membros do grupo atual.

**âmbito** de grupo : Um campo de definição de grupo, um ou "local", "global", ou "universal". Para mais informações, consulte o âmbito do grupo Ative Directory.
<br/>

## <a name="i"></a>I

**importação** : O processo de deslocação de objetos de origem de dados ligados para o espaço do conector para fins de criação, modificação, eliminação ou verificação. Uma importação pode ser uma operação completa ou delta. Para uma importação completa, a MIM solicita todos os objetos designados da fonte de dados ligada e elimina todos os objetos de encenação para os quais um objeto correspondente não tenha sido recebido durante esta importação. Como resultado, este passo de perfil de execução é útil para limpar os objetos de preparação no espaço do conector. Os objetos que foram recebidos da fonte de dados ligada são encenados no espaço do conector. Para que a importação delta forneça os resultados pretendidos, a fonte de dados ligada deve implementar uma forma de marca de água. A fonte de dados ligada utiliza a marca de água para indicar quando ocorreram as alterações mais recentes num objeto. A MIM lê a marca de água para determinar o que incluir na importação delta. Um exemplo é o Ative Directory USN.

**Import Attribute Flow (IAF)** : O fluxo de atributos de importação é o processo de importação de um atributo do espaço do conector para o metaverso. Este processo pode envolver a aplicação de mapeamentos de atributos um-para-um, usando extensões de regras para modificar atributos ou definir atributos estáticos.

**URL de imagem** : URL para um ficheiro de imagem a ser renderizado no Portal MIM 2016 UI.

**fluxo inicial** : Um fluxo inicial é um fluxo de valor de atributo que só é aplicado uma vez quando o recurso é criado pela primeira vez. Ou seja, uma palavra-passe inicial só é criada quando se cria uma conta pela primeira vez.

**fluxo de trabalho interativo** : Um fluxo de trabalho que requer resposta do utilizador que solicita uma alteração, como por exemplo para a realização de verificações de autenticação adicionais.
<br/>

## <a name="j"></a>J

**juntar-** A junção é o processo de ligação de um objeto de espaço de conector com um objeto metaverso existente. Os valores de atributos fluem apenas entre objetos ligados.

**aderir ao pedido de grupo** : Um pedido para adicionar um utilizador a um grupo.
<br/>

## <a name="l"></a>L

**bloqueio** : Uma configuração de configuração num recurso de pessoa na base de dados MIM 2016 que restringe essa pessoa de autenticar para MIM 2016 ou fazer reset de senha.

**portão de bloqueio** : Uma atividade de fluxo de trabalho na fase de autenticação do processamento de pedidos para bloquear um utilizador que não tenha autenticado. Consulte também a entrada para "lockout" e para "QA gate".

**limiar de bloqueio** : Trata-se de um controlo inteiro que especifica o número de vezes que um utilizador pode não completar o fluxo de trabalho de autenticação antes de ser bloqueado para a duração do bloqueio.A definição predefinição para isto é 3. O limite inferior é 0 e o limite superior é 99.

**duração do bloqueio** : Trata-se de um controlo inteiro que especifica a duração em minutos para que o utilizador esteja bloqueado depois de atingir o Limiar de Bloqueio.A definição predefinida para isto é de 15 minutos.O limite inferior para esta regulação é 1 e o limite superior é 9999.O limite superior permite ao administrador fixar o limite superior para maior do que um dia.

**contagem de limiares de bloqueio antes do bloqueio permanente** : Este é um controlo inteiro que permite ao administrador configurar um valor numérico para o número de vezes que um utilizador pode atingir o Limiar de Bloqueio antes de ser permanentemente bloqueado.  O bloqueio permanente implica que o utilizador deve ser desbloqueado pelo administrador do sistema. Por predefinição, este é definido para 3.O intervalo para esta definição é entre 1 e 99.
<br/>

## <a name="m"></a>M

**agente de gestão** : Um agente de gestão (MA) liga uma fonte de dados conectada específica ao metadiretório. É responsável pela transferência de dados da fonte de dados conectada para a MIM e as regras que determinariam a elegibilidade dos dados de identidade para participar no MIM e em quaisquer outras fontes de dados ligadas. Quando os dados na metadiscória são modificados, o agente de gestão pode exportar os dados para as fontes de dados conectadas para manter a fonte de dados ligada sincronizada com os dados na MIM. Um agente de gestão é usado em vez de ter um conector baseado em agente.

**Regra da Política de Gestão (MPR)** : As Regras de Política de Gestão (MPR) fornecem um mecanismo para modelar as regras de processamento de negócios para pedidos de entrada no servidor MIM 2016. Controlam as permissões de solicitação de operações nos recursos do MIM 2016, juntamente com os fluxos de trabalho que são desencadeados por estes pedidos.
   
   Existem dois tipos de MPRs:
   
- Solicitar MPRs: Conceder permissões e executar fluxos de trabalho (invocados antes de ser realizada uma operação solicitada).
- Definir MPRs de transição: Executar apenas fluxos de trabalho (reação a uma alteração de estado aplicada).
   
  Os principais objetos de design para MPRs são:
   
- Permissões de modelação
- Modelos de mapeamentos de fluxos de trabalho
- Transições de modelação
- Definições reflexivas de modelação
- Modelação de acesso ao utilizador não autenticado
- Modelar políticas temporais
- Permissões de filtragem de modelação

**Membro gerido manualmente** : A adesão ao grupo ou conjunto que consiste numa lista manualmente selecionada de utilizadores, grupos ou outros recursos.

**metaverse** : A loja de dados central utilizada pela MIM para conter a informação de identidade agregada de múltiplas fontes de dados ligadas, proporcionando uma única visão global e integrada de todos os objetos combinados. O metaverso não deve ser usado como tabela ou visualização de dados de identidade agregados para qualquer aplicação que não a MIM, uma vez que a corrupção pode resultar.

**caixa de correio monitorizada** : Uma caixa de correio que o Serviço MIM 2016 monitoriza para receber aprovação e Solicite e-mails do Add-in MIM para o Outlook.
<br/>

## <a name="n"></a>N

**atividade de notificação** : Uma atividade de fluxo de trabalho dentro da fase de ação do processamento de pedidos em que a MIM 2016 envia e-mails a um ou mais utilizadores para os notificar do pedido.

**mensagem de notificação** : Uma mensagem de correio eletrónico enviada por uma atividade de notificação. Consulte também a inscrição para "atividade de notificação".
<br/>

## <a name="o"></a>O

**ObjectID (ResourceID)** : Um atributo que contém um identificador globalmente único (GUID) atribuído pela MIM 2016 a cada recurso quando é criado. Isto também é conhecido como ID de recurso.

**identificador** de objetos : Sequência de números utilizados como identificador para um campo num certificado digital X.509, ou para um tipo de atributo ou classe de objeto num serviço de diretório baseado em LDAP. Os identificadores de objetos são normalmente atribuídos por fornecedores de software e organismos de padrões.

**Tipo de funcionamento** : O tipo de funcionamento é solicitado sobre o recurso gerido pela MIM 2016 através do serviço Web. Isto inclui criar e eliminar recursos, e ler e modificar os atributos de recursos. Além disso, as operações de Add/Remove permitem-lhe aplicar mais controlo à operação de modificação para controlar apenas a adição de valores a atributos ou a sua remoção.

**operador** : Um elemento de um filtro que especifica uma comparação ou outra relação entre os valores de dados.

**conjunto de origem (ou definição de recurso-alvo antes do pedido)** : um conjunto em que um recurso pertencia antes de uma alteração dos atributos desse recurso.
<br/>

## <a name="p"></a>P

**parâmetro** : Ao fornecer novos recursos, por vezes é possível fornecer valores de atributos de uma fonte externa, como um utilizador. Um valor de atributo é passado como um parâmetro para ser capaz de criar com sucesso um novo recurso.

**partição** : Um volume lógico de dados no espaço do conector. Um agente de gestão pode criar uma ou mais divisórias para dividir logicamente os dados em agrupamentos lógicos separados. Cada volume de dados é processado individualmente durante a sincronização.

**redefinição da palavra-passe** : Um procedimento pelo qual a palavra-passe de um utilizador pode ser alterada para um valor conhecido, em situações em que o utilizador tenha esquecido ou perdido a sua palavra-passe. Consulte também a inscrição para "registo".

**fase** : Cada pedido de criação, atualização ou eliminação de recursos é processado através de três fases de fluxo de trabalho. Na fase de autenticação, podem ser realizadas verificações de autenticação adicionais do utilizador que solicita. Na fase de autorização, são recolhidas as aprovações necessárias. Na fase de ação, as atividades são realizadas após o pedido de alteração do recurso ter sido comprometido.

**objetos reservados** : Objetos no espaço do conector que representam um único nível da hierarquia da fonte de dados ligada. Por exemplo, se pretender sincronizar objetos com uma floresta ative de diretório, tem de importar os recipientes que compõem o caminho para os objetos do Ative Directory. No exemplo, CN=MikeDan,OU=Users,DC=Microsoft,DC=Com, os objetos de espaço reservado seriam criados para DC=Com e DC=Microsoft,DC=Com. Além disso, um objeto reservado pode representar um objeto na fonte de dados ligada à qual se refere um valor de atributo de referência importado (por exemplo, o objeto a que o atributo do gestor se refere num objeto do Utilizador). Os objetos reservados não contêm valores de atributos e não podem ser ligados ao metaverso.

**Gestão de Políticas** : A Gestão de Políticas na MIM 2016 é possível através de uma consola baseada em Sharepoint para autoria e execução de políticas. Fluxos de trabalho extensíveis baseados na Fundação Windows workflow permitem que os utilizadores definam, automatizem e apliquem políticas de gestão de identidade. A Gestão de Políticas inclui também a sincronização heterogénea de identidade e a consistência que é conseguida através da integração de um vasto leque de sistemas operativos de rede, e-mail, base de dados, diretório, aplicação e acesso a ficheiros planos.

**atualização da política (ou executada na atualização da política)** : Se um fluxo de trabalho deve ser reexecutado como um efeito de uma alteração nos conjuntos ou MPRs que se referem a ela, o pavilhão de atualização de política é utilizado para indicar isto.

**precedência** : Uma ordem de regras de sincronização.

**conjunto principal** : Um conjunto utilizado na Regra da Política de Gestão para especificar o conjunto de recursos (normalmente utilizadores) que iniciam a avaliação da Regra da Política de Gestão.

**principal conjunto relativo ao recurso** : Esta é uma propriedade reflexiva. O seu valor é definido em termos de uma das propriedades de Recursos. É usado para definir regras dinâmicas de política de gestão cujas condições são avaliadas no contexto de cada recurso-alvo sendo processado.

**projeção** : O processo de criação de um objeto no metaverso com base nas regras de projeção e, em seguida, ligando automaticamente esse objeto a um objeto existente no espaço do conector.

**disposição** : O processo de criação, renomeação e desprovisionamento de objetos em espaços de conector pré-determinados com base em alterações a um objeto no metaverso. As regras de provisionamento podem ser criadas para serem chamadas sempre que um objeto metaverso for modificado. Estas regras podem executar ações de nível de objeto, tais como a criação de um novo objeto de espaço de conector ou a desconexão de objetos espaciais existentes que estão ligados ao objeto metaverso.
<br/>

## <a name="q"></a>Q

**Portão QA** : Uma atividade de fluxo de trabalho numa fase de autenticação, na qual o utilizador que solicita deve fornecer respostas a uma ou mais perguntas pré-determinadas. Esta atividade é normalmente utilizada no reset da palavra-passe, para desafiar o utilizador a provar a sua identidade, fornecendo ao utilizador uma seleção de perguntas pré-determinadas para as quais apenas esse utilizador saberia, para o qual o utilizador deve fornecer a resposta correta. Consulte também a entrada para "portão de bloqueio".

**Desafio QA** : Um desafio que exige que o utilizador responda a uma série de perguntas de forma a autenticar a MIM 2016.
<br/>

## <a name="r"></a>R

**Definições de palavra-passe aleatórias** : Uma definição que determina o número de caracteres necessários para definir uma palavra-passe no diretório externo.

**tipo de atributo de referência** : Um tipo de atributo em que os valores do atributo são o ObjectID (identificadores globalmente únicos) atribuem valores de outros recursos no MIM 2016.

**integridade referencial** : Uma restrição no MIM 2016 em que um atributo de referência não pode ter como valor um ObjectID de um recurso que foi eliminado.

**registo** : Um procedimento para configurar a palavra-passe de autosserviço reiniciada para um utilizador. Consulte também a entrada para "QA gate".

**re-inscrição** : A atualização do registo para um desafio de autenticação no MIM 2016, normalmente exigido após uma alteração da política administrativa para o registo de redefinição de password.

**criação** de relacionamentos : Configurar bandeiras de uma regra de sincronização que determina se os recursos devem ser criados automaticamente no MIM 2016, ou no sistema externo, se os recursos estiverem ausentes.

**critérios** de relacionamento : Definição de uma regra de sincronização que é usada para combinar recursos no servidor MIM e recursos em sistemas externos.

**Terminação da relação** : Indica se os recursos relacionados noutros sistemas externos devem ser desligados (e possivelmente eliminados) quando a regra de sincronização já não se aplica.

**gestão de pedidos** : A capacidade de um utilizador interagir e gerir os pedidos apresentados e os fluxos de trabalho associados.

**regra da política de gestão de pedidos (RMPR)** : Um tipo de regra de política de gestão que é avaliado e aplicado contra pedidos de entrada para realizar operações. Os RMPRS são usados principalmente para definições de política de acesso a autores na MIM. Por outras palavras, a resposta à forma como o pedido é tratado. Quando configurar um RMPR, o solicitador está no Conjunto projetado para executar uma operação.

   A arquitetura MIM define seis operações diferentes para as quais um RMPR pode ser definido para:
   
- Criar um recurso.
- Apagar um recurso.
- Leia um recurso.
- Adicione um valor a um atributo multivalorizado.
- Remova um valor de um atributo multivalorizado.
- Modifique um atributo de valor único.
   
  Quando definir um RMPR, deve selecionar pelo menos uma destas seis operações. As operações são sempre definidas no contexto do solicitador. Cada condição requer a definição de um alvo. Uma operação que é aplicada a um alvo pode resultar numa transição estatal do recurso alvo. Um RMPR é sempre invocado antes de ser realizada uma operação solicitada. Para caracterizar eficazmente o alvo da sua condição, é necessário configurar dois estados diferentes:
   
- Definição de recurso alvo Antes do pedido: O estado do alvo antes da aplicação do pedido.
- Definição de recurso alvo Após pedido: O estado do alvo após a aplicação do pedido.
   
  Se precisa de definir ambos os estados depende das operações que o seu RMPR está definido para fazer. Numa operação Create, o recurso solicitado não tem estado inicial. Portanto, só precisa de configurar a definição de recurso alvo após pedido" para uma operação Create.
   
  Uma operação de leitura ou eliminação não provoca uma transição de estado. Para estes dois tipos de operações, só é necessário especificar a Definição de Recursos-Alvo antes do Pedido.
   
  Para uma operação modificada ou todas as outras combinações de operações, é necessário configurar ambos os Estados, que podem ter os mesmos valores se não houver uma transição do Estado.
   
  Pode expressar os recursos relacionados com o solicitador (como o objeto de utilizador do próprio solicitador, o gestor de um utilizador-alvo ou o proprietário de um grupo alvo).
   
  A forma mais simples de uma resposta a uma condição é conceder permissões para realizar a operação solicitada. Além de conceder permissões, também pode definir outras operações como resposta a uma condição num RMPR. Na arquitetura MIM, estas operações são definidas sob a forma de fluxos de trabalho. No momento em que um determinado RMPR é processado, o sistema pode não ter informações suficientes para conceder permissão. Neste caso, pode definir etapas adicionais de autenticação e autorização no seu RMPR que se aplicam à pessoa que efetua um determinado pedido. Por exemplo, para conceder permissão para realizar a operação solicitada, poderá necessitar de interação manual de um utilizador para aprovar a operação.
   
  A seguinte figura descreve a arquitetura completa de um RMPR:
   
  ![Arquitetura RMPR](media/mim-2016-sp1-terms/8f5054080d3f8bd9c73d93a5e3ed51f9.gif)
   
  Quando um novo objeto de pedido é criado em MIM, o sistema consulta os RMPRs configurados para objetos correspondentes, comparando as condições de pedido com as condições configuradas nos RMPRs de gestão. Se estiverem localizados RMPRs correspondentes, são aplicados ao objeto de pedido em fila. O seguinte número descreve este processo:
   
  ![Os RMPRs correspondentes estão localizados e aplicados no objeto de pedido em fila](media/mim-2016-sp1-terms/65bdb65aaffa55a90707ff6f7b13632d.gif)
   
  No Portal MIM, as permissões de operações devem ser explicitamente concedidas. Por outras palavras, a menos que seja concedido por um RMPR, todas as operações de recursos são negadas. Cada objeto de pedido requer pelo menos um RMPR que concede permissão para realizar a operação solicitada num alvo.

**objeto de pedido** : Quando um utilizador executa uma tarefa no Portal MIM ou no Add-in MIM para o Outlook, é representado como um objeto de pedido. Os objetos de pedido representam um mecanismo de reporte conveniente para as atividades no seu sistema.

   Cada objeto de pedido tem estes componentes:
   
- Solicitador: Um recurso que pede para executar uma operação.
- Operação: A ação que o solicitador quer realizar.
- Alvo: O recurso que é o alvo da operação solicitada.
   
  Logicamente, um objeto de pedido é uma implementação da seguinte declaração:
   
  `The requester attempts to perform the following operation on this target...`
      
  A figura a seguir descreve a arquitetura geral de um objeto de pedido:
   
  ![Arquitetura de objeto pedido](media/mim-2016-sp1-terms/788e9b3a4fcddb78ff4a05dcb5e61192.gif)
   
  Cada objeto de pedido tem uma propriedade de estado para indicar o estado de processamento. Os pedidos de processamento podem requerer interação manual para completar um pedido. Por exemplo, o proprietário de um grupo pode ser obrigado a aprovar manualmente o pedido de outro utilizador para se juntar a um grupo. Além de uma interação manual, também pode configurar a MIM para processar automaticamente um pedido específico sem a necessidade de interação humana.
   
**Modelo de processamento** de pedidos : O modelo de processamento de pedidos na MIM é composto por três fases principais:

- Fase 1: Autenticação
- Fase 2: Autorização
- Fase 3: Ação
   
  Os fluxos de trabalho, cada um dos quais contêm uma ou mais atividades, podem ser anexados a cada uma destas fases e executados no contexto da execução de um único pedido. Um pedido pode ser iniciado a partir de uma única chamada de utilizador para um dos pontos finais do Serviço Web ou através de um utilizador que crie um pedido no Portal MIM.
   
  A seguinte figura mostra a relação dos componentes de processamento do pedido:
   
  ![Relação dos componentes de processamento de pedidos](media/mim-2016-sp1-terms/0e57445d8b9a3216c31add80eb26493f.gif)
   
  Os pedidos são processados na seguinte ordem:
   
- Pedido Criação de Objeto: MIM 2016 cria um objeto de pedido em resposta a uma chamada para um dos pontos finais do Serviço Web ou devido a um pedido iniciado através do Portal MIM.
   
- Avaliação MPR: Os direitos do solicitador a solicitar a ação são validados e o cálculo dos fluxos de trabalho aplicáveis é realizado. O pedido é verificado contra mapeamentos de quaisquer objetos MPR. Para mapear para um MPR, todos os campos aplicáveis do MPR para a operação solicitada têm de coincidir. Isto inclui o solicitador, operação, recurso alvo e atributos. Se todas estas condições, incluindo os atributos que estão a ser afetados, forem verdadeiras para um pedido de entrada, então o MPR apropriado é igual ao pedido. Um pedido deve mapear para pelo menos um MPR que conceda a permissão como parte da sua definição. Se isso for verdade, o pedido passa através da fase de verificação de permissões do processamento do pedido. Se isso não for verdade, o pedido falha. O sistema também determina as transições definidas que fazem parte do pedido e localiza todos os MPRs relacionados com base na transição.
   
- Autenticação: A MIM 2016 executa fluxos de trabalho de autenticação um de cada vez, numa ordem não determinística para confirmar a identidade do solicitador.
   
- Autorização: A MIM 2016 confirma a autorização do solicitador para a realização da operação solicitada sobre o recurso especificado no pedido. Todos os fluxos de trabalho de autorização dependente são executados em paralelo, mas um pedido não é comprometido com a Loja de Objetos MIM, a menos que todos os fluxos de trabalho tenham sido concluídos e todos tenham sido bem sucedidos.
   
- Processamento: MIM 2016 realiza a operação solicitada na Loja de Aplicações MIM.
   
- Ação: MIM 2016 executa todos os processos que deverão ocorrer por causa da operação solicitada. Todos os fluxos de trabalho de ação são executados em paralelo. As operações de leitura não têm quaisquer fluxos de trabalho aplicados ao seu processamento. Isto inclui os fluxos de trabalho configurados no RMPR, bem como os fluxos de trabalho nos MPRs baseados na transição definida.
   
  >[!NOTE]
  >Os pedidos que são iniciados pela Conta de Sincronização contornam todos os fluxos de trabalho de autenticação e autorização que lhes seriam aplicáveis. Todos os fluxos de trabalho de ação aplicáveis são aplicados.

**requestor** : A identidade do utilizador ou serviço que submeteu um pedido à MIM 2016.

**âmbito de solicitação** : Uma recolha configurada de utilizadores que podem apresentar um pedido. Pode ser "todos", ou um conjunto específico de utilizadores definido por um filtro.

**recurso** : Um caso de um certo tipo de recurso no MIM 2016. Cada recurso é identificado exclusivamente pelo seu atributo ObjectID (ResourceID).

**Configuração do display de controlo de recursos (RCDC)** : Os RCDCs são recursos de configuração que são utilizados para tornar a UI no Controlo de Recursos (RC) para a autoria de um tipo específico de recurso em MIM 2016.

**conjunto atual de recursos** : Parte das regras de política de gestão (MPR) definição de condição. A recolha de recursos-alvo no momento da ausição do pedido. Aplica-se para ler, eliminar e modificar tipos de operação.

**conjunto final de recursos** : Parte da definição de condição nas regras de política de gestão. A recolha de recursos-alvo após o pedido é processado. Aplica-se apenas para criar e modificar tipos de operações.

**hierarquia de recursos** : Num serviço de diretório, a hierarquia de uma entrada de recursos é a recolha de entradas de diretório entre a base de um contexto de nomeação e essa entrada de recursos.

**âmbito** de recursos : Um conjunto de recursos sobre os quais um pedido pode ser apresentado.

**tipo** de recurso : Parte de um esquema que define a representação de um recurso no MIM 2016.

**Mapeamento do tipo de recurso** : Uma relação entre um tipo de recurso usado para representar um recurso no MIM 2016 e uma classe de recursos usada para representar esse recurso no metaverso.

**função** : Um principal de segurança atribuído pela organização usado para gerir os direitos de acesso.

**extensão de regras** : Uma extensão de regras é uma biblioteca de ligações dinâmicas (.dll) que contém um conjunto definido de regras para a gestão de dados. Pode utilizar extensões de regras durante as sincronizações para prolongar a funcionalidade. Por exemplo, pode utilizar uma extensão de regras para combinar dados de dois atributos de origem e fluí-los para um atributo-alvo (por exemplo, `sn` e `givenName` para `displayName` ).

**história de execução** : Um conjunto de estatísticas que mostram os resultados de uma única execução de um agente de gestão.

**perfil de execução** : Um perfil de execução representa um conjunto de passos que especificam como executar um agente de gestão e configurações de configuração que determinam como um agente de gestão funciona. Um agente de gestão pode ter vários perfis de execução, que são armazenados com o agente de gestão. Um perfil de execução é composto por pelo menos um passo de perfil de execução.
<br/>

## <a name="s"></a>S

**pasta de pesquisa** : Consulte a entrada para "pasta de pesquisa de aprovações".

**âmbito de pesquisa** : Especifica as propriedades para um determinado contexto de pesquisa que um utilizador pode realizar a partir do Portal MIM 2016. Por exemplo, um utilizador poderia selecionar um âmbito de pesquisa de uma lista de "Todos os Utilizadores", "Todas as Listas de Distribuição", "As minhas aprovações pendentes", e os resultados da pesquisa seriam limitados a itens que satisfazem esses critérios, além de quaisquer termos de pesquisa especificados pelo utilizador.

**descritor de segurança** : Estrutura e dados associados que contenham as informações de segurança para um recurso securável. Um descritor de segurança identifica o proprietário do recurso e o grupo primário. Também pode conter um DACL que controla o acesso ao recurso, e um SACL que controla o registo de tentativas de acesso ao recurso.

**principal de segurança** : Uma identidade utilizada para a gestão da segurança, como uma conta de utilizador, que pode autenticar num serviço.

**símbolo de segurança** : Elemento de protocolo que transfere informações de autenticação e autorização, com base numa credencial. Nos protocolos de serviços web, um símbolo de segurança é representado como um elemento XML num cabeçalho SOAP, tal como definido pela WS-Security.

**serviço de token de segurança** : Um serviço que implementa o protocolo WS-Trust para gerir a confiança entre clientes e serviços com base na troca de fichas de segurança.

**fluxo de trabalho sequencial** : Todos os fluxos de trabalho em MIM 2016 são derivados do fluxo de trabalho sequencial da fundação Windows Workflow. Inclui vários fluxos de trabalho numa ordem sequencial.

**conta de serviço** : Uma conta Windows atribuída para ser utilizada por um serviço Windows, em vez de ser utilizada por um utilizador para iniciar sessão num sistema informático. Representa a conta do sistema da MIM.

**Conjunto** : Uma coleção de recursos nomeada. Normalmente, os conjuntos são usados para organizar recursos baseados em regras. A adesão a um Conjunto é gerida manualmente ou baseada em critérios. Isto significa que pode adicionar manualmente recursos a um Conjunto e pode definir critérios que adicionam automaticamente recursos a um Conjunto com base numa declaração de filtro. Quando um recurso preenche os critérios do filtro, é automaticamente adicionado ao Conjunto relacionado.

**Definir Regra da Política de Gestão de Transição (TMPR)** : Uma regra de política de gestão que é aplicada sobre alterações à adesão a um conjunto. Os TMPRs aplicam fluxos de trabalho de ação quer quando a transição de objetos para dentro ou para fora de um determinado conjunto no MPR.

   Existem dois tipos de TMPRs:
   
- Transição em: Um recurso torna-se membro do conjunto de transição.
- Transição para fora: Um recurso deixa o conjunto de transição.
   
   >[!NOTE]
   >Quando um conjunto de transição é eliminado, o sistema trata a eliminação como um evento de transição para os objetos afetados.
      
  A resposta é uma reação a uma mudança de estado aplicada. Quando o MPR relacionado é invocado, a condição já foi aplicada. Isto significa que os recursos afetados já transitaram para dentro ou para fora de um conjunto de transição. Para os TMPRs, o objetivo da resposta não é definir a reação a uma operação solicitada, mas sim definir a resposta a uma operação aplicada. Por outras palavras, para um MPR baseado na transição, é irrelevante a forma como um Estado foi alcançado. O que é relevante são as consequências de uma mudança de Estado.
   
  Ao configurar um MPR baseado em transição definido em MIM, tem de especificar as seguintes três definições:
   
- Conjunto de transição
- Tipo de transição
- Fluxos de trabalho de políticas
   
  Os fluxos de trabalho políticos são definições dos processos que precisam de ser invocados em resposta à mudança do Estado. Os casos de utilização mais comuns para os PMR estatais são a concessão ou revogação de direitos e o fornecimento e desprovisionamento em fontes de dados externas.
   
  A figura a seguir descreve a arquitetura completa de um MPR baseado em transição definido:

  ![Definir arquitetura TMPR](media/mim-2016-sp1-terms/e12154d35b48cec676f160930ff33662.gif)
  
  Os MPRs baseados na transição são ativados por pedidos. Quando um pedido é processado e aprovado por um RMPR, o serviço MIM também determina se um pedido aprovado resulta numa transição do Estado e se existe um MPR baseado na transição do Estado que lida com a mudança de Estado.
  
  O seguinte valor descreve a relação entre um pedido e um MPR baseado em transição definido:
  
  ![Relação entre um pedido e um conjunto de TMPR](media/mim-2016-sp1-terms/426be3a3a5380720fa1eaa3e7df360df.gif)

**SID** : Um valor único usado para identificar uma conta de utilizador, conta de grupo ou sessão de início de sessão.

**SABÃO** : Um protocolo para o intercâmbio de informações estruturadas entre componentes de software.

**sincronização** : O processo de manter os dados selecionados em várias fontes de dados de acordo. A sincronização representa operações em objetos exclusivamente dentro da MIM. A sincronização pode ser uma operação em todo o conjunto de dados para os quais é definido num agente de gestão ou numa operação delta de acordo com as alterações desde a última operação conhecida. O passo de perfil de sincronização define os processos de sincronização de entrada e saída.

   O passo de perfil de sincronização tem dois subtipos:
   
- Sincronização delta
- Sincronização completa
   
  Durante a sincronização delta, o MIM processa apenas objetos importados, que são os objetos de encenação que são sinalizados como importadores pendentes. Este passo de perfil de execução é útil para processar apenas os objetos que têm alterações pendentes, mas não foram processados durante uma execução de sincronização anterior.
   
  A sincronização delta é usada em dois perfis de execução predefinidos, e comporta-se de forma ligeiramente diferente em cada um deles. O primeiro perfil de execução é a Delta Synchronization, onde não é realizada nenhuma importação de qualquer fonte ligada, mas todos os objetos no espaço do conector são avaliados, e quaisquer objetos com alterações pendentes são processados. O segundo perfil de execução é a Delta Import e a Delta Synchronization combinadas. Este perfil de execução importa apenas os objetos e atributos da fonte de dados conectada cujos valores mudaram desde a última vez que o agente de gestão foi executado. As regras dos agentes de gestão são então reaplicadas apenas a objetos que tenham alterações pendentes da importação delta. Não são avaliados objetos sem alterações pendentes dessa importação delta.
   
  Durante a sincronização completa, a MIM avalia e aplica regras de sincronização a todos os objetos de preparação num espaço de conector. A sincronização total deve ser iniciada sempre que tenham sido aplicadas alterações às regras de um determinado ambiente. Dependendo do número de objetos no espaço do conector, este pode ser um funcionamento intensivo de tempo e recursos, pelo que devem ser evitadas alterações frequentes às regras de sincronização no seu ambiente de produção.

**filtro de sincronização** : Um filtro para evitar que os recursos no metaverso sejam transferidos para a base de dados MIM 2016.

**regra de sincronização** : Regra para a informação de recursos fluindo entre o servidor MIM (incluindo o motor de sincronização MIM) e o sistema externo ligado.
<br/>

## <a name="t"></a>T

**política temporal** : Um SET de transição definido MPR que está ligado a um conjunto temporal. A política é aplicada na passagem do tempo, à medida que os objetos transitam para dentro e para fora do conjunto com base na definição do Conjunto Temporal.

**Conjunto temporal** : Um tipo de objeto definido que se baseia em datas relativas. Os Conjuntos Temporais fornecem um mecanismo que pode automatizar totalmente o processo de transição para dentro ou para fora de um conjunto baseado na passagem do tempo. Por exemplo, um Conjunto Temporal pode ser definido para todos os grupos que expiram uma semana a partir de hoje. O sistema avalia automaticamente os objetos do sistema e adiciona-os a este conjunto diariamente. Outros exemplos permitem definições dinâmicas de uma referência temporal, como um filtro que se baseia em "x dias a partir de hoje".

**Evento cronometrado** : Um evento de transição que ocorre após um intervalo de tempo configurado ter decorrido ou uma data e hora específicas ter sido atingido.

**prazo** : Um período de tempo em que o MIM 2016 aguarda respostas de aprovação até que uma atividade seja escalada.

**conjunto de transição** : Um conjunto que é usado numa definição de regra de política de gestão de transição definida. A política é aplicada às alterações na adesão definida, que podem ser objetos que entram ou saem do conjunto, dependendo da configuração do TMPR.
<br/>

## <a name="u"></a>U

**grupo desbloqueado** : Grupo em que a adesão ao grupo pode ser alterada por utilizadores que não o proprietário do grupo.

**grupo universal** : Um grupo com âmbito universal é um grupo de Diretório Ativo que pode conter membros de uma determinada floresta. Um grupo universal pode ser atribuído permissões em qualquer domínio ou floresta. As listas de distribuição normalmente têm âmbito universal.Um grupo de segurança com âmbito universal pode garantir recursos dentro da mesma floresta.

**pedido de atualização** : Um pedido para alterar os atributos de um recurso.

**palavra-chave de utilização** : É utilizada uma palavra-chave de utilização para determinar quais os âmbitos de pesquisa apresentados para uma página específica no portal UI. Cada página de visualização da lista no UI especifica palavras-chave de uso zero ou mais, e o UI para essa página inclui todos os âmbitos de pesquisa que contêm palavras-chave correspondentes. Ao autorizar os âmbitos de pesquisa, os clientes podem especificar zero ou mais palavras-chave por âmbito de pesquisa, para personalizar quais os âmbitos de pesquisa que aparecem para uma determinada página na UI. Também é usado para determinar que recurso de página inicial e recurso de barra de navegação é mostrado a que conjunto de utilizadores. Também é usado na gestão de esquemas para proteger e rotular elementos de esquema que são necessários por vários componentes da MIM.
<br/>

## <a name="w"></a>W

**portal web** : Uma interface de utilizador implementada por uma aplicação de software através de um componente de um servidor web, como o IIS.

**serviço web** : Uma interface de protocolo para um serviço implementado usando um protocolo baseado em HTTP.

**fluxo de trabalho** : Um fluxo de trabalho é um conjunto de unidades elementares chamadas atividades que são armazenadas como um modelo que descreve um processo real. Os fluxos de trabalho fornecem uma forma de descrever a ordem e as relações dependentes entre os itens de trabalho. Este trabalho passa pelo modelo do início ao fim, e as atividades podem ser realizadas por pessoas ou por funções do sistema. Por outras palavras, se uma resposta a um pedido requer um processamento complexo, os passos são encapsulados num objeto de fluxo de trabalho. Os fluxos de trabalho são componentes opcionais e intimamente ligados a MPRs. Os fluxos de trabalho definem uma atividade ou atividades que devem ocorrer durante o processamento de um MPR. A MIM instala vários fluxos de trabalho predefinidos que podem ser usados como é ou como base para um fluxo de trabalho personalizado.

   Exemplos para atividades de fluxo de trabalho são:

- Envio de uma mensagem de correio eletrónico automatizado para solicitar aprovação.
- Restringir os atributos que um utilizador pode ver durante uma pesquisa personalizada.
- Validação de um novo grupo contra as diretrizes AD DS ou MIM.
- Adicionar ou remover o objeto do âmbito de uma regra de sincronização.
   
  Para responder a todos os requisitos de processamento no seu ambiente, a arquitetura MIM define três tipos de fluxos de trabalho:
   
- Autenticação: Realiza validação adicional de identidade de utilizador antes de continuar com o pedido
- Autorização: Valida um pedido através de uma sequência de atividades como a obtenção de aprovação externa necessária antes de processar um pedido.
- Ação: Processa quaisquer outras atividades após o pedido original ter sido concluído com sucesso.
   
  Estes três fluxos de trabalho fazem parte do modelo de processamento de pedidos.

**definição de fluxo de trabalho** : A definição de fluxo de trabalho é armazenada no formato XOML definido pela Windows Workflow Foundation (WF). Isto define as atividades, os parâmetros para as atividades, e a ordem pela qual devem ser executados.

**Workflow Designer** : A experiência de tempo de design para a construção de fluxos de trabalho.

**hospedeiro de fluxo de trabalho** : O componente do servidor que lida com o funcionamento dos fluxos de trabalho. Na MIM 2016, o Serviço MIM 2016 é o anfitrião do fluxo de trabalho.

**caso de fluxo de trabalho** : Uma instância em execução de uma definição de fluxo de trabalho como efeito de um pedido.

**gestão de fluxos de trabalho** : Uma funcionalidade MIM 2016 que trata da conceção de fluxos de trabalho, execução e gestão dos mesmos. A gestão do fluxo de trabalho é constituída pelo Workflow Designer, pela gestão de pedidos e pelo anfitrião do fluxo de trabalho.
