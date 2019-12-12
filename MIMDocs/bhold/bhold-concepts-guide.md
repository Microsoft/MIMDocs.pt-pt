---
title: Guia de conceitos do Microsoft BHOLD Suite | Microsoft Docs
description: Comece a trabalhar com os componentes do MIM 2016 ao instalar e configurar o Serviço de Sincronização.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/14/2017
ms.topic: conceptual
ms.assetid: ''
ms.prod: microsoft-identity-manager
ms.openlocfilehash: 3749b74fd867601ee05f8e45d273ad2de9144b5b
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "68701428"
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>Guia de conceitos do Microsoft BHOLD Suite

Microsoft Identity Manager 2016 (MIM) permite que as organizações gerenciem todo o ciclo de vida de identidades de usuário e suas credenciais associadas. Ele pode ser configurado para sincronizar identidades, gerenciar centralmente certificados e senhas e provisionar usuários em sistemas heterogêneos. Com o MIM, as organizações de ti podem definir e automatizar os processos usados para gerenciar identidades da criação à aposentadoria.

O Microsoft BHOLD Suite estende esses recursos do MIM adicionando o controle de acesso baseado em função. O BHOLD permite que as organizações definam funções de usuário e controlem o acesso a dados e aplicativos confidenciais. O acesso é baseado no que é apropriado para essas funções. O BHOLD Suite inclui serviços e ferramentas que simplificam a modelagem das relações de função dentro da organização. O BHOLD mapeia essas funções para direitos e para verificar se as definições de função e os direitos associados são aplicados corretamente aos usuários. Esses recursos são totalmente integrados ao MIM, oferecendo uma experiência direta para os usuários finais e a equipe de ti da mesma forma.

Este guia ajuda você a entender como o BHOLD Suite funciona com o MIM e aborda os seguintes tópicos:

- Controlo de acesso baseado em funções
- Atestado
- Análise
- Relatórios
- Conector de gerenciamento de acesso
- Integração do MIM

## <a name="role-based-access-control"></a>Controlo de acesso baseado em funções

O método mais comum para controlar o acesso do usuário a dados e aplicativos é por meio do DAC (controle de acesso discricionário). Nas implementações mais comuns, cada objeto significativo tem um proprietário identificado. O proprietário tem a capacidade de conceder ou negar acesso ao objeto para outras pessoas com base na identidade individual ou na associação de grupo. Na prática, o DAC normalmente resulta em uma infinidade de grupos de segurança, alguns que refletem a estrutura organizacional, outros que representam agrupamentos funcionais (como tipos de trabalho ou atribuições de projeto) e outros que consistem em coleções provisórias de usuários e dispositivos que são vinculados para fins mais temporários. À medida que as organizações crescem, a associação nesses grupos torna-se cada vez mais difícil de gerenciar. Por exemplo, se um funcionário for transferido de um projeto para outro, os grupos usados para controlar o acesso aos ativos de projetos deverão ser atualizados manualmente. Nesses casos, não é incomum que ocorram erros, erros que podem impedir a segurança ou a produtividade do projeto.

O MIM inclui recursos que ajudam a mitigar esse problema fornecendo controle automatizado sobre associação de grupo e lista de distribuição. No entanto, isso não aborda a complexidade intrínseca da proliferação de grupos que não estão necessariamente relacionados entre si de forma estruturada.

Uma maneira de reduzir significativamente essa proliferação é implantando o RBAC (controle de acesso baseado em função). O RBAC não substitui o DAC.  O RBAC se baseia no DAC fornecendo uma estrutura para classificar usuários e recursos de ti. Isso permite que você torne explícito sua relação e os direitos de acesso apropriados de acordo com essa classificação. Por exemplo, ao atribuir a um usuário atributos que especificam o cargo do usuário e as atribuições de projeto, o usuário pode receber acesso a ferramentas necessárias para o trabalho e os dados de usuários que o usuário precisa para contribuir com um projeto específico. Quando o usuário assume um trabalho diferente e atribuições de projeto diferentes, alterar os atributos que especificam o cargo e os projetos do usuário bloqueia automaticamente o acesso aos recursos necessários apenas para a posição anterior dos usuários.

