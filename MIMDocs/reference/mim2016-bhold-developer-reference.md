---
title: Referência do desenvolvedor da BHOLD para o Microsoft Identity Manager 2016 Microsoft Docs
description: Referência para programadores do BHOLD
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/14/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: edf9f88a4d96d212cef4aa41cbad7d0b4446534f
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762361"
---
# <a name="bhold-developer-reference-for-microsoft-identity-manager-2016"></a>Referência do desenvolvedor BHOLD para Microsoft Identity Manager 2016

O módulo BHOLD-core pode processar comandos de script. Pode ser feito utilizando diretamente o bscript.dll num projeto .NET. Também interagindo com o serviço web b1scriptservice.asmx interface. 

Antes de um script ser executado, todas as informações dentro do script devem ser recolhidas para compor este script. Estas informações podem ser recolhidas a partir das seguintes fontes:

   * Entrada do utilizador
   * Dados do BHOLD
   * Aplicações
   * Outros

Os dados BHOLD podem ser recuperados utilizando a função GetInfo do objeto script. Existe uma lista completa de comandos que podem apresentar todos os dados armazenados na base de dados BHOLD. No entanto, os dados apresentados estão sujeitos às permissões de visualização do utilizador que fez login. O resultado é na forma de um documento XML que pode ser analisado.

Outra fonte de informação pode ser uma das aplicações que são controladas pela BHOLD. O snap-in da aplicação tem uma função especial, o FunctionDispatch, que pode ser usado para apresentar informações específicas da aplicação. Isto é apresentado como um documento XML também.

Finalmente, se não houver outra forma, o script pode conter comandos diretamente para outras aplicações ou sistemas. Não A instalação de software extra no servidor BHOLD pode comprometer a segurança de todo o sistema.

Toda esta informação é colocada num documento XML e atribuída ao objeto de script BHOLD. O objeto combina este documento com uma função pré-definida. A função pré-definida é um documento XSL que traduz o documento de entrada do script num documento de comando BHOLD.

![Processamento de scripts BHOLD](media/mim2016-bhold-developer-reference/image001.png)

Os comandos são executados da mesma ordem que no documento. Se uma função falhar, todos os comandos executados são revirados.

## <a name="script-object"></a>Objeto de script
Esta secção descreve como usar o objeto do script.

### <a name="retrieve-bhold-information"></a>Recuperar informações do BHOLD
A função **GetInfo** é utilizada para obter informações a partir dos dados disponíveis no sistema de autorização BHOLD. A função requer um nome de função e, eventualmente, um ou mais parâmetros. Se esta função for bem sucedida, um objeto BHOLD ou uma recolha é devolvido sob a forma de um documento XML.

Se a função não for bem sucedida, a função GetInfo devolve uma corda vazia ou um erro. A descrição e o número de erros podem ser usados para obter mais informações sobre a falha.

A função GetInfo 'FunctionDispatch' pode ser utilizada para obter informações de uma aplicação controlada pelo sistema BHOLD. Esta função requer três parâmetros: O ID da aplicação, a função de despacho tal como está definido no ASI, e um documento XML com informação de suporte para o ASI. Se a função tiver sucesso, o resultado está disponível no formato XML no objeto de resultado.

O snippet abaixo é um exemplo simples de C# de GetInfo:

```C#
ScriptProcessor myScriptProcessor = new ScriptProcessor();
myScriptProcessor.Initializae("CORP\\b1user");
myScriptProcessor.GetInfo("OrgUnit", "1");
```

Da mesma forma, o objeto **BScript** também pode ser acedido através do serviço `b1scriptservice` web. Isto é feito adicionando uma referência web ao seu projeto usando http:// <server> :5151/BHOLD/Core/b1scriptservice.asmx onde <server> está o servidor com os binários BHOLD instalados. Para obter mais informações, consulte [adicionar uma referência de serviço web a um projeto do Estúdio Visual.](https://msdn.microsoft.com/library/d9w023sx(v=vs.71).aspx)

O exemplo a seguir mostra como utilizar a função **GetInfo** a partir de um serviço web. Este código recupera a Unidade Organizacional que tem um OrgID de 1 e, em seguida, exibe o nome dessa Unidade Organizacional no ecrã.

```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Xml;

namespace bhold_console
{
    class Program
    {
        static void Main(string[] args)
        {
             var bholdService = new BHOLDCORE.B1ScriptService();
             bholdService.Url = "http://app1.corp.contoso.com:5151/BHOLD/Core/b1scriptservice.asmx";
             string orgname= "";

             if (args.Length == 3)
             {
                 //Use explicit credentials from command line
                 bholdService.UseDefaultCredentials = false;
                 bholdService.Credentials = new System.Net.NetworkCredential(args[0], args[1], args[2]);
                 bholdService.PreAuthenticate = true;
             }
             else
             {
                 bholdService.UseDefaultCredentials = true;
                 bholdService.PreAuthenticate = true;
             }

             //Load BHOLD information into an xml document and loop through document to find the bholdDescription value
             var myOrgUnit = new System.Xml.XmlDocument();
             myOrgUnit.LoadXml(bholdService.GetInfo("OrgUnit","1","","");

            XmlNodeList myList = myOrgUnit.SelectNodes(("//item");

            foreach (XmlNode myNode in myList)
            {
                for (int i = 0; i < myNode.ChildNodes.Count; i++)
                {
                    if (myNode.ChildNodes[i].InnerText.ToString() == "bholdDescription")
                    {
                        orgname = myNode.ChildNodes[i + 1].InnerText.ToString();
                    }
                }
            }

            System.Console.WriteLine("The Organizational Unit Name is: " + orgname);

        }
    }
}
```

