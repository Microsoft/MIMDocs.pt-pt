---
title: O conector do Microsoft Identity Manager para o Microsoft Graph | Documentos da Microsoft
author: fimguy
description: Conector do Microsoft Identity Manager para o Microsoft Graph permite gerenciamento de ciclo de vida do AD contas de utilizador externo. Neste cenário, uma organização convidou convidados no respetivo diretório do Azure AD e pretende dar acesso desses convidados para aplicações baseadas em Kerberos ou autenticação de Windows-Integrated no local
keywords: ''
ms.author: billmath
manager: mtillman
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 2e376bcc88518b911f93ce9cd4ab920eb428815b
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358657"
---
<a name="microsoft-identity-manager-connector-for-microsoft-graph"></a>Conector do Microsoft Identity Manager para o Microsoft Graph
=======================================================================================

<a name="summary"></a>Resumo 
=======

O [conector do Microsoft Identity Manager para o Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495) permite cenários de integração adicional para os clientes do Azure AD Premium.  Envolvidas em MIM sincronização metaverso adicionais objetos obtidos a partir da [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1 e beta.

<a name="scenarios-covered"></a>Cenários abrangidos
=================

<a name="b2b-account-lifecycle-management"></a>Gestão de ciclo de vida da conta de B2B
--------------------------------

O cenário inicial para o conector do Microsoft Identity Manager para o Microsoft Graph é como um conector para o ajudar a automatizar a gestão de ciclo de vida de conta de AD DS para utilizadores externos. Neste cenário, uma organização está a sincronizar o funcionários com o Azure AD do AD DS com o Azure AD Connect e também convidou convidados no respetivo diretório do Azure AD. Convidar um resultados de convidados num objeto de utilizador externo a ser esse diretório da organização do Azure AD, que não está no AD DS dessa organização. Em seguida, a organização deseja conceder esses acesso de convidados a autenticação integrada do Windows no local ou aplicativos baseados em Kerberos, através da [proxy de aplicações do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) ou de outros mecanismos de gateway. O proxy de aplicações do Azure AD requer que cada utilizador tem de ter sua própria conta do AD DS, para fins de identificação e delegação.  

Para saber como configurar a sincronização de MIM automaticamente criar e manter contas AD DS para convidados, depois de ler as instruções neste artigo, continue a ler o artigo [colaboração de empresa-empresa (B2B) do Azure AD com o MIM 2016 SP1 proxy de aplicações do Azure](~/microsoft-identity-manager-2016-graph-b2b-scenario.md).  Esse artigo ilustra as regras de sincronização necessárias para o conector.

<a name="other-identity-management-scenarios"></a>Outros cenários de gestão de identidade
---------------

O conector pode ser utilizado para outro gestão de identidade específica cenários que envolvem o criarem, ler, atualizarem e eliminar objetos de utilizador, grupo e contatos no Azure AD, para além de utilizador e grupo de sincronização para o Azure AD. Ao avaliar os cenários possíveis,. tenha em atenção: este conector, não podendo ser manipulado num cenário, o que teria resultado num fluxo de dados se sobrepuserem, reais ou potencial sincronização entram em conflito com uma implementação do Azure AD Connect.  [O Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) é a abordagem recomendada para integrar diretórios no local com o Azure AD, ao sincronizar os utilizadores e grupos de diretórios no local para o Azure AD.  O Azure AD Connect tem muitos outros recursos de sincronização e permite cenários como a repetição de escrita de palavra-passe e de dispositivo, que não são possíveis para objetos criados por MIM. Se a dados estão a ser movidos para o AD DS, por exemplo, certifique-se de que está excluído do Azure AD Connect, tentando correspondem esses objetos no diretório do Azure AD.  Nem este conector pode ser utilizado para efetuar alterações a objetos do Azure AD, que foram criados pelo Azure AD Connect.



<a name="preparing-to-use-the-connector-for-microsoft-graph"></a>A preparar para utilizar o conector para o Microsoft Graph
=============================================================

<a name="authorizing-the-connector-to-retrieve-or-manage-objects-in-your-azure-ad-directory"></a>Autorizar o conector para recuperar ou gerir objetos no diretório do Azure AD
----------------------------------------------------

1.  O conector requer uma aplicação Web / aplicação API a ser criada no Azure AD, para que ele pode ser autorizado com permissões adequadas para operar no Azure AD objetos através do Microsoft Graph.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Figura 1. Novo registo de aplicação

2.  No portal do Azure, abra a aplicação criada e guarde o ID da aplicação, como um ID de cliente para utilizar mais tarde na página de conetividade o MA:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Figura 2. ID da Aplicação

3.  Gerar novo segredo do cliente ao abrir todas as definições-\> chaves. Defina uma descrição da chave e selecione a duração needful. Guarde as alterações. Um valor secreto não estarão disponível após sair da página.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Figura 3. Novo segredo do cliente

4.  Adicionar "Microsoft Graph API" para o aplicativo abrindo "Permissões necessárias."

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Figura 4. Adicionar nova API

A permissão seguinte deve ser adicionada à aplicação para permitir que ele use o "Microsoft Graph API", dependendo do cenário:

| Operação com o objeto | Permissão necessária                                                                  | Tipo de permissão |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Grupo de importação          | `Group.Read.All` ou `Group.ReadWrite.All`                                                | Aplicação     |
| Utilizador de importação           | `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` ou `Directory.ReadWrite.All` | Aplicação     |

Foi possível encontrar mais detalhes sobre as permissões necessárias [aqui](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference).

5. O aplicativo de conceder as permissões necessárias.


<a name="installing-the-connector"></a>Instalar o conector
========================

6.  Antes de instalar o conector, certifique-se de que tem o seguinte no servidor de sincronização: 

 - Estrutura do Microsoft .NET 4.5.2 ou posterior
 - Microsoft Identity Manager 2016 SP1 e tem de utilizar correção 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) ou posterior.