Como as funções podem ser contidas em funções de maneira hierárquica, (por exemplo, as funções do gerente de vendas e representante de vendas podem estar contidas na função de vendas mais geral), é fácil atribuir direitos apropriados a funções específicas e ainda fornecer direitos apropriados a todos que compartilham também a função mais inclusiva. Por exemplo, em um hospital, todos os funcionários médicos podem ter o direito de exibir registros de pacientes, mas apenas os médicos (uma subfunção da função médica) podem ter o direito de inserir as prescrições para o paciente. Da mesma forma, os usuários que pertencem à função administrativa podem ter acesso negado aos registros de pacientes, exceto para os administradores de cobrança (uma subfunção da função administrativa), que podem ter acesso a essas partes de registros de pacientes necessários para cobrar o paciente pelos serviços fornecido pelo hospital.

Um benefício adicional do RBAC é a capacidade de definir e impor a separação de tarefas (SoD). Isso permite que uma organização defina combinações de funções que concedem permissões que não devem ser mantidas pelo mesmo usuário, para que um usuário específico não possa receber funções que permitam ao usuário iniciar um pagamento e autorizar um pagamento, por exemplo. O RBAC fornece a capacidade de impor uma política desse tipo automaticamente, em vez de ter que avaliar a implementação efetiva da política em uma base por usuário.

### <a name="bhold-role-model-objects"></a>Objetos de modelo de função BHOLD

Com o BHOLD Suite, você pode especificar e organizar funções em sua organização, mapear usuários para funções e mapear as permissões apropriadas para funções. Essa estrutura é chamada de modelo de função e contém e conecta cinco tipos de objetos: 

- Unidades organizacionais
- Users
- Funções
- Permissões
- Aplicações

#### <a name="organizational-units"></a>Unidades organizacionais

As unidades organizacionais (OrgUnits) são as principais médias pelas quais os usuários são organizados no modelo de função BHOLD. Cada usuário deve pertencer a pelo menos um OrgUnit. (Na verdade, quando um usuário é removido da última unidade organizacional no BHOLD, o registro de dados do usuário é excluído do banco de dado BHOLD.)

> [!Important]
> As unidades organizacionais no modelo de função BHOLD não devem ser confundidas com as unidades organizacionais em Active Directory Domain Services (AD DS). Normalmente, a estrutura de unidade organizacional no BHOLD é baseada na organização e nas políticas de sua empresa, não nos requisitos da sua infraestrutura de rede.

Embora não seja necessário, na maioria dos casos, as unidades organizacionais são estruturadas em BHOLD para representar a estrutura hierárquica da organização real, semelhante à seguinte:

![](media/bhold-concepts-guide/org-chart.png)

Neste exemplo, o modelo de função organizationalganizatinal a unidade para a empresa como um todo (representado pelo Presidente, porque o Presidente não faz parte de uma unidade mororganizationalganizatinal) ou a unidade organizacional raiz BHOLD (que sempre Exists) pode ser usado para essa finalidade. O OrgUnits que representa as divisões corporativas de presidentes seria colocado na unidade organizacional corporativa. Em seguida, as unidades organizacionais correspondentes aos diretores de marketing e vendas seriam adicionadas às unidades organizacionais marketing e Sales, e as unidades organizacionais que representam os gerentes de vendas regionais seriam colocadas na unidade organizacional do Gerente de vendas de região leste. As vendas associadas, que não gerenciam outros usuários, se tornaram membros da unidade organizacional do gerente de vendas da região leste. Observe que os usuários podem ser membros de uma unidade organizacional em qualquer nível. Por exemplo, um assistente administrativo, que não é gerente e se reporta diretamente a um Vice-Presidente, seria membro da unidade organizacional do vice-presidente.

Além de representar a estrutura organizacional, as unidades organizacionais também podem ser usadas para agrupar usuários e outras unidades organizacionais de acordo com os critérios funcionais, como para projetos ou especialização. O diagrama a seguir mostra como as unidades organizacionais seriam usadas para agrupar os associados de vendas de acordo com o tipo de cliente:

![](media/bhold-concepts-guide/org-chart-02.png)

Neste exemplo, cada associado de vendas pertenceria a duas unidades organizacionais: uma representando o local da associação na estrutura de gerenciamento da organização e outra que representa a base de clientes da Associação (varejo ou corporativa). Cada unidade organizacional pode receber diferentes funções que, por sua vez, podem receber permissões diferentes para acessar os recursos de ti da organização. Além disso, as funções podem ser herdadas das unidades organizacionais pai, simplificando o processo de propagação de funções para os usuários. Por outro lado, as funções específicas podem ser impedidas de serem herdadas, garantindo que uma função específica esteja associada somente às unidades organizacionais apropriadas.

OrgUnits pode ser criado no BHOLD Suite usando o portal da Web do BHOLD core ou usando o gerador de modelos BHOLD.

