---
title: Configuração do ecrã de controlo de recursos XML referência Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 0eedee975a785bd20ee37c85262a0c5f678b09b5
ms.sourcegitcommit: 89511939730501458295fc8499490b2b378ce637
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2021
ms.locfileid: "98010715"
---
# <a name="resource-control-display-configuration-xml-reference"></a>Configuração do ecrã de controlo de recursos Referência XML

Os recursos de configuração do ecrã de controlo de recursos (RCDC) são recursos definidos pelo utilizador que pode utilizar para controlar como outros recursos na loja de dados SP1 (MIM) do Microsoft Identity Manager 2016 (MIM) aparecem na interface do utilizador (UI) para o utilizador final. Cada recurso RCDC contém um ficheiro de configuração XML que pode alterar para adicionar, modificar ou remover o texto de UI e os controlos de UI. Enquanto o MIM 2016 SP1 fornece vários recursos RCDC predefinidos, também pode criar recursos RCDC personalizados para recursos personalizados. Para obter mais informações sobre a utilização do UI RCDC no Portal FIM, consulte [Introdução à Configuração e Personalização do Portal FIM](/previous-versions/mim/ee534913(v=ws.10)) na documentação FIM.


## <a name="known-issues"></a>Problemas conhecidos

O valor predefinido em muitos controlos RCDC não é suportado.

Nesta versão, não é suportada a definição de valores predefinidos nos controlos de um controlo de recursos, exceto no controlo do botão de opção. Pode contornar este problema para uma caixa de entrega, especificando um valor predefinido que não esteja associado a qualquer valor para forçar o utilizador a alterar a seleção. Para resolver este problema com outros controlos, é necessário utilizar um fluxo de trabalho de autorização para fornecer um valor predefinido durante a apresentação do pedido.

## <a name="basic-structure"></a>Estrutura básica

Os dados XML de um recurso RCDC consistem num único elemento **XML de ObjectControlConfiguration.**

>[!NOTE]
>Para obter o esquema XSD completo, consulte <a href="#appendix-a">o esquema do Apêndice A: Esquema XSD predefinido</a>.

Segue-se o esquema XSD para o elemento **ObjectControlConfiguration:**

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

O elemento **ObjectControlConfiguration** contém os seguintes elementos:

- **ObjectDataSource**: Este elemento especifica o Nome de Um Nome de uma classe de fonte de dados que o Controlo de Recursos (RC) utiliza. Para uma descrição e definição de esquema, consulte a seguinte secção de fontes de dados neste documento. Um elemento **objectControlConfiguration** pode conter até 32 nós do elemento **ObjectDataSource.**

- **XmlDataSource**: Esta é uma fonte de dados simples que é mais utilizada para especificar o design de uma página de resumo. Para uma descrição e definição de esquema, consulte a seguinte secção de fontes de dados neste documento. Uma **Configuração objectcontrol:** o elemento pode conter até 32 nós do elemento **XmlDataSource.**

- **Painel**: O administrador pode personalizar o layout da página RCDC modificando elementos dentro dos elementos do Painel. Para mais informações, consulte a secção painel mais tarde neste documento. Um elemento **deconfiguação do controlo de objetos** deve ter apenas um elemento de painel.

- **Eventos**: Os administradores não podem fornecer código personalizado por trás, esta funcionalidade é limitada. Este é o Evento que um painel ou um controlo pode emitir, com base numa mudança de estado. Para mais informações, consulte a secção Eventos mais tarde neste documento. Um elemento **de configuração objectcontrol** pode conter opcionalmente um elemento **de Evento.** Em geral, o uso de **eventos personalizados** não é suportado a menos que especificamente desenvolvido em melhorias posteriores.

## <a name="data-sources"></a>Origens de dados

O Microsoft Identity Manager utiliza fontes de dados como forma de ligar dados a componentes de UI. Isto ajuda a facilitar a separação dos dados da camada de apresentação. Existem dois tipos de fontes de dados nos dados de configuração de recursos RCDC: **ObjectDataSource** e **XmlDataSource**.

-   **ObjectDataSources** especifica uma classe Microsoft .NET que fornece os dados ao RC. Existe um conjunto fixo de tipos disponíveis de ObjectDataSources desde que o administrador possa optar por consumir ao autorizar RCDCs.

-   **Os XMLDataSources** fornecem uma forma simples de estruturar dados baseados em XML, e podem ser usados por administradores para fornecer dados personalizados. Os dados XML devem ser especificados diretamente no RCDC, a menos que utilize a estrutura XML incorporada e predefinida. A estrutura XML incorporada é utilizada para gerar páginas sumárias no RC.

No RCDC, pode ligar estas fontes de dados a atributos dos controlos de UI especificados no RCDC para gerar a UI.

### <a name="objectdatasource-elements"></a>Elementos ObjectDataSource

O gestor de identidade da Microsoft fornece os tipos comuns de origem de dados na tabela seguinte que estão disponíveis para todos os tipos de recursos (exceto quando indicado).

| Nome tipo | Descrição | Encadernação bidireccion ao dos dois sentidos | Sintaxe de ligação |
|---|---|---|---|
| PrimaryResourceObjectDataSource | Isto representa o recurso FIM 2010 que está a ser criado, editado ou visualizado. O caminho na corda de encadernação é o nome do atributo. O tipo de recurso é especificado pelo atributo TargetObjectType do RCDC e não no atributo RCDC.ConfigurationData. | Sim | `[AttributeName]` valor do atributo objeto dado pelo seu nome. |
| PrimaryResourceDeltaDataSource  | Esta fonte de dados constrói o Delta XML que compara o estado original e o estado atual do recurso FIM 2010. O Delta XML gerado é consumido pelo controlo sumário RC para tornar a UI a pedido que o utilizador está a enviar. | Não | `DeltaXml` Isto é usado com o controlo de resumo para exibir o delta. |
| PrimaryResourceRightsDataSource | Esta fonte de dados fornece os direitos inline para cada atributo do recurso FIM 2010. Isto permite ao RC determinar antes da submissão quais as permissões que o utilizador tem nesse atributo e, em seguida, tornar a UI para esse atributo adequadamente. | Não | `[AttributeName]` |
| SchemaDataSource                | Esta fonte de dados pode ser usada para aceder a informações relacionadas com esquemas, tais como nome de exibição, descrição, se o atributo é ou não necessário, bem como informações do tipo de recurso. | Não | `[AttributeName].Required` Valor booleano que indica se o atributo deve ter um valor a ser válido. <br/> `[AttributeName].DisplayNameString` Valor indicando o nome de exibição da ligação. <br/> `[AttributeName].DescriptionString` Valor indicando a descrição da encadernação. <br/>`[AttributeName].StringRegexString` valor que indica String Regex de uma ligação. <br/> `[AttributeName].DisplayName` <br/> `[AttributeName].Description` <br/> `[AttributeName].IntegerValueMinimum` <br/>`[AttributeName].IntegerValueMaximum` <br/>`[AttributeName].LocalizedAllowedValues` |
| Fonte de Dados de Domínio                | Esta fonte de dados fornece uma enumeração de domínios, com base nos recursos de configuração de domínio. Esta fonte de dados só pode ser utilizada em RCDCs que são para recursos de grupo e recursos de utilizador. | Sim | Domínio |

Segue-se um exemplo de snippet RCDC que liga três fontes de dados ao controlo UocTextBox para editar o atributo Descrição de um grupo:

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

### <a name="xmldatasource-element"></a>Elemento XMLDataSource

Ao utilizar um elemento **XMLDataSource,** pode especificar dados personalizados que o RCDC pode consumir para um determinado recurso. Neste caso, os dados XML devem ser especificados no CCDC. Como alternativa, esta fonte de dados pode ser usada para referenciar uma estrutura de dados XML incorporada para tornar a UI para páginas sumárias. Controla o tipo de **XMLDataSource** a utilizar quando o define no RCDC.


| Nome tipo | Descrição | Encadernação bidireccion ao dos dois sentidos | Sintaxe de ligação |
|---|---|---|---|
| **XMLDataSource**            | A fonte de dados representa dados XML. Os dados podem estar em formatos XSL ou XSL incorporados:<ul><li>Formato XSL em Microsoft.IdentityManagement.WebUI.Controls.dll:<br/>```<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement. WebUI.Controls.Resources.DefaultSummary.xsl"> </my:XmlDataSource>```</li><li>Formato XSL incorporado:<br/>```<my:XmlDataSource my:Name="RequestStatusTransformXsl"><xsl:stylesheet version="1.0" xmlns:xsl=http://www.w3.org/1999/XSL/Transform xmlns:msxsl="urn:schemas-microsoft-com:xslt"></xsl:stylesheet></my:XmlDataSource>```</li></ul>  | Não | `Xpath[;namespaces]`onde `Xpath` é um xpath XML válido para selecionar a nota requerida, na maioria das vezes "/" (raiz). `namespaces` é uma lista opcional de cordas prefix=URI. A corda é delimitada por pontos e vírguis, conforme necessário para que o Xpath funcione contra os nomes XML. |
| **ReferênciaDeltaDataSource** | A fonte de dados representa deltas de atributos de referência multivalorizado. É utilizado apenas em RCDC para grupo e conjunto. <br/> Embora a fonte de dados não se limite a Grupos ou Conjuntos, requer alterações de código no hospedeiro RCDC para submeter tais deltas. Atualmente, Grupo e Conjunto são os únicos anfitriões que reconhecem esta fonte de dados.  | Sim | `[AttributeName].Add` onde `[AttributeName]` representa um atributo de referência e os dados devolvidos são as adições delta.<ul><li>Exemplo: `[ReferenceAttribute].Add`</li><li>Exemplo: `<my:Property my:Name="Value" my:Value="{Binding Source=delta, Path=ExplicitMember.Add, Mode=TwoWay}"/>`</li></ul>`[AttributeName].Remove` onde `[AttributeName]` representa um atributo de referência e os dados devolvidos são as remoção delta. <br/> DeltaXml <!-- Is bold formatting needed for DeltaXml? --> |
|**PedidoDetailsDataSource**| A fonte de dados representa o atributo RequestParameter de objetos Request. O parâmetro define o número máximo de valores de atributos a exibir por atributo multivalorizado. É utilizado apenas no RCDC para Pedido. `<my:ObjectDataSource my:TypeName="RequestDetailsDataSource" my:Name="requestDetails" my:Parameters="1000" />`| Não | DeltaXml |
|**RequestStatusDataSource**| A fonte de dados representa o atributo **RequestStatusDetails** de objetos Request. É utilizado apenas no RCDC para Pedido. | Não | DeltaXml |

Para definir uma fonte de dados XML personalizada, utilize o seguinte XML:

 ```XML
<my:XmlDataSource my:Name="MyCustomData" >
     %Insert custom, properly formatted XML data here%
</my:XmlDataSource>
```

Para utilizar o controlo de resumo incorporado XSL, defina a fonte de dados da seguinte forma:

```XML
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />
```

Se estiver a criar um RCDC para um tipo de recurso personalizado, pode utilizar este método para tornar automaticamente uma página de resumo para esse recurso personalizado.

