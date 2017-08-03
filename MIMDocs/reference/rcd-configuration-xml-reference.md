---
title: "Referência XML de configuração de visualização de controlo de recursos | Microsoft Docs"
description: 
keywords: 
author: fimguy
ms.author: fimguy
manager: mbaldwin
ms.date: 05/1/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: c6ed843fb15b150fc934062945ab76ba32d72d33
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# <a name="resource-control-display-configuration-xml-reference"></a>Referência XML de configuração de visualização de controlo de recursos

Recursos de configuração (RCDC) de apresentação de controlo de recursos são recursos definidos pelo utilizador que pode utilizar para controlar a forma como os outros recursos no arquivo de dados do Microsoft Identity Manager 2016 SP1 (MIM) são apresentados na interface de utilizador (IU) para o utilizador final. Cada recurso RCDC contém um ficheiro de configuração XML que pode alterar para adicionar, modificar ou remover o texto da IU e controlos de IU. Enquanto SP1 de MIM 2016 fornece a predefinição de vários recursos RCDC, também pode criar recursos RCDC personalizados para recursos personalizados. Para obter mais informações sobre como utilizar a IU de RCDC no Portal do FIM, consulte [introdução ao configurar e personalizar o Portal do FIM](http://go.microsoft.com/fwlink/?LinkID=165848) na documentação do FIM.


## <a name="known-issues"></a>Problemas Conhecidos

Valor predefinido na vários controlos de configuração de visualização do controlo de recursos não é suportada

Nesta versão, os valores predefinidos de definição controlos num controlo de recursos de mensagens em fila não é suportada, exceto para o controlo de botão de opção. Pode contornar este problema para uma caixa de lista pendente, especificando um valor predefinido, que não está associado a qualquer valor para forçar o utilizador para alterar a seleção. Para contornar este problema com outros controlos, tem de utilizar um fluxo de trabalho de autorização para fornecer um valor predefinido durante a submissão do pedido.

## <a name="basic-structure"></a>Estrutura básica

Os dados XML para um recurso RCDC constituem o elemento XML único **ObjectControlConfiguration.**

>[!NOTE]
Para o esquema XSD completo, consulte apêndice a: predefinido XSD esquema, mais à frente neste documento.

Segue-se o esquema XSD para o elemento de ObjectControlConfiguration:

```XML
<xsd:element name="ObjectControlConfiguration"\>
  <xsd:complexType\>
    <xsd:sequence\>
      <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:Panel"/>
      <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
    </xsd:sequence>
    <xsd:attribute ref="my:TypeName"/>
    <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
  </xsd:complexType>
</xsd:element>
```



O **ObjectControlConfiguration** elemento contém o seguinte:

1.  **ObjectDataSource**: este elemento Especifica TypeName uma classe de origem de dados que utiliza o controlo de recursos (RC). Para uma descrição e a definição de esquema, consulte a secção de fontes de dados seguinte neste documento. Um **ObjectControlConfiguration** elemento pode conter até 32 nós do **ObjectDataSource** elemento.

2.  **XmlDataSource**: Esta é uma origem de dados simples que é normalmente utilizada para especificar a estrutura de uma página de resumo. Para uma descrição e a definição de esquema, consulte a secção de fontes de dados seguinte neste documento. Um **ObjectControlConfiguration**: elemento pode conter até 32 nós do **XmlDataSource** elemento.

3.  **Painel**: administrador pode personalizar o esquema da página RCDC através da modificação de elementos no interior os elementos do painel. Para obter mais informações, consulte a secção painel posteriormente neste documento. Um **ObjectControlConfiguration** elemento deve ter apenas um elemento do painel.

4.  **Eventos**: os administradores não é possível fornecer personalizado code-behind, esta funcionalidade é limitada. Este é o evento que pode emitir um painel ou um controlo, com base numa alteração de estado. Para obter mais informações, consulte a secção eventos posteriormente neste documento. Um **ObjectControlConfiguration** elemento pode conter opcionalmente um **eventos** elemento. Em geral, a utilização de personalizado **eventos** não é suportada a menos que especificamente desenvolvidos no melhoramentos posteriores.

## <a name="data-sources"></a>Origens de dados

Microsoft Identity Manager utiliza origens de dados como uma forma de enlace de dados para componentes de IU. Isto ajuda-o facilitar a separação de dados da camada de apresentação. Existem dois tipos de origens de dados nos dados de configuração do recurso RCDC: **ObjectDataSource** e **XmlDataSource**.

-   **ObjectDataSources** especifique uma classe de Microsoft .NET que fornece os dados para o RC. Não há um conjunto de tipos disponíveis de ObjectDataSources fixo desde que o administrador pode escolher consumir durante a criação de RCDCs.

-   **XMLDataSources** fornecem uma forma simples de estrutura de dados baseado em XML e poderem ser utilizados por administradores para fornecer dados personalizados. Os dados XML tem de ser especificados diretamente no RCDC, a menos que utilize a estrutura XML incorporada, predefinida. A estrutura XML incorporada é utilizada para gerar páginas resumidas de RC.

No RCDC, é possível vincular estas origens de dados para os atributos dos controlos de IU que estão especificados na RCDC para gerar a IU.

### <a name="objectdatasource"></a>ObjectDataSource

Microsoft Identity manager fornece os dados origem tipos comuns na tabela seguinte que estão disponíveis para todos os tipos de recursos (exceto indicação em contrário).

| TypeName                        | Descrição     | Suporta o enlace bidireccional | Suportado sintaxe de enlace        |
|----------------|-----------|-------------------------|--------------|
| PrimaryResourceObjectDataSource | Representa o recurso do FIM 2010 que está a ser criado, visualizado ou editado. O caminho na cadeia de enlace é o nome de atributo. Tenha em atenção que o tipo de recurso é especificado pelo atributo TargetObjectType do RCDC em vez de no RCDC. Atributo ConfigurationData. | Sim                     | [AttributeName] Valor do atributo de objeto especificado pelo respetivo nome.    |
| PrimaryResourceDeltaDataSource  | Esta origem de dados cria o delta XML que compara o estado original e o estado atual do recurso FIM 2010. O delta gerado XML é consumido pelo controlo resumo RC para compor a IU de pedido que o utilizador é submeter.                                    | Não                      | DeltaXml: </br> Isto é utilizado com o controlo de resumo para apresentar o delta.                                                 |
| PrimaryResourceRightsDataSource | Esta origem de dados fornece os direitos de inline para cada atributo do recurso FIM 2010. Isto permite que o RC determinar previamente submissão que permissões que esse utilizador tem esse atributo e, em seguida, adequadamente a compor da IU para esse atributo.                     | Não                      | [AttributeName]                                                                                         |
| SchemaDataSource                | Esta origem de dados pode ser utilizada para aceder a informações relacionadas com o esquema, tal como o nome a apresentar, descrição, se pretende ou não é necessário o atributo, bem como informações de tipo de recurso.                                                                                             | Não                      | [AttributeName]. **Necessário** valor booleano que indica se o atributo tem de ter um valor para ser válida. <br/> [AttributeName]. **DisplayNameString** valor que indica que nome o enlace a apresentar <br/>[AttributeName]. **DescriptionString** valor que indica que descrição o enlace <br/>[AttributeName]. Valor de StringRegexString que indica o Regex de cadeia de um enlace. <br/>[AttributeName]. **DisplayName** <br/> [AttributeName]. **Descrição** <br/> [AttributeName]. [AttributeName]. **IntergerValueMinimum** <br/>[AttributeName]. **IntergerValueMaximum** <br/>[AttributeName]. **LocalizedAllowedValues**|
| DomainDataSource                | Esta origem de dados fornece uma enumeração de domínios, com base nos recursos de configuração do domínio. Tenha em atenção que esta origem de dados pode ser utilizada apenas no RCDCs destinadas a do grupo de recursos e recursos de utilizador.                                                                           | Sim                     | Domínio           |

Segue-se um fragmento RCDC de exemplo que está vinculado a três origens de dados para o controlo de UocTextBox para editar o atributo de descrição de um grupo:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceObjectDataSource" my:Name="object" my:Parameters=""/>
<my:ObjectDataSource my:TypeName="SchemaDataSource" my:Name="schema"/>
<my:ObjectDataSource my:TypeName="PrimaryResourceRightsDataSource" my:Name="rights"/>

     <my:Control my:Name="Description" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Description.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Description}">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
               <my:Property my:Name="Rows" my:Value="3"/>
               <my:Property my:Name="Columns" my:Value="60"/>
               <my:Property my:Name="MaxLength" my:Value="450"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=Description, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
```

### <a name="xmldatasource"></a>XMLDataSource

Utilizando um **XMLDataSource**, pode especificar dados personalizados que pode consumir o RCDC para um determinado recurso. Neste caso, os dados XML tem de ser especificados no RCDC. Como alternativa, esta origem de dados pode ser utilizada para fazer referência a uma estrutura de dados XML incorporada para compor da IU para páginas de resumidas. Pode controlar o tipo de **XMLDataSource** a utilizar ao definir o RCDC.


| TypeName                 | Descrição   | | |
|--------------------------|------------|
| **XMLDataSource**            | A origem de dados representa dados XML. Pode ser XSL ou XSL incorporado formatos: <br/>**Formato XSL:** <br/> Microsoft.IdentityManagement.WebUI.Controls.dll < meu: XmlDataSource meu: nome = " <br/>summaryTransformXsl"my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl">< / meu: XmlDataSource ><br/> **Formato XSL incorporado:** <br/> < meu: XmlDataSource meu: nome = "RequestStatusTransformXsl" > <br/> < xsl: stylesheet versão = "1.0" xmlns:xsl = http://www.w3.org/1999/XSL/Transform <br/> xmlns:msxsl = "urn: schemas-microsoft-com:xslt" ><br/>< / xsl: stylesheet >< / meu: XmlDataSource >                       |Não | ```Xpath[;namespaces]``` <br/> Onde: Xpath é um xpath XML válido para selecionar a nota necessária, com mais frequência "/" (raiz) <br/>Espaços de nomes é uma lista opcional de prefixo = cadeias URI, delimitadas por ponto e vírgula, se necessário para o xpath funcionar com namespaced XML. |
| **ReferenceDeltaDataSource** | A origem de dados representa deltas de atributos de referência com múltiplos valores. É utilizado apenas no RCDC para o grupo e o conjunto. <br/> Apesar da origem de dados não está limitada a grupos ou conjuntos, necessita de alterações de código no anfitrião RCDC essas deltas de submeter. Atualmente, o grupo e o conjunto são apenas anfitriões que reconhecem a esta origem de dados.  | Sim                      | [AttributeName]. Adicione em que [AttributeName] representa um atributo de referência e os dados devolvidos são adições delta. <br/> Exemplo: [ReferenceAttribute]. Adicionar <br/>Exemplo: ```<my:Property my:Name="Value" my:Value="{Binding Source=delta, Path=ExplicitMember.Add, Mode=TwoWay}" />``` <br/>[AttributeName]. Remova em que [AttributeName] representa um atributo de referência e os dados devolvidos são a remoção de diferenças. <br/> DeltaXml |
|**RequestDetailsDataSource**| A origem de dados representa o atributo RequestParameter de objetos de pedido. O parâmetro define o número máximo de valores de atributo a apresentar por atributo com múltiplos valores que é utilizado apenas no RCDC para o pedido. ```<my:ObjectDataSource my:TypeName="RequestDetailsDataSource" my:Name="requestDetails" my:Parameters="1000" />```| Não | DeltaXml |
|**RequestStatusDataSource**| A origem de dados representa o **RequestStatusDetails** atributo dos objetos de pedido. <br/>É utilizado apenas no RCDC para o pedido.  | Não | DeltaXml |

-   Para definir uma origem de dados XML personalizada:

 ```XML
   <my:XmlDataSource my:Name="MyCustomData" >
   %Insert custom, properly formatted XML data here%
   </my:XmlDataSource>
   ```

-   Para utilizar o controlo de resumo incorporado XSL, defina a origem de dados da seguinte forma:

```XML
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />
```

 Se estiver a criar um RCDC para um tipo de recurso personalizado, pode utilizar este método para compor automaticamente uma página de resumo para esse recurso personalizado.

Segue-se um exemplo de como criar um separador Resumo no RCDC, com o PrimaryResourceDeltaDataSource XMLDataSource utilizando o XSL incorporada:

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceDeltaDataSource" my:Name="delta" />
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />

<my:Grouping my:Name="summaryGroup" my:Caption="Summary” my:IsSummary="true">
     <my:Control my:Name="summaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}" />
              <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}" />
          </my:Properties>
     </my:Control>
</my:Grouping>
```

Como alternativa, o utilizador pode substituir o elemento de XmlDataSource especificado anteriormente com o seguinte formato para definir um esquema de uma página de resumo personalizado. Como uma referência, a predefinição XSL de resumo do FIM 2010 é incluída no apêndice b: predefinição resumo XSL, mais tarde neste documento.

```XML
<my:XmlDataSource my:Name="summaryTransformXsl">
     Insert valid XSL code here
</my:XmlDataSource>
```
### <a name="schema-for-data-sources"></a>Esquema de origens de dados
Segue-se o esquema XSD para os dois tipos de origens de dados:

```XML
<xsd:element name="ObjectDataSource">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
     </xsd:complexType>
</xsd:element>
<xsd:element name="XmlDataSource">
     <xsd:complexType  mixed="true">
          <xsd:sequence>
              <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
    </xsd:complexType>
</xsd:element>
```

