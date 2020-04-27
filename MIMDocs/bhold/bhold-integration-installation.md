---
title: Instalação de integração BHOLD FIM/MIM [ BHOLD FIM/MIM] Microsoft Docs
description: Módulo de integração BHOLD adiciona gestão de funções de self-service à MIM e FIM
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 3005e06606ec4b3b6854003213c712770376b35d
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042207"
---
# <a name="bhold-fimmim-integration-installation"></a>Instalação de Integração BHOLD FIM/MIM

O módulo de integração BHOLD FIM adiciona gestão de funções de self-service à Microsof Identity Manager, possibilitando aos utilizadores solicitarem funções adicionais e aplicando quem pode assumir essas funções. O módulo de Integração BHOLD FIM estende o Portal FIM para facilitar a gestão das funções dos utilizadores como parte da administração global DO FIM. Este tópico descreve como deve configurar a sua infraestrutura de rede para permitir instalar e utilizar o módulo de integração BHOLD FIM. Também cobre como instalar o módulo de integração BHOLD FIM e a configuração que é necessária após a instalação do módulo de integração BHOLD FIM.

## <a name="bhold-fim-integration-installation-requirements"></a>Requisitos de instalação de integração BHOLD FIM

O módulo de Integração BHOLD FIM alarga o Portal fim e o Serviço FIM para permitir aos utilizadores gerirem as suas funções dentro do Portal fim. Por esta razão, é essencial que o módulo BHOLD Core e as funcionalidades NECESSÁRIAs do FIM sejam instalados e configurados antes de instalar o módulo de integração BHOLD FIM.
Seguem-se os componentes do software que devem estar presentes no computador antes de poder instalar o módulo de integração BHOLD FIM:

- Portal e Serviço Microsoft Identity Manager 2016
- Microsoft Silverlight 3 ou mais tarde
- Serviços de informação na Internet e ASP.NET
- Ferramentas Microsoft Silverlight

