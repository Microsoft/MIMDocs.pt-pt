---
title: O agente de gestão do Microsoft Identity Manager para o Microsoft Graph | Microsoft Docs
author: fimguy
description: Microsoft Graph (pré-visualização) é a gestão de ciclo de vida do AD contas de utilizador externo. Neste cenário, uma organização tem convidou convidados no seu diretório do Azure AD e pretende conceder o acesso desses convidados a autenticação integrada do Windows no local ou baseada em Kerberos aplicações
keywords: ''
ms.author: davidste
manager: bhu
ms.date: 04/25/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: a0e2e280c3678867efc2ae8afa46c04ed38a1e11
ms.sourcegitcommit: 637988684768c994398b5725eb142e16e4b03bb3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/26/2018
---
<a name="the-microsoft-identity-manager-management-agent-for-microsoft-graph-public-preview"></a>O agente de gestão do Microsoft Identity Manager para o Microsoft Graph (pré-visualização pública)
=======================================================================================

<a name="summary"></a>Resumo 
=======

O [agente de gestão do Microsoft Identity Manager para o Microsoft Graph (pré-visualização)](http://go.microsoft.com/fwlink/?LinkId=717495) permite cenários de integração adicionais para os clientes do Azure AD Premium.

[O Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) integra diretórios no local com o Azure AD e garante que os utilizadores têm uma identidade comum e a autenticação consistente entre as aplicações de AD DS, Office 365, Azure e do SaaS integradas com o Azure AD, mediante a sincronização de utilizadores e grupos de diretórios no local ao Azure AD.   Este agente de gestão pode ser implementado para identidade especializada e operações de gestão de acesso para além do utilizador e sincronização de grupo para o Azure AD.  Este agente de gestão analisa no objetos de MIM sincronização metaverso adicionais obtidos a partir de [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1 e beta. 

<a name="scenarios-covered"></a>Cenários abrangidos
=================

<a name="b2b-account-lifecycle-management"></a>Gestão de ciclo de vida de contas do B2B
--------------------------------

O cenário inicial na pré-visualização para o agente de gestão do Microsoft Identity Manager para o Microsoft Graph (pré-visualização) é a gestão de ciclo de vida de contas de utilizador externo AD. Neste cenário, uma organização tem convidou convidados no seu diretório do Azure AD e pretende conceder o acesso desses convidados a autenticação integrada do Windows no local ou baseada em Kerberos aplicações, através de [aplicação do Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-publish)proxy ou de outros mecanismos de gateway. O proxy de aplicações do Azure AD requer que cada utilizador ter a sua própria conta do AD DS, para fins de identificação e a delegação

Cenários adicionais podem ser adicionados no futuro e [documentados aqui](./microsoft-identity-manager-2016-graph-b2b-scenario.md)

<a name="determining-your-deployment-topology"></a>Determinar a topologia de implementação
====================================


<a name="preparing-to-use-the-management-agentma-for-microsoft-graph"></a>A preparar para utilizar o Agent(MA) de gestão para o Microsoft Graph
=============================================================

<a name="authorizing-the-ma-to-manage-your-azure-ad-directory"></a>Autorizar o MA para gerir o diretório do Azure AD
----------------------------------------------------

1.  Agente de gestão do gráfico requer a aplicação Web / aplicação de API a serem criadas AzureAD.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Imagem em 1. Novo registo de aplicação

2.  Abra a aplicação criada e utilize o ID da aplicação, como o Id de cliente na página de conetividade o MA:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Imagem de 2. ID da aplicação

2.  Gerar o segredo do cliente novo, abrindo a todas as definições-\> chaves. Definir algumas descrição de chave e selecione needful duração. Guarde as alterações. Um valor secreto não estarão disponível após ao sair da página.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Imagem em 3. Novo segredo do cliente

3.  Adicionar "Microsoft Graph API" para a aplicação, abrindo "Permissões necessárias."

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Imagem em 4. Adicionar nova API

A permissão seguinte deve ser adicionada aos "Microsoft Graph API":

| Operação com o objeto | Permissão necessária                                                                  | Tipo de permissão |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Grupo de importação          | Group.Read.All ou Group.ReadWrite.All                                                | Aplicação     |
| Importação de utilizador           | User.Read.All ou User.ReadWrite.All ou Directory.Read.All ou Directory.ReadWrite.All | Aplicação     |

Foi possível encontrar mais detalhes sobre as permissões necessárias [aqui](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference)

1.  Criar conector com o ID de aplicação e o agente de gestão de cliente Secret.Each gerado deve ter sua própria aplicação AzureAD para evitar a execução de importação em paralelo para a mesma aplicação. Gráfico conector suporta a seguinte lista de tipos de objeto:

-   Função do

    -   Importação completa/Delta

    -   Exportar (adicionar, atualizar, eliminar)

-   Grupo

    -   Importação completa/Delta

    -   Exportar (adicionar, atualizar, eliminar)


A lista de tipos de atributo são suportadas:

-   Boolean

-   EDM

-   Edm.DateTimeOffset (cadeia no espaço de conector)

-   microsoft.graph.directoryObject (referência no espaço de conector para qualquer um dos objetos suportados)

-   Microsoft.Graph.Contact

Atributos com múltiplos valores (coleção) também são suportados para qualquer um formato de tipo na lista anterior.

Gráfico conector utiliza um atributo 'id' para âncora e DN para todos os objetos.

Mudar o nome não é suportado neste momento, porque GraphAPI não permite a alteração de objeto para o atributo 'id' para o objeto existente.

<a name="access-token-lifetime"></a>Duração do token de acesso
=====================

Uma aplicação de gráfico requer um token de acesso para aceder a GraphAPI. Um conector irá pedir um novo token de acesso para cada iteração de importação (importar iteração depende do tamanho da página). Por exemplo:

-   AzureAD contém 10000 objetos

-   Tamanho de página configurado no conector é 5000

Neste caso, existirão duas iterações durante a importação, cada um deles irá devolver 5 000 objetos a sincronizar. Por isso, um novo token de acesso será pedido duas vezes.

Tenha em atenção que, durante a exportação irá ser pedido um novo token de acesso para cada objeto que tem de ser adicionada/atualizar/eliminar.

<a name="installing-the-connector"></a>Instalar o conector
========================

Antes de utilizar o conector, certifique-se de que tem o seguinte no servidor de sincronização: o Microsoft .NET 4.5.2 Framework ou posterior tem de SP1 de 2016 Microsoft Identity Manager utilizar correção 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) ou posterior.

Os conectores para o Microsoft Identity Manager 2016 SP1, o conector do gráfico está disponível como uma transferência a partir do [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

<a name="connector-configuration"></a>Configuração do conector
=======================

Página de conetividade:

![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Imagem em 5. Página de conetividade

A página de conectividade (imagem 1) contém a versão da Graph API que é utilizada e nome de inquilino. O Id de cliente e o segredo do cliente representam o ID da aplicação e o valor de chave da aplicação end WebAPI que tem de ser criada no AzureAD.

Página de parâmetros global:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Imagem em 6. Página de parâmetros global

Página de parâmetros globais contém as seguintes definições:

Formato de DateTime – formato que é utilizado para qualquer atributo com o tipo de Edm.DateTimeOffset. Todas as datas são convertidas em cadeia utilizando nesse formato durante a importação. Formato de conjunto é aplicado a qualquer atributo que guarda data.

HTTP tempo limite (segundos) – tempo limite em segundos, que será utilizado durante cada chamada HTTP para o end WebAPI aplicação.

Force alterar palavra-passe de utilizador criada no próximo início de sessão – Esta opção é utilizada para o novo utilizador que será criado durante a exportação. Se a opção estiver ativada, em seguida, [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) propriedade será definida para verdadeiro, caso contrário será FALSO.

<a name="troubleshooting"></a>Resolução de Problemas
===============

**Ativar registos**

Se existirem quaisquer problemas no gráfico, em seguida, os registos poderia ser utilizados para localizar o problema. O conector do gráfico utiliza a mesma origem como em todos os conectores genéricos. Por isso, foi possível ativar a rastreios no [da mesma forma, como para conectores genéricos consulte o artigo wiki](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Ou apenas adicionando o seguinte para miiserver.exe.config (no interior da secção de system.diagnostics/sources):

\<nome da origem = switchValue "ConnectorsLog" = "Verboso"\>

\<Serviços de escuta\>

>   \<Adicionar initializeData = "ConnectorsLog" type="System.Diagnostics.EventLogTraceListener, sistema, versão = 4.0.0.0, Culture = independente, PublicKeyToken = b77a5c561934e089" name = "ConnectorsLogListener" traceOutputOptions = "LogicalOperationStack,   Pilha de chamadas de DateTime, Timestamp,"/\>

\<Remova o nome = "Predefinido" /\>

\</listeners\>

\</Source\>

Tenha em atenção: se 'Executar este agente de gestão num processo separado' ativado, então dllhost.exe.config deve ser utilizada em vez de miiserver.exe.config.

**Erro no token acesso expirado**

Conector pode devolver o erro 401 não autorizado, de mensagens HTTP "token de acesso tiver expirado.":

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Imagem de 7. "O token de acesso tiver expirado." Error

A causa deste problema poderá ser a configuração da duração do token de acesso do lado do Azure. Por predefinição, o token de acesso expira após 1 hora. Para aumentar o tempo de expiração, consulte [neste artigo](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes).

Exemplo de como esta utilizar [versão de pré-visualização do Azure AD PowerShell módulo público](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

Novo AzureADPolicy-definição \@('{"TokenLifetimePolicy": {"Versão": 1, **"AccessTokenLifetime": "5: 00:00"**}}') - DisplayName "OrganizationDefaultPolicyScenario" - IsOrganizationDefault \$true - tipo "TokenLifetimePolicy"

<a name="next-steps"></a>Passos Seguintes
----------
- [Explorador do gráfico (ótimo para resolução de problemas de chamada HTTP)]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Controlo de versões, de suporte e pode alterar políticas para o Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Transferir o agente de gestão do Microsoft Identity Manager para o Microsoft Graph (pré-visualização)](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-supported-guides"></a>Guias do cenário específico suportados
----------------------------------
[Implementação de MIM B2B ponto a ponto]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
