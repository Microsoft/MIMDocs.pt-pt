---
title: Guia do Microsoft BHOLD Suite conceitos | Microsoft Docs
description: "Comece a trabalhar com os componentes do MIM 2016 ao instalar e configurar o Serviço de Sincronização."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/14/2017
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 45054799cdc8bbe6d39fa2beb28e69d13cace031
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/15/2017
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>Guia do Microsoft BHOLD Suite conceitos

Microsoft Identity Manager 2016 (MIM) permite às organizações gerir o ciclo de vida completo das identidades de utilizador e as respetivas credenciais associadas. Pode ser configurada para sincronizar identidades, gerir centralmente certificados e palavras-passe e aprovisionar utilizadores em sistemas heterogéneos. Com o MIM, as organizações de TI podem definir e automatizar os processos utilizados para gerir identidades de criação para extinção.

Microsoft BHOLD Suite expande estas capacidades de MIM através da adição de controlo de acesso baseado em funções. BHOLD permite às organizações para definir funções de utilizador e para controlar o acesso a dados confidenciais e aplicações. O acesso com base em que é adequado para essas funções. BHOLD Suite inclui serviços e ferramentas que simplificam a modelação das relações de função na sua organização. BHOLD mapeia essas funções direitos e certifique-se de que as definições de função e os direitos associados são aplicados corretamente aos utilizadores. Estas funcionalidades estão totalmente integradas com o MIM, proporcionando uma experiência totalmente integrada para os utilizadores finais e a equipa de TI igual.

Este guia ajuda-o a compreender como funciona o BHOLD Suite com o MIM e inclui os tópicos seguintes:

- Controlo de acesso baseado em funções
- Atestado
- Análise de
- Relatórios
- Conector de gestão de acesso
- Integração de MIM

## <a name="role-based-access-control"></a>Controlo de acesso baseado em funções

O método mais comum para controlar o acesso de utilizador a dados e aplicações é através de controlo de acesso discricionário (DAC). As implementações mais comuns, todos os objetos significativos tem um proprietário identificado. O proprietário tem a capacidade para conceder ou negar o acesso ao objeto a outras pessoas com base na identidade individuais ou membros do grupo. Na prática, DAC normalmente resulta num plethora de grupos de segurança, algumas refletir a estrutura organizacional, outras pessoas que representam os agrupamentos funcionais (como tipos de tarefas ou atribuições de projeto) e outras pessoas que consistem makeshift coleções de utilizadores e dispositivos que estão ligados para fins de temporários mais. À medida que aumentam a organizações, a associação esses grupos torna-se cada vez mais difícil de gerir. Por exemplo, se um empregado é transferido de um projeto para outro, os grupos que são utilizados para controlar o acesso a recursos de projetos devem ser atualizados manualmente. Nestes casos, não é invulgar para prende possa ocorrer, prende pode impedir o projeto segurança ou de produtividade.

MIM inclui funcionalidades que ajudam a mitigar este problema, fornecendo automatizado controlo sobre a associação de lista de grupo e a distribuição. No entanto, isto se referem a intrínseca complexidade dos grupos proliferating que não estão necessariamente relacionadas entre si de uma forma structured.

Uma forma para reduzir significativamente este proliferação é implementar o controlo de acesso baseado em funções (RBAC). RBAC não displace DAC.  RBAC baseia-se DAC fornecendo uma arquitetura para classificar os utilizadores e recursos de TI. Isto permite-lhe efetuar explícito a relação dos mesmos e os direitos de acesso apropriadas, de acordo com essa classificação. Por exemplo, através da atribuição a um utilizador atributos que especificar os utilizadores título e tarefas atribuições de projeto, o utilizador podem ser concedidas acesso a ferramentas necessárias para a tarefa e os dados que o utilizador precisa para contribuir para um projeto específico do utilizador. Quando o utilizador assume uma tarefa diferente e atribuições de projeto diferentes, alterar os atributos que especificar cargo do utilizador e projetos automaticamente bloqueia o acesso aos recursos necessários apenas para a posição anterior de utilizadores.

Porque as funções podem ser incluídas em funções de uma forma hierárquica, (por exemplo, as funções do Gestor de vendas e representante podem ser incluídas sob a função mais geral de vendas), é fácil atribuir os direitos adequados para funções específicas e fornecer ainda ainda direitos adequados para todas as pessoas que partilha a função de mais inclusive. Por exemplo, um hospital, todo o pessoal médicas pode ser-lhe concedido o direito de ver registos patients, mas apenas physicians (subfunção da função médicas) foi ser-lhe concedidos o direito de entrar prescriptions para o patient. Da mesma forma, os utilizadores que pertencem à função clerical foi ser negados o acesso a registos patient exceto clerks (subfunção da função clerical), que podem ser concedidos acesso a essas partes de registos patients que são necessárias ao faturar-patient para serviços de faturação forneceu hospital.