Além disso, os módulos de Conector de Gestão de Núcleo e Acesso BHOLD já devem ser implantados num servidor no ambiente, e o FIM deve ser configurado com um ou mais agentes de gestão BHOLD. Para obter informações sobre a instalação e configuração do módulo BHOLD Core, consulte a [Instalação Core BHOLD](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Para obter informações sobre a instalação e utilização do módulo de conector de gestão de acesso, consulte a instalação do [Conector](https://technet.microsoft.com/library/jj874042(v=ws.10).aspx) de Gestão de Acesso e o Guia de Laboratório de [Teste: Conector de Gestão](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx)de Acesso BHOLD .

> [!IMPORTANT]
> O nome da base de dados de serviços FIM deve ser FIMService. A configuração de integração BHOLD FIM falhará se o FIM não tiver sido instalado com o nome de base de dados de serviço sem fim.

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a instalar o módulo de integração BHOLD FIM, deve criar um diretório BHOLD no diretório raiz do C: disco drive (C:\BHOLD).

Além disso, é necessário estar preparado para fornecer a informação que o assistente de integração BHOLD FIM necessita para completar a instalação. A seguinte folha de cálculo irá ajudá-lo a registar essa informação para que esteja pronto para fornecê-la quando for necessária.

### <a name="bholdfim-account-settings"></a>Definições da conta BHOLDFim

| **Item**                            | **Descrição**                                                                                                                                                                                                               | **Valor**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilize o Fornecedor de Segurança no Domínio** | Quando selecionado, especifica que a segurança dos Serviços de Domínio do Diretório Ativo controlará o acesso ao BHOLD Core.                                                                                                                    | Selecione a caixa de verificação. **Importante:** A instalação falhará se esta caixa de verificação não for selecionada.                                                                                                                                                                                                                   |
| **Domínio**                          | Especifica o domínio que contém a conta de **serviço** que criou ao instalar o BHOLD Core. Para mais informações, consulte [a Instalação BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Mude o nome apenas se estiver incorreto. **Importante:** Especifique o nome de domínio utilizando o nome NetBIOS (curto) e não o nome de domínio totalmente qualificado (FQDN). Por exemplo, se o FQDN do domínio for fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **Nome de utilizador**                        | Especifica o nome de início de sessão da conta de utilizador do serviço BHOLD Core.                                                                                                                                                              | Escreva aqui o nome da conta de utilizador:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                        | Especifica a palavra-passe da conta de utilizador do serviço.                                                                                                                                                                           | Escreva a palavra-passe aqui: **Importante:** Certifique-se de manter esta palavra-passe num local escondido e seguro.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>Definições de serviço FIM

| **Item**            | **Descrição**                                                                                                                                                                                                                               | **Valor**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Utilizador**            | Especifica o nome de início de sessão de uma conta com privilégios de administrador para o FIM. A Microsoft recomenda vivamente que não utilize a conta associada ao utilizador raiz no BHOLD Core (por predefinição, a conta utilizada para instalar o BHOLD Core). | Escreva aqui o nome da conta de utilizador:                                                                   |
| **Palavra-passe**        | Especifica a palavra-passe da conta de utilizador do administrador FIM.                                                                                                                                                                                 | Escreva a palavra-passe aqui: **Importante:** Certifique-se de manter esta palavra-passe num local escondido e seguro. |
| **Base de Dados FIM**    | Especifica o nome da base de dados do Serviço FIM.                                                                                                                                                                                               | SERVIÇO FIM                                                                                          |
| **Site IP/Porto** | Especifica o nome ou endereço IP do servidor fim portal e da porta do site.                                                                                                                                                               | Escreva aqui o nome ou endereço do servidor e a porta:                                                     |

### <a name="bhold-core-connection"></a>Ligação Core BHOLD

| **Item**               | **Descrição**                                                                                                                                                                                                                                                                                                                                                                               | **Valor**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Domínio**             | Especifica o nome do domínio da conta especificada no **Utilizador,** abaixo. Especifique o domínio no formato NetBIOS (curto).                                                                                                                                                                                                                                                                   | Escrever o nome de domínio da conta de utilizador aqui?                                                            |
| **Utilizador**               | Especifica o nome de início de sessão da conta de **um utilizador BHOLD que é um supervisor** de todos os utilizadores e funções e tem permissão para ligar e desvincular as funções do utilizador. A Microsoft recomenda vivamente que não utilize a conta associada ao utilizador raiz no BHOLD Core (por predefinição, a conta utilizada para instalar o BHOLD Core). Esta conta pode ser a mesma conta que usa para ligar ao FIM | Escreva aqui o nome da conta de utilizador:                                                                   |
| **Palavra-passe**           | Especifica a palavra-passe da conta de utilizador especificada no **Utilizador**.                                                                                                                                                                                                                                                                                                                             | Escreva a palavra-passe aqui: **Importante:** Certifique-se de manter esta palavra-passe num local escondido e seguro. |
| **Endereço IP/Máquina** | Especifica o endereço IP do servidor do website BHOLD Core. Não utilize o nome do servidor.                                                                                                                                                                                                                                                                                                        | Escreva o endereço IP aqui:                                                                          |
| **Número da porta**        | Especifica o número de porta do website BHOLD Core.                                                                                                                                                                                                                                                                                                                                          | Escreva o número da porta aqui:                                                                         |

## <a name="bhold-fim-integration-setup"></a>Configuração de integração BHOLD FIM

Para instalar o módulo de integração BHOLD FIM, inicie sessão como membro do grupo Domain Admins, descarregue o seguinte ficheiro e execute-o como administrador no servidor que pretende instalar o módulo de integração BHOLD FIM em:

- \_<em>\<Versão\></em>BholdFIMIntegration Version Release.msi

Substitua * \<\> * a Versão pelo número de versão da versão do lançamento de Integração BHOLD FIM que está a instalar.

Para executar o ficheiro do programa como administrador, clique no ficheiro à direita e, em seguida, clique em **Executar como administrador**.

![execução msi](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Tarefas de pós-instalação

Depois de instalar a Integração BHOLD FIM, tem de configurar o Microsoft SharePoint para dar permissões ao site da conta de serviço BHOLD. Além disso, se o Portal FIM estiver configurado para utilizar a segurança Secure Sockets Layer (SSL), deve modificar ficheiros que contenham referências aos endereços das páginas BHOLD adicionadas ao Portal FIM.

### <a name="configuring-sharepoint"></a>Configurar o SharePoint

Para funcionar corretamente, a Integração BHOLD FIM requer que a conta de serviço BHOLD tenha permissões de membro do site no Microsoft SharePoint.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>Para conceder permissões de membro do site para a conta de serviço BHOLD

1.  Inicie sessão no servidor que executa a Integração BHOLD FIM com privilégios de administrador.

2.  Clique em **Iniciar**, e depois clique em **Internet Exporer**.

3.  Na barra de <https://localhost> endereços, escreva se o SharePoint estiver <http://localhost>configurado para utilizar a segurança SSL, caso contrário escreva .

4.  No lado esquerdo da página do Site da **equipa,** clique em **Pessoas e Grupos**.

5.  Em **Grupos** clique em **Membros do Site da Equipa**, e na barra de ferramentas central-painel, clique em **Novo**, e, em seguida, clique em **Adicionar Utilizadores**.

6.  Na página **Adicionar Utilizadores: Página do Site da Equipa,** nos **Utilizadores/Grupos,** tipo BHOLDApplicationGroup e, em seguida, clique no botão 'Ver Nomes' na caixa **Utilizadores/Grupos.** O nome do grupo deve decidir incluir o nome de domínio.

7.  Clique **em Dar permissões diretamente aos utilizadores**, selecione Controlo Completo – Tem controlo **total**, e depois clique em **OK**.

8.  Verifique se o BHOLDApplicationGroup está listado em **Permissões: Site da equipa**e, em seguida, feche o Internet Explorer.

![execução msi](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>Configurar bHOLD para suportar SSL

Se o Portal FIM estiver configurado para utilizar a segurança SSL, tem de modificar ficheiros no servidor FIM para que as ligações às páginas BHOLD sejam abertas. Os ficheiros encontram-se ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```na seguinte pasta: .

A tabela seguinte lista os ficheiros e as versões originais e alteradas das cordas a editar.

| **Ficheiro**                  | **Corda original**                                                                                                                   | **Cadeia Alterada**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics.aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.html | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.html       |
| AttestationCampaigns.aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | 
| AttestationMain.aspx      |  http://<BHOLD_Server>/bhold/Attestation/Dashboard.aspx?hideMenu=1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/Dashboard.aspx?hideMenu=1 |
| Reportagem.aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.html |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.html |
| Selfservice.aspx          | RoleExchangePoint=http://\<*FIM_Server*\>: \< *FIM_Port*\>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport | RoleExchangePoint=https://\<*FIM_Server_FQDN*\>: \< *FIM_SSL_Port\>*\>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport |

Em que:

-   BHOLD_Server especifica o nome do servidor BHOLD como encontrado na versão original do ficheiro * \<\> *

-   MIM_Server especifica o nome do servidor FIM como encontrado na versão original do ficheiro * \<\> *

-   BHOLD_Server_FQDN especifica o nome de domínio totalmente qualificado (FQDN) do servidor BHOLD * \<\> *

-   MIM_Port especifica o número de porta do servidor FIM como encontrado na versão original do ficheiro * \<\> *

-   MIM_Server_FQDN especifica o FQDN do servidor FIM * \<\> *

-   MIM_SSL_Port especifica uma porta diferente para utilização com SSL no servidor FIM * \<\> *

### <a name="enable-approval-workflows-in-bhold-core"></a>Ativar fluxos de trabalho de aprovação em BHOLD Core

Quando o FIM e o BHOLD são integrados para autosserviço, os fluxos de trabalho para aprovações são executados no Serviço FIM. Isto é semelhante ao modelo de fluxo de trabalho para pedidos originários do Portal FIM, como quando um utilizador submete um pedido de adesão a uma lista de distribuição. Existem diferenças fundamentais entre os fluxos de trabalho de função BHOLD e outros fluxos de trabalho hospedados no Serviço FIM, no entanto. No caso do BHOLD, os parâmetros de fluxo de trabalho que especificam quais os utilizadores que devem aprovar um pedido de função têm origem no BHOLD, em vez de serem armazenados nas definições de fluxo de trabalho na base de dados do Serviço FIM. Estes parâmetros são fornecidos ao Serviço FIM pela BHOLD quando o primeiro pedido é feito, e um fluxo de trabalho de ação comunica os resultados de volta ao BHOLD Core.

BHOLD seleciona um aprovador para um pedido de self-service de uma de três maneiras:

-   **Gestor de linha como aprovador: seleção baseada em papéis para uma unidade organizacional (OrgUnit)** Se uma função tiver um tipo de função nomeado para Adapprover ou Escada rolante, e se essa função estiver ligada a um ou mais utilizadores no contexto de uma OrgUnit, os pedidos dos utilizadores dentro dessa OrgUnit devem ser aprovados por um dos utilizadores que esteja ligado ao papel com o modelo de aprovação ou escada rolante.

-   **Gestor de linha como aprovador: seleção baseada em atributos para uma OrgUnit** Cada OrgUnit pode ter um ou mais atributos que especificam os pseudónimos dos utilizadores que podem aprovar atribuições de papéis para outros utilizadores no OrgUnit. Estes atributos são nomeados approver1, approver2, e assim por diante. Quando um utilizador da OrgUnit solicita uma atribuição de funções, o BHOLD encaminha o pedido (através do FIM) para os utilizadores especificados pelos atributos do utilizador orgUnit. Se uma OrgUnit não tiver nenhum destes atributos definido, o BHOLD verifica as OrgUnits dos pais até à raiz orgUnit.

-   **Gestor de funções como aprovador: seleção baseada em atributos para um papel** Uma função pode ter um ou mais atributos (também nomeados approver1, e assim por diante) que especificam os pseudónimos dos utilizadores que podem aprovar a atribuição da função. Quando um utilizador pede a atribuir uma função que tenha estes atributos de aprovação definidos, o BHOLD encaminha o pedido para os utilizadores especificados pelos atributos.

Se um aprovador para um pedido de função de autosserviço não for especificado por um destes métodos, por padrão bHOLD atribui automaticamente a função sem necessitar de aprovação. Por esta razão, imediatamente após a instalação da Integração BHOLD FIM, deve configurar a raiz OrgUnit com o pseudónimo de um aprovador, como a conta raiz. Isto evitará que um utilizador seja involuntariamente concedido um papel antes de uma política de aprovação mais abrangente poder ser implementada.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>Para configurar um aprovador para a raiz OrgUnit

1.  Inicie sessão no servidor BHOLD Core como administrador.

2.  Clique em **Iniciar,** e depois clique no **Internet Explorer**.

3.  Na barra de endereços <http://localhost:5151/bhold/core>Do Internet Explorer, escreva e, em seguida, prima a tecla Enter.

4.  Na página inicial do BHOLD Core, em **atributo def,** clique nos **tipos de Atributos**.

5.  Na página do **tipo Atributo,** clique em **Adicionar**.

6.  Na **página**do tipo add , em **Identidade,** tipo approver1, na lista do **tipo Dedados,** clique em **AlphaNumeric,** no **comprimento máximo,** tipo 255, na **Lista de valores,** clique **em Não,** em **inglês,** tipo Aprovador 1, clique em **OK,** e depois clique **em Done**.

7.  No painel esquerdo, sob o **Attribute def** clique em **conjuntos de tipo atributo**.

8.  Na página de conjuntos do **tipo Atributo,** clique em **Adicionar**.

9.  Na página de conjunto de **atributos adicionar,** na **Descrição,** type OrgUnit Atributos, e, em seguida, clicar **EM OK**.

10. Na página **Deatributos OrgUnit,** expanda **os tipos de Atributos**e, em seguida, clique em **Modificar**.

11. Na lista do **tipo Atributo,** clique em **approver1,** clique em **Adicionar,** e depois clique **em Done**.

12. No painel esquerdo, clique nos **tipos de objetos**.

13. Na página dos tipos de **objetos,** clique em **OrgUnit**.

14. Na página **Do Tipo objeto/OrgUnit,** expanda **os conjuntos do tipo de atributo,** e clique em **Modificar**.

15. Na página de conjunto de tipo de **atributo de Link/OrgUnit,** em **Ordem**, tipo 10, na lista de conjuntos do **tipo Atributo,** clique em **Atributos OrgUnit,** clique em **Adicionar**, e, em seguida, clique **em Done**.

16. No painel esquerdo, em **Modelo,** clique em **unidades organizacionais**.

17. Na página **unidades organizacionais,** clique na **raiz**.

18. Na **página unidade/raiz organizacional,** clique em **Modificar**.

19. No grupo **Desacie atributos/página raiz** da unidade organizacional, no **Approver,** escreva o domínio e o nome de utilizador do utilizador que aprovará pedidos de atribuição de papéis, no*\<utilizador\>* do * \<\>domínio*\\do formato, onde * \<\> * o domínio é o nome de domínio NetBIOS (curto) e * \<\> * o utilizador é o nome de logon do utilizador.
20. Clique em **OK**.

> [!IMPORTANT]
> O domínio e o nome do utilizador devem corresponder ao pseudónimo predefinido de um utilizador na base de dados BHOLD Core.

Como alternativa à especificação de um aprovador para unidades organizacionais, pode especificar um aprovador para funções propostas na base de dados BHOLD Core. Para tal, crie o atributo do approver1, adicione-o a uma máquina de atributo associada ao tipo de objeto role, e depois modifique cada função proposta para especificar o executante.

Para proporcionar uma melhor segurança no fluxo de trabalho, para além dos aprovadores, deve designar modos de aprovação e utilizadores adicionais, criando e povoando os seguintes atributos para OrgUnits e funções:

- escada rolante<em>\<n\></em>

- proprietário<em>\<n\></em>

- segurançaOficial<em>\<n\></em>

- notificação<em>\<n\></em>

onde * \<\> n* indica um sufixo numérico opcional para fornecer múltiplos atributos do mesmo tipo.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>Verificar fluxos de trabalho de aprovação configurados no Serviço FIM

A instalação de integração BHOLD FIM cria conjuntos, definições de fluxo de trabalho e Regras de Política de Gestão (MPRs) para o Serviço FIM. Se tiver personalizado a sua implementação FIM para alterar os conjuntos de administradores ou os conjuntos de utilizadores que podem fazer pedidos, deve certificar-se de que os MPRs referenciam os conjuntos de utilizador corretos.

> [!NOTE]
> Antes de os utilizadores do Portal FIM poderem utilizar as funcionalidades de self-service fornecidas pelo BHOLD, as contas dos utilizadores devem ser sincronizadas na base de dados BHOLD do Serviço de Sincronização FIM. Em particular, deve haver um registo de utilizador na base de dados BHOLD Core e na base de dados do Serviço FIM para cada utilizador que possa fazer um pedido de self-service ou seja especificado como um aprovador ou escada rolante para pedidos de self-service.

## <a name="next-steps"></a>Passos seguintes

- Para obter informações sobre a instalação do Portal FIM e outras funcionalidades do FIM, consulte [Planeamento e Arquitetura](https://technet.microsoft.com/library/ee808044.aspx) na Biblioteca Técnica Microsoft Forefront.
- [Guia de instalação BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
