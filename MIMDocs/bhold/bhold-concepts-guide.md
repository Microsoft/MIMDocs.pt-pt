---
title: Guia de conceitos do Microsoft BHOLD Suite | Documentos da Microsoft
description: Comece a trabalhar com os componentes do MIM 2016 ao instalar e configurar o Serviço de Sincronização.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/14/2017
ms.assetid: ''
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 32bd77140cf70047eaa02d363a1348e73783f87a
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358844"
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>Guia de conceitos do Microsoft BHOLD Suite

Microsoft Identity Manager 2016 (MIM) permite às organizações gerir todo o ciclo de vida de identidades de utilizador e as respetivas credenciais associadas. Ele pode ser configurado para sincronizar as identidades centralmente gerir certificados e palavras-passe e disponibilize para os usuários entre sistemas heterogêneos. Com o MIM, as organizações de TI podem definir e automatizar os processos utilizados para gerir identidades de criação para extinção.

O Microsoft BHOLD Suite expande estas capacidades de MIM através da adição de controlo de acesso baseado em funções. BHOLD permite às organizações para definir funções de utilizador e para controlar o acesso a aplicações e dados confidenciais. O acesso se baseia o que é adequado para essas funções. BHOLD Suite inclui serviços e ferramentas que simplificam a Modelagem as relações de função na organização. BHOLD mapeia essas funções para direitos e certifique-se de que as definições de função e os direitos associados são aplicados corretamente aos utilizadores. Estas capacidades estão totalmente integradas com o MIM, fornecendo uma experiência totalmente integrada para os utilizadores finais e a equipe de TI semelhantes.

Este guia ajuda-o a compreender como o BHOLD Suite funciona com o MIM e abrange os seguintes tópicos:

- Controlo de acesso baseado em funções
- Atestado
- Análise
- Relatórios
- Conector de gestão de acesso
- Integração de MIM

## <a name="role-based-access-control"></a>Controlo de acesso baseado em funções

É o método mais comum para controlar o acesso de utilizador a dados e aplicativos por meio do controle de acesso discricionário (DAC). Em implementações mais comuns, cada objeto significativo tem um proprietário identificado. O proprietário tem a capacidade de conceder ou negar o acesso ao objeto para outros utilizadores com base na identidade individual ou associação de grupo. Na prática, DAC geralmente resulta numa grande quantidade de grupos de segurança, algumas que refletem a estrutura organizacional, outras pessoas que representam os agrupamentos funcionais (como tipos de tarefa ou atribuições de projeto) e outros que consistem em makeshift coleções de utilizadores e dispositivos que estão ligados para fins de temporários mais. À medida que aumentam a organizações, a associação a esses grupos se torna cada vez mais difícil de gerenciar. Por exemplo, se um funcionário é transferido de um projeto para outro, os grupos que são utilizados para controlar o acesso a ativos de projetos devem ser atualizados manualmente. Nesses casos, não é incomum para erros a ocorrência de erros que podem impedir os produtividade ou de segurança de projeto.

MIM inclui recursos que ajudam a mitigar este problema ao fornecer controle automatizado sobre associação de lista do grupo e a distribuição. No entanto, isso não aborda a complexidade intrínseca de grupos proliferando não necessariamente relacionadas entre si de uma forma estruturada.

Uma forma de reduzir significativamente este proliferação é ao implementar o controlo de acesso baseado em funções (RBAC). RBAC não apresentar o DAC.  RBAC baseia-se a DAC ao fornecer uma estrutura para classificar os utilizadores e recursos de TI. Isto permite-lhe tornar explícito a relação dos mesmos e os direitos de acesso apropriadas, de acordo com essa classificação. Por exemplo, ao atribuir a um utilizador cargo de atributos que especificar os utilizadores e as atribuições de projeto, o utilizador podem ser concedidas acesso a ferramentas necessárias para o trabalho e dados que o usuário precisa para contribuir para um projeto específico do usuário. Quando o usuário assume um trabalho diferente e atribuições de projeto diferente, alterar os atributos que especificam a cargo do utilizador e projetos bloqueia automaticamente o acesso aos recursos apenas necessário para a posição anterior de utilizadores.

Uma vez que funções podem ser contidas dentro de funções de uma forma hierárquica, (por exemplo, as funções do Gestor de vendas e o representante de vendas podem ser contidas na função mais geral de vendas), é fácil atribuir os direitos adequados para funções específicas e ainda fornecer obter direitos adequados para todas as pessoas que compartilha também a função mais inclusiva. Por exemplo, no hospital, todo o pessoal médico poderia ser-lhe concedido o direito de ver os registros de pacientes, mas apenas os médicos (subfunção da função médica) poderiam ser-lhe concedidos o direito de introduzir as prescrições para o paciente. Da mesma forma, os utilizadores que pertencem à função clerical poderiam ser negados o acesso a registos dos pacientes, com exceção clerks (subfunção da função clerical), que poderiam ter acesso para as partes de registros de pacientes que são necessárias para faturar o paciente para serviços de faturação fornecido do hospital.

