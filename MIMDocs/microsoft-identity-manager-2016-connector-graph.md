---
title: O conector Microsoft Identity Manager para Microsoft Graph Microsoft Docs
description: O conector do Microsoft Identity Manager para o Microsoft Graph permite a gestão do ciclo de vida da conta de utilizador externo. Neste cenário, uma organização convidou os hóspedes para o seu diretório AD Azure, e deseja dar a esses hóspedes acesso a aplicações de autenticação integrada no Windows ou kerberos
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: c426dff583ca51ca77bcb18fe024bf38698e3530
ms.sourcegitcommit: 3ff309115a0f3de114e3dff4eb3927dd7b01df4d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/15/2020
ms.locfileid: "90570774"
---
# <a name="microsoft-identity-manager-connector-for-microsoft-graph"></a>Conector do Gestor de Identidade da Microsoft para o Microsoft Graph


## <a name="summary"></a>Resumo 


O [conector Microsoft Identity Manager para o Microsoft Graph](https://go.microsoft.com/fwlink/?LinkId=717495) permite cenários de integração adicionais para clientes Azure AD Premium.  Surge na sincronização mim de objetos adicionais obtidos a partir do [Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) v1 e beta.

## <a name="scenarios-covered"></a>Cenários cobertos


### <a name="b2b-account-lifecycle-management"></a>Gestão do ciclo de vida da conta B2B


O cenário inicial para o conector Do Gestor de Identidade do Microsoft para o Microsoft Graph é como um conector para ajudar a automatizar a gestão do ciclo de vida da conta AD DS para utilizadores externos. Neste cenário, uma organização está a sincronizar os colaboradores para a Azure AD a partir da AD DS usando o Azure AD Connect, e também convidou os hóspedes para o seu diretório AD AZure. Convidar um hóspede resulta num objeto de utilizador externo que está no diretório AD Azure da organização, que não está no DS AD dessa organização. Em seguida, a organização pretende dar a esses hóspedes acesso a aplicações baseadas no Windows Integrated Authentication ou Kerberos, através do [proxy de aplicações AD Azure](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) ou outros mecanismos de gateway. O representante de aplicação Azure AD exige que cada utilizador tenha a sua própria conta AD DS, para fins de identificação e delegação.  

Para aprender a configurar a sincronização mim para criar e manter automaticamente as contas de DS AD para os hóspedes, depois de ler as instruções neste artigo, continuar a ler no artigo [Azure AD colaboração business-to-business (B2B) com MIM 2016 SP1 com Azure Application Proxy](~/microsoft-identity-manager-2016-graph-b2b-scenario.md).  Este artigo ilustra as regras de sincronização necessárias para o conector.

### <a name="other-identity-management-scenarios"></a>Outros cenários de gestão de identidade


O conector pode ser utilizado para outros cenários específicos de gestão de identidade envolvendo criar, ler, atualizar e eliminar objetos de utilizador, grupo e contacto em Azure AD, além da sincronização do utilizador e do grupo para a Azure AD. Ao avaliar cenários potenciais, tenha em mente: este conector não pode ser operado num cenário que resultaria numa sobreposição de fluxo de dados, conflito de sincronização real ou potencial com uma implementação Azure AD Connect.  [Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) é a abordagem recomendada para integrar diretórios no local com Azure AD, sincronizando utilizadores e grupos de diretórios no local para Azure AD.  O Azure AD Connect tem muitas mais funcionalidades de sincronização e permite cenários como a palavra-passe e a gravação do dispositivo, que não são possíveis para objetos criados pela MIM. Se os dados estiverem a ser trazidos para DS AD, por exemplo, certifique-se de que é excluído do Azure AD Connect tentando combinar esses objetos de volta com o diretório AD AZure.  Este conector também não pode ser utilizado para fazer alterações aos objetos AD Azure, que foram criados pelo Azure AD Connect.



## <a name="preparing-to-use-the-connector-for-microsoft-graph"></a>Preparando-se para usar o Conector para o Gráfico do Microsoft

### <a name="authorizing-the-connector-to-retrieve-or-manage-objects-in-your-azure-ad-directory"></a>Autorizar o conector a recuperar ou gerir objetos no seu diretório AD Azure


1.  O conector requer que seja criada uma aplicação Web /API em AD Azure, para que possa ser autorizada com permissões apropriadas para operar em objetos AD Azure através do Microsoft Graph.

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

Imagem 1. Novo registo de aplicação

2.  No portal Azure, abra a aplicação criada e guarde o ID da aplicação, como ID do Cliente para utilizar mais tarde na página de conectividade do MA:

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

Imagem 2. ID da Aplicação

3.  Gere um novo Segredo de Cliente abrindo todas as definições - \> Chaves. Desacorda alguma descrição da Chave e selecione a duração necessária. Guardar alterações. Um valor secreto não estará disponível depois de sair da página.

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

Imagem 3. Novo Segredo do Cliente

4.  Adicione "Microsoft Graph API" à aplicação abrindo "Permissões Necessárias".

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

Imagem 4. Adicionar nova API

A seguinte permissão deve ser adicionada à aplicação para permitir a sua utilização da "Microsoft Graph API", dependendo do cenário:

| Operação com objeto | Permissão necessária                                                                  | Tipo de permissão |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| Grupo de Importação          | `Group.Read.All` ou `Group.ReadWrite.All`                                                | Aplicação     |
| Utilizador de Importação           | `User.Read.All`, `User.ReadWrite.All` `Directory.Read.All` ou `Directory.ReadWrite.All` | Aplicação     |

Mais detalhes sobre as permissões necessárias podem ser encontrados [aqui.](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference)

5. Conceda ao pedido as permissões necessárias.


## <a name="installing-the-connector"></a>Instalação do conector


6.  Antes de instalar o Conector, certifique-se de que tem o seguinte no servidor de sincronização: 

 - Microsoft .NET 4.5.2 Framework ou mais tarde
 - Microsoft Identity Manager 2016 SP1, e deve usar hotfix 4.4.1642.0 [KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794) ou mais tarde.

7. O conector do Microsoft Graph, para além de outros conectores para o Microsoft Identity Manager 2016 SP1, está disponível como download do [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495).

8.  Reiniciar o Serviço de Sincronização MIM.
 
## <a name="connector-configuration"></a>Configuração do conector



9.  Na UI do Gestor de Serviço de Sincronização, selecione **Conectors** e **Crie**.
Selecione **Graph (Microsoft)**, crie um conector e dê-lhe um nome descritivo.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)


