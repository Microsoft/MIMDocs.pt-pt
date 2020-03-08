---
title: Conector Genérico LDAP [ Conector Genérico LDAP ] Microsoft Docs
description: Este artigo descreve como configurar o Conector LDAP genérico da Microsoft.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
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
ms.openlocfilehash: b8ed1b605b0a3c01ffd69a329f26583f1cb0a019
ms.sourcegitcommit: d98a76d933d4d7ecb02c72c30d57abe3e7f5d015
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78853707"
---
# <a name="generic-ldap-connector-technical-reference"></a>Referência técnica genérica do conector LDAP
Este artigo descreve o Conector Genérico LDAP. O artigo aplica-se aos seguintes produtos:

* Microsoft Identity Manager 2016 (MIM2016)
* Gestor de Identidade de Vanguarda 2010 R2 (FIM2010R2)
  * Deve utilizar o hotfix 4.1.3671.0 ou mais tarde [KB3092178](https://support.microsoft.com/kb/3092178).

Para MIM2016 e FIM2010R2, o Conector está disponível como download do [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

Ao referir-se aos RFCs IETF, este documento está a utilizar o formato (RFC [número RFC]/[secção no documento RFC]), por exemplo (RFC 4512/4.3).
Pode encontrar mais informações em [https://tools.ietf.org/. ](https://tools.ietf.org/) No painel esquerdo, introduza um número RFC na caixa de diálogo **do Doc e** teste-o para se certificar de que é válido.

## <a name="overview-of-the-generic-ldap-connector"></a>Visão geral do Conector Genérico LDAP
O Conector Genérico LDAP permite-lhe integrar o serviço de sincronização com um servidor LDAP v3.

Determinadas operações e elementos de esquema, tais como os necessários para a importação delta, não estão especificados nos RFCs iETF. Para estas operações, apenas são apoiados os diretórios LDAP explicitamente especificados.

Para a ligação aos diretórios, testamos utilizando a conta raiz/administração.  Para utilizar uma conta diferente para aplicar mais permissões granulares, poderá ter de rever com a sua equipa de diretório LDAP.

Do ponto de vista de alto nível, as seguintes funcionalidades são suportadas pela versão atual do conector:

| Funcionalidade | Suporte |
| --- | --- |
| Fonte de dados conectados |O Conector é suportado com todos os servidores LDAP v3 (conforme RFC 4510). Foi testado com o seguinte: <li>Microsoft Ative Directory Lightweight Directory Diretório S (AD LDS)</li><li>Catálogo Global do Diretório Ativo da Microsoft (AD GC)</li><li>389 Servidor de Diretório</li><li>Servidor de Diretório Apache</li><li>IBM Tivoli DS</li><li>Diretório de Isode</li><li>NetIQ eDirectia</li><li>Novell eDirectory</li><li>DJ aberto</li><li>Open DS</li><li>LDAP aberto (openldap.org)</li><li>Oracle (anteriormente Sun) Directory Server Enterprise Edition</li><li>Servidor de diretório virtual Radiante (VDS)</li><li>Servidor de diretório Sun One</li><li>Microsoft Ative Directory Domain Services (AD DS)</li><ul><li>Para a maioria dos cenários, deve utilizar o Conector de Diretório Ativo incorporado, uma vez que algumas funcionalidades podem não funcionar</li></ul>**Notáveis diretórios ou funcionalidades não suportadas:**<li>Microsoft Ative Directory Domain Services (AD DS)<ul><li>Serviço de Notificação de Alteração de Password (PCNS)</li><li>Provisionamento cambial</li><li>Eliminar de dispositivos de sincronização ativa</li><li>Suporte para nTDescurityDescriptor</li></ul></li><li>Diretório de Internet da Oracle (OID)</li> |
| Cenários |<li>Gestão do ciclo de vida do objeto</li><li>Gestão de Grupos</li><li>Gestão de Passwords</li> |
| Operações |As seguintes operações são apoiadas em todos os diretórios LDAP: <li>Importação Completa</li><li>Exportar</li>As seguintes operações só são apoiadas em diretórios especificados:<li>Importação delta</li><li>Definir palavra-passe, alterar palavra-passe</li> |
| Esquema |<li>O schema é detetado a partir do esquema LDAP (RFC3673 e RFC4512/4.2)</li><li>Suporta classes estruturais, classes aux e classe de objetos extensíveisObjecto (RFC4512/4.3)</li> |

### <a name="delta-import-and-password-management-support"></a>Suporte de gestão de importações delta e passwords
Diretórios suportados para a importação delta e gestão de passwords:

* Microsoft Ative Directory Lightweight Directory Diretório S (AD LDS)
  * Apoia todas as operações de importação de delta
  * Suportes Definir Palavra-passe
* Catálogo Global do Diretório Ativo da Microsoft (AD GC)
  * Apoia todas as operações de importação de delta
  * Suportes Definir Palavra-passe
* 389 Servidor de Diretório
  * Apoia todas as operações de importação de delta
  * Suportes Definir Palavra-passe e alterar palavra-passe
* Servidor de Diretório Apache
  * Não suporta a importação de delta uma vez que este diretório não tem um registo de mudança persistente
  * Suportes Definir Palavra-passe
* IBM Tivoli DS
  * Apoia todas as operações de importação de delta
  * Suportes Definir Palavra-passe e alterar palavra-passe
* Diretório de Isode
  * Apoia todas as operações de importação de delta
  * Suportes Definir Palavra-passe e alterar palavra-passe
* Novell eDirectory e NetIQ eDirectory
  * Suportes Adicionar, Atualizar e Renomear operações para a importação de delta
  * Não suporta apagar operações para a importação de delta
  * Suportes Definir Palavra-passe e alterar palavra-passe
* DJ aberto
  * Apoia todas as operações de importação de delta
  * Suportes Definir Palavra-passe e alterar palavra-passe
* Open DS
  * Apoia todas as operações de importação de delta
  * Suportes Definir Palavra-passe e alterar palavra-passe
* LDAP aberto (openldap.org)
  * Apoia todas as operações de importação de delta
  * Suportes Definir Palavra-passe
  * Não suporta alterar palavra-passe
* Oracle (anteriormente Sun) Directory Server Enterprise Edition
  * Apoia todas as operações de importação de delta
  * Suportes Definir Palavra-passe e alterar palavra-passe
* Servidor de diretório virtual Radiante (VDS)
  * Deve estar a utilizar a versão 7.1.1 ou superior
  * Apoia todas as operações de importação de delta
  * Suportes Definir Palavra-passe e alterar palavra-passe
* Servidor de diretório Sun One
  * Apoia todas as operações de importação de delta
  * Suportes Definir Palavra-passe e alterar palavra-passe

### <a name="prerequisites"></a>Pré-requisitos
Antes de utilizar o Conector, certifique-se de que tem o seguinte no servidor de sincronização:

* Microsoft .NET 4.5.2 Quadro ou posterior

### <a name="detecting-the-ldap-server"></a>Deteção do servidor LDAP
O Conector baseia-se em várias técnicas para detetar e identificar o servidor LDAP. O Conector utiliza o Root DSE, nome/versão do fornecedor, e inspeciona o esquema para encontrar objetos e atributos únicos conhecidos por existirem em certos servidores LDAP. Estes dados, se encontrados, são utilizados para pré-povoar as opções de configuração no Conector.

### <a name="connected-data-source-permissions"></a>Permissões de Origem de Dados Conectadas
Para efetuar operações de importação e exportação dos objetos do diretório ligado, a conta do conector deve dispor de permissões suficientes. O conector precisa de permissões para poder exportar e ler permissões para poder importar. A configuração da permissão é realizada dentro das experiências de gestão do próprio directório-alvo.

### <a name="ports-and-protocols"></a>Portos e protocolos
O conector utiliza o número de porta especificado na configuração, que por predefinição é 389 para LDAP e 636 para LDAPS.

Para LDAPS, deve utilizar SSL 3.0 ou TLS. O SSL 2.0 não é suportado e não pode ser ativado.

### <a name="required-controls-and-features"></a>Controlos e características necessários
Os seguintes controlos/funcionalidades LDAP devem estar disponíveis no servidor LDAP para que o conector funcione corretamente:  
`1.3.6.1.4.1.4203.1.5.3` filtros verdadeiros/falsos

O filtro True/False não é frequentemente reportado como suportado por diretórios LDAP e pode aparecer na **Página Global** sob **funcionalidades obrigatórias não encontradas**. É utilizado para criar filtros **OR** em consultas LDAP, por exemplo, quando importa vários tipos de objetos. Se conseguir importar mais do que um tipo de objeto, o seu servidor LDAP suporta esta funcionalidade.

Se utilizar um diretório onde um identificador único é a âncora, deve também estar disponível o seguinte (para mais informações, consulte a secção [Configure Anchors):](#configure-anchors)  
`1.3.6.1.4.1.4203.1.5.1` Todos os atributos operacionais

Se o diretório tiver mais objetos do que o que pode caber numa chamada para o diretório, então é aconselhável utilizar a paging. Para fazer a paging para trabalhar, precisa de uma das seguintes opções:

**Opção 1:**  
`1.2.840.113556.1.4.319` pagedResultsControl

**Opção 2:**  
`2.16.840.1.113730.3.4.9` VLVControl  
`1.2.840.113556.1.4.473` SortControl

Se ambas as opções estiverem ativadas na configuração do conector, é utilizado o PagedResultsControl.

`1.2.840.113556.1.4.417` ShowDeletedControl

O ShowDeletedControl é utilizado apenas com o método de importação delta USNChanged para poder ver objetos eliminados.

O conector tenta detetar as opções presentes no servidor. Se as opções não puderem ser detetadas, está presente um aviso na página Global nas propriedades do conector. Nem todos os servidores LDAP apresentam todos os controlos/funcionalidades que suportam e mesmo que este aviso esteja presente, o conector pode funcionar sem problemas.

### <a name="delta-import"></a>Importação delta
A importação delta só está disponível quando um diretório de apoio tiver sido detetado. Atualmente, são utilizados os seguintes métodos:

* LDAP Accesslog. Ver [http://www.openldap.org/doc/admin24/overlays.html#Access Logging](http://www.openldap.org/doc/admin24/overlays.html#Access%20Logging)
* LDAP Changelog. Ver [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
* Carimbo de tempo. Para o eDirecty Novell/NetIQ, o Conector utiliza a data/hora passada para ser criado e atualizado. O romance/NetIQ eDirectory não fornece meios equivalentes para recuperar objetos eliminados. Esta opção também pode ser utilizada se nenhum outro método de importação delta estiver ativo no servidor LDAP. Esta opção não é capaz de importar objetos apagados.
* USNChanged. Ver: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Não suportado
As seguintes características LDAP não são suportadas:

* Referências LDAP entre servidores (RFC 4511/4.1.10)

## <a name="create-a-new-connector"></a>Criar um novo Conector
Para criar um conector Genérico LDAP, no Serviço de **Sincronização** selecione Agente de **Gestão** e **Crie**. Selecione o Conector **Genérico LDAP (Microsoft).**

![CreateConnector](./media/microsoft-identity-manager-2016-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Conectividade
Na página de Conectividade, deve especificar as informações de Anfitrião, Porto e Encadernação. Dependendo da selecione Binding, poderão ser fornecidas informações adicionais nas seguintes secções.

![Conectividade](./media/microsoft-identity-manager-2016-connector-genericldap/connectivity.png)

* A definição de tempo de ligação só é utilizada para a primeira ligação ao servidor ao detetar o esquema.
* Se o Binding for Anonymous, então não são utilizados nomes de utilizador/ palavra-passe ou certificado.
* Para outras ligações, introduza informações em nome de utilizador/senha ou selecione um certificado.
* Se estiver a utilizar kerberos para autenticar, então também forneça o Realm/Domínio do utilizador.

A caixa de texto pseudónimos do **atributo** é utilizada para atributos definidos no esquema com sintaxe RFC4522. Estes atributos não podem ser detetados durante a deteção do esquema e o Conector precisa de ajuda para identificar esses atributos. Por exemplo, devem ser inseridos na caixa de pseudónimos atributos para identificar corretamente o atributo do Certificado de utilizador como um atributo binário:

`userCertificate;binary`

Segue-se um exemplo de como esta configuração pode parecer:

![Conectividade](./media/microsoft-identity-manager-2016-connector-genericldap/connectivityattributes.png)

Selecione os **atributos operacionais incluídos na** caixa de verificação schema para incluir também atributos criados pelo servidor. Estes incluem atributos como quando o objeto foi criado e o último tempo de atualização.

**Selecione Incluir atributos extensíveis em esquema** se forem utilizados objetos extensíveis (RFC4512/4.3) e permitir que esta opção permita que todos os atributos sejam utilizados em todos os objetos. A seleção desta opção torna o esquema muito grande, pelo que, a menos que o diretório conectado esteja a utilizar esta funcionalidade, a recomendação é manter a opção não selecionada.

### <a name="global-parameters"></a>Parâmetros globais
Na página Parâmetros Globais, configura o DN para o registo de alterações delta e funcionalidades lDAP adicionais. A página é pré-povoada com as informações fornecidas pelo servidor LDAP.

![Conectividade](./media/microsoft-identity-manager-2016-connector-genericldap/globalparameters.png)

A secção superior mostra informações fornecidas pelo próprio servidor, como o nome do servidor. O Conector também verifica que os controlos obrigatórios estão presentes na DSE raiz. Se estes controlos não estiverem listados, é apresentado um aviso. Alguns diretórios LDAP não listam todas as funcionalidades no Root DSE e é possível que o Conector funcione sem problemas, mesmo que haja um aviso.

As caixas de verificação **suportadas** controlam o comportamento de determinadas operações:

* Com a eliminação de árvores selecionada, uma hierarquia é eliminada com uma chamada LDAP. Com a eliminação da árvore não selecionada, o conector elimina-se recursivo se necessário.
* Com os resultados pagedos selecionados, o Conector faz uma importação páginada com o tamanho especificado nos passos de execução.
* O VLVControl e o SortControl são uma alternativa ao pagedResultsControl para ler dados do diretório LDAP.
* Se as três opções (pagedResultsControl, VLVControl e SortControl) não forem selecionadas, então o Conector importa todos os objetos numa única operação, o que pode falhar se for um grande diretório.
* ShowDeletedControl só é utilizado quando o método de importação Delta é USNChanged.

O log de alteração DN é o contexto de nomeação utilizado pelo log de mudança delta, por exemplo **cn=changelog**. Este valor deve ser especificado para poder fazer a importação delta.

Segue-se uma lista de DNs de registo de alteração predefinido:

| Active | Delta change log |
| --- | --- |
| Microsoft AD LDS e AD GC |Detetado automaticamente. USNChanged. |
| Servidor de Diretório Apache |Não disponível. |
| Diretório 389 |Alterar o registo. Valor predefinido a utilizar: **cn=changelog** |
| IBM Tivoli DS |Alterar o registo. Valor predefinido a utilizar: **cn=changelog** |
| Diretório de Isode |Alterar o registo. Valor predefinido a utilizar: **cn=changelog** |
| Novell/NetIQ eDirectory |Não disponível. Carimbo de tempo. O Conector utiliza a última data/hora atualizada para obter registos adicionados e atualizados. |
| DJ/DS aberto |Alterar o registo.  Valor predefinido a utilizar: **cn=changelog** |
| LDAP aberto |Registo de acesso. Valor predefinido a utilizar: **cn=accesslog** |
| Oráculo DSEE |Alterar o registo. Valor predefinido a utilizar: **cn=changelog** |
| VDS Radiante |Diretório virtual. Depende do diretório ligado ao VDS. |
| Servidor de diretório Sun One |Alterar o registo. Valor predefinido a utilizar: **cn=changelog** |

O atributo da palavra-passe é o nome do atributo que o Conector deve utilizar para definir a palavra-passe na alteração da palavra-passe e nas operações de conjunto de passwords.
Este valor é definido por padrão para **userPassword,** mas pode ser alterado quando necessário para um determinado sistema LDAP.

Na lista de divisórias adicionais, é possível adicionar espaços de nome adicionais não detetados automaticamente. Por exemplo, esta definição pode ser usada se vários servidores constituem um cluster lógico, que deve ser importado ao mesmo tempo. Assim como o Ative Directory pode ter vários domínios numa floresta, mas todos os domínios partilham um esquema, o mesmo pode ser simulado introduzindo os espaços de nome adicionais nesta caixa. Cada espaço de nome pode importar de diferentes servidores e está configurado na página Configure Partitions and Hierarchies. Utilize ctrl+Enter para obter uma nova linha.

### <a name="configure-provisioning-hierarchy"></a>Configure Hierarquia de Provisionamento
Esta página é usada para mapear o componente DN, por exemplo, ou, para o tipo de objeto que deve ser provisionado, por exemplo organizacionalUnidade.

![Hierarquia de Provisionamento](./media/microsoft-identity-manager-2016-connector-genericldap/provisioninghierarchy.png)

Ao configurar a hierarquia de provisionamento, pode configurar o Conector para criar automaticamente uma estrutura quando necessário. Por exemplo, se houver um espaço de nome dc=contoso,dc=com e um novo objeto cn=Joe, ou=Seattle, c=US, dc=contoso, dc=com é provisionado, então o Connector pode criar um objeto de tipo país para os EUA e uma unidade organizacional para Seattle se esses ainda não estiverem presentes no diretório.

### <a name="configure-partitions-and-hierarchies"></a>Configurar divisórias e hierarquias
Na página de divisórias e hierarquias, selecione todos os espaços de nome com objetos que planeia importar e exportar.

![Partições](./media/microsoft-identity-manager-2016-connector-genericldap/partitions.png)

Para cada espaço de nome, também é possível configurar definições de conectividade que sobreporiam os valores especificados no ecrã de Conectividade. Se estes valores forem deixados ao seu valor em branco padrão, a informação do ecrã de Conectividade é utilizada.

Também é possível selecionar quais os recipientes e OUs de que o Conector deve importar e exportar para.

Ao realizar uma pesquisa, isto é feito em todos os recipientes da divisória. Nos casos em que há um grande número de contentores, este comportamento leva à degradação do desempenho.

> [!NOTE]
> A partir da atualização de março de 2017 para as pesquisas genéricas de conector LDAP pode ser limitada no âmbito apenas aos recipientes selecionados. Isto pode ser feito selecionando a caixa de verificação 'Procurar apenas em recipientes seleccionados', como mostra a imagem abaixo.

![Pesquisar apenas recipientes selecionados](./media/microsoft-identity-manager-2016-connector-genericldap/partitions-only-selected-containers.png)

### <a name="configure-anchors"></a>Configure âncoras
Esta página tem sempre um valor pré-configurado e não pode ser alterada. Se o servidor tiver sido identificado, a âncora poderá ser povoada com um atributo imutável, por exemplo, o GUID para um objeto. Se não tiver sido detetado ou se se souber que não tem um atributo imutável, então o conector utiliza o dn (nome distinto) como âncora.

![âncoras](./media/microsoft-identity-manager-2016-connector-genericldap/anchors.png)


Segue-se uma lista de servidores LDAP e a âncora a ser utilizada:

| Active | Atributo de âncora |
| --- | --- |
| Microsoft AD LDS e AD GC |objectGUID |
| 389 Servidor de Diretório |dn |
| Diretório Apache |dn |
| IBM Tivoli DS |dn |
| Diretório de Isode |dn |
| Novell/NetIQ eDirectory |GUID |
| DJ/DS aberto |dn |
| LDAP aberto |dn |
| OdSEE oráculo |dn |
| VDS Radiante |dn |
| Servidor de diretório Sun One |dn |

## <a name="other-notes"></a>Outras notas
Esta secção fornece informações de aspetos específicos deste Conector ou por outras razões são importantes de saber.

### <a name="delta-import"></a>Importação delta
A marca delta em LDAP aberto é utc data/hora. Por esta razão, os relógios entre o Serviço de Sincronização FIM e o LDAP Aberto devem ser sincronizados. Caso contrário, algumas entradas no registo de mudança delta podem ser omitidas.

Para a Novell eDirectory, a importação delta não está a detetar qualquer exclusão de objetos. Por esta razão, é necessário executar periodicamente uma importação completa para encontrar todos os objetos apagados.

Para os diretórios com um registo de mudança delta que se baseia na data/hora, é altamente recomendado executar uma importação completa em momentos periódicos. Este processo permite que o motor de sincronização encontre e dissemelhanças entre o servidor LDAP e o que está atualmente no espaço do conector.

## <a name="troubleshooting"></a>Resolução de Problemas
* Para obter informações sobre como ativar a exploração de registos para perturbar o conector, consulte o Rastreio de [ETW para conectores](http://go.microsoft.com/fwlink/?LinkId=335731).
