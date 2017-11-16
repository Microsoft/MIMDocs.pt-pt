---
title: "Instalação de integração do BHOLD FIM/MIM | Microsoft Docs"
description: "Módulo de integração do BHOLD adicionar gestão de funções de self-service para o MIM e de FIM"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: ef68de19bd0eabd6d9203469ecc991d496f05846
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2017
---
# <a name="bhold-fimmim-integration-installation"></a>Instalação do BHOLD FIM/MIM integração

O módulo de integração do BHOLD FIM adiciona a gestão de funções de self-service para Microsof Identity Manager, tornando a possível para que os utilizadores peçam funções adicionais e impor que pode demorar em dessas funções. O módulo de integração do BHOLD FIM expande o Portal do FIM torna mais fácil de gerir as funções dos utilizadores como parte da administração de FIM geral. Este tópico descreve como tem de configurar a infraestrutura de rede para que possa instalar e utilizar o módulo de integração do BHOLD FIM. Também abrange como instalar o módulo de integração do BHOLD FIM e a configuração que é necessária depois de instalar o módulo de integração do BHOLD FIM.

## <a name="bhold-fim-integration-installation-requirements"></a>Requisitos de instalação de integração do BHOLD FIM

O módulo de integração do BHOLD FIM expande o FIM Portal e serviço FIM para permitir aos utilizadores para gerir as respetivas funções dentro do Portal do FIM. Por este motivo, é essencial que o módulo de núcleo do BHOLD e as funcionalidades necessárias do FIM ser instaladas e configuradas antes de instalar o módulo de integração do BHOLD FIM.
Seguem-se os componentes de software que tem de estar presentes no computador antes de poder instalar o módulo de integração do BHOLD FIM:

- Portal do Microsoft Identity Manager 2016 e o serviço
- Microsoft Silverlight 3 ou posterior
- Serviços de informação Internet e ASP.NET
- Ferramentas do Microsoft Silverlight