Uma vantagem adicional do RBAC é a capacidade de definir e impor a separação de deveres (SoD). Isto permite que uma organização definir combinações de funções que concede permissões que não devem ser mantidas pelo mesmo utilizador, para que um determinado utilizador não é possível atribuir funções que permitem ao utilizador para iniciar um pagamento e autorizar um pagamento, por exemplo. RBAC fornece a capacidade de impor uma política de tal automaticamente em vez de ter que avaliar a implementação da política numa base por utilizador efetiva.

### <a name="bhold-role-model-objects"></a>Objetos de modelo de função do BHOLD

Com BHOLD Suite, pode especificar e organizar funções na sua organização, os utilizadores de mapeamento de funções e permissões adequadas do mapa para funções. Esta estrutura é chamada um modelo de função e contém e liga-se cinco tipos de objetos: 

- Unidades organizacionais
- Users
- Funções
- Permissões
- Aplicações

#### <a name="organizational-units"></a>Unidades organizacionais

Unidades organizacionais (OrgUnits) são os meios principais através do qual os utilizadores estão organizados por modelo de função BHOLD. Todos os utilizadores têm de pertencer ao OrgUnit pelo menos um. (Na realidade, quando um utilizador é removido da unidade organizacional último na BHOLD, registo de dados do utilizador é eliminado da base de dados do BHOLD.)

>[!Important]
Unidades organizacionais no modelo de função BHOLD não devem ser confundidas com unidades organizacionais nos serviços de domínio do Active Directory (AD DS). Normalmente, a estrutura de unidade organizacional no BHOLD baseia-se na organização e as políticas da sua empresa, não os requisitos da sua infraestrutura de rede.

Embora não seja necessário, na maioria dos casos, unidades organizacionais são estruturadas na BHOLD para representar a estrutura hierárquica da organização real, semelhante à abaixo:

![](media/bhold-concepts-guide/org-chart.png)

Neste exemplo, o modelo de função seria organizationalganizatinal para corporation como um todo (representado pelo president, porque o president não faz parte de uma unidade de mororganizationalganizatinal), ou de unidade à unidade organizacional de raiz do BHOLD (que sempre existe) pode ser utilizado para o efeito. OrgUnits que representa os divisões empresariais headed pelos presidents vice-versa são colocadas na unidade organizacional empresarial. Unidades organizacionais seguintes correspondente directors de vendas e marketing seriam adicionadas às unidades organizacionais marketing e de vendas e unidades organizacionais que representa os gestores de vendas regionais são colocadas na unidade organizacional para o Gestor de vendas do Leste região. Associa venda, que não gere a outros utilizadores, teria de ser feita membros da unidade organizacional do Gestor de vendas do Leste região. Tenha em atenção que os utilizadores podem ser membros de uma unidade organizacional qualquer nível. Por exemplo, um assistente administrativo, que não é um Gestor de relatórios diretamente para um president vice-versa, teria de ser um membro da unidade organizacional do president de vice-versa.

Para além das que representa a estrutura organizacional, unidades organizacionais também podem ser utilizadas para agrupar utilizadores e outras unidades organizacionais, de acordo com os critérios funcionais, tal como para projetos ou especialização. O diagrama seguinte mostra como organizacional unidades teriam de ser utilizadas para agrupar associa venda, de acordo com o tipo de cliente:

![](media/bhold-concepts-guide/org-chart-02.png)

Neste exemplo, cada associar venda seria pertence a duas unidades organizacionais: um que representa o local do associar na estrutura de gestão da organização e outra que representa a base do cliente do associar (revenda ou da empresa). Cada unidade organizacional pode ser atribuída a funções diferentes que, por sua vez, podem ser atribuídas permissões diferentes para aceder a organização de recursos de TI. Além disso, as funções podem ser herdadas de unidades organizacionais principal, simplificar o processo de propagar funções aos utilizadores. Por outro lado, funções específicas podem ser impedidas de que está a ser herdadas, garantindo que uma função específica está associada apenas as unidades organizacionais adequadas.

OrgUnits podem ser criados na BHOLD Suite utilizando o portal web do BHOLD núcleos ou utilizando o gerador de modelo do BHOLD.

#### <a name="users"></a>Users