## <a name="events"></a>Eventos
Um evento define o estado de alteração de um controlo. A extensibilidade desta funcionalidade é limitada porque não é possível escrever uma função personalizada (processador) para definir o que é o comportamento após um evento é acionado. O mesmo elemento de eventos pode ser utilizado no elemento de painel. Para obter mais informações, consulte a secção painel posteriormente neste documento. Segue-se o esquema XSD para o elemento de evento:

```XML
<xsd:element name="Events">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Event">
     xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
     </xsd:element>
```


Um evento é um elemento vazio e tem dois atributos.

**Atributos:**

1.  **Nome**: Este é o nome exclusivo de um evento. A única suportada evento no **ObjectControlConfiguration** é o evento de carga. Este evento é acionado quando é primeiro carregar a página.

2.  **Processador**: Este é o nome exclusivo de um processador. Quando o evento é acionado, normalmente, um método de programa denomina-se ao processar a alteração do Estado do controlo. não são suportados os seguintes casos: remover um processador existente de um controlo existente, criar um processador e anexar a um controlo novo ou existente.

Exemplo:

Segue-se um exemplo de um elemento de eventos.
```XML
<my:Events>
    <my:Event my:Name="Load" my:Handler="OnLoad"/>
</my:Events>
```

**Painel**


O elemento de painel é o elemento principal um esquema de RCDC. Segue-se o esquema XSD para o elemento do painel:

```XML
<xsd:element name="Panel">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:DisplayAsWizard"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:AutoValidate"/>
     </xsd:complexType>
</xsd:element>
```

Este elemento contém um elemento recorrente, agrupamento. Para obter mais informações, consulte a secção de agrupamento neste documento.

O elemento de painel contém quatro atributos:

1.  **Nome**: O nome do painel. Este é um atributo necessário, de tipo de cadeia.

2.  **DisplayAsWizard**: este atributo atualmente foi preterido. O atributo de VerbContext correspondente no RCDC regulam se o esquema do recurso está no modo de assistente ou separador. Se estiver definido como 0 (modo de criação), é também no modo de assistente. Caso contrário, trata-se no modo de separador. Para obter mais informações, consulte Introdução ao configurar e personalizar o Portal do FIM na documentação.

3.  **Legenda**: este atributo atualmente foi preterido. O utilizador pode especificar legendas para uma página, incluindo um grupo que contém apenas informações de cabeçalho. Para obter mais informações, consulte a secção de agrupamento neste documento.

4.  **AutoValidate**: Este é um atributo booleano opcional. Quando estiver definido como validação for verdadeira, é acionado em relação a cada controlo no separador atual. Por predefinição, se o atributo está em falta, estiver definido como true. Pode ser utilizado em combinação com a propriedade RegularExpression. Para obter mais informações, consulte "RegularExpression" numa secção posterior deste documento.

## <a name="grouping"></a>Agrupamento

O elemento de agrupamento define o esquema geral de um painel. Atua como um contentor que agrupa em conjunto os controlos individuais em diferentes secções e separadores. O esquema XSD para o elemento de agrupamento é o seguinte:

```XML
<xsd:element name="Grouping">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:IsHeader"/>
          <xsd:attribute ref="my:IsSummary"/>
     </xsd:complexType>
</xsd:element>
```


Existem três tipos de **agrupamentos**:

1.  **Agrupamento de cabeçalho**: um cabeçalho de agrupamento é opcional. Só pode existir um agrupamento de cabeçalho num **painel**. Um agrupamento de cabeçalho é apresentada em cima um painel como legenda.
    Apenas um UocCaptionControl tem permissão para ser utilizado neste agrupamento. Para obter um exemplo de um agrupamento de cabeçalho, consulte a secção de exemplo.

2.  **Agrupamentos de conteúdo**: pelo menos um agrupamento de conteúdo não é necessário. Podem existir vários agrupamentos de conteúdo no painel de um. Um agrupamento de conteúdo é apresentado como o conteúdo de uma página RCDC principal. Cada agrupamento de conteúdo é apresentado como um separador no mesmo painel e pode conter entre 1 a 256 controlos. Consulte a secção de exemplo abaixo para obter um exemplo de um **agrupamento de conteúdo**.

3.  **Resumos agrupamentos**: um resumo de agrupamento é opcional. Só pode existir um agrupamento de resumo no painel de um. Um agrupamento de resumo é apresentado como o último separador de um painel. Apenas um **UocHtmlSummary** controlo pode ser utilizado um agrupamento de resumo para apresentar as alterações que o utilizador efetuou antes de submeter um pedido. Consulte a secção de exemplo abaixo para obter um exemplo de um agrupamento de resumo.

Cada tipo de agrupamento contém os seguintes elementos:

1.  **Ajudar**: este elemento fornece o texto de ajuda num separador. Também pode utilizar para adicionar uma ligação para um ficheiro de ajuda para o separador.

2.  **Controlos**: para obter informações sobre este elemento, consulte a secção de controlo deste documento. Cada agrupamento tem de ter 1 para 256 controlos, inclusivamente, dependendo do tipo de agrupamento.

3.  **Eventos**: para obter informações sobre este elemento, consulte a secção de eventos neste documento. Cada agrupamento pode, como opção, tem um evento. Os eventos que são suportados num elemento agrupamento são os seguintes:

    - **BeforeLeave**: Este evento é acionado quando o utilizador está preparado para deixar um separador de um agrupamento de conteúdo.
    - **AfterEnter**: Este evento é acionado quando o utilizador está preparado para introduzir um separador de um agrupamento de conteúdo.

Atributos:

1.  **Nome**: Este é o nome necessário de agrupamento. O **nome** têm de ser exclusivos dentro de **painel**.

2.  **Legenda**: O **legenda** aparece como a legenda de cabeçalho um agrupamento de cabeçalho. É apresentada como a legenda de separador de um conteúdo ou o resumo de agrupamento.

3.  **Descrição**: um atributo de cadeia opcional, **Descrição** está funcional apenas quando é utilizado um agrupamento de conteúdo. Utilize este elemento para conceder ao utilizador final alguns detalhes sobre as informações no mesmo separador.

  >[!NOTE]
  Se este atributo é utilizado um agrupamento de resumo, o XML é considerado inválida. Se este atributo é utilizado um agrupamento de cabeçalho, o XML é considerado válida mas ignorado.

4.  **Ativado**: um atributo booleano opcional, Enabled está definido como true quando está em falta. Se ativado estiver definido como false, o utilizador final verá um separador de desativado. Este atributo está funcional apenas no agrupamento de conteúdo.

  >[!NOTE]
  Se este atributo é utilizado um agrupamento de resumo, o XML é considerado inválida. Se este atributo é utilizado um agrupamento de cabeçalho, o XML é considerado válida mas ignorado.

5.  **Visível**: pode ocultar um separador de página RCDC ou o respetivo cabeçalho ao definir este atributo como false. Por predefinição, este atributo opcional, tipo de valor boleano é definido como true. Este atributo é funcionar apenas no agrupamento de conteúdo.

  >[!NOTE]
  Quando existe apenas um agrupamento de conteúdo no painel de um, esta funcionalidade não funciona. Quando existe mais do que um agrupamento de conteúdo no painel de um, comporta-se conforme descrito acima.

6.  **IsHeader**: este atributo é um atributo booleano opcional que define se o agrupamento é um agrupamento de cabeçalho. Se este atributo não for especificado, é definido como falso.

7.  **IsSummary**: Este é um atributo booleano opcional que define se o agrupamento é um agrupamento de resumo. Se este atributo não for especificado, é definido como falso.

![XML de configuração de RCD](media/rcd-configuration-xml-reference/image005.jpg)

O seguinte código de exemplo XML gera o agrupamento de cabeçalho anterior. O agrupamento de cabeçalho é a área com o agrupamento de cabeçalho de exemplo de texto.

```XML
<!--Sample for a Header Grouping-->
<my:Grouping my:Name="HeaderGroupingSample" my:IsHeader="true">
     <my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Header Grouping">
          <my:Properties>
               <my:Property my:Name="MaxHeight" my:Value="32"/>
               <my:Property my:Name="MaxWidth" my:Value="32"/>
          </my:Properties>
      </my:Control>
</my:Grouping>
<!--End of Header Grouping Sample-->
```

![XML de configuração de RCD](media\rcd-configuration-xml-reference/image007.jpg)

O seguinte código de exemplo XML gera o agrupamento de conteúdo anterior. O agrupamento de conteúdo é o mais à esquerda separador com o texto **agrupamento de conteúdo de exemplo**.

```XML
<!--Sample for a Content Grouping-->
<my:Grouping my:Name="ContentGroupingSample" my:Caption="Sample Content Grouping" my:Description="Some description for content grouping">
     <my:Control my:Name="DisplayName" my:TypeName="UocTextBox" my:Caption="Display name" my:Description="This is the display name of the set.">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="True"/>
               <my:Property my:Name="MaxLength" my:Value="128"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Content Grouping Sample-->
```

![XML de configuração de RCD](media/rcd-configuration-xml-reference/image010.jpg)

O seguinte código de exemplo XML gera o agrupamento de resumo anterior. O agrupamento de resumo é o separador à direita com o texto **resumo**.

```XML
<!--Sample for a Summary Grouping-->
<my:Grouping my:Name="Summary" my:Caption="Sample Summary Grouping" my:IsSummary="true">
     <my:Control my:Name="SummaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}"/>
               <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Summary Grouping Sample-->
```
### <a name="help"></a>Ajuda

O elemento de ajuda, como opção, pode ser incluído num agrupamento ou um elemento de controlo. Se for utilizada um agrupamento, tem de ser o primeiro elemento utilizado. Fornece ajuda textual para os utilizadores finais para ajudar a fornecer informações exatas relacionadas. Segue-se o esquema XSD para o elemento de ajuda:

```XML
<xsd:element name="Help">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:HelpText"/>
          <xsd:attribute ref="my:Link"/>
     </xsd:complexType>
</xsd:element>
```
Segue-se um exemplo do elemento de ajuda:
```XML
<my:Help my:HelpText="Some Help Text for Group Basic Info" my:Link="03e258a0-609b-44f4-8417-4defdb6cb5e9.htm#bkmk_grouping_GroupingBasicInfo" />
```

### <a name="control"></a>Controlar

Um elemento de agrupamento contém um ou mais elementos do controlo. Os controlos são os principais elementos um RCDC. Pode personalizar o elemento de agrupamento, definindo os diferentes elementos do controlo que contém. Segue-se o esquema XSD para o elemento de controlo:

```XML
<xsd:element name="Control">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:ExpandArea"/>
          <xsd:attribute ref="my:Hint"/>
          <xsd:attribute ref="my:AutoPostback"/>
          <xsd:attribute ref="my:RightsLevel"/>
     </xsd:complexType>
</xsd:element>
```

Um controlo contém os seguintes elementos:

1.  **Ajudar**: este elemento é ignorado. Funciona apenas no agrupamento.

2.  **CustomProperties**: este elemento não é suportado.

3.  **Opções de**: este elemento é utilizado apenas em combinação com o **UocDropDownList** ou **UocRadioButtonList** controlos. Não é funcional com quaisquer outros controlos. Consulte a secção de opções neste documento para a estrutura deste elemento. Consulte o controlo individuais para ver como é utilizada no contexto de um controlo.

4.  **Botões**: este elemento é utilizado apenas em combinação com o **UocListView** controlo. Não é funcional para quaisquer outros controlos. Para obter mais informações, consulte a secção de UocListView neste documento.

5.  Propriedades: Este elemento é utilizado em todos os controlos para especificar comportamentos adicionais de um controlo. Para obter informações sobre este elemento, consulte a secção de propriedades neste documento.

6.  **Eventos**: para a estrutura deste elemento, consulte a secção eventos anteriormente neste documento. Consulte a definição de controlo individuais para determinar qual o evento é utilizado nesse controlo.

Um controlo contém os seguintes atributos:

1.  **Nome**: Este é o nome do controlo. O nome de um controlo tem de ser exclusivo dentro de cada painel. Este é um atributo necessário, de tipo de cadeia.

2.  **TypeName**: este atributo especifica o tipo de controlo é. Este é um atributo necessário, de tipo de cadeia. Consulte a secção controlos individuais neste documento para cada nome de controlo.

3.  **Legenda**: pode utilizar este atributo para incluir uma legenda para o controlo.
    A legenda é, normalmente, o nome de apresentação dos dados que o controlo é apresentar ou inputting. Pode especificar um valor para a legenda explicitamente ou vinculá-lo com as informações do nome de apresentação de atributo do esquema. A legenda é apresentado no lado mais â esquerdo de um controlo de tamanho normal. Se um controlo é a expansão do ecrã inteiro, é apresentada a legenda sobre o controlo. Este é um atributo opcional, do tipo de cadeia. Para obter informações sobre como uma origem de dados com um atributo ou um valor de propriedade de enlace, consulte a secção de propriedades.

   O exemplo seguinte mostra como uma legenda pode ser utilizada explicitamente:

   ```XML
      <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias">…<my:Control/>
   ```
     O exemplo seguinte mostra como uma legenda pode ser utilizada com uma origem de dados. Se tiver utilizado o modelo para uma origem de dados apresentada anteriormente neste documento, a sua origem de dados é esquema. Recomendamos que vincular DisplayName o atributo com um atributo de legenda.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}">…<my:Control/>
    ```
4.  Ativado: Este é um atributo de tipo Booleano opcional. Ao definir este valor de atributo como false, o utilizador pode desativar um controlo. O valor predefinido é definido como true.

5.  Visível: Este é um atributo de tipo Booleano opcional. Pode utilizar este atributo para ocultar o controlo de todo. O valor predefinido é definido como true.

6.  Descrição: Utilize este atributo de tipo de cadeia opcional, para incluir uma descrição para ajudar o utilizador final a compreender o que deve colocam no controlo ou o que faz o controlo. Pode especificar um valor para a descrição ou vinculá-lo com as informações de descrição do atributo de esquema explicitamente. <br/>A descrição é apresentada no lado mais â esquerdo de um controlo de tamanho normal, por baixo da legenda. Se um controlo é a expansão do ecrã inteiro, a descrição é apresentada na parte superior do controlo, por baixo da legenda. Para obter informações sobre como uma origem de dados com um atributo ou um valor de propriedade de enlace, consulte a secção de propriedades neste documento.

7.  O exemplo seguinte mostra como uma descrição pode ser utilizada explicitamente:
  ```XML
  <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias" my:Description="This is explicit description.">…<my:Control/>
  ```
  Este exemplo mostra como uma descrição pode ser utilizada com uma origem de dados. Se tiver utilizado o modelo para uma origem de dados apresentada anteriormente neste documento, a origem de dados é **esquema**. Recomendamos que vincular o atributo **Descrição** com um atributo de descrição.
  ```XML
  <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=Alias.Description, Mode=OneWay}">…<my:Control/>
  ````