Um benefício adicional de RBAC é a capacidade de definir e impor a separação de funções (SoD). Isso permite que uma organização definir combinações de funções que concedem permissões que não devem ser mantidas pelo mesmo utilizador, para que um usuário específico não é possível atribuir funções que permitem ao utilizador para iniciar um pagamento e para autorizar um pagamento, por exemplo. RBAC fornece a capacidade de impor destas políticas automaticamente, em vez de ter que avaliar a implementação eficaz da política numa base por utilizador.

### <a name="bhold-role-model-objects"></a>Objetos de modelo de função do BHOLD

Com o BHOLD Suite, pode especificar e organizar as funções na sua organização, os utilizadores de mapa de funções e permissões adequadas do mapa para funções. Esta estrutura é chamada de um modelo de função e contém e liga-se cinco tipos de objetos: 

- Unidades organizacionais
- Users
- Funções
- Permissões
- Aplicações

#### <a name="organizational-units"></a>Unidades organizacionais

Unidades organizacionais (OrgUnits) são o principal meio pelo qual os utilizadores estão organizados no modelo de função do BHOLD. Todos os utilizadores têm de pertencer ao OrgUnit pelo menos um. (Na verdade, quando um utilizador for removido da última unidade organizacional no BHOLD, registo de dados do utilizador é eliminado da base de dados do BHOLD.)

> [!Important]
> Unidades organizacionais no modelo de função do BHOLD não devem ser confundidas com unidades organizacionais no Active Directory Domain Services (AD DS). Normalmente, a estrutura de unidade organizacional no BHOLD baseia-se na organização e as políticas da sua empresa, não os requisitos da sua infraestrutura de rede.

Embora não seja necessário, na maioria dos casos, unidades organizacionais estão estruturadas no BHOLD para representar a estrutura hierárquica da organização real, semelhante ao seguinte:

![](media/bhold-concepts-guide/org-chart.png)

Neste exemplo, o modelo de função seria organizationalganizatinal unidade para a corporação como um todo (representado por presidente, porque o presidente não faz parte de uma unidade de mororganizationalganizatinal) ou a unidade organizacional de raiz do BHOLD (que sempre existe) poderia ser usado para essa finalidade. OrgUnits que representa as divisões empresariais chegar aos vice-presidentes seria colocado na unidade organizacional empresarial. Em seguida, organizacionais unidades correspondente para os Diretores de marketing e vendas seriam adicionadas às unidades organizacionais marketing e vendas e unidades organizacionais que representa os gestores de vendas regionais seriam colocadas na unidade organizacional para o Gestor de vendas de região Leste. Associados de vendas, que não gerem a outros utilizadores, seriam se transformar em membros da unidade organizacional do Gestor de vendas de região Leste. Tenha em atenção que os utilizadores podem ser membros de uma unidade organizacional em qualquer nível. Por exemplo, um assistente administrativo, o que não é um gestor e os relatórios diretamente para um vice-presidente Corporativo, seria um membro da unidade organizacional do vice-presidente Corporativo.

Para além das que representa a estrutura organizacional, unidades organizacionais também podem ser utilizadas para agrupar utilizadores e de outras unidades organizacionais, de acordo com critérios funcionais, por exemplo, para projetos ou de especialização. O diagrama seguinte mostra como organizacional unidades seriam usadas para agrupar associados de vendas, de acordo com o tipo de cliente:

![](media/bhold-concepts-guide/org-chart-02.png)

Neste exemplo, cada representante de vendas seria pertencem a duas unidades organizacionais: uma que representa o local do associar na estrutura de gestão da organização e outra que representa a base de clientes do associar (revenda ou da empresa). Cada unidade organizacional pode ser atribuída a diferentes funções que, por sua vez, podem ser atribuídas permissões diferentes para a organização de acessar recursos de TI. Além disso, as funções podem ser herdadas do unidades organizacionais do pai, simplificando o processo de propagar funções aos utilizadores. Por outro lado, funções específicas podem ser impedidas de que está a ser herdada, garantindo que uma função específica está associada apenas a unidades organizacionais adequadas.

OrgUnits podem ser criadas no BHOLD Suite, através do portal web do BHOLD Core ou utilizando o gerador de modelo do BHOLD.

