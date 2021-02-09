---
title: '| do conector PowerShell Microsoft Docs'
description: Este artigo descreve como configurar o Conector Windows PowerShell da Microsoft.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 6dba8e34-a874-4ff0-90bc-bd2b0a4199b5
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.prod: microsoft-identity-manager
ms.date: 04/02/2018
ms.author: billmath
ms.openlocfilehash: 0dcba300f70756dbfa7a29011a37839247e6bf8a
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835935"
---
# <a name="windows-powershell-connector-technical-reference"></a>Referência técnica do Conector do Windows PowerShell
Este artigo descreve o Conector Windows PowerShell. O artigo aplica-se aos seguintes produtos:

* Microsoft Identity Manager 2016 (MIM2016)
* Gestor de Identidades Da Vanguarda 2010 R2 (FIM2010R2)
  * Deve utilizar o hotfix 4.1.3671.0 ou mais tarde [KB3092178](https://support.microsoft.com/kb/3092178).

Para MIM2016 e FIM2010R2, o Conector está disponível como download do [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-powershell-connector"></a>Visão geral do Conector PowerShell
O Conector PowerShell permite-lhe integrar o serviço de sincronização com sistemas externos que oferecem APIs baseados no Windows PowerShell. O conector fornece uma ponte entre as capacidades da estrutura extensível de gestão de conectividade extensível 2 (ECMA2) baseada em chamadas e o Windows PowerShell. Para obter mais informações sobre o quadro da ECMA, consulte a Referência do [Agente de Gestão extensível da Conectividade 2.2](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Pré-requisitos
Antes de utilizar o Conector, certifique-se de que tem o seguinte no servidor de sincronização:

* Microsoft .NET 4.5.2 Framework ou mais tarde
* Windows PowerShell 2.0, 3.0 ou 4.0

A política de execução do servidor serviço de sincronização deve ser configurada para permitir que o conector execute scripts Windows PowerShell. A menos que os scripts que o conector executa sejam assinados digitalmente, configuure a política de execução executando este comando:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Criar um novo Conector
Para criar um conector Windows PowerShell no serviço de sincronização, deve fornecer uma série de scripts Windows PowerShell que executam os passos solicitados pelo serviço de sincronização. Dependendo da fonte de dados a que se conecta e da funcionalidade que necessita, os scripts que deve implementar variam. Esta secção descreve cada um dos scripts que podem ser implementados e quando são necessários.

O conector Windows PowerShell foi concebido para armazenar cada um dos scripts dentro da base de dados do Serviço de Sincronização. Embora seja possível executar scripts que são armazenados no sistema de ficheiros, é mais fácil inserir o corpo de cada script diretamente na configuração do conector.

Para criar um conector PowerShell, no **Serviço de Sincronização** selecione **o Agente de Gestão** e **crie**. Selecione o **Conector PowerShell (Microsoft).**

![Criar Conector](./media/microsoft-identity-manager-2016-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Conectividade
Parâmetros de configuração de fornecimento para a ligação a um sistema remoto. Estes valores são armazenados de forma segura pelo Serviço de Sincronização e disponibilizados aos scripts Do Windows PowerShell quando o conector é executado.

![Conectividade](./media/microsoft-identity-manager-2016-connector-powershell/connectivity.png)

Pode configurar os seguintes parâmetros de conectividade:

**Conetividade**


|              Parâmetro               | Valor Predefinido |                                                                                                                                                                                                                  Objetivo                                                                                                                                                                                                                   |
|--------------------------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                Servidor                |    <Blank>    |                                                                                                                                                                                             O nome do servidor a que o conector deve ligar-se.                                                                                                                                                                                              |
|                Domínio                |    <Blank>    |                                                                                                                                                                                    Domínio da credencial para armazenar para utilização quando o conector é executado.                                                                                                                                                                                    |
|                 User                 |    <Blank>    |                                                                                                                                                                                   Nome de utilizador da credencial para armazenar para utilização quando o conector é executado.                                                                                                                                                                                   |
|               Palavra-passe               |    <Blank>    |                                                                                                                                                                                   Palavra-passe da credencial para armazenar para utilização quando o conector é executado.                                                                                                                                                                                   |
|    Imitar Conta de Conector     |     Falso     | Quando é verdade, o serviço de sincronização executa os scripts Windows PowerShell no contexto das credenciais fornecidas. Quando possível, recomenda-se que o **parâmetro $Credentials** seja passado a cada script em vez de personificação. Para obter mais informações sobre permissões adicionais que sejam necessárias para utilizar esta opção, consulte [Configuração Adicional para personificação](#additional-configuration-for-impersonation). |
| Carregue o perfil do utilizador ao fazer-se passar |     Falso     |                          Instrui o Windows a carregar o perfil do utilizador das credenciais do conector durante a personificação. Se o utilizador personificado tiver um perfil de roaming, o conector não carrega o perfil de roaming. Para obter mais informações sobre permissões adicionais necessárias para utilizar este parâmetro, consulte [Configuração Adicional para personificação](#additional-configuration-for-impersonation).                           |
|    Tipo de logon ao personificar     |     Nenhum      |                                                                                                                                                                      Tipo de logon durante a personificação. Para obter mais informações, consulte a documentação [do DwLogonType.][dw]                                                                                                                                                                       |
|         Apenas scripts assinados          |     Falso     |                                                                                                    Se for verdade, o conector Windows PowerShell valida que cada script tem uma assinatura digital válida. Se for falsa, certifique-se de que a política de execução do Windows PowerShell do servidor de Sincronização é assinada remotamente ou sem restrições.                                                                                                     |

**Módulo Comum**  
O conector permite armazenar um módulo Windows PowerShell partilhado na configuração. Quando o conector executa um script, o módulo Windows PowerShell é extraído para o sistema de ficheiros para que possa ser importado por cada script.

Para scripts de Sincronização de Importação, Exportação e Palavra-Passe, o módulo comum é extraído para a pasta MAData do conector. Para os scripts de descoberta de Schema, Validação, Hierarquia e Partição, o módulo comum é extraído para a pasta %TEMP%. Em ambos os casos, o script do módulo comum extraído é nomeado de acordo com a definição de nome do script do módulo comum.

Para carregar um módulo chamado FIMPowerShellConnectorModule.psm1 da pasta MAData, utilize a seguinte declaração: `Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

Para carregar um módulo chamado FIMPowerShellConnectorModule.psm1 da pasta %TEMP% utilize a seguinte declaração: `Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**Validação de parâmetros**  
O Script de Validação é um script opcional do Windows PowerShell que pode ser utilizado para garantir que os parâmetros de configuração do conector fornecidos pelo administrador são válidos. Validar servidor, credenciais de ligação e parâmetros de conectividade são usos comuns do script de validação. O script de validação é chamado após modificar os seguintes separadores e diálogos:

* Conectividade
* Parâmetros de Globais
* Configuração de partição

O script de validação recebe os seguintes parâmetros do conector:

| Name | Tipo de Dados | Descrição |
| --- | --- | --- |
| ConfigParameterPage |[ConfigParameterPage][cpp] |O separador de configuração ou diálogo que desencadeou o pedido de validação. |
| ConfigParameters |[KeyedCollection][keyk] [string, [ConfigParameter][cp]] |Tabela de parâmetros de configuração para o Conector. |
| Credencial |[PSCredential][pscred] |Contém quaisquer credenciais inseridas pelo administrador no separador Conectividade. |

O script de validação deve devolver um único objeto ParameterValidationResult ao pipeline.

**Descoberta de Schema**  
O guião do Schema Discovery é obrigatório. Este script devolve os tipos de objetos, atributos e atribui restrições que o Serviço de Sincronização utiliza ao configurar regras de fluxo de atributos. O script Schema Discovery é executado durante a criação do conector e povoa o esquema do conector. Também é utilizado pela ação Refresh Schema no Gestor de Serviços de Sincronização.

O script de descoberta de esquemas recebe os seguintes parâmetros do conector:

| Name | Tipo de Dados | Descrição |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk] [string, [ConfigParameter][cp]] |Tabela de parâmetros de configuração para o Conector. |
| Credencial |[PSCredential][pscred] |Contém quaisquer credenciais inseridas pelo administrador no separador Conectividade. |

O script deve devolver um único objeto [De Schema][schema] ao oleoduto. O objeto Schema é composto por objetos [SchemaType][schemaT] que representam tipos de objetos (por exemplo: utilizadores e grupos). O objeto SchemaType contém uma coleção de objetos [SchemaAttribute][schemaA] que representam os atributos (por exemplo: nome dado, apelido e endereço postal) do tipo.

**Parâmetros adicionais**  
Além das definições de configuração padrão, pode definir definições de configuração personalizadas adicionais específicas à instância do Conector. Estes parâmetros podem ser especificados nos níveis de conector, partição ou passo de execução e acedidos a partir do script do Windows PowerShell relevante. As definições de configuração personalizada podem ser armazenadas na base de dados do Serviço de Sincronização em formato de texto simples ou podem ser encriptadas. O Serviço de Sincronização encripta e desencripta automaticamente as definições de configuração seguras quando necessário.

Para especificar as definições de configuração personalizadas, separe o nome de cada parâmetro com uma vírgula (, .

Para aceder às definições de configuração personalizadas a partir de um script, deve sufixar o nome com um sublinhado \_ () e o âmbito do parâmetro (Global, Partition ou RunStep). Por exemplo, para aceder ao parâmetro Global FileName, utilize este corte de código: `$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Capacidades
O separador de capacidades do Designer de Agente de Gestão define o comportamento e a funcionalidade do conector. As seleções efetuadas neste separador não podem ser modificadas quando o conector tiver sido criado. Esta tabela lista as definições de capacidade.

![Capacidades](./media/microsoft-identity-manager-2016-connector-powershell/capabilities.png)

| Funcionalidade | Description |
| --- | --- |
| [Estilo de nome distinto][dnstyle] |Indica se o conector suporta nomes distintos e, em caso afirmativo, que estilo. |
| [Tipo de exportação][exportT] |Determina o tipo de objetos que são apresentados ao script Exportação. <li>AttributeReplace – inclui o conjunto completo de valores para um atributo multi-valor quando o atributo muda.</li><li>AttributeUpdate – inclui apenas os deltas a um atributo multi-valor quando o atributo muda.</li><li>MultivaluedReferenceAttributeUpdate - inclui um conjunto completo de valores para atributos multi-valorizados não referenciados e apenas deltas para atributos de referência multi-valor.</li><li>ObjectReplace – inclui todos os atributos para um objeto quando qualquer atributo muda</li> |
| [Normalização de Dados][DataNorm] |Instrui o Serviço de Sincronização a normalizar os atributos de âncora antes de serem fornecidos aos scripts. |
| [Confirmação de Objetos][oconf] |Configura o comportamento de importação pendente no Serviço de Sincronização. <li>Normal – comportamento padrão que espera que todas as alterações exportadas sejam confirmadas através da importação</li><li>NoDeleteConfirmation – quando um objeto é eliminado, não há nenhuma importação pendente gerada.</li><li>NoAddAndDeleteConfirmation – quando um objeto é criado ou eliminado, não há nenhuma importação pendente gerada.</li> |
| Use o DN como Âncora |Se o Estilo de Nome Distinto estiver definido para LDAP, o atributo de âncora para o espaço do conector é também o nome distinto. |
| Operações Simultâneas de Vários Conectores |Quando verificados, vários conectores Windows PowerShell podem funcionar simultaneamente. |
| Partições |Quando verificado, o conector suporta múltiplas divisórias e descoberta de partição. |
| Hierarquia |Quando verificado, o conector suporta uma estrutura hierárquica de estilo LDAP. |
| Permitir a importação |Quando verificado, o conector importa dados através de scripts de importação. |
| Permitir a Importação Delta |Quando verificado, o conector pode solicitar deltas a partir dos scripts de importação. |
| Permitir a exportação |Quando verificado, o conector exporta dados através de scripts de exportação. |
| Ativar a exportação completa |Quando verificados, os scripts de exportação suportam a exportação de todo o espaço do conector. Para utilizar esta opção, o Enable Export também deve ser verificado. |
| Sem valores de referência no primeiro passe de exportação |Quando verificados, os atributos de referência são exportados num segundo passe de exportação. |
| Ativar o renomear objeto |Quando verificados, nomes distintos podem ser modificados. |
| Delete-Add como substituir |Quando verificadas, as operações de exclusão-add são exportadas como uma única substituição. |
| Ativar operações de senha |Quando verificados, os scripts de sincronização de palavras-passe são suportados. |
| Ativar a senha de exportação no primeiro passe |Quando verificadas, as palavras-passe definidas durante o provisionamento são exportadas quando o objeto é criado. |

### <a name="global-parameters"></a>Parâmetros de Globais
O separador Parâmetros Globais no Designer de Agente de Gestão permite-lhe configurar os scripts Windows PowerShell que são executados pelo conector. Também pode configurar valores globais para configurações de configuração personalizadas definidas no separador Conectividade.

**Descoberta da Partição**  
Uma divisória é um espaço de nome separado dentro de um esquema compartilhado. Por exemplo, no Ative Directory cada domínio é uma divisória dentro de uma floresta. Uma partição é o agrupamento lógico para as operações de importação e exportação. As importações e as exportações têm a partição como contexto e todas as operações acontecem neste contexto. As divisórias devem representar uma hierarquia na LDAP. O nome distinto de uma divisória é utilizado na importação para verificar se todos os objetos devolvidos estão no âmbito de uma partição. O nome distinguido da partição também é utilizado durante o fornecimento do metaverso ao espaço do conector para determinar a partição com que um objeto deve ser associado durante a exportação.

O script de descoberta de partição recebe os seguintes parâmetros do conector:

| Name | Tipo de Dados | Descrição |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][string, [ConfigParameter][cp]] |Tabela de parâmetros de configuração para o Conector. |
| Credencial |[PSCredential][pscred] |Contém quaisquer credenciais inseridas pelo administrador no separador Conectividade. |

O script deve devolver ao oleoduto um único objeto [de partição][part] ou uma Lista[T] de objetos de partição.

**Descoberta da Hierarquia**  
O script de descoberta da hierarquia só é usado quando a capacidade de Estilo de Nome Distinto é LDAP. O script é utilizado para permitir que navegue e selecione um conjunto de recipientes que são considerados dentro ou fora do alcance para operações de importação e exportação. O script deve apenas fornecer uma lista de nós que são crianças diretas do nó raiz fornecido ao script.

O script de descoberta da hierarquia recebe os seguintes parâmetros do conector:

| Name | Tipo de Dados | Descrição |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][string, [ConfigParameter][cp]] |Tabela de parâmetros de configuração para o Conector. |
| Credencial |[PSCredential][pscred] |Contém quaisquer credenciais inseridas pelo administrador no separador Conectividade. |
| ParentNode |[HierárquicaNode][hn] |O nó de raiz da hierarquia sob a qual o guião deve devolver as crianças diretas. |

O guião deve devolver ao oleoduto um único objeto Hierárquica de Uma Criança ou uma Lista[T] de objetos hierárquis de crianças HierárquicaNode.

#### <a name="import"></a>Importar
Os conectores que suportam operações de importação devem implementar três scripts.

**Começar a importar**  
O roteiro de importação inicial é executado no início de um passo de importação. Durante este passo, pode estabelecer uma ligação ao sistema de origem e fazer passos preparatórios antes de importar dados do sistema conectado.

O roteiro de importação iniciante recebe os seguintes parâmetros do conector:

| Name | Tipo de Dados | Descrição |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][string, [ConfigParameter][cp]] |Tabela de parâmetros de configuração para o Conector. |
| Credencial |[PSCredential][pscred] |Contém quaisquer credenciais inseridas pelo administrador no separador Conectividade. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Informa o script sobre o tipo de corrida de importação (delta ou cheio), partição, hierarquia, marca de água e tamanho de página esperado. |
| Tipos |[Esquema][schema] |Esquema para o espaço do conector que é importado. |

O script deve devolver um único objeto [OpenImportConnectionResults][oicres] ao oleoduto, por exemplo: `Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Importar Dados**  
O script de dados de importação é chamado pelo conector até que o script indique que não há mais dados para importar. O conector Windows PowerShell tem um tamanho de página de 9.999 objetos. Se o seu script devolver mais de 9.999 objetos para importação, deve suportar a chamada. O conector expõe uma propriedade de dados personalizada que pode usar para armazenar uma marca de água para que cada vez que o script de dados de importação seja chamado, o seu script recomeça a importar objetos onde este foi deixado.

O roteiro de dados de importação recebe os seguintes parâmetros do conector:

| Name | Tipo de Dados | Descrição |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][string, [ConfigParameter][cp]] |Tabela de parâmetros de configuração para o Conector. |
| Credencial |[PSCredential][pscred] |Contém quaisquer credenciais inseridas pelo administrador no separador Conectividade. |
| GetImportEntriesRunStep |[ImportRunStep][irs] |Detém a marca de água (CustomData) que pode ser usada durante as importações de página e delta. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Informa o script sobre o tipo de corrida de importação (delta ou cheio), partição, hierarquia, marca de água e tamanho de página esperado. |
| Tipos |[Esquema][schema] |Esquema para o espaço do conector que é importado. |

O roteiro de dados de importação deve escrever uma lista[[CSEntryChange]][csec]objeto para o pipeline. Esta coleção é composta por atributos CSEntryChange que representam cada objeto que está sendo importado. Durante uma corrida de Importação Completa, esta coleção deve ter um conjunto completo de objetos CSEntryChange que têm todos os atributos para cada objeto. Durante uma Importação Delta, o objeto CSEntryChange deve conter os deltas de nível de atributo para cada objeto a importar, ou uma representação completa dos objetos que mudaram (modo de substituição).

**Fim da Importação**  
No final da corrida de importação, o roteiro de Fim de Importação é executado. Este script deve executar quaisquer tarefas de limpeza necessárias (por exemplo, fechar ligações aos sistemas e responder a falhas).

O script de importação final recebe os seguintes parâmetros do conector:

| Name | Tipo de Dados | Descrição |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][string, [ConfigParameter][cp]] |Tabela de parâmetros de configuração para o Conector. |
| Credencial |[PSCredential][pscred] |Contém quaisquer credenciais inseridas pelo administrador no separador Conectividade. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Informa o script sobre o tipo de corrida de importação (delta ou cheio), partição, hierarquia, marca de água e tamanho de página esperado. |
| CloseImportConnectionRunStep |[CloseImportConnectionRunStep][cecrs] |Informa o guião sobre a razão pela qual a importação foi encerrada. |

O script deve devolver um único objeto [CloseImportConnectionResults][cicres] ao oleoduto, por exemplo: `Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Exportar
Idênticos à arquitetura de importação do conector, os conectores que suportam a Exportação devem implementar três scripts.

**Começar a exportar**  
O roteiro de exportação inicial é executado no início de um passo de exportação. Durante este passo, pode estabelecer uma ligação ao sistema de origem e realizar quaisquer passos preparatórios antes de exportar dados para o sistema conectado.

O roteiro de exportação iniciante recebe os seguintes parâmetros do conector:

| Name | Tipo de Dados | Descrição |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][string, [ConfigParameter][cp]] |Tabela de parâmetros de configuração para o Conector. |
| Credencial |[PSCredential][pscred] |Contém quaisquer credenciais inseridas pelo administrador no separador Conectividade. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Informa o script sobre o tipo de corrida de exportação (delta ou cheio), partição, hierarquia e tamanho de página esperado. |
| Tipos |[Esquema][schema] |Esquema para o espaço do conector que é exportado. |

O script não deve devolver nenhuma saída ao oleoduto.

**Dados de Exportação**  
O Serviço de Sincronização chama o script de Dados de Exportação quantas vezes for necessário para processar todas as exportações pendentes. Se o espaço do conector tiver mais exportações pendentes do que o tamanho da página do conector, o script de dados de exportação pode ser chamado várias vezes e possivelmente várias vezes para o mesmo objeto.

O roteiro de dados de exportação recebe os seguintes parâmetros do conector:

| Name | Tipo de Dados | Descrição |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][string, [ConfigParameter][cp]] |Tabela de parâmetros de configuração para o Conector. |
| Credencial |[PSCredential][pscred] |Contém quaisquer credenciais inseridas pelo administrador no separador Conectividade. |
| Entradas CS |IList[CSEntryChange][csec] |Lista de todos os objetos espaciais do conector com exportações pendentes para serem processados durante este passe. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Informa o script sobre o tipo de corrida de exportação (delta ou cheio), partição, hierarquia e tamanho de página esperado. |
| Tipos |[Esquema][schema] |Esquema para o espaço do conector que é exportado. |

O script de dados de exportação deve devolver um objeto [PutExportEntriesResults][peeres] ao pipeline. Este objeto não necessita de incluir informações de resultados para cada conector exportado, a menos que ocorra um erro ou uma alteração no atributo de âncora. Por exemplo, para devolver um objeto PutExportEntriesResults ao pipeline: `Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**Exportação final**  
No final da corrida à exportação, o guião "Fim de Exportação" será executado. Este script deve executar quaisquer tarefas de limpeza necessárias (por exemplo, fechar ligações aos sistemas e responder a falhas).

O script de exportação final recebe os seguintes parâmetros do conector:

| Name | Tipo de Dados | Descrição |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][string, [ConfigParameter][cp]] |Tabela de parâmetros de configuração para o Conector. |
| Credencial |[PSCredential][pscred] |Contém quaisquer credenciais inseridas pelo administrador no separador Conectividade. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Informa o script sobre o tipo de corrida de exportação (delta ou cheio), partição, hierarquia e tamanho de página esperado. |
| CloseExportConnectionRunStep |[CloseExportConnectionRunStep][cecrs] |Informa o guião sobre a razão pela qual a exportação foi encerrada. |

O script não deve devolver nenhuma saída ao oleoduto.

#### <a name="password-synchronization"></a>Sincronização de Palavras-passe
Os conectores Windows PowerShell podem ser usados como alvo para alterações/resets de palavra-passe.

O script da palavra-passe recebe os seguintes parâmetros do conector:

| Name | Tipo de Dados | Descrição |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][string, [ConfigParameter][cp]] |Tabela de parâmetros de configuração para o Conector. |
| Credencial |[PSCredential][pscred] |Contém quaisquer credenciais inseridas pelo administrador no separador Conectividade. |
| Partição |[Partição][part] |Partição do diretório em que o CSEntry está. |
| CSEntry |[CSEntry][cse] |Entrada de espaço do conector para o objeto que é recebido uma alteração de palavra-passe ou reset. |
| Tipo de Operação |String |Indica se a operação é um reset **(SetPassword)** ou uma alteração **(ChangePassword**). |
| Opções password |[Opções password][pwdopt] |Bandeiras que especificam o comportamento pretendido de reposição da palavra-passe. Este parâmetro só está disponível se o OperationType for **o SetPassword**. |
| Palavra-velho |String |Preenchido com a antiga senha do objeto para alterações de senha. Este parâmetro só está disponível se o OperationType for **ChangePassword**. |
| Palavra-passe nova |String |Preenchido com a nova senha do objeto que o script deve definir. |

Não se espera que o script da palavra-passe devolva quaisquer resultados ao pipeline Windows PowerShell. Se ocorrer um erro no script da palavra-passe, o script deve lançar uma das seguintes exceções para informar o Serviço de Sincronização sobre o problema:

* [PasswordPolicyViolationException][pwdex1] – Lançado se a palavra-passe não cumprir a política de palavra-passe no sistema conectado.
* [PasswordIllFormException][pwdex2] – Lançado se a palavra-passe não for aceitável para o sistema conectado.
* [PasswordExtension][pwdex3] – Lançado para todos os outros erros no script da palavra-passe.

## <a name="sample-connectors"></a>Conectores de amostra
Para obter uma visão geral completa dos conectores de amostra disponíveis, consulte a coleção do [conector do conector do conector Windows PowerShell][samp].

## <a name="other-notes"></a>Outras notas
### <a name="additional-configuration-for-impersonation"></a>Configuração adicional para personificação
Conceder ao utilizador que se personibra as seguintes permissões no servidor do Serviço de Sincronização:

Leia o acesso às seguintes chaves de registo:

* HKEY_USERS \\ [SynchronizationServiceAccountSID]\Software\Microsoft\PowerShell
* HKEY_USERS \\ [SynchronizationServiceAccountSID]\Ambiente

Para determinar o Identificador de Segurança (SID) da conta de serviço de sincronização, execute os seguintes comandos PowerShell:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Leia o acesso às seguintes pastas do sistema de ficheiros:

* %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Serviço de Sincronização\Extensões
* %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Serviço de Sincronização\ExtensõesCache
* %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Serviço de Sincronização\MaData \\ {ConnectorName}

Substitua o nome do conector Windows PowerShell para o espaço reservado {ConnectorName}.

## <a name="troubleshooting"></a>Resolução de problemas
* Para obter informações sobre como permitir a sessão de registo para resolver problemas no conector, consulte o [Rastreio ETW para Conectores](https://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: https://go.microsoft.com/fwlink/?LinkId=394291
