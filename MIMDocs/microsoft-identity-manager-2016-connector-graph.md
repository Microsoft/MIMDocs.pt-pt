---
title: O conector de Microsoft Identity Manager para Microsoft Graph | Microsoft Docs
author: fimguy
description: O conector do Microsoft Identity Manager para Microsoft Graph permite o gerenciamento do ciclo de vida de conta do AD do usuário externo Nesse cenário, uma organização convidou convidados para seu diretório do Azure AD e deseja conceder a esses convidados acesso à autenticação integrada do Windows ou a aplicativos baseados em Kerberos
keywords: ''
ms.author: billmath
manager: mtillman
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 2e376bcc88518b911f93ce9cd4ab920eb428815b
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "64519441"
---
<a name="microsoft-identity-manager-connector-for-microsoft-graph"></a>Conector de Microsoft Identity Manager para Microsoft Graph
=======================================================================================

<a name="summary"></a>Resumo 
=======

O [conector de Microsoft Identity Manager para Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495) permite cenários de integração adicionais para clientes Azure ad Premium.  As superfícies de ti nos objetos adicionais do metaverso de sincronização do MIM obtidas da [API de Microsoft Graph](https://developer.microsoft.com/en-us/graph/) v1 e beta.

<a name="scenarios-covered"></a>Cenários cobertos
=================

<a name="b2b-account-lifecycle-management"></a>Gerenciamento do ciclo de vida da conta B2B
--------------------------------

O cenário inicial para o conector de Microsoft Identity Manager para Microsoft Graph é como um conector para ajudar a automatizar AD DS gerenciamento de ciclo de vida da conta para usuários externos. Nesse cenário, uma organização está sincronizando os funcionários com o Azure AD de AD DS usando Azure AD Connect e também convidou convidados para seu diretório do Azure AD. Convidar um convidado resulta em um objeto de usuário externo que está no diretório do Azure AD da organização, que não está no AD DS da organização. Em seguida, a organização deseja dar a esses convidados acesso à autenticação integrada do Windows local ou a aplicativos baseados em Kerberos, por meio do [proxy de aplicativo do Azure ad](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) ou de outros mecanismos de gateway. O proxy de aplicativo do Azure AD exige que cada usuário tenha sua própria conta de AD DS, para fins de identificação e delegação.  

Para saber como configurar a sincronização do MIM para criar e manter automaticamente contas AD DS para convidados, depois de ler as instruções neste artigo, continue lendo no artigo [colaboração B2B (Business-to-Business) do Azure AD com o MIM 2016 SP1 com aplicativo Azure proxy](~/microsoft-identity-manager-2016-graph-b2b-scenario.md).  Esse artigo ilustra as regras de sincronização necessárias para o conector.

<a name="other-identity-management-scenarios"></a>Outros cenários de gerenciamento de identidade
---------------

O conector pode ser usado para outros cenários de gerenciamento de identidade específicos que envolvem criar, ler, atualizar e excluir objetos de usuário, grupo e contato no Azure AD, além da sincronização de usuário e de grupo para o Azure AD. Ao avaliar possíveis cenários, tenha em mente: esse conector não pode ser operado em um cenário, o que resultaria em uma sobreposição de fluxo de dados, conflito de sincronização real ou potencial com uma implantação Azure AD Connect.  [Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) é a abordagem recomendada para integrar diretórios locais ao Azure AD, sincronizando usuários e grupos de diretórios locais para o Azure AD.  Azure AD Connect tem muito mais recursos de sincronização e permite cenários como senha e Write-back de dispositivo, que não são possíveis para objetos criados pelo MIM. Se os dados estiverem sendo trazidos para AD DS, por exemplo, certifique-se de que eles sejam excluídos do Azure AD Connect tentando corresponder esses objetos de volta ao diretório do AD do Azure.  Nem esse conector pode ser usado para fazer alterações nos objetos do Azure AD, que foram criados por Azure AD Connect.



<a name="preparing-to-use-the-connector-for-microsoft-graph"></a>Preparando para usar o conector para Microsoft Graph
=============================================================

<a name="authorizing-the-connector-to-retrieve-or-manage-objects-in-your-azure-ad-directory"></a>Autorizando o conector para recuperar ou gerenciar objetos no seu diretório do Azure AD
----------------------------------------------------

1.  O conector exige a criação de um aplicativo de API/aplicativo Web no Azure AD, para que ele possa ser autorizado com as permissões apropriadas para operar em objetos do Azure AD por meio de Microsoft Graph.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Figura 1. Novo registo de aplicação

2.  No portal do Azure, abra o aplicativo criado e salve a ID do aplicativo, como uma ID do cliente a ser usada posteriormente na página de conectividade do MA:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Figura 2. ID da Aplicação

3.  Gere novo segredo do cliente abrindo todas as configurações-\> chaves. Defina uma descrição de chave e selecione duração necessária. Salvar alterações. Um valor secreto não estará disponível depois de sair da página.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Figura 3. Novo segredo do cliente

4.  Adicione "API Microsoft Graph" ao aplicativo abrindo "permissões necessárias".

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Figura 4. Adicionar nova API

A permissão a seguir deve ser adicionada ao aplicativo para permitir que ele use a "API Microsoft Graph", dependendo do cenário:

| Operação com objeto | Permissão necessária                                                                  | Tipo de permissão |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Grupo de importação          | `Group.Read.All` ou `Group.ReadWrite.All`                                                | Aplicação     |
| Importar usuário           | `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` ou `Directory.ReadWrite.All` | Aplicação     |

Mais detalhes sobre as permissões necessárias podem ser encontrados [aqui](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference).

5. Conceda ao aplicativo as permissões necessárias.


<a name="installing-the-connector"></a>Instalando o conector
========================

6.  Antes de instalar o conector, verifique se você tem o seguinte no servidor de sincronização: 

 - Microsoft .NET estrutura 4.5.2 ou posterior
 - Microsoft Identity Manager 2016 SP1 e deve usar o hotfix 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) ou posterior.

