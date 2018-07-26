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
ms.openlocfilehash: ac11a4dfb23944d50dbbcf0b0d70c915f186c159
ms.sourcegitcommit: c773edc8262b38df50d82dae0f026bb49500d0a4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34479167"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy-public-preview"></a>Colaboração de empresa-empresa (B2B) do Azure AD com o Microsoft Identity Manager(MIM) 2016 SP1 com o Proxy de aplicações do Azure (pré-visualização pública)
============================================================================================================================

O cenário inicial em pré-visualização para é o gerenciamento de ciclo de vida do AD contas de utilizador externo.   Neste cenário, uma organização convidou convidados no respetivo diretório do Azure AD e pretende dar acesso desses convidados a autenticação integrada do Windows no local ou aplicativos baseados em Kerberos, por meio do [aplicação do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish)proxy ou de outros mecanismos de gateway. O proxy de aplicações do Azure AD requer que cada utilizador tem de ter sua própria conta do AD DS, para fins de identificação e a delegação

## <a name="scenario-specific-supported-guidance"></a>Cenário documentação de orientação específica suportados

Neste cenário, uma organização convidou convidados no respetivo diretório do Azure AD e pretende dar acesso desses convidados para o Windows no local. Integrados de autenticação ou aplicativos baseados em Kerberos, por meio da [aplicação do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) proxy ou de outros mecanismos de gateway. O proxy de aplicações do Azure AD requer que cada utilizador tem de ter sua própria conta do AD DS, para fins de identificação e a delegação

Algumas suposições feitas na configuração de B2B com o MIM e o Proxy de aplicações do Azure

-   Já tiver instalado o [agente de gestão de gráfico](microsoft-identity-manager-2016-connector-graph.md).

-   Tem uma local do AD e o Azure AD Connect conjunto de cópia de segurança para a sincronização de utilizadores e grupos para o Azure AD.

    -   Grupos do Office controlar aplicação aceder com [do Azure AD Connect](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory)

-   Já tiver configurado a conectores de Proxy de aplicações e grupos de conexão, caso contrário, pode visitar [aqui](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) para instalar e configurar

-   Publicado um ou mais aplicações que dependem da autenticação integrada do Windows ou contas individuais do AD através do Proxy de aplicação do Azure AD

-   Que convidou ou convidar convidados de um ou mais, que são criados no Azure AD <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>

-   Microsoft Identity Manager é instalada e básica configuração do serviço e o Portal e o agente de gestão do Active Directory.
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

## <a name="b2b-end-to-end-deployment"></a>Implantação de ponta a ponta de B2B

Cenário

Contoso Pharmaceuticals funciona Trey Research Inc. como parte do respetivo R & D departamento. Os funcionários do Trey Research tem de aceder a pesquisa fornecido pela Contoso Pharmaceuticals de aplicativo de relatório.

-   Contoso Pharmaceuticals estão num inquilino independente, para ter configurado um domínio personalizado.

-   Convidados de um utilizador externo: para a Contoso Pharmaceuticals inquilino.
    Este utilizador já aceitou o convite e pode aceder a recursos que são partilhados.

-   Publicado de uma aplicação através do Proxy de aplicações e neste cenário, como um exemplo para utilizar o Portal de utilizador convidado e serviço MIM participem do processo de MIM, cenários de suporte técnico de exemplo.

## <a name="create-the-graph-management-agent"></a>Criar o agente de gestão do gráfico

Nota: Antes de criar o conector Certifique-se, rever o [agente de gestão de gráfico](microsoft-identity-manager-2016-connector-graph.md).

Na IU do Gestor do serviço de sincronização, selecione **conectores** e **criar**.
Selecione **Graph (Microsoft)** e dê a ele um nome descritivo

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Conectividade

Na página de conectividade, tem de especificar a é a produção de versão da API de gráfico pronto **V 1.0**, é de não produção **Beta**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="capabilities"></a>Capacidades

Na página de parâmetros globais, configure o DN para o registo de alterações de delta e recursos adicionais do LDAP. A página é previamente preenchida com as informações fornecidas pelo servidor LDAP.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="global-parameters"></a>Parâmetros globais

Na página de parâmetros globais, configure o DN para o registo de alterações de delta e recursos adicionais do LDAP. A página é previamente preenchida com as informações fornecidas pelo servidor LDAP.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Configurar hierarquia de aprovisionamento

Esta página é utilizada para mapear o componente de DN, por exemplo, OU, para o tipo de objeto que deve ser aprovisionado, por exemplo organizationalUnit. em seguida a deixar esse clique de predefinição

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Configurar partições e hierarquias

Na página de partições e hierarquias, selecione todos os espaços de nomes com objetos que pretende importar e exportar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Selecione os tipos de objeto

Na página de partições e hierarquias, selecione todos os espaços de nomes com objetos que pretende importar e exportar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Selecionar atributos

No ecrã selecionar atributos, selecione os atributos necessários para gerir utilizadores B2B. O atributo "id" é obrigatório

-   ID

-   displayName

-   correio

-   givenName

-   Apelido

-   userPrincipalName

-   userType

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Configurar âncoras