7. O conector para o Microsoft Graph, além de outros conectores para o Microsoft Identity Manager 2016 SP1, está disponível como uma transferência a partir da [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

8.  Reinicie o serviço de sincronização de MIM.
 
<a name="connector-configuration"></a>Configuração do conector
=======================


9.  Na IU do Gestor do serviço de sincronização, selecione **conectores** e **criar**.
Selecione **Graph (Microsoft)** , criar um conector e atribua um nome descritivo.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)


10. O serviço de sincronização de MIM da interface do Usuário, especifique o ID da aplicação e gerado o segredo do cliente. Cada agente de gestão configurado na sincronização de MIM deve ter sua própria aplicação no Azure AD para evitar a execução de importação em paralelo para a mesma aplicação.


![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Figura 5. Página de conetividade

A página de conectividade (figura 5) contém a versão de API de gráfico que é utilizada e o nome do inquilino. O ID de cliente e o segredo de cliente representam a ID da aplicação e o valor de chave da aplicação WebAPI que tem de ser criada no Azure AD.

11. Faça as alterações necessárias na página de parâmetros globais:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Figura 6. Página de parâmetros global

Página de parâmetros globais contém as seguintes definições:

- Formato de DateTime – o formato utilizado para qualquer atributo com o tipo de Edm.DateTimeOffset. Todas as datas são convertidas em cadeia utilizando nesse formato durante a importação. Formato de conjunto é aplicado a qualquer atributo, que salva data.

 - HTTP tempo limite (segundos) – tempo limite em segundos, que será utilizado durante cada chamada HTTP ao aplicativo de WebAPI.

 - Força alterar palavra-passe para o utilizador criado no próximo início de sessão – Esta opção é utilizada para o novo utilizador que será criado durante a exportação. Se a opção estiver ativada, em seguida, [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) propriedade será definida como true, caso contrário, será false.

<a name="configuring-the-connector-schema-and-operations"></a>Configurar o esquema do conector e operações
=========================

12.   Configure o esquema.  O conector suporta a seguinte lista de tipos de objeto:

-   Função do

    -   Importação completa/Delta

    -   Exportar (adicionar, atualizar e eliminar)

-   Grupo

    -   Importação completa/Delta

    -   Exportar (adicionar, atualizar e eliminar)


A lista de tipos de atributos que são suportadas:

-   `Edm.Boolean`

-   `Edm.String`

-   `Edm.DateTimeOffset` (string no espaço conector)

-   `microsoft.graph.directoryObject` (referência no espaço conector para qualquer um dos objetos suportados)

-   `microsoft.graph.contact`

Atributos com múltiplos valores (coleção) também são suportados em qualquer um de um tipo a partir da lista acima.

O conector utiliza o '`id`"atributo de âncora e DN para todos os objetos.  Por conseguinte, a mudança de nome não é necessária, porque a Graph API não permite a um objeto alterar o atributo "id".


<a name="access-token-lifetime"></a>Duração do token de acesso
=====================

Um aplicativo gráfico exige um token de acesso para aceder à Graph API. Um conector irá pedir um novo token de acesso para cada iteração import (importar iteração depende do tamanho da página). Por exemplo:

-   O Azure AD contém 10000 objetos

-   Tamanho da página configurado no conector é 5000

Neste caso haja duas iterações durante a importação, cada um deles retornará 5000 objetos a sincronizar. Portanto, um novo token de acesso será pedido duas vezes.

Durante a exportação, um novo token de acesso será solicitado para cada objeto que tem de ser adicionada/atualizados/eliminados.

<a name="troubleshooting"></a>Resolução de Problemas
===============

**Ativar registos**

Se houver algum problema no gráfico, em seguida, registos podem ser utilizados para localizar o problema. Por isso, os rastreios podem ser ativados no [da mesma forma, como para os conectores genéricos](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Ou então simplesmente ao adicionar o seguinte ao `miiserver.exe.config` (dentro de `system.diagnostics/sources` secção):


\<nome da origem = switchValue de "ConnectorsLog" = "Verbose"\>

\<Serviços de escuta\>

>   \<Adicionar initializeData = "ConnectorsLog" type="System.Diagnostics.EventLogTraceListener, sistema, versão = Version=4.0.0.0, Culture = neutral, PublicKeyToken PublicKeyToken=b77a5c561934e089" name = "ConnectorsLogListener" traceOutputOptions = "LogicalOperationStack,   Pilha de chamadas de DateTime, Timestamp,"/\>

\<remover o nome = "Padrão" /\>

\</listeners\>

\</Source\>

>[!NOTE]
>Se 'Executar este agente de gestão num processo separado' estiver ativada, em seguida, `dllhost.exe.config` deve ser utilizado em vez de `miiserver.exe.config`.

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
- [API do Graph, problemas de chamar ótimo para resolução de problemas de HTTP]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Controlo de versões, suporte e quebra de alterar as políticas para o Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Transferir o conector do Microsoft Identity Manager para o Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-guides"></a>Guias específicos de cenário
----------------------------------
[Implementação de MIM B2B ponto a ponto]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
