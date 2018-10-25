---
title: Configurar o conector do Microsoft Identity Manager para o Microsoft Graph para B2B | Documentos da Microsoft
author: billmath
description: Conector do Microsoft Graph é o gerenciamento de ciclo de vida do AD contas de utilizador externo. Neste cenário, uma organização convidou convidados no respetivo diretório do Azure AD e pretende dar acesso desses convidados para aplicações baseadas em Kerberos ou autenticação de Windows-Integrated no local
keywords: ''
ms.author: billmath
manager: mtillman
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 139c58510117ad422529a4ff0facd23040023713
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358776"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy"></a>Colaboração de empresa-empresa (B2B) do Azure AD com o Microsoft Identity Manager(MIM) 2016 SP1 com o Proxy de aplicações do Azure
============================================================================================================================

O cenário inicial é o gerenciamento de ciclo de vida do AD contas de utilizador externo.   Neste cenário, uma organização convidou convidados no respetivo diretório do Azure AD e pretende dar acesso desses convidados para autenticação de Windows-Integrated no local ou aplicativos baseados em Kerberos, por meio do [proxy de aplicações do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) ou de outros mecanismos de gateway. O proxy de aplicações do Azure AD requer que cada utilizador tem de ter sua própria conta do AD DS, para fins de identificação e delegação.

## <a name="scenario-specific-guidance"></a>Orientações específicas de cenário

Algumas suposições feitas na configuração de B2B com o MIM e o Proxy de aplicações do Azure AD:

-   Já tiver implementado uma local do AD, e o Microsoft Identity Manager é instalada e básica configuração do agente de gerenciamento de diretório Active Directory do serviço MIM, o Portal de MIM, (AD MA) e o agente de gestão de FIM (FIM MA).
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

-   Já tiver seguido as instruções no artigo sobre como transferir e instalar o [conector de gráfico](microsoft-identity-manager-2016-connector-graph.md).

-   Tem o Azure AD Connect, configurado para sincronizar utilizadores e grupos para o Azure AD.

-   Tem o Azure AD Connect, configurado para sincronizar grupos do Office para controlar o aplicativo [voltar ao AD DS no local](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory)

-   Já tiver configurado a conectores de Proxy de aplicações e grupos de conexão, caso contrário, pode visitar [aqui](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) para instalar e configurar

-   Já publicou uma ou mais aplicações que dependem da autenticação integrada do Windows ou contas individuais do AD através do Proxy de aplicação do Azure AD

-   Que convidou ou convidar convidados de um ou mais, que resultaram num ou mais utilizadores a ser criados no Azure AD <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>



## <a name="b2b-end-to-end-deployment-example-scenario"></a>Cenário de exemplo de implementação de ponta a ponta de B2B

Este guia complementa o seguinte cenário:

Contoso Pharmaceuticals funciona com Trey Research Inc. como parte do respetivo R & D departamento. Os funcionários do Trey Research tem de aceder a pesquisa fornecido pela Contoso Pharmaceuticals de aplicativo de relatório.

-   Contoso Pharmaceuticals estão em seu próprio inquilino, para ter configurado um domínio personalizado.

-   Alguém convidou-se um utilizador externo para o inquilino da farmacêutica Contoso.
    Este utilizador já aceitou o convite e pode aceder a recursos que são partilhados.

-   Contoso Pharmaceuticals publicou uma aplicação através do Proxy de aplicações. Neste cenário, o aplicativo de exemplo é o Portal de MIM. Isso permitiria que um utilizador convidado a participar em processos MIM, por exemplo, em cenários de suporte técnico de ajuda ou para pedir acesso a grupos em MIM.


## <a name="configure-ad-and-azure-ad-connect-to-exclude-users-added-from-azure-ad"></a>Configurar o AD e o Azure AD Connect para excluir os utilizadores adicionados a partir do Azure AD

