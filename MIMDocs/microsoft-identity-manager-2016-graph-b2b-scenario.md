---
title: Configurar o conector do Microsoft Identity Manager para o Microsoft Graph para B2B/ Microsoft Docs
author: billmath
description: O conector Microsoft Graph é uma gestão externa do ciclo de vida da conta aD. Neste cenário, uma organização convidou os hóspedes para o seu diretório Azure AD, e deseja dar a esses hóspedes acesso a aplicações integradas no Windows ou as aplicações baseadas em Kerberos
keywords: ''
ms.author: billmath
manager: daveba
ms.date: 10/02/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 2f91a5c24df5475130755574c77b536f57e64d24
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044247"
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy"></a>Colaboração Azure AD business-to-business (B2B) com microsoft Identity Manager (MIM) 2016 SP1 com Procuração de Aplicação Azure
============================================================================================================================

O cenário inicial é a gestão externa do ciclo de vida da conta aD.   Neste cenário, uma organização convidou os hóspedes para o seu diretório Azure AD, e deseja dar a esses hóspedes acesso a aplicações integradas no Windows ou em Kerberos, através do proxy de [aplicação da AD Azure](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-publish) ou de outros mecanismos de gateway. O representante da aplicação Azure AD exige que cada utilizador tenha a sua própria conta AD DS, para efeitos de identificação e delegação.

## <a name="scenario-specific-guidance"></a>Orientação específica do cenário

Alguns pressupostos feitos na configuração de B2B com mim e procuração de aplicação ad-ad- Azure:

-   Já implementou um AD no local, e o Microsoft Identity Manager está instalado e configuração básica do Serviço MIM, portal MIM, Ative Directory Management Agent (AD MA) e AGENTE de Gestão FIM (FIM MA).
    <https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-deploy>

-   Já seguiu as instruções do artigo sobre como descarregar e instalar o [conector Graph](microsoft-identity-manager-2016-connector-graph.md).

-   Tem o Azure AD Connect configurado para sincronizar utilizadores e grupos para a AD Azure.

-   Já criou conectores e grupos de conectores proxy de aplicação, se não puder visitar [aqui](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector) para instalar e configurar

-   Já publicou uma ou mais aplicações, que dependem da Autenticação Integrada do Windows ou contas de AD individuais através do Azure AD App Proxy

-   Convidou ou convidou um ou mais convidados, o que resultou na criação de um ou mais utilizadores em Azure AD <https://docs.microsoft.com/azure/active-directory/active-directory-b2b-self-service-portal>



## <a name="b2b-end-to-end-deployment-example-scenario"></a>B2B cenário de exemplo de implantação final para fim

Este guia baseia-se no seguinte cenário:

A Contoso Pharmaceuticals trabalha com a Trey Research Inc. como parte do departamento de I&D. Os colaboradores da Trey Research precisam de aceder ao pedido de relatório de investigação fornecido pela Farmacêutica Contoso.

-   Os Farmacêuticos Contoso estão no seu próprio inquilino, para configurar um domínio personalizado.

-   Alguém convidou um utilizador externo para o inquilino da Farmacêutica Contoso.
    Este utilizador aceitou o convite e pode aceder a recursos partilhados.

-   A Contoso Pharmaceuticals publicou uma aplicação via App Proxy. Neste cenário, a aplicação de exemplo é o Portal MIM. Isto permitiria que um utilizador convidado participasse em processos MIM, por exemplo em cenários de help desk ou para solicitar acesso a grupos em MIM.


## <a name="configure-ad-and-azure-ad-connect-to-exclude-users-added-from-azure-ad"></a>Configure AD e Azure AD Connect para excluir utilizadores adicionados da AD Azure

Por padrão, o Azure AD Connect assumirá que os utilizadores não administrativos em Diretório Ativo precisam de ser sincronizados em Azure AD.  Se o Azure AD Connect encontrar um utilizador existente em AD Azure que corresponda ao utilizador a partir de AD no local, o Azure AD Connect corresponderá às duas contas e assumirá que se trata de uma sincronização anterior do utilizador e tornará a AD no local autorizada.  No entanto, este comportamento predefinido não é adequado para o fluxo B2B, onde a conta de utilizador tem origem em AD Azure. 

Por isso, os utilizadores trazidos para AD DS por MIM da Azure AD precisam de ser armazenados de uma forma que a Azure AD não tente sincronizar esses utilizadores de volta ao Azure AD.
Uma maneira de fazer isso é criar uma nova unidade organizacional em AD DS, e configurar o Azure AD Connect para excluir aquela unidade organizacional.  

Mais informações podem ser encontradas no [sincronizacro Azure AD Connect: Configurar a filtragem](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering). 
 

## <a name="create-the-azure-ad-application"></a>Criar a aplicação Azure AD 


Nota: Antes de criar em MIM Sync o agente de gestão para o conector gráfico, certifique-se de que reviu o guia para a implementação do [Conector gráfico,](microsoft-identity-manager-2016-connector-graph.md)e criou uma aplicação com um ID do cliente e segredo.
Certifique-se de que o pedido foi autorizado para pelo menos uma destas permissões: `User.Read.All`, `User.ReadWrite.All`, `Directory.Read.All` ou `Directory.ReadWrite.All`. 