#### <a name="users"></a>Users

Conforme observado acima, cada usuário deve pertencer a pelo menos uma OrgUnit (unidade organizacional). Como as unidades organizacionais são o mecanismo principal para associar um usuário a funções, na maioria das organizações, um determinado usuário pertence a vários OrgUnits para facilitar a associação de funções a esse usuário. Em alguns casos, no entanto, pode ser necessário associar uma função a um usuário além de qualquer OrgUnits à qual o usuário pertença. Consequentemente, um usuário pode ser atribuído diretamente a uma função, bem como obter funções do OrgUnits ao qual o usuário pertence.

Quando um usuário não está ativo dentro da organização (embora fora da licença médica, por exemplo), o usuário pode ser suspenso, o que revoga todas as permissões do usuário sem remover o usuário do modelo de função. Ao retornar ao imposto, o usuário pode ser reativado, o que restaura todas as permissões concedidas pelas funções do usuário.

Os objetos para usuários podem ser criados individualmente no BHOLD por meio do portal da Web do BHOLD core ou podem ser importados em massa usando o gerador de modelos do BHOLD ou usando o conector de gerenciamento de acesso com o serviço de sincronização do FIM para importar informações do usuário do fontes como Active Directory Domain Services ou aplicativos de recursos humanos.

Os usuários podem ser criados diretamente no BHOLD sem usar o serviço de sincronização do FIM. Isso pode ser útil ao modelar funções em um ambiente de pré-produção ou de teste. Você também pode permitir que usuários externos (como funcionários de um subcontratado) recebam funções e, portanto, recebam acesso aos recursos de ti sem serem adicionados ao banco de dados de funcionários; no entanto, esses usuários não poderão usar os recursos de autoatendimento do BHOLD.

#### <a name="roles"></a>Funções

Conforme observado anteriormente, no modelo RBAC (controle de acesso baseado em função), as permissões são associadas a funções em vez de usuários individuais. Isso possibilita fornecer a cada usuário as permissões necessárias para executar as tarefas do usuário alterando as funções do usuário em vez de conceder ou negar separadamente as permissões de usuário. Como consequência, a atribuição de permissões não exige mais a participação do departamento de ti, mas em vez disso, pode ser executada como parte do gerenciamento dos negócios. Uma função pode agregar permissões para acessar diferentes sistemas, seja diretamente ou por meio do uso de subfunções, reduzindo ainda mais a necessidade de envolvimento de ti no gerenciamento de permissões de usuário.

É importante observar que as funções são um recurso do próprio modelo RBAC; Normalmente, as funções não são provisionadas para aplicativos de destino. Isso permite que o RBAC seja usado junto com os aplicativos existentes que não são projetados para usar funções ou para alterar as definições de função para atender às necessidades da alteração de modelos de negócios sem precisar modificar os próprios aplicativos. Se um aplicativo de destino for projetado para usar funções, você poderá associar funções no modelo de função BHOLD com funções de aplicativo correspondentes, tratando as funções específicas do aplicativo como permissões.

No BHOLD, você pode atribuir uma função a um usuário principalmente por dois mecanismos:

- Atribuindo uma função a uma unidade organizacional (unidade organizacional) da qual o usuário é membro
- Atribuindo uma função diretamente a um usuário

Uma função atribuída a uma unidade organizacional pai, opcionalmente, pode ser herdada por suas unidades organizacionais Membros. Quando uma função é atribuída ou herdada por uma unidade organizacional, ela pode ser designada como uma função efetiva ou proposta. Se for uma função efetiva, todos os usuários na unidade organizacional serão atribuídos à função. Se for uma função proposta, ela deverá ser ativada para cada usuário ou unidade organizacional membro para se tornar efetiva para os membros da unidade organizacional ou do usuário. Isso possibilita atribuir aos usuários um subconjunto das funções associadas a uma unidade organizacional, em vez de atribuir automaticamente todas as funções da unidade organizacional a todos os membros. Além disso, as funções podem receber datas de início e término, e os limites podem ser colocados na porcentagem de usuários em uma unidade organizacional para a qual uma função pode ser efetivada.

O diagrama a seguir ilustra como um usuário individual pode ser atribuído a uma função no BHOLD:

![](media/bhold-concepts-guide/org-chart-flow.png)