Por predefinição, o Azure AD Connect irá partem do princípio de que os usuários não-administradores no Active Directory tem de ser sincronizada com o Azure AD.  Se o Azure AD Connect encontrar existente utilizador no Azure AD que corresponde ao utilizador do AD no local, Azure AD Connect irá corresponder as duas contas e partem do princípio de que se trata de uma sincronização anterior do utilizador e tornar no local AD autoritativa.  No entanto, este comportamento predefinido não é adequado para o fluxo de B2B, onde a conta de utilizador tem origem no Azure AD. 

Por conseguinte, os utilizadores colocados em AD DS por MIM, a partir do Azure AD tem de ser armazenados de forma a que o Azure AD não irá tentar sincronizar os utilizadores para o Azure AD.
Uma forma de fazer isso é criar uma nova unidade organizacional no AD DS e configurar o Azure AD Connect, para excluir essa unidade organizacional.  

Podem encontrar mais informações em [do Azure AD Connect: configurar a filtragem](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering). 
 

## <a name="create-the-azure-ad-application"></a>Criar aplicação do Azure AD 


Nota: Antes de criar na sincronização de MIM, o agente de gestão para o conector de gráfico, certifique-se de que consulta o guia para implantar o [conector de gráfico](microsoft-identity-manager-2016-connector-graph.md)e criei um aplicativo com um cliente ID e segredo.
Certifique-se de que o aplicativo foi autorizado a, pelo menos, um destas permissões: `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` ou `Directory.ReadWrite.All`. 

## <a name="create-the-new-management-agent"></a>Criar o novo agente de gestão


Na IU do Gestor do serviço de sincronização, selecione **conectores** e **criar**.
Selecione **Graph (Microsoft)** e dê a ele um nome descritivo.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Conectividade

Na página de conectividade, tem de especificar a versão de API do Graph. É PAI de pronto de produção **V 1.0**, é de não produção **Beta**.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="global-parameters"></a>Parâmetros globais

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Configurar hierarquia de aprovisionamento

Esta página é utilizada para mapear o componente de DN, por exemplo, OU, para o tipo de objeto que deve ser aprovisionado, por exemplo organizationalUnit. Não é necessária para este cenário, portanto, deixe-o como a predefinição e clique em seguinte.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Configurar partições e hierarquias

Na página de partições e hierarquias, selecione todos os espaços de nomes com objetos que pretende importar e exportar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Selecione os tipos de objeto

Na página de tipos de objeto, selecione os tipos de objeto que pretende importar. Tem de selecionar, pelo menos, "User".

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Selecionar atributos

No ecrã selecionar atributos, selecione os atributos do Azure AD que serão necessários para gerir utilizadores B2B no AD. O atributo "ID" é necessário.  Os atributos `userPrincipalName` e `userType` será utilizado mais tarde nesta configuração.  Outros atributos são opcionais, incluindo

-   `displayName`

-   `mail`

-   `givenName`

-   `surname`

-   `userPrincipalName`

-   `userType`

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Configurar âncoras

No ecrã Configurar âncora, configurar o atributo de âncora é uma etapa necessária. Por predefinição, utilizam o atributo ID para mapeamento de utilizador.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Configurar filtro de Conetor

Na página Configurar filtro de Conetor, o MIM permite-lhe filtrar os objetos com base no filtro de atributo. Neste cenário para B2B, o objetivo é trazer apenas os utilizadores com o valor do `userType` atributo que é igual a `Guest`e não os utilizadores com o userType igual `member`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Configurar associação e projeção regras

Este guia assume que irá criar uma regra de sincronização.  Como configurar regras de associação e projeção são processadas pela regra de sincronização, não é necessário ter que identificar uma associação e projeção no conector propriamente dito. Mantenha o padrão e clique em ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Configurar fluxo de atributos

