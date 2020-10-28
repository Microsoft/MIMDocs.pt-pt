---
title: História do lançamento da versão do conector Microsoft Docs
description: Este tópico lista todos os lançamentos dos Conectores para O Gestor de Identidade da Vanguarda (FIM) e Do Gestor de Identidade da Microsoft (MIM)
services: active-directory
documentationcenter: ''
author: EugeneSergeev
manager: daveba
editor: ''
reviewer: markwahl-msft
ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/23/2019
ms.author: esergeev
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3062058bc53a75c66959803ebb5b33849c11868b
ms.sourcegitcommit: babd0299472aa7e8c8d9af1b464bf4e91318aed8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2020
ms.locfileid: "92762582"
---
# <a name="connector-version-release-history"></a>Histórico de Versões do Conector

Os conectores ligam fontes de dados conectadas específicas ao Microsoft Identity Manager (MIM) e ao Azure AD Connect. (Em "Frontta Identity Manager", os conectores eram conhecidos como agentes de gestão.) Muitos dos conectores, tais como conectores para fornecer utilizadores no Ative Directory, são entregues como parte da instalação do Serviço de Sincronização MIM e do pacote de instalação do Azure AD Connect. Além disso, mais conectores, como servidores de diretório de terceiros, são enviados como um download separado para que possam ser atualizados com mais frequência para adicionar suporte para ligação a versões atualizadas mim de sistemas-alvo de terceiros.  

> [!NOTE]
> Este tópico é principalmente apenas sobre FIM e MIM Connectors. A menos que explicitamente chamados abaixo, estes Conectores não são suportados para instalação no Azure AD Connect. Os Conectores libertados são pré-instalados no Azure AD Connect ao atualizar para a Build especificada.