Conforme indicado acima, cada utilizador tem de pertencer a pelo menos uma unidade organizacional (OrgUnit). Porque a unidades organizacionais são o mecanismo de principal para associar um utilizador a funções, na maioria das organizações um utilizador especificado pertence a vários OrgUnits para tornar mais fácil associar funções de utilizador. No entanto, em alguns casos, poderá ser necessário associar uma função de utilizador para além dos qualquer OrgUnits que o utilizador pertence. Por conseguinte, um utilizador pode ser atribuído diretamente a uma função, bem como obter funções de OrgUnits que o utilizador pertence.

Quando um utilizador não está ativo na sua organização (tempo ausente para deixe médicas, por exemplo), o utilizador pode ser suspenso, que revoga permissões do utilizador sem remover o utilizador do modelo de função. Após regressar à duty, pode ser reativado o utilizador, que restaura todas as permissões concedidas pelas funções de utilizador.

Objetos para os utilizadores podem ser criados individualmente no BHOLD através do portal web do BHOLD núcleos ou podem ser importados em massa, utilizando o gerador de modelos do BHOLD ou utilizando o conector de gestão de acesso com o serviço de sincronização do FIM para importar as informações do utilizador origens como aplicações de serviços de domínio do Active Directory ou de recursos humanos.

Os utilizadores podem ser criados diretamente no BHOLD sem utilizar o serviço de sincronização do FIM. Isto pode ser útil quando modelação funções num ambiente de pré-produção ou de teste. Pode também autorizar os utilizadores externos (por exemplo, os funcionários de um subcontractor) para ser atribuídos a funções e assim dado acesso aos recursos de TI sem a ser adicionado à base de dados empregado; No entanto, estes utilizadores não poderão utilizar as funcionalidades do BHOLD self-service.

#### <a name="roles"></a>Funções

Como anteriormente apontou, sob o modelo de controlo (RBAC) de acesso baseado em funções, permissões associadas com as funções em vez de utilizadores individuais. Isto torna possível conceder as permissões necessárias para efetuar deveres do utilizador ao alterar funções de utilizador em vez de em separado para conceder ou negar as permissões de utilizador de cada utilizador. Como consequence, a atribuição de permissões já não requer a participação do departamento de IT, mas em vez disso, pode ser efetuada como parte da gestão de negócio. Uma função pode agregar permissões para aceder a sistemas diferentes, diretamente ou através da utilização de subroles, reduzindo ainda mais a necessidade de envolvimento IT na gestão de permissões de utilizador.

É importante ter em atenção que as funções são uma funcionalidade do próprio; modelo RBAC Normalmente, as funções não são aprovisionadas para aplicações de destino. Esta permite que o RBAC para ser utilizado em conjunto com as aplicações existentes que não foram concebidas para utilizar as funções ou para alterar a função de definições de ser satisfazer as necessidades da alteração modelos de empresas sem ter de modificar as aplicações próprios. Se uma aplicação de destino foi concebida para utilizar as funções, em seguida, pode funções associadas no modelo de função BHOLD com correspondente funções da aplicação por encará-los as funções específicas da aplicação, como as permissões.

No BHOLD, pode atribuir uma função a um utilizador principalmente através de dois mecanismos:

- Ao atribuir uma função para uma organização unidade (unidade organizacional), do qual o utilizador é um membro
- Ao atribuir uma função diretamente a um utilizador

Uma função atribuída a uma unidade organizacional de principal, opcionalmente, pode ser herdada pelo respetivas unidades organizacionais do membro. Quando uma função é atribuída ou herdada por uma unidade organizacional, pode ser designado como uma função efetiva ou proposta. Se se tratar de uma função em vigor, todos os utilizadores da unidade organizacional são atribuídos a função. Se se tratar de uma função proposta, tem de ser ativado para cada unidade organizacional de utilizador ou de membro para entram em vigor para esse utilizador ou por membros da unidade organizacional. Isto possibilita a atribuir aos utilizadores um subconjunto das funções associado uma unidade organizacional, em vez de atribuir todas as funções da unidade organizacional automaticamente a todos os membros. Além disso, as funções podem ser especificadas datas de início e fim e limites podem ser colocados numa percentagem de utilizadores dentro de uma unidade organizacional para o qual uma função pode ser eficaz.

O diagrama seguinte ilustra como um utilizador individual pode ser atribuído uma função no BHOLD:

![](media/bhold-concepts-guide/org-chart-flow.png)

