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
ms.openlocfilehash: ac11a4dfb23944d50dbbcf0b0d70c915f186c159
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/25/2018
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy-public-preview"></a>Colaboração de empresa-empresa (B2B) do Azure AD com o Microsoft Identity Manager(MIM) 2016 SP1 com o Proxy de aplicações do Azure (pré-visualização pública)
============================================================================================================================

O cenário inicial na pré-visualização para é utilizador externo gestão de ciclo de vida de contas do AD.   Neste cenário, uma organização tem convidou convidados no seu diretório do Azure AD e pretende conceder o acesso desses convidados a autenticação integrada do Windows no local ou baseada em Kerberos aplicações, através de [aplicação do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish)proxy ou de outros mecanismos de gateway. O proxy de aplicações do Azure AD requer que cada utilizador ter a sua própria conta do AD DS, para fins de identificação e a delegação

## <a name="scenario-specific-supported-guidance"></a>Orientações suportados específicos de cenário

Neste cenário, uma organização tem convidou convidados no seu diretório do Azure AD e pretende conceder o acesso desses convidados para o Windows no local. Integrado autenticação ou aplicações baseadas no Kerberos, através de [aplicação do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) proxy ou de outros mecanismos de gateway. O proxy de aplicações do Azure AD requer que cada utilizador ter a sua própria conta do AD DS, para fins de identificação e a delegação

Pressupostos alguns efetuados na configuração do B2B com o MIM e o Proxy de aplicações do Azure

-   Já instalou a [agente de gestão do gráfico](microsoft-identity-manager-2016-connector-graph.md).

-   Tiver no local AD e o Azure AD Connect conjunto de cópias de segurança para sincronizar os utilizadores e grupos para o Azure AD.

    -   Aceder grupos do Office controlar aplicações ao utilizar [do Azure AD Connect](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory)

-   Já configurou conetores da Proxy da aplicação e os grupos de conector, caso contrário, pode visitar [aqui](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) para instalar e configurar

-   Um ou mais aplicações que dependem da autenticação integrada do Windows ou contas de anúncios individuais através do Proxy de aplicações do Azure AD publicadas

-   Ter convidou ou convidar um ou mais nos convidados, que são criados no Azure AD <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>

-   Microsoft Identity Manager é instalada e básica configuração do serviço e Portal e o agente de gestão do Active Directory.
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

## <a name="b2b-end-to-end-deployment"></a>Implementação de ponto a ponto B2B

Cenário

Farmacêutica contoso funciona Inc. do Trey Research como parte do respetivo R & D departamento. Os funcionários do Trey Research devem ter acesso a investigação relatórios de aplicações fornecida pelo farmacêutica Contoso.

-   Farmacêutica contoso são um inquilino independente, ter configurado um domínio personalizado.

-   Convidou um utilizador externo: para o farmacêutica Contoso inquilino.
    Este utilizador foi aceite o convite e pode aceder a recursos que são partilhados.

-   Publicar uma aplicação através do Proxy de aplicação e, neste cenário, como um exemplo para utilizar o Portal de utilizador convidado e serviço MIM para participar no processo de MIM, cenários de suporte técnico de exemplo.

## <a name="create-the-graph-management-agent"></a>Criar o agente de gestão do gráfico

Nota: Antes de criar o conector Certifique-se, consultar o [agente de gestão do gráfico](microsoft-identity-manager-2016-connector-graph.md).

Na IU do Gestor do serviço de sincronização, selecione **conectores** e **criar**.
Selecione **gráfico (Microsoft)** e atribua-lhe um nome descritivo

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>conectividade

Na página de conectividade, tem de especificar está pronto a produção de versão do API Graph **V 1.0**, não a produção é **Beta**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="capabilities"></a>Capacidades

Na página de parâmetros globais, configure o DN no registo de alterações de delta e funcionalidades adicionais de LDAP. A página é pré-preenchidos com as informações fornecidas pelo servidor LDAP.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="global-parameters"></a>Parâmetros globais