Neste diagrama, A função A é atribuída a uma unidade organizacional como uma função herdável e, portanto, é herdada por suas unidades organizacionais Membros e todos os usuários nessas unidades organizacionais. A função B é atribuída como uma função proposta para uma unidade organizacional. Ele deve ser ativado antes que um usuário na unidade organizacional possa ser autorizado com as permissões da função. A função C é uma função efetiva, portanto, suas permissões se aplicam imediatamente a todos os usuários na unidade organizacional. A função D é vinculada diretamente ao usuário e, portanto, suas permissões se aplicam imediatamente a esse usuário.

Além disso, uma função pode ser ativada para um usuário com base nos atributos de um usuário. Para obter mais informações, consulte autorização baseada em atributo.

#### <a name="permissions"></a>Permissões

Uma permissão em BHOLD corresponde a uma autorização importada de um aplicativo de destino. Ou seja, quando BHOLD é configurado para trabalhar com um aplicativo, ele recebe uma lista de autorizações que o BHOLD pode vincular a funções. Por exemplo, quando Active Directory Domain Services (AD DS) é adicionado ao BHOLD como um aplicativo, ele recebe uma lista de grupos de segurança que, como permissões de BHOLD, podem ser vinculados a funções no BHOLD.

As permissões são específicas para aplicativos. O BHOLD fornece uma única exibição unificada de permissões para que as permissões possam ser associadas a funções sem exigir que os gerentes de função compreendam os detalhes de implementação das permissões. Na prática, diferentes sistemas podem impor uma permissão de forma diferente. O conector específico do aplicativo do serviço de sincronização do FIM para o aplicativo determina como as alterações de permissão para um usuário são fornecidas para esse aplicativo. 

#### <a name="applications"></a>Aplicações

BHOLD implementa um método para aplicar o controle de acesso baseado em função (RBAC) a aplicativos externos. Ou seja, quando BHOLD é provisionado com usuários e permissões de um aplicativo, o BHOLD pode associar essas permissões aos usuários, atribuindo funções aos usuários e, em seguida, vinculando as permissões às funções. Em seguida, o processo em segundo plano do aplicativo pode mapear as permissões corretas para seus usuários com base no mapeamento de função/permissão no BHOLD.

### <a name="developing-the-bhold-suite-role-model"></a>Desenvolvendo o modelo de função do BHOLD Suite

Para ajudá-lo a desenvolver seu modelo de função, o BHOLD Suite fornece o gerador de modelos, uma ferramenta que é fácil de usar e abrangente.

Antes de usar o gerador de modelos, você deve criar uma série de arquivos que definem os objetos que o gerador de modelo usa para construir o modelo de função. Para obter informações sobre como criar esses arquivos, consulte referência técnica do Microsoft BHOLD Suite.

A primeira etapa no uso do gerador de modelos BHOLD é importar esses arquivos para carregar os blocos de construção básicos no gerador de modelo. Quando os arquivos tiverem sido carregados com êxito, você poderá especificar critérios que o gerador de modelos usa para criar várias classes de funções:

- Funções de associação que são atribuídas a um usuário com base em OrgUnits (unidades organizacionais) às quais o usuário pertence
- Funções de atributo que são atribuídas a um usuário com base nos atributos do usuário no banco de dados BHOLD
- Funções propostas que estão vinculadas a uma unidade organizacional, mas devem ser ativadas para usuários específicos
- Funções de propriedade que concedem a um controle de usuário sobre unidades organizacionais e funções para as quais um proprietário não é especificado nos arquivos importados

> [!Important]
> Ao carregar arquivos, marque a caixa de seleção **reter modelo existente** somente em ambientes de teste. Em ambientes de produção, você deve usar o gerador de modelo para criar o modelo de função inicial. Você não pode usá-lo para modificar um modelo de função existente no banco de dados BHOLD.

Depois que o gerador de modelo cria essas funções no modelo de função, você pode exportar o modelo de função para o banco de dados BHOLD na forma de um arquivo XML.

### <a name="advanced-bhold-features"></a>Recursos avançados do BHOLD

As seções anteriores descreveram os recursos básicos do RBAC (controle de acesso baseado em função) no BHOLD. Esta seção descreve os recursos adicionais no BHOLD que podem fornecer segurança e flexibilidade aprimoradas para a implementação de RBAC da sua organização. Esta seção fornece visões gerais dos seguintes recursos do BHOLD:

- Cardinalidade
- Separação de deveres
- Permissões adaptáveis de contexto
- Autorização baseada em atributo
- Tipos de atributo flexíveis


#### <a name="cardinality"></a>Cardinalidade

