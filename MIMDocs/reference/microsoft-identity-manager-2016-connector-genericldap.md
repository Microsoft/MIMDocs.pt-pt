---
title: '| genérico do conector LDAP Microsoft Docs'
description: Este artigo descreve como configurar o Conector LDAP Genérico da Microsoft.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 984beeb0-4d91-4908-ad81-c19797c4891b
ms.reviewer: davidste
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 06/26/2018
ms.author: billmath
ms.openlocfilehash: 5b19b4fd9d45797fcc6b02091386a27aec3c0abf
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835728"
---
# <a name="generic-ldap-connector-technical-reference"></a>Referência técnica do Conector do LDAP Genérico
Este artigo descreve o Conector LDAP Genérico. O artigo aplica-se aos seguintes produtos:

* Microsoft Identity Manager 2016 (MIM2016)
* Gestor de Identidades Da Vanguarda 2010 R2 (FIM2010R2)
  * Deve utilizar o hotfix 4.1.3671.0 ou mais tarde [KB3092178](https://support.microsoft.com/kb/3092178).

Para MIM2016 e FIM2010R2, o Conector está disponível como download do [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkId=717495).

Quando se refere aos RFCs do IETF, este documento está a utilizar o formato (RFC [número RFC]/[secção no documento RFC]), por exemplo (RFC 4512/4.3).
Pode encontrar mais informações em [https://tools.ietf.org/](https://tools.ietf.org/) . No painel esquerdo, introduza um número RFC na caixa de diálogo **doc fetch** e teste-o para se certificar de que é válido.

## <a name="overview-of-the-generic-ldap-connector"></a>Visão geral do Conector LDAP Genérico
O Conector LDAP Genérico permite-lhe integrar o serviço de sincronização com um servidor LDAP v3.

Determinadas operações e elementos de esquema, tais como os necessários para a realização da importação delta, não estão especificados nos RFCs do IETF. Para estas operações, apenas são apoiados os diretórios LDAP explicitamente especificados.

Para nos ligarmos aos diretórios, testamos utilizando a conta raiz/administração.  Para utilizar uma conta diferente para aplicar mais permissões granulares, poderá ter de rever com a sua equipa de diretórios LDAP.

De uma perspetiva de alto nível, as seguintes funcionalidades são suportadas pela atual libertação do conector:

| Funcionalidade | Suporte |
| --- | --- |
| Fonte de dados conectada |O Conector é suportado com todos os servidores LDAP v3 (compatível com RFC 4510). Foi testado com o seguinte: <li>Microsoft Active Directory Lightweight Directory Services (AD LDS)</li><li>Microsoft Ative Directory Global Catalog (AD GC)</li><li>389 Servidor de Diretório</li><li>Servidor de Diretório Apache</li><li>IBM Tivoli DS</li><li>Diretório de Isode</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>DJ aberto</li><li>Open DS</li><li>LDAP Aberto (openldap.org)</li><li>Oracle (anteriormente Sun) Directory Server Enterprise Edition</li><li>Servidor de Diretório Virtual RadiantOne (VDS)</li><li>Servidor de Diretório Sun One</li><li>Microsoft Ative Directory Domain Services (AD DS)</li><ul><li>Para a maioria dos cenários, deve utilizar o Conector ative ative incorporado, uma vez que algumas funcionalidades podem não funcionar</li></ul>**Notáveis diretórios conhecidos ou funcionalidades não suportadas:**<li>Microsoft Ative Directory Domain Services (AD DS)<ul><li>Serviço de Notificação de Alteração de Palavra-Passe (PCNS)</li><li>Provisão cambial</li><li>Eliminar dispositivos de sincronização ativa</li><li>Suporte para nTDescurityDescriptor</li></ul></li><li>Diretório de Internet oracle (OID)</li> |
| Cenários |<li>Gestão do ciclo de vida de objetos</li><li>Gestão de Grupos</li><li>Gestão de palavra-passe</li> |
| Operações |As seguintes operações são apoiadas em todos os diretórios LDAP: <li>Importação Completa</li><li>Exportar</li>As seguintes operações só são suportadas em diretórios especificados:<li>Importação delta</li><li>Definir palavra-passe, alterar senha</li> |
| Esquema |<li>O esquema é detetado a partir do esquema LDAP (RFC3673 e RFC4512/4.2)</li><li>Suporta classes estruturais, classes aux e classe de objetos extensíveisObject (RFC4512/4.3)</li> |

### <a name="delta-import-and-password-management-support"></a>Delta importe e suporte de gestão de passwords
Diretórios apoiados para a gestão da importação e password da Delta:

* Microsoft Active Directory Lightweight Directory Services (AD LDS)
  * Suporta todas as operações de importação delta
  * Suporta definir palavra-passe
* Microsoft Ative Directory Global Catalog (AD GC)
  * Suporta todas as operações de importação delta
  * Suporta definir palavra-passe
* 389 Servidor de Diretório
  * Suporta todas as operações de importação delta
  * Suporta definir palavra-passe e alterar senha
* Servidor de Diretório Apache
  * Não suporta a importação delta uma vez que este diretório não tem um registo de mudança persistente
  * Suporta definir palavra-passe
* IBM Tivoli DS
  * Suporta todas as operações de importação delta
  * Suporta definir palavra-passe e alterar senha
* Diretório de Isode
  * Suporta todas as operações de importação delta
  * Suporta definir palavra-passe e alterar senha
* Novell eDirectory e NetIQ eDirectory
  * Suporta operações de adicionar, atualizar e renomear para importações delta
  * Não suporta Eliminar operações para importação delta
  * Suporta definir palavra-passe e alterar senha
* DJ aberto
  * Suporta todas as operações de importação delta
  * Suporta definir palavra-passe e alterar senha
* Open DS
  * Suporta todas as operações de importação delta
  * Suporta definir palavra-passe e alterar senha
* LDAP Aberto (openldap.org)
  * Suporta todas as operações de importação delta
  * Suporta definir palavra-passe
  * Não suporta alterar palavra-passe
* Oracle (anteriormente Sun) Directory Server Enterprise Edition
  * Suporta todas as operações de importação delta
  * Suporta definir palavra-passe e alterar senha
* Servidor de Diretório Virtual RadiantOne (VDS)
  * Deve estar a usar a versão 7.1.1 ou superior
  * Suporta todas as operações de importação delta
  * Suporta definir palavra-passe e alterar senha
* Servidor de Diretório Sun One
  * Suporta todas as operações de importação delta
  * Suporta definir palavra-passe e alterar senha

### <a name="prerequisites"></a>Pré-requisitos
Antes de utilizar o Conector, certifique-se de que tem o seguinte no servidor de sincronização:

* Microsoft .NET 4.5.2 Framework ou mais tarde

### <a name="detecting-the-ldap-server"></a>Deteção do servidor LDAP
O Conector conta com várias técnicas para detetar e identificar o servidor LDAP. O Conector utiliza o Root DSE, nome/versão do fornecedor, e inspeciona o esquema para encontrar objetos e atributos únicos conhecidos por existirem em certos servidores LDAP. Estes dados, se encontrados, são utilizados para pré-povoar as opções de configuração no Conector.

### <a name="connected-data-source-permissions"></a>Permissões de Fonte de Dados Conectadas
Para efetuar operações de importação e exportação dos objetos do diretório ligado, a conta de conector deve ter permissões suficientes. O conector precisa de autorizações de escrita para poder exportar e ler permissões para poder importar. A configuração de permissão é realizada dentro das experiências de gestão do próprio diretório alvo.

### <a name="ports-and-protocols"></a>Portos e protocolos
O conector utiliza o número de porta especificado na configuração, que por predefinição é 389 para LDAP e 636 para LDAPS.

Para LDAPS, deve utilizar SSL 3.0 ou TLS. O SSL 2.0 não está suportado e não pode ser ativado.

### <a name="required-controls-and-features"></a>Controlos e características necessários
Os seguintes controlos/funcionalidades LDAP devem estar disponíveis no servidor LDAP para que o conector funcione corretamente:  
`1.3.6.1.4.1.4203.1.5.3` Filtros verdadeiros/falsos

O filtro True/False não é frequentemente reportado como suportado por diretórios LDAP e pode aparecer na **Página Global** em **Funcionalidades Obrigatórias Não Encontradas**. É utilizado para criar filtros **OR** em consultas LDAP, por exemplo, quando importa vários tipos de objetos. Se puder importar mais do que um tipo de objeto, então o seu servidor LDAP suporta esta funcionalidade.

Se utilizar um diretório onde um identificador único é a âncora, também deve estar disponível (Para mais informações, consulte a secção [Âncoras configuradas):](#configure-anchors)  
`1.3.6.1.4.1.4203.1.5.1` Todos os atributos operacionais

Se o diretório tiver mais objetos do que o que pode caber numa chamada para o diretório, então é aconselhável utilizar a paging. Para a pesfiliar para o trabalho, precisa de uma das seguintes opções:

**Opção 1:**  
`1.2.840.113556.1.4.319` pagedResultsControl

**Opção 2:**  
`2.16.840.1.113730.3.4.9` VLVControl  
`1.2.840.113556.1.4.473` SortControl

Se ambas as opções estiverem ativadas na configuração do conector, é utilizado o PagedResultsControl.

`1.2.840.113556.1.4.417` ShowDeletedControl

O ShowDeletedControl é utilizado apenas com o método de importação delta USNChanged para poder ver objetos eliminados.

O conector tenta detetar as opções presentes no servidor. Se as opções não puderem ser detetadas, um aviso está presente na página Global nas propriedades do conector. Nem todos os servidores LDAP apresentam todos os controlos/funcionalidades que suportam e mesmo que este aviso esteja presente, o conector pode funcionar sem problemas.

### <a name="delta-import"></a>Importação delta
A importação delta só está disponível quando for detetado um diretório de apoio. São atualmente utilizados os seguintes métodos:

* Diálogo de acesso LDAP. Ver [ http://www.openldap.org/doc/admin24/overlays.html#Access Registo](http://www.openldap.org/doc/admin24/overlays.html#Access%20Logging)
* LDAP Changelog. Ver [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
* Hora de jogo. Para o eDirectory Novell/NetIQ, o Conector utiliza a última data/hora para obter objetos criados e atualizados. O eDirectory Novell/NetIQ não fornece meios equivalentes para recuperar objetos eliminados. Esta opção também pode ser utilizada se nenhum outro método de importação delta estiver ativo no servidor LDAP. Esta opção não é capaz de importar objetos eliminados.
* USN Mudou. Ver: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Não suportado
As seguintes funcionalidades LDAP não são suportadas:

* Referências LDAP entre servidores (RFC 4511/4.1.10)

## <a name="create-a-new-connector"></a>Criar um novo Conector
Para criar um conector LDAP genérico, no **Serviço de Sincronização** selecione **o Agente de Gestão** e **crie**. Selecione o **Conector LDAP Genérico (Microsoft).**

![CriarConnector](./media/microsoft-identity-manager-2016-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Conectividade
Na página conectividade, deve especificar as informações de Anfitrião, Porto e Encadernação. Dependendo da seleção da Ligação, poderão ser fornecidas informações adicionais nas seguintes secções.

![Conectividade](./media/microsoft-identity-manager-2016-connector-genericldap/connectivity.png)

* A definição de tempo de ligação só é utilizada para a primeira ligação ao servidor ao detetar o esquema.
* Se a Ligação for Anónima, não é utilizado nem o nome de utilizador/palavra-passe nem o certificado.
* Para outras ligações, introduza informações no nome de utilizador/palavra-passe ou selecione um certificado.
* Se estiver a utilizar kerberos para autenticar, então também forneça o Domínio/Domínio do utilizador.

A caixa de texto **de aliases de atributos** é usada para atributos definidos no esquema com sintaxe RFC4522. Estes atributos não podem ser detetados durante a deteção do esquema e o Conector precisa de ajuda para identificar esses atributos. Por exemplo, deve ser introduzido o seguinte na caixa de pseudónimos do atributo para identificar corretamente o atributo userCertificate como um atributo binário:

`userCertificate;binary`

O seguinte é um exemplo para a forma como esta configuração pode parecer:

![Conectividade](./media/microsoft-identity-manager-2016-connector-genericldap/connectivityattributes.png)

Selecione os **atributos operacionais incluídos na** caixa de verificação de esquemas para incluir também atributos criados pelo servidor. Estes incluem atributos como quando o objeto foi criado e última hora de atualização.

**Selecione Incluir atributos extensíveis no esquema** se forem utilizados objetos extensíveis (RFC4512/4.3) e permitir que esta opção permita que todos os atributos sejam utilizados em todos os objetos. A seleção desta opção torna o esquema muito grande, por isso, a menos que o diretório ligado esteja a usar esta função, a recomendação é manter a opção não selecionada.

### <a name="global-parameters"></a>Parâmetros de Globais
Na página De Parâmetros Globais, configura o DN para o registo de alteração delta e funcionalidades LDAP adicionais. A página é pré-povoada com as informações fornecidas pelo servidor LDAP.

![Conectividade](./media/microsoft-identity-manager-2016-connector-genericldap/globalparameters.png)

A secção superior mostra informações fornecidas pelo próprio servidor, como o nome do servidor. O Conector também verifica que os controlos obrigatórios estão presentes na DSE raiz. Se estes controlos não estiverem listados, é apresentado um aviso. Alguns diretórios LDAP não listam todas as funcionalidades do DSE raiz e é possível que o Conector funcione sem problemas, mesmo que exista um aviso.

As caixas de verificação **suportadas controlam** o comportamento de determinadas operações:

* Com a eliminação da árvore selecionada, uma hierarquia é eliminada com uma chamada LDAP. Com a eliminação da árvore não selecionado, o conector faz uma eliminação recursiva se necessário.
* Com os resultados paged selecionados, o Conector faz uma importação de página com o tamanho especificado nos passos de execução.
* O VLVControl e o SortControl são uma alternativa ao pagedResultsControl para ler dados do diretório LDAP.
* Se as três opções (pagedResultsControl, VLVControl e SortControl) não forem selecionadas, então o Conector importa todos os objetos numa única operação, o que pode falhar se for um grande diretório.
* O ShowDeletedControl só é utilizado quando o método de importação Delta é USNChanged.

O registo de alterações DN é o contexto de nomeação utilizado pelo registo de alteração delta, por exemplo **cn=changelog**. Este valor deve ser especificado para poder fazer importações delta.

Segue-se uma lista de DNs de registo de alteração predefinido:

| Diretório | Registo de mudança delta |
| --- | --- |
| Microsoft AD LDS e AD GC |Detetado automaticamente. USN Mudou. |
| Servidor de Diretório Apache |Não disponível. |
| Diretório 389 |Alterar registo. Valor predefinido a utilizar: **cn=changelog** |
| IBM Tivoli DS |Alterar registo. Valor predefinido a utilizar: **cn=changelog** |
| Diretório de Isode |Alterar registo. Valor predefinido a utilizar: **cn=changelog** |
| Novell/NetIQ eDirectory |Não disponível. Hora de jogo. O Conector utiliza a última data/hora atualizada para obter registos adicionados e atualizados. |
| DJ/DS aberto |Alterar registo.  Valor predefinido a utilizar: **cn=changelog** |
| LDAP aberto |Registo de acesso. Valor predefinido a utilizar: **cn=accesslog** |
| Oráculo DSEE |Alterar registo. Valor predefinido a utilizar: **cn=changelog** |
| VDs radiantes |Diretório virtual. Depende do diretório ligado ao VDS. |
| Servidor de Diretório Sun One |Alterar registo. Valor predefinido a utilizar: **cn=changelog** |

O atributo palavra-passe é o nome do atributo que o Conector deve utilizar para definir a palavra-passe nas operações de alteração de palavra-passe e de definição de palavra-passe.
Este valor é por padrão definido para **userPassword** mas pode ser alterado quando necessário para um determinado sistema LDAP.

Na lista adicional de divisórias, é possível adicionar espaços de nome adicionais não detetados automaticamente. Por exemplo, esta definição pode ser usada se vários servidores constam de um cluster lógico, que deve ser importado ao mesmo tempo. Assim como o Ative Directory pode ter vários domínios numa floresta, mas todos os domínios partilham um esquema, o mesmo pode ser simulado introduzindo os espaços de nomes adicionais nesta caixa. Cada espaço de nome pode importar de diferentes servidores e é configurado na página Configure Partitions and Hierarquias. Use ctrl+Enter para obter uma nova linha.

### <a name="configure-provisioning-hierarchy"></a>Configure Hierarquia de Provisionamento
Esta página é usada para mapear o componente DN, por exemplo, para o tipo de objeto que deve ser aprovisionado, por exemplo, a unidade organizacional.

![Hierarquia de Provisionamento](./media/microsoft-identity-manager-2016-connector-genericldap/provisioninghierarchy.png)

Ao configurar a hierarquia de provisionamento, pode configurar o Conector para criar automaticamente uma estrutura quando necessário. Por exemplo, se houver um namespace dc=contoso,dc=com e um novo objeto cn=Joe, ou=Seattle, c=US, dc=contoso, dc=com é provisionado, então o Connector pode criar um objeto de tipo país para os EUA e uma unidade organizacional para Seattle se estes não estiverem já presentes no diretório.

### <a name="configure-partitions-and-hierarchies"></a>Configurar Partições e Hierarquias
Na página de divisórias e hierarquias, selecione todos os espaços de nome com objetos que planeia importar e exportar.

![Partições](./media/microsoft-identity-manager-2016-connector-genericldap/partitions.png)

Para cada espaço de nome, também é possível configurar definições de conectividade que sobreponha os valores especificados no ecrã de Conectividade. Se estes valores forem deixados ao seu valor em branco predefinido, as informações do ecrã de Conectividade são utilizadas.

É igualmente possível selecionar quais os contentores e OUs que o Conector deve importar e exportar para.

Ao efetuá-lo, isto é feito em todos os recipientes da partição. Nos casos em que há um grande número de contentores este comportamento leva à degradação do desempenho.

> [!NOTE]
> A partir da atualização de março de 2017 às pesquisas genéricas do conector LDAP podem ser limitadas no âmbito apenas aos contentores selecionados. Isto pode ser feito selecionando a caixa de verificação 'Procurar apenas em recipientes selecionados', como mostra a imagem abaixo.

![Pesquisar apenas recipientes selecionados](./media/microsoft-identity-manager-2016-connector-genericldap/partitions-only-selected-containers.png)

### <a name="configure-anchors"></a>Configurar Âncoras
Esta página tem sempre um valor pré-configurado e não pode ser alterada. Se o fornecedor do servidor tiver sido identificado, então a âncora pode ser povoada com um atributo imutável, por exemplo o GUID para um objeto. Se não tiver sido detetado ou se souber que não tem um atributo imutável, então o conector utiliza dn (nome distinto) como âncora.

![âncoras](./media/microsoft-identity-manager-2016-connector-genericldap/anchors.png)


Segue-se uma lista de servidores LDAP e a âncora a ser utilizada:

| Diretório | Atributo de âncora |
| --- | --- |
| Microsoft AD LDS e AD GC |objectGUID |
| 389 Servidor de Diretório |dn |
| Diretório Apache |dn |
| IBM Tivoli DS |dn |
| Diretório de Isode |dn |
| Novell/NetIQ eDirectory |GUID |
| DJ/DS aberto |dn |
| LDAP aberto |dn |
| Oráculo ODSEE |dn |
| VDs radiantes |dn |
| Servidor de Diretório Sun One |dn |

## <a name="other-notes"></a>Outras notas
Esta secção fornece informações sobre aspetos específicos deste Conector ou por outras razões são importantes de saber.

### <a name="delta-import"></a>Importação delta
A marca de água delta em LDAP aberto é data/hora UTC. Por esta razão, os relógios entre o Serviço de Sincronização FIM e o LDAP Aberto devem ser sincronizados. Caso contrário, podem ser omitidas algumas entradas no registo de alteração delta.

Para a Novell eDirectory, a importação delta não deteta quaisquer exclusões de objetos. Por esta razão, é necessário executar uma importação completa periodicamente para encontrar todos os objetos eliminados.

Para diretórios com um registo de alteração delta que se baseia na data/hora, é altamente recomendado executar uma importação completa em horários periódicos. Este processo permite que o motor de sincronização encontre e dissimilaridades entre o servidor LDAP e o que está atualmente no espaço do conector.

## <a name="troubleshooting"></a>Resolução de problemas
* Para obter informações sobre como permitir a sessão de registo para resolver problemas no conector, consulte o [Rastreio ETW para Conectores](https://go.microsoft.com/fwlink/?LinkId=335731).