Na página de parâmetros globais, configure o DN no registo de alterações de delta e funcionalidades adicionais de LDAP. A página é pré-preenchidos com as informações fornecidas pelo servidor LDAP.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Configurar hierarquia de aprovisionamento

Esta página é utilizada para mapear o componente de DN, por exemplo UO, para o tipo de objeto que deve ser aprovisionado, por exemplo organizationalUnit. Deixe isto predefinido, clique em seguinte

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Configurar partições e hierarquias

Na página de partições e hierarquias, selecione todos os espaços de nomes com objetos que pretende importar e exportar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Selecione os tipos de objeto

Na página de partições e hierarquias, selecione todos os espaços de nomes com objetos que pretende importar e exportar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Selecionar atributos

No ecrã selecionar atributos, selecione os atributos necessários para gerir utilizadores B2B. O atributo "id" é necessário

-   ID

-   displayName

-   correio

-   givenName

-   Apelido

-   userPrincipalName

-   UserType

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Configurar âncoras

Com a âncora de configurar, configurar a âncora é um passo necessário. Por predefinição, utilize o atributo id para o mapeamento.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Configurar filtro de Conetor

Na configurar o filtro de Conetor página, permite-lhe filtrar objetos com base num filtro de atributo. Neste cenário para B2B, o objetivo é apenas colocar os utilizadores com o userType é igual a convidado e não membro.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Configurar associação e projeção regras

Configurar associação e projeção regras são processadas pela regra de sincronização, que não é necessária a necessitar de identificar uma associação e projeção no conector propriamente dito. Deixe a predefinição e clique em ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Configurar fluxo de atributos

Tal como a associação e projeção, não é necessária para definir o fluxo de atributos, dado que é o identificador pela regra de sincronização que é possível criar mais tarde. Deixe a predefinição e clique em ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Configurar desaprovisionamento

Configurar desaprovisionamento permitem-lhe eliminar o objeto, caso o objeto de metaverso é eliminado. Neste teste, iremos tornar disconnectors conforme o objetivo é deixe-os no Azure também iremos são não exportar qualquer coisa para o azure dado que está apenas a importação.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Configurar extensões

Configurar extensões esta gestão de agente é uma opção, mas não é necessária como estamos a utilizar uma regra de sincronização. Se decidimos uma regra de avançadas no fluxo de atributos anteriores, em seguida, isto seria uma opção de definir.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="creating-mim-service-synchronization-rules"></a>Criar regras de sincronização do serviço MIM

nos passos abaixo, iremos começar o mapeamento de conta de convidado de B2B e o fluxo de atributos. Alguns pressupostos que são efetuados aqui que já tiver o Active Directory gestão agente configurado e de FIM MA configurado para comunicar com o portal e serviço MIM.

Antes de criar a regra de sincronização, temos de criar um atributo chamado userPrincipalName associado ao objeto de pessoa utilizando o Designer de MV.

No cliente de sincronização, selecione Metaverse Designer

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Em seguida, selecione o tipo de objeto pessoa

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Em seguida em ações, clique em adicionar atributo

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Por fim, conclua os seguintes detalhes

O nome do atributo: **userPrincipalName**

Tipo de atributo: **cadeia (Indexable)**

Indexada = **verdadeiro**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

Os passos seguintes irão exigir a configuração mínima de configuração do agente de gestão do serviço FIM e o agente do Active Directory domínio dos serviços de gestão.

Obter mais detalhes podem ser encontrados aqui para a configuração <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> -como posso aprovisionar utilizadores para o AD DS

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Regra de sincronização: Importar utilizador convidado para MV para Metaverso de serviço de sincronização do Azure Active Directory<br>