#### <a name="users"></a>Users

Conforme indicado acima, cada utilizador tem de pertencer a pelo menos uma unidade organizacional (OrgUnit). Como as unidades organizacionais são o mecanismo principal para associar um utilizador a funções, na maioria das organizações um determinado utilizador pertence a vários OrgUnits para tornar mais fácil associar funções esse utilizador. No entanto, em alguns casos, poderá ser necessário associar uma função de um utilizador para além de qualquer OrgUnits que o utilizador pertence. Consequentemente, um utilizador pode ser atribuído diretamente a uma função, bem como obter funções do OrgUnits que o utilizador pertence.

Quando um utilizador não está ativo dentro da organização (tempo distância para deixe médica, por exemplo), o utilizador pode ser suspenso, que revoga permissões do utilizador sem remover o utilizador a partir do modelo de função. Após retornar para o imposto, pode ser reativado o usuário, que restaura todas as permissões concedidas pelas funções do usuário.

Objetos para os utilizadores podem ser criados individualmente no BHOLD através do portal web do BHOLD Core ou podem ser importados em massa com o gerador de modelos do BHOLD ou utilizando o conector de gestão de acesso com o serviço de sincronização do FIM para importar informações de usuário tais origens como aplicativos de serviços de domínio do Active Directory ou de recursos humanos.

Os utilizadores podem ser criados diretamente no BHOLD sem utilizar o serviço de sincronização do FIM. Isso pode ser útil ao modelar funções num ambiente de pré-produção ou de teste. Também pode permitir utilizadores externos (por exemplo, os funcionários de uma subcontratante) para ser atribuídos a funções e, portanto, a obter o acesso aos recursos de TI sem ser adicionada para a base de dados do funcionário; No entanto, estes utilizadores não poderá utilizar as funcionalidades de Self-serviços do BHOLD.

#### <a name="roles"></a>Funções

Como mencionado anteriormente, sob o modelo de controlo (RBAC) de acesso baseado em funções, permissões são associadas a funções em vez de utilizadores individuais. Isto torna possível dar a cada usuário as permissões necessárias para realizar os deveres do utilizador ao alterar as funções do usuário em vez de em separado para conceder ou negar as permissões de utilizador. Como conseqüência, a atribuição de permissões já não necessita de participação do departamento de TI, mas em vez disso, pode ser executada como parte da gestão de negócio. Uma função pode agregar as permissões para aceder a diferentes sistemas, diretamente ou através da utilização de subroles, reduzindo ainda mais a necessidade de envolvimento de TI no gerenciamento de permissões de utilizador.

É importante observar que funções são uma funcionalidade do modelo de RBAC em si, Normalmente, funções não são aprovisionadas para aplicações de destino. Esta permite que o RBAC para ser utilizado em conjunto com aplicativos existentes que não foram concebidos para utilizar as funções ou alterar a função de definições de ser atender às necessidades de alterar os modelos de negócio sem ter de modificar os próprios aplicativos. Se um aplicativo de destino foi concebido para utilizar as funções, em seguida, pode funções associadas no modelo de função do BHOLD com funções de aplicação correspondente ao tratar as funções específicas do aplicativo como permissões.

BHOLD, pode atribuir uma função a um utilizador principalmente por meio de dois mecanismos:

- Ao atribuir uma função a uma organização unidade (unidade organizacional) que o utilizador é membro
- Ao atribuir uma função diretamente a um utilizador

As funções atribuídas uma unidade organizacional de principal para, opcionalmente, podem ser herdadas por suas unidades organizacionais do membro. Quando uma função é atribuída a ou herdada por uma unidade organizacional, podem ser indicado como uma função em vigor ou proposta. Se se trata de uma função em vigor, todos os utilizadores na unidade organizacional são atribuídos a função. Se se trata de uma função proposta, tem de ser ativado para cada unidade organizacional no utilizador ou o membro para entram em vigor para esse utilizador ou por membros da unidade organizacional. Isto torna possível atribuir aos utilizadores um subconjunto das funções associadas uma unidade organizacional, em vez de atribuir todas as funções da unidade organizacional automaticamente a todos os membros. Além disso, funções podem ser dado datas de início e fim e limites podem ser colocados na porcentagem de usuários de uma unidade organizacional para o qual uma função pode ser eficaz.

O diagrama seguinte ilustra como um utilizador individual pode ser atribuído uma função no BHOLD:

![](media/bhold-concepts-guide/org-chart-flow.png)