O seguinte é um exemplo de como criar um separador resumo no RCDC, utilizando o elemento **PrimaryResourceDeltaDataSource** com o elemento **XMLDataSource** utilizando o XSL incorporado:

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

Como alternativa, o utilizador pode substituir o elemento XmlDataSource especificado anteriormente pelo seguinte formato para definir um layout personalizado de uma página de resumo. Como referência, o Resumo XSL final 2010 padrão está incluído no Apêndice B: Resumo Padrão XSL, mais tarde neste documento.

```XML
<my:XmlDataSource my:Name="summaryTransformXsl">
     Insert valid XSL code here
</my:XmlDataSource>
```
### <a name="schema-for-data-sources"></a>Esquema para fontes de dados
O seguinte esquema XSD gera os dois tipos de fontes de dados:

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

## <a name="event-element"></a>Elemento de evento
Um elemento **do Evento** define o estado de mudança de um Controlo. A extensibilidade desta funcionalidade é limitada porque não é possível escrever uma função personalizada (Handler) para definir qual é o comportamento após o desencadear de um evento. O mesmo elemento Evento pode ser utilizado no elemento Painel. Para mais informações, consulte a secção painel mais tarde neste documento.

Segue-se o esquema XSD para o elemento Evento:

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

Um **Evento** é um elemento vazio, e tem os seguintes atributos:

- **Nome**: Este é o nome único de um evento. O único evento suportado no **ObjectControlConfiguration** é o evento Load. Este evento é desencadeado quando a página é carregada pela primeira vez.

- **Handler:** Este é o nome único de um manipulador. Quando o evento é desencadeado, normalmente um método de programa é chamado para lidar com a mudança do estado do controlo. Os seguintes casos não são apoiados:

   - Retirando um manipulador existente de um controlo existente.
   - Criar um novo manipulador.
   - Fixação de um manipulador a um comando existente ou novo.

Segue-se um exemplo de um elemento **de Eventos:**

```XML
<my:Events>
    <my:Event my:Name="Load" my:Handler="OnLoad"/>
</my:Events>
```

## <a name="panel-element"></a>Elemento do painel
O elemento **painel** é o elemento central de um layout RCDC. Segue-se o esquema XSD para o elemento Painel:

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

O elemento **painel** contém um elemento recorrente, **Agrupamento**. Para mais informações, consulte a secção de Agrupamentos neste documento.

O elemento Painel tem os seguintes atributos:

- **Nome**: O nome do Painel. Este é um atributo necessário, tipo corda.

- **DisplayAsWizard**: Este atributo está atualmente precotado. O atributo VerbContext correspondente no RCDC rege-se se o layout do recurso estiver no modo Assistente ou no modo Separador. Se estiver definido para 0 (Modo Criar), também está no modo Wizard. Caso contrário, encontra-se no modo Separador. Para mais informações, consulte Introdução à Configuração e Personalização do Portal FIM na documentação.

- **Legenda**: Este atributo está atualmente deprecado. O utilizador pode especificar legendas para uma página, incluindo um Grupo que contém apenas informações de cabeçalho. Para mais informações, consulte a secção de Agrupamentos neste documento.

- **AutoValidate**: Este é um atributo Boolean opcional. Quando é definido para a verdadeira validação é acionado contra cada controlo na aba atual. Por padrão, se o atributo faltar, é definido como verdadeiro. Pode ser usado em combinação com a propriedade RegularExpression. Para obter mais informações, consulte "RegularExpression" numa secção posterior deste documento.

## <a name="grouping-element"></a>Elemento de agrupamento
O elemento **agrupamento** define o layout geral de um Painel. Funciona como um recipiente que agruba controlos individuais em diferentes secções e separadores. Segue-se o esquema XSD para o elemento agrupamento:

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

Existem três tipos de **elementos de agrupamento:**

- **Agrupamento de cabeçalho**: Um Agrupamento de Cabeçalho é opcional. Só pode haver um Agrupamento de Cabeçalho num **Painel**. Um Agrupamento de Cabeçalho aparece em cima de um painel como legenda. Apenas um UocCaptionControl pode ser utilizado neste agrupamento. Para obter um exemplo de agrupamento de cabeçalho, consulte a secção amostra.

- **Agrupamento de conteúdos**: É necessário pelo menos um Agrupamento de Conteúdos. Pode haver vários Agrupamentos de Conteúdo num Painel. Um Agrupamento de Conteúdos aparece como o conteúdo principal de uma página RCDC. Cada Agrupamento de Conteúdos aparece como um separador no mesmo Painel e pode conter de 1 a 256 controlos. Consulte a secção Exemplos de Um Agrupamento de **Conteúdos**.

- **Agrupamento de resumo**: Um Agrupamento de Resumos é opcional. Só pode haver um Agrupamento de Resumos num Painel. Um Agrupamento de Resumo aparece como o último separador de um Painel. Apenas um controlo **UocHtmlSummary** pode ser utilizado num Agrupamento Sumário para visualizar as alterações que o utilizador escoou antes de apresentar um pedido. Consulte a secção Exemplos de Um Agrupamento de Resumos.

Cada tipo de agrupamento contém os seguintes elementos:

- **Ajuda**: Este elemento fornece texto de ajuda num separador. Também pode usá-lo para adicionar um link a um ficheiro ajuda para o separador.

- **Controlos**: Para obter informações sobre este elemento, consulte a secção controlo neste documento. Cada agrupamento deve ter 1 a 256 controlos de forma inclusiva, dependendo do tipo de agrupamento.

- **Eventos**: Para obter informações sobre este elemento, consulte a secção Eventos neste documento. Cada agrupamento pode, como opção, ter um Evento. Os Eventos que são suportados num elemento de agrupamento são os seguintes:

    - **BeforeLeave**: Este Evento é acionado quando o utilizador está pronto para deixar um separador num agrupamento de conteúdos.
    - **AfterEnter**: Este Evento é acionado quando o utilizador está pronto para introduzir um separador num agrupamento de conteúdos.

Um Agrupamento pode conter os seguintes sete atributos:

- **Nome**: Este é o nome exigido do Agrupamento. O **nome** deve ser único no **painel**.

- **Legenda**: A **legenda aparece** como a legenda do cabeçalho num Agrupamento de Cabeçalho. Aparece como a legenda do separador de um agrupamento de Conteúdos ou Resumo.

- **Descrição**: Um atributo de corda opcional, **Descrição** só funciona quando é utilizado num Agrupamento de Conteúdos. Utilize este elemento para dar ao utilizador final alguns detalhes sobre as informações dentro do mesmo separador.

    >[!NOTE]
    >Se este atributo for utilizado num Agrupamento De Resumo, o XML é considerado inválido. Se este atributo for utilizado num Agrupamento de Cabeçalho, o XML é considerado válido, mas ignorado.
    >

- **Ativado**: Um atributo Boolean opcional, ativado é definido como verdadeiro quando está em falta. Se o Ativo estiver definido como falso, o utilizador final vê um separador Desativado. Este atributo é funcional apenas num agrupamento de conteúdos.

    >[!NOTE]
    >Se este atributo for utilizado num Agrupamento De Resumo, o XML é considerado inválido. Se este atributo for utilizado num Agrupamento de Cabeçalho, o XML é considerado válido, mas ignorado.
    >

- **Visível**: Pode ocultar um separador de página RCDC ou a sua posição definindo este atributo como falso. Por padrão, este atributo opcional, tipo Boolean, é definido como verdadeiro. Este atributo é funcional apenas num Agrupamento de Conteúdos.

    >[!NOTE]
    >Quando existe apenas um Agrupamento de Conteúdos num Painel, esta funcionalidade não funciona. Quando há mais de um Agrupamento de Conteúdos num Painel, comporta-se como descrito anteriormente.

- **IsHeader**: Este atributo é um atributo opcional, Boolean que define se o Agrupamento é um Agrupamento de Cabeçalhos. Se este atributo não for especificado, é definido como falso.

- **IsSummary**: Este é um atributo opcional, Boolean que define se o Agrupamento é um agrupamento Sumário. Se este atributo não for especificado, é definido como falso.

### <a name="examples-for-types-of-grouping-elements"></a>Exemplos para tipos de elementos de agrupamento
Esta secção contém exemplos para o elemento Agrupamentos.

#### <a name="example-header-grouping"></a>Exemplo: Agrupamento de cabeçalho
O seguinte número mostra uma amostra do Agrupamento de Cabeçalho:

![Agrupamento de cabeçalho](media/rcd-configuration-xml-reference/image005.jpg)

O XML seguinte gera uma amostra de Agrupamento de Cabeçalho. No XML, o Agrupamento de Cabeçalhos é a área com o texto de legenda "Agrupamento de Cabeçalho de amostra".

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

#### <a name="example-content-grouping"></a>Exemplo: Agrupamento de conteúdos
O seguinte número mostra uma amostra de agrupamento de conteúdos:

![Agrupamento de conteúdos](media/rcd-configuration-xml-reference/image007.jpg)

O XML seguinte gera uma amostra de Agrupamento de Conteúdos. No XML, o Agrupamento de Conteúdos é a área com o texto da legenda "Sample Content Grouping".

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

#### <a name="example-summary-grouping"></a>Exemplo: Agrupamento sumário

O seguinte número mostra uma amostra de agrupamento de resumo:

![Agrupamento de resumos](media/rcd-configuration-xml-reference/image010.jpg)

O XML seguinte gera uma amostra de Agrupamento de Resumo. No XML, o Agrupamento Sumário é a área com o texto da legenda "Agrupamento de Resumo da Amostra".

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

### <a name="help-element"></a>Elemento de ajuda
O elemento **de ajuda** pode ser incluído num elemento de agrupamento ou de controlo como elemento opcional. Se for utilizado num agrupamento, deve ser o primeiro elemento utilizado. Fornece ajuda textual aos utilizadores finais para ajudá-los a fornecer informações precisas. O seguinte esquema XSD é para o elemento ajuda:

```XML
<xsd:element name="Help">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:HelpText"/>
          <xsd:attribute ref="my:Link"/>
     </xsd:complexType>
</xsd:element>
```

O seguinte código de amostra XML gera um elemento de Ajuda:

```XML
<my:Help my:HelpText="Some Help Text for Group Basic Info" my:Link="03e258a0-609b-44f4-8417-4defdb6cb5e9.htm#bkmk_grouping_GroupingBasicInfo" />
```

### <a name="control-element"></a>Elemento de controlo
Um elemento de agrupamento contém um ou mais elementos **de controlo.** Os controlos são os principais elementos de um RCDC. Pode personalizar o elemento agrupamento definindo os diferentes elementos de Controlo que contém. O seguinte esquema XSD destina-se ao elemento Controlo:

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

Um elemento de controlo contém os seguintes elementos:

- **Ajuda:** Este elemento é ignorado. Funciona apenas no Agrupamento.

- **Ofertas Personalizadas**: Este elemento não é suportado.

- **Opções**: Este elemento é utilizado apenas em combinação com os controlos **UocDropDownList** ou **UocRadioButtonList.** Não é funcional com outros Controlos. Consulte a secção Opções neste documento para a estrutura deste elemento. Consulte a secção de controlo individual deste documento para ver como as opções são utilizadas por um controlo.

