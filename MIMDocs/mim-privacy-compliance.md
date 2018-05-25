---
title: Processamento de dados do Microsoft Identity Manager | Microsoft Docs
description: Compreender manuseamento facilite e relatórios sobre os dados dentro do ambiente, execute as ações de dados do Microsoft Identity Manager no fornecido com base nas funções operacionais e requisitos de sistema.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldiwn
ms.date: 05/22/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: 6bcf9ab26ba38f3c6eefbdb315d4975320a597b9
ms.sourcegitcommit: 66db63fe2813130764e52381f4f9c8e549d77d39
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/22/2018
---
# <a name="microsoft-identity-manager-data-handling"></a>Processamento de dados do Microsoft Identity Manager 

Este artigo fornecem orientações sobre como organização pode através da pesquisa, eliminar, decisões de operações Update e no relatório que organização terá praticar ou implementar através de várias origens de dados ligada. Antes de decidir sobre a abordagem de eliminar ou atualizar uma compreensão é fundamental a estrutura atual e a configuração do sistema identity manager (MIM). Seguem-se que alguns clientes de cenários terá de considerar e responder às seguintes questões: 

- Os dados que precisa para a gestão de identidades ajudar no processo empresarial?
- Onde dados atuais vai ser armazenado em MIM?
- Como irá utilizar estes dados no sistema?
- São partilhar estes dados com qualquer sources(Exporting) de dados de parceiros externos
- O que é a origem autoritativa para os dados e o processamento do mesmo?
- O que serão a retenção de dados e a eliminação de dados planear no local?
- Identificou todos os a tecnologia que precisa para processar e gerir dados?