Neste diagrama, a função de um é atribuída a uma unidade organizacional como uma função herdável e por isso, é herdada pelos respetivas unidades organizacionais do membro e todos os utilizadores nessas unidades organizacionais. Função B é atribuída como uma função proposta para uma unidade organizacional. Tem de ser ativado antes de um utilizador na unidade organizacional pode ser autorizado com as permissões da função. Função C é uma função em vigor, pelo que as respetivas permissões são imediatamente aplicam a todos os utilizadores na unidade organizacional. Função D é ligada diretamente ao usuário e por isso, as respetivas permissões aplicam imediatamente a esse utilizador.

Além disso, uma função pode ser ativada para um utilizador com base em atributos de um utilizador. Para obter mais informações, consulte a autorização baseada em atributo.

#### <a name="permissions"></a>Permissões

Uma permissão em BHOLD corresponde a uma autorização importada a partir de uma aplicação de destino. Ou seja, quando BHOLD está configurado para trabalhar com um aplicativo, ele recebe uma lista de autorizações BHOLD pode associar às funções. Por exemplo, quando os serviços de domínio do Active Directory (AD DS) é adicionado ao BHOLD como um aplicativo, ele recebe uma lista de grupos de segurança que, como as permissões do BHOLD, podem ser associados a funções na BHOLD.

As permissões são específicas de aplicativos. BHOLD fornece uma vista unificada de permissões para que as permissões podem ser associadas a funções sem a necessidade de gestores de função entender os detalhes de implementação das permissões. Na prática, diferentes sistemas poderão impor uma permissão de forma diferente. O conector de específicas da aplicação do serviço de sincronização do FIM para o aplicativo determina como as alterações de permissões para um utilizador são fornecidas para essa aplicação. 

#### <a name="applications"></a>Aplicações

BHOLD implementa um método para a aplicação de controlo de acesso baseado em funções (RBAC) para aplicativos externos. Ou seja, quando BHOLD está aprovisionada com utilizadores e permissões de um aplicativo, BHOLD pode associar essas permissões com utilizadores atribuir funções para os utilizadores e, em seguida, ligando as permissões para as funções. O processo do aplicativo em segundo plano, em seguida, pode mapear as permissões corretas para seus usuários com base no mapeamento de função/permissão na BHOLD.

### <a name="developing-the-bhold-suite-role-model"></a>Desenvolvendo o modelo de função de BHOLD Suite

Para ajudar a desenvolver o seu modelo de função, BHOLD Suite fornece o gerador de modelo, uma ferramenta que é fácil de usar e abrangente.

Antes de utilizar o gerador de modelos, tem de criar uma série de arquivos que definem os objetos que utiliza o gerador de modelos para construir o modelo de função. Para obter informações sobre como criar estes ficheiros, consulte a referência técnica do Microsoft BHOLD Suite.

Usando o gerador de modelo do BHOLD o primeiro passo é importar estes ficheiros para carregar os blocos modulares básicos para o gerador de modelos. Quando os ficheiros tenham sido carregados com êxito, em seguida, pode especificar critérios pelo gerador de modelo para criar várias classes de funções:

- Funções de associação que estão atribuídas a um utilizador com base em OrgUnits (unidades organizacionais) que o utilizador pertence
- Funções de atributo que estão atribuídas a um utilizador com base em atributos do utilizador da base de dados do BHOLD
- Proposta de funções que estão ligadas a uma unidade organizacional, mas tem de ser ativadas para utilizadores específicos
- Funções de propriedade que concedem um controle de usuário através de unidades organizacionais e as funções para o qual o proprietário não for especificado nos ficheiros de importados

> [!Important]
> Quando o carregamento de ficheiros, selecione o **manter o modelo existente** caixa de verificação apenas em ambientes de teste. Em ambientes de produção, tem de utilizar o gerador de modelo para criar o modelo de função inicial. Não é possível utilizá-lo para modificar um modelo de função existente na base de dados do BHOLD.

Depois de gerador de modelos cria estas funções no modelo de função, em seguida, pode exportar o modelo de função para a base de dados do BHOLD na forma de um arquivo XML.

### <a name="advanced-bhold-features"></a>Recursos avançados do BHOLD

As secções anteriores descritas as funcionalidades básicas de controlo de acesso baseado em funções (RBAC) na BHOLD. Esta seção descreve os recursos adicionais do BHOLD, que podem fornecer segurança aprimorada e flexibilidade de implementação da sua organização de RBAC. Esta secção fornece uma visão geral das seguintes funcionalidades do BHOLD:

- Cardinalidade
- Separação de funções
- Permissões de contexto adaptáveis
- Autorização baseada em atributo
- Tipos de atributo flexível


#### <a name="cardinality"></a>Cardinalidade

