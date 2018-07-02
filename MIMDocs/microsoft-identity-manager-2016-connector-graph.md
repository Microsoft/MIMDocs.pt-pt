---
title: O agente de gestão do Microsoft Identity Manager para o Microsoft Graph | Documentos da Microsoft
author: fimguy
description: Microsoft Graph (pré-visualização) é o gerenciamento de ciclo de vida do AD contas de utilizador externo. Neste cenário, uma organização convidou convidados no respetivo diretório do Azure AD e pretende dar acesso desses convidados para aplicações baseadas em Kerberos ou autenticação integrada do Windows no local
keywords: ''
ms.author: davidste
manager: bhu
ms.date: 04/25/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: a66d424e8388005855ac8e64623f5a00f89682e9
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34479371"
---
<a name="the-microsoft-identity-manager-management-agent-for-microsoft-graph-public-preview"></a>O agente de gestão do Microsoft Identity Manager para o Microsoft Graph (pré-visualização pública)
=======================================================================================

<a name="summary"></a>Resumo 
=======

O [agente de gestão do Microsoft Identity Manager para o Microsoft Graph (pré-visualização)](http://go.microsoft.com/fwlink/?LinkId=717495) permite cenários de integração adicional para os clientes do Azure AD Premium.

[O Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) integra os diretórios no local com o Azure AD e garante que os utilizadores têm uma identidade comum e consistente de autenticação para os aplicativos do AD DS, o Office 365, o Azure e o SaaS integrados no Azure AD, ao sincronizar os utilizadores e grupos de diretórios no local para o Azure AD.   Este agente de gestão pode ser implementada para identidade especializada e operações de gestão de acesso além do utilizador e a sincronização de grupo para o Azure AD.  Este agente de gestão apresenta em MIM sincronização metaverso adicionais objetos obtidos a partir da [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1 e beta. 

<a name="scenarios-covered"></a>Cenários abrangidos
=================

<a name="b2b-account-lifecycle-management"></a>Gestão de ciclo de vida da conta de B2B
--------------------------------

O cenário inicial em pré-visualização para o agente de gestão do Microsoft Identity Manager para o Microsoft Graph (pré-visualização) é a gestão de ciclo de vida da conta de utilizador externo AD. Neste cenário, uma organização convidou convidados no respetivo diretório do Azure AD e pretende dar acesso desses convidados a autenticação integrada do Windows no local ou aplicativos baseados em Kerberos, por meio do [aplicação do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish)proxy ou de outros mecanismos de gateway. O proxy de aplicações do Azure AD requer que cada utilizador tem de ter sua própria conta do AD DS, para fins de identificação e a delegação

Cenários adicionais podem ser adicionados no futuro e [documentados aqui](./microsoft-identity-manager-2016-graph-b2b-scenario.md)

<a name="determining-your-deployment-topology"></a>Determinar a topologia de implementação
====================================


<a name="preparing-to-use-the-management-agentma-for-microsoft-graph"></a>A preparar para utilizar o Agent(MA) de gestão para o Microsoft Graph
=============================================================

<a name="authorizing-the-ma-to-manage-your-azure-ad-directory"></a>Autorizar o MA para gerir o diretório do Azure AD
----------------------------------------------------

1.  Agente de gestão de gráfico requer aplicação Web / aplicação API a ser criada no AzureAD.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Figura 1. Novo registo de aplicação

2.  Abra a aplicação criada e utilize o ID da aplicação, como o Id de cliente na página de conetividade o MA:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Figura 2. ID da aplicação

2.  Gerar novo segredo do cliente ao abrir todas as definições-\> chaves. Defina uma descrição da chave e selecione a duração needful. Guarde as alterações. Um valor secreto não estarão disponível após sair da página.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Figura 3. Novo segredo do cliente

3.  Adicionar "Microsoft Graph API" para o aplicativo abrindo "Permissões necessárias."

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Figura 4. Adicionar nova API

A permissão seguinte deve ser adicionada ao "Microsoft Graph API":

| Operação com o objeto | Permissão necessária                                                                  | Tipo de permissão |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Grupo de importação          | Group.Read.All ou Group.ReadWrite.All                                                | Aplicação     |
| Utilizador de importação           | User.Read.All ou User.ReadWrite.All ou Directory.Read.All ou Directory.ReadWrite.All | Aplicação     |

Foi possível encontrar mais detalhes sobre as permissões necessárias [aqui](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference)

1.  Criar conector com o ID da aplicação e o agente de gestão de cliente Secret.Each gerado deve ter seu próprio aplicativo no AzureAD para evitar a execução de importação em paralelo para a mesma aplicação. Conector de gráfico oferece suporte a seguinte lista de tipos de objeto:

-   Função do

    -   Importação completa/Delta

    -   Exportar (adicionar, atualizar e eliminar)

-   Grupo

    -   Importação completa/Delta

    -   Exportar (adicionar, atualizar e eliminar)


A lista de tipos de atributos que são suportadas:

-   Boolean

-   EDM

-   Edm.DateTimeOffset (cadeia de caracteres no espaço conector)

-   microsoft.graph.directoryObject (referência no espaço conector para qualquer um dos objetos suportados)

-   Microsoft.Graph.Contact

Atributos com múltiplos valores (coleção) também são suportados em qualquer um de um formulário de tipo de lista anteriormente.

Conector de gráfico utiliza o atributo "id" para âncora e DN para todos os objetos.

Mudar o nome não é suportado neste momento, porque Graph não permite a alteração de objeto para o atributo "id" para o objeto existente.

<a name="access-token-lifetime"></a>Duração do token de acesso
=====================

Um aplicativo gráfico exige um token de acesso para acessar o Graph. Um conector irá pedir um novo token de acesso para cada iteração import (importar iteração depende do tamanho da página). Por exemplo:

-   AzureAD contém 10000 objetos

-   Tamanho da página configurado no conector é 5000

Neste caso, haverá duas iterações durante a importação, cada um deles retornará 5 000 objetos a sincronizar. Portanto, um novo token de acesso será pedido duas vezes.

Tenha em atenção que, durante a exportação irá ser pedido um novo token de acesso para cada objeto que tem de ser adicionada/atualizados/eliminados.

<a name="installing-the-connector"></a>Instalar o conector
========================

Antes de utilizar o conector, certifique-se de que tem o seguinte no servidor de sincronização: estrutura Microsoft .NET 4.5.2 ou posterior do Microsoft Identity Manager o 2016 SP1 deve usar correção 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) ou posterior.

Conectores do Microsoft Identity Manager 2016 SP1, o conector de gráfico está disponível como uma transferência a partir da [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

<a name="connector-configuration"></a>Configuração do conector
=======================

Página de conetividade:

![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Figura 5. Página de conetividade

A página de conectividade (figura 1) contém a versão de API de gráfico que é utilizada e o nome do inquilino. O Id de cliente e o segredo de cliente representam a ID da aplicação e o valor de chave da aplicação WebAPI que tem de ser criada no AzureAD.

Página de parâmetros global:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Figura 6. Página de parâmetros global

Página de parâmetros globais contém as seguintes definições:

Formato de DateTime – o formato utilizado para qualquer atributo com o tipo de Edm.DateTimeOffset. Todas as datas são convertidas em cadeia utilizando nesse formato durante a importação. Formato de conjunto é aplicado a qualquer atributo, que salva data.

HTTP tempo limite (segundos) – tempo limite em segundos, que será utilizado durante cada chamada HTTP ao aplicativo de WebAPI.

Força alterar palavra-passe para o utilizador criado no próximo início de sessão – Esta opção é utilizada para o novo utilizador que será criado durante a exportação. Se a opção estiver ativada, em seguida, [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) propriedade será definida como true, caso contrário, será false.

<a name="troubleshooting"></a>Resolução de Problemas
===============

**Ativar registos**

Se houver algum problema no gráfico, em seguida, registos podem ser utilizados para localizar o problema. O conector de gráfico utiliza a mesma origem como em todos os conectores genéricos. Por isso, os rastreios podem ser ativados no [da mesma forma, como para conectores genéricos, consulte wiki](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Ou simplesmente adicionando o seguinte miiserver.exe.config (dentro da seção de system.diagnostics/sources):

\<nome da origem = switchValue de "ConnectorsLog" = "Verbose"\>

\<Serviços de escuta\>

>   \<Adicionar initializeData = "ConnectorsLog" type="System.Diagnostics.EventLogTraceListener, sistema, versão = Version=4.0.0.0, Culture = neutral, PublicKeyToken PublicKeyToken=b77a5c561934e089" name = "ConnectorsLogListener" traceOutputOptions = "LogicalOperationStack,   Pilha de chamadas de DateTime, Timestamp,"/\>

\<remover o nome = "Padrão" /\>

\</listeners\>

\</Source\>

Tenha em atenção: se 'Executar este agente de gestão num processo separado' ativado então dllhost.exe.config deve ser utilizado em vez de miiserver.exe.config.

**Erro de expirada de token de acesso**

Conector pode devolver a mensagem do erro 401 de HTTP não autorizado, "token de acesso expirou.":

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Figura 7. "O token de acesso tiver expirado." Error

A causa deste problema pode ser a configuração da duração do token de acesso do lado do Azure. Por predefinição, o token de acesso expira após 1 hora. Para aumentar o tempo de expiração, veja [este artigo](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes).

Exemplo de como esta utilizar [do Azure AD PowerShell módulo versão de pré-visualização pública](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

Novo AzureADPolicy-definição \@("{"TokenLifetimePolicy": {"Versão": 1, **"AccessTokenLifetime":" 5: 00:00 "**}}') - DisplayName"OrganizationDefaultPolicyScenario"- IsOrganizationDefault \$true - tipo de "TokenLifetimePolicy"

<a name="next-steps"></a>Passos Seguintes
----------
- [API do Graph (ótimo para resolução de problemas de chamada HTTP)]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Controlo de versões, suporte e quebra de alterar as políticas para o Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Transferir o agente de gestão do Microsoft Identity Manager para o Microsoft Graph (pré-visualização)](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-supported-guides"></a>Guias do cenário específico suportados
----------------------------------
[Implementação de MIM B2B ponto a ponto]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
