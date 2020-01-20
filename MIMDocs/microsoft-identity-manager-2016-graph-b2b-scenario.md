---
title: Configurando o conector de Microsoft Identity Manager para Microsoft Graph para B2B | Microsoft Docs
author: billmath
description: O conector de Microsoft Graph é gerenciamento de ciclo de vida de conta do AD de usuário externo. Nesse cenário, uma organização convidou convidados para seu diretório do Azure AD e deseja conceder a esses convidados acesso à autenticação integrada do Windows ou a aplicativos baseados em Kerberos
keywords: ''
ms.author: billmath
manager: mtillman
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: ba70cd299f2ebec31555bb40b935a6b54779d198
ms.sourcegitcommit: 1ca298d61f6020623f1936f86346b47ec5105d44
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2020
ms.locfileid: "76256636"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy"></a>Colaboração entre empresas (B2B) do Azure AD com o Microsoft Identity Manager (MIM) 2016 SP1 com Aplicativo Azure proxy
============================================================================================================================

O cenário inicial é gerenciamento de ciclo de vida de conta do AD do usuário externo.   Nesse cenário, uma organização convidou convidados para seu diretório do Azure AD e deseja conceder a esses convidados acesso à autenticação integrada do Windows ou a aplicativos baseados em Kerberos, por meio do [proxy de aplicativo do Azure ad](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) ou de outros mecanismos de gateway. O proxy de aplicativo do Azure AD exige que cada usuário tenha sua própria conta de AD DS, para fins de identificação e delegação.

## <a name="scenario-specific-guidance"></a>Diretrizes específicas do cenário

Algumas suposições feitas na configuração de B2B com o MIM e o Azure Proxy de Aplicativo do AD:

-   Você já implantou um AD local e Microsoft Identity Manager está instalada e a configuração básica do serviço do MIM, portal do MIM, agente de gerenciamento do Active Directory (AD MA) e agente de gerenciamento do FIM (fim MA).
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

-   Você já seguiu as instruções no artigo sobre como baixar e instalar o [conector do Graph](microsoft-identity-manager-2016-connector-graph.md).

-   Você tem Azure AD Connect configurado para sincronizar usuários e grupos com o Azure AD.

-   Você já configurou conectores de proxy de aplicativo e grupos de conectores, se não puder visitar [aqui](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) para instalar e configurar

-   Você já publicou um ou mais aplicativos, que dependem da autenticação integrada do Windows ou de contas individuais do AD por meio do proxy de Aplicativo Azure AD

-   Você convidou ou convida um ou mais convidados, que resultaram em um ou mais usuários sendo criados no Azure AD <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>



## <a name="b2b-end-to-end-deployment-example-scenario"></a>Cenário de exemplo de implantação de ponta a ponta B2B

Este guia se baseia no seguinte cenário:

A Contoso Pharmaceuticals trabalha com a Trey Research Inc. como parte de seu departamento de R & D. Os funcionários da Trey Research precisam acessar o aplicativo de relatório de pesquisa fornecido pela Contoso Pharmaceuticals.

-   A Contoso Pharmaceuticals está em seu próprio locatário, para ter configurado um domínio personalizado.

-   Alguém convidou um usuário externo para o locatário Contoso Pharmaceuticals.
    Este usuário aceitou o convite e pode acessar recursos compartilhados.

-   A Contoso Pharmaceuticals publicou um aplicativo por meio do proxy de aplicativo. Nesse cenário, o aplicativo de exemplo é o portal do MIM. Isso permitiria que um usuário convidado participasse de processos do MIM, por exemplo, em cenários de suporte técnico ou para solicitar acesso a grupos no MIM.


## <a name="configure-ad-and-azure-ad-connect-to-exclude-users-added-from-azure-ad"></a>Configurar o AD e o Azure AD Connect para excluir usuários adicionados do Azure AD

Por padrão, Azure AD Connect presumirá que usuários não administradores no Active Directory precisam ser sincronizados no Azure AD.  Se Azure AD Connect encontrar um usuário existente no Azure AD que corresponda ao usuário do AD local, Azure AD Connect corresponderá às duas contas e assumirá que essa é uma sincronização anterior do usuário e tornará o AD local autoritativo.  No entanto, esse comportamento padrão não é adequado para o fluxo B2B, em que a conta de usuário é originada no Azure AD. 

Portanto, os usuários trazidos para AD DS pelo MIM do Azure AD precisam ser armazenados de uma maneira que o Azure AD não tentará sincronizar os usuários de volta ao Azure AD.
Uma maneira de fazer isso é criar uma nova unidade organizacional no AD DS e configurar Azure AD Connect para excluir essa unidade organizacional.  

Mais informações podem ser encontradas em [Azure ad Connect sincronização: configurar a filtragem](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering). 
 

## <a name="create-the-azure-ad-application"></a>Criar o aplicativo do Azure AD 