*Cardinalidade* refere-se na implementação de regras de negócio que são projetados para limitar o número de vezes que duas entidades podem estar relacionadas entre si. No caso do BHOLD, regras de cardinalidade podem ser estabelecidas para os utilizadores, permissões e funções.

Pode configurar uma função para limitar o seguinte:

- O número máximo de utilizadores para o qual pode ser ativada a uma função de proposta
- O número máximo de subroles que pode ser associado à função
- O número máximo de permissões que pode ser associado à função

Pode configurar uma permissão para limitar o seguinte:

- O número máximo de funções que podem ser associados à permissão
- O número máximo de utilizadores que podem ser concedidas a permissão

Pode configurar um utilizador para limitar o seguinte:

- O número máximo de funções que podem ser associados ao utilizador
- O número máximo de permissões que podem ser atribuídos ao usuário por meio de atribuições de funções

#### <a name="separation-of-duties"></a>Separação de funções

Separação de funções (SoD) é um princípio de negócios que procura para impedir que pessoas de terem a capacidade de efetuar ações que não devem estar disponíveis para uma única pessoa. Por exemplo, um funcionário deve não é possível para pedir um pagamento e para autorizar o pagamento. O princípio de SoD permite às organizações implementar um sistema de verificações e saldos para minimizar a exposição ao risco de erro do funcionário ou misconduct.

BHOLD implementa SoD, permitindo-lhe definir permissões incompatíveis. Quando estas permissões são definidas, BHOLD impõe SoD ao impedir que a criação de funções que estejam ligadas a permissões incompatíveis, se estes estão ligados diretamente ou por meio de herança e ao impedir que os utilizadores de que está a ser atribuídas várias funções que, quando combinados, concederia permissões incompatíveis, novamente por atribuição direta ou por meio de herança. Opcionalmente, pode ser substituído está em conflito.

#### <a name="context-adaptable-permissions"></a>Permissões de contexto adaptáveis

Ao criar permissões automaticamente a podem ser modificadas a com base num atributo de objeto, pode reduzir o número total de permissões que tem de gerir. Permissões de contexto adaptáveis (CAPs) permitem-lhe definir uma fórmula como um atributo de permissão que modifica a forma como a permissão é aplicada pelo aplicativo associado com a permissão. Por exemplo, pode criar uma fórmula que as alterações a permissão de acesso para uma pasta de ficheiros (por meio de um grupo de segurança associado à lista de controlo de acesso da pasta) com base em se um utilizador pertence a uma organização que contém unidade (unidade organizacional) em tempo integral ou de contrato de funcionários. Se o usuário é movido de uma unidade organizacional para outro, a permissão da nova é aplicada automaticamente e a permissão antiga está desativada. 

A fórmula de extremidade pode consultar os valores de atributos que foram aplicados a aplicações, permissões, unidades organizacionais e utilizadores.

#### <a name="attribute-based-authorization"></a>Autorização baseada em atributo

Uma forma para controlar se uma função que está ligada a uma unidade organizacional (unidade organizacional) é ativada para um usuário específico na unidade organizacional está a utilizar a autorização baseada em atributo (ABA). Ao utilizar o ABA, pode ativar automaticamente uma função apenas quando são cumpridas determinadas regras com base em atributos de um utilizador. Por exemplo, pode associar uma função a uma unidade organizacional que se torna ativa para um usuário apenas se o título da tarefa na regra ABA corresponde a cargo do utilizador. Isso elimina a necessidade de ativar manualmente uma função proposta de um utilizador. Em vez disso, uma função pode ser ativada para todos os utilizadores numa unidade organizacional com um valor de atributo que satisfaça a regra de ABA da função. As regras podem ser combinadas, para que uma função é ativada apenas quando os atributos de um utilizador atendem a todas as regras de ABA especificadas para a função.

É importante observar que os resultados de testes de regra ABA são limitados por definições de cardinalidade. Por exemplo, se a definição de cardinalidade de uma regra especifica mais do que dois usuários podem ser atribuídos uma função, e se uma regra de ABA caso contrário, seria ativar uma função para os utilizadores de quatro, a função será ativada apenas para os dois primeiros utilizadores que passam no teste de ABA.

#### <a name="flexible-attribute-types"></a>Tipos de atributo flexível

O sistema de atributos na BHOLD é altamente extensível. Pode definir novos tipos de atributo para esses objetos como usuários, organizacionais unidades (unidades organizacionais) e funções. Atributos podem ser definidos para que os valores que são números inteiros, booleano (Sim/não), alfanumérico, data, hora e e-mail endereços. Atributos podem ser especificados como valores únicos ou uma lista de valores.