Navegue para o serviço MIM e Portal e regras de Sycronization selecione e clique em novo.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png) Selecione "Criar recurso no FIM" ![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Regras do fluxo de:

| **Apenas fluxo inicial** | **Utilizado como o teste de existência** | **Fluxo (FIM valor ⇒ destino atributo)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [displayName⇒displayName] (javascript:void(0);)                        |
|                       |                           | [Esquerda (id, 20) ⇒accountName] (javascript:void(0);)                        |
|                       |                           | [id⇒uid] (javascript:void(0);)                                         |
|                       |                           | [userType⇒employeeType] (javascript:void(0);)                          |
|                       |                           | [givenName⇒givenName] (javascript:void(0);)                            |
|                       |                           | [surname⇒sn] (javascript:void(0);)                                     |
|                       |                           | [userPrincipalName⇒userPrincipalName] (javascript:void(0);)            |
|                       |                           | [id⇒cn] (javascript:void(0);)                                          |
|                       |                           | [mail⇒mail] (javascript:void(0);)                                      |
|                       |                           | [mobilePhone⇒mobilePhone] (javascript:void(0);)                        |

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Regra de sincronização: Criar a conta de utilizador convidado para o Active Directory 

Esta regra de sincronização cria o utilizador no active directory

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Regras do fluxo de:

| **Apenas fluxo inicial** | **Utilizado como o teste de existência** | **Fluxo (FIM valor ⇒ destino atributo)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName] (javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName] (javascript:void(0);)                            |
|                       |                           | [mail⇒mail] (javascript:void(0);)                                      |
|                       |                           | [sn⇒sn] (javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName] (javascript:void(0);)            |
| **Y**                 |                           | ["CN ="+ uid +", UO = B2BGuest, DC = scontoso, DC = com" ⇒dn] (javascript:void(0);) |
| **Y**                 |                           | [RandomNum (0,999) + userPrincipalName⇒unicodePwd] (javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl] (javascript:void(0);)                      |

### <a name="synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Regra de sincronização: Importação B2B convidado objetos SID utilizador para permitir início de sessão de MIM 

Esta regra de sincronização cria o utilizador no active directory

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)

Por último, iremos convidar o utilizador e volte a executar a gestão pela seguinte ordem:

-   Importação completa e a sincronização com o agente de gestão do MIMMA

-   Importação completa e a sincronização com o agente de gestão ADMA_SCONTOSO_B2B

-   Importação completa e a sincronização com o agente de gestão de gráfico de B2B

-   Exportação, a importação Delta e a sincronização no agente de gestão ADMA_SCONTOSO_B2B

-   Exportação, a importação Delta e a sincronização com o agente de gestão do MIMMA

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)

## <a name="finally-application-proxy-with-b2b-guest-and-logging-into-mim"></a>Por último: O Proxy de aplicações com o convidado B2B e iniciar sessão em MIM

Agora que as regras de sincronização foi criado em MIM. Definir a configuração de Proxy de aplicação, utilize o princípio de nuvem para permitir KCD no proxy de aplicações.
Além disso, em seguida adicionar o utilizador manualmente para os gerir utilizadores e grupos. As opções não para mostrar o utilizador até criação ocorreu em MIM para adicionar o convidado para um grupo de office aprovisionado uma vez requer configuração um pouco mais não abrangida neste documento.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)

| **Apenas fluxo inicial** | **Utilizado como o teste de existência** | **Fluxo (FIM valor ⇒ destino atributo)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName] (javascript:void(0);)                     |
|                       |                           | ["CONTOSO" ⇒domain] (javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid] (javascript:void(0);)                                      |

Depois de configurar a todos

Por último tem início de sessão de utilizador de B2B e ver a aplicação

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Passos Seguintes
----------

[Como Aprovisiono Utilizadores para o AD DS?](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[Referências de Funções para o FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[Como fornecer acesso remoto seguro a aplicações no local](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Transferir o agente de gestão do Microsoft Identity Manager para o Microsoft Graph (pré-visualização)](http://go.microsoft.com/fwlink/?LinkId=717495)