Observação: antes de criar no MIM, sincronize o agente de gerenciamento para o conector do Graph, verifique se você examinou o guia para implantar o [conector do Graph](microsoft-identity-manager-2016-connector-graph.md)e criou um aplicativo com uma ID do cliente e um segredo.
Certifique-se de que o aplicativo tenha sido autorizado para pelo menos uma destas permissões: `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` ou `Directory.ReadWrite.All`. 

## <a name="create-the-new-management-agent"></a>Criar o novo agente de gerenciamento


Na interface do usuário do Synchronization Service Manager, selecione **conectores** e **criar**.
Selecione **grafo (Microsoft)**  e dê a ele um nome descritivo.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Conectividade

Na página conectividade, você deve especificar a versão API do Graph. A PAI pronta para produção é a **V 1,0**, não produção é **beta**.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="global-parameters"></a>Parâmetros Globais

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Configurar a hierarquia de provisionamento

Essa página é usada para mapear o componente DN, por exemplo, OU, para o tipo de objeto que deve ser provisionado, por exemplo, organizationalUnit. Isso não é necessário para esse cenário, então deixe isso como o padrão e clique em Avançar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Configurar Partições e Hierarquias

Na página partições e hierarquias, selecione todos os namespaces com objetos que você planeja importar e exportar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Selecionar Tipos de Objeto

Na página tipos de objeto, selecione os tipos de objetos que você planeja importar. Você deve selecionar pelo menos ' usuário '.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Selecionar Atributos

Na tela Selecionar atributos, selecione os atributos do Azure AD que serão necessários para gerenciar usuários B2B no AD. O atributo "ID" é necessário.  Os atributos `userPrincipalName` e `userType` serão usados mais tarde nesta configuração.  Outros atributos são opcionais, incluindo

-   `displayName`

-   `mail`

-   `givenName`

-   `surname`

-   `userPrincipalName`

-   `userType`

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Configurar Âncoras

Na tela configurar âncora, a configuração do atributo Anchor é uma etapa necessária. Por padrão, use o atributo ID para mapeamento de usuário.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Configurar Filtro de Conector

Na página Configurar filtro do conector, o MIM permite que você filtre objetos com base no filtro de atributo. Nesse cenário para B2B, o objetivo é trazer apenas os usuários com o valor do atributo `userType` que é igual a `Guest`e não os usuários com o UserType que é igual a `member`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Configurar Regras de Associação e Projeção

Este guia pressupõe que você estará criando uma regra de sincronização.  Como a configuração de regras de junção e de projeção é tratada pela regra de sincronização, não é necessário ter que identificar uma junção e uma projeção no próprio conector. Mantenha o padrão e clique em OK.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Configurar Fluxo de Atributos

Este guia pressupõe que você estará criando uma regra de sincronização.  A projeção não é necessária para definir o fluxo de atributos na sincronização do MIM, pois ela é tratada pela regra de sincronização que é criada posteriormente. Mantenha o padrão e clique em OK.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Configurar o desprovisionamento

A configuração para configurar o desprovisionamento permite configurar a sincronização do MIM para excluir o objeto, se o objeto de metaverso for excluído. Nesse cenário, nós os tornamos desconectados, pois o objetivo é deixá-los no Azure AD. Nesse cenário, não estamos exportando nada para o Azure AD e o conector é configurado apenas para importação.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Configurar Extensões

A configuração de extensões nesse agente de gerenciamento é uma opção, mas não é necessária, pois estamos usando uma regra de sincronização. Se decidirmos usar uma regra avançada no fluxo de atributos anteriormente, haveria uma opção para definir a extensão de regras.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="extending-the-metaverse-schema"></a>Estendendo o esquema de metaverso


Antes de criar a regra de sincronização, precisamos criar um atributo chamado userPrincipalName vinculado ao objeto Person usando o designer do MV.

No cliente de sincronização, selecione metaverso designer

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Em seguida, selecione o tipo de objeto Person

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Em seguida, em ações, clique em Adicionar atributo

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Em seguida, preencha os seguintes detalhes

Nome do atributo: **userPrincipalName**

Tipo de atributo: **cadeia de caracteres (indexável)**

Indexado = **verdadeiro**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

## <a name="creating-mim-service-synchronization-rules"></a>Criando regras de sincronização de serviço do MIM

nas etapas abaixo, começamos o mapeamento da conta de convidado B2B e do fluxo de atributos. Algumas suposições são feitas aqui: que você já tem o Active Directory MA configurado e o FIM MA configurados para trazer os usuários ao portal e ao serviço do MIM.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

As próximas etapas exigirão a adição da configuração mínima ao FIM do MA e ao AD MA.

Mais detalhes podem ser encontrados aqui para o <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> de configuração-como provisionar usuários para AD DS

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Regra de sincronização: importar o usuário convidado para o MV para o metaverso do serviço de sincronização do Azure Active Directory<br>

Navegue até o portal do MIM, selecione regras de sincronização e clique em novo.  Crie uma regra de sincronização de entrada para o fluxo B2B por meio do conector do Graph.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png)