Com a âncora de configurar, configurar a âncora é uma etapa necessária. Por predefinição, utilizam o atributo id para o mapeamento.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Configurar filtro de Conetor

Na Configurar página de filtro de Conetor, pode filtrar os objetos com base no filtro de atributo. Neste cenário para B2B, o objetivo é apenas trazer os usuários com o userType igual a convidado e não é membro.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Configurar associação e projeção regras

Configurar associação e projeção de regras são processadas pela regra de sincronização, ela não é necessária a necessitar de identificar uma associação e projeção no conector propriamente dito. Mantenha o padrão e clique em ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Configurar fluxo de atributos

Tal como a associação e projeção, não é necessária para definir o fluxo de atributos, como ele é o identificador da regra de sincronização que é possível criar mais tarde. Mantenha o padrão e clique em ok.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Configurar desaprovisionamento

Configurar desaprovisionamento permitem-lhe eliminar o objeto se o objeto de metaverso é eliminado. Neste teste, podemos torná-los disconnectors como o objetivo é deixá-los no Azure também podemos são não exportar qualquer coisa para o azure, pois é apenas a importação.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Configurar extensões

Configurar extensões esta gestão de agente é uma opção, mas não necessário uma vez que estamos a utilizar uma regra de sincronização. Se, decidimos uma regra de avanço em versões anteriores, o fluxo de atributos, em seguida, isso seria uma opção para definir.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="creating-mim-service-synchronization-rules"></a>Criar regras de sincronização do serviço MIM

nos passos abaixo começamos o mapeamento de conta de convidado de B2B e o fluxo de atributos. Algumas suposições são feitas aqui que já tiver o Active Directory gestão agente configurado e o FIM MA configurado para comunicar com o portal e serviço MIM.

Antes de criar a regra de sincronização, é necessário criar um atributo chamado userPrincipalName vinculado ao objeto person usando o Designer de MV.

O cliente de sincronização, selecione o estruturador de Metaversos

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Em seguida, selecione o tipo de objeto Person

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Em seguida em ações, clique em adicionar atributo

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Por fim, conclua os seguintes detalhes

Nome de atributo: **userPrincipalName**

Tipo de atributo: **cadeia de caracteres (indexáveis)**

Indexados = **verdadeiro**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

Os passos seguintes exigirá a configuração mínima de configuração do agente de gestão do serviço FIM e o Active Directory domínio agente dos serviços de gestão.

Mais detalhes podem ser encontrados aqui para a configuração <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> -como faço para aprovisionar utilizadores para o AD DS

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Regra de sincronização: Importar utilizador convidado para MV para Metaverso do serviço de sincronização do Active Directory do Azure<br>

Navegue para o serviço MIM Portal e selecione Sycronization, regras e clique em novo.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png) Selecione "Criar recurso no FIM" ![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Regras de fluxo:

| **Apenas fluxo inicial** | **Utilizar como teste de existência** | **Fluxo (atributo de destino ⇒ de valor do FIM)**                          |
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

Esta regra de sincronização cria o utilizador no active directory

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
| **Y**                 |                           | ["CN ="+ uid +", UO = B2BGuest, DC = scontoso, DC = com" ⇒dn](javascript:void(0);) |
| **Y**                 |                           | [RandomNum (0,999) + userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Regra de sincronização: Importação B2B convidado objetos SID do utilizador para permitir o início de sessão para MIM 

Esta regra de sincronização cria o utilizador no active directory

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)

Por último, vamos convidar o utilizador e, em seguida, execute o gerenciamento pela seguinte ordem:

-   Importação completa e a sincronização com o agente de gestão do MIMMA

-   Importação completa e a sincronização no agente de gestão de ADMA_SCONTOSO_B2B

-   Importação completa e a sincronização no agente de gestão do gráfico de B2B

-   Exportação e importação Delta e sincronização no agente de gestão de ADMA_SCONTOSO_B2B

-   Exportação, a importação de Delta e a sincronização no agente de gestão de MIMMA

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)

## <a name="finally-application-proxy-with-b2b-guest-and-logging-into-mim"></a>Por fim: O Proxy de aplicações com convidados B2B e iniciar sessão em MIM

Agora que criamos as regras de sincronização em MIM. Definir a configuração do Proxy de aplicação, utilize o princípio de nuvem para permitir o KCD no proxy de aplicação.
Além disso, em seguida adicionou o usuário manualmente para os gerir utilizadores e grupos. As opções para não mostrar o utilizador, até que a criação ocorreu em MIM para adicionar o convidado para um grupo do office uma vez provisionado requer um pouco mais de configuração não abrangido neste documento.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)

| **Apenas fluxo inicial** | **Utilizar como teste de existência** | **Fluxo (atributo de destino ⇒ de valor do FIM)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ["CONTOSO" ⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |

Depois de configurar a todos

Por fim, tem início de sessão de utilizador de B2B e ver a aplicação

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Passos Seguintes
----------

[Como Aprovisiono Utilizadores para o AD DS?](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[Referências de Funções para o FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[Como fornecer acesso remoto seguro a aplicações no local](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Transferir o agente de gestão do Microsoft Identity Manager para o Microsoft Graph (pré-visualização)](http://go.microsoft.com/fwlink/?LinkId=717495)