Neste diagrama, A função de está atribuída a uma unidade organizacional como uma função herdável e, por isso, é herdada pelos respetivos unidades organizacionais do membro e todos os utilizadores dentro desses unidades organizacionais. Função B é atribuída como uma função proposta para uma unidade organizacional. Tem de ser ativado antes de um utilizador na unidade organizacional pode ser autorizado com as permissões da função. Função C é uma função em vigor, pelo que as respetivas permissões são imediatamente aplicam a todos os utilizadores na unidade organizacional. Função D está ligada diretamente ao utilizador e, por isso, as respetivas permissões aplicam imediatamente para esse utilizador.

Além disso, uma função pode ser ativada para um utilizador com base nos atributos de um utilizador. Para obter mais informações, consulte autorização baseadas em atributos.

#### <a name="permissions"></a>Permissões

Uma permissão no BHOLD corresponde a uma autorização importada a partir de uma aplicação de destino. Ou seja, quando BHOLD está configurado para trabalhar com uma aplicação, recebe uma lista de autorizações BHOLD pode ligar a funções. Por exemplo, quando os serviços de domínio do Active Directory (AD DS) é adicionado à BHOLD como uma aplicação, recebe uma lista de grupos de segurança que, como as permissões do BHOLD, podem ser ligados às funções nos BHOLD.

As permissões são específicas para as aplicações. BHOLD fornece uma vista unificada única de permissões para que as permissões podem ser associadas a funções sem necessidade de gestores de função para compreender os detalhes de implementação, as permissões. Na prática, sistemas diferentes poderão impor uma permissão de forma diferente. O conector específicos da aplicação do serviço de sincronização do FIM à aplicação determina a forma como as alterações de permissão para um utilizador são fornecidas para essa aplicação. 

#### <a name="applications"></a>Aplicações

BHOLD implementa um método para aplicação de controlo de acesso baseado em funções (RBAC) a aplicações externas. Ou seja, quando BHOLD está aprovisionado com utilizadores e permissões de uma aplicação, BHOLD pode associar essas permissões a utilizadores atribuir funções aos utilizadores e, em seguida, ligando as permissões para as funções. O processo da aplicação em segundo plano, em seguida, pode mapear as permissões corretas para os seus utilizadores baseados no mapeamento de função/permissão no BHOLD.

### <a name="developing-the-bhold-suite-role-model"></a>Desenvolver o modelo de função de BHOLD Suite

Para ajudar a desenvolver o seu modelo de função, BHOLD Suite fornece o gerador de modelos, uma ferramenta que é fácil de utilizar e abrangente.

Antes de utilizar o gerador de modelos, tem de criar uma série de ficheiros que definir os objetos que utiliza o gerador de modelos para construir o modelo de função. Para obter informações sobre como criar estes ficheiros, consulte a referência técnica do Microsoft BHOLD Suite.

O primeiro passo para utilizar o gerador de modelo do BHOLD é não importar estes ficheiros para carregar os blocos modulares básicos para o gerador de modelos. Quando os ficheiros foram carregados com êxito, em seguida, pode especificar critérios que utiliza o gerador de modelos para criar várias classes de funções:

- Funções de associação que estão atribuídas a um utilizador com base no OrgUnits (unidades organizacionais) que o utilizador pertence
- Funções de atributos que estão atribuídas a um utilizador com base nos atributos de utilizador na base de dados do BHOLD
- Funções proposto e que estão ligadas a uma unidade organizacional, mas tem de ser ativadas para utilizadores específicos
- Funções de propriedade que conceda um controlo de utilizador através de unidades organizacionais e de funções para o qual um proprietário não foi especificado nos ficheiros de importados

>[!Important]
Quando carregar ficheiros, selecione o **manter o modelo existente** caixa de verificação apenas em ambientes de teste. Em ambientes de produção, tem de utilizar o gerador de modelos para criar o modelo de função inicial. Não é possível utilizá-lo para modificar um modelo de função existente na base de dados do BHOLD.

Depois do gerador de modelos cria estas funções no modelo de função, em seguida, pode exportar o modelo de função para a base de dados do BHOLD sob a forma de um ficheiro XML.

### <a name="advanced-bhold-features"></a>Funcionalidades avançadas do BHOLD

Secções anteriores descritas as funcionalidades básicas de controlo de acesso baseado em funções (RBAC) na BHOLD. Esta secção descreve as funcionalidades adicionais do BHOLD que podem fornecer segurança avançada e flexibilidade de implementação da sua organização do RBAC. Esta secção fornece descrições gerais das funcionalidades BHOLD seguintes:

- Cardinalidade
- Separação de deveres
- Permissões de contexto adaptable
- Autorização baseadas em atributos
- Tipos de atributo flexível


#### <a name="cardinality"></a>Cardinalidade

