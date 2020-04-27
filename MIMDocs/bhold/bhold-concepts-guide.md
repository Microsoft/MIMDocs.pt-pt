---
title: Guia de conceitos da Suite Microsoft BHOLD [ Microsoft Docs
description: Comece a trabalhar com os componentes do MIM 2016 ao instalar e configurar o Serviço de Sincronização.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/14/2017
ms.topic: conceptual
ms.assetid: ''
ms.prod: microsoft-identity-manager
ms.openlocfilehash: f120709e517d82d4f94e72f4d0a44361f5552a1c
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042309"
---
# <a name="microsoft-bhold-suite-concepts-guide"></a>Guia de conceitos da Suite BHOLD da Microsoft

O Microsoft Identity Manager 2016 (MIM) permite que as organizações gerem todo o ciclo de vida das identidades dos utilizadores e das suas credenciais associadas. Pode ser configurado para sincronizar identidades, gerir centralmente certificados e senhas e fornecer utilizadores em sistemas heterogéneos. Com mim, as organizações de TI podem definir e automatizar os processos usados para gerir identidades desde a criação até à reforma.

O Microsoft BHOLD Suite alarga estas capacidades de MIM adicionando controlo de acesso baseado em papéis. O BHOLD permite que as organizações definam as funções dos utilizadores e controlem o acesso a dados e aplicações sensíveis. O acesso baseia-se no que é adequado para essas funções. A BHOLD Suite inclui serviços e ferramentas que simplificam a modelação das relações de papel dentro da organização. A BHOLD mapeia essas funções nos direitos e verifica que as definições de papel e os direitos associados são corretamente aplicados aos utilizadores. Estas capacidades estão totalmente integradas com mim, proporcionando uma experiência perfeita para utilizadores finais e pessoal de TI.

Este guia ajuda-o a entender como a BHOLD Suite trabalha com mim e aborda os seguintes tópicos:

- Controlo de acesso baseado em funções
- Atestação
- Análise
- Relatórios
- Conector de Gestão de Acesso
- Integração MIM

## <a name="role-based-access-control"></a>Controlo de acesso baseado em funções

O método mais comum para controlar o acesso dos utilizadores a dados e aplicações é através do controlo discricionário de acesso (DAC). Na maioria das implementações comuns, cada objeto significativo tem um proprietário identificado. O proprietário tem a capacidade de conceder ou negar o acesso ao objeto a outros com base na identidade individual ou na adesão ao grupo. Na prática, o DAC normalmente resulta numa panóplia de grupos de segurança, alguns que refletem a estrutura organizacional, outros que representam agrupamentos funcionais (como tipos de trabalho ou atribuições de projetos), e outros que consistem em coleções improvisadas de utilizadores e dispositivos que estão ligados para fins mais temporários. À medida que as organizações crescem, a adesão a estes grupos torna-se cada vez mais difícil de gerir. Por exemplo, se um trabalhador for transferido de um projeto para outro, os grupos que são utilizados para controlar o acesso aos ativos dos projetos devem ser atualizados manualmente. Nestes casos, não é incomum que ocorram erros, erros que podem impedir a segurança do projeto ou a produtividade.

A MIM inclui funcionalidades que ajudam a mitigar este problema, fornecendo controlo automatizado sobre a adesão ao grupo e à lista de distribuição. No entanto, isto não aborda a complexidade intrínseca de grupos em proliferação que não estão necessariamente relacionados uns com os outros de forma estruturada.

Uma forma de reduzir significativamente esta proliferação é através da implementação do controlo de acesso baseado em funções (RBAC). O RBAC não desloca o DAC.  O RBAC baseia-se no DAC, fornecendo um quadro para classificar os utilizadores e os recursos de TI. Isto permite-lhe explicitar a sua relação e os direitos de acesso que são adequados de acordo com essa classificação. Por exemplo, atribuindo a um utilizador atributos que especificam o título de trabalho dos utilizadores e as atribuições de projeto, o utilizador pode ter acesso às ferramentas necessárias para o trabalho do utilizador e dados que o utilizador precisa para contribuir para um determinado projeto. Quando o utilizador assume um trabalho diferente e diferentes atribuições de projeto, alterando os atributos que especificam o título de trabalho do utilizador e os projetos bloqueiam automaticamente o acesso aos recursos necessários apenas para a posição anterior dos utilizadores.

Uma vez que as funções podem ser contidas dentro de funções de forma hierárquica (por exemplo, as funções de gestor de vendas e representante de vendas podem ser contidas no papel mais geral das vendas), é fácil atribuir direitos adequados a papéis específicos e, no entanto, ainda assim fornecer direitos adequados a todos os que também partilham o papel mais inclusivo. Por exemplo, num hospital, todo o pessoal médico poderia ter o direito de ver os registos dos pacientes, mas apenas os médicos (uma subfunção do papel médico) poderiam ter o direito de introduzir prescrições para o paciente. Da mesma forma, os utilizadores pertencentes ao papel clerical poderiam ser impedidos de aceder aos registos dos pacientes, com exceção dos funcionários de faturação (uma subfunção do papel clerical), a quem seria concedido acesso às porções de registos de pacientes que são obrigados a cobrar ao doente os serviços prestados pelo hospital.

Um benefício adicional do RBAC é a capacidade de definir e impor a separação de direitos (SoD). Isto permite que uma organização defina combinações de funções que concedem permissões que não devem ser detidas pelo mesmo utilizador, de modo a que um determinado utilizador não possa ser atribuído funções que permitam ao utilizador iniciar um pagamento e autorizar um pagamento, por exemplo. O RBAC oferece a capacidade de aplicar automaticamente essa política em vez de ter de avaliar a aplicação efetiva da política numa base por utilizador.

### <a name="bhold-role-model-objects"></a>Objetos de modelo de papel BHOLD

Com o BHOLD Suite, pode especificar e organizar funções dentro da sua organização, mapear utilizadores para papéis e mapear permissões apropriadas para papéis. Esta estrutura é chamada de modelo, e contém e conecta cinco tipos de objetos: 

- Unidades organizacionais
- Utilizadores
- Funções
- Permissões
- Aplicações

#### <a name="organizational-units"></a>Unidades organizacionais

As unidades organizacionais (OrgUnits) são o principal meio pelo qual os utilizadores estão organizados no modelo de função BHOLD. Todos os utilizadores devem pertencer a pelo menos uma OrgUnit. (Na verdade, quando um utilizador é removido da última unidade organizacional em BHOLD, o registo de dados do utilizador é eliminado da base de dados BHOLD.)

> [!Important]
> As unidades organizacionais do modelo de papel BHOLD não devem ser confundidas com unidades organizacionais em Serviços de Domínio de Diretório Ativo (AD DS). Tipicamente, a estrutura de unidade organizacional no BHOLD baseia-se na organização e políticas do seu negócio, e não nos requisitos da sua infraestrutura de rede.

Embora não seja necessário, na maioria dos casos as unidades organizacionais são estruturadas em BHOLD para representar a estrutura hierárquica da organização real, semelhante à abaixo:

![](media/bhold-concepts-guide/org-chart.png)

Nesta amostra, o modelo seria organizacional para a corporação como um todo (representado pelo presidente, porque o presidente não faz parte de uma unidade mororganizaçãoganizatina), ou a unidade organizacional de raiz BHOLD (que sempre existe) poderia ser usada para esse fim. As OrgUnits que representam as divisões corporativas chefiadas pelos vice-presidentes seriam colocadas na unidade organizacional corporativa. Em seguida, as unidades organizacionais correspondentes aos diretores de marketing e vendas seriam adicionadas às unidades organizacionais de marketing e vendas, e as unidades organizacionais representativas dos gestores regionais de vendas seriam colocadas na unidade organizacional do gestor de vendas da região leste. Os associados de vendas, que não gerem outros utilizadores, seriam membros da unidade organizacional do gestor de vendas da região leste. Note que os utilizadores podem ser membros de uma unidade organizacional a qualquer nível. Por exemplo, um assistente administrativo, que não é gerente e reporta diretamente a um vice-presidente, seria membro da unidade organizacional do vice-presidente.

Além de representar a estrutura organizacional, as unidades organizacionais também podem ser usadas para agrupar utilizadores e outras unidades organizacionais de acordo com critérios funcionais, como projetos ou especialização. O diagrama seguinte mostra como as unidades organizacionais seriam usadas para agrupar associados de vendas de acordo com o tipo de cliente:

![](media/bhold-concepts-guide/org-chart-02.png)

Neste exemplo, cada associado de vendas pertenceria a duas unidades organizacionais: uma representando o lugar do associado na estrutura de gestão da organização, e outra representando a base de clientes do associado (retalho ou empresa). Cada unidade organizacional pode ser atribuída a diferentes funções que, por sua vez, podem ser atribuídas diferentes permissões para aceder aos recursos de TI da organização. Além disso, as funções podem ser herdadas das unidades organizacionais-mãe, simplificando o processo de propagação de funções para os utilizadores. Por outro lado, podem ser evitadas funções específicas, garantindo que um papel específico seja associado apenas às unidades organizacionais adequadas.

As OrgUnits podem ser criadas no BHOLD Suite utilizando o portal web BHOLD Core ou utilizando o Gerador do Modelo BHOLD.

#### <a name="users"></a>Utilizadores

Como referido acima, cada utilizador deve pertencer a pelo menos uma unidade organizacional (OrgUnit). Como as unidades organizacionais são o principal mecanismo para associar um utilizador a funções, na maioria das organizações um determinado utilizador pertence a várias OrgUnits para facilitar a associação de papéis com esse utilizador. Em alguns casos, no entanto, pode ser necessário associar uma função a um utilizador para além de qualquer OrgUnits a que o utilizador pertença. Consequentemente, um utilizador pode ser atribuído diretamente a uma função, bem como obter funções a partir das OrgUnits a que o utilizador pertence.

Quando um utilizador não está ativo dentro da organização (enquanto estiver fora para licença médica, por exemplo), o utilizador pode ser suspenso, o que revoga todas as permissões do utilizador sem remover o utilizador do modelo de função. Ao voltar ao serviço, o utilizador pode ser reativado, o que restaura todas as permissões concedidas pelas funções do utilizador.

Os objetos para os utilizadores podem ser criados individualmente no BHOLD através do portal Web BHOLD Core, ou podem ser importados a granel utilizando o Gerador de Modelos BHOLD, ou utilizando o Connector de Gestão de Acesso com o Serviço de Sincronização FIM para importar informações dos utilizadores de fontes como serviços de domínio de diretório ativo ou aplicações de recursos humanos.

Os utilizadores podem ser criados diretamente no BHOLD sem utilizar o Serviço de Sincronização FIM. Isto pode ser útil quando modelar papéis em ambiente de pré-produção ou teste. Também pode permitir que utilizadores externos (como empregados de um subcontratado) sejam atribuídos a funções e, assim, ter acesso aos recursos de TI sem serem adicionados à base de dados dos colaboradores; no entanto, estes utilizadores não poderão utilizar as funcionalidades de self-service BHOLD.

#### <a name="roles"></a>Funções

Como anteriormente notado, no âmbito do modelo de controlo de acesso baseado em funções (RBAC), as permissões estão associadas a funções e não a utilizadores individuais. Isto permite dar a cada utilizador as permissões necessárias para desempenhar as funções do utilizador alterando as funções do utilizador em vez de conceder ou negar separadamente as permissões do utilizador. Como consequência, a atribuição de permissões já não requer a participação do departamento de TI, mas pode ser realizada como parte da gestão do negócio. Uma função pode agregar permissões para aceder a diferentes sistemas, quer diretamente quer através da utilização de subfunções, reduzindo ainda mais a necessidade de envolvimento de TI na gestão das permissões dos utilizadores.

É importante notar que as funções são uma característica do próprio modelo RBAC; normalmente, as funções não são previstas para aplicações-alvo. Isto permite que o RBAC seja utilizado juntamente com aplicações existentes que não foram concebidas para utilizar funções ou para alterar as definições de papéis, atender às necessidades de mudança de modelos de negócio sem ter de modificar as próprias aplicações. Se uma aplicação-alvo for concebida para utilizar funções, então pode associar funções no modelo de função BHOLD com as funções de aplicação correspondentes, tratando as funções específicas da aplicação como permissões.

No BHOLD, pode atribuir uma função a um utilizador principalmente através de dois mecanismos:

- Atribuindo um papel a uma unidade organizacional (unidade organizacional) da qual o utilizador é membro
- Atribuindo uma função diretamente a um utilizador

Um papel atribuído a uma unidade organizacional-mãe opcionalmente pode ser herdado pelas suas unidades organizacionais membros. Quando um papel é atribuído ou herdado por uma unidade organizacional, pode ser designado como um papel eficaz ou proposto. Se for um papel eficaz, todos os utilizadores da unidade organizacional são atribuídos ao papel. Se for uma função proposta, deve ser ativada para que cada utilizador ou unidade organizacional membro se torne eficaz para os membros desse utilizador ou unidade organizacional. Isto permite atribuir aos utilizadores um subconjunto das funções associadas a uma unidade organizacional, em vez de atribuir automaticamente todas as funções da unidade organizacional a todos os membros. Além disso, podem ser dadas funções datas de início e fim, e podem ser colocados limites na percentagem de utilizadores dentro de uma unidade organizacional para a qual uma função pode ser eficaz.

O diagrama que se segue ilustra como um utilizador individual pode ser atribuído a uma função no BHOLD:

![](media/bhold-concepts-guide/org-chart-flow.png)

Neste diagrama, o papel A é atribuído a uma unidade organizacional como um papel hereditário, e assim é herdado pelas suas unidades organizacionais membros e todos os utilizadores dentro dessas unidades organizacionais. O papel B é atribuído como um papel proposto para uma unidade organizacional. Deve ser ativado antes que um utilizador da unidade organizacional possa ser autorizado com as permissões do papel. O papel C é um papel eficaz, pelo que as suas permissões se aplicam imediatamente a todos os utilizadores da unidade organizacional. A função D está ligada diretamente ao utilizador e, por isso, as suas permissões aplicam-se imediatamente a esse utilizador.

Além disso, uma função pode ser ativada para um utilizador com base nos atributos de um utilizador. Para mais informações, consulte a autorização baseada no Atributo.

#### <a name="permissions"></a>Permissões

Uma autorização no BHOLD corresponde a uma autorização importada de um pedido-alvo. Isto é, quando o BHOLD está configurado para trabalhar com uma aplicação, recebe uma lista de autorizações que o BHOLD pode ligar às funções. Por exemplo, quando os Serviços de Domínio do Diretório Ativo (AD DS) são adicionados ao BHOLD como uma aplicação, recebe uma lista de grupos de segurança que, como permissões BHOLD, podem estar ligados a funções no BHOLD.

As permissões são específicas das aplicações. O BHOLD fornece uma visão única e unificada das permissões para que as permissões possam ser associadas a funções sem exigir que os gestores de papéis compreendam os detalhes de implementação das permissões. Na prática, diferentes sistemas podem impor uma permissão de forma diferente. O conector específico da aplicação do Serviço de Sincronização FIM para a aplicação determina como as alterações de permissão para um utilizador são fornecidas a essa aplicação. 

#### <a name="applications"></a>Aplicações

A BHOLD implementa um método de aplicação do controlo de acesso baseado em funções (RBAC) a aplicações externas. Isto é, quando o BHOLD é aprovisionado com utilizadores e permissões a partir de uma aplicação, o BHOLD pode associar essas permissões aos utilizadores, atribuindo funções aos utilizadores e ligando as permissões às funções. O processo de fundo da aplicação pode então mapear as permissões corretas aos seus utilizadores com base no mapeamento de papel/permissão no BHOLD.

### <a name="developing-the-bhold-suite-role-model"></a>Desenvolvimento do modelo de papel BHOLD Suite

Para ajudá-lo a desenvolver o seu modelo, o BHOLD Suite fornece Model Generator, uma ferramenta que é fácil de usar e abrangente.

Antes de utilizar o Gerador de Modelos, tem de criar uma série de ficheiros que definam os objetos que o Model Generator utiliza para construir o modelo. Para obter informações sobre como criar estes ficheiros, consulte a Referência Técnica da Microsoft BHOLD Suite.

O primeiro passo para a utilização do Gerador de Modelos BHOLD é importar estes ficheiros para carregar os blocos básicos de construção no Gerador de Modelos. Quando os ficheiros tiverem sido carregados com sucesso, pode então especificar critérios que o Model Generator utiliza para criar várias classes de funções:

- Funções de adesão atribuídas a um utilizador com base nas OrgUnits (unidades organizacionais) a que o utilizador pertence
- Atributo funções atribuídas a um utilizador com base nos atributos do utilizador na base de dados BHOLD
- Funções propostas que estão ligadas a uma unidade organizacional mas devem ser ativadas para utilizadores específicos
- Funções de propriedade que concedem um controlo do utilizador sobre unidades organizacionais e funções para as quais um proprietário não é especificado nos ficheiros importados

> [!Important]
> Ao carregar ficheiros, selecione a caixa de verificação do **Modelo Retentor existente** apenas em ambientes de teste. Em ambientes de produção, é preciso utilizar o Model Generator para criar o modelo inicial. Não é possível utilizá-lo para modificar um modelo de papel existente na base de dados BHOLD.

Depois de o Model Generator criar estas funções no modelo, pode então exportar o modelo para a base de dados BHOLD sob a forma de um ficheiro XML.

### <a name="advanced-bhold-features"></a>Funcionalidades Avançadas de BHOLD

Secções anteriores descreveram as características básicas do controlo de acesso baseado em funções (RBAC) em BHOLD. Esta secção descreve funcionalidades adicionais no BHOLD que podem proporcionar uma maior segurança e flexibilidade à implementação do RBAC pela sua organização. Esta secção fornece visões gerais das seguintes funcionalidades BHOLD:

- Cardinalidade
- Separação de direitos
- Permissões adaptáveis ao contexto
- Autorização baseada no atributo
- Tipos de atributos flexíveis


#### <a name="cardinality"></a>Cardinalidade

*Cardinalidade* refere-se à implementação de regras empresariais que visam limitar o número de vezes que duas entidades podem estar relacionadas entre si. No caso do BHOLD, podem ser estabelecidas regras de cardinalidade para funções, permissões e utilizadores.

Pode configurar uma função para limitar o seguinte:

- O número máximo de utilizadores para os quais uma função proposta pode ser ativada
- O número máximo de subfunções que podem estar ligados ao papel
- O número máximo de permissões que podem estar ligadas ao papel

Pode configurar uma permissão para limitar o seguinte:

- O número máximo de funções que podem ser ligadas à permissão
- O número máximo de utilizadores que podem ser autorizados

Pode configurar um utilizador para limitar o seguinte:

- O número máximo de funções que podem ser ligadas ao utilizador
- O número máximo de permissões que podem ser atribuídas ao utilizador através de atribuições de funções

#### <a name="separation-of-duties"></a>Separação de direitos

A separação de deveres (SoD) é um princípio comercial que procura impedir que os indivíduos ganhem a capacidade de realizar ações que não devem estar disponíveis para uma única pessoa. Por exemplo, um trabalhador não deve poder solicitar um pagamento e autorizar o pagamento. O princípio da SoD permite que as organizações implementem um sistema de controlos e saldos para minimizar a sua exposição ao risco de erro ou má conduta do trabalhador.

BHOLD implementa SoD permitindo-lhe definir permissões incompatíveis. Quando estas permissões são definidas, o BHOLD aplica a SoD evitando a criação de funções ligadas a permissões incompatíveis, quer estejam ligadas diretamente ou através de herança, e impedindo que os utilizadores sejam atribuídos a múltiplas funções que, quando combinadas, concederiam permissões incompatíveis, novamente por atribuição direta ou através de herança. Opcionalmente, os conflitos podem ser ultrapassados.

#### <a name="context-adaptable-permissions"></a>Permissões adaptáveis ao contexto

Ao criar permissões que podem ser modificadas automaticamente com base num atributo do objeto, pode reduzir o número total de permissões que tem de gerir. Permissões adaptáveis ao contexto (CAPs) permitem definir uma fórmula como um atributo de permissão que modifica a forma como a permissão é aplicada pela aplicação associada à permissão. Por exemplo, pode criar uma fórmula que altera a permissão de acesso a uma pasta de ficheiros (através de um grupo de segurança associado à lista de controlo de acesso da pasta) com base no facto de um utilizador pertencer a uma unidade organizacional (unidade organizacional) que contenha funcionários a tempo inteiro ou contratados. Se o utilizador for transferido de uma unidade organizacional para outra, a nova permissão é automaticamente aplicada e a antiga permissão é desativada. 

A fórmula CAP pode consultar os valores dos atributos que foram aplicados a aplicações, permissões, unidades organizacionais e utilizadores.

#### <a name="attribute-based-authorization"></a>Autorização baseada no atributo

Uma forma de controlar se uma função que está ligada a uma unidade organizacional (unidade organizacional) é ativada para um determinado utilizador na unidade organizacional é utilizar a autorização baseada em atributos (ABA). Ao utilizar a ABA, só é possível ativar automaticamente uma função quando determinadas regras baseadas nos atributos de um utilizador forem cumpridas. Por exemplo, pode ligar um papel a uma unidade organizacional que só se torna ativa para um utilizador se o título de trabalho do utilizador corresponder ao título de trabalho na regra ABA. Isto elimina a necessidade de ativar manualmente uma função proposta para um utilizador. Em vez disso, uma função pode ser ativada para todos os utilizadores de uma unidade organizacional que tenham um valor de atributo que satisfaça a regra ABA do papel. As regras só podem ser combinadas, de modo a que uma função seja ativada apenas quando os atributos de um utilizador satisfazem todas as regras da ABA especificadas para o papel.

É importante notar que os resultados dos testes de regra da ABA são limitados por configurações de cardinalidade. Por exemplo, se a definição de cardinalidade de uma regra especificar que não mais do que dois utilizadores podem ser atribuídos a uma função, e se uma regra DaBA ativar uma função para quatro utilizadores, a função será ativada apenas para os dois primeiros utilizadores que passam no teste ABA.

#### <a name="flexible-attribute-types"></a>Tipos de atributos flexíveis

O sistema de atributos no BHOLD é altamente extensível. Pode definir novos tipos de atributos para objetos como utilizadores, unidades organizacionais (unidades organizacionais) e funções. Os atributos podem ser definidos para ter valores que são inteiros, Boolean (sim/não), alfanuméricos, data, hora e endereços de e-mail. Os atributos podem ser especificados como valores únicos ou uma lista de valor.

## <a name="attestation"></a>Atestação

O BHOLD Suite fornece ferramentas que pode utilizar para verificar se os utilizadores individuais receberam permissões adequadas para realizar as suas tarefas de negócio. O administrador pode utilizar o portal fornecido pelo módulo De Attestation BHOLD para conceber uma gestão do processo de atestado.

O processo de atestado é conduzido através de campanhas em que os administradores de campanha têm a oportunidade e os meios para verificar se os utilizadores pelos quais são responsáveis têm acesso adequado a aplicações geridas pelo BHOLD e autorizações corretas dentro dessas aplicações. Um dono de campanha é designado para supervisionar a campanha e garantir que a campanha está a ser realizada corretamente. Uma campanha pode ser criada para ocorrer uma vez ou numa base recorrente.

Normalmente, o administrador de uma campanha será um gestor que irá atestar os direitos de acesso dos utilizadores pertencentes a uma ou mais unidades organizacionais pelas quais o gestor é responsável. Os administradores podem ser automaticamente selecionados para os utilizadores que estão a ser atestados numa campanha com base nos atributos dos utilizadores, ou os administradores para uma campanha podem ser definidos enumerando-os num ficheiro que mapeia todos os utilizadores que estão a ser atestados na campanha a um administrador.

Quando uma campanha começa, o BHOLD envia uma mensagem de notificação por e-mail aos administradores e proprietários da campanha e envia lembretes periódicos para ajudá-los a manter o progresso na campanha. Os administradores são direcionados para um portal de campanha onde podem ver uma lista dos utilizadores pelos quais são responsáveis e as funções que são atribuídas a esses utilizadores. Os administradores podem então confirmar se são responsáveis por cada um dos utilizadores listados e aprovar ou negar os direitos de acesso de cada um dos utilizadores listados.

Os donos da campanha também usam o portal para monitorizar o andamento da campanha, e as atividades de campanha são registadas para que os donos de campanha possam analisar as ações que foram tomadas no decorrer da campanha.

## <a name="analytics"></a>Análise

Uma das considerações importantes na implementação de um sistema abrangente de controlo de acesso baseado em direitos (RBAC) é o equilíbrio entre manter um controlo rigoroso do acesso e evitar barreiras desnecessárias (ou, pior, inesperadas) de acesso. O esforço para manter este equilíbrio resulta frequentemente numa estrutura de controlo de acesso tão complexa que as interações inesperadas entre políticas são quase inevitáveis.

Por essa razão, é importante poder analisar os efeitos das políticas de controlo de acesso antes de serem efetivamente implementadas. O módulo Analytics da BHOLD Suite dá-lhe a capacidade de realizar esta análise, permitindo-lhe desenvolver regras que representem as suas políticas e, em seguida, mostrar aos utilizadores cujas permissões estão em conformidade ou em conflito com a regra. Com base nesta análise, pode ajustar a política ou modificar papéis e permissões para eliminar quaisquer conflitos com a política.

O portal BHOLD Analytics dá-lhe a capacidade de construir conjuntos de regras que consistem numa ou mais regras que cria para testar uma determinada política ou grupo de políticas. Uma regra consiste nas seguintes partes principais:

- Um título e descrição que lhe permitem identificar e descrever a regra
- Um estado que indica se a regra está pronta para revisão, sendo revista ou aprovada
- Um conjunto de elementos (como utilizadores ou permissões) que a regra foi concebida para testar
- Filtros de subconjunto opcional que são expressões que pode utilizar para selecionar um subgrupo apropriado do elemento a examinar
- Um ou mais filtros de regras que são expressões que representam a política que está a ser testada.

Uma regra pode testar qualquer um dos seguintes conjuntos de elementos:

- Utilizadores
- Unidades Organizacionais
- Funções
- Permissões
- Aplicações
- Contas

O diagrama que se segue ilustra uma regra simples que consiste em duas regras de subconjunto e duas regras de filtro:

![](media/bhold-concepts-guide/rules.png)

Note a diferença no efeito de falha de um filtro de subconjunto e de falhar num filtro de regra: A falha de um filtro de subconjunto remove um objeto de elemento do teste por filtros subsequentes, enquanto falha um filtro de regra faz com que o objeto seja reportado como não conforme. Apenas os objetos que passam todos os filtros de subconjunto e todos os filtros de regra são reportados como conformes.

Cada filtro é constituído por um tipo, um operador (que é dependente do tipo), uma chave (um dos elementos) e um valor contra o qual a chave é testada pelo operador. Por exemplo, o filtro seguinte testaria se o número de utilizadores num subconjunto de elementos ultrapassa 10:


|   |   |   |   |   |
|---|---|---|---|---|
|**Tipo:**   | Número de   |
| **Chave:**  | Utilizadores  |
| **Operador**  | >  |
| **Value:** | 10 |

Os filtros de regras podem ser de três tipos e utilizar operadores específicos do seu tipo, conforme indicado:

- Atributo
  - < e >
  - = e !=
  - **Contém**
  - **Não contém**
- Número de
  - < e >
  - = e !=
- Restrição
  - **Deve ter qualquer e deve ter todos**
  - **Não pode ter nenhum e não pode ter todos**
  - **Só pode ter qualquer e só pode ter todos**
  - **Exclusivamente ter qualquer e exclusivamente ter todos e exclusivamente todos**

> [!Note]
> Os filtros restritivos podem utilizar os operadores indicados para testar uma chave contra um conjunto de valores múltiplos.

Por exemplo, se quiser testar a implementação de uma política de segregação de deveres (SoD) que indique que nenhum utilizador que tenha autorização de pagamento de pedido também deve ter autorização de pagamento aprovado, pode construir uma regra como a seguinte:

|   |  |
|---|--|
|Nome:| Teste de SoD de Pagamento|
|Elemento:| Utilizadores|
|Filtro de subset:| Ter qualquer permissão Solicitação Pagamento|
|Filtro de regras: | Não pode ter qualquer permissão Aprovar Pagamento|

Ao executar esta regra, o módulo BHOLD Analytics apresenta o número de utilizadores no subconjunto selecionado (o número de utilizadores com a permissão de pagamento de pedido), o número de utilizadores que cumprem a regra e o número de utilizadores que não cumprem a regra. Em seguida, pode apresentar os utilizadores não conformes para que possa corrigir a inconsistência.

Além de apresentar os resultados, também pode guardar o relatório como um ficheiro de valor separado de vírgula (CSV) ou XML para permitir analisar os resultados mais tarde. Também pode personalizar o relatório resultante para mostrar informações adicionais que podem ajudá-lo a entender melhor o impacto. Por exemplo, se estiver a testar os utilizadores, pode apresentar (ou reportar) as contas dos utilizadores conformes ou não conformes para que possa ver quais as aplicações envolvidas.

Como uma regra pode conter vários filtros, pode ligar filtros para testar se existe uma combinação específica de condições. Por predefinição, o resultado é o produto de um teste e boolean de todos os filtros, mas pode especificar que será realizado um teste DE OR da combinação do filtro.

Por exemplo, se a sua política comercial exigir que os gestores tenham a permissão de pagamento modificado ou a autorização de pagamento de aprovação, então poderá testar o cumprimento da política construindo uma regra como a seguinte:


|  |  |
|--|--|
|Nome: | Modificar teste de sod de pagamento|
|Elemento: | Utilizadores |
|Filtro de subset: | Ter qualquer gestor de funções|
| Filtros de regras: |Deve ter qualquer permissão Modificar pagamento </br> Deve ter qualquer permissão Aprovar Pagamento|

Por predefinição, qualquer utilizador que seja um gestor que tenha o Pagamento Modificado e a permissão de Pagamento de Pedido será reportado como conforme. No entanto, a política exige que um gestor tenha qualquer permissão, não necessariamente ambos. Para testar o cumprimento real da apólice, deve utilizar o operador OR Boolean com a regra para determinar se existem gestores que não tenham a permissão de pagamento modificado ou a autorização de pagamento.

Ao contrário de outros operadores, o **Exclusivamente tem todos** **os** operadores que indicam a conformidade com objetos que de outra forma seriam excluídos por um filtro de subconjunto. Por exemplo, para testar uma política que todos os gestores (e apenas gestores) têm a permissão De aprovação de revisões, você poderia construir uma regra da seguinte forma:

|  |  |
|--|--|
|Nome: | Teste de Aprovação de Revisão|
|Elemento: | Utilizadores|
| Filtro de subset: | Ter qualquer gestor de funções
|Filtro de regras: | Exclusivamente ter qualquer permissão Aprovar Comentários|

Esta regra irá reportar como utilizadores conformes que são gestores e têm a permissão De aprovação de revisões e utilizadores que não são gestores e que não têm a permissão de Aprovação de Revisões. Inversamente, os gestores que não têm essa permissão e utilizadores que não são gestores, mas que têm essa permissão, são reportados como não conformes.

Como notado anteriormente, pode combinar regras num conjunto de regras, facilitando-lhe a categorização e gestão de regras para satisfazer os seus requisitos de negócio.

Também pode definir um conjunto de filtros globais que, quando ativados, se aplicam a qualquer regra que seja testada. Se precisar frequentemente de excluir um determinado subconjunto de registos ao testar regras em diferentes regras, pode especificar filtros globais que pode ativar ou desativar conforme necessário.

## <a name="reporting"></a>Relatórios

O módulo BHOLD Reporting dá-lhe a capacidade de visualizar informação no modelo através de uma variedade de relatórios. O módulo BHOLD Reporting fornece um vasto conjunto de relatórios incorporados, além de que inclui um assistente que pode usar para criar relatórios personalizados básicos e avançados. Ao executar um relatório, pode apresentar imediatamente os resultados ou guardar os resultados num ficheiro Microsoft Excel (.xlsx). Para visualizar este ficheiro utilizando o Microsoft Excel 2000, o Microsoft Excel 2002 ou o Microsoft Excel 2003, pode descarregar e instalar o Microsoft Office Compatibilidade Pack para formatos word, Excel e PowerPoint File.


Utiliza o módulo BHOLD Reporting principalmente para produzir relatórios baseados em informações de papel atuais. Para gerar relatórios para auditoria a alterações à informação de identidade, o Gestor de Identidade de Vanguarda 2010 R2 tem uma capacidade de reporte para o Serviço FIM que é implementado no Armazém de Dados do Gestor de Serviços do System Center. Para mais informações sobre relatórios FIM, consulte a documentação do Gestor de Identidade de Vanguarda 2010 e do Gestor de Identidade de Vanguarda 2010 R2 na Biblioteca Técnica de Gestão de Identidade da Vanguarda.

As categorias abrangidas pelos relatórios incorporados incluem:

- Administração
- Atestação
- Controlos
- Controlo de Acesso Interno
- Registo
- Modelo
- Estatísticas
- Fluxo de trabalho

Pode criar relatórios e adicioná-los a estas categorias, ou pode definir as suas próprias categorias nas quais pode colocar relatórios personalizados e incorporados.

À medida que constrói um relatório, o assistente intervém-o através do fornecimento dos seguintes parâmetros:

- Identificação de informações, incluindo nome, descrição, categoria, utilização e público
- Campos a serem exibidos no relatório
- Consultas que especificam quais itens devem ser analisados
- Ordem em que as linhas devem ser ordenadas
- Campos usados para quebrar o relatório em secções
- Filtros para refinar os elementos que são devolvidos no relatório

Em cada fase do assistente, pode pré-visualizar o relatório tal como o definiu até agora e guardá-lo se não precisar de especificar parâmetros adicionais. Também pode voltar aos passos anteriores para alterar os parâmetros especificados anteriormente no assistente.

## <a name="access-management-connector"></a>Conector de Gestão de Acesso

O módulo BHOLD Suite Access Management Connector suporta sincronização inicial e contínua de dados em BHOLD. O Conector de Gestão de Acesso trabalha com o Serviço de Sincronização FIM para mover dados entre a base de dados BHOLD Core, o metaverso MIM e aplicações-alvo e lojas de identidade.

Versões anteriores do BHOLD exigiam vários MAs para controlar o fluxo de dados entre mim e tabelas de bases de dados bhold intermédias. Na BHOLD Suite SP1, o Conector de Gestão de Acesso permite definir agentes de gestão (MAs) em MIM que fornecem transferência de dados diretamente entre BHOLD e MIM.

## <a name="mim-integration"></a>Integração MIM

Uma característica importante e poderosa do Forefront Identity Manager 2010 e do Forefront Identity Manager 2010 R2 é o portal de self-service que permite aos utilizadores finais visualizar e gerir a sua identidade e informação de adesão. A MIM Integration alarga o Portal MIM com a gestão de funções de self-service. Por exemplo, utilizando as funcionalidades BHOLD no Portal MIM, um utilizador pode solicitar a atribuição de funções e pode ver funções ativas e pedidos pendentes. Capacidades adicionais podem ser concedidas a administradores delegados, tais como a capacidade de solicitar atribuições de papéis para outros utilizadores.

É importante notar que as extensões bHOLD ao Portal MIM apoiam a função de autosserviço e a gestão do fluxo de trabalho, e reporte. Outras funções de administração BHOLD, bem como atestado, são fornecidas pelos portais web dos módulos BHOLD, que são hospedados separadamente do Portal MIM.

## <a name="next-steps"></a>Passos seguintes

- [Guia de instalação BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