Na etapa critérios de relacionamento, certifique-se de selecionar "criar recurso no FIM".
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Configure as seguintes regras de fluxo de atributo de entrada.  Certifique-se de preencher os atributos `accountName`, `userPrincipalName` e `uid`, pois eles serão usados mais tarde neste cenário:

| **Somente fluxo inicial** | **Usar como teste de existência** | **Flow (atributo Source valor ⇒ FIM)**                          |
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

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Regra de sincronização: criar conta de usuário convidado para Active Directory 

Esta regra de sincronização cria o usuário no Active Directory.  Certifique-se de que o fluxo para `dn` deve posicionar o usuário na unidade organizacional que foi excluída do Azure AD Connect.  Além disso, atualize o fluxo para `unicodePwd` para atender à sua política de senha do AD-o usuário não precisará saber a senha.  Observe que o valor de `262656` para `userAccountControl` codifica os sinalizadores `SMARTCARD_REQUIRED` e `NORMAL_ACCOUNT`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Regras de fluxo:

| **Somente fluxo inicial** | **Usar como teste de existência** | **Flow (atributo de destino do valor FIM ⇒)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName](javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [sn⇒sn](javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
| **Y**                 |                           | ["CN ="+ uid +", UO = B2BGuest, DC = contoso, DC = com" ⇒dn](javascript:void(0);) |
| **Y**                 |                           | [RandomNum (0,999) + userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="optional-synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Regra de sincronização opcional: importar SID de objetos de usuário convidado B2B para permitir o logon no MIM 

Essa regra de sincronização de entrada leva o atributo SID do usuário de Active Directory de volta para o MIM, para que o usuário possa acessar o portal do MIM.  O portal do MIM requer que o usuário tenha os atributos `samAccountName`, `domain` e `objectSid` populados no banco de dados do serviço do MIM.

Configure o sistema externo de origem como o `ADMA`, pois o atributo `objectSid` será definido automaticamente pelo AD quando o MIM criar o usuário.
 
Observe que, se você configurar os usuários para serem criados no serviço do MIM, verifique se eles não estão no escopo de nenhum conjunto destinado a regras de política de gerenciamento de SSPR de funcionários.  Talvez seja necessário alterar suas definições de conjunto para excluir usuários que foram criados pelo fluxo B2B. 

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)


| **Somente fluxo inicial** | **Usar como teste de existência** | **Flow (atributo Source valor ⇒ FIM)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ["CONTOSO" ⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |


## <a name="run-the-synchronization-rules"></a>Executar as regras de sincronização

Em seguida, convidamos o usuário e, em seguida, executamos as regras de sincronização do agente de gerenciamento na seguinte ordem:

-   Importação e sincronização completas no agente de gerenciamento de `MIMMA`.  Isso garante que a sincronização do MIM tenha as regras de sincronização mais recentes configuradas.

-   Importação e sincronização completas no agente de gerenciamento de `ADMA`.  Isso garante que o MIM e o Active Directory sejam consistentes.  Neste ponto, ainda não haverá exportações pendentes para convidados.

-   Importação e sincronização completas no agente de gerenciamento de grafo B2B.  Isso traz os usuários convidados para o metaverso.  Neste ponto, uma ou mais contas terão exportação pendente para `ADMA`.  Se não houver exportações pendentes, verifique se os usuários convidados foram importados para o espaço do conector e se as regras foram configuradas para que elas recebam contas do AD.

-   Exportação, importação Delta e sincronização no agente de gerenciamento de `ADMA`.  Se as exportações falharem, verifique a configuração da regra e determine se houve algum requisito de esquema ausente. 

-   Exportação, importação Delta e sincronização no agente de gerenciamento de `MIMMA`.  Quando isso for concluído, não deverá haver mais nenhuma exportação pendente.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)


## <a name="optional-application-proxy-for-b2b-guests-logging-into-mim-portal"></a>Opcional: proxy de aplicativo para convidados B2B fazendo logon no portal do MIM

Agora que criamos as regras de sincronização no MIM. Na configuração de proxy de aplicativo, defina usar a entidade de segurança de nuvem para permitir o KCD no proxy de aplicativo.
Além disso, o próximo adicionou o usuário manualmente para gerenciar usuários e grupos. As opções para não mostrar o usuário até que a criação tenha ocorrido no MIM para adicionar o convidado a um grupo do Office uma vez provisionado requer um pouco mais de configuração não abordada neste documento.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)


Depois de tudo configurado, tenha logon de usuário B2B e veja o aplicativo.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Próximos Passos
----------

[Como Aprovisiono Utilizadores para o AD DS?](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[Referências de Funções para o FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[How to provide secure remote access to on-premises applications](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started) (Como fornecer acesso remoto seguro a aplicações no local)

[Baixar o conector de Microsoft Identity Manager para Microsoft Graph](https://go.microsoft.com/fwlink/?LinkId=717495)