*Cardinalidade* refere-se para a implementação de regras de negócio que foram concebidos para limitar o número de vezes que podem estar relacionado com duas entidades para si. No caso do BHOLD, regras de cardinalidade podem ser estabelecidas para os utilizadores, funções e permissões.

Pode configurar uma função para limitar o seguinte:

- O número máximo de utilizadores para o qual uma função proposta pode ser activada
- O número máximo de subroles que pode ser associado à função
- O número máximo de permissões que pode ser associado à função

Pode configurar uma permissão para limitar o seguinte:

- O número máximo de funções que pode ser associado a permissão
- O número máximo de utilizadores que podem ser concedidas a permissão

Pode configurar um utilizador para limitar o seguinte:

- O número máximo de funções que podem ser ligados ao utilizador
- O número máximo de permissões que pode ser atribuída ao utilizador através do atribuições de função

#### <a name="separation-of-duties"></a>Separação de deveres

Separação de deveres (SoD) é um princípio de negócio que procura para impedir que os indivíduos obtenham a capacidade de efetuar as ações que não devem estar disponíveis para uma única pessoa. Por exemplo, um empregado deve não é possível pedir um pagamento e autorizar o pagamento. O princípio de SoD permite às organizações implementar um sistema de verificações e saldos para minimizar a respetiva exposição ao risco de erro de empregado ou misconduct.

BHOLD implementa SoD, permitindo-lhe definir as permissões de incompatíveis. Quando estas permissões são definidas, BHOLD impõe SoD ao impedir que a criação de funções que estejam ligadas a incompatíveis permissões, se estes estão ligados diretamente ou através de herança e ao impedir que os utilizadores de que está a ser atribuídas várias funções que, quando combinados, seria conceder permissões incompatíveis, novamente através da atribuição direta ou através de herança. Opcionalmente, pode ser substituído está em conflito.

#### <a name="context-adaptable-permissions"></a>Permissões de contexto adaptable

Ao criar as permissões que podem ser automaticamente modificadas, com base num atributo de objeto, pode reduzir o número total de permissões, que tem de gerir. Permissões de contexto adaptable (CAPs) permitem-lhe definir uma fórmula como um atributo de permissão modifica a forma como a permissão é aplicada a aplicação associada com a permissão. Por exemplo, pode criar uma fórmula que as alterações a permissão de acesso para uma pasta de ficheiros (através de um grupo de segurança associado a lista de controlo de acesso a pasta) com base em se um utilizador pertence a um organizacional que contém unidade (unidade organizacional) tempo inteiro ou contrato de funcionários. Se o utilizador é mover de uma unidade organizacional para outra, a nova permissão é aplicada automaticamente e a permissão antiga é desativada. 

A fórmula de extremidade pode consultar os valores dos atributos que tenham sido aplicados aos utilizadores, unidades organizacionais, permissões e aplicações.

#### <a name="attribute-based-authorization"></a>Autorização baseadas em atributos

Uma forma para controlar se uma função que está ligada a uma unidade organizacional (unidade organizacional) está ativada para um utilizador específico na unidade organizacional consiste em utilizar autorização baseadas em atributos (ABA). Ao utilizar ABA, pode ativar automaticamente uma função apenas quando forem cumpridas determinadas regras com base nos atributos de um utilizador. Por exemplo, pode ligar uma função para uma unidade organizacional que fica ativa para um utilizador apenas se o título de trabalho na regra ABA corresponde a título de trabalho do utilizador. Isto elimina a necessidade de ativar manualmente uma função proposta para um utilizador. Em vez disso, uma função pode ser ativada para todos os utilizadores numa unidade organizacional tem um valor de atributo que satisfaça a regra de ABA a função. As regras podem ser combinadas, para que uma função é ativada apenas quando os atributos de um utilizador satisfazem todas as regras de ABA especificadas para a função.

É importante ter em atenção que os resultados dos testes de regra ABA são limitados pelas definições de cardinalidade. Por exemplo, se a definição de cardinalidade de uma regra especifica mais do que dois utilizadores podem ser atribuídos uma função, e se uma regra de ABA caso contrário, prefere ativar uma função para quatro utilizadores, a função será ativada apenas para os primeiros dois utilizadores que passaram o teste de ABA.

#### <a name="flexible-attribute-types"></a>Tipos de atributo flexível

O sistema de atributos do BHOLD é altamente extensível. Pode definir novos tipos de atributo para estes objetos como utilizadores, organizacionais, as unidades (unidades organizacionais) e funções. Os atributos podem ser definidos para valores que são números inteiros, booleano (Sim/não), alfanuméricos, data, hora e e-mail endereços. Os atributos podem ser especificados como valores único ou uma lista de valor.