## <a name="create-the-new-management-agent"></a>Criar o Novo Agente de Gestão


No UI gestor de serviços de sincronização, selecione **Conectores** e **Crie**.
Selecione **Graph (Microsoft)**  e dê-lhe um nome descritivo.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>Conectividade

Na página conectividade, deve especificar a versão API do gráfico. Produção pronta PAI é **V 1.0**, Não Produção é **Beta**.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="global-parameters"></a>Parâmetros globais

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Configure Hierarquia de Provisionamento

Esta página é usada para mapear o componente DN, por exemplo, ou, para o tipo de objeto que deve ser provisionado, por exemplo organizacionalUnidade. Isto não é necessário para este cenário, por isso deixe isto como padrão e clique em seguida.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>Configurar divisórias e hierarquias

Na página de divisórias e hierarquias, selecione todos os espaços de nome com objetos que planeia importar e exportar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>Selecione tipos de objetos

Na página dos tipos de objetos, selecione os tipos de objetos que pretende importar. Deve selecionar pelo menos 'Utilizador'.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>Selecionar Atributos

No ecrã Select Atributos, selecione atributos do Azure AD que serão necessários para gerir os utilizadores B2B em AD. É necessário o atributo "ID".  Os atributos `userPrincipalName` e `userType` serão usados mais tarde nesta configuração.  Outros atributos são opcionais, incluindo

-   `displayName`

-   `mail`

-   `givenName`

-   `surname`

-   `userPrincipalName`

-   `userType`

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>Configure âncoras

No ecrã Configure Anchor, configurar o atributo de âncora é um passo necessário. por predefinição, utilize o atributo de ID para mapeamento do utilizador.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>Configure filtro de conector

Na página de filtro de conector configurar, mim permite filtrar objetos com base no filtro do atributo. Neste cenário para o B2B, o objetivo é apenas trazer utilizadores com o valor do atributo `userType` que seja igual a `Guest`, e não utilizadores com o utilizadorType que seja igual a `member`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>Configure regras de adesão e projeção

Este guia assume que estará a criar uma regra de sincronização.  Como as regras de configuração de Adesão e Projeção são tratadas por regra de sincronização, não é necessário identificar uma adesão e projeção no próprio conector. Deixe o padrão e clique bem.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>Configure fluxo de atributo

Este guia assume que estará a criar uma regra de sincronização.  A projeção não é necessária para definir o fluxo de atributos em MIM Sync, uma vez que é tratado pela regra de sincronização que é criada mais tarde. Deixe o padrão e clique bem.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Configurar a desprovisionamento

A definição para configurar a desprovisionamento permite configurar a sincronização mim para eliminar o objeto, se o objeto metaverso for eliminado. Neste cenário, fazemos deles desconectadores, pois o objetivo é deixá-los em Azure AD. Neste cenário, não estamos a exportar nada para a AD Azure, e o conector está configurado apenas para importar.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>Configurar extensões

Configurar extensões neste agente de gestão é uma opção, mas não é necessária porque estamos a usar uma regra de sincronização. Se decidirmos usar uma regra avançada no fluxo de atributos mais cedo, então haveria uma opção para definir a extensão das regras.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="extending-the-metaverse-schema"></a>Estender o esquema metaverso


Antes de criar a regra de sincronização, precisamos de criar um atributo chamado UserPrincipalName ligado ao objeto da pessoa usando o MV Designer.

No cliente de Sincronização, selecione Metaverse Designer

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

Em seguida, selecione o tipo de objeto de pessoa

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

Próximo sob ações clique em Adicionar Atributo

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

Em seguida, complete os seguintes detalhes

Nome do atributo: **userPrincipalName**

Tipo de atributo: **String (Indexável)**

Indexado = **Verdadeiro**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

## <a name="creating-mim-service-synchronization-rules"></a>Criação de Regras de Sincronização de Serviço mim

nos degraus abaixo iniciamos o mapeamento da conta de hóspedes B2B e o fluxo de atributos. São aqui apresentados alguns pressupostos: que já tem o MA de Diretório Ativo configurado e o FIM MA configurado para levar os utilizadores ao Serviço mim e portal.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

Os próximos passos exigirão a adição de uma configuração mínima ao FIM MA e ao AD MA.

Mais detalhes podem ser encontrados aqui para a configuração <https://technet.microsoft.com/library/ff686263(v=ws.10).aspx> - Como posso fornecer utilizadores a DS DS

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>Regra de sincronização: Importar utilizador convidado para MV para serviço de sincronização Metaverse do Diretório Ativo Azure<br>

Navegue para o Portal MIM, selecione Regras de Sincronização e clique em novas.  Crie uma regra de sincronização de entrada para o fluxo B2B através do conector gráfico.
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png)

No passo dos critérios de relacionamento, certifique-se de selecionar "Criar recurso no FIM".
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