8. ExpandArea: Este atributo indica se o controlo abrange o ecrã inteiro. Este é um atributo de tipo Booleano opcional. O valor predefinido é definido como falso.

    >[!NOTE]
    Os atributos e descrição da legenda estão desativados quando este atributo está definido como true. Tem de utilizar o controlo de UocLabel para fornecer uma legenda para um controlo expandido.
9. **Sugestão**: Este é um atributo opcional, do tipo de cadeia. O texto no atributo de sugestão ajuda-o utilizador final decidir o que é uma entrada válida para o controlo. A sugestão aparece por baixo do controlo.

10.  **AutoPostback**: Este é um atributo de tipo Booleano opcional. O valor predefinido é falso. Se definido como FALSO, atualizar a página pode não atualizar o controlo. Para informações sobre AutoPostback, procure a propriedade de controlo do Microsoft ASP.NET UI com o mesmo nome.

11. **RightsLevel**: Este é um atributo opcional, do tipo de cadeia. É possível vincular este atributo só com direitos de inline com uma origem de dados. O controlo dinamicamente está ativado ou desativado, com base nos direitos de utilizador. Para obter informações sobre como ligar a origens de dados com um atributo ou um valor de propriedade, consulte a secção de propriedades neste documento.

    Este exemplo mostra como um **RightsLevel** atributo pode ser utilizado com uma origem de dados. Se tiver utilizado o modelo para uma origem de dados apresentada anteriormente neste documento, a origem de dados é **direitos**. Utilize o nome de atributo como o caminho.

### <a name="properties"></a>Propriedades

Pode utilizar uma propriedade para personalizar ainda mais o comportamento de cada controlo. Uma propriedade é um elemento vazio. O esquema XSD para o elemento de propriedade é o seguinte:
```XML
<xsd:element name="Properties">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Property">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```



Cada propriedade tem dois atributos necessários:

1.  **Nome**: este atributo de tipo de cadeia é o nome exclusivo da propriedade.
    Controlos diferentes tem propriedades diferentes. Existem algumas propriedades comuns que podem ser utilizadas por todos os controlos. Para obter mais informações sobre nomes de que estão disponíveis para um determinado controlo, consulte a secção de controlos individuais e propriedades comuns.

2.  **Valor**: Este é o valor da propriedade. O tipo de dados dos dependentes do valor em cuja propriedade está atribuído à. Consulte a secção seguinte para o formato do valor permitido para propriedades específicas.

Algumas propriedades podem estar vinculadas com informações de uma origem de dados. Para tal, tem de utilizar o seguinte formato de cadeia. Ver propriedades individuais na secção controlos individuais para saber como vinculá-los com uma origem de dados.

```
<my:Property my:Name="Required" my:Value="[Formatted String]"/>

Formatted String :=  “{Binding “ + [SourceExpression] + “,” + [PathExpression] + “,” + [ModeExpression]? + “}”

SourceExpression:= “Source=” + [ObjectDataSourceName]

PathExpression:= “Path=” + [AttributeName]|[AttributePropertyName]

ModeExpression:= “Mode=” + [ModeChoice]

ModeChoice:= “OneWay”|”TwoWay”

ObjectDataSourceName:= The value of any string assign to node /ObjectControlConfiguration/ObjectDataSource/Name.

AttributeName:= valid schema attribute name from the data source.

AttributePropertyName:= valid property name of a schema attribute from the data source.

````
**EXEMPLO:**

```XML
<my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
<my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>

```



<a name="common-properties"></a>Propriedades comuns
-----------------

Todos os controlos RCDC especificadas neste documento podem ter as seguintes propriedades comuns. Pode utilizar estas propriedades, juntamente com outras propriedades que são específicas para controlos diferentes.

1.  Obrigatório: Esta propriedade indica que o campo é um campo obrigatório ou um campo opcional. Um campo obrigatório tem de ser preenchido com um valor. Um valor vazio não é suportado no caso de entrada de cadeia. Um campo opcional pode ser deixado em branco. Se este campo é um campo obrigatório com nenhum valor preenchido, é apresentada uma mensagem de erro por cima do controlo de entrada. Explicitamente pode especificar se um campo é necessária ou opcional. Também é possível vincular o campo com as informações de esquema de um vínculo entre um atributo e um tipo de recurso especificado. Por predefinição, se esta propriedade está em falta, significa que o controlo é um controlo de entrada opcional.

   Segue-se um exemplo que utiliza um valor explícito para esta propriedade:

    ```XML
    <my:Property my:Name="Required" my:Value="True"/>
    ```
   Este é um exemplo que utiliza uma origem de dados dinâmicos para esta propriedade. Se tiver utilizado o modelo para uma origem de dados mostrada na secção anterior deste documento, a sua origem de dados é esquema. Utilize \<nome de atributo\>. É necessário como o caminho.

    ```XML
    <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
    ```
2. **Só de leitura**: ao definir esta propriedade para verdadeiro, o utilizador final ocorre com o controlo num modo só de leitura. Este é um atributo de tipo Booleano opcional.
    O valor predefinido é definido como falso. No entanto, por vezes, o comportamento desta propriedade é substituído pelo tipo de direitos de que uma pessoa tenha no enlace de dados com o controlo. Por exemplo, se um utilizador não tem direitos para atualizar um campo e o campo está vinculado com direitos de inline, o utilizador verá os dados de um modo só de leitura que até ser esta propriedade for definida como false.

3.  **RegularExpression**: Esta propriedade especifica as restrições que são impostas no valor no controlo. Os formatos deste valor de propriedade são os formatos que são suportados no StringRegex .NET padrão. Para obter mais informações, consulte [expressões regulares do .NET Framework](http://go.microsoft.com/fwlink/?LinkId=165361). Se o controlo é utilizado para um valor de entrada, o valor é verificado em relação a restrição especificado nesta propriedade quando o utilizador atempts para sair da página atual.
    É apresentada a mensagem de erro sobre o controlo que tem a entrada inválida. O utilizador pode especificar explicitamente uma expressão regular de cadeia. O utilizador também pode vinculá-lo com as informações de esquema de um atributo especificado. Por predefinição, se esta propriedade está em falta, significa que o controlo não verifica se existem quaisquer restrições em cadeias de entrada.
    Segue-se um exemplo que utiliza um valor explícito para esta propriedade:

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
    ```
    Este é um exemplo que utiliza uma origem de dados dinâmicos para esta propriedade. Se tiver utilizado o modelo para uma origem de dados apresentada anteriormente neste documento, a sua origem de dados é esquema. Utilize o <attribute name>. StringRegex como o caminho.
    ```XML
    <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=Alias.StringRegex, Mode=OneWay}"/>
    ```
4.  Visível: Este é um atributo de tipo Booleano opcional. Pode utilizar este atributo para ocultar o controlo de todo. O valor predefinido é definido como true.

### <a name="options"></a>Opções

O elemento de opções inclui um ou mais subnodes opção. O elemento de opções é utilizado apenas com os controlos UocRadioButtonList e UocDropDownList. Para obter detalhes sobre como utilizá-los, consulte a secção controlos individuais. Segue-se o esquema XSD para o elemento de opções:

```XML
<xsd:element name="Options">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Option">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```


Atributos:

1.  Valor: Este é um atributo obrigatório do tipo cadeia. O atributo valor deve ser exclusivo num mesmo controlo. Apenas a z, podem ser utilizados carateres sensível.

2.  Legenda: Este atributo necessário o nome a apresentar de cada opção.

3.  Sugestão: Este é um atributo opcional. Utilize este atributo para fornecer mais informações e sugestões para o utilizador final.

### <a name="environment-variables"></a>Variáveis de ambiente

As variáveis de ambiente na tabela seguinte podem ser utilizadas em qualquer configuração RCDC.

| variável | Descrição |
|--------|--------|
| `<LoginID>`       | Apresenta o ID do utilizador que está atualmente sessão iniciado.           |
| `<LoginDomain>`   | Apresenta o domínio do utilizador que está atualmente sessão iniciado.       |
| `<Today>  `       | Apresenta a data e hora atuais                                |
| `<FromToday_nnn>` | Apresenta a data, plus nnn e hora atuais. nnn é um número inteiro.  |
| `<ObjectID> `     | O ID de recurso primário RCDC.                                     |
| `<Attribute_xxx>` | Devolve um atributo especificado, xxx, do recurso RCDC primário. |

### <a name="debugging-xml-configuration-files"></a>Ficheiros de configuração do XML de depuração