## <a name="attestation"></a>Atestado

BHOLD Suite fornece ferramentas que pode utilizar para verificar que os utilizadores individuais tem recebidos as permissões adequadas para realizar as tarefas de negócio. O administrador pode utilizar o portal fornecido pelo módulo de atestado do BHOLD para estruturar uma gerir o processo de atestado.

É efetuado através de campanhas no qual campanha stewards são dada a oportunidade do processo de atestado e significa verificar que os utilizadores que são responsáveis tem acesso adequado para aplicações geridas do BHOLD e as permissões corretas no essas aplicações. Um proprietário da campanha está designado para supervisionam a campanha e certifique-se de que a campanha está a ser executada corretamente. É possível criar uma campanha para ocorrer uma vez ou periodicamente.

Normalmente, steward para uma campanha será um gestor a quem será atestem os direitos de acesso de utilizadores que pertençam a um ou mais unidades organizacionais para o qual o Gestor de é responsável. Stewards podem ser selecionados automaticamente para os utilizadores que está a ser atestados numa campanha com base nos atributos de utilizador, ou podem ser definidos os stewards para uma campanha enumerando-las num ficheiro que mapeia cada utilizador que está a ser atestado na campanha para um steward.

Quando uma campanha começa, BHOLD envia uma mensagem de notificação de correio eletrónico para o proprietário e stewards campanha e, em seguida, envia lembretes periódicas para ajudar a manter o progresso na campanha. Stewards são direcionados para um portal de campanha onde podem ver uma lista de utilizadores que são responsáveis e as funções que são atribuídas aos utilizadores. Os stewards, em seguida, podem confirmar se estes são responsáveis por cada um dos utilizadores listados e aprovar ou recusar os direitos de acesso de cada um dos utilizadores listados.

Os proprietários de campanha também utilizam o portal para monitorizar o progresso da campanha e atividades de campanha são registadas para que proprietários de campanha podem analisar as ações que foram executadas durante a campanha.

## <a name="analytics"></a>Análise de

Uma das considerações importantes sobre quando implementar um sistema de controlo (RBAC) de acesso baseado em direitos abrangente de equilíbrio entre manter o controlo de acesso e evitar desnecessários (ou, um, inesperado) barreiras as eficazes para aceder. O esforço para manter esta equilíbrio frequentemente resulta numa estrutura de controlo de acesso é, por isso, complexa que inesperado interações entre as políticas são quase obrigatório.

Por esse motivo, é importante ser capaz de analisar os efeitos das políticas de controlo de acesso antes de poderem, na verdade, são colocados no local. O módulo de análise de BHOLD Suite fornece que está em conformidade com a capacidade de efetuar esta análise, permitindo-lhe desenvolver regras que representam as políticas e, em seguida, que mostra os utilizadores cujas permissões ou estão em conflito com a regra. Com base nesta análise, pode ajustar a política ou modificar as funções e permissões para eliminar quaisquer conflitos com a política.

O portal do BHOLD Analytics dá-lhe a capacidade de construir rulesets consistem de uma ou mais regras que criar para testar uma determinada política ou grupo de políticas. Uma regra consiste as principais partes seguintes:

- Um título e uma descrição que lhe permitem identificam e descrevem a regra
- Um Estado que indica se a regra está pronta para revisão, que está a ser revisto, ou aprovado
- Um elemento definido (tais como permissões de utilizadores ou) que a regra foi concebida para testar
- Filtros de subconjunto opcional que são as expressões que pode utilizar para selecionar um subgrupo adequado do elemento ser reduzida
- Um ou mais regras filtros expressões que representam a política que está a ser testada.

Uma regra pode testar a qualquer um dos seguintes conjuntos de elemento:

- Users
- Unidades organizacionais
- Funções
- Permissões
- Aplicações
- Contas

O diagrama seguinte ilustra uma regra simples consiste em regras de subconjunto dois e duas regras de filtro:

![](media/bhold-concepts-guide/rules.png)

Tenha em atenção a diferença no efeito de um filtro de subconjunto a falhar e de falha de um filtro de regra: um filtro de subconjunto a falhar remove um objeto de elemento do teste por subsequentes filtros, enquanto um filtro de regra a falhar faz com que o objeto sejam comunicados como não compatíveis. Apenas os objetos que passar todos os filtros de subconjunto e todos os filtros de regra são reportados como estando em conformidade.

Cada filtro é composta por um tipo, um operador (que é dependente do tipo), uma chave (um dos elementos) e um valor em relação às quais a chave é testada pela operadora de rede. Por exemplo, o seguinte filtro seria testar se o número de utilizadores de um subconjunto de elemento exceder 10:


