---
title: Instalação da integração do FIM/MIM com o BHOLD | Microsoft Docs
description: Módulo de integração do BHOLD adicionar gerenciamento de função de autoatendimento para MIM e FIM
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 317c9ae4c940a509b6ac328cd5bb7cd7baa4dde9
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "64516043"
---
# <a name="bhold-fimmim-integration-installation"></a>Instalação da integração do FIM/MIM com o BHOLD

O módulo integração de FIM com o BHOLD adiciona o gerenciamento de função de autoatendimento ao Microsoft Identity Manager, possibilitando que os usuários solicitem funções adicionais e imponham quem pode assumir essas funções. O módulo integração de FIM com o BHOLD estende o portal do FIM para facilitar o gerenciamento das funções dos usuários como parte da administração geral do FIM. Este tópico descreve como você deve configurar sua infraestrutura de rede para permitir que você instale e use o módulo integração de FIM com o BHOLD. Ele também aborda como instalar o módulo integração de FIM com o BHOLD e a configuração necessária depois de instalar o módulo integração de FIM com o BHOLD.

## <a name="bhold-fim-integration-installation-requirements"></a>Requisitos de instalação da integração do FIM com o BHOLD

O módulo integração de FIM com o BHOLD estende o portal do FIM e o serviço FIM para permitir que os usuários gerenciem suas funções no portal do FIM. Por esse motivo, é essencial que o módulo BHOLD Core e os recursos necessários do FIM sejam instalados e configurados antes de instalar o módulo integração de FIM com o BHOLD.
A seguir estão os componentes de software que devem estar presentes no computador antes que você possa instalar o módulo integração de FIM com o BHOLD:

- Portal e serviço do Microsoft Identity Manager 2016
- Microsoft Silverlight 3 ou posterior
- Serviços de informações da Internet e ASP.NET
- Ferramentas do Microsoft Silverlight