Este guia assume que irá criar uma regra de sincronização.  Projeção não é necessária para definir o fluxo de atributos na sincronização de MIM, como ele é manipulado pela regra de sincronização que é criada mais tarde. Mantenha o padrão e clique em ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Configurar desaprovisionamento

A definição para configurar desaprovisionamento permite-lhe configurar a sincronização de MIM para eliminar o objeto, se o objeto de metaverso for eliminado. Neste cenário, vamos torná-los disconnectors como o objetivo é deixá-los no Azure AD. Neste cenário, não são exportar qualquer coisa com o Azure AD e o conector está configurado apenas para importação.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Configurar extensões

Configurar extensões esta gestão de agente é uma opção, mas não é necessário porque estamos usando uma regra de sincronização. Se, decidimos usar uma regra avançada no fluxo de atributos anteriormente, em seguida, haveria uma opção para definir a extensão de regras.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="extending-the-metaverse-schema"></a>Expandir o esquema do metaverso


Antes de criar a regra de sincronização, é necessário criar um atributo chamado userPrincipalName vinculado ao objeto person usando o Designer de MV.

O cliente de sincronização, selecione o estruturador de Metaversos

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Em seguida, selecione o tipo de objeto Person

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Em seguida em ações, clique em adicionar atributo

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Em seguida, conclua os seguintes detalhes

Nome de atributo: **userPrincipalName**

Tipo de atributo: **cadeia de caracteres (indexáveis)**

Indexados = **verdadeiro**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

## <a name="creating-mim-service-synchronization-rules"></a>Criar regras de sincronização do serviço MIM

nos passos abaixo começamos o mapeamento de conta de convidado de B2B e o fluxo de atributos. Algumas suposições são feitas aqui: que já tiver o Active Directory MA configurada e o FIM MA configurado para trazer os usuários para o Portal e serviço MIM.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

Os passos seguintes irão exigir a adição de configuração mínima para o FIM MA e o AD MA.

Mais detalhes podem ser encontrados aqui para a configuração <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> -como faço para aprovisionar utilizadores para o AD DS

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Regra de sincronização: Importar utilizador convidado para MV para Metaverso do serviço de sincronização do Active Directory do Azure<br>

Navegue para o Portal de MIM, selecione as regras de sincronização e clique em novo.  Crie uma regra de sincronização de entrada para o fluxo de B2B via conector do gráfico.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png)

No passo de critérios de relação, certifique-se de que selecione "Criar recurso no FIM".
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Configure as seguintes regras de fluxo de atributos de entrada.  Certifique-se de que preencher os `accountName`, `userPrincipalName` e `uid` atributos à medida que serão utilizados posteriormente neste cenário:

| **Apenas fluxo inicial** | **Utilizar como teste de existência** | **Fluxo (atributo da FIM do ⇒ de valor de origem)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [displayName⇒displayName](javascript:void(0);)                        |
|                       |                           | [Esquerda (id, 20) ⇒accountName](javascript:void(0);)                        |
|                       |                           | [id⇒uid](javascript:void(0);)                                         |
|                       |                           | [userType⇒employeeType](javascript:void(0);)                          |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [surname⇒sn](javascript:void(0);)                                     |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
|                       |                           | [id⇒cn](javascript:void(0);)                                          |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [mobilePhone⇒mobilePhone](javascript:void(0);)                        |

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Regra de sincronização: Criar a conta de utilizador convidado ao Active Directory 

Esta regra de sincronização cria o utilizador no Active Directory.  Certifique-se o fluxo para `dn` tem de colocar o utilizador na unidade organizacional que foi excluída do Azure AD Connect.  Além disso, atualize o fluxo para `unicodePwd` para satisfazer a política de palavra-passe do AD - o utilizador não tem de saber a palavra-passe.  Tenha em atenção o valor de `262656` para `userAccountControl` codifica os sinalizadores `SMARTCARD_REQUIRED` e `NORMAL_ACCOUNT`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Regras de fluxo:

| **Apenas fluxo inicial** | **Utilizar como teste de existência** | **Fluxo (atributo de destino ⇒ de valor do FIM)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName](javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [sn⇒sn](javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
| **Y**                 |                           | ["CN ="+ uid +", UO = B2BGuest, DC = contoso, DC = com" ⇒dn](javascript:void(0);) |
| **Y**                 |                           | [RandomNum (0,999) + userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="optional-synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Regra de sincronização opcional: Importação B2B convidado objetos SID do utilizador para permitir o início de sessão para MIM 

Esta regra de sincronização de entrada recupera atributo de SID do utilizador do Active Directory para MIM, para que o usuário pode acessar o Portal de MIM.  O Portal de MIM requer que o utilizador tem os atributos `samAccountName`, `domain` e `objectSid` povoadas na base de dados do serviço MIM.

Configurar o sistema externo de origem como o `ADMA`, como o `objectSid` atributo será definido automaticamente pelo AD quando o MIM cria o utilizador.
 
Tenha em atenção que se configurar os utilizadores a ser criado no serviço de MIM, certifique-se de que não estão no âmbito de todos os conjuntos de que regras de política de gestão do funcionário SSPR se destina.  Terá de alterar as definições de conjunto para excluir os utilizadores que tenham sido criados pelo fluxo de B2B. 

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)


| **Apenas fluxo inicial** | **Utilizar como teste de existência** | **Fluxo (atributo da FIM do ⇒ de valor de origem)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ["CONTOSO" ⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |


## <a name="run-the-synchronization-rules"></a>Executar as regras de sincronização

Em seguida, nós convidar o utilizador e, em seguida, execute a gestão de regras de sincronização do agente na seguinte ordem:

-   Total de importação e sincronização no `MIMMA` agente de gestão.  Isto garante a que sincronização de MIM tem as regras de sincronização mais recente configuradas.

-   Total de importação e sincronização no `ADMA` agente de gestão.  Isto garante que o MIM e o Active Directory são consistentes.  Neste momento, não ainda haverá quaisquer exportações pendentes para convidados.

-   Importação completa e a sincronização no agente de gestão do gráfico de B2B.  Isso traz os utilizadores convidados para o metaverso.  Neste momento, uma ou mais contas será pendentes exportação para `ADMA`.  Se não houver nenhum exportações pendentes, em seguida, verifique que os utilizadores convidados foram importados para o espaço conector e que as regras foram configuradas para os mesmos obter contas do AD.

-   Exportação e importação Delta e sincronização no `ADMA` agente de gestão.  Se as exportações falhou, em seguida, verifique a configuração de regra e determinar se ocorreram quaisquer requisitos de esquema em falta. 

-   Exportação e importação Delta e sincronização no `MIMMA` agente de gestão.  Quando estiver concluído, deverá já não existir qualquer exportações pendentes.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)


## <a name="optional-application-proxy-for-b2b-guests-logging-into-mim-portal"></a>Opcional: Proxy da aplicação para B2B os convidados iniciar sessão no Portal de MIM

Agora que criamos as regras de sincronização em MIM. Definir a configuração de Proxy de aplicações, utilizar o principal de nuvem para permitir o KCD no proxy de aplicação.
Além disso, em seguida adicionou o usuário manualmente para os gerir utilizadores e grupos. As opções para não mostrar o utilizador, até que a criação ocorreu em MIM para adicionar o convidado para um grupo do office uma vez provisionado requer um pouco mais de configuração não abrangido neste documento.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)


Depois de tudo configurado, tem início de sessão de utilizador de B2B e ver a aplicação.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Passos Seguintes
----------

[Como Aprovisiono Utilizadores para o AD DS?](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[Referências de Funções para o FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[Como fornecer acesso remoto seguro a aplicações no local](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Transferir o conector do Microsoft Identity Manager para o Microsoft Graph](http://go.microsoft.com/fwlink/?LinkId=717495)