Além disso, os módulos do núcleo do BHOLD e conector de gestão de acesso já tem de ser implementados num servidor no ambiente e FIM tem de ser configurado com um ou mais agentes de gestão do BHOLD. Para obter informações sobre como instalar e configurar o módulo do BHOLD Core, consulte [BHOLD Core instalação](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). Para obter informações sobre como instalar e utilizar o módulo de conector de gestão de acesso, consulte [instalação do conector de gestão de acesso](https://technet.microsoft.com/en-us/library/jj874042(v=ws.10).aspx) e [Guia do laboratório de teste: conector de gestão de acesso do BHOLD](https://technet.microsoft.com/en-us/library/jj853085(v=ws.10).aspx).

>[!IMPORTANT]
O nome da base de dados do serviço FIM tem de ser fimservice reinício. Configuração de integração do BHOLD FIM falhará FIM não tenha sido instalado com o nome predefinido de base de dados para o serviço do FIM.

## <a name="before-you-begin"></a>Antes de começar

Antes de começar a instalar o módulo de integração do BHOLD FIM, tem de criar um diretório do BHOLD no diretório de raiz da unidade de disco c: (C:\BHOLD).

Além disso, tem de estar preparado para fornecer as informações que o Assistente de configuração de integração do BHOLD FIM necessita para concluir a instalação. A folha de cálculo seguinte irão ajudá-lo, registe essa informação para estará pronto para fornecê-lo quando for necessário.

### <a name="bholdfim-account-settings"></a>Definições da conta de BHOLDFim

| **Item**                            | **Descrição**                                                                                                                                                                                                               | **Valor**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilize um fornecedor de segurança no domínio** | Quando selecionada, especifica que a segurança dos serviços de domínio do Active Directory controlará o acesso para BHOLD núcleo.                                                                                                                    | Selecione a caixa de verificação. **Importante:** a instalação falhará se esta caixa de verificação não estiver selecionada.                                                                                                                                                                                                                   |
| **Domínio**                          | Especifica o domínio que contém o **conta de serviço** que criou ao instalar BHOLD Core. Para obter mais informações, consulte [BHOLD Core instalação](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx). | O nome de domínio é fornecido automaticamente pelo assistente. Altere o nome apenas se estiver incorreto. **Importante:** especifique o nome de domínio com o nome de NetBIOS (abreviado), não o nome de domínio completamente qualificado (FQDN). Por exemplo, se o FQDN do domínio for fabrikam.com, especifique o nome de domínio como FABRIKAM. |
| **Nome de Utilizador**                        | Especifica o nome de início de sessão da conta de utilizador do serviço de núcleo do BHOLD.                                                                                                                                                              | Escreva o nome da conta de utilizador aqui:                                                                                                                                                                                                                                                                                    |
| **Palavra-passe**                        | Especifica a palavra-passe da conta de utilizador de serviço.                                                                                                                                                                           | Escrever a palavra-passe aqui: **importante:** não se esqueça de manter esta palavra-passe numa localização segura, oculta.                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>Definições de serviço do FIM

| **Item**            | **Descrição**                                                                                                                                                                                                                               | **Valor**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **User**            | Especifica o nome de início de sessão de uma conta com privilégios de administrador para FIM. A Microsoft recomenda vivamente que não utilize a conta associada ao utilizador raiz no BHOLD Core (por predefinição, a conta utilizada para instalar o núcleo do BHOLD). | Escreva o nome da conta de utilizador aqui:                                                                   |
| **Palavra-passe**        | Especifica a palavra-passe da conta de utilizador de administrador FIM.                                                                                                                                                                                 | Escrever a palavra-passe aqui: **importante:** não se esqueça de manter esta palavra-passe numa localização segura, oculta. |
| **Base de dados do FIM**    | Especifica o nome da base de dados do serviço FIM.                                                                                                                                                                                               | Fimservice reinício                                                                                          |
| **IP/porta do Web site** | Especifica o nome ou endereço IP do servidor do Portal do FIM e a porta do Web site.                                                                                                                                                               | Escreva o nome do servidor ou o endereço e a porta aqui:                                                     |

### <a name="bhold-core-connection"></a>Ligação de núcleo do BHOLD

| **Item**               | **Descrição**                                                                                                                                                                                                                                                                                                                                                                               | **Valor**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **Domínio**             | Especifica o nome do domínio da conta especificada no **utilizador**, abaixo. Especifique o domínio no formato de NetBIOS (abreviado).                                                                                                                                                                                                                                                                   | Escrever o nome de domínio de conta de utilizador aqui?                                                            |
| **User**               | Especifica o nome de início de sessão da conta de **um utilizador BHOLD que seja um supervisor** de todos os utilizadores e funções e se tem permissões para a ligação e desassociar funções de utilizador. A Microsoft recomenda vivamente que não utilize a conta associada ao utilizador raiz no BHOLD Core (por predefinição, a conta utilizada para instalar o núcleo do BHOLD). Esta conta pode ser a mesma conta que utilizar para ligar ao FIM | Escreva o nome da conta de utilizador aqui:                                                                   |
| **Palavra-passe**           | Especifica a palavra-passe da conta de utilizador especificada no **utilizador**.                                                                                                                                                                                                                                                                                                                             | Escrever a palavra-passe aqui: **importante:** não se esqueça de manter esta palavra-passe numa localização segura, oculta. |
| **Endereço IP/máquina** | Especifica o endereço IP do servidor do Web site do BHOLD Core. Não utilize o nome do servidor.                                                                                                                                                                                                                                                                                                        | Escreva o endereço IP aqui:                                                                          |
| **Número de porta**        | Especifica o número de porta do Web site BHOLD Core.                                                                                                                                                                                                                                                                                                                                          | Escreva o número de porta aqui:                                                                         |

## <a name="bhold-fim-integration-setup"></a>Configuração de integração do BHOLD FIM

Para instalar o módulo de integração do BHOLD FIM, inicie sessão como membro do grupo Admins do domínio, transfira o ficheiro seguinte e executá-la como administrador no servidor que tenciona instalar o módulo de integração do BHOLD FIM em:

- BholdFIMIntegration*\<versão\>*\_Release.msi

Substitua  *\<versão\>*  com o número de versão da versão do BHOLD FIM integração que está a instalar.

Para executar o ficheiro de programa como administrador, clique com o botão direito do ficheiro e, em seguida, clique em **executar como administrador**.

![execução msi](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>Tarefas de pós-instalação

Depois de instalar a integração do BHOLD FIM, tem de configurar o Microsoft SharePoint para conceder as permissões de proprietário do site de conta de serviço do BHOLD. Além disso, se o Portal do FIM está configurado para utilizar a segurança de Secure Sockets Layer (SSL), tem de modificar os ficheiros que contêm referências a endereços de páginas BHOLD adicionados ao Portal do FIM.

### <a name="configuring-sharepoint"></a>Configurar o SharePoint

Para funcionar corretamente, integração do BHOLD FIM requer que a conta de serviço do BHOLD para ter permissões de membro de site Microsoft SharePoint.

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>Para conceder permissões de membro de site para a conta de serviço do BHOLD

1.  Inicie sessão no servidor que executa a integração do BHOLD FIM com privilégios de administrador.

2.  Clique em **iniciar**e, em seguida, clique em **Internet Exporer**.

3.  Na barra de endereço, escreva <https://localhost> se SharePoint estiver configurado para utilizar a segurança SSL, caso contrário, escreva <http://localhost>.

4.  No lado esquerdo do **Site de equipa** página, clique em **pessoas e grupos**.

5.  Em **grupos** clique **os membros da equipa Site**e na barra de ferramentas painel central, clique em **novo**e, em seguida, clique em **adicionar utilizadores**.

6.  No **adicionar utilizadores: Site de equipa** na página **utilizadores/grupos**, escreva BHOLDApplicationGroup e, em seguida, clique no botão Verificar nomes sob o **utilizadores/grupos** caixa. O nome do grupo deve resolver para incluir o nome de domínio.

7.  Clique em **dar permissões de utilizadores diretamente**, selecione **controlo total – tem controlo total**e, em seguida, clique em **OK**.

8.  Certifique-se de que BHOLDApplicationGroup está listado no **permissões: Site de equipa**e, em seguida, feche o Internet Explorer.

![execução msi](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>Configurar BHOLD para suportar o SSL

Se o Portal do FIM está configurado para utilizar a segurança SSL, tem de modificar ficheiros no servidor do FIM, para que as ligações para páginas do BHOLD irão abrir. Os ficheiros estão localizados na seguinte pasta: ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD```.

A tabela seguinte lista os ficheiros e as versões originais e as alterações das cadeias para ser editado.

| **Ficheiro**                  | **Cadeia original**                                                                                                                   | **Cadeia foi alterada**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics.aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.HTML | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.HTML       |
| AttestationCampaigns.aspx |    http://<BHOLD_Server>/bhold/attestation/Campaigns.aspx?hideMenu=1 | https://<BHOLD_Server_FQDN>/bhold/attestation/Campaigns.aspx?hideMenu=1 | 
| AttestationMain.aspx      |  http://<BHOLD_Server>/bhold/attestation/dashboard.aspx?hideMenu=1        | https://<BHOLD_Server_FQDN>/bhold/attestation/dashboard.aspx?hideMenu=1 |
| Reporting.aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.HTML |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.HTML |
| Selfservice.aspx          | RoleExchangePoint = http: / /\<*FIM_Server*\>: \< *FIM_Port*\>/BHOLD/RoleExchangePoint/BHOLDRoleExchangePoint.svc, TransportMode = transporte | RoleExchangePoint = https: / /\<*FIM_Server_FQDN*\>: \< *FIM_SSL_Port\>*\>/BHOLD/RoleExchangePoint / BHOLDRoleExchangePoint.svc,TransportMode=Transport |

Em que:

-   *\<BHOLD_Server\>*  Especifica o nome do servidor do BHOLD como encontrada na versão original do ficheiro

-   *\<MIM_Server\>*  Especifica o nome do servidor do FIM como encontrada na versão original do ficheiro

-   *\<BHOLD_Server_FQDN\>*  Especifica o nome de domínio completamente qualificado (FQDN) do servidor do BHOLD

-   *\<MIM_Port\>*  Especifica o número de porta do servidor do FIM como encontrada na versão original do ficheiro

-   *\<MIM_Server_FQDN\>*  Especifica o FQDN do servidor do FIM

-   *\<MIM_SSL_Port\>*  Especifica uma porta diferente para utilização com SSL no servidor do FIM

### <a name="enable-approval-workflows-in-bhold-core"></a>Ativar fluxos de trabalho de aprovação no núcleo do BHOLD

Quando o FIM e BHOLD estão integrados para Self-Service, os fluxos de trabalho para aprovações são executados no serviço FIM. Isto é semelhante ao modelo de fluxo de trabalho para pedidos que têm origem no Portal do FIM, por exemplo, quando um utilizador submete um pedido para associar uma lista de distribuição. Existem principais diferenças entre fluxos de trabalho do BHOLD função e outros fluxos de trabalho alojados no serviço FIM, no entanto. No caso do BHOLD, parâmetros de fluxo de trabalho que especifica quais os utilizadores têm de aprovar um pedido de função têm origem na BHOLD, em vez de ser armazenados nas definições de fluxo de trabalho na base de dados do serviço FIM. Estes parâmetros são fornecidos para o serviço FIM pelo BHOLD quando o primeiro pedido é efetuado, e um fluxo de trabalho da ação comunica os resultados novamente para BHOLD Core.

BHOLD seleciona um aprovador para um pedido de self-service dos três formas:

-   **Gestor de linha como aprovador: seleção baseada em funções para uma unidade organizacional (OrgUnit)** se uma função tem um atributo com o nome roletype está definida como aprovador ou Escalator e se essa função estiver associada a um ou mais utilizadores no contexto de um OrgUnit, pedidos de os utilizadores dentro desse OrgUnit devem ser aprovados por um dos utilizadores que está ligada à função com o aprovador ou Escalator roletype.

-   **Gestor de linha como aprovador: baseadas em atributos de seleção para um OrgUnit** OrgUnit cada pode ter um ou mais atributos que especificam os aliases de utilizadores que podem aprovar atribuições de funções para outros utilizadores no OrgUnit. Estes atributos são denominados approver1, approver2 e assim sucessivamente. Quando um utilizador a OrgUnit solicita uma atribuição de função, BHOLD encaminha o pedido (FIM) para os utilizadores especificados pelos atributos de aprovador OrgUnit. Se um OrgUnit não tem este conjunto de atributos, BHOLD verifica principal OrgUnits até OrgUnit raiz.

-   **Gestor de funções como aprovador: baseadas em atributos de seleção para uma função** uma função pode ter um ou mais atributos (também denominado approver1 e assim sucessivamente) que especificam os aliases de utilizadores que podem aprovar a atribuição da função. Quando um utilizador solicita a ser atribuída uma função que tenha estes atributos aprovador definida, BHOLD encaminha o pedido para os utilizadores especificados pelos atributos.

Se um aprovador para um pedido de função personalizada não for especificado por um destes métodos, por predefinição BHOLD atribui automaticamente a função sem necessidade de aprovação. Por este motivo, imediatamente depois de instalar a integração do BHOLD FIM, deve configurar a raiz OrgUnit com o alias de um aprovador, tais como a conta raiz. Isto irá impedir que um utilizador de inadvertidamente ser concedida uma função antes de uma política de aprovação mais abrangente que pode ser implementada.

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>Para configurar um aprovador para a raiz OrgUnit

1.  Inicie sessão no servidor principal do BHOLD como administrador.

2.  Clique em **iniciar**e, em seguida, clique em **Internet Explorer**.

3.  Na barra de endereço do Internet Explorer, escreva <bhold/http://localhost:5151/core>e, em seguida, prima a tecla Enter.

4.  Do núcleo do BHOLD página inicial, em **atributo def**, clique em **tipos de atributo**.

5.  No **tipo de atributo** página, clique em **adicionar**.

6.  No **página de tipo de atributo adicionar**, na **identidade**, escreva approver1, o **tipo de dados** lista, clique em **alfanumérica**, no **Comprimento de máximo**, escreva 255, **lista de valores**, clique em **não**, na **inglês**, escreva o aprovador 1, clique em **OK**e, em seguida, clique em **feito**.

7.  No painel esquerdo, em **atributo def** clique **conjuntos de tipo de atributo**.

8.  No **conjuntos de tipo de atributo** página, clique em **adicionar**.

9.  No **adicionar atributo tipo definido** na página **Descrição**, escreva OrgUnit atributos e, em seguida, clique em **OK**.

10. No **OrgUnit atributos** página, expanda **tipos de atributo**e, em seguida, clique em **modificar**.

11. No **tipo de atributo** lista, clique em **approver1**, clique em **adicionar**e, em seguida, clique em **feito**.

12. No painel esquerdo, clique em **tipos de objeto**.

13. No **tipos de objeto** página, clique em **OrgUnit**.

14. No **OrgUnit/tipo de objeto** página, expanda **conjuntos de tipo de atributo**e, em seguida, clique em **modificar**.

15. No **associar conjunto de tipo de atributo/OrgUnit** na página **ordem**, escreva 10, o **o atributo tipo definido** lista, clique em **OrgUnit atributos**, Clique em **adicionar**e, em seguida, clique em **feito**.

16. No painel esquerdo, em **modelo**, clique em **unidades organizacionais**.

17. No **unidades organizacionais** página, clique em **raiz**.

18. No **raiz/unidade organizacional** página, clique em **modificar**.

19. No **modificar os atributos/raiz da unidade organizacional** na página **aprovador**, escreva o nome de utilizador e domínio do utilizador que irá aprovar pedidos de atribuição de função, no formato  *\<domínio\>*\\*\<utilizador\>*, onde  *\<domínio\>*  é o Nome de domínio NetBIOS (abreviado) e  *\<utilizador\>*  é o nome de início de sessão do utilizador.
20. Clique em **OK**.

>[!IMPORTANT]
O nome de domínio e o utilizador tem de corresponder ao alias predefinido de um utilizador na base de dados do BHOLD Core.

Como alternativa a especificar um aprovador para unidades organizacionais, pode especificar um aprovador para funções propostas na base de dados do BHOLD Core. Para tal, criar o atributo approver1, adicione-o para um atributo typeset associadas com o tipo de objeto de função e, em seguida, modifique cada função proposta para especificar o aprovador.

Para fornecer uma melhor segurança de fluxo de trabalho, para além dos aprovadores, deverá designar modos de aprovação adicionais e os utilizadores através da criação e preencher os seguintes atributos de OrgUnits e as funções:

- escalator*\<n\>*

- proprietário*\<n\>*

- securityOfficer*\<n\>*

- notificação*\<n\>*

onde  *\< n \>*  indica um sufixo numérico opcional para fornecer vários atributos do mesmo tipo.

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>Certifique-se de fluxos de trabalho de aprovação configurados no serviço do FIM

Instalação de integração do BHOLD FIM cria conjuntos, definições de fluxo de trabalho e regras de política de gestão (MPRs) para o serviço FIM. Se tinha personalizado a implementação de FIM para alterar os conjuntos de administradores ou os conjuntos de utilizadores que podem fazer pedidos, deve certificar-se de que as MPRs referenciam os conjuntos de utilizador correto.

>[!NOTE]
Antes dos utilizadores do FIM Portal podem utilizar as funcionalidades self-service fornecidas pelo BHOLD, as contas têm de ser sincronizadas para a base de dados do BHOLD do serviço de sincronização do FIM. Em particular, tem de existir um registo de utilizador na base de dados do BHOLD núcleos e na base de dados do serviço FIM para cada utilizador que pode efetuar um pedido de self-service ou como um aprovador ou escalator para pedidos de self-service.

## <a name="next-steps"></a>Próximos passos

- Para obter informações sobre como instalar o Portal do FIM e outras funcionalidades do FIM, consulte [planeamento e arquitetura](https://technet.microsoft.com/library/ee808044.aspx) na biblioteca técnica do Microsoft Forefront.
- [Guia de instalação do BHOLD](bhold-installation-guide.md)
- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
