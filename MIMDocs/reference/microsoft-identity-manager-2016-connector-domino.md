---
title: Conector Lotus Domino Microsoft Docs
description: Este artigo descreve como configurar o Conector Lotus Domino da Microsoft.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: e07fd469-d862-470f-a3c6-3ed2a8d745bf
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 04/05/2018
ms.author: billmath
ms.openlocfilehash: 46e80bce942b32bce7b7c3fb148973c249136b3f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762187"
---
# <a name="lotus-domino-connector-technical-reference"></a>Referência técnica do Conector do Lotus Domino
Este artigo descreve o Conector Lotus Domino. O artigo aplica-se aos seguintes produtos:

* Microsoft Identity Manager 2016 (MIM2016)
* Gestor de Identidades Da Vanguarda 2010 R2 (FIM2010R2)
  * Deve utilizar o hotfix 4.1.3671.0 ou mais tarde [KB3092178](https://support.microsoft.com/kb/3092178).

Para MIM2016 e FIM2010R2, o Conector está disponível como download do [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-lotus-domino-connector"></a>Visão geral do Conector Lotus Domino
O Conector Lotus Domino permite-lhe integrar o serviço de sincronização com o servidor Lotus Domino da IBM.

De uma perspetiva de alto nível, as seguintes funcionalidades são suportadas pela atual libertação do conector:

| Funcionalidade | Suporte |
| --- | --- |
| Fonte de dados conectada |Servidor: <li>Lotus Domino 8.5.x</li><li>Lotus Domino 9.x</li>Cliente:<li>Lotus Domino 8.5.x</li><li>Notas de Lótus 9.x</li> |
| Cenários |<li>Gestão do ciclo de vida de objetos</li><li>Gestão de Grupos</li><li>Gestão de palavra-passe</li> |
| Operações |<li>Importação Completa e Delta</li><li>Exportar</li><li>Desafine e altere a palavra-passe na palavra-passe HTTP</li> |
| Esquema |<li>Pessoa (Utilizador roaming, Contacto (pessoas sem certificado))</li><li>Group</li><li>Recurso (Recurso, Sala, Reunião Online)</li><li>Base de dados de correio eletrónico</li><li>Descoberta dinâmica de atributos para objetos suportados</li><li>Suporte até 250 certificadores personalizados com uma organização & unidades de organização (OU)</li> |

O conector Lotus Domino utiliza o cliente Lotus Notes para comunicar com o Lotus Domino Server. Como consequência desta dependência, um Cliente de Notas lotus suportado deve ser instalado no servidor de sincronização. A comunicação entre o cliente e o servidor é implementada através da interface Lotus Notes .NET Interop (Interop.domino.dll). Esta interface facilita a comunicação entre a plataforma Microsoft.NET e o cliente lotus Notes e suporta o acesso a documentos e vistas da Lotus Domino. Para a importação delta, também é possível que a interface nativa C++ seja utilizada (dependendo do método de importação delta selecionado).

### <a name="prerequisites"></a>Pré-requisitos
Antes de utilizar o Conector, certifique-se de que tem os seguintes pré-requisitos no servidor de sincronização:

* Microsoft .NET 4.5.2 Framework ou mais tarde
* O cliente Lotus Notes deve ser instalado no seu servidor de sincronização
* O Conector Lotus Domino requer que a base de dados de esquemas padrão da Lotus Domino LDAP (schema.nsf) esteja presente no servidor Dominário. Se não estiver presente, pode instalá-lo executando ou reiniciando o serviço LDAP no servidor Domino.

### <a name="connected-data-source-permissions"></a>Permissões de Fonte de Dados Conectadas
Para executar qualquer uma das tarefas suportadas no conector Lotus Domino, deve ser membro dos seguintes grupos:

* Administradores de acesso completo
* Administradores
* Administradores de bases de dados

A tabela que se segue enumera as permissões necessárias para cada operação:

| Operação | Direitos de Acesso |
| --- | --- |
| Importar |<li>Ler documentos públicos</li><li> Administrador de acesso completo (Quando é membro do grupo de administradores de acesso completo, tem automaticamente acesso efetivo em ACL.)</li> |
| Exportação e Definir Senha |Acesso efetivo: <li>Criar documentos</li><li>Eliminar documentos</li><li>Ler documentos públicos</li><li>Escrever documentos públicos</li><li>Replicar ou copiar documentos</li>Para as operações de exportação, também precisa das seguintes funções: <li>CriarResource</li><li>Agrupamento de Grupos</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li> |

### <a name="direct-operations-and-adminp"></a>Operações diretas e Administração
As operações vão diretamente para o diretório do Domino ou através do processo AdminP. As tabelas que se seguem listam todos os objetos, operações suportados e, se aplicável, o método de implementação relacionado:

**Livro de endereços primários**

| Objeto | Criar | Atualizar | Eliminar |
| --- | --- | --- | --- |
| Pessoa |Administrador |Direct |Administrador |
| Group |Administrador |Direct |Administrador |
| MailInDB |Direct |Direct |Direct |
| Recurso |Administrador |Direct |Administrador |

**Livro de endereços secundário**

| Objeto | Criar | Atualizar | Eliminar |
| --- | --- | --- | --- |
| Pessoa |N/D |Direct |Direct |
| Group |Direct |Direct |Direct |
| MailInDB |Direct |Direct |Direct |
| Recurso |N/D |N/D |N/D |

Quando um recurso é criado, um documento de Notas é criado. Da mesma forma, quando um recurso é eliminado, o documento notas é eliminado.

### <a name="ports-and-protocols"></a>Portos e protocolos
Os servidores IBM Lotus Notes e Domino comunicam através da Chamada de Procedimento Remoto de Notas (NRPC) onde o NRPC deve utilizar O TCP/IP. O número de porta padrão é 1352, mas pode ser alterado pelo administrador da Domino.

### <a name="not-supported"></a>Não suportado
As seguintes operações não são suportadas pela atual libertação do conector Lotus Domino:

* Mover caixa de correio entre servidores.

## <a name="create-a-new-connector"></a>Criar um novo Conector
### <a name="client-software-installation-and-configuration"></a>Instalação e Configuração de Software do Cliente
As notas de lótus devem ser instaladas no servidor **antes** da instalação do Conector.

Quando instalar, certifique-se de que faz uma **instalação de** um único utilizador . A **instalação multiutilizador** predefinido não funciona.  
![Notas1](./media/microsoft-identity-manager-2016-connector-domino/notes1.png)

Na página de funcionalidades, instale apenas as funcionalidades de Notas lotus necessárias e **o Logotipo Único do Cliente.** É necessário um único início de sessão para que o conector possa iniciar sessão no servidor Domino.  
![Notas2](./media/microsoft-identity-manager-2016-connector-domino/notes2.png)

**Nota:** Inicie notas de Lotus uma vez com um utilizador que está localizado no mesmo servidor que a conta que utiliza como conta de serviço do conector. Certifique-se também de fechar o cliente Lotus Notes no servidor. Não pode funcionar ao mesmo tempo que o Conector tenta ligar-se ao servidor Domino.

### <a name="create-connector"></a>Criar Conector
Para criar um conector Lotus Domino, no **Serviço de Sincronização** selecione **o Agente de Gestão** e **crie** . Selecione o **Conector Lotus Domino (Microsoft).**  
![CriarConnector](./media/microsoft-identity-manager-2016-connector-domino/createconnector.png)

Se a sua versão do serviço de sincronização oferecer a capacidade de configurar **arquitetura,** certifique-se de que o conector está definido para o seu valor padrão a ser executado no **Processo** .

### <a name="connectivity"></a>Conectividade
Na página conectividade, deve especificar o nome do servidor Lotus Domino e introduzir as credenciais de início de súm.  
![Conectividade](./media/microsoft-identity-manager-2016-connector-domino/connectivity.png)

A propriedade Domino Server suporta dois formatos para o nome do servidor:

* ServerName
* Nome de servidor/nome de diretório

O formato **ServerName/DirectoryName** é o formato preferido deste atributo porque fornece uma resposta mais rápida quando o conector contacta o Servidor Dominó.

O ficheiro UserID fornecido é armazenado na base de dados de configuração do serviço de sincronização.

Para **a Delta Import** tem estas opções:

* **Nenhuma.** O Conector não faz importações delta.
* **Adicionar/atualizar** . O Conector faz operações de adição e atualização de importações delta. Para eliminar, é necessária uma operação **de importação completa.** Esta operação está a utilizar o interacte .Net.
* **Adicionar/atualizar/eliminar** . O Conector faz a delta import add, update e delete operations. Esta operação está a utilizar as interfaces C++ nativas.

Nas **Opções Schema** tem as seguintes opções:

* **Schema padrão** . O Conector deteta o esquema do servidor Domino. Esta seleção é a opção padrão.
* **DSML-Schema** . Só é utilizado se o servidor Domino não expor o esquema. Em seguida, pode criar um ficheiro DSML com o esquema e importá-lo em vez disso. Para obter mais informações sobre a DSML, consulte [o OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml).

Quando clica em Seguinte, verifica-se os parâmetros de configuração do UserID e da palavra-passe.

### <a name="global-parameters"></a>Parâmetros de Globais
Na página De Parâmetros Globais, configura o fuso horário e a opção de importação e operação de exportação.  
![Parâmetros de Globais](./media/microsoft-identity-manager-2016-connector-domino/globalparameters.png)

O parâmetro **do Serviço De Horas do Domino** define a localização do seu Servidor Dominó.

Esta opção de configuração é necessária para apoiar as operações **de importação delta,** uma vez que permite que o serviço de sincronização determine alterações entre as duas últimas importações.

> [!Note]
> A partir da atualização de março de 2017, o ecrã de parâmetros Global inclui a opção de eliminar a base de dados de correio do utilizador durante a eliminação do utilizador.

![Apagar a caixa de correio do utilizador](./media/microsoft-identity-manager-2016-connector-domino/AdminP.png)

#### <a name="import-settings-method"></a>Definições de importação, método
O **Perform Full Import By** tem estas opções:

* Pesquisa
* Vista (Recomendado)

**A pesquisa** está a usar a indexação em Domino, mas é comum que os índices não sejam atualizados em tempo real e os dados devolvidos do servidor nem sempre estejam corretos. Para um sistema com muitas alterações, esta opção geralmente não funciona bem e fornece apagamentos falsos em algumas situações. No entanto, **a procura** é mais rápida do que **a visualização.**

**A vista** é a opção recomendada, uma vez que fornece o estado correto dos dados. É um pouco mais lento que a procura.

#### <a name="creation-of-virtual-contact-objects"></a>Criação de Objetos de Contacto Virtual
A **criação ativa do \_ objeto de contacto** tem estas opções:

* Nenhum
* Valores não-referenciais
* Valores de referência e não-referência

Em Domino, os atributos de referência podem conter muitos formatos diferentes para referenciar outros objetos. Para poder representar diferentes variações, o Conector implementa \_ objetos de contacto, também **conhecidos** como Contactos Virtuais (VC). Estes objetos são criados para que possam se juntar a objetos MV existentes ou projetados como novos objetos. Desta forma, as referências de atributos podem ser preservadas.

Ao ativar esta definição e se o conteúdo de um atributo de referência não for um formato DN, é criado um \_ objeto contacto. Por exemplo, um atributo de membro de um grupo pode conter endereços SMTP. Também é possível ter nome curto e outros atributos presentes em atributos de referência. Para este cenário, selecione **Valores de Não Referência** . Esta configuração é a configuração mais comum para implementações do Domino.

Quando a Lotus Domino está configurada para ter livros de endereços separados com diferentes nomes distintos que representam o mesmo objeto, é possível também criar \_ objetos de contacto para todos os valores de referência que são encontrados num livro de endereços. Para este cenário, selecione a opção **Valores de Referência e Valores não-Referenciais.**

Se tem vários valores no atributo **FullName** em Domino, então também pretende ativar a criação de Contactos Virtuais para que as referências possam ser resolvidas. Por exemplo, este atributo pode ter múltiplos valores após um casamento ou divórcio. Selecione a caixa de verificação **Ativar ... O FullName tem vários valores** para este cenário.

Ao juntar-se aos atributos corretos, os \_ objetos de contacto seriam unidos ao objeto MV.

Estes objetos têm VC= \_ Contacto adicionado ao seu DN.

#### <a name="import-settings-conflict-object"></a>Definições de importação, objeto de conflito
**Excluir objeto de conflito**

Numa grande implementação do Domino, é possível que vários objetos tenham o mesmo DN devido a problemas de replicação. Nestes casos, o conector veria dois objetos com diferentes UniversalIDs, mas o mesmo DN. Este conflito provocaria a criação de um objeto transitório no espaço do conector. O Conector pode ignorar os objetos que foram selecionados em Domino como vítimas de replicação. A recomendação é manter esta caixa de verificação selecionada.

#### <a name="export-settings"></a>Exportar definições
Se a opção **Utilizar o AdminP para atualizar referências** não for selecionado, então a exportação de atributos de referência, como membro, é uma chamada direta e não utiliza o processo AdminP. Utilize esta opção apenas quando a AdminP não tiver sido configurada para manter a integridade referencial.

#### <a name="routing-information"></a>Informação de encaminhamento
Em Domino, é possível que um atributo de referência tenha informações de encaminhamento incorporadas como um sufixo ao DN. Por exemplo, o atributo de membro num grupo pode conter <strong>CN= example/organization@ABC </strong>. O sufixo @ABC é a informação de encaminhamento. A informação de encaminhamento é usada pela Domino para enviar e-mails para o sistema Domino correto, que poderia ser um sistema numa organização diferente. No campo 'Informações de encaminhamento', pode especificar os sufixos de encaminhamento utilizados no âmbito da organização no âmbito do Conector. Se um destes valores for encontrado como um sufixo num atributo de referência, a informação de encaminhamento é removida da referência. Se o sufixo de encaminhamento de um valor de referência não puder ser igualado a um desses valores especificados, é criado um \_ objeto de contacto. Estes \_ objetos de contacto são criados com **RO=@ <RoutingSuffix>** inserido no DN. Para estes \_ objetos de contacto são também adicionados os seguintes atributos para permitir a junção a um objeto real, se necessário: \_ nome de encaminhamento, \_ nome de contacto, nome de \_ exibição e UniversalID.

#### <a name="additional-address-books"></a>Livros de endereços adicionais
Se não tiver assistência de **diretório** instalada, que fornece o nome de livros de endereços secundários, então pode introduzir manualmente estes livros de endereços.

#### <a name="multivalued-transformation"></a>Transformação Multivalorizada
Muitos atributos na Lotus Domino são multi-valorizados. Os atributos metaversos correspondentes são tipicamente únicos. Ao configurar a opção de importação e de operação exportação, permite que o conector ajude na tradução necessária dos atributos afetados.

**Exportar**  
A opção de operação exportação suporta dois modos:

* Item do apêndice
* Substituir artigo

**Substituir Item** – Quando selecionar esta opção, o conector remove sempre os valores atuais do atributo em Domino e substitui-os pelos valores fornecidos. O valor fornecido pode ser de valor único ou multi-valor.

Exemplo: O atributo Assistente de um objeto de pessoa tem os seguintes valores:

* CN=Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN=John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Se um novo assistente chamado **David Alexander** for designado para este objeto de pessoa, o resultado é:

* CN=David Alexander/OU=Contoso/O=Americas,NAB=names.nsf

**Item do apêndice** – Quando seleciona esta opção, o conector mantém os valores existentes no atributo em Domino e insere novos valores no topo da lista de dados.

Exemplo: O atributo Assistente de um objeto de pessoa tem os seguintes valores:

* CN=Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN=John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Se um novo assistente chamado **David Alexander** for designado para este objeto de pessoa, o resultado é:

* CN=David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
* CN=Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN=John Smith/OU=Contoso/O=Americas,NAB=names.nsf

**Importar**  
A opção de operação Import suporta dois modos:

* Predefinição
* Multivalorizado para Valor Único

**Predefinição** – Quando seleciona a opção Predefinição, todos os valores de todos os atributos são importados.

**Multivalorizado para Valor Único** – Quando seleciona esta opção, um atributo multi-valor é convertido num atributo de valor único. Se existir mais de um valor, o valor na parte superior (este valor é tipicamente também o mais recente) é usado.

Exemplo: O atributo Assistente de um objeto de pessoa tem os seguintes valores:

* CN=David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
* CN=Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
* CN=John Smith/OU=Contoso/O=Americas,NAB=names.nsf

A mais recente atualização a este atributo é **David Alexander.** Como a opção de operação Import é definida como Multivalued para Single Value, o conector apenas importa **David Alexander** para o espaço do conector.

A lógica de converter atributos multi-valorizados em atributos de valor único não se aplica ao atributo do membro do grupo e ao atributo fullname da pessoa.

É igualmente possível configurar regras de importação e transformação de exportação para atributos multivalorizados por atributo, como exceção à regra global. Para configurar esta opção, introduza [objectótipo]. [atribuiame] na lista de **atributos de exclusão de importação** e na **lista de exclusão de exportação** das listas de texto. Por exemplo, se introduzir Person.Assistant e a bandeira global for definida para importar todos os valores, apenas o primeiro valor é importado para o assistente.

#### <a name="certifiers"></a>Certificadores
Todas as Unidades Organizacionais/Organizacionais estão listadas pelo conector. Para poder exportar objetos pessoais para o livro de endereços primário, é necessário um certificador com a sua senha.

Se todos os certificadores tiverem a mesma senha, a **Palavra-passe para todos os Certificados** pode ser utilizada. Em seguida, pode introduzir a palavra-passe aqui e apenas especificar o ficheiro certificador.

Se você apenas importar, então você não tem que especificar quaisquer certificadores.

### <a name="configure-provisioning-hierarchy"></a>Configure Hierarquia de Provisionamento
Quando configurar o conector Lotus Domino, pule nesta página de diálogo. O conector Lotus Domino não suporta o provisionamento da hierarquia.  
![Hierarquia de provisionamento](./media/microsoft-identity-manager-2016-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Configurar Partições e Hierarquias
Ao configurar divisões e hierarquias, deve selecionar o livro de endereços primário chamado NAB=names.nsf. Além do livro de endereços primário, pode selecionar livros de endereços secundários se existirem.  
![Partições](./media/microsoft-identity-manager-2016-connector-domino/partitions.png)

### <a name="select-attributes"></a>Selecionar Atributos
Ao configurar os seus atributos, deve selecionar todos os atributos que estejam pré-fixados com **\_ MMS \_** . Estes atributos são necessários quando fornece novos objetos à Lotus Domino

![Atributos](./media/microsoft-identity-manager-2016-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Gestão do ciclo de vida de objetos
Esta secção fornece uma visão geral dos diferentes objetos em Domino.

### <a name="person-objects"></a>Objetos de pessoa
O objeto da pessoa representa os utilizadores em Unidades de Organização e Organização. Além dos atributos predefinidos, o administrador Domino pode adicionar atributos personalizados a um objeto Pessoa. No mínimo, um objeto de Pessoa deve incluir todos os atributos obrigatórios. Para obter uma lista completa de atributos obrigatórios, consulte [Lotus Notes Properties](#lotus-notes-properties). Para registar um objeto de pessoa, devem ser cumpridos os seguintes pré-requisitos:

* O livro de endereços (names.nsf) deve ter sido definido e deve ser o livro de endereços primário.
* Deve ter o Id certificador O/OU e a palavra-passe para registar um determinado utilizador na Unidade de Organização/Organização.
* Você deve definir um conjunto específico de propriedades de Notas de Lotus para um objeto de pessoa. Estas propriedades são utilizadas para o fornecimento do objeto da pessoa. Para mais informações, consulte a secção denominada [Lotus Notes Properties](#lotus-notes-properties) mais tarde neste documento.
* A palavra-passe HTTP inicial para uma pessoa é um atributo e definido durante o provisionamento.
* O objeto da pessoa deve ser um dos três tipos suportados seguintes:
  1. Utilizador normal que tem um ficheiro de correio e um ficheiro de id do utilizador
  2. Utilizador de Roaming (um Utilizador Normal que inclui todos os ficheiros de bases de dados de roaming)
  3. Contactos (utilizador sem ficheiro de id)

As pessoas (exceto contactos) podem ser agrupadas em Utilizadores americanos e utilizadores internacionais, conforme definido pelo valor da \_ propriedade MMS \_ IDRegType. Estas pessoas usam o Cliente notas para aceder aos servidores da Lotus Domino, têm um ID de Notas e um documento pessoa. Se estiverem a usar o correio de Notas, também têm um ficheiro de correio. O utilizador deve estar registado para se tornar ativo. Para obter mais informações, consulte:

* [Configuração de utilizadores de notas](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
* [Registo do Utilizador](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
* [Gerir utilizadores](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
* [Renomear utilizadores](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_RENAMING_A_LOTUS_INOTES_USER_STEPS.html)

Todas estas operações são realizadas na Lotus Domino e depois importadas para o serviço de sincronização.

### <a name="resources-and-rooms"></a>Recursos e Quartos
Um recurso é outro tipo de base de dados em Lotus Domino. Os recursos podem ser salas de conferências com vários tipos de equipamentos, como projetores. Existem subtipos de recursos suportados pelo conector Lotus Domino que são definidos pelo atributo Tipo de Recurso:

| Tipo de Recurso | Atributo tipo de recurso |
| --- | --- |
| Quarto |1 |
| Recurso (Outros) |2 |
| Encontro Online |3 |

Para que o tipo de objeto de recurso funcione, é necessário o seguinte:

* A base de dados de Reserva de Recursos já deve existir no servidor Domino conectado
* O site já está definido para o Recurso

A base de dados de Reserva de Recursos contém três tipos de documentos:

* Perfil do site
* Recurso
* Reserva

Para obter mais informações sobre a configuração da base de dados de Reservas de Recursos, consulte [configurar a base de dados de Reservas de Recursos.](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html)

**Criar, atualizar e eliminar recursos**  
As operações Criar, Atualizar e Eliminar são realizadas pelo conector Lotus Domino na base de dados de Reserva de Recursos. Os recursos são criados como documentos em Names.nsf (isto é, o livro de endereços primário). Para obter mais informações sobre a edição e eliminação de Recursos, consulte [editar e eliminar documentos de Recursos.](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html)

**Operação de importação e exportação de recursos**  
Os Recursos podem ser importados e exportados do serviço de sincronização, como qualquer outro tipo de objeto. Selecione o tipo de objeto como Recurso durante a configuração. Para uma operação de exportação bem sucedida, deve ter detalhes para o tipo de recurso, base de dados de conferências e nome do site.

### <a name="mail-in-databases"></a>Bases de Dados Mail-In
Uma base de dados Mail-In é uma base de dados que foi concebida para receber e-mails. É uma caixa de correio Lotus Domino que não está associada a nenhuma conta específica do utilizador da Lotus Domino (isto é, não tem o seu próprio ficheiro de ID e senha). Uma base de dados por correio eletrónico tem um UserID único ("nome curto") associado a ele e o seu próprio endereço de e-mail.

Se houver necessidade de uma caixa de correio separada com o seu próprio endereço de e-mail que possa ser partilhado entre diferentes utilizadores (por exemplo, group@contoso.com ), é criada uma base de dados por correio. O acesso a esta caixa de correio é controlado através da sua Lista de Controlo de Acesso (ACL), que contém os nomes dos utilizadores de Notas que podem abrir a caixa de correio.

Para obter uma lista dos atributos necessários, consulte a secção chamada [Atributos Obrigatórios](#mandatory-attributes) mais tarde neste artigo.

Quando uma base de dados é projetada para receber um e-mail, um documento de base de dados Mail-In é criado na Lotus Domino. Este documento deve existir no Diretório Dominó de todos os servidores que armazenam uma cópia da base de dados. Para obter uma descrição detalhada sobre a criação de um documento de base de dados por correio eletrónico, consulte [criar um documento de base de dados Mail-In](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html).

Antes de criar uma Base de Dados Mail-In, a base de dados já deveria existir (deveria ter sido criada pela Lotus Admin) no servidor Domino.

### <a name="group-management"></a>Gestão de Grupos
Você pode obter uma visão detalhada da gestão do grupo Lotus Domino a partir dos seguintes recursos:

* [Usando grupos](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
* [Criar um grupo](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
* [Criar e modificar grupos](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
* [Gerir grupos](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
* [Renomear um grupo](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Gestão de palavra-passe
Para um utilizador registado da Lotus Domino, existem dois tipos de senhas:

1. Palavra-passe do utilizador (armazenada em ficheiro User.id)
2. Senha de Internet / HTTP

O conector Lotus Domino suporta apenas operações com senha HTTP.

Para realizar a gestão de passwords, deve ativar a gestão de passwords para o conector no Designer de Agente de Gestão. Para ativar a gestão da palavra-passe, selecione Ative a gestão da **palavra-passe** na página de diálogo **de configuração de extensões.**  
![Configurar extensões](./media/microsoft-identity-manager-2016-connector-domino/configureextensions.png)

O suporte do conector Lotus Domino após operações na senha de Internet:

* Definir palavra-passe: Definir a palavra-passe define uma nova palavra-passe HTTP/Internet no utilizador em Domino. Por predefinição, a conta também é desbloqueada. A bandeira de desbloqueio está exposta na interface WMI do Motor De Sincronização.
* Alterar palavra-passe: Neste cenário, um utilizador pode querer alterar a palavra-passe ou é solicitado a alterar a palavra-passe após um determinado tempo. Para que esta operação se realize, tanto (a antiga como a nova senha) são obrigatórias. Uma vez alterada, a nova senha é atualizada na Lotus Domino.

Para obter mais informações, consulte:

* [Utilizando a funcionalidade de bloqueio da Internet](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
* [Gestão de senhas de Internet](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Informação de Referência
Esta secção lista tais como descrições de atributos e requisitos de atributos para o conector Lotus Domino.

### <a name="lotus-notes-properties"></a>Propriedades de Notas de Lótus
Ao fornecer pessoas objetos ao seu diretório Lotus Domino, os seus objetos devem ter um conjunto específico de propriedades com valores específicos povoados. Estes valores são apenas necessários para criar operações.

A tabela que se segue lista estas propriedades e fornece uma descrição das suas propriedades.

| Propriedade | Descrição |
| --- | --- |
| \_MMS_AltFullName |O nome completo alternativo do utilizador. |
| \_MMS_AltFullNameLanguage |O idioma a utilizar para especificar o nome completo alternativo do utilizador. |
| \_MMS_CertDaysToExpire |O número de dias a contar da data atual antes do termo do certificado. Se não for especificado, a data de predefinição é de dois anos a contar da data atual. |
| \_MMS_Certifier |Propriedade que contém o nome da hierarquia organizacional do certificador. Por exemplo: OU=Organizationunit,O=org,C=Country. |
| \_MMS_IDPath |Se a propriedade estiver vazia, nenhum ficheiro de identificação do utilizador é criado localmente no Sync Server. Se a propriedade contiver um nome de ficheiro, um ficheiro de identificação do utilizador é criado na pasta madata. A propriedade também pode conter um caminho completo. |
| \_MMS_IDRegType |As pessoas podem ser classificadas como contactos, utilizadores americanos e utilizadores internacionais. A tabela a seguir enumera os valores possíveis: <li>0 - Contacto</li><li>1 - Utilizador americano</li><li>2 - Utilizador internacional</li> |
| \_MMS_IDStoreType |Propriedade necessária para utilizadores americanos e internacionais. A propriedade contém um valor inteiro que especifica se a identificação do utilizador é armazenada como anexo no livro de endereços notas ou no ficheiro de correio da pessoa. Se o ficheiro de ID do Utilizador for um anexo na lista de endereços, pode ser criado opcionalmente como um ficheiro com \_ MMS_IDPath. <li>Vazio - Guarde o ficheiro de identificação no Cofre de Identificação, sem ficheiro de identificação (utilizado para contactos).</li><li> 1 - Anexo na agenda de notas. A \_ propriedade MMS_Password deve ser definida para ficheiros de identificação do utilizador que sejam anexos</li><li>2 - Armazenar iD no ficheiro de correio presencial. O \_ MMS_UseAdminP deve ser definido como falso para permitir que o ficheiro de correio seja criado durante o registo da Pessoa. A \_ propriedade MMS_Password deve ser definida para ficheiros de identificação do utilizador.</li> |
| \_MMS_MailQuotaSizeLimit |O número de megabytes que são permitidos para a base de dados de ficheiros de e-mail. |
| \_MMS_MailQuotaWarningThreshold |O número de megabytes que são permitidos para a base de dados de ficheiros de e-mail antes de um aviso é emitido. |
| \_MMS_MailTemplateName |O ficheiro de modelo de e-mail que é usado para criar o ficheiro de e-mail do utilizador. Se um modelo for especificado, o ficheiro de correio é criado usando o modelo especificado. Se nenhum modelo for especificado, o ficheiro de modelo predefinido é utilizado para criar o ficheiro. |
| \_MMS_OU |Propriedade opcional que é o nome DA sob o certificador. Esta propriedade deve estar vazia para contactos. |
| \_MMS_Password |Propriedade necessária para utilizadores. A propriedade contém a senha para o ficheiro de identificação do objeto. |
| \_MMS_UseAdminP |A propriedade deve ser definida como verdadeira se o ficheiro de correio deve ser criado pelo processo AdminP no servidor Domino (assíncrona para o processo de exportação). Se a propriedade for definida como falsa, o ficheiro de correio é criado com o utilizador Domino (sincronizado no processo de exportação). |

Para um utilizador com um ficheiro de identificação associado, a \_ propriedade MMS_Password deve conter um valor. Para acesso por e-mail através do cliente Lotus Notes, as propriedades MailServer e MailFile de um utilizador devem conter um valor.

Para aceder a e-mails através de um navegador Web, as seguintes propriedades devem conter valores:

* MailFile - Propriedade obrigatória que contém o caminho no servidor Lotus Domino onde o ficheiro de correio está armazenado.
* MailServer - Propriedade obrigatória que contém o nome do servidor Lotus Domino. Este valor é o nome a utilizar quando criou o ficheiro de correio lotus no servidor Domino.
* HTTPPassword - Propriedade opcional que contém a palavra-passe de acesso à Web para o objeto.

Para aceder ao Servidor Dominó sem capacidade de correio, a propriedade HTTPPassword deve conter um valor. A propriedade MailFile e a propriedade MailServer podem estar vazias.

Com \_ MMS_ IDStoreType = 2 (id de loja em ficheiro mail), a propriedade MailSystem da NotesRegistrationclass está definida para REG_MAILSYSTEM_INOTES (3).

### <a name="mandatory-attributes"></a>Atributos Obrigatórios
O conector Lotus Domino suporta principalmente este tipo de objetos (tipos de documentos):

* Group
* Base de Dados Mail-In
* Pessoa
* Contacto (Pessoa sem certificador)
* Recurso

Esta secção lista os atributos que são obrigatórios para cada objeto suportado para exportar para um servidor Domino.

| Tipo de Objeto | Atributos Obrigatórios |
| --- | --- |
| Group |<li>Nome da lista</li> |
| Base de Dados Main-In |<li>FullName</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li> |
| Pessoa |<li>LastName</li><li>MailFile</li><li>Nome curto</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li> |
| Contacto (Pessoa sem certificador) |<li>\_MMS_IDRegType</li> |
| Recurso |<li>FullName</li><li>ResourceType</li><li>ConfDB</li><li>Capacidade de Recursos</li><li>Site</li><li>DisplayName</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li> |

## <a name="common-issues-and-questions"></a>Questões e questões comuns
### <a name="schema-detection-does-not-work"></a>A deteção de esquemas não funciona
Para ser capaz de detetar o esquema, é necessário que o ficheiro schema.nsf esteja presente no servidor Domino. Este ficheiro só aparece se o LDAP for instalado no servidor. Se o esquema não for detetável, verifique o seguinte:

* O esquema de ficheiro.nsf está presente na pasta raiz do Servidor Dominó
* O utilizador tem permissões para ver o ficheiro schema.nsf.
* Force o reinício do servidor LDAP. Abra **a Consola Lotus Domino** e utilize o comando Tell **LDAP ReloadSchema** para recarregar o esquema.

### <a name="not-all-secondary-address-books-are-visible"></a>Nem todos os livros de endereços secundários são visíveis
O Conector Domino conta com a funcionalidade **Directy Assistance** para ser capaz de encontrar os livros de endereços secundários. Se faltam os livros de endereços secundários, verifique se a [Assistência do Diretório](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_ABOUT_DIRECTORY_ASSISTANCE.html) foi ativada e configurada no Servidor Dominó.

### <a name="custom-attributes-in-domino"></a>Atributos personalizados em Domino
Existem várias formas em Domino de estender o esquema para que apareça como um atributo personalizado consumível pelo Conector.

**Abordagem 1: Estender o esquema do Domino de Lótus**

1. Crie uma cópia do Modelo do Diretório Dominó {PUBNAMES. NTF} seguindo [estes passos](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (não deve personalizar o modelo de diretório padrão IBM Lotus Domino):
2. Abra a cópia do modelo de diretório Domino {CONTOSO. Modelo NTF} que foi criado no Domino Designer e siga estes passos:
   * Clique em Elementos Compartilhados e expanda subformes
   * Clique duas vezes no subforme ${ObjectName}HerableSchema (onde {ObjectName} é o nome da classe de objeto estrutural padrão, por exemplo: Pessoa).
   * Nomeie o atributo que pretende adicionar no esquema {MyPersonAtrribute} e correspondente a esse atributo. Crie um campo selecionando o Menu **Criar** e, em seguida, selecione **Campo** do menu.
   * No campo adicionado, desapedaça as suas propriedades selecionando o seu Tipo, Estilo, tamanho, fonte e outros parâmetros relacionados na janela propriedades de campo.
   * Mantenha o atributo Valor padrão igual ao nome dado para esse atributo (Por exemplo, se o nome do atributo for MyPersonAttribute, mantenha o valor predefinido com o mesmo nome).
   * Guarde o subforme ${ObjectName}HerableSchema com valores atualizados.
3. Substitua o modelo de diretório dominó {PUBNAMES. NTF} com o novo modelo personalizado {CONTOSO. NTF} seguindo [estes passos](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
4. Feche a Domino Admin e abra a Consola Domino para reiniciar o serviço LDAP e recarregar o Esquema LDAP:
   * Na Consola Domino, insira o comando sob o texto **do Comando Dominó** arquivado para reiniciar o serviço LDAP - [Reiniciar a Tarefa LDAP](https://www.ibm.com/support/knowledgecenter/SSKTMJ_10.0.1/admin/conf_startingandstoppingtheldapservice_c.html).
   * Para recarregar o esquema LDAP use Tell LDAP command - Tell LDAP ReloadSchema
5. Abra o Domino Admin e selecione People & Groups para ver atributo adicionado é refletido no domino Add Person.
6. Abrir o separador De Schema.nsf do separador **Ficheiros** e ver atributo adicionado é refletido na classe de objetos LDAP dominóPerson.

**Abordagem 2: Criar uma auxClass com atributo personalizado e associar-se à classe de objetos**

1. Crie uma cópia do Modelo do Diretório Dominó {PUBNAMES. NTF} seguindo [estes passos](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (Nunca personalize o modelo de diretório padrão IBM Lotus Domino):
2. Abra a cópia do modelo de diretório Domino {CONTOSO. Modelo NTF} que foi criado, no Domino Designer.
3. No painel esquerdo, selecione Código Partilhado e, em seguida, Subformes.
4. Clique em Novo Subforme
5. Faça o seguinte para especificar as propriedades do novo subforme:
   * Com o novo subforme aberto, escolha Design - Subform Properties
   * Ao lado da propriedade Name, insira um nome para a classe de objeto auxiliar - por exemplo, TestSubform.
   * Mantenha a propriedade Opções "Incluir na Subforma Inserção... diálogo " selecionado
   * Desmarcar a propriedade Options "Render pass through HTML in Notes".
   * Deixe as outras propriedades da mesma forma e feche a caixa Subform Properties.
   * Guarde e feche a nova subformação.
6. Faça o seguinte para adicionar um campo para definir a classe de objeto auxiliar:
   * Abra a subformação que criou.
   * Escolha Criar - Campo.
   * Ao lado do Nome no separador Básicos da caixa de diálogo de campo, especifique qualquer nome, por exemplo: {MyPersonTestAttribute}.
   * No campo adicionado, desacione as suas propriedades selecionando o seu Tipo, Estilo, Tamanho, fonte e propriedades relacionadas.
   * Mantenha o atributo Valor padrão igual ao nome dado para esse atributo (Por exemplo, se o nome do atributo for MyPersonTestAttribute, mantenha o valor predefinido com o mesmo nome).
   * Guarde o subforme com valores atualizados e faça o seguinte:
     * No painel esquerdo, selecione Código Compartilhado e, em seguida, Subformes
     * Selecione o novo subforme e escolha Design - Design Properties.
     * Clique no terceiro separador a partir da esquerda e selecione **Propagate esta proibição de alteração de design** .
7. Abra o subforme ${ObjectName}ExtensibleSchema (onde {ObjectName} é o nome da classe de objeto estrutural padrão, por exemplo – Pessoa).
8. Insira o Recurso e selecione o subforme (que criou, por exemplo – TestSubform) e guarde a subformação ${ObjectName}ExtensibleSchema.
9. Substitua o modelo de diretório dominó {PUBNAMES. NTF} com o novo modelo personalizado {CONTOSO. NTF} seguindo [estes passos](https://www.ibm.com/support/knowledgecenter/SSKTMJ_8.5.3/com.ibm.help.domino.admin85.doc/H_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
10. Feche a Domino Admin e abra a Consola Domino para reiniciar o serviço LDAP e recarregar o Esquema LDAP:
    * Na Consola Domino, insira o comando sob o texto **do Comando Dominó** arquivado para reiniciar o serviço LDAP - [Reiniciar a Tarefa LDAP](https://www.ibm.com/support/knowledgecenter/SSKTMJ_10.0.1/admin/conf_startingandstoppingtheldapservice_c.html).
    * Para recarregar o esquema LDAP utilize o comando LDAP **Informe-o recarretaSchema** .
11. Abra o Domino Admin e selecione People & Groups para ver o atributo adicionado é refletido no separador Domino Add Person (no separador Outros).
12. Abrir o separador Schema.nsf do separador **Ficheiros** e ver atributo adicionado é refletido na classe de objeto auxiliar TestSubform LDAP.

**Abordagem 3: Adicione o atributo personalizado à classe ExtensibleObject**

1. Abra o ficheiro {Schema.nsf} colocado no diretório de raiz
2. Selecione as classes de objetos LDAP do menu esquerdo em **Todos os Documentos de Esquema** e clique no botão de classe Adicionar **Objeto:**
3. Fornecer nome LDAP sob a forma de {zzzExtensibleSchema} (onde zzz é o nome da classe de objeto estrutural padrão, por exemplo Pessoa). Por exemplo, para estender o esquema para a classe de objetos pessoais, forneça o nome LDAP {PersonExtensibleSchema}.
4. Forneça o nome da classe Objeto Superior, para o qual pretende estender o esquema. Por exemplo, para estender o esquema para a classe de objetos pessoais, forneça o nome da classe Objeto Superior {dominoPerson}:
5. Forneça um OID válido correspondente à classe de objetos.
6. Selecione atributos estendidos/personalizados nos campos Tipos de Atributos Obrigatórios ou Opcionais de acordo com o requisito:
7. Depois de adicionar os atributos necessários à Classe ExtensívelObject, clique em **Save & Close** .
8. Um ExtensibleObjectClass é criado para a classe de objetos predefinidos com atributos estendidos.

## <a name="troubleshooting"></a>Resolução de problemas
* Para obter informações sobre como permitir a sessão de registo para resolver problemas no conector, consulte o [Rastreio ETW para Conectores](https://go.microsoft.com/fwlink/?LinkId=335731).