O exemplo VBScript a seguir utiliza o serviço web via SOAP e utiliza o GetInfo. Para exemplos adicionais para SOAP 1.1, SOAP 1.2 e HTTP POST, consulte a secção de Referência Gerida BHOLD ou pode navegar para o serviço web diretamente a partir de um navegador e vê-los lá.

```VB
Dim SOAPRequest
Dim SOAPParameters
Dim SOAPResponse
Dim xmlhttp

Set xmlhttp = CreateObject("Microsoft.XMLHTTP")

xmlhttp.open "POST", "http://app1.corp.contoso.com:5151/BHOLD/Core/b1scriptservice.asmx", False, "CORP\Administrator", "abc123*2k"

xmlhttp.setRequestHeader "Content-type", "text/xml; charset=utf-8"
xmlhttp.setRequestHeader "SOAPAction", "http://B1/B1ScriptService/GetInfo"

SOAPRequest = "<?xml version='1.0' encoding='utf-8'?> <soap:Envelope" & vbCRLF
SOAPRequest = SOAPRequest & " xmlns:xsi=""http://" & vbCRLF
SOAPRequest = SOAPRequest & " www.w3.org/2001/XMLSchema-instance""" & vbCRLF
SOAPRequest = SOAPRequest & " xmlns:xsd=""http://www.w3.org/2001/XMLSchema""" & vbCRLF
SOAPRequest = SOAPRequest & " xmlns:soap=""http://schemas.xmlsoap.org/soap/envelope/"">" & vbCRLF
SOAPRequest = SOAPRequest & " <soap:Body>" & vbCRLF
SOAPRequest = SOAPRequest & " <GetInfo xmlns=""http://B1/B1ScriptService"">" & vbCRLF
SOAPRequest = SOAPRequest & " <functionName>OrgUnit</functionName>" & vbCRLF
SOAPRequest = SOAPRequest & " <parameter1>1</parameter1>" & vbCRLF
SOAPRequest = SOAPRequest & " <parameter2></parameter2>" & vbCRLF
SOAPRequest = SOAPRequest & " <parameter3></parameter3>" & vbCRLF
SOAPRequest = SOAPRequest & " </GetInfo>" & vbCRLF
SOAPRequest = SOAPRequest & " </soap:Body>" & vbCRLF
SOAPRequest = SOAPRequest & " </soap:Envelope>"
MsgBox SOAPRequest

xmlhttp.send SOAPRequest 

SOAPResponse = xmlhttp.responseText

MsgBox SOAPResponse
```

## <a name="execute-scripts"></a>Executar scripts

A função **ExecuteScript** do objeto **BScript** pode ser usada para executar scripts. Esta função requer dois parâmetros. O primeiro parâmetro é o documento XML que contém as informações personalizadas a serem usadas pelo script. O segundo parâmetro é o nome do script predefinido a ser usado. In In the BHOLD predefined scripts directy, aqui deve haver um documento XSL com o mesmo nome que a função, mas com a extensão .xsl.