Este tópico lista todas as versões do pacote de conectores genéricos que foram lançados separadamente da MIM.  Para obter uma lista de conectores suportados com MIM, consulte [conectores suportados no MIM 2016 SP1](../supported-management-agents.md).  Alguns parceiros criaram os seus próprios conectores desta forma, estando disponível uma lista completa no wiki [FIM 2010 e MIM 2016: Management Agents from Partners](https://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-mim-2016-management-agents-from-partners.aspx).


Links relacionados:

* [Baixar conectores mais recentes](https://go.microsoft.com/fwlink/?LinkId=717495)
* [Documentação genérica de referência do conector LDAP](microsoft-identity-manager-2016-connector-genericldap.md)
* [Documentação genérica de referência do conector SQL](microsoft-identity-manager-2016-connector-genericsql.md)
* [Documentação de referência do conector de serviços web](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws)
* Documentação de referência [do conector PowerShell](microsoft-identity-manager-2016-connector-powershell.md)
* [Documentação de referência do Conector Lotus Domino](microsoft-identity-manager-2016-connector-domino.md)
* Documentação de referência do [conector da loja de utilizadores sharePoint](https://go.microsoft.com/fwlink/?LinkID=331344)

## <a name="1113020-september-2020"></a>1.1.1302.0 (Setembro de 2020)
### <a name="fixed-issues"></a>Problemas corrigidos
- Conector de gráficos
  - Corrigi um problema com atualização de esquema para */beta* endpoint

## <a name="1113010-august-2020"></a>1.1.1301.0 (Agosto de 2020)
### <a name="fixed-issues"></a>Problemas corrigidos
- Conector de gráficos
  - Corrigiu um bug com falhas de importação causadas pelo valor de atributo *do UserType* vazio
  - Fixo um bug com cache de conector de falhas de importação delta não sendo legível causado por erros de descoberta
  - Os intervalos de tempo reportados por procuração e serviço podem ser automaticamente experimentados
  - Descoberta de esquema atualizada para */beta* endpoint 
### <a name="enhancements"></a>Melhoramentos 
- Conector de gráficos
  - Suporte adicional para valores de leitura e escrita de atributos de extensão de diretório personalizado
  - Apoio adicional para leitura *Dos Principais* membros dos grupos no ponto final */beta* 
  - Melhorias de desempenho para as operações de importação delta relacionadas com a descoberta de esquemas
  - O conector de gráficos já pode convidar utilizadores de membros externos

  > [!NOTE]
  > Se tiver usado o convite para hóspedes na construção 1.1.1170.0 do conector, por favor atualize as suas regras de sincronização com a seguinte lógica:

  - Fluxos de saída
    - Um utilizador é convidado a exportar a criação do utilizador, e a exportação inclui um atributo *De Correio,* mas não um *atributo UserPrincipalName* .  Se o *UserPrincipalName* for fornecido, então um utilizador será criado em vez de convidado
    - O atributo *UserType* apenas define se um utilizador se tornará *membro* ou *convidado* (predefinição para *Membro* se não for definido)
  - Fluxos de entrada
    - Os valores do atributo *UserPrincipalName* dos utilizadores externos são renderizados "as-is"

## <a name="1111700-april-2020"></a>1.1.1170.0 (Abril 2020)
### <a name="fixed-issues"></a>Problemas corrigidos
- Conector do SQL genérico
   - Fixo um bug com estratégia de exportação baseada em consultas e atualizações de atributos multi-valor
- Conector de notas de lótus
   - Os grupos de livros de endereços de notas secundárias já não são eliminados pelo processo *AdminP.* A operação de eliminação direta é usada agora
- Conector LDAP genérico
   - Fixo um bug com atributos de operações de diretório LDAP, por exemplo *pwdUpdateTime,* não visível no esquema
### <a name="enhancements"></a>Melhoramentos 
- Conector de gráficos   
   - UPNs de utilizadores de hóspedes externos já não são renderizados 'as-is', em vez disso são mostrados no espaço do conector para parecer e-mails
   - Suporte adicional para o provisionamento de utilizadores convidados B2B

   Para convidar utilizadores convidados B2B, você precisa:
   - Conceda permissões para convidar os hóspedes para a sua aplicação AD Azure associada ao conector Graph
   - Complete a secção de configuração do conector para convidar utilizadores externos: desacione o URL de redirecionamento de convite (obrigatório) e escolha se envia e-mails de convite
   - Desabrar os atributos obrigatórios na sua regra de sincronização de saída:
     - "Guest"=> *userType* (apenas fluxo inicial)
     - endereço de e-mail externo=> *nome do utilizadorPrincipal*
     - CustomExpression ("CN="="="csObjectID+""OBJECT=user")=> *dn* (apenas fluxo inicial)
     - csObjectID= *id* >(apenas fluxo inicial)

## <a name="1111300-february-2020"></a>1.1.1130.0 (Fevereiro de 2020)
### <a name="fixed-issues"></a>Problemas corrigidos
- Conector de gráficos
   - Exportação já não falha ao tentar adicionar um membro a um grupo que já foi adicionado
   - Suporte de atributo virtual 'export_password' é fixo, não há necessidade de configurar o fluxo de atributos 'password'
   - Fluxo fixo de exportação de atributos de cadeia multi-valor
   - Fixos vários bugs que afetam a importação delta
   - Vários potenciais bugs de formato de data fixos
- Conector do SQL genérico
   - Vários bugs de UI corrigidos
   - Manuseamento de referências incorretas fixas durante a importação delta
   - Fixo um bug com a estratégia de importação delta de rastreio de alterações SQL e tabelas multi-valorizadas para importar mudanças de membro do grupo corretamente
   - Fixo um bug com valores de atributos não apurados na exportação
   - Fixo um bug com último elemento de atributo de referência multi-valor não eliminado na exportação
   - Fixo um bug com atualização de esquema fazendo com que os atributos de referência sejam definidos em cordas
   - Fixo um bug com parâmetros de procedimento armazenados valores sendo truncados a 397 bytes
   - Fixo um bug com tabelas Oráculo e vistas deteção de esquema sendo sensível a casos
- Conector de notas de lótus
   - Desempenho melhorado ao importar membros do grupo
   - Importação total já não falha com "erros de referência nulos"
   - Corrigiu um bug com a eliminação da caixa de correio notes quando a ACL está definida
   - Nomes de grupo vazios já não causam falhas nas importações delta
   - Fixo um bug com caracteres não impressos deixados em atributos após supressões de valores de corda
- Conector LDAP genérico
   - A importação da Delta já não mostra valor literal de "substituição" quando não é definido qualquer valor no mente de origem
### <a name="enhancements"></a>Melhoramentos 
- Conector de gráficos
   - Suporte adicional para nuvens soberanas e capacidade de configurar URLs de Login e Recursos Gráficos
   - Os atributos não suportados são filtrados e escondidos nas propriedades do conector após a descoberta do esquema

## <a name="4418001-july-2019"></a>4.4.1800.1 (Julho 2019)
### <a name="enhancements"></a>Melhoramentos:
- Conector da loja de perfis de utilizador SharePoint
   - Suporte adicional para SharePoint Server 2019. O conector está disponível como download do [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=279713) 

## <a name="119530-june-2019"></a>1.1.953.0 (Junho 2019)

### <a name="fixed-issues"></a>Questões fixas:
- Conector LDAP genérico
   - A importação delta já não falha se o campo de Mudanças estiver vazio no Diretório de Internet da Oracle
- Conector do SQL genérico
   - Corrigi um problema com a estratégia de importação de consultas SQL personalizada usando parâmetros StartIndex e EndIndex, resultando apenas na primeira página a ser importada
   - Corrigiu um problema com a estratégia de importação de mesa/visualização ao ler a MS SQL, resultando apenas na importação da primeira página
- Conector de gráficos:
   - Os contactos organizacionais são agora tratados corretamente, o e-mail já não está em falta
   - Corrigiu um problema com as operações de importação e exportação de atributos do gestor. O conector já não falha quando o gerente está vazio. O valor do gestor é atualizado corretamente em Azure AD
   - Corrigiu um problema com a importação delta de valores vazios
   - Corrigi um problema com o conector a colidir com a importação delta quando o ficheiro de cache local é corrompido
   - Corrigiu vários problemas com exportação incorreta de valores vazios ou quando apenas o caso do valor do atributo mudou
   - O conector agora lida com operações de eliminação/adicionar para o mesmo atributo durante a execução de exportação corretamente
   - Corrigiu vários problemas com importações e exportações de longa duração quando os tokens de acesso expiravam durante o período de funcionamento do conector. Connector renova agora fichas de acesso quando necessário sem falha
   - Corrigi um problema com o último membro de um grupo que não foi eliminado
### <a name="enhancements"></a>Melhoramentos:
- Conector do SQL genérico
   - o parâmetro de tempo de comando de um leitor de dados está definido para combinar o tempo limite do conector. Se tiver consultas de longa duração, demorando mais de 30 segundos a concluir, pode aumentar o valor do parâmetro "Tempo limite de comando" na secção "Conectividade".
- Conector de gráficos: 
   - Adicionou uma estratégia de importação completa de membros multi-roscados para melhorar o desempenho das importações. A importação delta continua a ser uma operação de rosca única
   - Suporte adicionado para tipos de esquemas complexos resultantes como OnPremisesExtentionAttributes.* estando agora disponível
   - Suporte adicional para export_password atributo para evitar erros de mudança de exportação-não-reimportados e não mostrar a palavra-passe inicial no espaço do conector. O comportamento é semelhante a outros conectores ECMA2
   - Adicionei um manipulador para apoiar pedidos https de estrangulamento. Quando a réplica AD AD do Azure recebe demasiados pedidos de um cliente, pode responder com Retry-After instrução. O conector vai parar e tentar novamente em vez de falhar
   - O perfil de importação delta deixará de começar se os filtros de consulta forem definidos. Se quiser importar apenas objetos específicos da Azure AD, por exemplo, os utilizadores que têm o apelido que começa com A*, então a funcionalidade de importação delta será bloqueada


## <a name="119130-january-2019"></a>1.1.913.0 \( janeiro 2019\)

### <a name="fixed-issues"></a>Questões fixas:

* SQL genérico:
    * Fixou um erro de regressão no Conector SQL Genérico onde estava apenas a ler os primeiros 5000 objetos.
* Conector de gráficos:
    * Corrigiu um problema em que o Conector API gráfico não conseguiu ler/escrever a partir de um inquilino ou criar um novo conector quando a opção beta é selecionada em conectividade.

### <a name="enhancements"></a>Melhoramentos:

* Conector de gráficos:
    * Desagram o protocolo TLS 1.2 como o padrão para o conector gráfico.
* Conector SQL genérico:
    * O Conector SQL genérico foi testado com o Oracle 12c e o Oracle 18c.

## <a name="119110"></a>1.1.911.0

> [!NOTE]  
> Esta versão de construção do conector destina-se apenas a ser utilizada nas implementações do Azure AD Connect e não é fornecida para utilização em implementações MIM.
> 
> Em comparação com o lançamento anterior do conector, não contém melhorias ou atualizações para os clientes MIM.

-  Atualiza os conectores não standard (por exemplo, Conector LDAP Genérico e Conector SQL Genérico) enviados com Azure AD Connect.  

## <a name="118610"></a>1.1.861.0

### <a name="fixed-issues"></a>Questões fixas:
* Alterar títulos para toda a configuração do conector de 'Frente para Microsoft'

* Notas de Lótus: 
    * Erros na COM com Lotus às vezes não devolvem erros
* LDAP genérico:
    * Gldap Extra símbolo para DI com servidor de diretório PING
* Serviços Web Genéricos:
    * Erro na exportação de json parsing
* SQL genérico:
    * Atributo Binário de Exportação
    * Os tipos de objetos não podem ser sublagem uns dos outros
    * As alterações na tabela multi-valorizadas não são acompanhadas no funcionamento da "importação Delta", se "Estratégia Delta" for "Change Tracking"
* Conector de gráficos (Visualização pública)
    * Erro nas Eliminações de Grupo
    * Atualizar User-Agent para http header
    * Conector não valida ID do Cliente e Segredo do Cliente
    * Nome do inquilino arquivado deve ser aparado

### <a name="enhancements"></a>Melhoramentos:
* Notas de Lótus: *Adicione a capacidade de aumentar o tempo limite através da UI
* Conector de gráfico (Visualização pública) *O atributo palavra-passe é filtrado na Importação para eliminar "Export-not-reimported".
    *Adicione suporte ao parâmetro de consulta $filter - Limitado a operações com todos os filtros que funcionam em consulta delta, também funcionará no conector *Atualizado para usar o nextLink diretamente em vez de extrair skipToken para detalhes de paging [aqui](https://developer.microsoft.com/en-us/graph/docs/concepts/paging)

## <a name="118300"></a>1.1.830.0

### <a name="fixed-issues"></a>Questões fixas:
* Sistema de Conectors Resolvido.Diagnostics.EventLogInternal.InternalWriteEvent (Mensagem: Um dispositivo ligado ao sistema não está a funcionar)
* Nesta versão dos conectores, será necessário atualizar o redirecionamento de encadernação de 3.3.0-4.1.3.0 para 4.1.4.0 em miiserver.exe.config
* Serviços Web Genéricos:
    * Resposta JSON válida resolvida não pôde ser guardada na ferramenta de configuração
* SQL genérico:
    * A exportação gera sempre apenas consulta de atualização para o funcionamento da eliminação. Adicionado para gerar uma consulta de eliminação
    * A consulta SQL, que obtém objetos para o funcionamento da Delta Import, se a "Estratégia Delta" for 'Change Tracking' foi corrigida. Nesta implementação, a limitação conhecida: a Delta Import com o modo 'Change Tracking' não acompanha as alterações nos atributos multi-valorizados
    * Possibilidade acrescida de gerar uma consulta de eliminação para o caso, quando é necessário eliminar o último valor do atributo multivalorizado e esta linha não contém quaisquer outros dados exceto o valor, que é necessário eliminar.
    * Sistema.Manipulação do argumentExcepção quando implementados parâmetros de SAÍDA por SP
    * Consulta incorreta para fazer o funcionamento da exportação em campo, que tem varbinário(máx)
    * Problema com parâmetroSA variável foi inicializada duas vezes (nas funções ExportAttributes e GetQueryForMultiValue)


## <a name="116490-aadconnect-116490"></a>1.1.649.0 (AADConnect 1.1.649.0)

### <a name="fixed-issues"></a>Questões fixas:

* Notas de Lótus:
  * Filtrar a opção de certificadores personalizados
  * Importação da classe ImportOperations fixou a definição de que operações podem ser executadas no modo 'Vistas' e que no modo 'Search'.
* LDAP genérico:
  * O Diretório OpenLDAP utiliza o DN como âncora em vez de entradaUI. Nova opção para o conector GLDAP, que permite modificar âncora
* SQL genérico:
  * Exportação fixa para o campo, que tem varbinário(máx).
  * Ao adicionar dados binários de uma fonte de dados a um objeto CSEntry, a função DataTypeConversion falhou em bytes zero. Função de DataTypeConversion da classe CSEntryOperationBase.




### <a name="enhancements"></a>Melhoramentos:

* SQL genérico:
  * A capacidade de configurar o modo de execução do procedimento armazenado com parâmetros nomeados ou não é adicionada numa janela de configuração do agente de gestão Genérico SQL na página 'Parâmetros Globais'. Na página 'Parâmetros Globais', existe caixa de verificação com a etiqueta "Use parâmetros nomeados para executar um procedimento armazenado", que é responsável pelo modo de execução do procedimento armazenado com parâmetros nomeados ou não.
    * Atualmente, a capacidade de executar procedimentos armazenados com parâmetros nomeados funciona apenas para bases de dados IBM DB2 e MSSQL. Para bases de dados Oracle e MySQL esta abordagem não funciona: 
      * A sintaxe SQL do MySQL não suporta parâmetros nomeados em procedimentos armazenados.
      * O condutor da ODBC para o Oráculo não suporta parâmetros nomeados para parâmetros nomeados em procedimentos armazenados)

## <a name="116040-aadconnect-116140"></a>1.1.604.0 (AADConnect 1.1.614.0)


### <a name="fixed-issues"></a>Questões fixas:

* Serviços Web Genéricos:
  * Corrigiu um problema que impedia a criação de um projeto SOAP quando existiam dois ou mais pontos finais.
* SQL genérico:
  * No funcionamento da importação, o GSQL não estava a converter o tempo corretamente, quando guardado para o espaço do conector. A data e o formato de tempo padrão para o espaço do conector do GSQL foi alterado de 'yyyy-MM-dd hh:mm:mm:ssZ' para 'yyyy-MM-dd HH:mm:ssZ'.

## <a name="115510-aadconnect-115530"></a>1.1.551.0 (AADConnect 1.1.553.0)

### <a name="fixed-issues"></a>Questões fixas:

* Serviços Web Genéricos:
  * A ferramenta WSconfig não converteu corretamente a matriz Json de "pedido de amostra" para o método de serviço REST. Isto causou problemas com a serialização deste conjunto Json para o pedido REST.
  * Ferramenta de configuração do conector de serviço web não suporta o uso de símbolos de espaço em nomes de atributos JSON 
    * Um padrão de substituição pode ser adicionado manualmente ao ficheiro WSConfigTool.exe.config, por exemplo,  ```<appSettings> <add key="JSONSpaceNamePattern" value="__" /> </appSettings>```
      > [!NOTE]
      > A tecla JSONSpaceNamePattern é necessária para exportação receberá o seguinte erro: Mensagem: Nome vazio não é legal. 

* Notas de Lótus:
  * Se a opção **Permitir que os certificadores personalizados para Unidades Organizacionais/Organizacionais** sejam desativados, então o conector falha durante a exportação (Atualização) Depois do fluxo de exportação todos os atributos são exportados para a Domino, mas no momento da exportação um KeyNotFoundException é devolvido ao Sync. 
    * Isto acontece porque a operação do renome falha quando tenta alterar DN (atributo UserName) alterando um dos atributos abaixo:  
      - LastName
      - FirstName
      - Médio-Initial
      - AltFullName
      - AltFullNameLanguage
      - ou
      - altcommonname

  * Quando se permite que os certificadores personalizados para a opção **Organização/Unidades Organizacionais** estejam ativados, mas os certificadores necessários ainda estão vazios, então ocorrerá o KeyNotFoundException.

### <a name="enhancements"></a>Melhoramentos:

* SQL genérico:
  * **Cenário: redesenhado implementado:** recurso "*"
  * **Descrição da solução:** Alteração da abordagem para [o manuseamento de atributos de referência multi-valor.](microsoft-identity-manager-2016-connector-genericsql.md)


### <a name="fixed-issues"></a>Questões fixas:

* Serviços Web Genéricos:
  * Não é possível importar a configuração do Servidor se o Conector WebService estiver presente
  * O WebService Connector não está a trabalhar com vários Serviços Web

* SQL genérico:
  * Nenhum tipo de objeto está listado para atributo referenciado de valor único
  * Delta import on Change Tracking strategy elimina objeto quando valor é removido da tabela multi-valor
  * OverflowExcepção no conector GSQL com DB2 em AS/400

Lótus:
  * Opção adicional para ativar\desativar a pesquisa de OUs antes de abrir a página GlobalParameters

## <a name="114430"></a>1.1.443.0

Lançado: 2017 março

### <a name="enhancements"></a>Melhoramentos 

* SQL genérico:</br>
  **Sintomas do cenário:**   É uma limitação bem conhecida com o Conector SQL onde apenas permitimos uma referência a um tipo de objeto e requerem referência cruzada com os membros. </br>
  **Descrição da solução:** No passo de processamento para referências escolhidas é escolhida a opção "*", todas as combinações de tipos de objetos serão devolvidas ao motor de sincronização.

> [!Important]
> - Isto vai criar muitos espaços reservados
> - É necessário certificar-se de que o naming é um tipo único de objetos cruzados.


* LDAP genérico:</br>
  **Cenário:** Quando apenas poucos recipientes são selecionados em partição específica, então a pesquisa ainda será feita em toda a partição. O specific será filtrado pelo Serviço de Sincronização, mas não por MA, que pode causar degradação do desempenho. </br>

  **Descrição da solução:** Alterou o código do conector GLDAP para que fosse possível passar por todos os recipientes e objetos de busca em cada um deles, em vez de procurar em toda a partição.


* Lotus Domino:

  **Cenário:** Apoio de eliminação de correio dodo para uma remoção de pessoa durante uma exportação. </br>
  **Solução:** Suporte de eliminação de correio configurável para a remoção de uma pessoa durante uma exportação.

### <a name="fixed-issues"></a>Questões fixas:
* Serviços Web Genéricos:
  * Ao alterar o URL de serviço em projetos padrão SAP WSconfig através da Ferramenta de Configuração do WebService, então o seguinte erro acontece: Não foi possível encontrar uma parte do caminho

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* LDAP genérico:
  * O Conector GLDAP não vê todos os atributos em LDS AD
  * O assistente quebra quando não são detetados atributos UPN a partir do esquema de diretório LDAP
  * Delta Imports Falha com erros de descoberta não presentes durante a importação completa, quando o atributo "objectclass" não é selecionado
  * Uma página de configuração "Configurar partições e hierarquias" não mostra nenhum objeto que tipo seja igual à partição de servidores novos no Genérico  
  LDAP MA. Só mostraram objetos da partição RootDSE.


* SQL genérico:
  * Correção para marca de água SqL genérica Delta Import atributo multivalorizado não importado bug
  * Ao exportar valores eliminados\adicionados de atributo multivalorizado, não são eliminados\adicionados na fonte de dados.  


* Notas de Lótus:
  * No entanto, um campo específico "Nome Completo" é mostrado corretamente no metaverso ao exportar para Notas o valor do atributo é Nulo ou Vazio.
  * Correção para erro certificador duplicado
  * Quando o Objeto sem dados é selecionado no Conector Lotus Domino com outros objetos, então recebemos o erro discovery enquanto executamos a Full-Import.
  * Quando a Delta Import está a funcionar no Conector Lotus Domino, no final dessa corrida, o serviço Microsoft.IdentityManagement.MA.LotusDomino.Service.exe às vezes devolve um Erro de Aplicação.
  * A adesão ao grupo funciona bem e é mantida, exceto quando executa a exportação para tentar remover um utilizador da adesão, mostra-se bem sucedida com uma atualização, mas o utilizador não é realmente removido da adesão em Notas de Lotus.
  * Foi adicionada uma oportunidade de escolher o modo de exportação como "Item de apêndice na parte inferior" na configuração GUI da Lotus MA para anexar novos itens na parte inferior durante a exportação para atributos multi-valorizados.
  * O conector adicionará a lógica necessária para eliminar o ficheiro da Pasta de Correio e do Cofre de Identificação.
  * Eliminar membros que não trabalham para membro do NAB.
  * Os valores devem ser eliminados com sucesso do atributo multi-valor

## <a name="111170"></a>1.1.117.0
Lançado: 2016 março

**Novo Conector**  
Libertação inicial do [Conector SQL Genérico.](microsoft-identity-manager-2016-connector-genericsql.md)

**Novas funcionalidades:**

* Conector LDAP genérico:
  * Apoio adicional à importação da Delta com a Isode.
* Conector de Serviços Web:
  * Atualizou a atividade csEntryChangeResult e definiu a atividade DoportErrorCode para permitir que os erros de nível de objeto sejam devolvidos ao motor de sincronização.
  * Atualize os modelos SAP6 e SAP6User para utilizar a nova funcionalidade de erro de nível de objeto.
* Conector Lotus Domino:
  * Para exportação, precisa de um certificador por livro de endereços. Agora pode usar a mesma palavra-passe para todos os certificadores para facilitar a gestão.

**Questões fixas:**

* Conector LDAP genérico:
  * Para a IBM Tivoli DS, alguns atributos de referência não foram detetados corretamente.
  * Para o LDAP aberto durante uma importação delta, os espaços brancos no início e no fim das cordas foram truncados.
  * Para a Novell e a NetIQ, uma exportação que movia um objeto entre OUs/contentores e ao mesmo tempo renomeado o objeto falhou.
* Conector de Serviços Web:
  * Se o serviço web tinha vários pontos finais para a mesma ligação, então o Conector não descobriu corretamente estes pontos finais.
* Conector Lotus Domino:
  * Uma exportação do atributo FullName para uma base de dados por correio não funcionou.
  * Uma exportação que adicionou e retirou membros de um grupo apenas exportou os membros adicionados.
  * Se um Documento de Notas for inválido (o atributo éValide definido para falso), então o Conector falha.

## <a name="older-releases"></a>Lançamentos mais antigos
Antes de março de 2016, os Connectors foram lançados como tópicos de apoio.

**LDAP Genérico**

* [KB3078617](https://support.microsoft.com/kb/3078617) - 1.0.0597, 2015 setembro
* [KB3044896](https://support.microsoft.com/kb/3044896) - 1.0.0549, 2015 março
* [KB3031009](https://support.microsoft.com/kb/3031009) - 1.0.0534, 2015 janeiro
* [KB3008177](https://support.microsoft.com/kb/3008177) - 1.0.0419, 2014 setembro
* [KB2936070](https://support.microsoft.com/kb/2936070) - 4.3.1082, 2014 março

**WebServices**

* [KB3008178](https://support.microsoft.com/kb/3008178) - 1.0.0419, 2014 setembro

**PowerShell**

* [KB3008179](https://support.microsoft.com/kb/3008179) - 1.0.0419, 2014 setembro

**Lótus Domino**

* [KB3096533](https://support.microsoft.com/kb/3096533) - 1.0.0597, 2015 setembro
* [KB3044895](https://support.microsoft.com/kb/3044895) - 1.0.0549, 2015 março
* [KB2977286](https://support.microsoft.com/kb/2977286) - 5.3.0712, 2014 agosto
* [KB2932635](https://support.microsoft.com/kb/2932635) - 5.3.1003, 2014 fevereiro  
* [KB2899874](https://support.microsoft.com/kb/2899874) - 5.3.0721, 2013 outubro
* [KB2875551](https://support.microsoft.com/kb/2875551) - 5.3.0534, 2013 agosto

## <a name="troubleshooting"></a>Resolução de problemas 

> [!NOTE]
> Ao atualizar o Microsoft Identity Manager ou o AADConnect com a utilização de qualquer um dos conectores ECMA2. 

Tem de atualizar a definição do conector no momento da atualização para corresponder ou receberá o seguinte erro no registo do evento de aplicação começando a reportar o ID 6947: "Versão de montagem na configuração do conector AAD ("X.X.XXX. X") é mais cedo do que a versão real ("X.X.XXX. X") de "C:\Program Files\Microsoft Azure AD Sync\Extensions\Microsoft.IAM.Connector.GenericLdap.dll".

Para refrescar a definição:
* Abra as propriedades para a instância do conector
* Clique no separador 'Ligação/ Ligar-se ao separador'
  * Introduza a palavra-passe para a conta do Conector
* Clique em cada um dos separadores de propriedade, por sua vez
  * Se este tipo de Conector tiver um separador Partições, com um botão 'Refresh', clique no botão 'Refresh' enquanto está no separador
* Depois de todos os separadores de propriedade terem sido acedidos, clique no botão OK para guardar as alterações.

## <a name="other-connectors"></a>Outros conectores

Além dos conectores listados acima, os conectores do SharePoint e um conector legado para o Windows Azure Ative Directory também foram distribuídos separadamente da MIM.

### <a name="sharepoint-user-profile"></a>Perfil de utilizador do SharePoint

O Conector de Gestor de Identidade da SharePoint ajuda-o a sincronizar informações de identidade na Loja de Perfil do Utilizador no SharePoint 2013 e no SharePoint 2016.   A versão 4.3.2430.0 deste conector, publicado 12/19/2016 pode ser descarregada a partir do [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=41164).

Mais informações sobre este conector podem ser encontradas no [rollup de hotfix](https://support.microsoft.com/en-us/help/3156030/hotfix-rollup-build-4-3-2201-0-is-available-for-forefront-identity-man) e instruções sobre como [usar uma solução MIM de amostra no SharePoint Server 2016](https://docs.microsoft.com/SharePoint/administration/use-a-sample-mim-solution-in-sharepoint-server-2016).

### <a name="forefront-identity-manager-connector-for-windows-azure-active-directory-legacy-connector"></a>Conector de gestor de identidade da vanguarda para o Windows Azure Ative Directory (conector legado)

O Azure AD Connector for FIM foi uma tecnologia antiga para sincronizar informações de identidade ao Azure Ative Directory. O Conector AZURE AD para FIM, versão 1.0.6635.0069 a partir de 19 de fevereiro de 2014, encontra-se no congelamento de funcionalidades. [Não](https://www.microsoft.com/en-us/download/details.aspx?id=41166) recebe nenhuma atualização, mas ainda é suportado. A solução de utilização do FIM e do Azure AD Connector foi substituído pelo [Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect). A Microsoft recomenda que não inicie uma nova implementação utilizando este Conector.

## <a name="next-steps"></a>Passos seguintes

Saiba mais sobre a documentação de referência [do Conector LDAP Genérico](microsoft-identity-manager-2016-connector-genericldap.md) Saiba mais sobre a documentação de referência[do Conector SQL Genérico](microsoft-identity-manager-2016-connector-genericsql.md) Saiba mais sobre a documentação de referência do [Conector de Serviços Web](microsoft-identity-manager-2016-ma-ws.md) Saiba mais sobre a documentação de referência do [Conector PowerShell](microsoft-identity-manager-2016-connector-powershell.md) Saiba mais sobre a documentação de referência [do Conector Lotus Domino](microsoft-identity-manager-2016-connector-domino.md)