## <a name="attestation"></a>Atestado

O BHOLD Suite fornece ferramentas que pode usar para verificar que os utilizadores individuais tem recebidos as permissões adequadas para realizar suas tarefas de negócios. O administrador pode utilizar o portal fornecido pelo módulo do BHOLD Attestation para estruturar uma gerir o processo de atestado.

O processo de atestado é realizado por meio de campanhas na qual campanha stewards a oportunidade e significa que verificar se os utilizadores para os quais são responsáveis têm acesso adequado para aplicações geridas com o BHOLD e as permissões corretas no esses aplicativos. Um proprietário da campanha é designado para supevisionar da campanha e para se certificar de que a campanha está a ser executada corretamente. Uma campanha pode ser criada uma vez ou periodicamente.

Normalmente, steward para uma campanha está gerente, que será atestar os direitos de acesso de utilizadores que pertencem a um ou mais unidades organizacionais para os quais o Gestor de é responsável. Stewards podem ser selecionados automaticamente para os utilizadores que está a ser atestados quanto à presença de uma campanha com base em atributos de utilizador ou os stewards para uma campanha podem ser definidas listando-os num arquivo que mapeia cada utilizador que está a ser atestado na campanha uma steward.

Quando é iniciada uma campanha, BHOLD envia uma mensagem de notificação de e-mail para o stewards de campanha e o proprietário e, em seguida, envia lembretes periódicos para os ajudar a manter o progresso na campanha. Stewards são direcionados para um portal de campanha, onde podem ver uma lista de utilizadores para os quais são responsáveis e as funções que são atribuídas a esses utilizadores. Os stewards, em seguida, podem confirmar se eles são responsáveis por todos os usuários listados e aprovar ou negar os direitos de acesso de todos os usuários listados.

Os proprietários de campanha também utilizam o portal para monitorizar o progresso da campanha e atividades de campanha são registadas para que os proprietários de campanha podem analisar as ações que foram executadas no decorrer da campanha.

## <a name="analytics"></a>Análise

Um das considerações importantes quando a implementação de um sistema de controle (RBAC) de acesso com base em direitos abrangente é o equilíbrio entre manter o controlo de acesso restritos e evitando desnecessários (ou, pior, inesperado) barreiras para aceder. O esforço necessário para manter esse equilíbrio, muitas vezes, resulta numa estrutura de controle de acesso que é tão complexa que inesperado interações entre as políticas são praticamente inevitáveis.

Por esse motivo, é importante ser capaz de analisar os efeitos das políticas de controlo de acesso antes de eles, na verdade, são colocados em vigor. O módulo de análise de BHOLD Suite oferece-está em conformidade com a capacidade de executar essa análise, permitindo-lhe desenvolver regras que representam as suas políticas e, em seguida, que mostra os utilizadores cujas permissões ou conflito com a regra. Com base nesta análise, pode ajustar a política ou modificar funções e permissões para eliminar quaisquer conflitos com a política.

Portal do BHOLD Analytics dá-lhe a capacidade de construir conjuntos de regras que consistem numa ou mais regras que criar para testar uma determinada política ou grupo de políticas. Uma regra inclui as seguintes partes principais:

- Um título e uma descrição que lhe permite identificar e descrevem a regra
- Um status que indica se a regra está pronta para revisão, que está a ser revisto ou aprovado
- Um elemento definido (por exemplo, utilizadores ou permissões) que a regra foi concebida para testar
- Filtros de subconjunto opcional que são expressões que pode utilizar para selecionar um subgrupo apropriado do elemento a ser examinada
- Um ou mais filtros de regra que são expressões que representam a política que está sendo testada.

Uma regra pode testar qualquer um dos seguintes conjuntos de elemento:

- Users
- Unidades organizacionais
- Funções
- Permissões
- Aplicações
- Contas

O diagrama seguinte ilustra uma regra simples que consiste de regras de subconjunto de duas e duas regras de filtro:

![](media/bhold-concepts-guide/rules.png)

Tenha em atenção a diferença no efeito de um filtro de subconjunto de falha e de falha de um filtro de regra: um filtro de subconjunto a falhar remove um objeto de elemento de teste, filtros subsequentes, enquanto um filtro de regra a falhar faz com que o objeto a ser comunicado como não conforme. Apenas os objetos que passam todos os filtros de subconjunto e todos os filtros de regra são comunicados como estando em conformidade.

Cada filtro consiste num tipo, um operador (que é o tipo dependente), uma chave (um dos elementos) e um valor relativamente à qual a chave é testada pela operadora de rede. Por exemplo, o filtro seguinte seria testar se o número de usuários num subconjunto de elemento exceder 10:


|   |   |   |   |   |
|---|---|---|---|---|
|**Tipo:**   | Número de   |
| **Chave:**  | Users  |
| **Operador**  | >  |
| **Valor:** | 10 |

Os filtros de regras podem ter três tipos e usar os operadores específicos ao seu tipo, conforme indicado:

- Atributo
  - < e >
  - = e! =
  - **Contém**
  - **Não contém**
- Número de
  - < e >
  - = e! =
- Restritivas
  - **Tem de ter qualquer um e tem de ter tudo**
  - **Não pode ter qualquer um e não pode ter tudo**
  - **Pode apenas ter qualquer um e apenas pode ter tudo**
  - **Exclusivamente ter qualquer e exclusivamente ter tudo**

> [!Note]
> Filtros restritivos podem utilizar os operadores indicados para testar uma chave com um conjunto de valores múltiplos.

Por exemplo, se quiser testar a implementação de uma segregação de política de tarefas (SoD) que afirma que nenhum usuário que tenha permissão de pagamento do pedido é também ter permissão de pagamento aprovar, poderia construir uma regra semelhante ao seguinte:

|   |  |
|---|--|
|Nome:| Teste de SoD de pagamento|
|Elemento:| Users|
|Filtro de subconjunto:| Com qualquer permissão do pedido de pagamento|
|Filtro de regra: | Não pode ter qualquer permissão aprovar pagamento|

Quando executa esta regra, o módulo do BHOLD Analytics apresenta o número de utilizadores existente no subconjunto selecionado (o número de utilizadores com a permissão de pagamento do pedido), o número de utilizadores que estão em conformidade com a regra e o número de utilizadores que não são compatíveis com a regra. Em seguida, pode apresentar os utilizadores não conformes para que pode corrigir a inconsistência.

Além de exibir os resultados, também pode guardar o relatório como um ficheiro de valores separados por vírgulas (CSV) ou o arquivo XML para que possa analisar os resultados mais tarde. Também pode personalizar o relatório resultante para mostrar informações adicionais que podem ajudá-lo a compreender melhor o impacto. Por exemplo, se estiver a testar utilizadores, pode apresentar (ou de relatório) as contas dos utilizadores em conformidade ou não conformes para que possa ver quais aplicativos estão envolvidos.

Uma vez que uma regra pode conter vários filtros, pode se conectar filtros para testar se uma determinada combinação de condições existe. Por predefinição, o resultado é o produto de um booleano e teste de todos os filtros, mas pode especificar que um teste ou da combinação do filtro ser efetuada.

Por exemplo, se a política de empresa necessitar de gestores de ter a permissão de pagamento de modificar ou a permissão de pagamento aprovar, em seguida, poderia testar conformidade com a política construindo uma regra semelhante ao seguinte:


|  |  |
|--|--|
|Nome: | Modificar o teste de SoD de pagamento|
|Elemento: | Users |
|Filtro de subconjunto: | Ter qualquer função de Gestor|
| Filtros de regra: |Tem de ter qualquer permissão de modificação de pagamento </br> Tem de ter qualquer permissão aprovar pagamento|

Por padrão, qualquer utilizador que é o gerente de que tem o pagamento de modificar e a permissão de pedir pagamento vai ser comunicada como estando em conformidade. No entanto, a política requer que um Gerenciador de ter qualquer permissão, não necessariamente ambos. Para testar a conformidade real com a política, tem de utilizar o operador OR booleano com a regra para determinar se existem quaisquer gerentes que não tem a permissão de pagamento de modificar ou permissão de aprovar o pagamento.

Ao contrário de outros operadores, o **exclusivamente tiver quaisquer** e o **exclusivamente têm todas** operadores indicam a compatibilidade de objetos que caso contrário, teria de ser excluído por um filtro de subconjunto. Por exemplo, para testar uma política que todos os gerentes de (e únicos gerentes) tem a permissão de aprovar revisões, poderia construir uma regra da seguinte forma:

|  |  |
|--|--|
|Nome: | Teste de aprovação de revisão|
|Elemento: | Users|
| Filtro de subconjunto: | Ter qualquer função de Gestor
|Filtro de regra: | Exclusivamente ter qualquer permissão aprovar as revisões|

Esta regra reportará como em conformidade com os utilizadores são gerentes e ter a permissão de aprovar revisões e os utilizadores que não são gerentes e que não tem a permissão de aprovar revisões. Por outro lado, os gerentes de que não tem essa permissão e os utilizadores que não são gerentes, mas tem essa permissão são comunicados como não conforme.