A *cardinalidade* refere-se à implementação de regras de negócio que são projetadas para limitar o número de vezes que duas entidades podem estar relacionadas entre si. No caso do BHOLD, as regras de cardinalidade podem ser estabelecidas para funções, permissões e usuários.

Você pode configurar uma função para limitar o seguinte:

- O número máximo de usuários para os quais uma função proposta pode ser ativada
- O número máximo de subfunções que podem ser vinculadas à função
- O número máximo de permissões que podem ser vinculadas à função

Você pode configurar uma permissão para limitar o seguinte:

- O número máximo de funções que podem ser vinculadas à permissão
- O número máximo de usuários que podem receber a permissão

Você pode configurar um usuário para limitar o seguinte:

- O número máximo de funções que podem ser vinculadas ao usuário
- O número máximo de permissões que podem ser atribuídas ao usuário por meio de atribuições de função

#### <a name="separation-of-duties"></a>Separação de deveres

A separação de tarefas (SoD) é um princípio de negócios que busca impedir que indivíduos obtenham a capacidade de executar ações que não devem estar disponíveis para uma única pessoa. Por exemplo, um funcionário não pode solicitar um pagamento e autorizar o pagamento. O princípio do SoD permite que as organizações implementem um sistema de verificações e saldos para minimizar sua exposição ao risco de erros ou condutas de funcionários.

O BHOLD implementa o SoD, permitindo que você defina permissões incompatíveis. Quando essas permissões são definidas, o BHOLD impõe SoD, impedindo a criação de funções vinculadas a permissões incompatíveis, estejam elas vinculadas diretamente ou por meio de herança e impedindo que os usuários recebam várias funções que, quando combinado, concederia permissões incompatíveis, novamente por atribuição direta ou por meio de herança. Opcionalmente, os conflitos podem ser substituídos.

#### <a name="context-adaptable-permissions"></a>Permissões adaptáveis de contexto

Ao criar permissões que podem ser modificadas automaticamente com base em um atributo de objeto, é possível reduzir o número total de permissões que você precisa gerenciar. As permissões adaptáveis de contexto (CAPs) permitem que você defina uma fórmula como um atributo de permissão que modifica como a permissão é aplicada pelo aplicativo associado à permissão. Por exemplo, você pode criar uma fórmula que altera a permissão de acesso para uma pasta de arquivos (por meio de um grupo de segurança associado à lista de controle de acesso da pasta) com base em se um usuário pertence a uma unidade organizacional (unidade organizacional) que contém funcionários em tempo integral ou de contrato. Se o usuário for movido de uma unidade organizacional para outra, a nova permissão será aplicada automaticamente e a permissão antiga será desativada. 

A fórmula de extremidade pode consultar os valores de atributos que foram aplicados a aplicativos, permissões, unidades organizacionais e usuários.

#### <a name="attribute-based-authorization"></a>Autorização baseada em atributo

Uma maneira de controlar se uma função vinculada a uma unidade organizacional (unidade organizacional) é ativada para um usuário específico na unidade organizacional é usar a autorização baseada em atributo (ABA). Usando o ABA, você pode ativar automaticamente uma função somente quando determinadas regras com base nos atributos de um usuário são atendidas. Por exemplo, você pode vincular uma função a uma unidade organizacional que se torna ativa para um usuário somente se o cargo do usuário corresponder ao cargo na regra ABA. Isso elimina a necessidade de ativar manualmente uma função proposta para um usuário. Em vez disso, uma função pode ser ativada para todos os usuários em uma unidade organizacional que tenham um valor de atributo que satisfaça a regra de ABA da função. As regras podem ser combinadas, de forma que uma função seja ativada somente quando os atributos de um usuário atenderem a todas as regras de ABA especificadas para a função.

É importante observar que os resultados de testes de regra ABA são limitados pelas configurações de cardinalidade. Por exemplo, se a configuração de cardinalidade de uma regra especificar que não é possível atribuir uma função a mais de dois usuários, e se uma regra ABA, caso contrário, ativaria uma função para quatro usuários, a função será ativada somente para os primeiros dois usuários que passarem no teste ABA.

#### <a name="flexible-attribute-types"></a>Tipos de atributo flexíveis

O sistema de atributos no BHOLD é altamente extensível. Você pode definir novos tipos de atributo para esses objetos como usuários, unidades organizacionais (unidades organizacionais) e funções. Os atributos podem ser definidos para ter valores inteiros, Boolianos (Sim/não), alfanuméricos, de data, hora e endereços de email. Os atributos podem ser especificados como valores únicos ou uma lista de valores.