|   |   |   |   |   |
|---|---|---|---|---|
|**Tipo:**   | Número de   |
| **Chave:**  | Users  |
| **Operador**  | >  |
| **Valor:** | 10 |

Os filtros de regras podem ter três tipos e utilizar os operadores específicos para o respetivo tipo, conforme indicado:

- Atributo
  - < e >
  - = e! =
  - **Contém**
  - **Não contém**
- Número de
  - < e >
  - = e! =
- Restrita
  - **Tem de ter qualquer e tem de ter todos**
  - **Não pode ter qualquer e não pode ter todos**
  - **Pode apenas quaisquer e só pode ter todos**
  - **Exclusivamente ter qualquer e exclusivamente ter todos**

>[!Note]
Filtros restritivos podem utilizar os operadores indicados para testar uma chave de um conjunto de valores múltiplos.

Por exemplo, se pretendesse testar a implementação de uma segregação da política de deveres (SoD) que indica que nenhum utilizador com permissão do pedido de pagamento também é ter a permissão de pagamento aprovar, foi possível construir uma regra, como o seguinte:

|   |  |
|---|--|
|Nome:| Teste de SoD de pagamento|
|Elemento:| Users|
|Filtro de subconjunto:| Com qualquer permissão do pedido de pagamento|
|Filtro de regra: | Não pode ter qualquer permissão aprovar pagamento|

Quando executar esta regra, o módulo do BHOLD Analytics apresenta o número de utilizadores no subconjunto selecionado (o número de utilizadores com permissões de pagamento de pedido), o número de utilizadores que estão em conformidade com a regra e o número de utilizadores que não está em conformidade com a regra. Em seguida, pode apresentar os utilizadores não compatíveis para pode corrigir a inconsistência.

Para além de visualizar os resultados, também pode guardar o relatório como um ficheiro de valores separados por vírgulas (CSV) ou o ficheiro XML para permitir-lhe analisar os resultados mais tarde. Também pode personalizar o relatório resultante para mostrar informações adicionais que podem ajudar a compreender melhor o impacto. Por exemplo, se estiver a testar utilizadores, pode apresentar (ou relatório) as contas de utilizadores compatível ou incompatível para que possa ver as aplicações que estão relacionadas.

Porque uma regra pode conter vários filtros, pode ligar os filtros para testar se existe uma determinada combinação de condições. Por predefinição, o resultado é o produto de um teste e booleano de todos os filtros, mas pode especificar que um teste ou da combinação do filtro ser efetuada.

Por exemplo, se a política da sua empresa precisar de gestores de tenha a permissão de pagamento de modificar ou a permissão de pagamento aprovar, foi teste conformidade com a política ao construir uma regra, como o seguinte:


|  |  |
|--|--|
|Nome: | Modificar o pagamento SoD teste|
|Elemento: | Users |
|Filtro de subconjunto: | Ter uma função de Gestor|
| Filtros de regra: |Tem de ter qualquer permissão modificar pagamento </br> Tem de ter qualquer permissão aprovar pagamento|

Por predefinição, qualquer utilizador que é um Gestor de que tem o pagamento modificar e a permissão do pedido de pagamento incidirá como estando em conformidade. No entanto, a política requer que um Gestor de ter a permissão, não necessariamente ambos. Para testar real conformidade com a política, tem de utilizar o operador booleano existente ou com a regra para determinar se existem quaisquer gestores de que não tem permissão de pagamento de modificar ou permissão de aprovar o pagamento.

Ao contrário de outros operadores, o **exclusivamente ter qualquer** e o **exclusivamente têm todas** operadores indicam conformidade para objetos que caso contrário, seria possível excluídos por um filtro de subconjunto. Por exemplo, para testar uma política que todos os gestores (e apenas gestores) tem a permissão de aprovar revisões, foi possível construir uma regra da seguinte forma:

|  |  |
|--|--|
|Nome: | Teste de aprovação da revisão|
|Elemento: | Users|
| Filtro de subconjunto: | Ter uma função de Gestor
|Filtro de regra: | Exclusivamente ter quaisquer revisões de aprovar de permissão|

Esta regra irá reportar como compatível com utilizadores que são gestores e ter a permissão de aprovar revisões e os utilizadores que não são gestores e que não tenham a permissão de aprovar revisões. Por outro lado, gestores de que tem essa permissão e os utilizadores que não são gestores mas tem essa permissão são reportados como não compatíveis.

