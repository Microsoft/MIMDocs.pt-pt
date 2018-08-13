---
title: Instalação do BHOLD FIM/MIM integração | Documentos da Microsoft
description: Módulo de integração do BHOLD adicionar gerenciamento de funções de autoatendimento para MIM e de FIM
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 08a0aaa60891727482e80c8998cc075eacf042cf
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290174"
---
# <a name="bhold-fimmim-integration-installation"></a>Instalação do BHOLD Integration FIM/MIM

O módulo de integração do BHOLD FIM adiciona a gestão de funções de autoatendimento para Microsof Identity Manager, tornando possível para que os utilizadores peçam funções adicionais e a imposição de quem pode realizar nessas funções. O módulo de integração do BHOLD FIM estende o Portal do FIM para que seja fácil de gerir funções dos utilizadores como parte da administração de FIM geral. Este tópico descreve como deve configurar a sua infraestrutura de rede para que possa instalar e utilizar o módulo de integração do BHOLD FIM. Também aborda como instalar o módulo de integração do BHOLD FIM e a configuração necessária depois de instalar o módulo de integração do BHOLD FIM.

## <a name="bhold-fim-integration-installation-requirements"></a>Requisitos de instalação do BHOLD FIM integração

O módulo de integração do BHOLD FIM estende o Portal do FIM e serviço de FIM para permitir que os utilizadores para gerir as respetivas funções no Portal do FIM. Por esse motivo, é essencial que o módulo do BHOLD Core e os recursos necessários de FIM ser instalados e configurados antes de instalar o módulo de integração do BHOLD FIM.
Seguem-se os componentes de software que tem de estar presentes no computador antes de poder instalar o módulo de integração do BHOLD FIM:

- Portal do Microsoft Identity Manager 2016 e o serviço
- Microsoft Silverlight 3 ou posterior
- Serviços de informação de Internet e ASP.NET
- Ferramentas do Microsoft Silverlight