Para ajudar a compreender um ambiente de MIM atual pode utilizar a seguinte ferramenta Documente o seu ambiente de MIM ou diferir para os documentos de estrutura de implementação.
- [Documentor de MIM - permite exportar a configuração atual](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Procurar e identificar os dados pessoais
Procurar os dados dentro de MIM será depende a configuração e o programa de configuração. A maioria dos ambientes estão interligados, mas para efeitos de clareza quebrou-los pelo componente de alto nível.

### <a name="synchronization-service"></a>Serviço de Sincronização

Todos os dados em MIM está relacionada com utilizadores é derivado do Active Directory (AD) e origens de dados de RH. Quando procurar dados pessoais, o primeiro local, deve considerar a pesquisa é AD ou ligado a origens de dados. 

Se não tiver a certeza de que a origem de autoridade pode controlar este utilizador a partir da consola do Gestor de serviço de sincronização de MIM, clique a barra de pesquisa de Metaverso para ver os dados pessoais identificáveis armazenados na base de dados. Os utilizadores podem procurar um utilizador específico ou um atributo.

- Para efetuar uma revisão ou a pesquisa de dados de objetos de utilizador
    - Abra o cliente do serviço de sincronização
        - Utilizando o metaverso designer permitem-lhe ver o fluxo de atributos importa e precedência.
![privacidade-de mim-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - Utilizando a pesquisa de metaverso permite-lhe procurar qualquer objeto e o atributo na base de dados ![de mim-privacidade-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
Depois de encontrar o objeto, clicando no objeto irá abrir a página de perfil de utilizador. Fornecem os detalhes do objeto, com os detalhes sobre o objeto, os respetivos atributos, abrangentes modificado pela última vez e a origem de autoridade e origem de dados ligada relacionados derivam do exemplo de configuração de agente de gestão abaixo.

![privacidade-conformidade de mim. PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Portal e serviço / PAM
Se tiver uma instância do serviço e Portal ou PAM instalado que está a ser é importante capaz de procurar utilizadores. 

Se tiver instalado o Portal, pode utilizar a IU para procurar em qualquer atributo ou a consulta para um determinado utilizador.

Se tiver apenas o servidor de serviço (sem a IU do Portal) instalado pode executar uma sintaxe de pesquisa com base nas [PSSnapin de FIMAutomation], localizado de exemplo [aqui](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx).

PAM pode utilizar a mesma sintaxe acima ou pode utilizar o [MIMPAM módulo](https://docs.microsoft.com/en-us/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) especificamente o cmdlet get-pamuser para procurar o utilizador no ambiente de PAM.

É outras opções de relatórios para procurar nos dados disponíveis no portal e serviço.
- [Criação de relatórios híbridos](https://docs.microsoft.com/en-us/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Relatórios com SCSM](https://docs.microsoft.com/en-us/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
Serviço de núcleo do Bhold tem uma IU que lhe permite pesquisar para um utilizador ou atributos. 

![pesquisa do bhold](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Se estiver a sincronizar BHOLD com [conector de gestão de acesso](https://docs.microsoft.com/en-us/microsoft-identity-manager/bhold/bhold-access-management-connector-install) para o serviço de sincronização será capaz de ver os objetos de utilizador ligada e os atributos a enviar para o principal do BHOLD.

Também pode carregar o módulo de relatórios do BHOLD.

- [Relatórios do BHOLD](https://docs.microsoft.com/en-us/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Gestão de Certificados
Pesquisa de serviço de gestão de certificados está incorporada na IU. O administrador irá iniciar e selecione a 'Localizar utilizador e ver ou gerir as respetivas informações'  

![pesquisa de cm](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Exportar os dados pessoais
Porque os dados relacionados com as entidades de MIM deriva de várias origens, a maioria dos dados é armazenada na base de dados do serviço de sincronização. Por este motivo, deve exportar dados relacionados com o objeto de sincronização de MIM ou pode determinar o proprietário dos dados.

### <a name="synchronization-service"></a>Serviço de Sincronização
Serviços de sincronização para exportar dados simplesmente selecione os dados a partir da IU de pesquisa e copie e colagem um csv ou o formato preferencial. Outra forma de exportar estes dados está a criar um MA baseados em ficheiros para remover o atuais dados necessários sobre um utilizador sinalizado de interesse. É possível encontrar um exemplo da utilização baseada em ficheiros MA [aqui](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/).


### <a name="service-and-portal--pam"></a>Portal e serviço / PAM
Encontrar de exemplo de serviço e portal, juntamente com o PAM, pode exportar estes dados, executados uma sintaxe de pesquisa com base nas [PSSnapin de FIMAutomation], [aqui](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) e encaminhá-la para [csv](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6).

PAM pode utilizar a mesma sintaxe acima ou pode utilizar o [MIMPAM módulo](https://docs.microsoft.com/en-us/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) especificamente o get-pamuser para procurar o utilizador no ambiente PAM e encaminhá-la a um csv.

- [Consultar o serviço MIM através do PowerShell de exemplo](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
Dados do Bhold podem ser exportados utilizando bhold módulo de relatórios para o formato de preferencial.

### <a name="certificate-management"></a>Gestão de Certificados
Dados de gestão de certificados relacionados com os dados pessoais estão ligados ao active directory. Um administrador pode exportar estes dados através do powershell do Active Directory.

## <a name="updating-personal-data"></a>Atualizar dados pessoais

Dados pessoais sobre os utilizadores ou os objetos em MIM soluções normalmente são derivados de objeto do utilizador em origens de dados da sua organização. Porque as alterações efetuadas ao perfil do utilizador na origem de RH ou outro sistema autoritativo de registo, como o AD, em seguida, são refletidas no serviço de sincronização de MIM.

### <a name="synchronization-service"></a>Serviço de Sincronização

Para efetuar operações de gestão, os administradores têm de fazer parte de operações de sincronização ou administrador definido [aqui](https://docs.microsoft.com/en-us/previous-versions/mim/jj590183(v%3dws.10)).

A atualização de dados, é necessário definir regras da origem de autoridade. Consola de gestão de ajuda a identificar a origem de autoridade para atualizá-lo na origem. Outra opção é criar regra de sincronização ou extensão de regra para controlar a atualização de dados se a origem, como dados de RH ainda tem de permanecer. Estas são as opções de avialible suportado.

Para obter mais informações sobre diferentes formas de atualizar o atributo, veja a seguir. 

- [Utilizar extensões de regras](https://msdn.microsoft.com/en-us/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Compreender a Sincronização de Dados com Sistemas Externos](https://docs.microsoft.com/en-us/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Portal e serviço / PAM

Portal e serviço para incluir dados de PAM podem ser atualizadas através dos cmdlets FIMAutomation ou PAM. Se tiver o Portal, pode também diretamente atualizar ao pesquisar e modificar o objecto. Um aspeto Note e consoante a configuração simplesmente a atualização do portal não significa permanecerá. Como origem de autoridade está altamente dependente de configuração geral.

### <a name="bhold"></a>BHOLD

Os utilizadores podem ser atualizados diretamente com a interface de utilizador do BHOLD Core ou o conector de gestão de acesso.

### <a name="certificate-management"></a>Gestão de Certificados

Os utilizadores no serviço de gestão de certificados são todos os um reflexão do Active Directory. Ao atualizar a utilização do Active Directory para alterar os detalhes do objecto.

## <a name="deleting-personal-data"></a>Eliminar dados pessoais

>[!Note] 
> Este artigo fornece orientação sobre formas de eliminar os dados pessoais do Microsoft Identity Manager e pode ser utilizado para suportar as obrigações sob o GDPR. Se estiver à procura de informações gerais sobre GDPR, consulte o [secção GDPR do portal do serviço de confiança](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

Dados em MIM são sincronizados e sempre atualizados a partir do respetivo origem de dados ligada. Quando é eliminado um objecto no destino, os dados do objeto em MIM podem ser mantidos para fins de investigação de segurança. Eliminação de objetos é configurada por regras de origem de dados ligada ou regra extension(code) e/ou as regras de eliminação de objeto.

### <a name="synchronization-service"></a>Serviço de Sincronização
Como muitas formas de processar dados ou elimine dados, consoante os processos de negócios do serviço de sincronização. Para ajudar a compreender, seguem-se alguns artigos para ajudar a compreender as opções eliminar e atualizar atributos: 

- [Compreender o Desaprovisionamento](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Utilizar extensões de regras](https://msdn.microsoft.com/en-us/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Melhores práticas de MIM](https://docs.microsoft.com/en-us/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Portal e serviço / PAM

Recomenda-se para o serviço e Portal que mantenha a configuração de retenção de recursos do sistema de 30 dias do predefinido. Isto indica o serviço, quando irá eliminar, não apenas os dados do pedido, mas também qualquer objeto que tem de ser limpa do sistema. Depois do processo ocorre, todos os dados associados a este objeto é eliminado Isto inclui todos os dados de registo SSPR. Este desempenha para a configuração de eliminação de objeto acima. Temos uma tabela foram armazenamos o guid de objetos. Para reduzir o tamanho geral da tabela na compilação 4.4.1459 adicionámos um processo denominado FIM_DeleteExpiredSystemObjectsJob detalhes sobre este processo pode ser encontrado [aqui](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden).

![de mim-privacidade-conformidade-srrc. PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

Bhold como a maioria dos sistemas ligados para o serviço de sincronização pode ser configurado para eliminar uma vez o objeto de origem, como HR é removido. Este é configurado no agente de gestão. e controladas pelas regras de eliminação de objetos, tal como descrito nas funcionalidades do serviço de sincronizações.

Outra opção consiste em remover o objeto de utilizador à direita da interface de utilizador do BHOLD Core. Dependendo da configuração Isto foi ajustar a funcionar mas tenha em atenção aprovisionamento lógica foi possível voltar a criar este utilizador se não eliminar na origem.
![de mim-privacidade-conformidade-bholdr. PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>Gestão de Certificados
Para remover um utilizador de CM, elimine o utilizador está no active directory.

Gestão de certificados que só irá armazenar o uid do perfil de serviços de certificados com sAMAccountName de domínio. Depois do utilizador for eliminado do AD a cache de utilizador existe apenas para os certificados, mudar tiverem inscrito. Não é recomendada a eliminar qualquer coisa na base de dados, isto pode provocar perigo geral para a operação do ambiente.

## <a name="opt-out-of-telemetry"></a>Ativamente de telemetria
Compilações anteriores FIM/MIM utilizado para recolhe telemetria anónimos sobre cada implementação e transmite dados através de HTTPS para servidores de Microsoft. Estes dados foi utilizados pela Microsoft para ajudar a melhorar versões futuras do FIM/MIM no passado.

>[!Note] 
> Em versões posteriores do 4.5.x.x ou dados de maiores coleção será desativada.

Desativar dados de coleção na versão anterior, execute Alterar modo e anular a seleção de linha de comandos da seguinte:

![de mim-privacidade-conformidade-ceip. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

ou editar o registo e defina o valor como 0: (componente) CEIP HKLM\SOFTWARE\Microsoft\Forefront identidade Manager\2010

![de mim-privacidade-conformidade-ceip2. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>Passos Seguintes 
- [Para orientações de privacidade SQL relacionados](https://docs.microsoft.com/en-us/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [Secção GDPR do portal do serviço de confiança](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [Arquivo do FIM 2010: Preparação - implementação do Forefront Identity Manager 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)