Quando estiver a desenvolver ou modificar ficheiros de configuração XML para uma RCDC, pode ajudar a reduzir os erros ao validar o XML com ficheiros de XSD, utilizando um editor, como Microsoft Visual Studio®. Para obter mais informações, consulte [uma introdução às ferramentas de XML no Visual Studio 2005](http://go.microsoft.com/fwlink/?LinkID=74512).

### <a name="customizing-a-help-file"></a>Personalizar o ficheiro de ajuda

Se criar novos recursos e atributos, poderá pretender atualizar os ficheiros de ajuda existentes no Portal do FIM com conteúdo para os seus recursos personalizados. Ficheiros de ajuda no Portal do FIM estão no formato. htm e podem ser editados manualmente.

>[!IMPORTANT]
Para obter mais informações sobre a criação de atributos personalizados, consulte a introdução ao recurso personalizado e gestão de atributo na documentação do FIM 2010.

>[!IMPORTANT]
Esta secção fornece informações sobre as noções básicas de formatação ou edição de HTML. Para modificar os ficheiros de ajuda já deve estar familiarizado com a edição de HTML


**Localização dos ficheiros de ajuda**: todos os ficheiros de ajuda para o SP1 Portal do Microsoft Identity Management 2016 estão localizados na seguinte pasta no servidor de serviço de MIM:

  `<ProgramFiles>\Common Files\Microsoft Shared\Web Server Extensions\12\Template\Layouts\MSILM2\Help\1033\html`

**Como localizar o ficheiro de ajuda adequado**: todos os ficheiros de ajuda para o FIM Portal são denominados com um identificador exclusivo global (GUID). Para localizar o ficheiro correto para o seu recurso personalizado:

1.  No Portal do FIM, abra o ficheiro de ajuda na página de Portal que pretende personalizar.

2.  O ficheiro de ajuda com o botão direito e, em seguida, clique em **propriedades**.

3.  Realce e copie o `<GUID\>.htm` ficheiro no campo do endereço de URL.

4.  Navegue para a pasta onde estão armazenados os ficheiros de ajuda e procure o ficheiro.

**Adicionar conteúdo para um novo atributo**: ao adicionar o conteúdo descritivo para um novo atributo dentro de um elemento de agrupamento existente (separador):

1.  Identificar e localize o ficheiro de ajuda adequado.

2.  Com um editor de HTML, abra o ficheiro.

3.  Localize onde pretende adicionar o conteúdo. Normalmente, trata-se num parágrafo adicional, por exemplo:

`<p xmlns="">A new paragraph with customized information.</p>`

Também pode ser um item inserido uma lista existente, por exemplo:
```
<li class="unordered"><b>First Name</b> – The first name of the User.<br>

<li class="unordered"><b>Last Name</b> - The last name of the User.<br>

<li class="unordered"><b>Added a new line</b><br>
```

**Adicionar conteúdo para um novo elemento de agrupamento**: A maioria das páginas do FIM Portal vários agrupamento elementos (ou separadores) e a ajuda associada ficheiros tem bookmarked secções que se relacionam com a cada elemento de agrupamento. Os marcadores no HTML especificados nas secções. Por exemplo, este é o HTML para o separador de informações de trabalho a partir do ficheiro de ajuda para a página de utilizador de criar no Portal do FIM:

```
<a name="bkmk_grouping_WorkInfo" xmlns=""></a><h3 class="subHeading" xmlns="">Work Info</h3><p class="subHeading" xmlns=""></p><div class="subSection" xmlns="">
```

Este é referenciado pelo elemento do agrupamento **WorkInfo** no ficheiro XML de dados de configuração para o **configuração para criação de utilizador de** RCDC. Tenha em atenção que o `\<GUID\>.htm` especificados no nome de ficheiro e o marcador o meu: parâmetro de ligação:

```
<my:Grouping my:Name="WorkInfo" my:Caption="%SYMBOL_WorkInfoTabCaption_END%" my:Enabled="true" my:Visible="true"> <my:Help my:HelpText="%SYMBOL_WorkInfoTabHelpText_END%" my:Link="5e18a08b-4b20-48b8-90c6-c20f6cbeeb44.htm#bkmk_grouping_WorkInfo"/>
```

**Exemplos de controlo simples**

A figura seguinte mostra um exemplo de controlos de outra caixa de texto simples em modos diferentes.

Exemplo:

![](media/rcd-configuration-xml-reference/image016.gif)

O segmento de código seguinte cria o primeiro controlo de caixa de texto, que utiliza o texto explícito para todos os atributos e as propriedades:

```
<!-- Sample for a simple control to use explicit information. (with hints)-->
<my:Control my:Name="ExplicitControl" my:TypeName="UocTextBox" my:Caption="Explicit Control" my:Description="This is explicit description." my:Hint="This is a Hint (enter any text).">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
          <my:Property my:Name="Text" my:Value="Enter Information Here"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use explicit information.-->

```


O segmento de código seguinte cria o controlo de caixa de texto segundo, que utiliza a técnica de enlace dinâmico para ligar o controlo com uma origem de dados diferentes:

```
<!-- Sample for a simple control to use stored data information.-->
<my:Control my:Name="DynamicControl" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=DisplayName, Mode=OneWay}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required, Mode=OneWay}"/>
          <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=DisplayName.StringRegex, Mode=OneWay}"/>
          <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use stored data information.-->
```


O segmento de código seguinte cria a etiqueta expandida terceira e o controlo de caixa de texto:

```
<!-- Sample for a simple expanded control with caption control.-->
<my:Control my:Name="SampleExpandLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is an expanded control."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ExpandedControl" my:TypeName="UocTextBox"
          my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="Columns" my:Value="40"/>
          <my:Property my:Name="Text" my:Value="Expanded control (enter text)"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple expanded control.-->
```

O segmento de código seguinte cria o controlo de caixa de texto desativado quarto.
Embora este controlo não for apresentado uma visível diferença entre o estado desativado e o estado ativado, o utilizador já não pode introduzir dados na caixa de texto.

```
<!-- Sample for a simple disabled control.-->
<my:Control my:Name="DisabledControl" my:TypeName="UocTextBox" my:Caption="Disabled Control" my:Description="This is disabled simple control." my:Enabled="false">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="MaxLength" my:Value="128"/>
          <my:Property my:Name="Text" my:Value="Disabled control"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple disabled control.-->
```
## <a name="individual-controls"></a>Controlos individuais

### <a name="uocbutton"></a>UocButton

**Nome**: UocButton

**Descrição**: Este é um controlo de botão simples que pode utilizar para acionar determinadas ações. No entanto, porque não é possível especificar o suas próprias processador está limitada a utilização deste controlo.

**Propriedades**:

1.  **Todas as propriedades comuns**: para obter informações sobre esta propriedade, consulte a secção de propriedades comuns deste documento.

2.  **Texto**: Esta propriedade especifica o texto que aparece no botão. Este é um atributo opcional, do tipo de cadeia. O texto cria um valor de cadeia explícita.

Evento:

   • **OnButtonClicked**: O evento é emitido quando o botão é clicado.

Exemplo:

![](media/rcd-configuration-xml-reference/image017.png)


O segmento XML seguinte produz o botão simple:

```
<!--Sample enabled simple button control-->
<my:Control my:Name="ButtonControl" my:TypeName="UocButton" my:Caption="SampleButton" my:Description="This is a simple button."
my:Hint="Click the button">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="Text" my:Value="Click Me"/>
     </my:Properties>
</my:Control>
<!--End of sample enabled simple button control -->
```

### <a name="uoccaptioncontrol"></a>UocCaptionControl

**Nome**: UocCaptionControl

**Descrição**: este controlo é utilizado para apresentar a legenda de uma página RCDC. Este controlo foi concebido para ser utilizada apenas como um controlo único de um agrupamento de cabeçalho.
Utilizá-la em qualquer outro contexto pode causar problemas de composição ou erros de portais.

**Modo**: só de leitura (OneWay)

**Propriedades:**

1.  **Todas as propriedades comuns:** para obter mais informações, consulte a secção de propriedades comuns deste documento.

2.  **MaxHeight:** esta propriedade especifica a altura máxima do ícone na secção de legenda. Esta propriedade é opcional. Esta propriedade tem um valor inteiro em pixéis. O valor predefinido é de 32 pixéis.

Exemplo:

![](media/rcd-configuration-xml-reference/image020.jpg)

O segmento de código seguinte gera o **cabeçalho legenda**:
```
<!--Sample header caption control-->
<my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Header Caption" my:Description="Description Starts here.">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="32"/>
          <my:Property my:Name="MaxWidth" my:Value="32"/>
     </my:Properties>
</my:Control>
<!--End of sample header caption control-->

```


Este segmento de código gera o **legenda conteúdo explícita:**

```
<my:Control my:Name="SampleContentCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Explicit Content Caption" my:Description="Explicit content caption with smaller icon">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="20"/>
          <my:Property my:Name="MaxWidth" my:Value="20"/>
     </my:Properties>
</my:Control>
<!--End of sample caption-->
```

O segmento de código seguinte gera o **nome a apresentar** legenda dinâmica:
```
<!--Sample content dynamic caption-->
<my:Control my:Name="Caption3" my:TypeName="UocCaptionControl" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}"/>
<!--End of sample caption -->
```

### <a name="uoccheckbox"></a>UocCheckBox

**Nome**: UocCheckBox

Descrição: Este é um controlo de caixa de verificação simples. Recomendamos que o utilizador vincular este controlo com dados de tipo Booleano. Este controlo pode ser utilizado como um controlo só de leitura ou de um controlo atualizável, com base nos dados que está vinculada a.

>[!NOTE]
Nesta versão, ao utilizar a caixa de verificação controlar no modo de edição para apresentar um atributo booleano, se o atributo não tem um valor atribuído anteriormente, o controlo de recursos adiciona um valor de **falso** para o atributo quando **OK** é clicado no modo de edição. O trabalho é aproximadamente sempre criar um atributo booleano que parte do princípio de que não existência é o mesmo que **falso**, ou utilizar outros controlos, como um botão de opção atributos Booleanos.

**Propriedades**:

1.  **Todas as propriedades comuns**: para obter mais informações, consulte a secção de propriedades comuns deste documento.

2.  **DefaultValue**: Esta é uma propriedade opcional, de tipo Booleano. O valor predefinido é definido como falso. Este campo especifica o comportamento predefinido de uma caixa de verificação.
    Isto pode ser especificado explicitamente.

3.  **Verificado**: Esta é uma propriedade opcional, de tipo Booleano. O valor predefinido é definido como falso. Este valor substitui a propriedade DefaultValue quando estiver presente, juntamente com o atributo ValorPredefinido. Este campo especifica o comportamento de uma caixa de verificação. Como DefaultValue, isto pode de ser especificado explicitamente ou vinculado com dados do servidor.

4.  **Texto**: Este é um atributo opcional, do tipo de cadeia. O texto é apresentado no lado direito da caixa de verificação. Pode utilizar esta propriedade para especificar o texto que fornece mais informações para o utilizador final.

**Eventos**:

   • CheckedChanged: quando a caixa de verificação altera o estado, este evento é emitido.

No exemplo seguinte, um enlace personalizado é criado entre o tipo de recurso personalizado e o atributo **IsConfigurationType**. O XML é utilizado na RCDC de um tipo de recurso personalizado.

Exemplo:

![](media/rcd-configuration-xml-reference/image022.png)

O seguinte código segmento produz um **caixa de verificação dinâmica**, conforme mostrado como caixa de verificação dinâmicos na imagem anterior. Este tipo de enlace é, geralmente, mais versátil e útil de uma caixa de verificação explícita. O atributo tem de pertencer ao tipo de recursos atual.

```
<!--Sample dynamic check box-->
<my:Control my:Name="SampleDynamicCheckBox" my:TypeName="UocCheckBox" my:Caption="Dynamic Check Box" my:Description="This is a dynamic check box. It saves to data source." my:RightsLevel="{Binding Source=rights, Path=IsConfigurationType}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="{Binding Source=schema, Path=IsConfigurationType.DisplayName, Mode=OneWay}"/>
          <my:Property my:Name="Checked" my:Value="{Binding Source=object, Path=IsConfigurationType, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample dynamic check box -->
```


### <a name="uoccommonmultivaluecontrol"></a>UocCommonMultiValueControl

**Descrição**: Este é um controlo de caixa de texto com várias linhas que suporte a formatação da cadeia especial. Cada valor existente entre as entradas com múltiplos valores é separada umas das outras por ponto e vírgula; ou uma quebra de linha na caixa de texto. Recomendamos que este controlo estar vinculado com dados de tipos de cadeia e o número inteiro com múltiplos valores, abreviados. Este controlo suporta o modo só de leitura e o modo atualizável.

**Propriedades**:

1.  **Todas as propriedades comuns**: para obter mais informações, consulte a secção de propriedades comuns deste documento.

2.  **Tipo de dados**: Este é um atributo necessário, de tipo de cadeia. Pode especificar-o como um **cadeia, número inteiro**, ou **DateTime** tipo explicitamente. Também é possível vincular o atributo com o atributo de esquema **DataType** propriedade. Um tipo de referência com múltiplos valores deve ser processado pela **UOCListView** ou **UOCIdentityPicker**. Valor booleano com múltiplos valores não é um tipo de dados suportada.

3.  **Linhas**: Este é um atributo opcional, do tipo número inteiro. Pode definir a altura da caixa de num número de carateres. O valor predefinido é definido como 1.

4.  **Colunas**: Este é um atributo opcional, do tipo número inteiro. Pode definir quantos ao nível da caixa está num número de carateres. O valor predefinido é definido como
    20.

5.  **Valor**: Este é um atributo opcional, do tipo de cadeia. É possível vincular este atributo apenas com a origem de dados.

Eventos:

   • **ValueListChanged**: Este evento é acionado quando o valor atual no controlo é alterado.

No exemplo seguinte, um atributo com múltiplos valores de cadeia denominado **AMultiValueString** é criado e vinculado ao tipo de recurso personalizado. Neste exemplo funciona apenas depois de criar este enlace.

**Example:**

![](media/rcd-configuration-xml-reference/image024.jpg)

O segmento de código seguinte gera a ilustração anterior:

```
<!--Sample multivalue control-->
<my:Control my:Name="SampleDynamicMultiValueControl" my:TypeName="UocCommonMultiValueControl" my:Caption="{Binding Source=schema, Path=AMultiValueString.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=AMultiValueString.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=AMultiValueString}">
     <my:Properties>
          <my:Property my:Name="Rows" my:Value="6"/>
          <my:Property my:Name="Columns" my:Value="60"/>
          <my:Property my:Name="DataType" my:Value="String"/>
          <!--not supported for above property my:Value={Binding Source=schema, Path=AMultiValueString.DataType, Mode=OneWay}"/>-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AMultiValueString, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample multivalue control -->
```



### <a name="uocdatetimecontrol"></a>UocDateTimeControl


**Nome**: UocDateTimeControl

**Descrição**: Isto é semelhante a um controlo de caixa de texto, mas **Descrição** aceita apenas determinado formato. No modo só de leitura, aparece como uma etiqueta. Para o formato da cadeia de entrada que é suportado, consulte o **DateTimeFormat** propriedade nesta secção.

**Propriedades**:

1.  **Todas as propriedades comuns**: para obter mais informações, consulte a secção de propriedades comuns deste documento.

2.  **DateTimeFormat**: Este é um atributo opcional, do tipo de cadeia. Os formatos suportados são DateTime ou DateOnly. O valor predefinido é definido no formato DateTime.
    a. Formato de DateTime: este atributo é a formatados como mm/dd/aaaa hh: ss AM.

      <[!NOTE]
      Ambos **DateTime** e **DateOnly** formato são suportadas, independentemente do utilizador que está a especificar a diferença.
3.  **Valor**: Este é um atributo opcional, do tipo de cadeia. Vincular este atributo com uma origem de dados de recursos. O valor deste atributo tem de estar em conformidade com o formato de datetime correto.

Eventos:

   • **DateTimeChanged**: quando as alterações do valor de datetime, ocorre o evento.

Exemplo:

![](media/rcd-configuration-xml-reference/image027.jpg)

O segmento de código seguinte produz o primeiro **DateTime** controlo.

```
<!--Sample explicit DateTime control-->
<my:Control my:Name="SampleExplicitDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="Explicit Date Time Control" my:Description="The data shown here is explicit and in date time format.">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateTime"/>
          <my:Property my:Name="Value" my:Value="11/11/2008 00:00:00"/>
     </my:Properties>
</my:Control>
<!--End of sample explicit DateTime control -->
```


O segmento de código seguinte produz o segundo **DateTime** controlo. Se tiver utilizado o código de exemplo na secção de origens de dados, o **ExpirationTime** atributo está vinculado a todos os tipos de recursos. Por conseguinte, pode utilizá-lo com o código seguinte:

```
<!--Sample dynamic DateTime control-->
<my:Control my:Name="SampleDynamicDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="{Binding Source=schema, Path=ExpirationTime.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ExpirationTime.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ExpirationTime}">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateOnly"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExpirationTime, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic explicit DateTime control -->
```



### <a name="uocdropdownlist"></a>UocDropDownList

**Nome**: UocDropDownList

Descrição: Este é um controlo de caixa de lista pendente simples. Este controlo é normalmente utilizado quando pretender selecionar opções de um conjunto definido de opções. Tipos de dados de cadeia, número inteiro, datetime e booleanos são ideais para este controlo.

**Propriedades**:

1.  **Todas as propriedades comuns**: para obter mais informações, consulte a secção de propriedades comuns deste documento.

2.  **ValuePath**: A propriedade para obter o atributo de valor de ItemSource. Quando ItemSource é especificado como personalizada, o caminho do valor é definido como valor. Cria um enlace com o campo de valor do elemento de opção definido posteriormente neste documento.

3.  **CaptionPath**: A propriedade para obter o atributo de valor de ItemSource. Quando ItemSource é especificado como personalizada, o caminho de valor está definido para a legenda. Cria um enlace com o campo de legenda do elemento de opção definido posteriormente neste documento.

4.  **HintPath**: A propriedade para obter o atributo de valor de ItemSource. Quando ItemSource é especificado como personalizada, o caminho do valor é definido para a sugestão. Cria um enlace com o campo de sugestão do elemento de opção definido posteriormente neste documento.

5.  **ItemSource**: uma coleção de ListControlItems que define as opções na lista. O utilizador explicitamente pode configurar esta definição para personalizar e utilize o elemento de opção para especificar o valor da cadeia.

6.  **SelectedValue**: O valor que está atualmente selecionado. Esta é uma propriedade necessária, do tipo de cadeia. Esta propriedade está vinculada com dados de cadeias da origem de dados.

Eventos:

  • SelectedIndexChanged: O evento ocorre quando a seleção na caixa de lista pendente é alterado.

Opções:

Para a estrutura de um elemento de opção, consulte a secção "Opção" neste documento.

1.  **Valor**: O valor de um único elemento de opção pode ser definido como qualquer cadeia que é a entrada da origem de dados que o controlo está vinculado a válida.

2.  **Legenda**: legenda pode ser qualquer valor de cadeia.

3.  **Sugestão**: sugestão pode ser qualquer valor de cadeia.

Exemplo:

![](media/rcd-configuration-xml-reference/image030.jpg)


![](media/rcd-configuration-xml-reference/image031.jpg)

>[!NOTE]
Para tornar o exemplo funcionar, tem de vincular um atributo de tipo de cadeia existente, **âmbito** com o tipo de recurso personalizado que se aplica a RCDC.


Este segmento de código gera na lista pendente:

```
<!--Sample for drop-down list control-->
<my:Control my:Name="Scope" my:TypeName="UocDropDownList" my:Caption="{Binding Source=schema, Path=Scope.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Scope}">
     <my:Options>
          <my:Option my:Value="DomainLocal" my:Caption="Domain Local" my:Hint="to secure a local resource (i.e. a file share on your computer)" />
          <my:Option my:Value="Global" my:Caption="Global" my:Hint="to secure resources across your team or division" />
          <my:Option my:Value="Universal" my:Caption="Universal" my:Hint="to use this group across your organization" />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Scope.Required" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="ItemSource" my:Value="Custom" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=Scope, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for drop-down list control-->
```


### <a name="uocfiledownload"></a>UocFileDownload

Nome: UocFileDownload

Descrição: Este controlo contém uma hiperligação. Quando se clica na hiperligação, é apresentada uma página de Windows guarde o ficheiro. O utilizador pode guardar o ficheiro para a respetiva unidade local.
A opção de abrir também é suportada se o Internet Explorer é possível compor o formato de ficheiro. Os tipos de dados recomendadas para utilizar este controlo com são formatados cadeia (XML) e tipos binários.

>[!NOTE]
Nesta versão do Microsoft Identity Manager 2016 SP1, o utilizador tem de fechar a janela do Internet Explorer em que seja abrir o ficheiro e, em seguida, atualize a página. Depois de atualizar a janela do Internet Explorer, o utilizador, em seguida, pode iniciar a transferência para guardar ou abrir o mesmo ficheiro novamente na janela do original.


Propriedades:

1.  **Todas as propriedades comuns**: para obter mais informações, consulte a secção de propriedades comuns deste documento.

2.  **Texto**: Este é um atributo de tipo de cadeia opcional, que define o texto da hiperligação. O utilizador pode especificar uma cadeia explícita para esta propriedade.

3.  **Valor**: Este é um atributo necessário. Especifica o vínculo de atributos no servidor cujo conteúdo está a ser transferido.

4.  **PromptedFileName**: Este é um atributo opcional, do tipo de cadeia. Este é o nome de ficheiro que é sugerido para o utilizador quando se guardam o ficheiro transferido.

5.  **ContentType**: Este é um atributo necessário, de tipo de cadeia. Este é o tipo de ficheiro que os dados são guardados no. Texto ou binário são as duas opções de cadeia suportados. Se se tratar de texto, o valor de retorno é considerado como uma cadeia de comprimento.
    Caso contrário, para o binário, o valor de retorno é considerados como estando byte []. Se o texto é selecionado, o utilizador pode, como opção, adicionar a está a ser um sufixo para especificar o tipo de formato de texto. Por exemplo, texto/xml é válido.


>[!NOTE]
Quando o valor que está vinculado a este controlo está vazio, o controlo está em falta a hiperligação para ser utilizado para acionar a ação de transferência. Isto acontece porque não há nada para transferir.


**Eventos**:

Não existem não há eventos para este controlo.

**Exemplo**:

![](media/rcd-configuration-xml-reference/image035.png)


>[!NOTE]
Antes de carregar este ficheiro de exemplo, o utilizador tem de criar um vínculo entre um tipo de recurso personalizado e o atributo de ConfigurationData existente.


O segmento de código seguinte gera o controlo de transferência do ficheiro na figura anterior:

```
<!--Sample dynamic download control-->
<my:Control my:Name="SampleDynamicFileDownloadControl" my:TypeName="UocFileDownload" my:Caption="{Binding Source=schema, Path=ConfigurationData.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ConfigurationData.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ConfigurationData}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="Download Dummy xml"/>
          <my:Property my:Name="PromptedFileName" my:Value="DummyXML.xml"/>
          <my:Property my:Name="ContentType" my:Value="text/xml"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ConfigurationData}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic download control -->
```


### <a name="uocfileupload"></a>UocFileUpload

**Nome**: UocFileUpload

**Descrição**: este controlo contém uma caixa de texto que apresenta a localização do ficheiro local para ser carregado e um botão do ficheiro de procura e um botão de carregamento. Quando o utilizador final clica num botão de procura, é apresentada uma janela de ficheiro aberto do Windows. O utilizador final pode selecionar um ficheiro no respetivo disco local para carregar. Quando o ficheiro está selecionado, a localização do ficheiro é apresentada na caixa de texto. Quando clica no botão carregar, o ficheiro é carregado para a origem de dados local do lado do cliente. O conteúdo do ficheiro ainda não é submetido ao servidor. Os tipos de dados recomendadas para utilizar este controlo com são os seguintes: formato de cadeia (XML) ou tipos binários.

>[!NOTE]
Não é uma indicação do progresso de carregamento ou estado. Quando o ficheiro é carregado para a origem de dados local, a caixa de texto está desmarcada.


Propriedades:

1.  Todas as propriedades comuns: Para obter informações, consulte a secção de propriedades comuns deste documento.

2.  Valor: Este é um atributo necessário. Especifica o vínculo de atributos de esquema no servidor ao qual os dados são carregados.

3.  ContentType: Este é um atributo opcional, do tipo de cadeia. Este é o tipo de dados que o ficheiro é guardado no servidor. Isto pode ser definido para texto ou binário. Quando a propriedade está em falta, o valor predefinido é binário.

4.  MaxFileSize: Este é um atributo opcional, do tipo de cadeia. MaxFileSize define como grande pode ser o tamanho de ficheiro carregado. Por predefinição, se a propriedade está em falta, o tamanho máximo é 1 megabytes (MB).

5.  PromptedForNoValue: Este é um atributo opcional, do tipo de cadeia. Define o texto que aparece para o utilizador quando um ficheiro não está a ser carregado.

Eventos:

   • FileUploaded: Este evento é emitido quando o ficheiro é carregado com êxito.

Exemplo:

![](media/rcd-configuration-xml-reference/image040.png)

>[!NOTE]
Para efetuar o seguinte código de exemplo funcione, tem de criar um novo atributo de tipo binário com o nome ABinaryAttribute e, em seguida, crie um novo enlace entre um tipo de recurso personalizado e este atributo.


O segmento de código seguinte gera o controlo de carregamento na figura anterior:
```
<!--Sample dynamic upload control-->
<my:Control my:Name="SampleDynamicFileUploadControl" my:TypeName="UocFileUpload" my:Caption="{Binding Source=schema, Path=ABinaryAttribute.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ABinaryAttribute.Description, Mode=OneWay}” my:RightsLevel="{Binding Source=rights, Path=ABinaryAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABinaryAttribute.Required}"/>
          <my:Property my:Name="ContentType" my:Value="Binary"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ABinaryAttribute, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic upload control -->
```

### <a name="uocfilterbuilder"></a>UocFilterBuilder

Nome: UocFilterBuilder

Descrição: Este é um controlo complexo, que permite ao utilizador para compor uma expressão de XPath de MIM 2016. Alguns expressões XPath não são suportadas. Para obter informações sobre como utilizar o construtor de filtros, consulte a ajuda do construtor de filtros.

Propriedades:

1.  Todas as propriedades comuns: Para obter mais informações, consulte a secção de propriedades comuns deste documento.

2.  PermittedObjectTypes: Isto define uma lista de tipos de recursos a apresentar na instrução select do construtor de filtros. Para obter informações sobre como utilizar o construtor de filtros, consulte o ajuda do construtor de filtros. A cadeia é no formato de ResourceTypeA, ResourceTypeB, onde cada tipo de recurso é separado por uma vírgula (,).

3.  Valor: Este é o valor com o qual o construtor do filtro é composto.
    Apenas um enlace com dados de tipo de cadeia que contém uma expressão de XPath é suportado. O atributo de filtro é um atributo recomendado para este controlo de enlace.

4.  PreviewButtonVisible: Esta é uma propriedade de tipo Booleano opcional. Quando esta propriedade está definida como false, o utilizador não consegue ver um botão de pré-visualização. O valor predefinido é definido como true. Este botão pode ser utilizado em combinação com um controlo de vista de lista para pré-visualizar os resultados de uma expressão de XPath.

5.  ExcludeGroupMembership: Esta é uma propriedade booleana. Quando esta propriedade está definida como true, não é possível criar um filtro que utiliza \<atributo de referência\> (por exemplo, ResourceID) é membro de \<objeto de grupo\>. Por outras palavras, quando esta propriedade está definida como true, não é possível criar um filtro que utiliza o diretório de associação de grupo.

6.  PreviewButtonCaption: Esta é uma cadeia opcional. Quando PreviewButtonVisible está definido como VERDADEIRO, pode utilizar esta propriedade para dar o botão um texto personalizado. O texto é apresentado no botão de pré-visualização.

Eventos:

   • OnFilterChanged: Este evento é acionado quando o conteúdo de construtor de filtro é alterado.

Exemplo:

![](media/rcd-configuration-xml-reference/image044.png)

>[!NOTE]
Antes de utilizar este código de exemplo, crie um novo enlace entre um atributo de filtro existente e um tipo de recurso personalizado.


Segue-se o código de exemplo que inclua um controlo UOCLabel, um construtor de filtros simples com PermittedObjectTypes e uma vista de lista de pré-visualização. Tem de apontar a vista de lista ListFilter propriedade e o construtor de filtros de propriedade de valor para o mesmo atributo de origem de dados para ligar os dois.

```
<!--Sample filter builder with preview list-->
<my:Control my:Name="ComplexFilterBuilderLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is a Filter Builder with preview."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ComplexFilterBuilder" my:TypeName="UocFilterBuilder" my:RightsLevel="{Binding Source=rights, Path=Filter}" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="PermittedObjectTypes" my:Value="Person,Group" />
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Filter, Mode=TwoWay}" />
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Filter.Required, Mode=OneWay}" />
     </my:Properties>
</my:Control>
<my:Control my:Name="FilterBuilderwithpreview" my:TypeName="UocListView" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName,ObjectType,AccountName" />
          <my:Property my:Name="EmptyResultText" my:Value="There is no members according to the filter definition." />
       <my:Property my:Name="PageSize" my:Value="10" />
       <my:Property my:Name="ShowTitleBar" my:Value="false" />
       <my:Property my:Name="ShowActionBar" my:Value="false" />
       <my:Property my:Name="ShowPreview" my:Value="false" />
       <my:Property my:Name="ShowSearchControl" my:Value="false" />
       <my:Property my:Name="EnableSelection" my:Value="false" />
       <my:Property my:Name="SingleSelection" my:Value="false" />
       <my:Property my:Name="ItemClickBehavior" my:Value=" ModelessDialog "/>
       <my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}" />
     </my:Properties>
</my:Control>
<!--end of sample filter builder with preview-->
```


<a name="uochtmlsummary"></a>UocHtmlSummary
--------------

Nome: UocHtmlSummary

Descrição: Pode utilizar este controlo para definir uma página de resumo na página de RCDC.
Esta página de resumo aparece depois do utilizador final submete um pedido. Este controlo só pode ser utilizado um agrupamento de resumo, e tem de ser o controlo só de números. Recomendamos vivamente que utilize o código de exemplo é fornecido.

>[!NOTE]
Este controlo não foi testado extensivamente.


Propriedades:

1.  Todas as propriedades comuns: Para obter informações sobre esta propriedade, consulte a secção de propriedades comuns deste documento.

2.  ModificationsXml: Esta propriedade tem de ser formatada como {enlace origem = delta, caminho = DeltaXml}, onde o delta está definido no cabeçalho de configuração ObjectDataSource.

3.  TransformXsl: Esta propriedade é, normalmente, o formato {enlace origem = summaryTransformXsl, caminho = /}, onde summaryTransformXsl está definido no cabeçalho de configuração XmlDataSource.

Para obter um exemplo existente deste controlo, consulte "Resumo agrupamento" na secção agrupamento anteriormente neste documento.

### <a name="uochyperlink"></a>UocHyperLink

Nome: UocHyperLink

Descrição: Este é um controlo de hiperligação simples. Pode utilizar este controlo para apresentar informações como uma hiperligação.

Propriedades:

1.  Todas as propriedades comuns: Para obter informações, consulte a secção de propriedades comuns deste documento.

2.  ObjectReference: Esta propriedade é opcional, tipo de referência. Se um recurso válido. é referenciado pelo GUID que é definido nesta propriedade, hiperligação fornece uma forma para aceder ao recurso de utilizador final. Isto é mutuamente exclusivo com a propriedade NavigateUrl (abaixo).

3.  Texto: Esta propriedade é opcional, tipo de cadeia. Utilize esta propriedade para definir o texto que aparece como hiperligação.

4.  NavigateUrl: Esta propriedade é opcional, tipo de cadeia. Utilize esta propriedade para definir o URL de caminho completo que liga hiperligação. Isto é mutuamente exclusivo com a propriedade ObjectReference (acima).

Exemplo:

![](media/rcd-configuration-xml-reference/image049.jpg)

>[!NOTE]
É necessário um GUID válido de um recurso para esta opção para associar. Neste caso, o segundo hyperlink é gerado com um GUID válido. Primeiro pode ser qualquer Web site.


O segmento de código seguinte gera uma hiperligação redirecting:

```
<!--Sample for a hyperlink that redirects page.-->
<my:Control my:Name="RedirectHyperlink" my:TypeName="UocHyperLink" my:Caption="Redirect Hyperlink" my:Description="This is a hyperlink that takes you to other pages.">
     <my:Properties>
          <my:Property my:Name="NavigateUrl" my:Value="http://www.microsoft.com"/>
          <my:Property my:Name="Text" my:Value="Microsoft Home Page"/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that redirect page-->

```

O segmento de código seguinte gera uma hiperligação em que referencia um recurso. A referência explícita pode ser substituída pela expressão de {enlace origem = objeto, caminho = criador} para vincular este com uma origem de dados. Isto pode ser válido apenas quando Gestor de recursos existe e é do valor de tipo de referência.

```
<!--Sample for a hyperlink that reference object-->
<my:Control my:Name="ReferenceHyperlink" my:TypeName="UocHyperLink" my:Caption="Reference Hyperlink" my:Description="This is a hyperlink gives you an object view of the reference object">
     <my:Properties>
          <my:Property my:Name="ObjectReference" my:Value="e4e048b1-9e43-415e-806c-cf44c429c34c"/>
          <my:Property my:Name="Text" my:Value="View a group in FIM 2010."/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that reference object-->
```

### <a name="uocidentitypicker"></a>UocIdentityPicker

Nome: UocIdentityPicker

Descrição: Este controlo é composta por uma caixa Resolve opcional e uma janela de procura. A caixa de resolver opcional é composta por uma caixa de texto opcional para introduzir a identidade, um botão de resolver para resolver a identidade e um botão de procura para solicitar uma janela pop-up de procura. A janela de procurar possibilita que o utilizador seleccione identidades através de um controlo de vista de lista. A identidade selecionada na janela de procurar é apresentada na caixa de resolução.

Propriedades:

1.  Todas as propriedades comuns: Para obter informações sobre esta propriedade, consulte a secção de propriedades comuns deste documento.

2.  UsageKeywords: Esta é uma propriedade de cadeia opcional. Pode definir uma lista de âmbitos de pesquisa para ser utilizado no selecionador de recurso ao fornecer uma lista das palavras-chave de utilização que são suportados pela estrutura SearchScopeConfiguration, onde cada palavra-chave é separado por um (').

3.  Filtro: Esta é uma propriedade de cadeia opcional. O utilizador fornece uma expressão de XPath para analisar o selecionador de recursos para apresentar apenas os itens que se adequam dentro de um âmbito definido. Esta propriedade é mutuamente exclusiva com a propriedade UsageKeywords (acima). Quando o âmbito de pesquisa é aplicado, esta propriedade não tem efeito.

4.  ResultObjectType: Esta é uma propriedade de cadeia opcional. O tipo de recurso é utilizado para compor recursos na lista de caixa de diálogo de pop-up. Isto é utilizado com o filtro para ajudar o Seletor de identidade identificar que tipo de recurso é devolvido pelo filtro e compor os dados em conformidade. Esta propriedade é mutuamente exclusiva com a propriedade UsageKeywords (consultar acima). Quando o âmbito de pesquisa é aplicado, isto não tem qualquer efeito. A cadeia que é aceite para esta propriedade é um nome de tipo de recurso única e válido, por exemplo, pessoa.
    Quando o filtro é esperado para devolver vários tipos de recursos, recursos é utilizado.

5.  PreviewTitle: Este é o título de pré-visualização utilizado numa vista de lista. Para obter informações sobre esta propriedade, consulte a secção de UocListView.

6.  ListViewTitle: Esta é uma propriedade de cadeia opcional. Pode utilizar esta propriedade para definir o texto apresentado na parte superior da vista de lista como um título.

7.  Valor: Esta é uma propriedade de cadeia opcional. Recomenda-se que o enlace com um atributo de esquema para ligar o valor com uma origem de dados.

8.  Modo: Esta é uma propriedade de cadeia opcional. Utilize esta propriedade para definir se um valor pode ser selecionado pelo Seletor de identidade ou podem ser selecionadas várias identidades. SingleResult e MultipleResult são os valores permitidos. Por predefinição, está definido para SingleResult.

9.  ObjectTypes: Esta propriedade é opcional, tipo de cadeia. Pode definir uma lista de tipos de recursos que o utilizador final pode resolver entradas contra na caixa de resolver de Seletor de identidade. A lista é composta por uma lista de nomes de tipo de recurso, separados por vírgulas (,).

10. AttributesToSearch: Esta é uma cadeia-typeproperty opcional. Pode definir uma lista de atributos para ser utilizado para resolver o item no Seletor de identidade, em que a lista é uma lista de atributos de esquema, separados por vírgulas (,). Por exemplo, se AttributesToSearch está definido como DisplayName, Alias, significa que o utilizador é capaz de procurar os itens com o DisplayName = \<procurar valor\> ou Alias =\<procurar valor\>. Recomendamos que os nomes de atributo que são introduzidos aqui ser atributos válidos em tipos de recurso de destino da origem de dados que é sited no valor. Os tipos de recurso de destino podem ser encontrados no campo ObjectTypes. Todos os atributos tem de ser válidos em todos os tipos de recursos específico que são citou no campo ObjectTypes.

11. ColumnsToDisplay: Esta propriedade é opcional, tipo de cadeia. O utilizador fornece uma lista de nomes de atributo de esquema, separados por vírgulas (,). Os atributos que são definidos aqui constituem a coluna da vista de lista no selecionador de identidade.

12. Linhas: Esta propriedade é opcional, número inteiro. Funciona apenas quando o modo está definido como MultipleResult. Utilize esta propriedade para definir a altura da caixa de texto de resolver para um tamanho específico em unidades de caráter.

13. MainSearchScreenText: Esta propriedade é opcional, tipo de cadeia. Este é o texto personalizado que é apresentado enquanto a pesquisa está em execução na janela de procura.

Eventos:

 • SelectedObjectChanged: Este evento é emitido quando o utilizador altera os recursos selecionados.

Exemplo:

![](media/rcd-configuration-xml-reference/image052.png)

>[!NOTE]
Para este exemplo funcione, tem de criar um novo enlace entre o atributo de gestor e qualquer tipo de recurso personalizado que se aplica esta XML.


O segmento de código seguinte gera um Seletor de identidade em modo único utilizando as propriedades de filtro e ResultObjectType como parte do RCDC:

```
<!--Sample for a single-selection identity picker using Filter and Result Object Type-->
<my:Control my:Name="SingleSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A Single Selection Identity Picker" my:Description="The user is allowed to select only one entry here." my:RightsLevel="{Binding Source=rights, Path=Manager}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Manager.Required}"/>
          <my:Property my:Name="Mode" my:Value="SingleResult" />
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--single valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Manager , Mode=TwoWay}" />
          <!--Scoping the list explicitly to All Persons name contains letter "e"-->
          <my:Property my:Name="Filter" my:Value="/Person[contains(JobTitle, 'Manager')]"/>
          <!--Result object type specify the type is Person-->
          <my:Property my:Name="ResultObjectType" my:Value="Person"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select only one entry" />
          <my:Property my:Name="PreviewTitle" my:Value="Entry selected:" />
     </my:Properties>
</my:Control>
<!--End of sample for a single-selection identity picker.-->
```



Exemplo:

![](media/rcd-configuration-xml-reference/image056.jpg)

>[!NOTE]
Para tornar este código de exemplo funcione, tem de vincular o atributo ExplicitMember (um atributo de referência com múltiplos valores) para o tipo de recurso personalizado. Também tem de criar âmbitos de pesquisa com a propriedade de UsageKeyword definida ao grupo e pessoa.


O segmento de código seguinte cria o controlo na figura anterior:

```
<!--Sample for a multiselection Identity Picker uses Search Scope-->
<my:Control my:Name="multiSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A multi Selection Identity Picker" my:Description="The user is allowed to select more than one entry here" my:RightsLevel="{Binding Source=rights, Path=ExplicitMember}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ExplicitMember.Required}"/>
          <my:Property my:Name="Mode" my:Value="MultipleResult" />
          <my:Property my:Name="Rows" my:Value="10" />
          <!--There are existing search scopes that has key word "Person" and "Group" use both sets of search scopes here.-->
          <my:Property my:Name="UsageKeywords" my:Value="Person,Group"/>
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--multi valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExplicitMember , Mode=TwoWay}" />
          <my:Property my:Name="ResultObjectType" my:Value="Resource"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select multiple entries" />
          <my:Property my:Name="PreviewTitle" my:Value="Entries selected" />
     </my:Properties>
</my:Control>
<!--End of sample for a multiselection Identity Picker.-->
```


### <a name="uoclabel"></a>UocLabel

Nome: UocLabel

Descrição: Esta é um simples, como só de leitura, texto etiqueta controlo. Recomendamos que este controlo ser utilizado para apresentar dados só de leitura.

Propriedades:

1.  Todas as propriedades comuns: Para obter informações sobre esta propriedade, consulte a secção de propriedades comuns deste documento.

2.  Texto: Este é um atributo de tipo de cadeia. Defina esta propriedade ao fornecer um valor de cadeia explícita ou através do enlace-la com uma origem de dados. Um enlace de exemplo que atribui walue desta propriedade é {enlace origem = objeto, caminho =\<nome de atributo válido\>.

Para um exemplo do controlo UocLabel, consulte o artigo controlo simple na secção de exemplos simples de controlo.

### <a name="uoclistview"></a>UocListView

Nome: UocListView

Descrição: Este é um controlo de vista de lista avançadas. É composto por uma vista de lista simples, uma procura simples opcional, um controlo de procura avançada opcional, uma caixa de pré-visualização de seleção opcionais e uma barra de botão de ação. A procura simples opcional é composta por um âmbito de pesquisa e uma caixa de texto de procura simples. O controlo de procura avançada tem um construtor de filtros. A vista de lista mostra uma lista prerendered de recursos. Esta operação também pode mostrar os resultados da pesquisa feitos os controlos de procura neste controlo. Barra de botão de ação define que ação pode ser executada com base na seleção da vista de lista. A caixa de seleção pré-visualização mostra os itens selecionados da vista de lista.

>[!IMPORTANT]
UocListView não funciona com atributos de referência de valor único. Pode ser utilizado apenas com atributos de referência com múltiplos valores. Para atributos de referência de valor único, consulte UocIdentityPicker neste documento.


Propriedades:

1.  Todas as propriedades comuns: Para obter informações sobre esta propriedade, consulte a secção de propriedades comuns deste documento.

2.  SelectedValue: Esta propriedade é opcional, tipo de cadeia que, normalmente, está vinculada a um atributo de referência com múltiplos valores aceitar uma lista das cadeias de formato de GUID.

3.  PageSize: Esta é uma propriedade de número inteiro opcional. O utilizador pode especificar o número de entradas se encaixa na página num controlo de vista de lista. O valor predefinido é 10 entradas. Qualquer número inteiro positivo é válido.

4.  UsageKeyword: Esta propriedade é opcional, tipo de cadeia. O utilizador pode especificar uma lista de palavras-chave que definem o âmbito de pesquisa é utilizado no controlo de pesquisa da vista de lista. Existem recursos de âmbito de pesquisa no servidor do FIM 2010. O atributo na estrutura de SearchScopeConfiguration, denominado UsageKeyword, é utilizado para agrupar um conjunto de âmbitos de pesquisa. A vista de lista consome essa lista de palavras-chave. Cada palavra-chave é separado por uma vírgula (,).
    Isto a usagekeyword utilizado no âmbito de pesquisa correspondente que pretende apresentar nesta vista de lista. Esta é apenas em efeito quando ShowSearchControl propriedade está definida como true.

5.  SearchControlAutoPostback: Esta propriedade é opcional booleana. Defina o valor desta propriedade como verdadeiro para efetuar autopostback quando é acionada uma pesquisa. Por predefinição, SearchControlAutoPostback está definido como false.

6.  EmptyResultText: Esta propriedade é opcional, tipo de cadeia. Por predefinição, é definida como não existem itens, mas pode ser definida para qualquer valor de cadeia. Este texto é apresentado quando um resultado de pesquisa está vazio.

7.  ButtonHeight: Esta propriedade é opcional, tipos de números inteiros. Defina o valor desta propriedade como qualquer valor de número inteiro positivo. Esta propriedade define a altura dos botões na barra de ação em pixéis. O valor predefinido é de 32 pixéis.

8.  ButtonWidth: Esta propriedade é opcional, tipos de números inteiros. Defina o valor desta propriedade como qualquer valor de número inteiro positivo. Esta propriedade define a largura dos botões na barra de ação em pixéis. O valor predefinido é de 32 pixéis.

9.  CaptionImageMaxHeight: Esta propriedade é opcional, tipos de números inteiros. Defina o valor desta propriedade como qualquer número inteiro positivo. Esta propriedade define a altura de ícone máximo de uma legenda opcional. O valor predefinido é de 32 pixéis.

10. CaptionImageMaxWidth: Esta propriedade é opcional, tipos de números inteiros. Defina o valor desta propriedade como qualquer número inteiro positivo. Esta propriedade define a largura de ícone máximo de uma legenda opcional. O valor predefinido é de 32 pixéis.

11. CaptionImageUrl: Esta propriedade é opcional, tipo de cadeia. Esta propriedade define um URL que liga a uma imagem que é apresentado como a imagem de legenda.

12. PreviewTitle: Esta propriedade é opcional, tipo de cadeia. Utilize esta propriedade para definir o texto que aparece na parte superior da caixa de pré-visualização de seleção.

13. EnableSelection: Esta é uma propriedade de tipo Booleano opcional. Utilize esta propriedade para definir se uma vista de lista está no modo de seleção. Se uma vista de lista está no modo de seleção, uma coluna de caixas de verificação é apresentada na coluna mais à esquerda da vista de lista e uma caixa de pré-visualização de seleção aparece na parte inferior da vista de lista. O valor predefinido desta propriedade está definido como true.

14. SingleSelection: Esta é uma propriedade de tipo Booleano opcional. Se o modo de seleção está ativado para a vista de lista, a definição deste valor como verdadeiro limita o utilizador final para selecionar apenas um item da lista. Por predefinição, o valor desta propriedade é definido como false. Isto significa que, por predefinição, o utilizador final pode selecionar vários itens na lista.

15. RedirectUrl: Esta propriedade é opcional, tipo de cadeia. Utilize esta propriedade para especificar uma página de redirecionamento quando para um item com hiperligação é clicado na lista. Este URL pode conter os marcadores de posição são substituídos com o valor real durante o tempo de execução. Os marcadores de posição seguem da seguinte forma:

     ◦ {0} - objectType

     ◦ {1} - objectID

     ◦ {2} - displayName

16.  ShowTitleBar: Esta é uma propriedade de tipo Booleano opcional. Utilize esta propriedade para especificar se a barra de título deve estar visível. O valor predefinido desta propriedade é falso.

17.  ShowActionBar: Esta é uma propriedade de tipo Booleano opcional. Utilize esta propriedade para especificar se a área da barra de ação deve estar visível. O valor predefinido desta propriedade é verdadeiro.

18.  ShowPreview: Esta é uma propriedade de tipo Booleano opcional. Utilize esta propriedade para especificar se a área de pré-visualização deve estar visível. O valor predefinido desta propriedade é verdadeiro.

19.  ShowSearchControl: Esta é uma propriedade de tipo Booleano opcional. Utilize esta propriedade para especificar se o controlo de pesquisa deverá estar visível. O valor predefinido desta propriedade é verdadeiro.

20.  ResultObjectType: Esta propriedade é opcional, tipo de cadeia. Utilize esta propriedade para especificar o tipo de objeto esperado dos resultados da pesquisa. O valor predefinido desta propriedade é o recurso. Se o resultado da pesquisa contém vários tipos de recursos, este valor deve ser especificado como recurso.

21.  ColumnsToDisplay: Esta propriedade é opcional. Utilize esta propriedade para especificar quais os atributos que pretende que a vista de lista para serem apresentadas como colunas. O valor predefinido desta propriedade é DisplayName, ResourceType. Cada coluna é representada pelo nome do sistema de um atributo. Cada coluna é separada por uma vírgula (,). Não é necessário especificar um valor para esta propriedade quando a vista de lista é utilizada no modo de seleção. No modo de seleção, a definição de coluna provém do atributo SearchScopeColumn do âmbito de pesquisa que está atualmente selecionado.

22.  ListFilter: Esta propriedade é opcional, tipo de cadeia. Este é o xpath que é utilizado para compor a vista de lista, não sendo apenas em efeito quando ShowSearchControl for definida como false. Quando este valor for especificado, a vista de lista utiliza este valor da propriedade para consultas e a vista de lista não está no modo de seleção. O filtro pode optar por de estar vinculado a um atributo de cadeia de recurso, da seguinte forma:<br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}"/>` <br/> <br/>
     ou ser uma cadeia que contém alguns variável de ambiente predefinidas, da seguinte forma: <br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="/Approval[Request=''%ObjectID%'']"/>`

23.  TargetAttribute: Esta é uma propriedade obsoleta. O valor deve ser o nome do sistema de um atributo referenciado com múltiplos valores. Recomendamos que esta propriedade não utilizada mais. Por exemplo, na gestão de grupo, em vez do seguinte:

  `<my:Property my:Name="TargetAttribute" my:Value="ExplicitMember"/>` <br/>

  Deve ser:`<my:Property my:Name=”ListFilter” my:Value=”/Group[ObjectID=’%ObjectID%’]/ExplicitMember”/>` <br/><br/>
24.  ItemClickBehavior: Esta propriedade é opcional, tipo de cadeia. Utilize esta propriedade para especificar se pretende que o, clique em ver um item da vista de lista para acionar uma nova colocação server ou para apresentar um detalhe do item. São suportados dois valores de opção: ModelessDialog e servidor. O valor predefinido é ModelessDialog.

25.  SearchOnLoad: Esta é uma propriedade opcional, tipo de valor booleano que especifica se o controlo de vista de lista deve consultar a carga. Esta propriedade é aplicável apenas quando a vista de lista está no modo de seleção. O valor predefinido para esta propriedade for VERDADEIRO. Pode desativá-la se espera que o utilizador, normalmente, digite um texto para procurar para obter um resultado significativo. Neste caso, a vista de lista inicialmente mostra uma mensagem a informar o utilizador como efetuar uma pesquisa. O texto pode ser personalizado pelas seguintes propriedades:

26.  MainSearchScreenText: Neste opcional, a propriedade de tipo de cadeia é aplicável apenas quando SearchOnload está definido como true. Esta propriedade pode ser utilizada para personalizar o texto que aparece no meio da vista de lista quando a vista de lista não procurar automaticamente. O valor predefinido para esta propriedade é localizar os recursos pretendidos utilizando a pesquisa acima. Pode especificar um valor para colocar o texto mais relevantes para o seu cenário.

27.  SubSearchScreenText: Neste opcional, propriedade de tipo de cadeia é utilizada para personalizar o texto que aparece abaixo MainSearchScreenText. Normalmente, não é necessário especificar um valor para esta propriedade, a menos que pretenda adicionar algumas instruções adicionais sobre como utilizar a vista de lista.

Para obter exemplos de como utilizar a vista de lista, juntamente com o controlo de UocFilterBuilder como uma lista de pré-visualização, consulte exemplos UocFilterBuilder anteriormente neste documento. Também pode ser utilizado o UocListView sem o construtor de filtros.

### <a name="uocnumericbox"></a>UocNumericBox

Nome: UocNumericBox

Descrição: Esta é uma caixa de texto simples que assume apenas valores de número inteiro. Este controlo suporta o modo só de leitura e o modo atualizável.

Propriedades:

1.  Todas as propriedades comuns: Para obter informações sobre esta propriedade, consulte a secção de propriedades comuns deste documento.

2.  MaxValue: Esta propriedade é opcional, tipos de números inteiros. Utilize esta propriedade para definir uma validação do lado do cliente para o controlo. O valor que o utilizador final introduz não pode exceder este valor. Pode introduzir um número inteiro explícito ou vincular este com dados de número inteiro de uma origem de dados utilizando {enlace origem = esquema caminho = IntegerMaximum}.

3.  MinValue: Esta propriedade é opcional, tipos de números inteiros. Utilize esta propriedade para definir uma validação do lado do cliente para o controlo. O valor que o utilizador final introduz não pode ser inferior a este valor. Pode introduzir um número inteiro explícito ou vincular este com dados de número inteiro de uma origem de dados utilizando o enlace de origem = esquema, caminho = IntegerMinimum}.

4.  DefaultValue: Esta propriedade é opcional, tipos de números inteiros. Utilize esta propriedade para definir um valor predefinido para o controlo se o controlo é utilizado para criar novos dados. Este valor apenas pode estar explicitamente definido como um número inteiro estático.

5.  Valor: Esta propriedade é opcional, tipos de números inteiros. Quando vincular isto com um número inteiro tipo de dados de uma origem de dados, o valor do atributo de que é apresentado quando a página é carregada e, em seguida, são guardado para a origem de dados após submissão.

Processador:

   • TextChanged: Este evento é emitido quando o valor atual no interior do controlo é alterado.

Exemplo:

![](media/rcd-configuration-xml-reference/image061.jpg)

>[!NOTE]
O código de exemplo seguintes gera primeira caixa numérico. A caixa de um valor numérico não está ligada com uma origem de dados ou quaisquer informações de esquema.

```
<!--Sample for an explicit Numeric Box-->
<my:Control my:Name="SampleExplicitNumericBox" my:TypeName="UocNumericBox" my:Caption="An Explicit NumericBox" my:Description="This is a dummy numeric box that is not linked with data source.">
     <my:Properties>
          <my:Property my:Name="MinValue" my:Value="1"/>
          <my:Property my:Name="MaxValue" my:Value="100"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
     </my:Properties>
</my:Control>
<!--End of sample for an explicit Numeric Box.-->

```

O código de exemplo seguintes gera a segunda caixa numérico.

>[!NOTE]
Para este exemplo funcionar, primeiro tem de criar um novo, tipos de números inteiros atributo AnIntegerAttribute chamado e vinculá-lo com o tipo de recurso personalizado.

```
<!--Sample for a dynamically rendered numeric box-->
<my:Control my:Name="SampleDynamicNumericBox" my:TypeName="UocNumericBox" my:Caption="{Binding Source=schema, Path=AnIntegerAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=AnIntegerAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=AnIntegerAttribute}">
     <my:Properties>
          <my:Property my:Name="MaxValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMaximum}"/>
          <my:Property my:Name="MinValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMinimum}"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AnIntegerAttribute, Mode=TwoWay}"/>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.Required}"/>
     </my:Properties>
</my:Control>
<!--End of sample for a dynamically numeric box.-->
```


### <a name="uocpicturebox"></a>UocPictureBox

Nome: UocPictureBox

Descrição: Este controlo é utilizado para compor imagem, dados de tipo binário. Recomendamos que este controlo utilizada com dados de tipo binário. A imagem pode ser composta por um URL de imagem fornecido, dados de tipo binário ou a origem do atributo que contém dados de tipo de imagem.

Propriedades:

1.  Todas as propriedades comuns: Para obter informações sobre esta propriedade, consulte a secção de propriedades comuns deste documento.

2.  ImageUrl: Esta propriedade é opcional, tipo de cadeia. Introduza o URL da imagem de destino.

3.  MaxHeight: Esta propriedade é opcional, cadeia tipo. Define a altura máxima da imagem a compor em pixéis.

4.  MaxWidth: Esta propriedade é opcional, tipo de cadeia. Define a largura máxima da imagem a compor em pixéis.

5.  ImageData: Esta é uma propriedade de tipo binário. Utilize esta propriedade para vincular a uma origem de dados com a imagem apresentada. A origem de dados vinculados tem de ser de binário.
    Também pode utilizar este campo para definir explicitamente uma imagem, fornecendo os dados no formato do byte [].

6.  ImageResource: Esta é uma propriedade opcional, de tipo binário.

7.  AlternativeText: Esta propriedade é opcional, tipo de cadeia. Esta propriedade é apresentada como texto alternativo quando não é possível apresentar a imagem.

Exemplo:

>[!NOTE]
Para utilizar este exemplo, tem de ter um enlace com o controlo de dados de imagem existente.


O segmento de código seguinte gera um controlo de caixa de imagem que está vinculado a uma origem de dados com o controlo:

```
<!--Sample for a Picture Box control binding with a data source-->
<my:Control my:Name="SamplePictureBoxImageData" my:TypeName="UocPictureBox" my:RightsLevel="{Binding Source=rights, Path=Photo}">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageData" my:Value="{Binding Source=object, Path=Photo}" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

O segmento de código seguinte gera um controlo de caixa de imagem que está vinculado a uma imagem de URL com o controlo:

```
<!--Sample for a Picture Box control bind with explicit URL-->
<my:Control my:Name="SamplePictureBoxImageUrl" my:TypeName="UocPictureBox">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageUrl" my:Value="http://www.microsoft.com/dummypicture.jpg" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

### <a name="uocradiobuttonlist"></a>UocRadioButtonList

Nome: UocRadioButtonList

Descrição: Esta é uma simples, lista de botão de opção. As opções são mutuamente exclusivas nesta lista. Este controlo é recomendado quando os utilizadores tem cinco ou menos opções à sua escolha. Caso contrário, é recomendado UOCListView.

Propriedades:

1.  Todas as propriedades comuns: Para obter informações sobre esta propriedade, consulte a secção de propriedades comuns deste documento.

2.  ValuePath: O caminho do valor é definido como valor. Cria um enlace com o campo de valor de elemento opção que está definido neste documento.

3.  CaptionPath: O caminho de valor está definido para a legenda. Cria um enlace com o campo de legenda do elemento opção que está definido neste documento.

4.  HintPath: O caminho do valor é definido para a sugestão. Cria um enlace com o campo de sugestão do elemento de opção que está definido neste documento.

5.  SelectedValue: O valor que está atualmente selecionado. Esta é uma propriedade necessária, do tipo de cadeia. Esta propriedade está vinculado com dados de cadeias da origem de dados.

Eventos:

1.  SelectedIndexChanged

2.  CheckedChanged

Opções:

Só pode existir dois elementos de opção nas opções para este controlo.

1.  Valor: O campo de valor num único elemento opção tem de ser definido como True ou False.

2.  Legenda: Isto pode ser qualquer valor de cadeia.

3.  Sugestão: Isto pode ser qualquer valor de cadeia.

Exemplo:

![](media/rcd-configuration-xml-reference/image063.jpg)

>[!NOTE]
Para este exemplo funcione, tem de criar um novo atributo booleano, ABooleanAttribute e vinculá-lo com o tipo de recurso personalizado.

O segmento de código seguinte cria a lista de botão de opção na figura anterior:

```
<!--Sample for option button list control-->
<my:Control my:Name="SampleRadioButtonList" my:TypeName="UocRadioButtonList" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Options>
          <my:Option my:Value="False" my:Caption="Set Value To False" my:Hint="By selecting this option, you are setting the value of the attribute to false." />
          <my:Option my:Value="True" my:Caption="Set Value To True" my:Hint="By selecting this option, you are setting the value of the attribute to true." />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for option button list control-->
```


### <a name="uocsimpleradiobutton"></a>UocSimpleRadioButton


Nome: UocSimpleRadioButton

Descrição: Esta é um simples, controlo de botão de opção. A utilização deste controlo é semelhante a uma caixa de verificação simple. Existem dois botões de opção, que mostra side by side with etiquetas de texto. É recomendado o controlo de enlace para dados de tipo Booleano.

Propriedades:

1.  Todas as propriedades comuns: Para obter informações sobre esta propriedade, consulte a secção de propriedades comuns deste documento

2.  TrueText: Esta propriedade é opcional, tipo de cadeia. Este é o texto que aparece quando o botão de opção está selecionado.

3.  FalseText: Esta propriedade é opcional, tipo de cadeia. Este é o texto que aparece quando o botão de opção não estiver seleccionado.

4.  SelectedItem: Esta é uma propriedade de tipo Booleano opcional. Este valor indica se o botão de opção está selecionado. Isto é possível vincular com dados de tipo Booleano de uma origem de dados. O valor predefinido é definido como falso.

Eventos:

   • CheckedChanged: quando as alterações de botão de opção estado a partir de selecionado para desmarcada, ou o oposto, este sinal é emitido.

Exemplo:

![](media/rcd-configuration-xml-reference/image066.png)

>[!NOTE]
Para tornar o exemplo funcionar, tem de criar um novo atributo booleano ABooleanAttribute e vinculá-lo ao seu tipo de recurso personalizado. Os dados RCDC são aplicados o mesmo tipo de recurso personalizado.

O segmento de código seguinte gera o botão de opção na figura anterior:

```
<!--Sample for simple option button control-->
<my:Control my:Name="SampleSimpleRadioButton" my:TypeName="UocSimpleRadioButton" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="FalseText" my:Value="False"/>
          <my:Property my:Name="TrueText" my:Value="True"/>
          <my:Property my:Name="SelectedItem" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for simple option button control-->
```


### <a name="uoctextbox"></a>UocTextBox

Nome: UocTextBox

Descrição: Esta é uma caixa de texto simples que suporta o tipo de cadeia de entrada. Recomendamos que utilize este controlo para o enlace com dados de tipo de cadeia.

Propriedades:

1.  Todas as propriedades comuns: Para obter informações sobre esta propriedade, consulte a secção de propriedades comuns deste documento.

2.  MaxLength: Este é um atributo opcional, do tipo número inteiro. Esta propriedade especifica o comprimento máximo para uma cadeia de entrada. O valor predefinido para esta propriedade é de 128 carateres.

3.  Texto: Esta propriedade é opcional, tipo de cadeia. Este é o texto que aparece na caixa de texto. Pode definir uma cadeia explícita, que é apresentado na caixa de texto durante o carregamento inicial do controlo ou vinculá-lo para um atributo de esquema de um tipo de cadeia.

4.  Linhas: Esta propriedade é opcional, tipos de números inteiros. Esta propriedade define a altura da caixa de texto em unidades de caráter. O valor predefinido é 1 caráter.

5.  Colunas: Esta propriedade é opcional, tipos de números inteiros. Esta propriedade define a largura da caixa de texto em unidades de caráter. O valor predefinido é 20 carateres.

6.  Moldagem: Esta é uma propriedade de tipo Booleano opcional. Ao definir o valor desta propriedade como VERDADEIRO, o utilizador ativa a funcionalidade de encapsular a palavra de mensagens em fila na caixa de texto. O valor predefinido desta propriedade está definido como true.

7.  UniquenessValidationXPath: Esta propriedade é opcional, tipo de cadeia. Este utiliza uma expressão de filtro FIM XPath válida e assegura que o valor de entrada pelo utilizador é exclusivo nos recursos que estão no âmbito do filtro.
    Por exemplo, para garantir que o utilizador solicitou nome a apresentar é exclusivo no todos os grupos de segurança de correio ativado na base de dados do serviço FIM, teria de utilizar o XPath `/Group[DisplayName=’%VALUE%’ and Type=’MailEnabledSecurity’`. A ação de validação é executada quando o utilizador saia da página. Esta propriedade só é suportada em RCDC para a criação de um recurso.

8.  UniquenessErrorMessage: Esta propriedade é opcional, tipo de cadeia. Esta cadeia é utilizada para apresentar uma mensagem de erro se a validação de UniquenessValidationXPath falhar, e pode ser texto explícito ou uma cadeia de recurso de variável. Se esta propriedade não for especificada, a mensagem de erro predefinida para uma validação falhada é "% valor % já existe. Tente um diferente."

Eventos:

   • TextChanged: Este evento é emitido quando o texto dentro da caixa de texto é alterado.

Consulte a secção de amostras de controlo de simples para um exemplo completo deste controlo.

## <a name="appendix-a-default-xsd-schema"></a>Apêndice a: esquema XSD predefinido

Segue-se o esquema XSD completo para todas as RCDCs de predefinidos que são fornecidos com o Microsoft Identity Manager 2016 SP1:

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<xsd:schema targetNamespace="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:my="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xd="http://schemas.microsoft.com/office/infopath/2003" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <xsd:attribute name="TypeName" type="my:requiredString"/>
    <xsd:attribute name="Name" type="my:requiredAlphanumericString"/>
    <xsd:attribute name="Parameters" type="xsd:string"/>
    <xsd:attribute name="DisplayAsWizard" type="xsd:boolean"/>
    <xsd:attribute name="Caption" type="xsd:string"/>
    <xsd:attribute name="AutoValidate" type="xsd:boolean"/>
    <xsd:attribute name="Enabled" type="xsd:string"/>
    <xsd:attribute name="Visible" type="xsd:string"/>
    <xsd:attribute name="IsSummary" type="xsd:boolean"/>
    <xsd:attribute name="IsHeader" type="xsd:boolean"/>
    <xsd:attribute name="HelpText" type="xsd:string"/>
    <xsd:attribute name="Link" type="xsd:string"/>
    <xsd:attribute name="Description" type="xsd:string"/>
    <xsd:attribute name="ExpandArea" type="xsd:boolean"/>
    <xsd:attribute name="Hint" type="xsd:string"/>
    <xsd:attribute name="AutoPostback" type="xsd:string"/>
    <xsd:attribute name="RightsLevel" type="my:requiredString"/>
    <xsd:attribute name="Value" type="xsd:string"/>
    <xsd:attribute name="Handler" type="my:requiredString"/>
    <xsd:attribute name="ImageUrl" type="xsd:string"/>
    <xsd:attribute name="RedirectUrl" type="xsd:string"/>
    <xsd:attribute name="ClickBehavior" type="xsd:string"/>
    <xsd:attribute name="EnableMode" type="xsd:string"/>
    <xsd:attribute name="ValueType" type="xsd:string"/>
    <xsd:attribute name="Condition" type="xsd:string"/>
    <xsd:element name="ObjectControlConfiguration">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:Panel"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="ObjectDataSource">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="XmlDataSource">
        <xsd:complexType  mixed="true">
      <xsd:sequence>
        <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
      </xsd:sequence>
      <xsd:attribute ref="my:Name"/>
      <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Panel">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:DisplayAsWizard"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:AutoValidate"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Grouping">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:IsHeader"/>
            <xsd:attribute ref="my:IsSummary"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Help">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:HelpText"/>
            <xsd:attribute ref="my:Link"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Control">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:ExpandArea"/>
            <xsd:attribute ref="my:Hint"/>
            <xsd:attribute ref="my:AutoPostback"/>
            <xsd:attribute ref="my:RightsLevel"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="CustomProperties">
        <xsd:complexType mixed="true">
            <xsd:sequence>
                <xsd:any minOccurs="0" maxOccurs="unbounded" namespace="##targetNamespace" processContents="lax"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Options">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
            </xsd:sequence>
            <xsd:attribute ref="my:ValueType"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Option">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Buttons">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Button" minOccurs="1" maxOccurs="8"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Button">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                    <xsd:attribute ref="my:ImageUrl"/>
                    <xsd:attribute ref="my:ClickBehavior"/>
                    <xsd:attribute ref="my:RedirectUrl"/>
                    <xsd:attribute ref="my:Enabled"/>
                    <xsd:attribute ref="my:Visible"/>
                    <xsd:attribute ref="my:EnableMode"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Properties">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Property">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Condition"/>                 
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Events">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Event">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
          <xsd:attribute ref="my:Parameters"/>
        </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:simpleType name="requiredString">
        <xsd:restriction base="xsd:string">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
  <xsd:simpleType name="requiredAlphanumericString">
    <xsd:restriction base="xsd:string">
      <xsd:pattern value="[A-Za-z0-9_]{1,128}"/>
    </xsd:restriction>
  </xsd:simpleType>
    <xsd:simpleType name="requiredAnyURI">
        <xsd:restriction base="xsd:anyURI">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="requiredBase64Binary">
        <xsd:restriction base="xsd:base64Binary">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:schema>
```



## <a name="appendix-b-default-summary-xsl"></a>Apêndice b: predefinição resumo XSL

Segue-se a concluída resumo XSL que é fornecido com o Microsoft Identity Manager 2016 SP1:
```XML
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:msxsl="urn:schemas-microsoft-com:xslt">
  <xsl:template name="output-attribute-value">
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <xsl:choose>
      <xsl:when test="$type='Binary'">
        <xsl:value-of select="$attribute" disable-output-escaping="yes"/>
      </xsl:when>
      <xsl:when test="$type='Text'">
        <xsl:text xml:space="preserve" disable-output-escaping="yes">(text data)</xsl:text>
      </xsl:when>
      <xsl:otherwise>
        <xsl:value-of select="translate($attribute,' ','&#160;')" disable-output-escaping="yes"/>
      </xsl:otherwise>
    </xsl:choose>
  </xsl:template>

  <xsl:template name="output-modified-value">
    <xsl:param name="name"/>
    <xsl:param name="attribute1"/>
    <xsl:param name="text1"/>
    <xsl:param name="attribute2"/>
    <xsl:param name="text2"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template name="output-localized-attribute-value">
    <xsl:param name="locale"/>
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template match="/">
    <xsl:choose>
      <xsl:when test="ModifiedAttributes[@ActionType='Create']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Create">
          <Attribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the value]"/>
          other <Attribute> elements
        </ModifiedAttributes>
        -->
        <table cellspacing="0" cellpadding="3" class="commonSummaryListViewGridBorder">
          <tr align="left" class="listViewHeader" style="height:22px;">
            <th class="commonSummaryListViewHeaderCellBR">Attribute</th>
            <th class="commonSummaryListViewHeaderCellBR">Value</th>
          </tr>
          <xsl:for-each select="ModifiedAttributes/Attribute">
            <xsl:sort select="@DisplayName" order="ascending"/>
            <tr class="listViewRow" style="height:22px;">
              <xsl:if test="position() mod 2 != 0">
                <td class="commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
              <xsl:if test="position() mod 2 != 1">
                <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
            </tr>
          </xsl:for-each>
        </table>
      </xsl:when>
      <xsl:when test="ModifiedAttributes[@ActionType='Modify']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Modify">
          <SingleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the old value]" SetValue="[the new value]"/>
          other <SingleAttribute> elements
          <MultipleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InsertedItem="[inserted items separated by ';']" RemovedItem="[removed items separated by ';']"/>
          other <MultipleAttribute> elements
        </ModifiedAttributes>
        -->
        <table class="commonSummaryListViewGridBorder" cellspacing="0" cellpadding="3">
          <xsl:if test="ModifiedAttributes[count(SingleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Single-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
              <th class="commonSummaryListViewHeaderCellBR">New Value</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/SingleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="listViewRow">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" cellpadding="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@InitializedValue"/>
                  <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@SetValue"/>
                  <xsl:with-param name="text2">(value removed)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
          <xsl:if test="ModifiedAttributes[count(MultipleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Multiple-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
              <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/MultipleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="uocSummaryTitleTR">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@RemovedItem"/>
                  <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@InsertedItem"/>
                  <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
        </table>
      </xsl:when>
    </xsl:choose>
  </xsl:template>
</xsl:stylesheet>
```