Configure as seguintes regras de fluxo de atributode de entrada.  Certifique-se de que povoa os atributos `accountName`, `userPrincipalName` e `uid`, pois serão utilizados mais tarde neste cenário:

| **Apenas fluxo inicial** | **Usar como Teste de Existência** | **Fluxo (Valor Fonte ; Atributo FIM)**                          |
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

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>Regra de sincronização: Criar a conta de utilizador convidado para o Diretório Ativo 

Esta regra de sincronização cria o utilizador em Diretório Ativo.  Certifique-se de que o fluxo de `dn` deve colocar o utilizador na unidade organizacional que foi excluída do Azure AD Connect.  Além disso, atualize o fluxo para `unicodePwd` para cumprir a sua política de senha aD - o utilizador não precisará de saber a palavra-passe.  Note que o valor da `262656` para `userAccountControl` codifica as bandeiras `SMARTCARD_REQUIRED` e `NORMAL_ACCOUNT`.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

Regras de fluxo:

| **Apenas fluxo inicial** | **Usar como Teste de Existência** | **Fluxo (Valor FIM ) Atributo de destino)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName](javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [sn⇒sn](javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
| **Y**                 |                           | ["CN ="+ uid +", UO = B2BGuest, DC = contoso, DC = com" ⇒dn](javascript:void(0);) |
| **Y**                 |                           | [RandomNum (0,999) + userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="optional-synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>Regra opcional de sincronização: Importar Objetos de utilizador convidado B2B SID para permitir o login para MIM 

Esta regra de sincronização de entrada traz o atributo SID do utilizador do Ative Directory de volta para MIM, para que o utilizador possa aceder ao Portal MIM.  O Portal MIM exige que o utilizador tenha os atributos `samAccountName`, `domain` e `objectSid` povoados na base de dados do Serviço MIM.

Configure o sistema externo de origem como o `ADMA`, uma vez que o atributo `objectSid` será definido automaticamente pela AD quando mim criar o utilizador.
 
Note que se configurar os utilizadores a serem criados no Serviço MIM, certifique-se de que não estão no âmbito de quaisquer conjuntos destinados às regras de política de gestão de SSPR dos empregados.  Pode ser necessário alterar as definições definidas para excluir os utilizadores criados pelo fluxo B2B. 

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)


| **Apenas fluxo inicial** | **Usar como Teste de Existência** | **Fluxo (Valor Fonte ; Atributo FIM)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ["CONTOSO" ⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |


## <a name="run-the-synchronization-rules"></a>Executar as regras de sincronização

Em seguida, convidamos o utilizador e, em seguida, executamos as regras de sincronização do agente de gestão na seguinte ordem:

-   Importação completa e sincronização no `MIMMA` Agente de Gestão.  Isto garante que mim Sync tem as mais recentes regras de sincronização configuradas.

-   Importação completa e sincronização no `ADMA` Agente de Gestão.  Isto garante que mim e Diretório Ativo são consistentes.  Neste momento, ainda não haverá exportações pendentes para os hóspedes.

-   Importação completa e sincronização no Agente de Gestão de Gráficos B2B.  Isto traz os utilizadores convidados para o metaverso.  Neste momento, uma ou mais contas estarão pendentes de exportação para `ADMA`.  Se não houver exportações pendentes, verifique se os utilizadores convidados foram importados para o espaço do conector e que as regras foram configuradas para que lhes fossem dadas contas AD.

-   Exportação, Delta Import e Sincronização no `ADMA` Management Agent.  Se as exportações falharem, verifique a configuração da regra e determine se existem requisitos de esquema em falta. 

-   Exportação, Delta Import e Sincronização no `MIMMA` Management Agent.  Quando isto estiver concluído, não deverá continuar a haver exportações pendentes.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)


## <a name="optional-application-proxy-for-b2b-guests-logging-into-mim-portal"></a>Opcional: Procuração de candidatura para hóspedes B2B que aceda ao Portal MIM

Agora que criámos as regras de sincronização na MIM. Na configuração do Proxy da Aplicação, defina a utilização do principal da nuvem para permitir o KCD no proxy da aplicação.
Além disso, em seguida, adicionou o utilizador manualmente aos utilizadores e grupos de gestão. As opções de não mostrar o utilizador até que a criação tenha ocorrido em MIM para adicionar o hóspede a um grupo de escritórios uma vez que o provisionado requer um pouco mais de configuração não abrangida neste documento.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)


Uma vez configurado, faça login no utilizador B2B e consulte a aplicação.

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>Passos Seguintes
----------

[Como Aprovisiono Utilizadores para o AD DS?](https://technet.microsoft.com/library/ff686263(v=ws.10).aspx)

[Referências de Funções para o FIM 2010](https://technet.microsoft.com/library/ff800820(v=ws.10).aspx)

[Como fornecer acesso remoto seguro a aplicações no local](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started)

[Descarregue o conector do Microsoft Identity Manager para o Microsoft Graph](https://go.microsoft.com/fwlink/?LinkId=717495)