Conforme indicado anteriormente, pode combinar regras num ruleset, tornando mais fácil para categorizar e gerir as regras para satisfazer os seus requisitos empresariais.

Também pode definir um conjunto de global filtra que, quando ativadas, aplicar a regra que é testada. Se tiver frequentemente excluir um subconjunto específico de registos ao testar regras no rulesets diferente, pode especificar filtros global que pode ativar ou desativar conforme necessário.

## <a name="reporting"></a>Relatórios

O módulo de relatórios do BHOLD dá-lhe a capacidade para ver informações no modelo de função através de uma variedade de relatórios. O módulo de relatórios do BHOLD fornece um vasto conjunto de relatórios incorporados, mais inclui um assistente que pode utilizar para criar relatórios personalizados básicos e avançados. Quando executar um relatório, pode apresentar os resultados imediatamente ou guardar os resultados num ficheiro (ou) do Microsoft Excel. Para ver este ficheiro utilizando o Microsoft Excel 2000, o Microsoft Excel 2002 ou o Microsoft Excel 2003, pode transferir e instalar o pacote de compatibilidade do Microsoft Office para Word, Excel e PowerPoint formatos de ficheiro.


Utilize o módulo de relatórios do BHOLD principalmente para produzir relatórios que se baseiam em informações de função atual. Para gerar relatórios de auditorias alterações às informações de identidade, o Forefront Identity Manager 2010 R2 tem uma capacidade de relatórios para o serviço FIM que é implementado no armazém de dados do System Center Service Manager. Para obter mais informações sobre a FIM de relatórios, consulte a documentação do Forefront Identity Manager 2010 e do Forefront Identity Manager 2010 R2 na biblioteca técnica do Forefront Identity Management.

Categorias abrangidas pelos relatórios incorporados incluem o seguinte:

- Administration
- Atestado
- controlos
- Controlo de acesso inward
- Registo
- Model
- Estatísticas
- Fluxo de Trabalho

Pode criar relatórios e adicioná-los para estas categorias, ou pode definir as seus próprios categorias em que pode colocar os relatórios incorporados e personalizados.

Como criar um relatório, o Assistente para os passos fornecendo os seguintes parâmetros:

- Informações de identificação, incluindo o nome, descrição, categoria, utilização e público-alvo
- Campos a apresentar no relatório
- As consultas que especifique quais os itens estão a ser analisado
- Ordem na qual as linhas devem ser ordenados
- Campos utilizados para interromper o relatório secções
- Filtros para refinar os elementos que são devolvidos no relatório

Em cada fase do assistente, pode pré-visualizar o relatório à medida que definiu-lo até ao momento e guardá-lo se não pretender especificar parâmetros adicionais. Pode também mover novamente para passos anteriores para alterar os parâmetros que especificou anteriormente no assistente.

## <a name="access-management-connector"></a>Conector de gestão de acesso

O módulo de conector de gestão de acesso do BHOLD Suite suporta inicial e contínua a sincronização de dados para BHOLD. O conector de gestão de acesso funciona com o serviço de sincronização do FIM para mover dados entre a base de dados do BHOLD Core, o MIM metaverso e aplicações de destino e arquivos de identidade.

Versões anteriores do BHOLD necessários vários MAs controlar o fluxo de dados entre MIM e tabelas de base de dados do BHOLD intermédias. No SP1 do BHOLD Suite, o conector de gestão de acesso permite definir agentes de gestão (MAs) em MIM que fornecer transferência de dados diretamente entre BHOLD e de MIM.

## <a name="mim-integration"></a>Integração de MIM

Uma funcionalidade importante e poderosa do Forefront Identity Manager 2010 e do Forefront Identity Manager 2010 R2 é o portal self-service que permite aos utilizadores finais ver e gerir as suas informações de identidade e de associação. Integração de MIM expande o Portal de MIM com gestão de funções de self-service. Por exemplo, ao utilizar as funcionalidades do BHOLD no Portal de MIM, um utilizador pode pedir a atribuição de função e pode ver as funções do Active Directory e pedidos pendentes. Capacidades adicionais podem ser concedidas aos administradores delegados, tais como a capacidade de atribuições de funções de pedido para outros utilizadores.

É importante ter em atenção que as extensões BHOLD ao Portal de MIM suportam a gestão de fluxo de trabalho e função de self-service e de relatórios. Outras funções de administração do BHOLD, bem como o atestado, é fornecido pelos portais web dos módulos BHOLD, que estão alojados separadamente a partir do Portal de MIM.

## <a name="next-steps"></a>Próximos passos

- [Guia de instalação do BHOLD](../deploy-use/bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