Se a função não tiver sucesso, a função ExecuteScript devolve o valor Falso. A descrição do erro e o número podem ser usados para saber o que correu mal. Segue-se um exemplo de utilização do método web ExecuteXML. Este método invoca o ExecuteScript.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Sample
{
    class Program
    {
        static void Main(string[] args)
        {
            var bholdService = new BHOLDCORE.B1ScriptService();
            bholdService.Url = "http://app1.corp.contoso.com:5151/BHOLD/Core/b1scriptservice.asmx";

            if (args.Length == 3)
            {
                //Use explicit credentials from command line
                bholdService.UseDefaultCredentials = false;
                bholdService.Credentials = new System.Net.NetworkCredential(args[0], args[1], args[2]);
                bholdService.PreAuthenticate = true;
            }
            else
            {
                bholdService.UseDefaultCredentials = true;
                bholdService.PreAuthenticate = true;
            }
            System.Console.WriteLine( "Add user #3 to role #44, result: {0}", bholdService.ExecuteXml(roleAddUser("44", "3")) );
            System.Console.WriteLine("Add user D1 to role 'MR-OU2 No Users', result: {0}", bholdService.ExecuteXml(roleAddUser("MR-OU2 No Users", "D1")));

        }

        private static System.Xml.XmlNode roleAddUser(string roleId, string userId)
        {
            var script = new System.Xml.XmlDocument();
            script.LoadXml(string.Format("<functions>"
                                        +"  <function name='roleadduser' roleid='{0}' userid='{1}' />"
                                        +"</functions>",
                                        roleId,
                                        userId)
                           );
            return script.DocumentElement;
```

## <a name="bholdscriptresult"></a>BholdScriptResult

Esta função GetInfo está disponível após a execução da **função executor.** A função devolve uma cadeia formatada XML que contém o relatório completo de execução. O nó script contém a estrutura XML do script executado.

Para cada função que falha durante a execução do script, uma função de nó é adicionada com o nome dos nós. ExecuteXML e Error é adicionado ao final do documento todos os IDs gerados são adicionados.

Note que apenas as funções, que contêm um erro, são adicionadas. Um número de erro de '0' significa que a função não é executada. 

```
<Bhold>
  <Script>
    <functions>
      <function name="OrgUnitadd" description="OrgUnit1" parentid="root" orgtypeid="root" return="@ID@"/>     
      <function name="roleaddOrgUnit" OrgUnitid="OrgUnit1" roleid="Role1_OrgUnit1" return="@ID@"/>      
    </functions>
  </Script>
  <Function>
    <Name>OrgUnitadd</Name>
    <ExecutedXML>
      <function name="OrgUnitadd" description="OrgUnit1" parentid="root" orgtypeid="root" return="@ID@"/>
    </ExecutedXML>
    <Error Number="5" Description="Violation of UNIQUE KEY constraint 'IX_OrgUnits'. Cannot insert duplicate key in object 'dbo.OrgUnits'.
The statement has been terminated."/>
  </Function>
  <Function>
    <Name>roleaddOrgUnit</Name>
    <ExecutedXML>
      <function name="roleaddOrgUnit" OrgUnitid="OrgUnit1" roleid="Role1_OrgUnit1" return="@ID@"/>
    </ExecutedXML>
    <Error Number="0" Description=""/>
  </Function>
  <IDS>
    <ID name="@ID@">35</ID>
  </IDS>
</Bhold>
```

## <a name="id-parameters"></a>Parâmetros de ID
Os parâmetros de identificação recebem tratamento especial. Os valores não numéricos são utilizados como valor de pesquisa para localizar as entidades correspondentes na loja de dados BHOLD. Quando o valor de pesquisa não é único, a primeira entidade que cumpre o valor de pesquisa é devolvida.

Para distinguir os valores de pesquisa numérica dos IDs, é possível utilizar um prefixo. Quando os primeiros seis caracteres do valor de pesquisa são iguais a 'no_id:' então estes caracteres são despojados antes do valor ser usado para pesquisa. Podem ser utilizados caracteres wildcard '%' SQL.

Os seguintes campos são utilizados com o valor de pesquisa:

| Tipo de ID   | Campo de pesquisa |
|---|---|
| OrgUnitID | Descrição  |
| roleID    | Descrição  |
| taskID    | Descrição  |
| userID    | DefaultAlias |

## <a name="script-access-and-permissions"></a>Acesso ao script e permissões
O código do lado do servidor nas páginas do servidor ativo é utilizado para executar os scripts. Portanto, o acesso ao script significa acesso a estas páginas. O sistema BHOLD mantém informações sobre os pontos de entrada das páginas personalizadas. Esta informação inclui a página inicial e a descrição da função (vários idiomas devem ser suportados).

Um utilizador está autorizado a entrar nas páginas personalizadas e executar um script. Cada ponto de entrada é apresentado como uma tarefa. Cada utilizador que obteve esta tarefa através de uma função ou de uma unidade é capaz de executar a função correspondente.

Uma nova função no menu apresenta todas as funções personalizadas que podem ser executadas pelo utilizador. Porque um script pode executar ações no sistema BHOLD sob uma identidade diferente do utilizador iniciado. É possível dar permissão para realizar uma ação específica sem ter supervisão sobre qualquer objeto. Por exemplo, isto poderia ser útil para um empregado que só é permitido entrar novos clientes na empresa. Estes scripts também podem ser usados para criar páginas de auto-registo.

## <a name="command-script"></a>Script de comando
O script de comando contém uma lista de funções que são executadas pelo sistema BHOLD. A lista está escrita num documento XML que está em conformidade com as seguintes definições:


|   Script de comando   |              `<functions>functions</functions>`               |
|--------------------|---------------------------------------------------------------|
|     funções      |                      função {função}                      |
|      função      | função <nome="funçãoName" Funcionamentos [devolução] (/> \| parâmetro > Listo </função>) |
|    funçãoName    | Um nome de função válido, conforme descrito nas seguintes secções. |
| funçãoParametros |                     { funçãoParameter }                     |
| funÇãoParametro  |               parâmetroName = "parâmetroValue"                |
|   nome de parâmetroName    |                    Um nome de parâmetro válido.                    |
|   parâmetrosValue   |                      @variableValor \|                      |
|       valor        |                   Um valor de parâmetro válido.                    |
|   lista de parâmetros    |          <parameters> {parâmetrosItem}  </parameters>          |
|   parâmetroItem    | <parameter name="parameterName"> parâmetrosValue </parameter>  |
|       regressar       |                      return=" @variable @"                      |
|      variável      |                    Um nome variável personalizado.                    |

A XML tem as seguintes traduções de caracteres especiais:

| XML | Caráter |
|---|---|
| ```&amp;```  | & |
| ```&lt;```   | < |
| ```&gt;```   | > |
| ```&quot;``` | " |
| ```&apos;``` | ' |

Estes caracteres XML podem ser usados em identificadores, mas não são recomendados.

O seguinte código mostra um exemplo de um documento de comando válido com três funções:

```
<functions>

   <functionname="OrgUnitAdd" parentID="34" description="Acme Inc." orgtypeID="5" return="@UnitID@" />

   <function name="UserAdd" description="John Doe" alias="jdoe" languageID="1" OrgUnitID="@UnitID@" />

   <function name="TaskAddFile" taskID="93" path="/customers/purchase">
      <parameters>
         <parameter name="history"> True</parameter>
      </parameters>
   </function>

</functions>
```

A função **OrgUnitAdd** armazena o ID da unidade criada numa variável chamada UnitID. Esta variável é utilizada como entrada para a função UserAdd. O valor de retorno desta função não é utilizado. As secções seguintes descrevem todas as funções disponíveis, os parâmetros necessários e os seus valores de devolução.

## <a name="execute-functions"></a>Executar funções
Esta secção descreve como utilizar as funções de execução.

### <a name="abaattributeruleadd"></a>ABAAttributeRuleAdd
Crie uma nova regra de atributos num tipo de atributo específico. As regras de atributos só podem ser ligadas a um tipo de atributo.

A regra de atributos especificado pode estar ligada a todos os tipos de atributos possíveis. 

A regraType não pode ser alterada com o comando "ABAattributeruletypeupdate". Requer que a descrição do atributo seja única.


|    Argumentos    |                                                                                                                                                                                                                  Tipo                                                                                                                                                                                                                  |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   Descrição   |                                                                                                                                                                                                                  Texto                                                                                                                                                                                                                  |
|    RuleType     | Especifique o tipo de regra de atributo. Dependendo do tipo de regra de atributo, outros argumentos devem ser incluídos. Os seguintes valores do tipo regra são válidos:<ul><li>0: Expressão regular (adicionar argumento "valor").</li><li>1: Valor (adicionar argumentos "operador" e "valor").</li><li>2: Lista de valores.</li><li>3: Intervalo (adicionar argumentos "rangemin" e "rangemax").</li><li>4: Idade (adicionar argumentos "operador" e "valor").</li></ul> |
|  InvertResult   | ```["0"|"1"|"N"|"Y"]``` |
| AtributoTypeID |                                                                                                                                                                                                                  Texto                                                                                                                                                                                                                  |

| Argumentos opcionais | Tipo |
|---|---|
| Operador | Texto <br>**Nota:** Este argumento é obrigatório se **a RegraType** for 1 ou 4. Os valores possíveis são '=', '<', ou '>'. As tags XML precisam de usar " &gt; " para '>' e &lt; " para '<'. |
| RangeMin | Número <br>**Nota:** Este argumento é obrigatório se **a RegraType** for 3. |
| RangeMax | Número <br>**Nota:** Este argumento é obrigatório se **a RegraType** for 3. |
| Valor    | Texto <br>**Nota:** Este argumento é obrigatório se **a RegraType** for 0, 1 ou 4. O argumento deve ser um valor numérico ou alfanumérico. |
| Atributo de retornoRuleID | Texto   |


### <a name="applicationadd"></a>applicationadd
Cria uma nova aplicação, devolve o ID da nova aplicação.

| Argumentos | Tipo |
|---|---|
| descrição |      |
| máquina     |      |
| módulo      |      |
| parameter   |      |
| protocolo    |      |
| nome de utilizador    |      |
| palavra-passe    |      |
| svroleID (opcional)                | Se este argumento não estiver presente, é utilizado um papel de supervisor do utilizador atual. |
| Aplicaçãoaliasformula (opcional) | A fórmula de pseudónimo é usada para criar um pseudónimo para um utilizador quando é atribuído a uma permissão da aplicação. O pseudónimo é criado se o utilizador ainda não tiver um pseudónimo para esta aplicação. Se não for dado qualquer valor, o pseudónimo padrão do utilizador é utilizado como pseudónimo para a aplicação. A fórmula é formatada como ` [<<objecttype>>.<<nameofobjecttypeattribute>>(startindexoffset,length offset)]` . A compensação é opcional. Apenas os atributos de Utilizador e Aplicação poderiam ser utilizados. Pode ser utilizado um texto gratuito. Os caracteres reservados são suporte quadrado esquerdo ([) e suporte quadrado direito (]). Por exemplo: `[Application.bholdDescription]\[User.bholdDefAlias(1,5)]`. |
| Tipo de retorno | Identificação da nova aplicação. |

### <a name="attributesetvalue"></a>AtributoSetValue
Define o valor de um tipo de atributo ligado ao tipo de objeto. Requer que as descrições do tipo de objeto e do tipo de atributo são únicas.

| Argumentos | Tipo |
|---|---|
| ObjectTypeID     | Texto |
| ObjectID         | Texto |
| AtributoTypeID  | Texto |
| Valor            | Texto |
| Tipo de retorno      | Tipo |

### <a name="attributetypeadd"></a>AtributoTypeAdd
Insere um novo tipo de atributo/tipo de propriedade.


|        Argumentos        |                                               Tipo                                                |
|-------------------------|---------------------------------------------------------------------------------------------------|
|       DataTypeID        |                                               Texto                                                |
| Descrição (=Identidade) | Texto <br>**Nota:** Não podem ser utilizadas palavras reservadas, incluindo "a", "frm", "id", "usr" e "bhold". |
|        MaxLength        |                                       Número em [1,..,255]                                        |
| ListOfValues (booleano)  |                                          ```["0"|"1"|"N"|"Y"]```                               |
|      PadrãoValue       |                                               Texto                                                |
|       Tipo de retorno       |                                               Tipo                                                |
|     AtributoTypeID     |                                               Texto                                                |

### <a name="attributetypesetadd"></a>AtributoTypeSetAdd
Insere um novo conjunto de tipo de atributo. Requer que a descrição de um conjunto de tipo de atributo seja única.

| Argumentos | Tipo |
|---|---|
| Descrição (=Identidade) |Texto |
| Tipo de retorno | Tipo |
| AtributoTypeSetID | Texto |

### <a name="attributetypesetaddattributetype"></a>AtributoTypeSetAddAttributeType
Insere um novo tipo de atributo num conjunto de tipo de atributo existente. Requer que as descrições do tipo de atributo e do tipo de atributo sejam únicas.


|     Argumentos      |                              Tipo                              |
|--------------------|----------------------------------------------------------------|
| AtributoTypeSetID |                              Texto                              |
|  AtributoTypeID   |                              Texto                              |
|       Encomenda        |                             Número                             |
|     LocalizaçãoID     | Texto <br>**Nota:** A localização é "grupo" ou "solteiro". |
|     Obrigatório      |                    ```["0"|"1"|"N"|"Y"]```                  |
|    Tipo de retorno     |                              Tipo                              |

### <a name="objecttypeaddattributetypeset"></a>ObjectTypeAddAttributeTypeSet
Adiciona um conjunto de tipo de atributo a um tipo de objeto. Requer que a descrição do tipo de objeto e o conjunto do tipo de atributos sejam únicos. Os tipos de objetos são: Sistema, OrgUnit, Utilizador, Tarefa.

| Argumentos | Tipo |
|---|---|
| ObjectTypeID | Texto | 
| AtributoTypeSetID | Texto | 
| Encomenda | Número | 
| Visible | <ul><li>0: O conjunto do tipo de atributo é visível.</li><li>2: O conjunto de tipo de atributo é visível quando o botão **mais informação** é selecionado.</li><li>1: O conjunto do tipo de atributo é invisível.</li></ul> | 
| Tipo de retorno | Tipo | 

### <a name="orgunitadd"></a>OrgUnitadd
Cria uma nova unidade organizacional, devolve a identificação da nova unidade organizacional.

| Argumentos | Tipo |
|---|---|
| descrição | | 
| orgtypeID | | 
| parentID | | 
| OrgUnitinheritedroles (opcional) | | 
| Tipo de retorno | Tipo | 
| ID da nova unidade | O parâmetro OrgUnitinheritedroles <br>tem o valor sim ou não. | 

### <a name="orgunitaddsupervisor"></a>OrgUnitaddsupervisor
Faça de um utilizador um supervisor de uma unidade organizacional.

| Argumentos | Tipo |
|---|---|
| svroleID | O utilizador de argumentoID também pode ser usado. Neste caso, a função de supervisor por defeito é selecionada. Um papel de supervisor predefinido tem um nome como **__svrole** seguido por um número. O utilizador de argumentoID pode ser utilizado para retrocompatibilidade. | 
| OrgUnitID | | 

### <a name="orgunitadduser"></a>OrgUnitadduser
Faça de um utilizador um membro de uma unidade organizacional.

| Argumentos | Tipo |
|---|---|
| userID | | 
| OrgUnitID | | 

### <a name="orgunitdelete"></a>OrgUnitdelete
Remove uma unidade organizacional.

| Argumentos | Tipo |
|---|---|
| OrgUnitID | | 

### <a name="orgunitdeleteuser"></a>OrgUnitdeleteuser
Remove um utilizador como membro de uma unidade organizacional.

| Argumentos | Tipo |
|---|---|
| userID | | 
| OrgUnitID | | 

### <a name="roleadd"></a>roleadd
Cria um novo papel.

| Argumentos | Tipo |
|---|---|
| Descrição | | 
| svrole | | 
| svroleID (opcional) | Se este argumento não estiver presente, será utilizado um papel de supervisor do utilizador atual. | 
| ContextoSAdaptável (opcional) | ```["0","1","N","Y"]``` | 
| MaxPermissions (opcional) | Número inteiro | 
| MaxRoles (opcional) | Número inteiro | 
| MaxUsers (opcional) | Número inteiro | 
| Tipo de retorno | Tipo | 
| ID do novo papel | | 

### <a name="roleaddorgunit"></a>roleaddOrgUnit
Atribui um papel a uma unidade organizacional.


|    Argumentos    |                                      Tipo                                      |
|-----------------|--------------------------------------------------------------------------------|
|    OrgUnitID    |                                     roleID                                     |
| herdarThisRole | "verdadeiro" ou "falso", indica se o papel é proposto às unidades subjacentes. |

### <a name="roleaddrole"></a>roleaddrole
Atribui um papel como subrole de outro papel.

| Argumentos | Tipo |
|---|---|
| roleID | | 
| subRoleID | | 

### <a name="roleaddsupervisor"></a>roleaddsupervisor
Faça de um utilizador um supervisor de um papel.

| Argumentos | Tipo |
|---|---|
| svroleID | O utilizador de argumentoID também pode ser usado. Neste caso, a função de supervisor por defeito é selecionada. Um papel de supervisor predefinido tem um nome como **__svrole** seguido por um número. O utilizador de argumentoID pode ser utilizado para retrocompatibilidade. | 
| roleID | | 

### <a name="roleadduser"></a>roleadduser
Atribui uma função a um utilizador. O papel não pode ser um papel adaptável de contexto quando não é dado nenhum contextoID.

| Argumentos | Tipo |
|---|---|
| userID | | 
| roleID | | 
| duraçãoType (opcional) | Pode conter os valores 'grátis', 'horas' e 'dias'. | 
| duração Comprimento (opcional) | Obrigatório quando a duração OType é 'horas' ou 'dias'. deve conter o valor inteiro para o número de horas ou dias que a função é atribuída a um utilizador. | 
| início (opcional) | Data e hora quando a função é atribuída. Quando este atributo é omitido, o papel é atribuído imediatamente. O formato de data é 'YYYYY-MM-DDThh:nn:ss', onde apenas são necessários ano, mês e dia. por exemplo , "2004-12-11" e "2004-11-28T08:00" são valores válidos. | 
| fim (opcional) | Data e hora quando o papel for revogado. Quando a duraçãoType e duração O comprimento de comprimento é dado, este valor é ignorado. O formato de data é 'YYYYY-MM-DDThh:nn:ss', onde apenas são necessários ano, mês e dia. por exemplo , "2004-12-11" e "2004-11-28T08:00" são valores válidos. | 
| linkreason | Requerido quando começa, termina ou a duração é dada, caso contrário é ignorado. | 
| contextid (opcional) | ID da unidade organizacional, apenas necessária para funções adaptáveis de contexto. | 

### <a name="roledelete"></a>roledelete
Elimina um papel.

| Argumentos | Tipo |
|---|---|
| roleID | | 

### <a name="roledeleteuser"></a>roledeleteuser
Remove a atribuição de funções a um utilizador. As funções herdadas pelo utilizador são revogadas por este comando.

| Argumentos | Tipo |
|---|---|
| userID | | 
| roleID | | 
| contextID (opcional) |  | 

### <a name="roleproposeorgunit"></a>roleproposeOrgUnit
Propõe um papel para atribuí-lo aos membros e às sub-OrgUnits de uma OrgUnit.

| Argumentos | Tipo |
|---|---|
| OrgUnitID | | 
| roleID | | 
| duraçãoType (opcional) | Pode conter valores 'livres', 'horas' e 'dias'. | 
| duraçãoLength | Requerido quando a duração OType for 'horas' ou 'dias', deve conter o valor inteiro para o número de horas ou dias que a função é atribuída a um utilizador. | 
| duraçãoFixo | "verdadeiro" ou "falso", indica se a atribuição desta função a um utilizador deve ser igual à duração. | 
| herdarThisRole | "verdadeiro" ou "falso", indica se o papel é proposto às unidades subjacentes. | 

### <a name="taskadd"></a>taskadd
Cria uma nova tarefa, devolve o ID da nova tarefa.

| Argumentos | Tipo |
|---|---|
| aplicaçãoID | | 
| descrição | Texto com um máximo de 254 caracteres. | 
| nome de tarefa | Texto com um máximo de 254 caracteres. | 
| tokenGroupID | | 
| svroleID (opcional) | Se este argumento não estiver presente, será utilizado um papel de supervisor do utilizador atual. | 
| contextoDizável (opcional) | ```["0","1","N","Y"]``` | 
| subconstrução (opcional) | ```["0","1","N","Y"]``` | 
| reação de auditoria (opcional) | <ul><li>0: Desconhecido (padrão)</li><li>1: Relatório Apenas</li><li>2: AlertaAppAll</li><li>3: AlertaAppObsolete</li><li>4: AlertaAppMissing</li><li>5: EnforceAppAll</li><li>6: EnforceAppObsolete</li><li>7: EnforceAppMissing</li><li>8: AlertEnforceAppAll</li><li>9: AlertEnforceAppObsolete</li><li>10: AlertEnforceAppMissing</li><li>11: ImportAll</li></ul> | 
| auditalertmail (opcional) | O endereço de e-mail para os alertas sobre esta permissão é enviado pelo auditor. Se este argumento não estiver presente, o endereço de e-mail de alerta do auditor é utilizado. | 
| MaxRoles (opcional) | Número inteiro | 
| MaxUsers (opcional) | Número inteiro | 
| Tipo de retorno | Identificação da nova tarefa. | 

### <a name="taskadditask"></a>taskadditask
Indique que duas tarefas são incompatíveis.

| Argumentos | Tipo |
|---|---|
| taskID | | 
| taskID2 | | 

### <a name="taskaddrole"></a>taskaddrole
Atribui uma tarefa a um papel.

| Argumentos | Tipo |
|---|---|
| roleID | | 
| taskID | | 

### <a name="taskaddsupervisor"></a>taskaddsupervisor
Faça de um utilizador um supervisor de uma tarefa.

| Argumentos | Tipo |
|---|---|
| svroleID | O utilizador de argumentoID também pode ser usado. Neste caso, a função de supervisor por defeito é selecionada. Um papel de supervisor predefinido tem um nome como **__svrole** seguido por um número. O utilizador de argumentoID pode ser utilizado para retrocompatibilidade. | 
| taskID | | 

### <a name="useradd"></a>sapo de utilizador
Cria um novo utilizador, devolve o ID do novo utilizador.

| Argumentos | Tipo |
|---|---|
| descrição | | 
| alias | | 
| idiomaID | <ul><li>1: Inglês</li><li>2: Holandês</li></ul> | 
| OrgUnitID | | 
fim de curso (opcional) |O formato de data é 'YYYYY-MM-DDThh:nn:ss', onde apenas são necessários ano, mês e dia. por exemplo , "2004-12-11" e "2004-11-28T08:00" são valores válidos. | 
| incapacitado (opcional) | <ul><li>0: Ativado</li><li>1: Deficiente</li></ul> | 
| MaxPermissions (opcional) | Número inteiro | 
| MaxRoles (opcional) | Número inteiro | 
| Tipo de retorno | Identificação do novo utilizador. | 

### <a name="useraddrole"></a>UserAddRole
Adiciona uma função de utilizador.
<!--- missing content -->

| Argumentos | Tipo |
|---|---|
|  | | 

### <a name="userdeleterole"></a>UserDeleteRole 
Elimina uma função de utilizador.
<!--- missing content -->

| Argumentos | Tipo |
|---|---|
|  | | 

### <a name="userupdate"></a>Userupdate
Atualiza um utilizador.

| Argumentos | Tipo |
|---|---|
| UserID | | 
| descrição (opcional)|  | 
| language | <ul><li>1: Inglês</li><li>2: Holandês</li></ul> | 
| utilizadorDisabled (opcional) |<ul><li>0: Ativado</li><li>1: Deficiente</li></ul> | 
| UserEndDate (opcional) | O formato de data é 'YYYYY-MM-DDThh:nn:ss', onde apenas são necessários ano, mês e dia. por exemplo , "2004-12-11" e "2004-11-28T08:00" são valores válidos. | 
| primeiro Nome (opcional) | | 
| nome médio (opcional) | | 
| último nome (opcional) | | 
| maxPermissions (opcional) | Número inteiro |
| maxRoles (opcional) | Número inteiro | 


## <a name="getinfo-functions"></a>Funções GetInfo
O conjunto de funções descritas nesta secção pode ser utilizado para recuperar informações armazenadas no sistema BHOLD. Cada função pode ser chamada utilizando a função GetInfo a partir do objeto BScript. Alguns objetos requerem parâmetros. Os dados devolvidos estão sujeitos às permissões de visualização e aos objetos supervisionados do utilizador que iniciaram sessão.

### <a name="getinfo-arguments"></a>Argumentos GetInfo

| Nome | Descrição |
|---|---|
| aplicações | Devolve uma lista de candidaturas. | 
| atributos | Devolve uma lista de tipos de atributos. | 
| orgótipos | Devolve uma lista de tipos de unidades organizacionais. | 
| OrgUnits | Devolve uma lista de unidades organizacionais sem os atributos das unidades organizacionais. | 
| Orgunitproposedroles | Devolve uma lista de funções propostas ligadas à unidade organizacional. | 
| OrgUnitroles | Devolve uma lista de funções diretamente ligadas à unidade organizacional | 
| Objecttypetributypes | | 
| permissões | | 
| permissões | | 
| funções | Devolve uma lista de papéis. | 
| roletasks | Devolve uma lista de tarefas do papel dado. | 
| tarefas | Devolve todas as tarefas conhecidas pela BHOLD. | 
| utilizadores | Devolve uma lista de utilizadores. | 
| utilizadoresroles | Devolve a lista de funções de supervisor vinculada do utilizador dado. | 
| minimizações de utilizador | Devolve a lista de permissões do utilizador dado. | 

### <a name="orgunit-info"></a>Informações orgunit

| Nome | Parâmetros | Tipo de retorno |
|---|---|---|
| OrgUnit | OrgUnitID | OrgUnit | 
| Atribuição de orgunitasiat | OrgUnitID | Coleção | 
| OrgUnits| filtro (opcional), propótipo (opcional)<br> Procura por unidades que contenham a cadeia descrita no **filtro** no propótipo descrito em **proptype .** Se este ID for omitido, o filtro aplica-se à descrição da unidade. Se não for fornecido filtro, todas as unidades visíveis são devolvidas. | Coleção | 
| OrgUnitOrgUnits | OrgUnitID | Coleção | 
| OrgUnitparents | OrgUnitID | Coleção | 
| OrgUnitpropertyvalues | OrgUnitID | Coleção | 
| OrgUnitproptypes |  | Coleção | 
| OrgUnitusers | OrgUnitID | Coleção | 
| Orgunitproposedroles | OrgUnitID | Coleção | 
| OrgUnitroles | OrgUnitID | Coleção | 
| OrgUnitinheritedroles | OrgUnitID | Coleção | 
| OrgUnitsupervisors | OrgUnitID | Coleção | 
| OrgUnitinheritedsupervisors| OrgUnitID | Coleção | 
| OrgUnitsupervisorroles | OrgUnitID | Coleção | 

### <a name="role-information"></a>Informação sobre funções

| Nome | Parâmetros | Tipo de retorno |
|---|---|---|
| papel | roleID | Objeto | 
| funções | filtro (opcional) | Coleção | 
| roleasiattributes | roleID | Coleção | 
| roleOrgUnits | roleID | Coleção | 
| roleparentroles | roleID | Coleção | 
| rolesubroles | roleID | Coleção | 
| rolesupervisores | roleID| Coleção | 
| rolesupervisorroles | roleID | Coleção | 
| roletasks | roleID | Coleção | 
| roleusers | roleID | Coleção | 
| rolesupervisorroles | roleID | Coleção | 
| propostasdrogUnits | roleID | Coleção | 
| propunhadroleusers | roleID | Coleção | 

### <a name="permission---task-information"></a>Permissão - Informações de tarefa

| Nome | Parâmetros | Tipo de retorno |
|---|---|---|
| permissão | TaskID | Permissão | 
| permissões | filtro (opcional) | Coleção | 
| permissõestribue | TaskID | Coleção | 
| permissões | TaskID | Coleção | 
| permissõestribuidores | - | Coleção | 
| permissões de permissão | TaskID | Coleção | 
| permissões de permissões | TaskID | Coleção | 
| permissões de supervisão | TaskID | Coleção | 
| permissõesupervisorroles | TaskID | Coleção | 
| permissões | TaskID | Coleção | 
| tarefa | TaskID | Tarefa | 
| tarefas| filtro (opcional) | Coleção | 
| taskattachments | TaskID | Coleção | 
| taskparams | TaskID | Coleção | 
| taskroles | TaskID | Coleção | 
| tasksupervisors | TaskID | Coleção | 
| tasksupervisorroles | TaskID | Coleção | 
| taskusers | TaskID | Coleção | 

### <a name="user-information"></a>Informações do utilizador

| Nome | Parâmetros | Tipo de retorno |
|---|---|---|
| utilizador | UserID | Utilizador | 
| utilizadores | filtro (opcional), atributo (opcional) <br>Pesquisas por utilizadores que contenham no atributo especificado por atributo a cadeia especificada por filtro. Se este ID for omitido, o filtro aplica-se ao pseudónimo do utilizador por defeito. Se não for fornecido nenhum filtro, todos os utilizadores visíveis são devolvidos. Por exemplo:<ul><li>`GetInfo("users")` devolve todos os utilizadores.</li><li>`GetInfo("users", "%dmin%")` devolve todos os utilizadores com a cadeia "dmin" no pseudónimo padrão.<br></li><li>Suponha que os utilizadores tenham um atributo extra chamado `"City".GetInfo("users", "%msterda%", "City")` . Esta chamada devolve a todos os utilizadores que tenham a cadeia "msterda" no atributo City.</li></ul> | UserCollection | 
| aplicações de utilizadores | UserID | Coleção | 
| Minimizações de utilizador | UserID | Coleção | 
| userroles | UserID | Coleção | 
| utilizadoresroles | UserID | Coleção | 
| utilizadorestas | UserID | Coleção | 
| unidades de utilizadores | UserID | Coleção | 
| usertasks | UserID | Coleção | 
| unidades de utilizador | UserID | Coleção | 

### <a name="return-types"></a>Tipos de retorno
Nesta secção, são descritos os tipos de devolução da função GetInfo.

| Nome | Tipo de retorno |
|---|---|
| Coleção |```=<ITEMS>{<ITEM description="..." id="..." />}</ITEMS>``` | 
| Objeto |```=<ITEM type="…" description="..." />``` | 
| OrgUnit |```= <ITEM id="…" description="..." orgtype="..." parent="..."> <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 
| Permissão |```= <ITEM id="…" description="…" name="…" tokengroup="…" application="…" > <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 
| Funções |```= <ITEMS> {<ITEM id="…" description="…" />} </ITEMS>``` | 
| Função |```= <ITEM id="…" description="… " > <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 
| Tarefa | Ver Permissão | 
| Utilizadores | ```= <ITEMS> {<ITEM description="…" id="…" alias="…" />} </ITEMS>``` | 
| Utilizador |```= <ITEM id="…" description="…" alias="…" firstname="…" lastname="…" uuid="…" language="…"> <LIST> {<ITEM> <KEY>… </KEY> <VALUE> … </VALUE> </ITEM>} </LIST> </ITEM>``` | 

## <a name="script-sample"></a>Amostra de script

Uma empresa tem um servidor BHOLD e quer um script automatizado que cria novos clientes. A informação sobre a empresa e o seu gestor de compras entra numa página web personalizada. Cada cliente é apresentado no modelo como uma unidade sob os clientes da unidade. O gestor de compras é tão bem como um membro como supervisor desta unidade. É criada uma função que dá aos proprietários o direito de comprar em nome do novo cliente.

No entanto, este cliente não existe na aplicação. Existe uma função especial implementada no asi FunctionDispatch que cria uma nova conta de cliente no pedido de compra. Cada cliente tem um tipo de cliente.

Os tipos possíveis também podem ser apresentados pela função FunctionDispatch. O AA escolhe o tipo correto para o novo cliente.

Criar um papel e tarefa para apresentar os privilégios de compra. O verdadeiro privilégio de compra é apresentado pelo ASI como um `/customers/customer id/purchase` ficheiro. Este ficheiro deve estar ligado à nova tarefa.

A página do servidor ativo que reúne a informação é assim:

```VB
<%@ Language=VBScript %>
<% Option Explicit %>
<html>
<body>
<form action="MySubmit.asp" method=post>
<input type="hidden" name="OrgUnitID" 
     value="<% = Request("ID") %>">
Company <input type="text" name="Description"> <br>
Type <select name="OrgType">
<%Dim oOrgType
For Each oOrgType on bscript.getinfo("Orgtypes") %>
<option value="<% = oOrgType.OrgTypeID %>">
<% = oOrgType.Description %>
</option> <%
Next %>
</select>  <br>
Manager <input type="text" name=" manager"> <br>
Alias <input type=" text" name=" alias"> <br>
e-mail <input type=" text" name=" email"> <br>
<input type="submit">
</form>
</body>
</html>
```

Todas as páginas personalizadas teriam de fazer é solicitar a informação certa e criar um documento XML com as informações solicitadas. Neste exemplo, a página MySubmit transforma os dados no documento XML, atribuindo-os ao **b1script. Parâmetros** objeto e finalmente chama a `b1script.ExecuteScript("MyScript")` função.

O seguinte script de entrada mostra este exemplo:

```
<customer>
<description>ACME inc.</description>
<orgtype>5<orgtype>
<name>John Doe</name>
<alias>jdoe</alias>
<email>jdoe@acme.com</email>
</customer>
```

Este script de entrada não contém quaisquer comandos para BHOLD. Isto porque este script não é executado diretamente pela BHOLD; em vez disso, esta é a entrada para uma função pré-definida. Esta função pré-definida traduz este objeto a um documento XML com comandos BHOLD. Este mecanismo impede o utilizador de enviar scripts para o sistema BHOLD que contenham funções que o utilizador não está autorizado a executar, tais como setUser e despachos de função para um ASI.

```
<?xml version="1.0" encoding="utf-8" ?> 
- <functions xmlns="http://tempuri.org/BscriptFunctions.xsd">

  <function name="roleadduser" roleid="" userid="" /> 
  <function name="roledeleteuser" roleid="" userid="" /> 
  </functions>
```

## <a name="next-steps"></a>Passos seguintes
- [Guia de conceitos do BHOLD](../bhold/bhold-concepts-guide.md)
- [Histórico de versões do BHOLD](version-bhold-history.md)