## <a name="attestation"></a>Atestado

O BHOLD Suite fornece ferramentas que você pode usar para verificar se usuários individuais receberam as permissões apropriadas para realizar suas tarefas comerciais. O administrador pode usar o portal fornecido pelo módulo de atestado BHOLD para criar um gerenciar o processo de atestado.

O processo de atestado é conduzido por meio de campanhas nas quais os administradores de campanha recebem a oportunidade e significa verificar se os usuários para os quais eles são responsáveis têm acesso apropriado a aplicativos gerenciados por BHOLD e as permissões corretas em esses aplicativos. Um proprietário de campanha é designado para supervisionar a campanha e garantir que a campanha esteja sendo executada corretamente. Uma campanha pode ser criada para ocorrer uma vez ou em uma base recorrente.

Normalmente, o administrador de uma campanha será um gerente que atestará os direitos de acesso de usuários que pertencem a uma ou mais unidades organizacionais para as quais o gerente é responsável. Os administradores podem ser selecionados automaticamente para os usuários que estão sendo atestados em uma campanha com base em atributos de usuário, ou os administradores de uma campanha podem ser definidos por meio de sua listagem em um arquivo que mapeia cada usuário que está sendo atestado na campanha para um administrador.

Quando uma campanha é iniciada, o BHOLD envia uma mensagem de notificação por email para os administradores e o proprietário da campanha e, em seguida, envia lembretes periódicos para ajudá-los a manter o progresso na campanha. Os administradores são direcionados para um portal de campanha, onde podem ver uma lista dos usuários para os quais eles são responsáveis e as funções atribuídas a esses usuários. Os administradores podem confirmar se são responsáveis por cada um dos usuários listados e aprovar ou negar os direitos de acesso de cada um dos usuários listados.

Os proprietários da campanha também usam o portal para monitorar o progresso da campanha e as atividades de campanha são registradas para que os proprietários da campanha possam analisar as ações que foram executadas no decorrer da campanha.

## <a name="analytics"></a>Análise

Uma das considerações importantes ao implementar um sistema de RBAC (controle de acesso baseado em direitos) abrangente é o equilíbrio entre manter o controle de acesso estrito e evitar barreiras desnecessárias (ou piores, inesperadas) para o acesso. O esforço para manter esse equilíbrio geralmente resulta em uma estrutura de controle de acesso tão complexa que as interações inesperadas entre as políticas são praticamente inevitáveis.

Por esse motivo, é importante poder analisar os efeitos das políticas de controle de acesso antes que elas sejam realmente colocadas em vigor. O módulo de análise do BHOLD Suite oferece a capacidade de executar essa análise, permitindo que você desenvolva regras que representem suas políticas e, em seguida, mostrem os usuários cujas permissões estão em conformidade ou entrem em conflito com a regra. Com base nessa análise, você pode ajustar a política ou modificar funções e permissões para eliminar conflitos com a política.

O portal do BHOLD Analytics oferece a capacidade de construir conjuntos de regras que consistem em uma ou mais regras que você cria para testar uma determinada política ou grupo de políticas. Uma regra consiste nas seguintes partes principais:

- Um título e uma descrição que permitem identificar e descrever a regra
- Um status que indica se a regra está pronta para revisão, sendo revisada ou aprovada
- Um conjunto de elementos (como usuários ou permissões) que a regra foi projetada para teste
- Filtros de subconjunto opcionais que são expressões que você pode usar para selecionar um subgrupo apropriado do elemento a ser examinado
- Um ou mais filtros de regra que são expressões que representam a política que está sendo testada.

Uma regra pode testar qualquer um dos seguintes conjuntos de elementos:

- Users
- Unidades organizacionais
- Funções
- Permissões
- Aplicações
- Contas

O diagrama a seguir ilustra uma regra simples que consiste em duas regras de subconjunto e duas regras de filtro:

![](media/bhold-concepts-guide/rules.png)

Observe a diferença no efeito de falha de um filtro de subconjunto e de falha em um filtro de regra: a falha de um filtro de subconjunto remove um objeto do elemento do teste pelos filtros subsequentes, enquanto a falha de um filtro de regra faz com que o objeto seja relatado como não compatível. Somente os objetos que passam todos os filtros de subconjunto e todos os filtros de regra são relatados como em conformidade.

Cada filtro consiste em um tipo, um operador (que é dependente de tipo), uma chave (um dos elementos) e um valor em relação ao qual a chave é testada pelo operador. Por exemplo, o seguinte filtro testaria se o número de usuários em um subconjunto de elementos excede 10:


|   |   |   |   |   |
|---|---|---|---|---|
|**Tipo:**   | Número de   |
| **Chaves**  | Users  |
| **Operador**  | >  |
| **Valor:** | 10 |

Os filtros de regras podem ser de três tipos e usam operadores específicos para seu tipo, conforme indicado:

- Atributo
  - < e >
  - = e! =
  - **Contém**
  - **Não contém**
- Número de
  - < e >
  - = e! =
- Restritiva
  - **Deve ter qualquer e deve ter todos**
  - **Não pode ter nenhum e não pode ter todos**
  - **Só pode ter qualquer e só pode ter todos**
  - **Ter exclusivamente any e exclusivamente ter todos**

> [!Note]
> Filtros restritivos podem usar os operadores indicados para testar uma chave em relação a um conjunto de vários valores.

Por exemplo, se você quisesse testar a implementação de uma política de SoD (segregação de tarefas) que afirma que nenhum usuário que tem a permissão solicitar pagamento também tem a permissão aprovar pagamento, você pode construir uma regra semelhante à seguinte:

|   |  |
|---|--|
|Nome:| Teste de SoD de pagamento|
|Elementos| Users|
|Filtro de subconjunto:| Tendo um pagamento de solicitação de permissão|
|Filtro de regra: | Não é possível ter nenhuma permissão para aprovar o pagamento|

Quando você executa essa regra, o módulo análise do BHOLD exibe o número de usuários no subconjunto selecionado (o número de usuários com a permissão solicitar pagamento), o número de usuários que estão em conformidade com a regra e o número de usuários que não estão em conformidade com a regra. Em seguida, é possível exibir os usuários incompatíveis para que você possa corrigir a inconsistência.

Além de exibir os resultados, você também pode salvar o relatório como um arquivo de valores separados por vírgulas (CSV) ou XML para permitir que você analise os resultados posteriormente. Você também pode personalizar o relatório resultante para mostrar informações adicionais que podem ajudá-lo a entender melhor o impacto. Por exemplo, se você estiver testando usuários, poderá exibir (ou relatar) as contas dos usuários em conformidade ou em não conformidade para que você possa ver quais aplicativos estão envolvidos.

Como uma regra pode conter vários filtros, você pode conectar filtros para testar se há uma determinada combinação de condições. Por padrão, o resultado é o produto de um teste booliano e de todos os filtros, mas você pode especificar que um teste ou da combinação de filtro seja executado.

Por exemplo, se sua política de negócios exigir que os gerentes tenham a permissão Modificar pagamento ou a permissão aprovar pagamento, você poderá testar a conformidade com a política construindo uma regra como a seguinte:


|  |  |
|--|--|
|Nome: | Modificar teste de SoD de pagamento|
|Elementos | Users |
|Filtro de subconjunto: | Tendo qualquer Gerenciador de função|
| Filtros de regra: |Deve ter qualquer permissão Modificar pagamento </br> Deve ter qualquer permissão aprovar pagamento|

Por padrão, qualquer usuário que seja um gerente que tenha a permissão Modificar pagamento e solicitar pagamento será relatado como em conformidade. No entanto, a política requer que um gerente tenha uma das permissões, não necessariamente ambas. Para testar a conformidade real com a política, você deve usar o operador booliano OR com a regra para determinar se há algum gerente que não tenha a permissão Modificar pagamento ou aprovar a permissão de pagamento.

Ao contrário de outros operadores, o **exclusivo tem any** e, **exclusivamente, todos os** operadores indicam a conformidade para objetos que, de outra forma, seriam excluídos por um filtro de subconjunto. Por exemplo, para testar uma política que todos os gerentes (e somente gerentes) têm a permissão aprovar revisões, você pode construir uma regra da seguinte maneira:

|  |  |
|--|--|
|Nome: | Examinar teste de aprovação|
|Elementos | Users|
| Filtro de subconjunto: | Tendo qualquer Gerenciador de função
|Filtro de regra: | Ter exclusivamente qualquer permissão para aprovar revisões|

Esta regra relatará como usuários em conformidade que são gerentes e têm a permissão aprovar revisões e os usuários que não são gerentes e que não têm a permissão aprovar revisões. Por outro lado, os gerentes que não têm essa permissão e os usuários que não são gerentes, mas têm essa permissão são relatados como não compatíveis.

Conforme observado anteriormente, você pode combinar regras em um RuleSet, facilitando a categorização e o gerenciamento de regras para atender às suas necessidades de negócios.