- **Botões**: Este elemento é utilizado apenas em combinação com o **Controlo UocListView.** Não é funcional para outros controlos. Para mais informações, consulte a secção UocListView neste documento.

- **Propriedades**: Este elemento é utilizado em todos os Controlos para especificar comportamentos adicionais de um Controlo. Para obter informações sobre este elemento, consulte a secção Propriedades neste documento.

- **Eventos**: Para a estrutura deste elemento, consulte a secção Eventos anteriormente neste documento. Consulte a secção de controlo individual deste documento para ver quais os eventos utilizados num controlo.

Um elemento de controlo pode conter os seguintes 10 atributos:

- **Nome:** Este é o nome do comando. O nome de um Controlo deve ser único dentro de cada painel. Este é um atributo necessário, tipo corda.

- **TipoName**: Este atributo especifica que tipo de Controlo é. Este é um atributo necessário, tipo corda. Consulte a secção Controlos Individuais neste documento para cada nome de controlo.

- **Legenda**: Pode utilizar este atributo para incluir uma legenda para o controlo. A legenda é normalmente o nome de visualização dos dados que o controlo está a visualizar ou a introduzir. Pode especificar explicitamente um valor para a legenda ou ligá-la com informações do nome do visor do esquema. A legenda aparece no lado esquerdo de um controlo de tamanho normal. Se um controlo estiver a abranger o ecrã completo, a legenda aparece sobre o controlo. Este é um atributo opcional, tipo corda. Para obter informações sobre como ligar uma fonte de dados a um atributo ou um valor de propriedade, consulte a secção Propriedades.

    O exemplo a seguir mostra como uma legenda pode ser usada explicitamente:

    ```XML
      <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias">…<my:Control/>
    ```

    O exemplo a seguir mostra como uma legenda pode ser usada com uma fonte de dados. Se tiver usado o modelo para uma fonte de dados mostrada anteriormente neste documento, a sua fonte de dados é esquema. Recomendamos que ligue o Nome de Exibição do atributo com um atributo legenda.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}">…<my:Control/>
    ```

- **Ativado**: Este é um atributo opcional, tipo Boolean. Ao definir este valor de atributo para falso, o utilizador pode desativar um Controlo. O valor predefinido é definido como verdadeiro.

- **Visível**: Este é um atributo opcional, tipo Boolean. Pode usar este atributo para ocultar todo o controlo. O valor predefinido é definido como verdadeiro.

- **Descrição**: Utilize este atributo opcional, tipo de corda, para incluir uma descrição para ajudar o utilizador final a compreender o que deve colocar no controlo ou o que o controlo faz. Pode especificar explicitamente um valor para a descrição ou ligá-lo à informação de descrição do atributo esquema.

    A Descrição aparece no lado mais esquerdo de um controlo de tamanho normal por baixo da legenda. Se um controlo estiver a abranger o ecrã completo, a descrição aparece na parte superior do controlo por baixo da legenda. Para obter informações sobre como ligar uma fonte de dados a um atributo ou um valor de propriedade, consulte a secção Propriedades neste documento.

    O exemplo a seguir mostra como uma Descrição pode ser usada explicitamente:

    ```XML
    <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias" my:Description="This is explicit description.">…<my:Control/>
    ```

    Este exemplo mostra como uma Descrição pode ser usada com uma fonte de dados. Se tiver utilizado o modelo para uma fonte de dados mostrada anteriormente neste documento, a sua fonte de dados é **o esquema**. Recomendamos que ligue a **Descrição** do atributo a um atributo Descrição.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=Alias.Description, Mode=OneWay}">…<my:Control/>
    ```

- **ExpandirArea**: Este atributo indica se o controlo abrange todo o ecrã. Este é um atributo opcional, tipo Boolean. O valor predefinido é definido como falso.

    >[!NOTE]
    >Os atributos legendar e descrição são desativados quando este atributo é definido como verdadeiro. Utilize o controlo UocLabel para fornecer uma legenda para um controlo expandido.
    >

- **Atenção:** Este é um atributo opcional, tipo corda. O texto no atributo Dica ajuda o utilizador final a decidir o que é uma entrada válida para o controlo. A dica aparece por baixo do controlo.

- **AutoPostback**: Este é um atributo opcional, tipo Boolean. O valor predefinido é false. Se for definido como falso, refrescar a página pode não refrescar o controlo. Para obter informações sobre o AutoPostback, procure a propriedade de controlo de UI microsoft ASP.NET com o mesmo nome.

- **DireitosLevel**: Este é um atributo opcional, tipo corda. Pode ligar este atributo apenas com direitos de linha com uma fonte de dados. O controlo é ligado ou desativado dinamicamente, com base nos direitos do utilizador. Para obter informações sobre como ligar fontes de dados com um atributo ou um valor de propriedade, consulte a secção Propriedades neste documento.

    Este exemplo mostra como um atributo **RightsLevel** pode ser usado com uma fonte de dados. Se tiver utilizado o modelo para uma fonte de dados mostrada anteriormente neste documento, a sua fonte de dados tem **direitos**. Use o nome do atributo como O Caminho.
    <!--- no example provided -->

### <a name="property-element"></a>Elemento de propriedade
Pode utilizar um elemento **Property** para personalizar ainda mais o comportamento de cada controlo. Uma propriedade é um elemento vazio. O seguinte esquema XSD é para o elemento Propriedade:

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

Cada Propriedade tem os seguintes dois atributos necessários:

- **Nome**: Este atributo tipo corda é o nome único da Propriedade. Diferentes controlos têm propriedades diferentes. Existem algumas propriedades comuns que podem ser usadas por todos os controlos. Para obter mais informações sobre os nomes disponíveis para um determinado controlo, consulte as secções de controlo de propriedades comuns e controlos individuais deste documento.

- **Valor**: Este é o valor do Imóvel. O tipo de dados do valor depende do imóvel a que é atribuído. Consulte a secção seguinte para ver o formato de valor permitido para propriedades específicas.


#### <a name="bind-property-with-data-source-content"></a>Vincular propriedade com conteúdo de fonte de dados
Algumas propriedades podem estar ligadas com informações de uma fonte de dados. Utilize o seguinte formato de corda para fazer esta ligação. Consulte a descrição das propriedades individuais na secção de controlo individual deste documento para aprender a ligar propriedades a uma fonte de dados.

```
<my:Property my:Name="Required" my:Value="[Formatted String]"/>

   Formatted String :=  “{Binding “ + [SourceExpression] + “,” + [PathExpression] + “,” + [ModeExpression]? + “}

   SourceExpression:= “Source=” + [ObjectDataSourceName]

   PathExpression:= “Path=” + [AttributeName]|[AttributePropertyName]

   ModeExpression:= “Mode=” + [ModeChoice]

   ModeChoice:= “OneWay”|”TwoWay”

   ObjectDataSourceName:= The value of any string assign to node /ObjectControlConfiguration/ObjectDataSource/Name.

   AttributeName:= valid schema attribute name from the data source.

   AttributePropertyName:= valid property name of a schema attribute from the data source.
```

O seguinte XML mostra como ligar uma fonte de dados a um elemento **de propriedade:**

```XML
<my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
<my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
```


<h2 id="common-properties">Common properties (Propriedades comuns)</h2>

Todos os controlos RCDC especificados neste documento podem ter as propriedades comuns que são descritas nesta secção. Pode utilizar estas Propriedades juntamente com outras Propriedades específicas de diferentes controlos.

- **Obrigatório**: Esta propriedade indica que o campo é um campo obrigatório ou um campo opcional. Um campo obrigatório deve ser preenchido com um valor. Um valor vazio não é suportado para a entrada de cordas. Um campo opcional pode ser deixado vazio. Se este campo for um campo obrigatório sem valor preenchido, aparece uma mensagem de erro no topo do controlo de entrada. Pode especificar explicitamente se um campo é necessário ou opcional. Também pode ligar o campo à informação de esquema de uma determinada ligação entre um atributo e um tipo de recurso. Por predefinição, se esta propriedade faltar, significa que o controlo é um controlo de entrada opcional.

    O exemplo a seguir utiliza um valor explícito para esta propriedade:

    ```XML
    <my:Property my:Name="Required" my:Value="True"/>
    ```

    Este é um exemplo que utiliza uma fonte de dados dinâmica para esta propriedade. Se tiver utilizado o modelo para uma fonte de dados mostrada na secção anterior deste documento, a sua fonte de dados é esquema. Use `<attribute name>.Required` como o Caminho.

    ```XML
    <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
    ```

- **Leia Também:** Ao definir esta propriedade como verdadeira, o utilizador final experimenta o controlo num modo apenas de leitura. Este é um atributo opcional, tipo Boolean. O valor predefinido é definido como falso. No entanto, por vezes, o comportamento desta propriedade é substituído pelo tipo de direitos que uma pessoa tem sobre os dados que se ligam ao controlo. Por exemplo, se um utilizador não tiver direitos para atualizar um campo e o campo estiver ligado aos direitos de inline, o utilizador vê os dados num modo apenas de leitura, mesmo que esta propriedade esteja definida como falsa.

