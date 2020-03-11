---
title: O conector do Microsoft Identity Manager para o Microsoft Graph  Microsoft Docs
author: fimguy
description: O conector microsoft Identity Manager para o Microsoft Graph permite a gestão externa do ciclo de vida da conta aD do utilizador. Neste cenário, uma organização convidou os hóspedes para o seu diretório Azure AD, e deseja dar a esses hóspedes acesso a aplicações integradas no Windows ou as aplicações baseadas em Kerberos
keywords: ''
ms.author: billmath
manager: daveba
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 462b649ca02519e5af5c3b1243506a74efa7052a
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044264"
---
# <a name="microsoft-identity-manager-connector-for-microsoft-graph"></a>Conector do Microsoft Identity Manager para o Microsoft Graph


## <a name="summary"></a>Resumo 


O [conector Microsoft Identity Manager para o Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495) permite cenários de integração adicionais para os clientes Azure AD Premium.  Aparece no metésículo de sincronização MIM objetos adicionais obtidos a partir do [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1 e beta.

## <a name="scenarios-covered"></a>Cenários cobertos


### <a name="b2b-account-lifecycle-management"></a>Gestão do ciclo de vida da conta B2B


O cenário inicial para o conector do Microsoft Identity Manager para o Microsoft Graph é como um conector para ajudar a automatizar a gestão do ciclo de vida da conta AD DS para utilizadores externos. Neste cenário, uma organização está a sincronizar colaboradores da Azure AD da AD DS utilizando o Azure AD Connect, e também convidou convidados para o seu diretório Azure AD. Convidar um hóspede resulta num objeto de utilizador externo que está no diretório Azure AD daquela organização, que não está no DS AD daquela organização. Em seguida, a organização pretende dar a esses hóspedes acesso a aplicações integradas do Windows Integrated Autenticação ou Kerberos, através do proxy de [aplicação da AD Azure](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) ou de outros mecanismos de gateway. O representante da aplicação Azure AD exige que cada utilizador tenha a sua própria conta AD DS, para efeitos de identificação e delegação.  

Para aprender a configurar a sincronização mim para criar e manter automaticamente as contas aD DS para os hóspedes, depois de ler as instruções neste artigo, continuar a ler no artigo [Azure AD colaboração negócio-a-negócio (B2B) com mim 2016 SP1 com Procuração de Aplicação Azure.](~/microsoft-identity-manager-2016-graph-b2b-scenario.md)  Este artigo ilustra as regras de sincronização necessárias para o conector.

### <a name="other-identity-management-scenarios"></a>Outros cenários de gestão de identidade


O conector pode ser utilizado para outros cenários específicos de gestão de identidade que envolvam criar, ler, atualizar e eliminar objetos de utilizador, grupo e contacto em Azure AD, além da sincronização do utilizador e do grupo para a AD Azure. Ao avaliar cenários potenciais, tenha em mente: este conector não pode ser operado num cenário, o que resultaria numa sobreposição do fluxo de dados, conflito real ou potencial de sincronização com uma implementação do Azure AD Connect.  [O Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) é a abordagem recomendada para integrar diretórios no local com a Azure AD, sincronizando utilizadores e grupos de diretórios no local para a Azure AD.  O Azure AD Connect tem muito mais funcionalidades de sincronização e permite cenários como a reescrita de palavras-passe e dispositivos, que não são possíveis para objetos criados pela MIM. Se os dados estiverem a ser introduzidos em DS AD, por exemplo, certifique-se de que está excluído do Azure AD Connect tentando combinar esses objetos de volta ao diretório Azure AD.  Este conector também não pode ser utilizado para fazer alterações nos objetos AD Azure, que foram criados pela Azure AD Connect.



## <a name="preparing-to-use-the-connector-for-microsoft-graph"></a>Preparando-se para utilizar o Conector para o Microsoft Graph

### <a name="authorizing-the-connector-to-retrieve-or-manage-objects-in-your-azure-ad-directory"></a>Autorizar o conector a recuperar ou gerir objetos no seu diretório Azure AD


1.  O conector requer que seja criada uma aplicação Web /Aplicação API em AD Azure, para que possa ser autorizada com permissões adequadas para operar em objetos AD Azure através do Microsoft Graph.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Imagem 1. Novo registo de candidaturas

2.  No portal Azure, abra a aplicação criada e guarde o ID de aplicação, como ID do Cliente para usar mais tarde na página de conectividade do MA:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Imagem 2. ID da Aplicação

3.  Gere o novo Segredo do Cliente abrindo todas as definições -\> Keys. Defina alguma descrição da Chave e selecione duração necessária. Guarde as alterações. Um valor secreto não estará disponível depois de sair da página.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Imagem 3. Novo Segredo de Cliente

4.  Adicione "Microsoft Graph API" à aplicação abrindo "Permissões Necessárias".

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Imagem 4. Adicione nova API

A seguinte permissão deve ser adicionada à aplicação para permitir a utilização da "Microsoft Graph API", dependendo do cenário:

| Operação com objeto | Permissão necessária                                                                  | Tipo de permissão |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Grupo de Importação          | `Group.Read.All` ou `Group.ReadWrite.All`                                                | Aplicação     |
| Utilizador de Importação           | `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` ou `Directory.ReadWrite.All` | Aplicação     |

Mais detalhes sobre permissões necessárias podem ser encontrados [aqui.](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference)

5. Conceda ao pedido as permissões necessárias.


## <a name="installing-the-connector"></a>Instalação do conector


6.  Antes de instalar o Conector, certifique-se de que tem o seguinte no servidor de sincronização: 

 - Microsoft .NET 4.5.2 Quadro ou posterior
 - Microsoft Identity Manager 2016 SP1, e deve usar hotfix 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) ou posterior.