Você também pode definir um conjunto de filtros globais que, quando habilitados, se aplicam a qualquer regra testada. Se, com frequência, você precisa excluir um subconjunto específico de registros ao testar regras em diferentes RuleSets, você pode especificar filtros globais que podem ser habilitados ou desabilitados conforme necessário.

## <a name="reporting"></a>Relatórios

O módulo relatório do BHOLD fornece a capacidade de exibir informações no modelo de função por meio de uma variedade de relatórios. O módulo relatório do BHOLD fornece um amplo conjunto de relatórios internos, além de incluir um assistente que pode ser usado para criar relatórios personalizados básicos e avançados. Ao executar um relatório, você pode exibir imediatamente os resultados ou salvar os resultados em um arquivo do Microsoft Excel (. xlsx). Para exibir esse arquivo usando o Microsoft Excel 2000, o Microsoft Excel 2002 ou o Microsoft Excel 2003, você pode baixar e instalar o pacote de compatibilidade Microsoft Office para formatos de arquivo do Word, Excel e PowerPoint.


Você usa o módulo de relatório do BHOLD principalmente para produzir relatórios com base nas informações de função atuais. Para gerar relatórios para auditoria de alterações em informações de identidade, o Forefront Identity Manager 2010 R2 tem um recurso de relatório para o serviço FIM que é implementado no data warehouse de System Center Service Manager. Para obter mais informações sobre relatórios do FIM, consulte a documentação do Forefront Identity Manager 2010 e do Forefront Identity Manager 2010 R2 na biblioteca técnica do Forefront Identity Management.

As categorias cobertas pelos relatórios internos incluem o seguinte:

- Administration
- Atestado
- Controlos
- Controle de acesso interno
- Registo
- Model
- Estatísticas
- Fluxo de Trabalho

Você pode criar relatórios e adicioná-los a essas categorias, ou pode definir suas próprias categorias nas quais você pode inserir relatórios internos e personalizados.

À medida que você cria um relatório, o assistente o orienta durante o fornecimento dos seguintes parâmetros:

- Informações de identificação, incluindo nome, descrição, categoria, uso e público-alvo
- Campos a serem exibidos no relatório
- Consultas que especificam quais itens devem ser analisados
- Ordem na qual as linhas devem ser classificadas
- Campos usados para dividir o relatório em seções
- Filtros para refinar os elementos que são retornados no relatório

Em cada estágio do assistente, você pode visualizar o relatório enquanto o definiu até agora e salvá-lo se não precisar especificar parâmetros adicionais. Você também pode retornar às etapas anteriores para alterar os parâmetros especificados anteriormente no assistente.

## <a name="access-management-connector"></a>Conector de gerenciamento de acesso

O módulo do conector de gerenciamento de acesso do BHOLD Suite dá suporte à sincronização inicial e contínua de dados no BHOLD. O conector de gerenciamento de acesso funciona com o serviço de sincronização do FIM para mover dados entre o BHOLD Core Database, o metaverso do MIM e os aplicativos de destino e armazenamentos de identidade.

As versões anteriores do BHOLD exigiam vários MAs para controlar o fluxo de dados entre as tabelas do banco de dado do MIM e do BHOLD intermediário. No BHOLD Suite SP1, o conector de gerenciamento de acesso permite que você defina os agentes de gerenciamento (MAs) no MIM que fornecem transferência de dados diretamente entre BHOLD e MIM.

## <a name="mim-integration"></a>Integração do MIM

Um recurso importante e poderoso do Forefront Identity Manager 2010 e do Forefront Identity Manager 2010 R2 é o portal de autoatendimento que permite que os usuários finais exibam e gerenciem suas informações de identidade e associação. A integração do MIM estende o portal do MIM com o gerenciamento de função de autoatendimento. Por exemplo, usando os recursos do BHOLD no portal do MIM, um usuário pode solicitar a atribuição de função e pode exibir funções ativas e solicitações pendentes. Recursos adicionais podem ser concedidos a administradores delegados, como a capacidade de solicitar atribuições de função para outros usuários.

É importante observar que as extensões BHOLD para o portal do MIM dão suporte a funções de autoatendimento e gerenciamento de fluxo de trabalho e relatórios. Outras funções de administração do BHOLD, bem como atestado, são fornecidas pelos portais da Web dos módulos do BHOLD, que são hospedados separadamente do portal do MIM.

## <a name="next-steps"></a>Próximos passos

- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