- **RegularExpression**: Esta propriedade especifica restrições que são impostas sobre o valor do controlo. Os formatos deste valor patrimonial são os formatos suportados na norma .NET StringRegex. Para obter mais informações, consulte [.NET Framework Expressões Regulares](https://go.microsoft.com/fwlink/?LinkId=165361). Se o controlo for utilizado para introduzir um valor, o valor é verificado contra a restrição especificada nesta propriedade quando o utilizador tenta sair da página atual. A mensagem de erro aparece em cima do controlo que tem entrada inválida. O utilizador pode especificar explicitamente uma expressão regular de cadeia. O utilizador também pode ligá-lo com informações de esquema de um determinado atributo. Por padrão, se esta propriedade estiver em falta, significa que o controlo não verifica quaisquer restrições nas cordas de entrada.

    O exemplo a seguir utiliza um valor explícito para esta propriedade:

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
    ```

    Este é um exemplo que utiliza uma fonte de dados dinâmica para esta propriedade. Se tiver utilizado o modelo para uma fonte de dados mostrada anteriormente neste documento, a sua fonte de dados é o esquema. Use o `<attribute name>.StringRegex` como o Caminho.

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=Alias.StringRegex, Mode=OneWay}"/>
    ```

- **Visível**: Este é um atributo opcional, tipo Boolean. Pode usar este atributo para ocultar todo o controlo. O valor predefinido é definido como verdadeiro.


<h3 id="options-element">Elemento de opções</h3>

O elemento Opções inclui um ou mais subnodes **de opção.** O elemento **Opções** é utilizado apenas com os **controlos UocRadioButtonList** e **UocDropDownList.** Para obter mais informações sobre como utilizar estes controlos, consulte a secção de controlos individuais deste documento.

O seguinte esquema XSD é para o elemento Opções:

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

O elemento **Opções** tem os seguintes atributos:

- **Valor**: Este é um atributo necessário do tipo de corda. O atributo de valor deve ser único dentro do mesmo controlo. Apenas A a Z, caracteres insensíveis a casos podem ser usados.

- **Legenda**: Este atributo requerido é o nome de visualização de cada Opção.

- **Atenção:** Este é um atributo opcional. Utilize este atributo para fornecer mais informações e dicas ao utilizador final.


## <a name="environment-variables"></a>Variáveis de ambiente

As seguintes variáveis ambientais podem ser utilizadas em qualquer configuração RCDC:

| Variável | Descrição |
|---|---|
| `<LoginID>`       | Exibe o ID do utilizador que está a fazer login.           |
| `<LoginDomain>`   | Exibe o domínio do utilizador que está atualmente a iniciar sessão.       |
| `<Today>  `       | Exibe a data e hora atuais                                |
| `<FromToday_nnn>` | Apresenta a data atual, mais `nnn` e a hora, onde `nnn` está um número inteiro.  |
| `<ObjectID> `     | O ID de recursos primários da RCDC.                                     |
| `<Attribute_xxx>` | Devolve um atributo especificado, xxx, do recurso primário RCDC. |


## <a name="debug-xml-configuration-files"></a>Ficheiros de configuração Debug XML

Quando estiver a desenvolver ou modificar ficheiros de configuração XML para um RCDC, pode ajudar a reduzir os erros validando o XML contra ficheiros XSD utilizando um editor como o Microsoft Visual Studio. Para obter mais informações, consulte [Uma Introdução às Ferramentas XML no Visual Studio 2005](https://go.microsoft.com/fwlink/?LinkID=74512).


## <a name="customize-help-files"></a>Personalizar ficheiros de ajuda

Se criar novos recursos e atributos, poderá querer atualizar os ficheiros de Ajuda existentes no Portal FIM com conteúdo para os seus recursos personalizados. Os ficheiros de ajuda no Portal FIM estão .htm formato e podem ser editados manualmente. Para obter mais informações sobre a criação de atributos personalizados, consulte Introdução à Gestão de Recursos Personalizados e Gestão de Atributos na documentação FIM 2010.

>[!IMPORTANT]
>Neste artigo não são fornecidas informações sobre os fundamentos da formatação ou edição de HTML. Espera-se que os utilizadores saibam como editar ficheiros HTML.

### <a name="location-of-help-files"></a>Localização dos ficheiros de ajuda
Todos os ficheiros de ajuda do Portal SP1 do Microsoft Identity Manager 2016 estão localizados na pasta `<ProgramFiles>\Common Files\Microsoft Shared\Web Server Extensions\12\Template\Layouts\MSILM2\Help\1033\html` do servidor de serviço MIM.

### <a name="locate-a-specific-help-file"></a>Localizar um ficheiro de ajuda específico
Todos os ficheiros de ajuda do Portal FIM são nomeados com um identificador globalmente único (GUID). Para localizar o ficheiro correto para o seu recurso personalizado:

1. No Portal FIM, abra o ficheiro Ajuda na página Portal que pretende personalizar.

2. Clique com o botão direito no ficheiro Ajuda e selecione **Propriedades.**

3. Realce e copie o `<GUID\>.htm` ficheiro no campo endereço **URL.**

4. Navegue na pasta onde os ficheiros Ajuda são armazenados e procure o ficheiro.


## <a name="add-content-for-attribute-in-existing-grouping-element"></a>Adicione conteúdo para atributo no elemento de agrupamento existente
Para adicionar conteúdo descritivo para um novo atributo dentro de um elemento de agrupamento existente (separador):

1. Identifique e localize o ficheiro de ajuda apropriado.

2. Utilizando um editor HTML, abra o ficheiro.

3. Localize onde pretende adicionar o conteúdo. Normalmente, isto está num parágrafo adicional, por exemplo:

    `<p xmlns="">A new paragraph with customized information.</p>`

    Pode também ser um item inserido numa lista existente, por exemplo:

    ```
    <li class="unordered"><b>First Name</b> – The first name of the User.<br>
    <li class="unordered"><b>Last Name</b> - The last name of the User.<br>
    <li class="unordered"><b>Added a new line</b><br>
    ```

## <a name="add-content-for-existing-grouping-element"></a>Adicione conteúdo para o elemento de agrupamento existente
A maioria das páginas do Fim Portal têm vários elementos de Agrupamento (ou separadores), e os ficheiros de Ajuda que o acompanham têm secções marcadas que se relacionam com cada elemento de Agrupamento. Os marcadores do HTML estão especificados nas secções. Por exemplo, este é o html para o separador Informações de Trabalho a partir do ficheiro Ajuda para a página 'Criar utilizador' no Portal FIM:

```
<a name="bkmk_grouping_WorkInfo" xmlns=""></a><h3 class="subHeading" xmlns="">Work Info</h3><p class="subHeading" xmlns=""></p><div class="subSection" xmlns="">
```

É referenciado pelo elemento de agrupamento **WorkInfo** no ficheiro Data XML de configuração para a **Configuração para criação de** utilizador RCDC. O `\<GUID\>.htm` nome do ficheiro e o marcador estão especificados no `my:Link` parâmetro:

```
<my:Grouping my:Name="WorkInfo" my:Caption="%SYMBOL_WorkInfoTabCaption_END%" my:Enabled="true" my:Visible="true"> <my:Help my:HelpText="%SYMBOL_WorkInfoTabHelpText_END%" my:Link="5e18a08b-4b20-48b8-90c6-c20f6cbeeb44.htm#bkmk_grouping_WorkInfo"/>
```

### <a name="simple-control-samples"></a>Amostras de controlo simples
Esta secção fornece amostras para a criação de diferentes controlos simples de caixa de texto.

A seguinte figura mostra alguns controlos simples da caixa de texto em diferentes modos:

![Controlos simples de caixa de texto](media/rcd-configuration-xml-reference/image016.gif)

O seguinte segmento de código cria o primeiro controlo de caixa de texto, que utiliza texto explícito para todos os atributos e propriedades:

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

O seguinte segmento de código cria o segundo controlo de caixa de texto, que utiliza uma técnica de ligação dinâmica para ligar o controlo a uma fonte de dados diferente:

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

O seguinte segmento de código cria o terceiro rótulo expandido e controlo de caixa de texto:

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

O seguinte segmento de código cria o quarto controlo de caixa de texto desativado. Embora este controlo não apresente uma diferença visível entre o estado desativado e o estado ativado, o utilizador já não pode introduzir dados na caixa de texto.

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

Esta secção documenta os controlos individuais fornecidos com o Microsoft Identity Manager 2016 SP1.

### <a name="uocbutton"></a>UocButton

**Nome**: UocButton

**Descrição**: Este é um simples controlo de botões que pode utilizar para desencadear determinadas ações. No entanto, como não é possível especificar o seu próprio manipulador, o uso deste controlo é limitado.

**Propriedades:**

- Todas as propriedades comuns: Para obter informações sobre estas propriedades, consulte <a href="#common-properties">propriedades comuns.</a>

- **Texto**: Esta propriedade especifica o texto que aparece no botão. Este é um atributo opcional, tipo corda. O texto tem um valor de corda explícito.

**Eventos:**

- **OnButtonClicked**: O evento é emitido quando o botão é clicado.

**Exemplo:**

![Controlo UocButton](media/rcd-configuration-xml-reference/image017.png)

O seguinte segmento XML produz um simples botão de controlo UocButton:

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

**Descrição**: Este controlo é utilizado para visualizar a legenda de uma página RCDC. Este comando foi concebido apenas para ser utilizado como um único controlo num Agrupamento de Cabeçalhos. A sua utilização em qualquer outro contexto pode causar problemas de renderização ou erros no portal.

**Modo**: Ler apenas (OneWay)

**Propriedades:**

- Todas as propriedades comuns: Para obter informações sobre estas propriedades, consulte <a href="#common-properties">propriedades comuns.</a>

- **MaxHeight:** Esta propriedade especifica a altura máxima do ícone na secção de legendas. Esta propriedade é opcional. Esta propriedade tem um valor inteiro em pixels. O valor predefinido é de 32 pixels.

**Eventos:**

- Não há eventos para este controlo.

**Exemplo:**

![Controlo uocCaptionControl](media/rcd-configuration-xml-reference/image020.jpg)

O seguinte segmento de código gera uma **legenda de cabeçalho:**

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

O seguinte segmento de código gera uma **legenda de conteúdo explícito:**

```
<my:Control my:Name="SampleContentCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Explicit Content Caption" my:Description="Explicit content caption with smaller icon">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="20"/>
          <my:Property my:Name="MaxWidth" my:Value="20"/>
     </my:Properties>
</my:Control>
<!--End of sample caption-->
```

O seguinte segmento de código gera uma legenda dinâmica **display Name:**

```
<!--Sample content dynamic caption-->
<my:Control my:Name="Caption3" my:TypeName="UocCaptionControl" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}"/>
<!--End of sample caption -->
```


### <a name="uoccheckbox"></a>UocCheckBox

**Nome**: UocCheckBox

**Descrição**: Trata-se de um simples controlo da caixa de verificação. Recomendamos que o utilizador ligue este controlo a dados do tipo Boolean. Este controlo pode ser usado como um controlo apenas de leitura ou um controlo updatable, com base nos dados a que se liga.

>[!NOTE]
>Nesta versão, quando se utiliza o controlo da caixa de verificação no modo de edição para apresentar um atributo Boolean, se o atributo não tiver um valor previamente atribuído, o Controlo de Recursos adiciona um valor de **falso** ao atributo quando **OK** é clicado no modo de edição. O trabalho é criar sempre um atributo Boolean que pressupõe que a não existência é a mesma que **falsa,** ou usar outros controlos, como um botão de rádio para atributos Boolean.

**Propriedades:**

- Todas as propriedades comuns: Para obter informações sobre estas propriedades, consulte <a href="#common-properties">propriedades comuns.</a>

- **DefaultValue**: Esta é uma propriedade opcional, tipo Boolean. O valor predefinido é definido como falso. Este campo especifica o comportamento padrão de uma caixa de verificação. Isto pode ser especificado explicitamente.

- **Verificado**: Esta é uma propriedade opcional, tipo Boolean. O valor predefinido é definido como falso. Este valor substitui a propriedade DefaultValue quando está presente juntamente com o DefaultValue. Este campo especifica o comportamento de uma caixa de verificação. Tal como o DefaultValue, este pode ser especificado explicitamente ou ligado aos dados do servidor.

- **Texto**: Este é um atributo opcional, tipo corda. O texto é mostrado à direita da caixa de verificação. Pode utilizar esta propriedade para especificar texto que forneça mais informações ao utilizador final.

**Eventos:**

- **Verificado :** Quando a caixa de verificação muda de estado, este evento é emitido.

**Exemplo:**

No exemplo seguinte, é criada uma ligação personalizada entre o tipo de recurso personalizado e o atributo **IsConfigurationType**. O XML é utilizado no RCDC de um tipo de recurso personalizado.

![Controlo UocCheckBox](media/rcd-configuration-xml-reference/image022.png)

O seguinte segmento de código produz uma **caixa de verificação dinâmica,** como mostrado como Dynamic Check Box na figura anterior. Este tipo de encadernação é mais versátil e útil do que uma caixa de verificação explícita. O atributo deve pertencer ao tipo de recurso atual.

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

**Nome**: UocCommonMultiValueControl

**Descrição**: Trata-se de um controlo multiline de caixa de texto que suporta formatação de cordas especiais. Cada valor entre as entradas multivalorizadas é separado entre si por um ponto e vírgula (;) ou uma rutura de linha na caixa de texto. Recomendamos a ligação deste controlo com dados de tipos multivalorizados, de cordas curtas e inteiros. Este controlo suporta o modo apenas de leitura e o modo de updatable.

**Propriedades:**

- Todas as propriedades comuns: Para obter informações sobre estas propriedades, consulte <a href="#common-properties">propriedades comuns.</a>

- **DataType**: Este é um atributo necessário, tipo corda. Pode especificar isto como um tipo **de String, Integer** ou **DateTime** explicitamente. Também pode ligar o atributo com a propriedade **DataType** do atributo de esquema. Um tipo de referência multivalorizada deve ser manuseado por **UOCListView** ou **UOCIdentityPicker**. Multivalorizado Boolean não é um tipo de dados suportado.

- **Linhas**: Este é um atributo opcional, do tipo inteiro. Pode definir a altura da caixa em número de caracteres. O valor predefinido é definido para 1.

- **Colunas**: Trata-se de um atributo opcional, do tipo inteiro. Pode definir quantas largas a caixa é em número de caracteres. O valor predefinido é definido para 20.

- **Valor**: Este é um atributo opcional, tipo corda. Só pode ligar este atributo à fonte de dados.

**Eventos:**

- **ValueListChanged**: Este evento é desencadeado quando o valor atual no controlo muda.

**Exemplo:**

No exemplo seguinte, é criado um atributo de corda multivalorizado chamado **AMultiValueString** e ligado ao tipo de recurso personalizado. Este exemplo só funciona depois de esta ligação ser criada.

![Controlo UocCommonMultiValueControl](media/rcd-configuration-xml-reference/image024.jpg)

O seguinte segmento de código gera um controlo **UocCommonMultiValueControl:**

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

**Descrição**: Isto é semelhante a um controlo de caixa de texto, mas **a Descrição** aceita apenas um determinado formato. No modo só de leitura, parece uma etiqueta. Para o formato da cadeia de entrada suportada, consulte a propriedade **DateTimeFormat** nesta secção.

**Propriedades:**

- Todas as propriedades comuns: Para obter informações sobre estas propriedades, consulte <a href="#common-properties">propriedades comuns.</a>

- **DataTimeFormat**: Este é um atributo opcional, tipo corda. Os formatos suportados são **DateTime** e **DateOnly**. O valor predefinido é definido para o formato **DateTime.**

  - **DataTime**: O atributo é formatado como mm/dd/yy hh:mm:mm:ss AM.
  - **DataOnly**: O atributo é formatado como mm/dd/yy.

    >[!NOTE]
    >Ambos os formatos **DateTime** e **DateOnly** são suportados, independentemente do utilizador que esteja a especificar a diferença.
    >

- **Valor**: Este é um atributo opcional, tipo corda. Liga este atributo a uma fonte de dados de recursos. O valor deste atributo tem de estar em conformidade com o formato de datamento correto.

**Eventos:**

- **DataTimechange**: Quando o valor da data muda, o evento ocorre.

**Exemplo:**

![Controlo UocDateTimeControl](media/rcd-configuration-xml-reference/image027.jpg)

O seguinte segmento de código produz o primeiro controlo **DateTime.**

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

O seguinte segmento de código produz o segundo controlo **DateTime.** Se tiver utilizado o código de amostra na secção fontes de dados, o atributo **ExpirationTime** está ligado a todos os tipos de recursos. Portanto, pode usá-lo com o seguinte código:

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

**Descrição**: Trata-se de um simples controlo de caixa para baixo. Este controlo é utilizado para selecionar opções a partir de um conjunto de escolhas definido. Os tipos de dados de cordas, inteiros, datas e Boolean são bons candidatos a este controlo.

**Propriedades:**

- Todas as propriedades comuns: Para obter informações sobre estas propriedades, consulte <a href="#common-properties">propriedades comuns.</a>

- **ValuePath**: A propriedade para obter o atributo Valor do ItemSource. Quando itemSource é especificado como Personalizado, o caminho de valor é definido como Valor. Liga-se ao campo Valor a partir do elemento Opção, conforme descrito nesta secção.

- **CaptionPath**: A propriedade para obter o atributo Valor do ItemSource. Quando itemSource é especificado como Personalizado, o caminho de valor é definido como Legenda. Liga-se ao campo legenda do elemento Opção, conforme descrito nesta secção.

- **HintPath**: A propriedade para obter o atributo Valor do ItemSource. Quando o ItemSource é especificado como Personalizado, o caminho de valor é definido para Sugestão. Liga-se ao campo De sugestão a partir do elemento Opção, conforme descrito nesta secção.

- **ItemSource**: Uma coleção de ListControlItems que define as escolhas na lista. O utilizador pode explicitamente defini-lo para Personalizar e utilizar o elemento Opção, tal como descrito nesta secção, para especificar o valor da cadeia.

- **SeleccionadoValue**: O valor atualmente selecionado. Esta é uma propriedade necessária, tipo corda. Esta propriedade está ligada com dados de cadeia da fonte de dados.

**Eventos:**

- **SeleccionadoIndexChanged**: O evento ocorre quando a seleção na caixa de entrega muda.

**Opções:**

Para a estrutura de um elemento **Opções,** consulte <a href="#options-element">o elemento Opções</a>.

- **Valor**: O valor de um elemento de opções únicas pode ser definido para qualquer cadeia que seja a entrada válida da fonte de dados a que o controlo se liga.

- **Legenda**: A legenda pode ser qualquer valor de corda.

- **Dica:** A dica pode ser qualquer valor de corda.

**Exemplo:**

![Controlo UocDropDownList](media/rcd-configuration-xml-reference/image030.jpg)


![Opções num controlo UocDropDownList](media/rcd-configuration-xml-reference/image031.jpg)

>[!NOTE]
>Para fazer a amostra funcionar, deve ligar um atributo existente, tipo de corda **Scope,** com o tipo de recurso personalizado a que o RCDC se aplica.


O seguinte segmento de código gera uma lista de drop-down:

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


### <a name="uocfiledownload"></a>Carga UocFileDown

**Nome**: UocFileDownload

**Descrição**: Este controlo contém uma hiperligação. Quando a hiperligação é clicada, aparece uma página de Ficheiros de Salvamento do Windows. O utilizador pode guardar o ficheiro para a unidade local. A opção Open também é suportada se o Internet Explorer puder tornar o formato de ficheiro. Os tipos de dados recomendados para utilizar este controlo são os tipos de cadeias formatadas (XML) e binários.

>[!NOTE]
>Nesta versão do Microsoft Identity Manager 2016 SP1, o utilizador deve fechar a janela do Internet Explorer na qual abriu o ficheiro e, em seguida, atualizar a página. Depois de refrescar a janela do Internet Explorer, o utilizador pode então iniciar o download para guardar ou abrir novamente o mesmo ficheiro na janela original.

**Propriedades:**

- Todas as propriedades comuns: Para obter informações sobre estas propriedades, consulte <a href="#common-properties">propriedades comuns.</a>

- **Texto**: Trata-se de um atributo opcional, tipo de corda, que define o texto de hiperligação. O utilizador pode especificar uma cadeia explícita para esta propriedade.

- **Valor**: Este é um atributo obrigatório. Especifica a ligação do atributo no servidor cujo conteúdo deve ser descarregado.

- **Nome solicitadoFile :** Este é um atributo opcional, tipo corda. Este é o nome do ficheiro que é sugerido ao utilizador quando guardam o ficheiro descarregado.

- **ConteúdoType**: Este é um atributo necessário, tipo corda. Este é o tipo de ficheiro em que os dados são guardados. Texto ou binário são as duas opções de cordas suportadas. Se for texto, o valor de retorno é considerado como uma cadeia longa. Caso contrário, para binário, o valor de retorno é considerado byte[]. Se o texto for selecionado, o utilizador pode, como opção, adicionar um sufixo para especificar o tipo de formato em que o texto se encontra. Por exemplo, o texto/xml é válido.

>[!NOTE]
>Quando o valor que está ligado a este controlo estiver vazio, falta o controlo da hiperligação a utilizar para desencadear uma ação de descarregamento. Isto é porque não há nada para descarregar.

**Eventos:**

- Não há eventos para este controlo.

**Exemplo:**

![Controlo de carga UocFileDown](media/rcd-configuration-xml-reference/image035.png)

>[!NOTE]
>Antes de carregar este ficheiro de amostra, o utilizador deve criar uma ligação entre um tipo de recurso personalizado e o atributo ConfigurationData existente.

O seguinte segmento de código gera um controlo de descarregamento de ficheiros:

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

**Descrição**: Este controlo contém uma caixa de texto que mostra a localização do ficheiro local a ser carregado, um botão de ficheiro de navegação e um botão de upload. Quando o utilizador final clica num botão Navegar, aparece uma janela do Ficheiro Aberto do Windows. O utilizador final pode selecionar um ficheiro na sua unidade local para carregar. Quando o ficheiro é selecionado, a localização do ficheiro é mostrada na caixa de texto. Quando o botão Upload é clicado, o ficheiro é carregado para a fonte de dados local do lado do cliente. O conteúdo do ficheiro ainda não foi submetido ao servidor. Os tipos de dados recomendados para utilizar este controlo são os seguintes: cadeias formatadas (XML) ou tipos binários.

>[!NOTE]
>Não há indicação do progresso ou do estado do upload. Quando o ficheiro é enviado para a fonte de dados local, a caixa de texto é limpa.

**Propriedades:**

- Todas as propriedades comuns: Para obter informações sobre estas propriedades, consulte <a href="#common-properties">propriedades comuns.</a>

- **Valor**: Este é um atributo obrigatório. Especifica a ligação do atributo esquema no servidor para o qual os dados são carregados.

- **ConteúdoType**: Este é um atributo opcional, tipo corda. Este é o tipo de dados a que o ficheiro é guardado no servidor. Isto pode ser definido para Texto ou Binário. Quando a propriedade está em falta, o valor padrão é binário.

- **MaxFileSize:** Este é um atributo opcional, tipo corda. O MaxFileSize define o tamanho do ficheiro carregado. Por predefinição, se a propriedade faltar, o tamanho máximo é de 1 megabyte (MB).

- **PromptedForNoValue**: Este é um atributo opcional, tipo corda. Define o texto que aparece para o utilizador quando um ficheiro não está a ser carregado.

**Eventos:**

- **FileUploaded**: Este evento é emitido quando o ficheiro é carregado com sucesso.

**Exemplo:**

![Controlo UocFileUpload](media/rcd-configuration-xml-reference/image040.png)

>[!NOTE]
>Para fazer funcionar o seguinte código de amostra, deve criar um novo atributo binário chamado ABinaryAttribute e, em seguida, criar uma nova ligação entre um tipo de recurso personalizado e este atributo.

O seguinte segmento de código gera um controlo de upload:
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

**Nome**: UocFilterBuilder

**Descrição**: Trata-se de um controlo complexo que permite ao utilizador fazer uma expressão MIM 2016 XPath. Algumas expressões XPath não são suportadas. Para obter informações sobre como utilizar o construtor de filtros, consulte a Ajuda para o construtor de filtros.

**Propriedades:**

- Todas as propriedades comuns: Para obter informações sobre estas propriedades, consulte <a href="#common-properties">propriedades comuns.</a>

- **PermitidosTips DeObjecectOs**: Isto define uma lista de tipos de recursos a serem mostrados na declaração selecionada de um construtor de filtros. Para obter informações sobre como utilizar o construtor de filtros, consulte o construtor de filtros Ajuda. A cadeia está no formato de ResourceTypeA, ResourceTypeB, onde cada tipo de recurso é separado por uma vírgula ','.

- **Valor**: Este é o valor com que o construtor de filtros é renderizado. Apenas uma ligação com dados do tipo de corda que contém uma expressão XPath é suportada. O atributo Filtro é um atributo recomendado para a ligação deste controlo.

- **PreviewButtonVisible**: Esta é uma propriedade opcional, tipo Boolean. Quando esta propriedade é definida como falsa, o utilizador não vê um botão de pré-visualização. O valor predefinido é definido como verdadeiro. Este botão pode ser usado em combinação com um controlo de visualização de lista para visualizar os resultados de uma expressão XPath.

- **Exclui oGroupMembership**: Esta é uma propriedade booleana. Quando esta propriedade é definida como verdadeira, não é possível criar um filtro que utilize \<Reference Attribute\> (por exemplo, ResourceID) é membro de \<Group object\> . Por outras palavras, quando esta propriedade é definida como verdadeira, não é possível criar um filtro que utilize o diretório de membros do grupo.

- **Pré-visualizaçãoCaptonCaption**: Esta é uma cadeia opcional. Quando o PreviewButtonVisible estiver definido como verdadeiro, pode utilizar esta propriedade para dar ao botão um texto personalizado. O texto aparece no botão pré-visualização.

**Eventos:**

- **OnFilterChanged**: Este evento é acionado quando o conteúdo do construtor de filtros muda.

**Exemplo:**

![Controlo UocFilterBuilder](media/rcd-configuration-xml-reference/image044.png)

O seguinte código de amostra inclui um controlo UOCLabel, um simples construtor de filtros com PermitidosObjectTypes e uma visualização de lista de pré-visualização. Aponte a propriedade ListFilter e o construtor de filtros Valor propriedade para o mesmo atributo de fonte de dados para ligar os dois.

>[!NOTE]
>Antes de utilizar este código de amostra, crie uma nova ligação entre um atributo filtro existente e um tipo de recurso personalizado.

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


### <a name="uochtmlsummary"></a>UocHtmlSummary

**Nome**: UocHtmlSummary

**Descrição**: Pode utilizar este controlo para definir uma página de resumo numa página do RCDC. Esta página de resumo aparece depois de o utilizador final apresentar um pedido. Este controlo só pode ser utilizado num Agrupamento Sumário e deve ser o único controlo. Recomendámos vivamente que usasse o código de amostra que é fornecido.

>[!NOTE]
>Este controlo não foi testado extensivamente.

**Propriedades:**

- Todas as propriedades comuns: Para obter informações sobre estas propriedades, consulte <a href="#common-properties">propriedades comuns.</a>

- **ModificaçõesXml**: Esta propriedade deve ser formatada como {Binding Source=delta, Path=DeltaXml}, onde o delta é definido no cabeçalho de configuração ObjectDataSource.

- **TransformXsl**: Esta propriedade é formatada como {Binding Source=resumoTransformXsl, Path=/}, onde o resumoTransformXsl é definido no cabeçalho de configuração XmlDataSource.

**Eventos:**

- Não há eventos para este controlo.

**Exemplo:**

Para obter uma amostra deste controlo, consulte o exemplo de um Agrupamento Sumário na secção elemento de agrupamento deste documento.


### <a name="uochyperlink"></a>UocHyperLink

**Nome**: UocHyperLink

**Descrição**: Trata-se de um simples controlo de hiperligação. Pode utilizar este controlo para apresentar informações como hiperligação.

**Propriedades:**

- Todas as propriedades comuns: Para obter informações sobre estas propriedades, consulte <a href="#common-properties">propriedades comuns.</a>

- **ObjectReference**: Trata-se de uma propriedade opcional, tipo de referência. Se um recurso válido for referenciado pelo GUID que é definido nesta propriedade, a hiperligação fornece ao utilizador final uma forma de aceder ao recurso. Isto é mutuamente exclusivo com a propriedade NavigateUrl.

- **Texto**: Esta é uma propriedade opcional, tipo corda. Você usa esta propriedade para definir o texto que aparece como a hiperligação.

- **NavigateUrl**: Esta é uma propriedade opcional, tipo corda. Você usa esta propriedade para definir o URL de percurso completo a que a hiperligação se liga. Isto é mutuamente exclusivo com propriedade ObjectReference.

**Eventos:**

- Não há eventos para este controlo.

**Exemplo:**

![Controlo UocHyperLink](media/rcd-configuration-xml-reference/image049.jpg)

>[!NOTE]
>Precisa de um RECURSO GUIADO válido para o ligar. Neste caso, a segunda hiperligação é gerada com um GUID válido. O primeiro pode ser qualquer web site.

O seguinte segmento de código gera uma hiperligação de redirecionamento:

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

O seguinte segmento de código gera uma hiperligação que faz referência a um recurso. A referência explícita pode ser substituída pela expressão {Binding Source=object, Path=Creator} para ligar isto a uma fonte de dados. Isto só pode ser válido quando o gestor do recurso existe e é de valor de referência.

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

**Nome**: UocIdentityPicker

**Descrição**: Este controlo consiste numa caixa de resolução opcional e numa janela de navegação. A caixa resolve opcional consiste numa caixa de texto opcional para introduzir a identidade, um botão Resolver para resolver a identidade e um botão de navegar para solicitar uma janela de navegação pop-up. A janela Procurar permite ao utilizador selecionar identidades através de um controlo de visualização de listas. A identidade selecionada a partir da janela Browse reflete-se na caixa Resolver.

**Propriedades:**

- Todas as propriedades comuns: Para obter informações sobre estas propriedades, consulte <a href="#common-properties">propriedades comuns.</a>

- **UsageKeywords**: Esta é uma propriedade de cordas opcional. Pode definir uma lista de âmbitos de pesquisa a utilizar no Picker de Recursos, fornecendo uma lista das palavras-chave de utilização que são suportadas pela estrutura SearchScopeConfiguration, onde cada palavra-chave é separada por um apóstrofo (').

- **Filtro:** Esta é uma propriedade de cordas opcional. O utilizador fornece uma expressão XPath para visualizar o selecionador de recursos para exibir apenas os itens que se encaixam dentro de um âmbito definido. Esta propriedade é mutuamente exclusiva com a propriedade UsageKeywords. Quando o âmbito de pesquisa é aplicado, esta propriedade não tem efeito.

- **ResultadoObjectType**: Esta é uma propriedade de cordas opcional. O tipo de recurso é utilizado para renderizar recursos na lista de caixas de diálogo pop-up. Isto é usado com o Filtro para ajudar o Apanhador de Identidade a identificar que tipo de recurso é devolvido pelo Filtro e a tornar os dados em conformidade. Esta propriedade é mutuamente exclusiva com a propriedade UsageKeywords. Quando o âmbito de pesquisa é aplicado, este não tem efeito. A cadeia que é aceite para esta propriedade é qualquer nome único, válido, tipo recurso, por exemplo, Pessoa. Quando se espera que o filtro devolva vários tipos de recursos, o Recurso é utilizado.

- **Pré-visualizaçãoTitle**: Este é o título de pré-visualização utilizado numa vista de lista. Para obter informações sobre esta propriedade, consulte a secção UocListView.

- **ListViewTitle**: Esta é uma propriedade de cordas opcional. Pode utilizar esta propriedade para definir o texto mostrado no topo da vista da lista como um título.

- **Valor**: Esta é uma propriedade de cordas opcional. Recomenda-se que o ligue a um atributo de esquema para ligar o valor a uma fonte de dados.

- **Modo**: Esta é uma propriedade de cordas opcional. Você usa esta propriedade para definir se um valor pode ser selecionado pelo Escolhidor de Identidade ou várias identidades podem ser selecionadas. SingleResult e MultipleResult são os valores permitidos. Por predefinição, está definido para SingleResult.

- **ObjectTypes**: Esta é uma propriedade opcional, tipo corda. Pode definir uma lista de tipos de recursos que o utilizador final pode resolver as entradas na caixa "Resolução de Escolha de Identidade". A lista consiste numa lista de nomes do tipo de recursos separados por uma vírgula ','.

- **AtributosToSearch**: Esta é uma propriedade opcional, tipo corda. Pode definir uma lista de atributos a utilizar para resolver o item no Apanhador de Identidade, onde a lista é uma lista de atributos de esquema, separados por uma vírgula ',', ''. Por exemplo, se o AtributoToSearch estiver definido `DisplayName, Alias` para, o utilizador pode pesquisar os itens com `DisplayName = \<search value\>` ou `Alias=\<search value\>` . Os nomes de atributos que são introduzidos aqui devem ser atributos válidos sobre os tipos de recursos-alvo da fonte de dados que está especificado na propriedade Valor. Os tipos de recursos-alvo podem ser encontrados no campo ObjectTypes. Todos os atributos devem ser válidos em quaisquer tipos de recursos que sejam citados no campo ObjectTypes.

- **ColumnsToDisplay**: Esta é uma propriedade opcional, tipo corda. O utilizador fornece uma lista de nomes de atributos de esquema, separados por uma vírgula ','. Os atributos que são definidos aqui compõem a coluna da vista da lista no Apanhador de Identidade.

- **Linhas**: Trata-se de uma propriedade completa e opcional. Só funciona quando o modo está definido para MultipleResult. Utilize esta propriedade para definir a altura da caixa de texto Resolve para um dado tamanho nas unidades de caracteres.

- **MainSearchScreenText**: Esta é uma propriedade opcional, tipo corda. Este é o texto personalizado que aparece enquanto a pesquisa está a decorrer na janela Browse.

**Eventos:**

- **SeleccionadosObjectChanged**: Este evento é emitido quando o utilizador altera os recursos selecionados.

**Exemplo:**

![Controlo UocIdentityPicker no modo SingleResult](media/rcd-configuration-xml-reference/image052.png)

>[!NOTE]
>Para que esta amostra funcione, deve criar uma nova ligação entre o atributo Manager e qualquer tipo de recurso personalizado a que este XML se aplique.

O seguinte segmento de código gera um Apanhador de Identidade no modo SingleResult utilizando as propriedades Filter e ResultObjectType como parte do RCDC:

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

A seguinte figura mostra um apanhador de identidade no modo MultipleResult:

![Controlo UocIdentityPicker no modo MultipleResult](media/rcd-configuration-xml-reference/image056.jpg)

>[!NOTE]
>Para que este código de amostra funcione, deve ligar o atributo ExplicitMem (um atributo de referência multivalorizado) ao tipo de recurso personalizado. Crie âmbitos de pesquisa com a propriedade UsageKeyword definida para Pessoa e Grupo.

O seguinte segmento de código cria um Apanhador de Identidade no modo MultipleResult:

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

**Nome**: UocLabel

**Descrição**: Trata-se de um controlo simples, apenas de leitura, de etiquetas de texto. Recomendamos que este controlo seja utilizado para visualizar dados apenas de leitura.

**Propriedades:**

- Todas as propriedades comuns: Para obter informações sobre estas propriedades, consulte <a href="#common-properties">propriedades comuns.</a>

- **Texto**: Este é um atributo do tipo corda. Você define esta propriedade fornecendo um valor de cadeia explícito ou ligando-o com uma fonte de dados. Uma ligação de amostra que atribui o valor desta propriedade é {Binding Source=object, Path= \<valid attribute name\> .

Para obter uma amostra do controlo UocLabel, consulte o controlo simples na secção de amostras de controlo simples.


### <a name="uoclistview"></a>UocListView

**Nome**: UocListView

**Descrição**: Trata-se de um controlo avançado de visualização de listas. Consiste numa simples vista de lista, uma pesquisa simples opcional, um controlo de pesquisa avançado opcional, uma caixa de pré-visualização de seleção opcional e uma barra de botão de ação. A pesquisa simples opcional consiste num âmbito de pesquisa e numa caixa de texto de pesquisa simples. O controlo avançado de pesquisa é um construtor de filtros. A lista mostra uma lista prerendered de recursos. Também pode mostrar resultados de pesquisa provenientes dos controlos de pesquisa neste controlo. A barra de botão de ação define que medidas podem ser tomadas com base na seleção na vista da lista. A caixa de pré-visualização de seleção mostra quais os itens selecionados a partir da vista da lista.

>[!IMPORTANT]
>O UocListView não funciona com atributos de referência de valor único. Só pode ser utilizado com atributos de referência multivalorizado. Para obter atributos de referência de valor único, consulte o UocIdentityPicker neste documento.

**Propriedades:**

- Todas as propriedades comuns: Para obter informações sobre estas propriedades, consulte <a href="#common-properties">propriedades comuns.</a>

- **SeleccionadoValue**: Esta é uma propriedade opcional, tipo corda, que está ligada a um atributo de referência multivalorizado aceitando uma lista de cordas formatadas guid.

- **PageSize**: Trata-se de uma propriedade completa opcional. O utilizador pode especificar quantas entradas cabem numa página num controlo de visualização de lista. O valor predefinido é de 10 entradas. Qualquer inteiro positivo é válido.

- **UsageKeyword**: Esta é uma propriedade opcional, tipo corda. O utilizador pode especificar uma lista de palavras-chave que definem o âmbito de pesquisa utilizado no controlo de pesquisa de visualização de listas. Existem recursos de alcance de pesquisa no servidor FIM 2010. O atributo numa estrutura de Configuração do SearchScope, chamado UsageKeyword, é usado para agrupar um conjunto de âmbitos de pesquisa. A lista de visualizações consome essa lista de palavras-chave. Cada palavra-chave é separada por uma vírgula (,). Esta é a palavra-chave de utilização utilizada no âmbito de pesquisa correspondente que pretende mostrar nesta vista de lista. Isto só está em vigor quando a propriedade ShowSearchControl é definida como verdadeira.

- **SearchControlAutoPostback**: Esta é uma propriedade booleana opcional. Desaccione o valor desta propriedade para realizar autopostback quando uma pesquisa é desencadeada. Por predefinição, searchControlAutoPostback é definido como falso.

- **EmptyResultText**: Esta é uma propriedade opcional, tipo corda. Por predefinição, está definido para Não itens, mas pode ser definido para qualquer valor de cadeia. Este texto aparece quando um resultado de pesquisa está vazio.

- **ButtonHeight**: Esta é uma propriedade opcional, do tipo inteiro. Desajei o valor deste imóvel a qualquer valor inteiro positivo. Esta propriedade define a altura dos botões na barra de ação em pixels. O valor predefinido é de 32 pixels.

- **ButtonWidth**: Esta é uma propriedade opcional, do tipo inteiro. Desajei o valor deste imóvel a qualquer valor inteiro positivo. Esta propriedade define a largura dos botões na barra de ação em pixels. O valor predefinido é de 32 pixels.

- **CaptionImageMaxHeight**: Esta é uma propriedade opcional, do tipo inteiro. Desajei o valor deste imóvel a qualquer inteiro positivo. Esta propriedade define a altura máxima do ícone de uma legenda opcional. O valor predefinido é de 32 pixels.

- **CaptionImageMaxWidth**: Esta é uma propriedade opcional, do tipo inteiro. Desajei o valor deste imóvel a qualquer inteiro positivo. Esta propriedade define a largura máxima do ícone de uma legenda opcional. O valor predefinido é de 32 pixels.

- **CaptionImageUrl**: Esta é uma propriedade opcional, tipo corda. Esta propriedade define um URL que se liga a uma imagem que aparece como a imagem da legenda.

- **Pré-visualização :** Esta é uma propriedade opcional, tipo corda. Utiliza esta propriedade para definir o texto que aparece em cima da caixa de pré-visualização de seleção.

- **EnableSelection**: Trata-se de uma propriedade opcional, tipo Boolean. Você usa esta propriedade para definir se uma vista de lista está no modo de seleção. Se uma visualização da lista estiver no modo de seleção, aparece uma coluna de caixas de verificação na coluna mais à esquerda da vista da lista e aparece uma caixa de pré-visualização de seleção na parte inferior da vista da lista. O valor padrão desta propriedade é definido como verdadeiro.

- **SingleSelection**: Esta é uma propriedade opcional, tipo Boolean. Se o modo de seleção estiver ligado para a visualização da lista, definir este valor para verdadeiros limites o utilizador final para selecionar apenas um item da lista. Por padrão, o valor desta propriedade é definido como falso. Isto significa que, por padrão, o utilizador final pode selecionar vários itens da lista.

- **RedirectUrl**: Esta é uma propriedade opcional, tipo corda. Utilize esta propriedade para especificar uma página para redirecionar para quando um item hiperligado é clicado na lista. Este URL pode conter espaços reservados que são substituídos pelo valor real durante o tempo de funcionação. Os espaços reservados são os seguintes:

    - {0} objectType
    - {1} objectID
    - {2} displayName

- **ShowTitleBar**: Esta é uma propriedade opcional, tipo Boolean. Utilize esta propriedade para especificar se a barra de título deve ser visível. O valor padrão desta propriedade é falso.

- **ShowActionBar**: Esta é uma propriedade opcional, tipo Boolean. Utilize esta propriedade para especificar se a área da barra de ação deve ser visível. O valor padrão desta propriedade é verdadeiro.

- **ShowPreview**: Esta é uma propriedade opcional, tipo Boolean. Utilize esta propriedade para especificar se a área de pré-visualização deve ser visível. O valor padrão desta propriedade é verdadeiro.

- **ShowSearchControl**: Esta é uma propriedade opcional, tipo Boolean. Utilize esta propriedade para especificar se o controlo de pesquisa deve ser visível. O valor padrão desta propriedade é verdadeiro.

- **ResultadoObjectType**: Esta é uma propriedade opcional, tipo corda. Utilize esta propriedade para especificar o tipo de objeto esperado dos resultados de pesquisa. O valor padrão desta propriedade é Recurso. Se o resultado da pesquisa contiver vários tipos de recursos, este valor deve ser especificado como Recurso.

- **ColumnsToDisplay**: Esta é uma propriedade opcional. Utilize esta propriedade para especificar quais os atributos que pretende que a vista da lista seja exibida como colunas. O valor predefinido desta propriedade é DisplayName, ResourceType. Cada coluna é representada pelo nome do sistema de um atributo. Cada coluna é separada por uma vírgula', Não tem de especificar um valor para esta propriedade quando a vista da lista é utilizada no modo de seleção. No modo de seleção, a definição de coluna provém do atributo SearchScopeColumn do âmbito de pesquisa que está atualmente selecionado.

- **ListFilter**: Esta é uma propriedade opcional, tipo corda. Este é o xpath que é usado para tornar a visualização da lista, e só está em vigor quando a propriedade ShowSearchControl é definida como falsa. Quando este valor é especificado, a vista de lista utiliza este valor de propriedade para consultas e a vista da lista não está no modo de seleção. O filtro pode ser ligado a um atributo de cadeia do recurso:

    `<my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}"/>`

    ou ser uma cadeia que contém alguma variável de ambiente predefinido:

    `<my:Property my:Name="ListFilter" my:Value="/Approval[Request=''%ObjectID%'']"/>`

- **TargetAttribute**: Esta é uma propriedade obsoleta. O seu valor deve ser o nome do sistema de um atributo de referência multivalorizado. Recomendamos que esta propriedade não seja mais usada. Por exemplo, na gestão do grupo, em vez de utilizar:

    `<my:Property my:Name="TargetAttribute" my:Value="ExplicitMember"/>`

    Utilização:

    `<my:Property my:Name=”ListFilter” my:Value=”/Group[ObjectID=’%ObjectID%’]/ExplicitMember”/>`

- **ItemClickBehavior**: Esta é uma propriedade opcional, tipo corda. Utilize esta propriedade para especificar se deseja clicar num item de visualização da lista para ativar um registo do servidor ou para exibir uma visão detalhada do item. Dois valores de opção são suportados: ModelessDialog e Server. O valor predefinido é ModelessDialog.

- **SearchOnLoad**: Trata-se de uma propriedade opcional, tipo Boolean, que especifica se o controlo de visualização de listas deve consultar a carga. Esta propriedade só é aplicável quando a vista da lista está no modo de seleção. O valor padrão desta propriedade é verdadeiro. Pode desligá-lo se espera que o utilizador escreva normalmente texto em busca de obter um resultado significativo. Neste caso, a visualização da lista mostra inicialmente uma mensagem a dizer ao utilizador como realizar uma pesquisa. O texto pode ser personalizado pelas seguintes propriedades:

- **MainSearchScreenText**: Esta propriedade opcional, tipo de corda, só é aplicável quando searchOnload é definido como verdadeiro. Esta propriedade pode ser usada para personalizar texto que aparece no meio da vista da lista quando a vista da lista não pesquisa automaticamente. O valor padrão desta propriedade é encontrar os recursos usando a pesquisa, como descrito anteriormente. Pode especificar um valor para tornar o texto mais relevante para o seu cenário.

- **SubSearchScreenText**: Esta propriedade opcional, tipo de corda é usada para personalizar o texto que aparece após a propriedade **MainSearchScreenText.** Normalmente, não tem de especificar um valor para esta propriedade, a menos que queira adicionar algumas instruções adicionais sobre como usar a vista da lista.

**Eventos:**

- Não há eventos para este controlo.

**Exemplo:**

Por exemplo, como utilizar a visualização da lista juntamente com o controlo UocFilterBuilder como uma lista de pré-visualização, consulte as amostras do UocFilterBuilder mais cedo neste documento. O UocListView também pode ser utilizado sem o construtor de filtros.


### <a name="uocnumericbox"></a>UocNumericBox

**Nome**: UocNumericBox

**Descrição**: Esta é uma caixa de texto simples que leva apenas valores inteiros. Este controlo suporta o modo apenas de leitura e o modo de updatable.

**Propriedades:**

- Todas as propriedades comuns: Para obter informações sobre estas propriedades, consulte <a href="#common-properties">propriedades comuns.</a>

- **MaxValue:** Esta é uma propriedade opcional, do tipo inteiro. Utilize esta propriedade para definir uma validação do lado do cliente para o controlo. O valor que o utilizador final introduz não pode exceder este valor. Pode introduzir um número inteiro explícito ou ligá-lo a dados inteiros de uma fonte de dados utilizando {Binding Source=schema, Path=IntegerMaximum}.

- **MinValue**: Esta é uma propriedade opcional, do tipo inteiro. Utilize esta propriedade para definir uma validação do lado do cliente para o controlo. O valor que o utilizador final introduz não pode ser inferior a este valor. Pode introduzir um número inteiro explícito ou ligá-lo a dados inteiros de uma fonte de dados utilizando {Binding Source=schema, Path=IntegerMinimum}.

- **DefaultValue**: Trata-se de uma propriedade opcional, do tipo inteiro. Utilize esta propriedade para definir um valor padrão para o controlo se o controlo for utilizado para criar novos dados. Este valor só pode ser explicitamente definido para um inteiro estático.

- **Valor**: Trata-se de uma propriedade opcional, do tipo inteiro. Quando o liga a dados do tipo inteiro de uma fonte de dados, o valor desse atributo aparece quando a página está carregada e, em seguida, é guardado na fonte de dados após a submissão.

**Eventos:**

- **TextChanged**: Este evento é emitido quando o valor atual dentro do controlo muda.

**Exemplo:**

![Controlo UocNumericBox](media/rcd-configuration-xml-reference/image061.jpg)

>[!NOTE]
>O seguinte código de amostra gera a primeira caixa numérica. A caixa numérica não está ligada a uma fonte de dados ou a qualquer informação de esquema.

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

O seguinte código de amostra gera a segunda caixa numérica.

>[!NOTE]
>Para que esta amostra funcione, primeiro deve criar um novo atributo do tipo inteiro chamado AnIntegerAttribute e ligá-lo ao tipo de recurso personalizado.

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

**Nome**: UocPictureBox

**Descrição**: Este controlo é utilizado para render imagens, dados binários. Recomendamos que este controlo seja utilizado com dados binários. A imagem pode ser renderizada quer por um URL de imagem fornecido, dados de tipo binário, quer pela fonte de atributo que contém dados do tipo imagem.

**Propriedades:**

- Todas as propriedades comuns: Para obter informações sobre estas propriedades, consulte <a href="#common-properties">propriedades comuns.</a>

- **ImageUrl**: Esta é uma propriedade opcional, tipo corda. Introduza o URL da imagem do alvo.

- **MaxHeight**: Trata-se de uma propriedade opcional, tipo de corda. Define a altura máxima da imagem a renderizar em pixéis.

- **MaxWidth:** Esta é uma propriedade opcional, tipo corda. Define a largura máxima da imagem a renderizar em pixéis.

- **ImageData**: Trata-se de uma propriedade binária. Utilize esta propriedade para ligar uma fonte de dados com a imagem apresentada. A fonte de dados vinculada tem de ser binária. Também pode utilizar este campo para definir explicitamente uma imagem fornecendo dados no formato byte[]

- **ImageResource**: Trata-se de uma propriedade opcional, tipo binário.

- **AlternativeText**: Esta é uma propriedade opcional, tipo corda. Esta propriedade aparece como texto alternativo quando a imagem não pode ser exibida.

**Eventos:**

- Não há eventos para este controlo.

**Exemplo:**

>[!NOTE]
>Para utilizar esta amostra, deve ter um dado de imagem existente ligado ao controlo.

O seguinte segmento de código gera um controlo de caixa de imagem que liga uma fonte de dados ao controlo:

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

O seguinte segmento de código gera um controlo de caixa de imagem que liga uma imagem URL com o controlo:

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

**Nome**: UocRadioButtonList

**Descrição**: Esta é uma lista simples de botões de opção. As escolhas são mutuamente exclusivas nesta lista. Este controlo é recomendado quando os utilizadores têm cinco ou menos opções para escolher. Caso contrário, recomenda-se o UOCListView.

**Propriedades:**

- Todas as propriedades comuns: Para obter informações sobre estas propriedades, consulte <a href="#common-properties">propriedades comuns.</a>

- **ValuePath**: O caminho de valor está definido para Valor. Liga-se ao campo Valor a partir do elemento Opção, conforme descrito nesta secção.

- **CaptionPath**: O caminho de valor está definido para legenda. Liga-se ao campo de legendagem do elemento Opção, conforme descrito nesta secção.

- **HintPath**: O caminho de valor está definido para Sugestão. Liga-se ao campo De sugestão a partir do elemento Opção, conforme descrito nesta secção.

- **SeleccionadoValue**: O valor atualmente selecionado. Esta é uma propriedade necessária, tipo corda. Esta propriedade liga-se com dados de cadeia da fonte de dados.

**Eventos:**

- **SeleccionadoIndexChanged**: O evento ocorre quando o botão de rádio selecionado muda.

- **Verificado :** Quando o botão de rádio muda de estado, este evento é emitido.

**Opções:**

Só pode haver dois **elementos opções** como opções para este controlo. Para a estrutura de um elemento **Opções,** consulte <a href="#options-element">o elemento Opções</a>.

- **Valor**: O campo Valor num único elemento de Opção tem de ser definido para Verdadeiro ou Falso.

- **Legenda:** Isto pode ser qualquer valor de corda.

- **Atenção:** Isto pode ser qualquer valor de corda.

**Exemplo:**

![Controlo UocRadioButtonList](media/rcd-configuration-xml-reference/image063.jpg)

>[!NOTE]
>Para que esta amostra funcione, deve criar um novo atributo Boolean, ABooleanAttribute, e ligá-lo ao seu tipo de recurso personalizado.

O seguinte segmento de código cria uma lista de botões de opção:

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

**Nome**: UocSimpleRadioButton

**Descrição**: Trata-se de um controlo simples e de botão de opção. A utilização deste controlo é semelhante a uma simples caixa de verificação. Existem dois botões de opção, mostrando lado a lado com a rotulagem de texto. Recomenda-se a ligação do controlo aos dados do tipo Boolean.

**Propriedades:**

- Todas as propriedades comuns: Para obter informações sobre estas propriedades, consulte <a href="#common-properties">propriedades comuns.</a>

- **TrueText**: Esta é uma propriedade opcional, tipo corda. Este é o texto que aparece quando o botão de opção é selecionado.

- **FalseText**: Esta é uma propriedade opcional, tipo corda. Este é o texto que aparece quando o botão de opção não é selecionado.

- **SelectedItem**: Esta é uma propriedade opcional, tipo Boolean. Este valor indica que o botão de opção é selecionado. Isto pode ligar-se a dados do tipo Boolean a partir de uma fonte de dados. O valor predefinido é definido como falso.

**Eventos:**

- **Verificado :** Quando o botão de opção muda de estado de selecionado para não selecionado, ou o contrário, este sinal é emitido.

**Exemplo:**

![Controlo UocSimpleRadioButton](media/rcd-configuration-xml-reference/image066.png)

>[!NOTE]
>Para fazer a amostra funcionar, você deve criar um novo atributo Boolean ABooleanAttribute e ligá-lo ao seu tipo de recurso personalizado. Os dados do RCDC são aplicados ao mesmo tipo de recurso personalizado.

O seguinte segmento de código gera um botão de opção:

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

**Nome**: UocTextBox

**Descrição**: Esta é uma caixa de texto simples que suporta a entrada do tipo de corda. Recomendamos que utilize este controlo para se ligar a dados do tipo de corda.

**Propriedades:**

- Todas as propriedades comuns: Para obter informações sobre estas propriedades, consulte <a href="#common-properties">propriedades comuns.</a>

- **MaxLength**: Este é um atributo opcional, do tipo inteiro. Esta propriedade especifica o comprimento máximo para uma entrada de corda. O valor padrão desta propriedade é de 128 caracteres.

- **Texto**: Esta é uma propriedade opcional, tipo corda. Este é o texto que aparece na caixa de texto. Pode definir uma cadeia explícita que aparece na caixa de texto durante o carregamento inicial do controlo ou ligá-la a um atributo de esquema de um tipo de corda.

- **Linhas**: Esta é uma propriedade opcional, do tipo inteiro. Esta propriedade define a altura da caixa de texto em unidades de caracteres. O valor predefinido é um personagem.

- **Colunas**: Trata-se de uma propriedade opcional, do tipo inteiro. Esta propriedade define a largura da caixa de texto em unidades de caracteres. O valor predefinido é de 20 caracteres.

- **Wrap**: Esta é uma propriedade opcional, tipo Boolean. Ao definir o valor desta propriedade para verdadeiro, o utilizador permite a função De Embrulho word na caixa de texto. O valor padrão desta propriedade é definido como verdadeiro.

- **UniquenessValidationXPath**: Esta é uma propriedade opcional, tipo corda. É necessária uma expressão de filtro FIM XPath válida e garante que a entrada de valor pelo utilizador é única dentro dos recursos que estão no âmbito do filtro. Por exemplo, para garantir que o nome de exibição solicitado pelo utilizador é único dentro de todos os grupos de segurança habilitados por correio no Serviço FIM DB, utilizaria o XPath `/Group[DisplayName=’%VALUE%’ and Type=’MailEnabledSecurity’` . A ação de validação é realizada quando o utilizador sai da página. Esta propriedade é suportada apenas no RCDC para a criação de um recurso.

- **UniquenessErrorMessage**: Esta é uma propriedade opcional, tipo corda. Esta cadeia é usada para exibir uma mensagem de erro se a validação UniquenessValidationXPath falhar e puder ser texto explícito ou uma Variável de Recursos de Cadeia. Se esta propriedade não for especificada, a mensagem de erro por defeito para uma validação falhada é "%VALUE% já existe. Por favor, tente um diferente.

**Eventos:**

- **TextChanged**: Este evento é emitido quando o texto dentro da caixa de texto é alterado.

**Exemplo:**

Consulte a secção de amostras de controlo simples para obter uma amostra completa deste controlo.


<br/>
<h2 id="appendix-a">Apêndice A: Esquema XSD padrão</h2>

Esta secção mostra o esquema XSD completo para todos os RCDCs predefinidos que são fornecidos com Microsoft Identity Manager 2016 SP1.

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

<br/>
<h2 id="appendix-b">Apêndice B: Resumo Padrão XSL</h2>

Esta secção mostra o XSL completo que é fornecido com o Microsoft Identity Manager 2016 SP1.

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