7. O conector para Microsoft Graph, além de outros conectores para Microsoft Identity Manager 2016 SP1, está disponível como um download no [centro de download da Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

8.  Reinicie o serviço de sincronização do MIM.
 
<a name="connector-configuration"></a>Configuração do conector
=======================


9.  Na interface do usuário do Synchronization Service Manager, selecione **conectores** e **criar**.
Selecione **grafo (Microsoft)**  , crie um conector e dê a ele um nome descritivo.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)


10. Na interface do usuário do serviço de sincronização do MIM, especifique a ID do aplicativo e o segredo do cliente gerado. Cada agente de gerenciamento configurado na sincronização do MIM deve ter seu próprio aplicativo no Azure AD para evitar a execução de importação em paralelo para o mesmo aplicativo.


![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Figura 5. Página de conectividade

A página conectividade (Figura 5) contém a versão API do Graph que é usada e o nome do locatário. A ID do cliente e o segredo do cliente representam a ID do aplicativo e o valor da chave do aplicativo WebAPI que deve ser criado no Azure AD.

11. Faça as alterações necessárias na página parâmetros globais:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Figura 6. Página de parâmetros globais

A página parâmetros globais contém as seguintes configurações:

- Formato de data e hora – formato usado para qualquer atributo com o tipo EDM. DateTimeOffset. Todas as datas são convertidas em cadeia de caracteres usando esse formato durante a importação. O formato definido é aplicado a qualquer atributo, que salva a data.

 - Tempo limite de HTTP (segundos) – tempo limite em segundos que será usado durante cada chamada HTTP para o aplicativo WebAPI.

 - Forçar alteração de senha para o usuário criado no próximo sinal – essa opção é usada para um novo usuário que será criado durante a exportação. Se a opção estiver habilitada, a propriedade [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) será definida como true, caso contrário, será false.

<a name="configuring-the-connector-schema-and-operations"></a>Configurando o esquema e as operações do conector
=========================

12.   Configure o esquema.  O conector dá suporte à seguinte lista de tipos de objeto:

-   Função do

    -   Importação completa/Delta

    -   Exportar (adicionar, atualizar, excluir)

-   Grupo

    -   Importação completa/Delta

    -   Exportar (adicionar, atualizar, excluir)


A lista de tipos de atributo com suporte:

-   `Edm.Boolean`

-   `Edm.String`

-   `Edm.DateTimeOffset` (cadeia de caracteres no espaço do conector)

-   `microsoft.graph.directoryObject` (referência no espaço do conector para qualquer um dos objetos com suporte)

-   `microsoft.graph.contact`

Também há suporte para os atributos com valores (coleção) de um tipo da lista acima.

O conector usa o atributo '`id`' para Anchor e DN para todos os objetos.  Portanto, Rename não é necessário, porque API do Graph não permite que um objeto altere seu atributo ' ID '.


<a name="access-token-lifetime"></a>Tempo de vida do token de acesso
=====================

Um aplicativo de grafo requer um token de acesso para acessar o API do Graph. Um conector solicitará um novo token de acesso para cada iteração de importação (a iteração de importação depende do tamanho da página). Por exemplo:

-   O Azure AD contém 10000 objetos

-   O tamanho da página configurado no conector é 5000

Nesse caso, haverá duas iterações durante a importação, cada uma delas retornará 5000 objetos para sincronização. Portanto, um novo token de acesso será solicitado duas vezes.

Durante a exportação, um novo token de acesso será solicitado para cada objeto que deve ser adicionado/atualizado/excluído.

<a name="troubleshooting"></a>Resolução de Problemas
===============

**Ativar registos**

Se houver algum problema no grafo, os logs poderão ser usados para localizar o problema. Portanto, os rastreamentos podem ser habilitados da [mesma forma como para conectores genéricos](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Ou apenas adicionando o seguinte a `miiserver.exe.config` (dentro da seção `system.diagnostics/sources`):


nome de origem do \<= "ConnectorsLog" switchValue = "Verbose"\>

\<ouvintes\>

>   \<Add initializeData = "ConnectorsLog" tipo = "System. Diagnostics. EventLogTraceListener, sistema, versão = 4.0.0.0, Culture = neutral, PublicKeyToken = b77a5c561934e089" Name = "ConnectorsLogListener" traceOutputOptions = "LogicalOperationStack, DateTime, carimbo de data/hora, pilha de chamadas"/\>

\<remover nome = "padrão"/\>

\</Listeners\>

\</Source\>

>[!NOTE]
>Se ' executar este agente de gerenciamento em um processo separado ' estiver habilitado, `dllhost.exe.config` deverá ser usado em vez de `miiserver.exe.config`.

**Erro de token de acesso expirado**

O conector pode retornar o erro HTTP 401 não autorizado, mensagem "o token de acesso expirou.":

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Figura 7. "O token de acesso expirou." Error

A causa desse problema pode ser a configuração do tempo de vida do token de acesso do lado do Azure. Por padrão, o token de acesso expira após 1 hora. Para aumentar o tempo de expiração, consulte [Este artigo](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes).

Exemplo disso usando a [versão de visualização pública do módulo do PowerShell do Azure ad](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

\@New-AzureADPolicy-Definition (' {"TokenLifetimePolicy": {"Version": 1, **"AccessTokenLifetime": "5:00:00"** }} ')-DisplayName "OrganizationDefaultPolicyScenario"-IsOrganizationDefault \$tipo true "TokenLifetimePolicy"

<a name="next-steps"></a>Próximos passos
----------
- [Gerenciador de gráficos, ótimo para solucionar problemas de chamada HTTP]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Controle de versão, suporte e políticas de alteração significativas para Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Baixar o conector de Microsoft Identity Manager para Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-guides"></a>Guias específicos do cenário
----------------------------------
[Implantação de ponta a ponta do MIM B2B]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
