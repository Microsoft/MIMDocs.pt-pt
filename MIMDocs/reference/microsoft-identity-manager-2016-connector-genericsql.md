---
title: Conector SQL Genérico Microsoft Docs
description: Este artigo descreve como configurar o Conector SQL Genérico da Microsoft.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: fd8ccef3-6605-47ba-9219-e0c74ffc0ec9
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 04/02/2018
ms.author: billmath
ms.openlocfilehash: 862ea1ed910905daa0f2e0be97838f3da2423661
ms.sourcegitcommit: 2950ef38055bf78e3b98e4cd84771b04c2aa5584
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/08/2020
ms.locfileid: "92762572"
---
# <a name="generic-sql-connector-technical-reference"></a>Referência técnica genérica do Conector SQL
Este artigo descreve o Conector SQL Genérico. O artigo aplica-se aos seguintes produtos:

* Microsoft Identity Manager 2016 (MIM2016)
* Gestor de Identidades Da Vanguarda 2010 R2 (FIM2010R2)
  * Deve utilizar o hotfix 4.1.3671.0 ou mais tarde [KB3092178](https://support.microsoft.com/kb/3092178).

Para MIM2016 e FIM2010R2, o Conector está disponível como download do [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkId=717495).

Para ver este Conector em ação, consulte o artigo [do Conector SQL Genérico passo a passo.](microsoft-identity-manager-2016-connector-genericsql-step-by-step.md)

## <a name="overview-of-the-generic-sql-connector"></a>Visão geral do Conector SQL Genérico
O Conector SQL Genérico permite-lhe integrar o serviço de sincronização com um sistema de base de dados que oferece conectividade ODBC.  

De uma perspetiva de alto nível, as seguintes funcionalidades são suportadas pela atual libertação do conector:

| Funcionalidade | Suporte |
| --- | --- |
| Fonte de dados conectada |O Conector é suportado com todos os controladores ODBC de 64 bits*. Foi testado com o seguinte: <li>Microsoft SQL Server & SQL Azure</li><li>IBM DB2 10.x</li><li>IBM DB2 9.x</li><li>Oráculo 10 & 11g</li><li>Oráculo 12c e 18c</li><li>MySQL 5.x</li> |
| Cenários |<li>Gestão do ciclo de vida de objetos</li><li>Gestão de palavra-passe</li> |
| Operações |<li>Importação Completa e Importação Delta, Exportação</li><li>Para exportação: Adicionar, Eliminar, Atualizar e Substituir</li><li>Definir palavra-passe, alterar senha</li> |
| Esquema |<li>Descoberta dinâmica de objetos e atributos</li> |

> [!NOTE]
> *As ligações a fontes de dados não listadas acima estão atualmente limitadas a cenários apenas de leitura.

### <a name="prerequisites"></a>Pré-requisitos
Antes de utilizar o Conector, certifique-se de que tem o seguinte no servidor de sincronização:

* Microsoft .NET 4.5.2 Framework ou mais tarde
* Motoristas clientes ODBC de 64 bits
* Se estiver a utilizar o conector para comunicar com o Oracle 12c, isto requer o Cliente Instantâneo Oracle 12.2.0.1 ou mais recente com o pacote ODBC.
* Se estiver a utilizar o conector para comunicar com o Oracle 18c, isto requer o Cliente Instantâneo Oracle 18.3.0.0 ou mais recente com o Pacote ODBC, e a variável do sistema NLS_LANG a ser definida para suportar caracteres UTF8.

### <a name="permissions-in-connected-data-source"></a>Permissões na fonte de dados conectada
Para criar ou executar qualquer uma das tarefas suportadas no conector GENÉRICO SQL, tem de ter:

* db_datareader
* db_datawriter

### <a name="ports-and-protocols"></a>Portos e protocolos
Para as portas necessárias para o funcionação do controlador ODBC, consulte a documentação do vendedor de bases de dados.

## <a name="create-a-new-connector"></a>Criar um novo Conector
Para criar um conector SQL genérico, no **Serviço de Sincronização** selecione **o Agente de Gestão** e **crie** . Selecione o **Conector GENÉRICO SQL (Microsoft).**

![CriarConnector](./media/microsoft-identity-manager-2016-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Conectividade
O Conector utiliza um ficheiro DSN ODBC para conectividade. Crie o ficheiro DSN utilizando **fontes de dados ODBC encontradas** no menu inicial em **Ferramentas Administrativas** . Na ferramenta administrativa, crie um **Ficheiro DSN** para que possa ser fornecido ao Conector.

![CriarConnector](./media/microsoft-identity-manager-2016-connector-genericsql/connectivity.png)

O ecrã de conectividade é o primeiro quando cria um novo Conector SQL Genérico. Primeiro tem de fornecer as seguintes informações:

* Caminho de arquivo DSN
* Autenticação
  * Nome de Utilizador
  * Palavra-passe

A base de dados deve suportar um destes métodos de autenticação:

* **Autenticação do Windows** : A base de dados de autenticação utiliza as credenciais do Windows para verificar o utilizador. O nome de utilizador/palavra-passe especificado é utilizado para autenticar na base de dados. Esta conta precisa de permissões para a base de dados.
* **Autenticação SQL** : A base de dados de autenticação utiliza o nome de utilizador/palavra-passe definido pelo ecrã de Conectividade para ligar à base de dados. Se armazenar o nome de utilizador/pasword no ficheiro DSN, as credenciais fornecidas no ecrã de Conectividade têm precedência.
* **Autenticação da Base de Dados Azure SQL** : Para obter mais informações, consulte [a base de dados SQL utilizando a autenticação do Diretório Ativo Azure](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure).

**DN é Âncora** : Se selecionar esta opção, o DN também é utilizado como atributo de âncora. Pode ser usado para uma implementação simples, mas também tem a seguinte limitação:

* O conector suporta apenas um tipo de objeto. Portanto, quaisquer atributos de referência só podem referenciar o mesmo tipo de objeto.

**Tipo de exportação: Substituição do objeto:** Durante a exportação, quando apenas alguns atributos foram alterados, todo o objeto com todos os atributos é exportado e substitui o objeto existente.

### <a name="schema-1-detect-object-types"></a>Esquema 1 (Detetar tipos de objetos)
Nesta página, vai configurar como o Conector vai encontrar os diferentes tipos de objetos na base de dados.

Cada tipo de objeto é apresentado como uma partição e configurado ainda mais em **Divisórias e Hierarquias configuradas.**

![schema1a](./media/microsoft-identity-manager-2016-connector-genericsql/schema1a.png)

**Método de deteção do tipo de objeto** : O Conector suporta estes métodos de deteção do tipo de objeto.

* **Valor Fixo** : Fornece a lista de tipos de objetos com uma lista separada por vírgula. Por exemplo: `User,Group,Department`.  
  ![schema1b](./media/microsoft-identity-manager-2016-connector-genericsql/schema1b.png)
* **Tabela/Visualização/Procedimento armazenado** : Forneça o nome do procedimento tabela/vista/armazenado e, em seguida, o nome da coluna que fornece a lista de tipos de objetos. Se utilizar um procedimento armazenado, também forneça parâmetros para o mesmo no formato **[Nome]:[Direção]:[Valor]** . Forneça cada parâmetro numa linha separada (utilize Ctrl+Enter para obter uma nova linha).  
  ![schema1c](./media/microsoft-identity-manager-2016-connector-genericsql/schema1c.png)
* **Consulta SQL** : Esta opção permite-lhe fornecer uma consulta SQL que devolve uma única coluna com tipos de objetos, por `SELECT [Column Name] FROM TABLENAME` exemplo. A coluna devolvida deve ser de tipo de corda (varchar).

### <a name="schema-2-detect-attribute-types"></a>Schema 2 (Detetar tipos de atributos)
Nesta página, vai configurar como os nomes e tipos de atributos serão detetados. As opções de configuração estão listadas para cada tipo de objeto detetado na página anterior.

![schema2a](./media/microsoft-identity-manager-2016-connector-genericsql/schema2a.png)

**Método de deteção do tipo** de atributo : O Conector suporta estes métodos de deteção do tipo de atributo com todos os tipos de objetos detetados no ecrã Schema 1.

* **Tabela/Visualização/Procedimento armazenado** : Forneça o nome da tabela/visualização/procedimento armazenado que deve ser utilizado para encontrar os nomes dos atributos. Se utilizar um procedimento armazenado, também forneça parâmetros para o mesmo no formato **[Nome]:[Direção]:[Valor]** . Forneça cada parâmetro numa linha separada (utilize Ctrl+Enter para obter uma nova linha). Para detetar os nomes de atributos num atributo multi-valor, forneça uma lista separada de vírgulas de Tabelas ou Vistas. Os cenários multivalorizados não são suportados quando a tabela dos pais e das crianças tem os mesmos nomes de colunas.
* **Consulta SQL** : Esta opção permite-lhe fornecer uma consulta SQL que devolve uma única coluna com nomes de atributos, por `SELECT [Column Name] FROM TABLENAME` exemplo. A coluna devolvida deve ser de tipo de corda (varchar).

### <a name="schema-3-define-anchor-and-dn"></a>Schema 3 (Definir âncora e DN)
Esta página permite configurar âncora e atributo DN para cada tipo de objeto detetado. Pode selecionar vários atributos para tornar a âncora única.

![schema3a](./media/microsoft-identity-manager-2016-connector-genericsql/schema3a.png)

* Os atributos multi-valorizados e boolean não estão listados.
* O mesmo atributo não pode ser utilizado para DN e âncora, a menos que **o DN seja Âncora** seja selecionado na página de Conectividade.
* Se **o DN é Âncora** é selecionado na página de Conectividade, esta página requer apenas o atributo DN. Este atributo também seria usado como atributo de âncora.

  ![schema3b](./media/microsoft-identity-manager-2016-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>Esquema 4 (Definir tipo de atributo, referência e direção)
Esta página permite configurar o tipo de atributo, como inteiro, binário ou Boolean, e direção para cada atributo. Todos os atributos do esquema de página **2** estão listados, incluindo atributos multi-valor.

![schema4a](./media/microsoft-identity-manager-2016-connector-genericsql/schema4a.png)

* **DataType** : Usado para mapear o tipo de atributo aos tipos conhecidos pelo motor de sincronização. O padrão é utilizar o mesmo tipo detetado no esquema SQL, mas o DateTime e o Reference não são facilmente detetáveis. Para estes, é necessário especificar **data hora** ou **referência** .
* **Direção** : Pode definir a direção de atributo para Importação, Exportação ou ImportExport. ImportExport é padrão.

![schema4b](./media/microsoft-identity-manager-2016-connector-genericsql/schema4b.png)

Notas:

* Se um tipo de atributo não for detetável pelo Conector, utiliza o tipo de dados de corda.
* **As tabelas aninhadas** podem ser consideradas tabelas de base de dados de uma coluna. A Oracle armazena as fileiras de uma mesa aninhada sem ordem em particular. No entanto, quando recupera a tabela aninhada numa variável PL/SQL, as linhas recebem subscritos consecutivos a partir de 1. Isso dá-lhe acesso a linhas individuais.
* **Varrys** não são suportados no conector.

### <a name="schema-5-define-partition-for-reference-attributes"></a>Schema 5 (Definir partição para atributos de referência)
Nesta página, configura-se para todos os atributos de referência a que a partição (tipo de objeto) um atributo se refere.

![schema5](./media/microsoft-identity-manager-2016-connector-genericsql/schema5.png)

Se utilizar **o DN é âncora,** então deve utilizar o mesmo tipo de objeto do que aquele a que se refere. Não é possível fazer referência a outro tipo de objeto.

> [!NOTE]
> A partir da atualização de março de 2017 existe agora uma opção para "*" Quando esta opção for escolhida, todos os tipos de membros possíveis serão importados.

![globalparameters3](./media/microsoft-identity-manager-2016-connector-genericsql/any-option.png)

> [!IMPORTANT]
>  A partir de maio de 2017, a \* " aka **any option** foi alterada para apoiar o fluxo de importação e exportação. Se pretender utilizar esta opção, a sua tabela/vista multi-valorizada deverá ter um atributo que contenha o tipo de objeto.

![](./media/microsoft-identity-manager-2016-connector-genericsql/any-02.png)

 </br> Se "*" for selecionado, o nome da coluna com o tipo de objeto também deve ser especificado.</br> ![](./media/microsoft-identity-manager-2016-connector-genericsql/any-03.png)

Após a importação, verá algo semelhante à imagem abaixo:

  ![globalparameters3](./media/microsoft-identity-manager-2016-connector-genericsql/after-import.png)



### <a name="global-parameters"></a>Parâmetros de Globais
A página De Parâmetros Globais é usada para configurar o método Delta Import, Date/Time e Password.

![globalparameters1](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters1.png)



O Conector SQL Genérico suporta os seguintes métodos para a Importação Delta:

* **Gatilho** : Ver [a geração de vistas delta utilizando gatilhos](https://technet.microsoft.com/library/cc708665.aspx).
* **Marca de água** : Uma abordagem genérica que pode ser utilizada com qualquer base de dados. A consulta de marca de água é pré-povoada com base no fornecedor de bases de dados. Uma coluna de marca de água deve estar presente em todas as mesas/vistas utilizadas. Esta coluna deve rastrear inserções e atualizações nas tabelas como e nas suas tabelas dependentes (multi-valor ou criança). Os relógios entre o Serviço de Sincronização e o servidor de base de dados devem ser sincronizados. Caso contrário, poderão ser omitidas algumas entradas na importação delta.  
  Limitação:
  * A estratégia de marca de água não suporta objetos apagados.
* **Snapshot** : (Funciona apenas com o Microsoft SQL Server) [Gerando vistas delta usando snapshots](https://technet.microsoft.com/library/cc720640.aspx)
* **Change Tracking** : (Funciona apenas com o Microsoft SQL Server) [Sobre o Tracking de Alterações](https://msdn.microsoft.com/library/bb933875.aspx)  
  Limitações:
  * O atributo & DN deve ser parte da chave primária para o objeto selecionado na tabela.
  * A consulta SQL não é suportada durante a importação e exportação com change tracking.

**Parâmetros adicionais** : Especifique o fuso horário do servidor de bases de dados indicando onde está o servidor de base de dados. Este valor é utilizado para suportar os vários formatos de data & atributos de tempo.

O Conector armazena sempre a data e a hora da data no formato UTC. Para poder converter corretamente a data e os horários, o fuso horário do servidor de base de dados e o formato utilizado devem ser especificados. O formato deve ser expresso em formato .Net.

Durante a exportação, todos os atributos de data devem ser fornecidos ao Conector no formato de hora UTC.

![globalparameters2](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters2.png)

**Configuração da palavra-passe** : O conector fornece capacidades de sincronização de palavras-passe e suporta definir e alterar a palavra-passe.

O Conector fornece dois métodos para suportar a sincronização da palavra-passe:

* **Procedimento armazenado** : Este método requer dois procedimentos armazenados para suportar a palavra-passe De & Alterar. Digite todos os parâmetros para adicionar e alterar a operação de senha em **Definir Password SP** e alterar parâmetros SP de **palavra-passe,** respectivamente, de acordo com o exemplo abaixo.
  ![globalparameters3](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters3.png)
* **Extensão de senha** : Este método requer extensão de password DLL (é necessário fornecer o Nome DLL de extensão que está a implementar a interface [IMAExtensible2Password).](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) O conjunto de extensão de palavra-passe deve ser colocado na pasta de extensão para que o conector possa carregar o DLL em tempo de execução.
  ![globalparameters4](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters4.png)

Também tem de ativar a Gestão de Passwords na página **'Configurar' Extensão.**
![globalparameters5](./media/microsoft-identity-manager-2016-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Configurar Partições e Hierarquias
Na página de divisórias e hierarquias, selecione todos os tipos de objetos. Cada tipo de objeto é a sua própria divisória.

![divisórias1](./media/microsoft-identity-manager-2016-connector-genericsql/partitions1.png)

Também pode sobrepor os valores definidos na página **De Conectividade** ou **Parâmetros Globais.**

![divisórias2](./media/microsoft-identity-manager-2016-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Configurar Âncoras
Esta página é lida apenas uma vez que a âncora já foi definida. O atributo de âncora selecionado é sempre anexado ao tipo de objeto para garantir que permanece único entre os tipos de objetos.

![âncoras](./media/microsoft-identity-manager-2016-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>Parâmetro de passo de corrida configurar
Estes passos estão configurados nos perfis de execução do Conector. Estas configurações fazem o trabalho real de importação e exportação de dados.

### <a name="full-and-delta-import"></a>Importação Completa e Delta
Suporte genérico do conector SQL Full e Delta Import utilizando estes métodos:

* Tabela
* Vista
* Procedimento Armazenado
* Consulta SQL

![degrau 1](./media/microsoft-identity-manager-2016-connector-genericsql/runstep1.png)

**Tabela/Vista**  
Para importar atributos multi valorizados para um objeto, tem de fornecer o nome de tabela/visualização em **Nome da tabela/vistas multi-valorizadas** e respetivas condições de união na **condição de Junção** com a tabela dos pais. Se houver mais de uma tabela multi-valorizada na fonte de dados, pode utilizar a união para uma única visão.

> [!IMPORTANT]
> O agente de gestão genérico SQL só pode trabalhar com uma tabela multi-valorizada. Não coloque em Nome da tabela/vistas multi-valorizadas mais do que um nome da tabela. É a limitação do SQL genérico.


Exemplo: Pretende importar o objeto do Empregado e todos os seus atributos multi-valor. Existem duas tabelas, denominadas Empregado (mesa principal) e Departamento (multi-valor).
Faça o seguinte:

* **Tipo empregado** na **tabela/vista/SP** .
* Tipo Departamento em **Nome de tabela/vistas multi-valorizadas** .
* Digite a condição de associação entre o Departamento de & do Empregado em **Condição de Junção,** por exemplo `Employee.DEPTID=Department.DepartmentID` .
  ![degrau2](./media/microsoft-identity-manager-2016-connector-genericsql/runstep2.png)

**Procedimentos armazenados**  
![degrau 3](./media/microsoft-identity-manager-2016-connector-genericsql/runstep3.png)

* Se tiver muitos dados, recomenda-se implementar a paginação com os seus Procedimentos Armazenados.
* Para o seu Procedimento Armazenado para suportar a paginação, precisa fornecer Índice de Início e Índice final. Ver: [Paging eficientemente através de grandes quantidades de dados](https://msdn.microsoft.com/library/bb445504.aspx).
* @StartIndexe @EndIndex são substituídos no tempo de execução com o respetivo valor do tamanho da página configurado na página **Configure Step.** Por exemplo, quando o conector recupera a primeira página e o tamanho da página é definido 500, em tal situação @StartIndex seria 1 e @EndIndex 500. Estes valores aumentam quando o conector recupera as páginas subsequentes e altera o @StartIndex @EndIndex valor &.
* Para executar o Procedimento Despensado Parametrizado, forneça os parâmetros em `[Name]:[Direction]:[Value]` formato. Introduza cada parâmetro numa linha separada (Use Ctrl + Enter para obter uma nova linha).
* O conector GENÉRICO SQL também suporta a operação de importação de servidores ligados no Microsoft SQL Server. Se as informações forem obtidas a partir de uma tabela no servidor linked, então a Tabela deve ser fornecida no formato: `[ServerName].[Database].[Schema].[TableName]`
* O Conector SQL genérico suporta apenas os objetos que têm uma estrutura semelhante (nome ou tipo de dados) entre a informação dos passos de execução e a deteção de esquemas. Se o objeto selecionado a partir do esquema e as informações fornecidas no passo de execução forem diferentes, então o Conector SQL não pode suportar este tipo de cenários.

**Consulta SQL**  
![degrau4](./media/microsoft-identity-manager-2016-connector-genericsql/runstep4.png)

![degrau5](./media/microsoft-identity-manager-2016-connector-genericsql/runstep5.png)

* Várias consultas de conjuntos de resultados não suportadas.
* A consulta SQL suporta a paginação e fornece índice de início e índice de fim como uma variável para suportar a paginação.

### <a name="delta-import"></a>Importação Delta
![runstep6](./media/microsoft-identity-manager-2016-connector-genericsql/runstep6.png)

A configuração da Delta Import requer mais configuração em comparação com a Full Import.

* Se escolher a abordagem Trigger ou Snapshot para rastrear as alterações delta, então forneça a base de dados History Table ou Snapshot na History Table ou na caixa **de nomes da base de dados Snapshot.**
* Você também precisa fornecer condições de junção entre a tabela História e a tabela dos pais, por exemplo `Employee.ID=History.EmployeeID`
* Para rastrear a transação na tabela dos pais a partir da tabela de histórico, deve fornecer o nome da coluna que contém as informações de funcionamento (Adicionar/Atualizar/Eliminar).
* Se escolher a Marca de Água para rastrear as alterações delta, em seguida, forneça o nome da coluna que contém as informações de funcionamento no **Nome da Coluna de Marca de Água** .
* A coluna **de atributos 'Tipo de alteração'** é necessária para o tipo de alteração. Esta coluna mapeia uma alteração que ocorre na tabela primária ou na tabela de vários valores para um tipo de alteração na vista delta. Esta coluna pode conter o tipo de alteração Modify_Attribute para alteração do nível de atributo ou um tipo de adicionar, modificar ou eliminar o tipo de alteração para um tipo de alteração de nível de objeto. Se for algo diferente do valor predefinido Adicionar, Modificar ou Eliminar, então pode definir esses valores usando esta opção.

### <a name="export"></a>Exportar
![degrau 7](./media/microsoft-identity-manager-2016-connector-genericsql/runstep7.png)

Suporte genérico do conector SQL Exportação utilizando quatro métodos apoiados, tais como:

* Tabela
* Vista
* Procedimento Armazenado
* Consulta SQL

**Tabela/Vista**  
Se escolher a opção Tabela/Visualização, então o conector gera as respetivas consultas para fazer a Exportação.

**Procedimentos armazenados**  
![runstep8](./media/microsoft-identity-manager-2016-connector-genericsql/runstep8.png)

Se escolher a opção Procedimento Armazenado, a Exportação requer três procedimentos armazenados diferentes para executar operações de Inserção/Atualização/Eliminação.

* **Adicionar Nome SP** : Este SP funciona se algum objeto vier ao conector para inserção na respetiva tabela.
* **Atualização NOME SP** : Este SP funciona se algum objeto vier ao conector para atualização na respetiva tabela.
* **Excluir nome SP** : Este SP funciona se algum objeto vier ao conector para eliminação na respetiva tabela.
* Atributo selecionado a partir do esquema utilizado como valor de parâmetro ao procedimento armazenado. Por exemplo, `@EmployeeName: INPUT: EmployeeName` (O Nome do Empregado é selecionado no esquema do conector e o conector substitui o respetivo valor durante a exportação)
* Para executar o procedimento de armazenancia parametrizada, forneça parâmetros em `[Name]:[Direction]:[Value]` formato. Introduza cada parâmetro numa linha separada (Use Ctrl + Enter para obter uma nova linha).

**Consulta SQL**  
![runstep9](./media/microsoft-identity-manager-2016-connector-genericsql/runstep9.png)

Se escolher a opção de consulta SQL, a Export requer três consultas diferentes para executar operações de Inserção/Atualização/Eliminação.

* **Insira consulta** : Esta consulta é executado se algum objeto vier ao conector para inserção na respetiva tabela.
* **Consulta de atualização** : Esta consulta é executado se algum objeto vier ao conector para atualização na respetiva tabela.
* **Eliminar consulta** : Esta consulta é executado se algum objeto vier ao conector para eliminação na respetiva tabela.
* Atributo selecionado a partir do esquema utilizado como valor de parâmetro para a consulta, por exemplo `Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Resolução de problemas
* Para obter informações sobre como permitir a sessão de registo para resolver problemas no conector, consulte o [Rastreio ETW para Conectores](https://go.microsoft.com/fwlink/?LinkId=335731).