Além disso, os módulos BHOLD Core e conector de gerenciamento de acesso já devem estar implantados em um servidor no ambiente e o FIM deve ser configurado com um ou mais agentes de gerenciamento do BHOLD. Para obter informações sobre como instalar e configurar o módulo BHOLD Core, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). Para obter informações sobre como instalar e usar o módulo do conector de gerenciamento de acesso, consulte [instalação do conector de gerenciamento](https://technet.microsoft.com/library/jj874042(v=ws.10).aspx) de acesso e guia de laboratório de [teste: conector de gerenciamento de acesso do BHOLD](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

> [!IMPORTANT]
> O nome do banco de dados do serviço FIM deve ser FIMService. A instalação da integração do FIM com o BHOLD falhará se o FIM não tiver sido instalado com o nome do banco de dados do serviço do FIM padrão

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a instalar o módulo integração de FIM com o BHOLD, você deve criar um diretório BHOLD no diretório raiz da unidade de disco C: (C:\BHOLD).

Além disso, você precisa estar preparado para fornecer as informações que o assistente de configuração da integração do FIM do BHOLD requer para concluir a instalação. A planilha a seguir o ajudará a registrar essas informações para que você fique pronto para fornecê-las quando necessário.

### <a name="bholdfim-account-settings"></a>Configurações de conta do Bhold

| **Item**                            | **Descrição**                                                                                                                                                                                                               | **Valor**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Usar provedor de segurança no domínio** | Quando selecionado, especifica que Active Directory Domain Services segurança controlará o acesso ao BHOLD Core.                                                                                                                    | Selecione a caixa de verificação. **Importante:** A instalação falhará se essa caixa de seleção não estiver marcada.                                                                                                                                                                                                                   |
| **Domínio**                          | Especifica o domínio que contém a **conta de serviço** que você criou ao instalar o BHOLD Core. Para obter mais informações, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome somente se ele estiver incorreto. **Importante:** Especifique o nome de domínio usando o nome NetBIOS (curto), não o FQDN (nome de domínio totalmente qualificado). Por exemplo, se o FQDN do domínio for fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **Nome de Utilizador**                        | Especifica o nome de logon da conta de usuário do serviço do BHOLD Core.                                                                                                                                                              | Escreva o nome da conta de usuário aqui:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                        | Especifica a senha da conta de usuário do serviço.                                                                                                                                                                           | Escreva a senha aqui: **importante:** Lembre-se de manter essa senha em um local oculto e seguro.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>Configurações do serviço FIM

| **Item**            | **Descrição**                                                                                                                                                                                                                               | **Valor**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **User**            | Especifica o nome de logon de uma conta com privilégios de administrador para o FIM. A Microsoft recomenda enfaticamente que você não use a conta associada ao usuário raiz no BHOLD Core (por padrão, a conta usada para instalar o BHOLD Core). | Escreva o nome da conta de usuário aqui:                                                                   |
| **Palavra-passe**        | Especifica a senha da conta de usuário do administrador do FIM.                                                                                                                                                                                 | Escreva a senha aqui: **importante:** Lembre-se de manter essa senha em um local oculto e seguro. |
| **Banco de dados do FIM**    | Especifica o nome do banco de dados de serviço do FIM.                                                                                                                                                                                               | FIMService                                                                                          |
| **IP/porta do site** | Especifica o nome ou endereço IP do servidor do portal do FIM e a porta do site.                                                                                                                                                               | Grave o nome do servidor ou endereço e porta aqui:                                                     |

### <a name="bhold-core-connection"></a>Conexão do BHOLD Core

| **Item**               | **Descrição**                                                                                                                                                                                                                                                                                                                                                                               | **Valor**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Domínio**             | Especifica o nome do domínio da conta especificada no **usuário**, abaixo. Especifique o domínio no formato NetBIOS (curto).                                                                                                                                                                                                                                                                   | Gravar o nome de domínio da conta de usuário aqui?                                                            |
| **User**               | Especifica o nome de logon da conta de **um usuário BHOLD que é um supervisor** de todos os usuários e funções e tem permissão para vincular e desvincular funções de usuário. A Microsoft recomenda enfaticamente que você não use a conta associada ao usuário raiz no BHOLD Core (por padrão, a conta usada para instalar o BHOLD Core). Essa conta pode ser a mesma conta que você usa para se conectar ao FIM | Escreva o nome da conta de usuário aqui:                                                                   |
| **Palavra-passe**           | Especifica a senha da conta de usuário especificada no **usuário**.                                                                                                                                                                                                                                                                                                                             | Escreva a senha aqui: **importante:** Lembre-se de manter essa senha em um local oculto e seguro. |
| **Endereço IP/computador** | Especifica o endereço IP do servidor de site do BHOLD Core. Não use o nome do servidor.                                                                                                                                                                                                                                                                                                        | Grave o endereço IP aqui:                                                                          |
| **Número da porta**        | Especifica o número da porta do site do BHOLD Core.                                                                                                                                                                                                                                                                                                                                          | Escreva o número da porta aqui:                                                                         |

## <a name="bhold-fim-integration-setup"></a>Configuração da integração do FIM com o BHOLD

Para instalar o módulo integração de FIM com o BHOLD, faça logon como um membro do grupo Admins. do domínio, baixe o seguinte arquivo e execute-o como administrador no servidor no qual você pretende instalar o módulo integração de FIM com o BHOLD:

- BholdFIMIntegration<em>\<versão\></em>\_Release. msi

Substitua *\<versão\>* pelo número de versão da versão de integração do fim do BHOLD que você está instalando.

Para executar o arquivo de programa como administrador, clique com o botão direito do mouse no arquivo e clique em **Executar como administrador**.

![executando o MSI](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Tarefas de pós-instalação

Depois de instalar a integração do FIM com o BHOLD, você deve configurar o Microsoft SharePoint para fornecer as permissões de proprietário do site da conta de serviço do BHOLD. Além disso, se o portal do FIM estiver configurado para usar a segurança de protocolo SSL (SSL), você deverá modificar os arquivos que contêm referências aos endereços de páginas BHOLD adicionadas ao portal do FIM.

### <a name="configuring-sharepoint"></a>Configurando o SharePoint

Para funcionar corretamente, a integração do FIM com o BHOLD requer que a conta de serviço do BHOLD tenha permissões de membro de site no Microsoft SharePoint.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>Para conceder permissões de membro de site para a conta de serviço BHOLD

1.  Faça logon no servidor que executa a integração do BHOLD FIM com privilégios de administrador.

2.  Clique em **Iniciar**e em **Explorador da Internet**.

3.  Na barra de endereços, digite <https://localhost> se o SharePoint estiver configurado para usar a segurança SSL; caso contrário, digite <http://localhost>.

4.  No lado esquerdo da página do **site de equipe** , clique em **pessoas e grupos**.

5.  Em **grupos** , clique em **membros do site da equipe**e, na barra de ferramentas do painel central, clique em **novo**e em **Adicionar usuários**.

6.  Na página **Adicionar usuários: site de equipe** , em **usuários/grupos**, digite BHOLDApplicationGroup e clique no botão verificar nomes na caixa **usuários/grupos** . O nome do grupo deve ser resolvido para incluir o nome de domínio.

7.  Clique em **conceder permissões aos usuários diretamente**, selecione **controle total – tem controle total**e, em seguida, clique em **OK**.

8.  Verifique se BHOLDApplicationGroup está listado em **permissões: site da equipe**e feche o Internet Explorer.

![executando o MSI](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>Configurando o BHOLD para dar suporte a SSL

Se o portal do FIM estiver configurado para usar a segurança SSL, você deverá modificar os arquivos no servidor do FIM para que os links para páginas do BHOLD sejam abertos. Os arquivos estão localizados na seguinte pasta: ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```.

A tabela a seguir lista os arquivos e as versões originais e alteradas das cadeias de caracteres a serem editadas.

| **Ficheiro**                  | **Cadeia de caracteres original**                                                                                                                   | **Cadeia de caracteres alterada**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics. aspx            |   http://< BHOLD_Server >/bhold/Analytics/index_fim. html | https://< BHOLD_Server_FQDN >/bhold/Analytics/index_fim. html       |
| AttestationCampaigns. aspx |    http://< BHOLD_Server >/bhold/Attestation/Campaigns.aspx? hideMenu = 1 | https://< BHOLD_Server_FQDN >/bhold/Attestation/Campaigns.aspx? hideMenu = 1 | 
| AttestationMain. aspx      |  http://< BHOLD_Server >/bhold/Attestation/Dashboard.aspx? hideMenu = 1        | https://< BHOLD_Server_FQDN >/bhold/Attestation/Dashboard.aspx? hideMenu = 1 |
| Reporting. aspx            | http://< BHOLD_Server >/bhold/Reporting/index_fim. html |  https://< BHOLD_Server_FQDN >/bhold/Reporting/index_fim. html |
| Autoatendimento. aspx          | RoleExchangePoint = http://\<*FIM_Server*\>: \<*FIM_Port*\>/BHOLD/RoleExchangePoint/BHOLDRoleExchangePoint. svc, Transportmode = transporte | RoleExchangePoint = https://\<*FIM_Server_FQDN*\>: \<*FIM_SSL_Port\>* \>/BHOLD/RoleExchangePoint/BHOLDRoleExchangePoint. svc, Transportmode = transporte |

Em que:

-   *\<BHOLD_Server\>* especifica o nome do servidor BHOLD como encontrado na versão original do arquivo

-   *\<MIM_Server\>* especifica o nome do servidor do fim como encontrado na versão original do arquivo

-   *\<BHOLD_Server_FQDN\>* especifica o FQDN (nome de domínio totalmente qualificado) do servidor BHOLD

-   *\<MIM_Port\>* especifica o número da porta do servidor do fim, conforme encontrado na versão original do arquivo

-   *\<MIM_Server_FQDN\>* especifica o FQDN do servidor do fim

-   *\<MIM_SSL_Port\>* especifica uma porta diferente para uso com SSL no servidor do fim

### <a name="enable-approval-workflows-in-bhold-core"></a>Habilitar fluxos de trabalho de aprovação no BHOLD Core

Quando o FIM e o BHOLD são integrados para autoatendimento, os fluxos de trabalho para aprovações são executados no serviço FIM. Isso é semelhante ao modelo de fluxo de trabalho para solicitações originadas no portal do FIM, como quando um usuário envia uma solicitação para ingressar em uma lista de distribuição. No entanto, há diferenças importantes entre fluxos de trabalho de função BHOLD e outros fluxos de trabalho hospedados no serviço FIM. No caso de BHOLD, os parâmetros de fluxo de trabalho que especificam quais usuários devem aprovar uma origem de solicitação de função em BHOLD, em vez de serem armazenados nas definições de fluxo de trabalho no banco de dados de serviço do FIM. Esses parâmetros são fornecidos ao serviço FIM pelo BHOLD quando a primeira solicitação é feita, e um fluxo de trabalho de ação comunica os resultados de volta para o BHOLD Core.

BHOLD seleciona um Aprovador para uma solicitação de autoatendimento de uma das três maneiras:

-   **Gerenciador de linha como Aprovador: seleção baseada em função para uma unidade organizacional (OrgUnit)** Se uma função tiver um atributo chamado RoleType que está definido como Aprovador ou escalonamento, e se essa função estiver vinculada a um ou mais usuários no contexto de um OrgUnit, as solicitações de usuários nesse OrgUnit deverão ser aprovadas por um dos usuários que estão vinculados à função com o aprovador ou o redirecionador RoleType.

-   **Gerenciador de linha como Aprovador: seleção baseada em atributo para um OrgUnit** Cada OrgUnit pode ter um ou mais atributos que especificam os aliases de usuários que podem aprovar atribuições de função para outros usuários no OrgUnit. Esses atributos são nomeados approver1, approver2 e assim por diante. Quando um usuário no OrgUnit solicita uma atribuição de função, o BHOLD roteia a solicitação (até o FIM) para os usuários especificados pelos atributos do aprovador OrgUnit. Se um OrgUnit não tiver nenhum desses atributos definido, BHOLD verificará o pai OrgUnits até o OrgUnit raiz.

-   **Gerenciador de funções como Aprovador: seleção baseada em atributo para uma função** Uma função pode ter um ou mais atributos (também chamados de approver1 e assim por diante) que especificam os aliases de usuários que podem aprovar a atribuição da função. Quando um usuário solicita a atribuição de uma função que tem esses atributos de aprovador definidos, o BHOLD roteia a solicitação para os usuários especificados pelos atributos.

Se um Aprovador de uma solicitação de função de autoatendimento não for especificado por um desses métodos, por padrão, o BHOLD atribuirá automaticamente a função sem a necessidade de aprovação. Por esse motivo, imediatamente após a instalação da integração do BHOLD FIM, você deve configurar o OrgUnit raiz com o alias de um aprovador, como a conta raiz. Isso impedirá que um usuário receba uma função involuntariamente antes que uma política de aprovação mais abrangente possa ser implementada.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>Para configurar um Aprovador para o OrgUnit raiz

1.  Faça logon no servidor BHOLD Core como administrador.

2.  Clique em **Iniciar**e em **Internet Explorer**.

3.  Na barra de endereços do Internet Explorer, digite <http://localhost:5151/bhold/core>e pressione a tecla Enter.

4.  Na home page BHOLD Core, em DEF. de **atributo**, clique em **tipos de atributo**.

5.  Na página **tipo de atributo** , clique em **Adicionar**.

6.  Na **página Adicionar tipo de atributo**, em **identidade**, digite approver1, na lista **tipo de dados** , clique em **alfanumérico**, em **tamanho máximo**, digite 255, em **lista de valores**, clique em **não**, em **Inglês**, digite aprovador 1, clique em **OK**e, em seguida, clique em **concluído**.

7.  No painel esquerdo, em DEF. de **atributo** , clique em **conjuntos de tipos de atributo**.

8.  Na página **conjuntos de tipos de atributo** , clique em **Adicionar**.

9.  Na página **Adicionar conjunto de tipos de atributo** , em **Descrição**, digite atributos de OrgUnit e clique em **OK**.

10. Na página **atributos do OrgUnit** , expanda **tipos de atributo**e clique em **Modificar**.

11. Na lista **tipo de atributo** , clique **em approver1**, clique em **Adicionar**e, em seguida, clique em **concluído**.

12. No painel esquerdo, clique em **tipos de objeto**.

13. Na página **tipos de objeto** , clique em **OrgUnit**.

14. Na página **tipo de objeto/OrgUnit** , expanda **conjuntos de tipos de atributo**e clique em **Modificar**.

15. Na página **vincular tipo de atributo Set/OrgUnit** , em **ordem**, digite 10, na lista **conjunto de tipos de atributo** , clique em **atributos de OrgUnit**, clique em **Adicionar**e, em seguida, clique em **concluído**.

16. No painel esquerdo, em **modelo**, clique em **unidades organizacionais**.

17. Na página **unidades organizacionais** , clique em **raiz**.

18. Na página **unidade organizacional/raiz** , clique em **Modificar**.

19. Na página **modificar atributos da unidade organizacional/raiz** , em **Aprovador**, digite o domínio e o nome de usuário do usuário que aprovará as solicitações de atribuição de função, no formato *\<domínio\>* \\\<\>de *usuário* , em que *\<\>de domínio* é o nome de domínio NetBIOS (curto) e\< *\>de usuário* é o nome de logon do usuário.
20. Clique em **OK**.

> [!IMPORTANT]
> O domínio e o nome de usuário devem corresponder ao alias padrão de um usuário no banco de dados BHOLD Core.

Como alternativa para especificar um Aprovador para unidades organizacionais, você pode especificar um Aprovador para funções propostas no banco de dados BHOLD Core. Para fazer isso, crie o atributo approver1, adicione-o a um tipo de atributo especificado associado ao tipo de objeto Role e, em seguida, modifique cada função proposta para especificar o aprovador.

Para fornecer melhor segurança de fluxo de trabalho, além dos aprovadores, você deve designar modos de aprovação e usuários adicionais criando e preenchendo os seguintes atributos para OrgUnits e funções:

- escalonamento<em>\<n\></em>

- proprietário<em>\<n\></em>

- securityOfficer<em>\<n\></em>

- notificação<em>\<n\></em>

em que *\<n\>* indica um sufixo numérico opcional para fornecer vários atributos do mesmo tipo.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>Verificar os fluxos de trabalho de aprovação configurados no serviço FIM

A instalação da integração do FIM com o BHOLD cria conjuntos, definições de fluxo de trabalho e MPRs (regras de política de gerenciamento) para o serviço FIM. Se você personalizou sua implantação do FIM para alterar os conjuntos de administradores ou os conjuntos de usuários que podem fazer solicitações, certifique-se de que o MPRs faça referência aos conjuntos de usuários corretos.

> [!NOTE]
> Antes que os usuários do portal do FIM possam usar os recursos de autoatendimento fornecidos pelo BHOLD, as contas dos usuários devem ser sincronizadas no banco de dados BHOLD do serviço de sincronização do FIM. Em particular, deve haver um registro de usuário no banco de dados do BHOLD Core e no banco de dados de serviço do FIM para cada usuário que pode fazer uma solicitação de autoatendimento ou é especificado como um Aprovador ou um encaminhador para solicitações de autoatendimento.

## <a name="next-steps"></a>Próximos passos

- Para obter informações sobre como instalar o portal do FIM e outros recursos do FIM, consulte [planejamento e arquitetura](https://technet.microsoft.com/library/ee808044.aspx) na biblioteca técnica do Microsoft Forefront.
- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