Além disso, os módulos do BHOLD Core e o conector de gestão de acesso já tem de ser implementados num servidor no ambiente e FIM tem de ser configurado com um ou mais agentes de gestão do BHOLD. Para obter informações sobre como instalar e configurar o módulo do BHOLD Core, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Para obter informações sobre como instalar e utilizar o módulo de conector de gestão de acesso, consulte [instalação do conector de gestão de acesso](https://technet.microsoft.com/library/jj874042(v=ws.10).aspx) e [guia de laboratório de teste: conector do BHOLD Access Management](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

> [!IMPORTANT]
> O nome da base de dados do serviço FIM tem de ser FIMService. Configuração de integração do BHOLD FIM irá falhar se o FIM não foi instalado com o nome de base de dados do serviço FIM padrão.

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a instalar o módulo de integração do BHOLD FIM, tem de criar um diretório do BHOLD no diretório de raiz da unidade de disco c: (C:\BHOLD).

Além disso, precisa estar preparado para fornecer as informações que o Assistente de configuração de integração do BHOLD FIM necessita para concluir a instalação. A seguinte planilha ajudará a gravar essa informação para que estará pronto para fornecer quando for necessário.

### <a name="bholdfim-account-settings"></a>Definições da conta de BHOLDFim

| **Item**                            | **Descrição**                                                                                                                                                                                                               | **Valor**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilize um fornecedor de segurança no domínio** | Quando selecionada, especifica que a segurança de serviços de domínio do Active Directory irá controlar o acesso a BHOLD Core.                                                                                                                    | Selecione a caixa de verificação. **Importante:** a instalação falhará se esta caixa de verificação não estiver selecionada.                                                                                                                                                                                                                   |
| **Domínio**                          | Especifica o domínio que contém o **conta de serviço** que criou ao instalar o BHOLD Core. Para obter mais informações, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome apenas se estiver incorreto. **Importante:** especifique o nome de domínio com o nome NetBIOS do (curto), não o nome de domínio completamente qualificado (FQDN). Por exemplo, se o FQDN do domínio fabrikam.com, especifique o nome de domínio como a FABRIKAM. |
| **Nome de Utilizador**                        | Especifica o nome de início de sessão da conta de utilizador de serviço do BHOLD Core.                                                                                                                                                              | Escreva o nome da conta de utilizador aqui:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                        | Especifica a palavra-passe da conta de utilizador de serviço.                                                                                                                                                                           | Escrever a palavra-passe aqui: **importante:** Certifique-se de que tenha esta palavra-passe num local seguro oculto.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>Definições do serviço FIM

| **Item**            | **Descrição**                                                                                                                                                                                                                               | **Valor**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **User**            | Especifica o nome de início de sessão de uma conta com privilégios de administrador para o FIM. A Microsoft recomenda vivamente que não utilize a conta associada ao utilizador raiz no BHOLD Core (por predefinição, a conta utilizada para instalar o BHOLD Core). | Escreva o nome da conta de utilizador aqui:                                                                   |
| **Palavra-passe**        | Especifica a palavra-passe da conta de utilizador de administrador do FIM.                                                                                                                                                                                 | Escrever a palavra-passe aqui: **importante:** Certifique-se de que tenha esta palavra-passe num local seguro oculto. |
| **Base de dados FIM**    | Especifica o nome da base de dados do serviço FIM.                                                                                                                                                                                               | FIMService                                                                                          |
| **IP/porta do Web site** | Especifica o nome ou endereço IP do servidor do Portal do FIM e a porta do Web site.                                                                                                                                                               | Escreva o nome do servidor ou o endereço e a porta aqui:                                                     |

### <a name="bhold-core-connection"></a>Ligação do BHOLD Core

| **Item**               | **Descrição**                                                                                                                                                                                                                                                                                                                                                                               | **Valor**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Domínio**             | Especifica o nome do domínio da conta especificada no **utilizador**, abaixo. Especifique o domínio no formato NetBIOS de (pequeno).                                                                                                                                                                                                                                                                   | Escrever o nome de domínio de conta de utilizador aqui?                                                            |
| **User**               | Especifica o nome de início de sessão da conta de **um usuário BHOLD, que é um supervisor** de todos os utilizadores e funções e tem permissão para ligar e desligar as funções de utilizador. A Microsoft recomenda vivamente que não utilize a conta associada ao utilizador raiz no BHOLD Core (por predefinição, a conta utilizada para instalar o BHOLD Core). Esta conta pode ser a mesma conta que utiliza para ligar para o FIM | Escreva o nome da conta de utilizador aqui:                                                                   |
| **Palavra-passe**           | Especifica a palavra-passe da conta de utilizador especificada no **utilizador**.                                                                                                                                                                                                                                                                                                                             | Escrever a palavra-passe aqui: **importante:** Certifique-se de que tenha esta palavra-passe num local seguro oculto. |
| **Endereço IP/máquina** | Especifica o endereço IP do servidor de Web site do BHOLD Core. Não utilize o nome do servidor.                                                                                                                                                                                                                                                                                                        | Escreva o endereço IP aqui:                                                                          |
| **Número de porta**        | Especifica o número de porta do site do BHOLD Core.                                                                                                                                                                                                                                                                                                                                          | Escreva o número de porta aqui:                                                                         |

## <a name="bhold-fim-integration-setup"></a>Configuração de integração do BHOLD FIM

Para instalar o módulo de integração do BHOLD FIM, inicie sessão como membro do grupo Admins do domínio, transfira o ficheiro seguinte e executá-lo como administrador no servidor que pretende instalar o módulo de integração do BHOLD FIM em:

- BholdFIMIntegration<em>\<versão\></em>\_Release.msi

Substitua *\<versão\>* com o número de versão da versão de integração do BHOLD FIM que está a instalar.

Para executar o ficheiro de programa como administrador, o ficheiro com o botão direito e, em seguida, clique em **executar como administrador**.

![com o msi](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Tarefas de pós-instalação

Após a instalação do BHOLD FIM integração, tem de configurar o Microsoft SharePoint para conceder as permissões de proprietário do site de conta de serviço do BHOLD. Além disso, se o Portal do FIM está configurado para utilizar a segurança de Secure Sockets Layer (SSL), tem de modificar os ficheiros que contêm referências para os endereços de páginas do BHOLD adicionados ao Portal do FIM.

### <a name="configuring-sharepoint"></a>Configurando o SharePoint

Para funcionar corretamente, a integração do BHOLD FIM requer a conta de serviço do BHOLD ter permissões de membros do site no Microsoft SharePoint.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>Para conceder permissões de membro de site para a conta de serviço do BHOLD

1.  Inicie sessão no servidor que executa a integração do BHOLD FIM com privilégios de administrador.

2.  Clique em **começar**e, em seguida, clique em **Internet Exporer**.

3.  Na barra de endereço, escreva <https://localhost> se o SharePoint estiver configurado para utilizar a segurança SSL, caso contrário, escreva <http://localhost>.

4.  No lado esquerdo dos **Site de equipa** página, clique em **pessoas e grupos**.

5.  Sob **grupos** clique em **membros do Site de equipe**e na barra de ferramentas da painel central, clique em **New**e, em seguida, clique em **adicionar usuários**.

6.  Na **adicionar utilizadores: Site da Equipe** na página **utilizadores/grupos**, escreva BHOLDApplicationGroup e, em seguida, clique no botão Verificar nomes no **utilizadores/grupos** caixa. O nome do grupo deve resolver para incluir o nome de domínio.

7.  Clique em **conceder permissões de utilizadores diretamente**, selecione **controlo total – tem controlo total**e, em seguida, clique em **OK**.

8.  Certifique-se de que BHOLDApplicationGroup consta no **permissões: Site da Equipe**e, em seguida, feche o Internet Explorer.

![com o msi](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>Configurar BHOLD para oferecer suporte a SSL

Se o Portal do FIM está configurado para utilizar a segurança SSL, tem de modificar ficheiros no servidor do FIM, para que links para páginas do BHOLD será aberto. Os ficheiros estão localizados na seguinte pasta: ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```.

A tabela seguinte lista os ficheiros e as versões originais e as alterações das cadeias de caracteres sejam editadas.

| **Ficheiro**                  | **Cadeia de caracteres original**                                                                                                                   | **Cadeia de caracteres alterada**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics.aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.html | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.html       |
| AttestationCampaigns.aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | 
| AttestationMain.aspx      |  http://<BHOLD_Server>/bhold/Attestation/Dashboard.aspx?hideMenu=1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/Dashboard.aspx?hideMenu=1 |
| Reporting.aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.html |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.html |
| Selfservice.aspx          | RoleExchangePoint=http://\<*FIM_Server*\>: \<*FIM_Port*\>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport | RoleExchangePoint=https://\<*FIM_Server_FQDN*\>: \<*FIM_SSL_Port\>*\>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport |

Em que:

-   *\<BHOLD_Server\>*  Especifica o nome do servidor do BHOLD como encontrada na versão original do ficheiro

-   *\<MIM_Server\>*  Especifica o nome do servidor do FIM como encontrada na versão original do ficheiro

-   *\<BHOLD_Server_FQDN\>*  Especifica o nome de domínio completamente qualificado (FQDN) do servidor do BHOLD

-   *\<MIM_Port\>*  Especifica o número de porta do servidor do FIM como encontrada na versão original do ficheiro

-   *\<MIM_Server_FQDN\>*  Especifica o FQDN do servidor do FIM

-   *\<MIM_SSL_Port\>*  Especifica uma porta diferente para utilização com SSL no servidor do FIM

### <a name="enable-approval-workflows-in-bhold-core"></a>Ativar os fluxos de trabalho de aprovação no BHOLD Core

Ao FIM e BHOLD estão integrados para Self-Service, os fluxos de trabalho para aprovações são executados no serviço do FIM. Isso é semelhante ao modelo de fluxo de trabalho de pedidos que têm origem no Portal do FIM, por exemplo, quando um utilizador submete um pedido para aderir uma lista de distribuição. Existem diferenças importantes entre fluxos de trabalho do BHOLD função e outros fluxos de trabalho alojados no serviço do FIM, no entanto. No caso do BHOLD, os parâmetros de fluxo de trabalho que especificam que utilizadores têm de aprovar um pedido de função são originados no BHOLD, em vez de a ser armazenados nas definições de fluxo de trabalho da base de dados do serviço FIM. Esses parâmetros são fornecidos para o serviço FIM pelo BHOLD quando a primeira solicitação é feita, e um fluxo de trabalho da ação comunica os resultados de volta para BHOLD Core.

BHOLD seleciona um aprovador para um pedido de self-service de uma das três formas:

-   **Gestor de linha como aprovador: seleção baseada em funções para uma unidade organizacional (OrgUnit)** se uma função tem um atributo denominado roletype que está definida como aprovador ou Escalator, e se essa função estiver associada a um ou mais utilizadores no contexto de um OrgUnit, os pedidos de os utilizadores dentro desse OrgUnit devem ser aprovados por um dos utilizadores que está ligado à função com o aprovador ou Escalator roletype.

-   **Gestor de linha como aprovador: seleção de baseadas em atributos de mensagens em fila para um OrgUnit** OrgUnit cada pode ter um ou mais atributos que especificam os aliases de utilizadores que podem aprovar atribuições de funções para outros utilizadores no OrgUnit. Esses atributos são nomeados approver1 approver2 e assim por diante. Quando um utilizador no OrgUnit solicita uma atribuição de função, o BHOLD encaminha a solicitação (usando o FIM) para os utilizadores especificados pelos atributos de aprovador OrgUnit. Se um OrgUnit não tiver este conjunto de atributos, BHOLD verifica principal OrgUnits até a raiz OrgUnit.

-   **Gestor de funções como aprovador: com base no atributo de seleção para uma função** uma função pode ter um ou mais atributos (também denominado approver1 e assim por diante) que especificam os aliases de utilizadores que podem aprovar a atribuição da função. Quando um utilizador solicita a ser atribuída uma função que tenha estes atributos de aprovador definida, o BHOLD encaminha o pedido para os utilizadores especificados pelos atributos.

Se um aprovador para um pedido de função de self-service não é especificado por um dos seguintes métodos, por predefinição BHOLD atribui automaticamente a função sem a necessidade de aprovação. Por esse motivo, logo após a instalação do BHOLD FIM integração, deve configurar a raiz OrgUnit com o alias de um aprovador, tal como a conta de raiz. Isto irá impedir que um utilizador de forma não intencional ser concedida uma função antes de uma política de aprovação mais abrangente pode ser implementada.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>Para configurar um aprovador para a raiz OrgUnit

1.  Inicie sessão no servidor do BHOLD Core como um administrador.

2.  Clique em **começar**e, em seguida, clique em **Internet Explorer**.

3.  Na barra de endereço do Internet Explorer, escreva <http://localhost:5151/bhold/core>e, em seguida, prima a tecla Enter.

4.  Com as principais do BHOLD página inicial, em **atributo def**, clique em **os tipos de atributo**.

5.  Sobre o **tipo de atributo** página, clique em **Add**.

6.  Na **página de tipo de atributo de adicionar**, na **identidade**, escreva approver1, o **tipo de dados** , clique em **alfanumérico**, no **Comprimento máximo**, escreva 255, na **lista de valores**, clique em **não**, no **inglês**, escreva 1 do aprovador, clique em **OK**e, em seguida, clique em **feito**.

7.  No painel esquerdo, sob **atributo def** clique em **conjuntos de tipo de atributo**.

8.  Sobre o **conjuntos de tipo do atributo** página, clique em **Add**.

9.  Sobre o **adicionar conjunto de tipo de atributo** na página **Descrição**, escreva OrgUnit atributos e, em seguida, clique em **OK**.

10. Sobre o **OrgUnit atributos** página, expanda **os tipos de atributo**e, em seguida, clique em **modificar**.

11. Na **tipo de atributo** , clique em **approver1**, clique em **Add**e, em seguida, clique em **feito**.

12. No painel esquerdo, clique em **tipos de objeto**.

13. Sobre o **tipos de objeto** página, clique em **OrgUnit**.

14. Sobre o **OrgUnit/tipo de objeto** página, expanda **conjuntos de tipo do atributo**e, em seguida, clique em **modificar**.

15. No **ligar o conjunto de tipo de atributo/OrgUnit** na página **ordem**, digite 10, o **conjunto do tipo de atributo** , clique em **OrgUnit atributos**, Clique em **Add**e, em seguida, clique em **feito**.

16. No painel esquerdo, sob **modelo**, clique em **unidades organizacionais**.

17. Sobre o **unidades organizacionais** página, clique em **raiz**.

18. Sobre o **raiz/unidade organizacional** página, clique em **modificar**.

19. Sobre o **modificar atributos/raiz da unidade organizacional** na página **aprovador**, escreva o nome de utilizador e domínio do utilizador que irá aprovar pedidos de atribuição de função, no formato  *\<domínio\>*\\*\<utilizador\>*, em que *\<domínio\>* é o Nome de domínio NetBIOS do (curto) e *\<usuário\>* é o nome de início de sessão do utilizador.
20. Clique em **OK**.

> [!IMPORTANT]
> O nome de domínio e o utilizador tem de corresponder ao alias padrão de um utilizador na base de dados do BHOLD Core.

Como alternativa à especificação de um aprovador para unidades organizacionais, pode especificar um aprovador para funções de proposta na base de dados do BHOLD Core. Para fazer isso, criar o atributo approver1, adicionar escritas-lo a um atributo associados ao tipo de objeto de função e, em seguida, modifique cada função proposta para especificar o aprovador.

Para proteger melhor o fluxo de trabalho, além de aprovadores, deve designar modos de aprovação adicional e os usuários criando e preenchendo os seguintes atributos de OrgUnits e as funções:

- escalator<em>\<n\></em>

- owner<em>\<n\></em>

- securityOfficer<em>\<n\></em>

- notification<em>\<n\></em>

em que *\<n\>* indica um sufixo numérico opcional para fornecer vários atributos do mesmo tipo.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>Certifique-se de fluxos de trabalho de aprovação configurados no serviço do FIM

Instalação do BHOLD FIM integração cria conjuntos de definições de fluxo de trabalho e regras de política de gestão (MPRs) para o serviço FIM. Se tinha personalizado a implementação do FIM para alterar os conjuntos de administradores ou os conjuntos de utilizadores que podem fazer pedidos, deve certificar-se de que os MPRs referenciam os conjuntos de utilizador correto.

> [!NOTE]
> Antes dos utilizadores do Portal do FIM podem utilizar as funcionalidades de self-service de mensagens em fila fornecidas pelo BHOLD, as contas de utilizador têm de ser sincronizadas para a base de dados do BHOLD do serviço de sincronização do FIM. Em particular, tem de existir um registo de utilizador da base de dados do BHOLD Core e do banco de dados do serviço FIM para cada utilizador que pode fazer um pedido de self-service ou está especificado como um aprovador ou escalator para solicitações de autoatendimento.

## <a name="next-steps"></a>Passos Seguintes

- Para obter informações sobre como instalar o Portal do FIM e outros recursos do FIM, consulte [planejamento e arquitetura](https://technet.microsoft.com/library/ee808044.aspx) na biblioteca técnica do Microsoft Forefront.
- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