Conforme observado anteriormente, pode combinar regras num conjunto de regras, tornando mais fácil para categorizar e gerir regras para cumprir os requisitos comerciais.

Também pode definir um conjunto global de filtros que, quando ativada, aplicam-se a qualquer regra que está a ser testada. Se a frequência tem de excluir um subconjunto específico de registos quando testa as regras em diferentes conjuntos de regras, pode especificar filtros globais que pode ativar ou desativar conforme necessário.

## <a name="reporting"></a>Relatórios

O módulo de relatórios do BHOLD dá-lhe a capacidade de ver informações no modelo de função através de uma variedade de relatórios. O módulo do BHOLD Reporting fornece um amplo conjunto de relatórios incorporados, além disso, ele inclui um assistente que pode utilizar para criar relatórios personalizados básicos e avançados. Quando executa um relatório, pode exibir os resultados imediatamente ou guardar os resultados num arquivo do Microsoft Excel (. xlsx). Para ver este ficheiro com o Microsoft Excel 2000, o Microsoft Excel 2002 ou o Microsoft Excel 2003, pode transferir e instalar o Microsoft Office Compatibility Pack para o Word, Excel e formatos de arquivo do PowerPoint.


Utilize o módulo de relatórios do BHOLD principalmente para produzir relatórios que se baseiam em informações de função atual. Para gerar relatórios de auditorias de alterações às informações de identidade, o Forefront Identity Manager 2010 R2 tem uma capacidade de relatórios para o serviço FIM que é implementado no armazém de dados do System Center Service Manager. Para obter mais informações sobre o FIM de relatórios, consulte a documentação do Forefront Identity Manager 2010 e o Forefront Identity Manager 2010 R2 na biblioteca técnica do Forefront Identity Management.

Categorias abrangidas por relatórios incorporados incluem o seguinte:

- Administration
- Atestado
- Controles
- Controlo de acesso inward
- Registo
- Model
- Estatísticas
- Fluxo de Trabalho

Pode criar relatórios e adicioná-los a essas categorias, ou pode definir suas próprias categorias em que pode colocar os relatórios incorporados e personalizados.

À medida que cria um relatório, o assistente orienta-o por meio de fornecer os seguintes parâmetros:

- Informações de identificação, incluindo o nome, descrição, categoria, utilização e público-alvo
- Campos a apresentar no relatório
- As consultas que especifique quais os itens são para ser analisado
- Ordem na qual as linhas devem ser ordenados
- Os campos utilizados para quebrar o relatório em secções
- Filtros para refinar os elementos que são devolvidos no relatório

Em cada etapa do assistente, pode visualizar o relatório à medida que defini-lo até aqui e salvá-lo se não tiver de especificar parâmetros adicionais. Pode também mover novamente para passos anteriores para alterar os parâmetros que especificou anteriormente no assistente.

## <a name="access-management-connector"></a>Conector de gestão de acesso

O módulo de conector de gestão de acesso do BHOLD Suite suporta sincronização inicial e contínua de dados em BHOLD. O conector de gestão de acesso funciona com o serviço de sincronização do FIM para mover dados entre a base de dados do BHOLD Core, o metaverso de MIM e aplicativos de destino e armazenamentos de identidades.

As versões anteriores do BHOLD necessária MAs vários controlar o fluxo de dados entre MIM e tabelas de base de dados do BHOLD intermediárias. No SP1 do BHOLD Suite, o conector de gestão de acesso permite definir agentes de gestão (MAs) em MIM que fornecem a transferência de dados diretamente entre BHOLD e MIM.

## <a name="mim-integration"></a>Integração de MIM

Uma funcionalidade poderosa e importante do Forefront Identity Manager 2010 e do Forefront Identity Manager 2010 R2 é o portal self-service que permite aos utilizadores finais ver e gerir as suas informações de identidade e a associação. Integração de MIM estende o Portal de MIM com gestão de funções de autoatendimento. Por exemplo, ao utilizar as funcionalidades do BHOLD no Portal de MIM, um utilizador pode pedir a atribuição de função e pode ver as funções do Active Directory e pedidos pendentes. Capacidades adicionais podem ser concedidas aos administradores delegados, como a capacidade de atribuições de funções de pedido para outros utilizadores.

É importante observar que as extensões do BHOLD ao Portal de MIM suportam a função de self-service e gerenciamento de fluxo de trabalho e geração de relatórios. Outras funções de administração do BHOLD, bem como o atestado, é fornecido pelos portais web dos módulos do BHOLD, que estão alojados em separado a partir do Portal de MIM.

## <a name="next-steps"></a>Passos Seguintes

- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