10. No serviço de sincronização MIM UI, especifique o ID de aplicação e gerou o Client Secret. Cada agente de gestão configurado na MIM Sync deve ter a sua própria aplicação no Azure AD para evitar a importação em paralelo para a mesma aplicação.


![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

Imagem 5. Página de conectividade

A página de conectividade (Imagem 5) contém a versão API do gráfico que é usada e o nome do inquilino. O ID do Cliente e o Cliente Secret representam o ID de aplicação e o valor chave da aplicação WebAPI que deve ser criada em Azure AD.

11. Faça as alterações necessárias na página De Parâmetros Globais:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

Imagem 6. Página de Parâmetros Globais

A página de parâmetros globais contém as seguintes definições:

- Formato DateTime – formato que é utilizado para qualquer atributo com o tipo Edm.DateTimeOffset. Todas as datas são convertidas em corda utilizando este formato durante a importação. O formato definido é aplicado para qualquer atributo, que guarda a data.

 - TEMPO DE TEMPO HTTP (segundos) – tempo limite em segundos que serão utilizados durante cada chamada HTTP para aplicação WebAPI.

 - Força alterar a palavra-passe para o utilizador criado no próximo sinal – esta opção é utilizada para um novo utilizador que será criado durante a exportação. Se a opção estiver ativada, então force a propriedade [DoChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) será definida como verdadeira, caso contrário será falsa.

## <a name="configuring-the-connector-schema-and-operations"></a>Configuração do esquema e das operações do conector


12.   Configure o esquema.  O conector suporta a seguinte lista de tipos de objetos:

-   Utilizador

    -   Importação Total/Delta

    -   Exportação (Adicionar, Atualizar, Excluir)

-   Group

    -   Importação Total/Delta

    -   Exportação (Adicionar, Atualizar, Excluir)


A lista de tipos de atributos suportados:

-   `Edm.Boolean`

-   `Edm.String`

-   `Edm.DateTimeOffset` (corda no espaço do conector)

-   `microsoft.graph.directoryObject` (referência no espaço do conector a qualquer um dos objetos suportados)

-   `microsoft.graph.contact`

Os atributos multivalorizados (Coleção) também são suportados para qualquer um dos tipos da lista acima.

O conector utiliza o `id` atributo ' para âncora e DN para todos os objetos.  Portanto, não é necessário renomear o nome, uma vez que a API do Gráfico não permite que um objeto altere o seu atributo 'id'.


## <a name="access-token-lifetime"></a>Vida útil simbólica de acesso


Uma aplicação Graph requer um token de acesso para aceder à API do gráfico. Um conector solicitará um novo símbolo de acesso para cada iteração de importação (a iteração de importação depende do tamanho da página). Por exemplo:

-   A AZure AD contém 10000 objetos

-   Tamanho da página configurado no conector é 5000

Neste caso, haverá duas iterações durante a importação, cada uma delas devolverá 5000 objetos ao Sync. Então, um novo sinal de acesso será solicitado duas vezes.

Durante a exportação, será solicitado um novo token de acesso para cada objeto que deve ser adicionado/atualizado/eliminado.

## <a name="troubleshooting"></a>Resolução de problemas


**Ativar registos**

Se houver algum problema no Gráfico, então os registos podem ser usados para localizar o problema. Assim, os vestígios podem ser ativados [da mesma forma que para os conectores genéricos.](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx) Ou apenas adicionando o seguinte à `miiserver.exe.config` `system.diagnostics/sources` (secção interna):

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
>Se 'Executar este agente de gestão num processo separado' estiver ativado, então `dllhost.exe.config` deve ser utilizado em vez de `miiserver.exe.config` .

**Erro expirado do token de acesso**

O conector pode devolver o erro HTTP 401 Não Autorizado, mensagem "O token de acesso expirou.":

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

Imagem 7. "O sinal de acesso expirou." Erro

A causa desta questão pode ser a configuração do tempo de vida do Azure. Por predefinição, o token de acesso expira após 1 hora. Para aumentar o tempo de validade, consulte [este artigo.](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes)

Exemplo disso usando o lançamento do [módulo público Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureADPreview)

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

New-AzureADPolicy -Definition \@ ('{"TokenLifetimePolicy":{"Versão":1, **"AccessTokenLifetime":"5:00:00"**}})-DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault \$ true -Type "TokenLifetimePolicy"

## <a name="next-steps"></a>Passos seguintes

- [Graph Explorer, ótimo para resolver problemas questões de chamada HTTP]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Políticas de versão, suporte e quebra de mudanças para o Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Baixe o conector do Microsoft Identity Manager para o Microsoft Graph](https://go.microsoft.com/fwlink/?LinkId=717495) 
 [Implantação mim B2B ponta ao fim]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