7. O conector do Microsoft Graph, além de outros conectores para o Microsoft Identity Manager 2016 SP1, está disponível como download do [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

8.  Reiniciar o Serviço de Sincronização MIM.
 
## <a name="connector-configuration"></a>Configuração do conector



9.  No UI gestor de serviços de sincronização, selecione **Conectores** e **Crie**.
Selecione **Graph (Microsoft)**  , crie um conector e dê-lhe um nome descritivo.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)


10. No serviço de sincronização MIM UI, especifique o ID de aplicação e gerado Client Secret. Cada agente de gestão configurado no MIM Sync deve ter a sua própria aplicação em Azure AD para evitar a importação em paralelo para a mesma aplicação.


![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Imagem 5. Página de conectividade

A página de conectividade (Imagem 5) contém a versão Graph API que é usada e nome de inquilino. O ID do cliente e o Segredo do Cliente representam o ID de aplicação e o valor chave da aplicação WebAPI que deve ser criada em Azure AD.

11. Faça as alterações necessárias na página Parâmetros Globais:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Imagem 6. Página de Parâmetros Globais

A página de parâmetros globais contém as seguintes definições:

- Formato DateTime – formato utilizado para qualquer atributo com o tipo Edm.DateTimeOffset. Todas as datas são convertidas em cadeia utilizando esse formato durante a importação. O formato definido é aplicado para qualquer atributo, que salva a data.

 - TEMPO DE TEMPO HTTP (segundos) – tempo insuque em segundos que será utilizado durante cada chamada http para aplicação WebAPI.

 - Palavra-passe de alteração de força para utilizador criado no próximo sinal – esta opção é utilizada para um novo utilizador que será criado durante a exportação. Se a opção estiver ativada, então [forceAPasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) a propriedade será definida como verdadeira, caso contrário será falsa.

## <a name="configuring-the-connector-schema-and-operations"></a>Configurar o esquema e as operações do conector


12.   Configure o esquema.  O conector suporta a seguinte lista de tipos de objetos:

-   Função do

    -   Importação Total/Delta

    -   Exportação (Adicionar, Atualizar, Excluir)

-   Grupo

    -   Importação Total/Delta

    -   Exportação (Adicionar, Atualizar, Excluir)


A lista dos tipos de atributos que são suportados:

-   `Edm.Boolean`

-   `Edm.String`

-   `Edm.DateTimeOffset` (corda no espaço do conector)

-   `microsoft.graph.directoryObject` (referência no espaço do conector a qualquer um dos objetos suportados)

-   `microsoft.graph.contact`

Os atributos multivalorizados (Coleção) também são suportados para qualquer tipo da lista acima.

O conector utiliza o atributo '`id`' para âncora e DN para todos os objetos.  Portanto, não é necessário mudar o nome, uma vez que a API do gráfico não permite que um objeto altere o seu atributo 'id'.


## <a name="access-token-lifetime"></a>Vida útil do token de acesso


Uma aplicação graph requer um sinal de acesso para aceder à API do gráfico. Um conector solicitará um novo sinal de acesso para cada iteração de importação (a iteração da importação depende do tamanho da página). Por exemplo:

-   Azure AD contém 10000 objetos

-   O tamanho da página configurado no conector é 5000

Neste caso haverá duas iterações durante a importação, cada uma delas devolverá 5000 objetos a Sync. Então, um novo sinal de acesso será solicitado duas vezes.

Durante a exportação será solicitado um novo sinal de acesso para cada objeto que deve ser adicionado/atualizado/eliminado.

## <a name="troubleshooting"></a>Resolução de Problemas


**Ativar registos**

Se houver algum problema no Graph, então os registos podem ser usados para localizar o problema. Assim, os vestígios podem ser ativados da mesma forma que para os [conectores genéricos](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx). Ou apenas adicionando o seguinte à `miiserver.exe.config` (dentro `system.diagnostics/sources` secção):

```
\<source name="ConnectorsLog" switchValue="Verbose"\>

\<listeners\>

\<add initializeData="ConnectorsLog"
type="System.Diagnostics.EventLogTraceListener, System, Version=4.0.0.0,
Culture=neutral, PublicKeyToken=b77a5c561934e089"
name="ConnectorsLogListener" traceOutputOptions="LogicalOperationStack,
DateTime, Timestamp, Call stack" /\>

\<remove name="Default" /\>

\</listeners\>

\</source\>
```
>[!NOTE]
>Se estiver ativado o "executar este agente de gestão num processo separado", `dllhost.exe.config` deve ser utilizado em vez de `miiserver.exe.config`.

**Erro expirado de acesso**

O conector pode devolver http error 401 Não autorizado, mensagem "Acesso de acesso expirou."

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Imagem 7. "O sinal de acesso expirou." Error

A causa deste problema pode ser a configuração do tempo de acesso do lado Azure. Por padrão, o sinal de acesso expira após 1 hora. Para aumentar o tempo de validade, consulte [este artigo](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes).

Exemplo disso usando lançamento público de [pré-visualização do módulo PowerShell Da AD Azure](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

New-AzureADPolicy -Definition \@('{"TokenLifetimePolicy":{"Versão":1, **"AccessTokenLifetime":"5:00:00"** }}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault \$true -Type "TokenLifetimePolicy"

## <a name="next-steps"></a>Próximos passos

- [Graph Explorer, ótimo para problemas de resolução de problemas HTTP questões de chamada]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Verdição, suporte e quebra de políticas de mudança para o Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Baixe o conector do Microsoft Identity Manager para o Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495)
MIM [B2B End to End Implementação]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
