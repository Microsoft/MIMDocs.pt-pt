---
title: Manipulação de dados do Microsoft Identity Manager | Documentos da Microsoft
description: Compreender os dados do Microsoft Identity Manager manipulação facilite e reportar dados dentro do ambiente, agir na atribuídas com base em funções operacionais e o requisito de sistema.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldiwn
ms.date: 05/22/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: 4cb17b6e30a59c8acb3871e694ed4df72538deab
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49334045"
---
# <a name="microsoft-identity-manager-data-handling"></a>Manipulação de dados do Microsoft Identity Manager 

Neste artigo fornecem orientações sobre como a organização pode através da pesquisa, delete, decisões de operações de atualização e relatório que sua empresa precisará de praticar ou implementar em várias origens de dados ligada. Antes de decidir sobre sua abordagem de eliminar ou atualizar uma compreensão o design atual e a configuração do sistema identity manager (MIM) é fundamental. Seguem-se que alguns cenários clientes precisam considerar e responda às seguintes perguntas: 

- Os dados que precisa para identity management ajudar no processo de negócio?
- Onde será dados atuais para ser armazenado em MIM?
- Como irá utilizar estes dados no sistema?
- Partilham estes dados com qualquer sources(Exporting) de dados de parceiros externos
- O que é a fonte autoritativa para os dados e o processamento do mesmo?
- O que serão a retenção de dados e a eliminação de dados planear no local?
- Identificou-se de toda a tecnologia que precisa para processar e gerir os dados?

Para ajudar a compreender um ambiente de MIM atual, pode utilizar a seguinte ferramenta para documentar seu ambiente de MIM ou diferir aos seus documentos de design de implementação.
- [Documentor de MIM - permite exportar a configuração atual](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>A procurar e identificar os dados pessoais
Pesquisar dados dentro de MIM será dependente da configuração e a configuração. A maioria dos ambientes estão interligados, mas por motivos de clareza, decompôs-los pelo componente de alto nível.

### <a name="synchronization-service"></a>Serviço de Sincronização

Todos os dados em MIM que está relacionado aos utilizadores é obtida a partir do Active Directory (AD) e origens de dados de RH. Ao pesquisar os dados pessoais, o primeiro lugar, deve considerar a pesquisa é AD ou ligado a origens de dados. 

Se não-se de que a origem de autoridade pode monitorizar este utilizador a partir da consola de Gestor de serviço de sincronização de MIM, clique na barra de pesquisa de Metaverso para ver os dados pessoais identificáveis, que são armazenados na base de dados. Os utilizadores podem pesquisar um usuário específico ou um atributo.

- Para efetuar uma revisão ou a pesquisa de dados de objetos de utilizador
    - Abra o cliente do serviço de sincronização
        - Usando o metaverse designer permite-lhe ver o fluxo de atributos importa e precedência.
![mim-privacidade-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - Utilizando a pesquisa de metaverso permite que faça pesquisas em qualquer objeto e um atributo na base de dados ![mim-privacidade-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
Depois de encontrar o objeto, clicar no objeto abrirá a página de perfil do usuário. Os detalhes dos objetos fornecem a com os detalhes abrangentes sobre o objeto, seus atributos, modificado pela última vez e a origem de autoridade e de origem de dados ligada relacionados derivam do exemplo de configuração de agente de gestão abaixo.

![privacidade-conformidade de mim. PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Portal e serviço / PAM
Se tiver uma instância do serviço e Portal ou de PAM instalado que está a ser é importante conseguir procurar utilizadores. 

Se tiver instalado o Portal, pode utilizar a interface do Usuário para pesquisar em qualquer atributo ou a consulta para um usuário específico.

Se tiver apenas o servidor de serviço (sem a IU do Portal) instalado pode executar uma sintaxe de pesquisa com base em [PSSnapin de FIMAutomation], o exemplo encontrado [aqui](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx).

PAM pode utilizar a mesma sintaxe acima ou pode utilizar o [MIMPAM módulo](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) especificamente o cmdlet get-pamuser para procurar o utilizador dentro do ambiente de PAM.

Outras opções de relatórios para pesquisar nos dados disponíveis é no serviço e portal.
- [Criação de relatórios híbridos](https://docs.microsoft.com/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Relatórios com o SCSM](https://docs.microsoft.com/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
O serviço do Bhold Core tem uma interface do Usuário que permite-lhe pesquisar para um utilizador ou atributos. 

![pesquisa do bhold](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Se estiver a sincronizar BHOLD com [conector de gestão de acesso](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-access-management-connector-install) para o serviço de sincronização, poderá ver os objetos do usuário conectado e os atributos seu envio para o núcleo do BHOLD.

Também é possível carregar o módulo do BHOLD Reporting.

- [BHOLD Reporting](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Gestão de Certificados
Pesquisa de serviço de gestão de certificados está incorporada na interface do Usuário. O administrador irá iniciar e selecione a 'Localizar utilizador e a vista ou gerenciem suas informações"  

![pesquisa de cm](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Exportar os dados pessoais
Uma vez que os dados relacionados com a entidades em MIM são derivados de várias origens, maior parte dos dados são armazenados na base de dados do serviço de sincronização. Por esse motivo, deve exportar dados relacionados com o objeto de sincronização de MIM ou pode determinar o proprietário dos dados.

### <a name="synchronization-service"></a>Serviço de Sincronização
Serviços de sincronização para exportar dados simplesmente selecionar os dados da pesquisa da interface do Usuário e a cópia e cole um csv ou formato preferencial. Outra forma de exportar estes dados é criar uma MA baseados em ficheiros para soltar dados atuais necessários sobre um utilizador sinalizado de interesse. Pode encontrar um exemplo da utilização baseada em ficheiros MA [aqui](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/).


### <a name="service-and-portal--pam"></a>Portal e serviço / PAM
Serviço e portal, juntamente com o PAM, pode exportar estes dados, executados uma sintaxe de pesquisa com base em [PSSnapin de FIMAutomation], o exemplo encontradas [aqui](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) e encaminhá-la para [csv](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6).

PAM pode utilizar a mesma sintaxe acima ou pode utilizar o [MIMPAM módulo](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) especificamente o get-pamuser para procurar o utilizador no ambiente do PAM e encaminhá-la a um csv.

- [Consultar o serviço MIM com o PowerShell de exemplo](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
Dados do Bhold podem ser exportados usando o bhold reporting módulo para seu formato preferido.

### <a name="certificate-management"></a>Gestão de Certificados
Dados de gestão de certificados relacionados com dados pessoais estão ligados ao active directory. Um administrador pode exportar estes dados através do powershell do Active Directory.

## <a name="updating-personal-data"></a>A atualizar os dados pessoais

Normalmente, os dados pessoais sobre os utilizadores ou objetos em soluções de MIM são derivados do objeto do usuário em origens de dados ligados da sua organização. Uma vez que todas as alterações efetuadas ao perfil de usuário na origem de RH, ou de outro sistema autoritativo de registo, como o AD, em seguida, são refletidas no serviço de sincronização de MIM.

### <a name="synchronization-service"></a>Serviço de Sincronização

Para realizar operações de gestão, os administradores têm de fazer parte de operações de sincronização ou definida pelo administrador [aqui](https://docs.microsoft.com/previous-versions/mim/jj590183(v%3dws.10)).

A atualização de dados é feita através da definição de regras da origem de autoridade. Console de gerenciamento ajuda a identificar a origem de autoridade para atualizá-lo na origem. Outra opção é criar a regra de sincronização ou extensão de regra para controlar a atualização de dados se a origem, como dados de RH ainda precisa de permanecer. Estas são as opções de avialible suportado.

Para obter mais informações sobre as diferentes formas de atualizar atributo, veja a seguir. 

- [Utilizar extensões de regras](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Compreender a Sincronização de Dados com Sistemas Externos](https://docs.microsoft.com/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Portal e serviço / PAM

Portal e serviço para incluir dados de PAM podem ser atualizados com os cmdlets FIMAutomation ou de PAM. Se tiver o Portal, pode também diretamente atualizar ao pesquisar e modificar o objeto. Uma coisa para nota e dependendo da configuração de atualizar a partir do portal não significa que ele permanecerá. Como a origem de autoridade é altamente dependente de configuração geral.

### <a name="bhold"></a>BHOLD

Os utilizadores podem ser atualizados diretamente com a interface de utilizador do BHOLD Core ou o conector de gestão de acesso.

### <a name="certificate-management"></a>Gestão de Certificados

Os utilizadores no serviço de gestão de certificado são todos os um reflexo do Active Directory. Para atualizar o uso do Active Directory para alterar os detalhes do objeto.

## <a name="deleting-personal-data"></a>A eliminar os dados pessoais

>[!Note] 
> Este artigo fornece orientações sobre as formas de eliminar os dados pessoais do Microsoft Identity Manager e pode ser utilizado para suportar as suas obrigações sob o GDPR. Se quiser obter informações gerais sobre o RGPD, veja a [secção RGPD do Portal de Confiança do Serviço](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

Dados em MIM são sincronizados e sempre atualizados a partir da sua origem de dados ligada. Quando um objeto é eliminado no destino, os dados do objeto em MIM podem ser mantidos para efeitos de investigação de segurança. Eliminação de objetos está configurada por regras de origem de dados ligada ou regra extension(code) e/ou as regras de eliminação do objeto.

### <a name="synchronization-service"></a>Serviço de Sincronização
Como várias formas de lidar com dados ou eliminar dados consoante os processos de negócio do serviço de sincronização. Para ajudar a compreender, seguem-se alguns artigos para ajudar a compreender as opções de eliminar e atualizar atributos: 

- [Compreender o Desaprovisionamento](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Utilizar extensões de regras](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Práticas recomendadas de MIM](https://docs.microsoft.com/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Portal e serviço / PAM

Recomenda-se para o serviço e Portal que mantenha a configuração de retenção de recursos do sistema de 30 dias do padrão. Isso indica ao serviço, quando irá eliminar, não apenas dados de pedido, mas também qualquer objeto que tem de ser limpos do sistema. Depois do processo ocorre, todos os dados associados a este objeto é eliminado Isto inclui todos os dados de registo SSPR. Ele exerce para a configuração da eliminação do objeto acima. Temos uma tabela foram armazenamos o guid dos objetos. Para reduzir o tamanho geral da tabela na compilação 4.4.1459 adicionámos um processo chamado FIM_DeleteExpiredSystemObjectsJob detalhes sobre este processo pode ser encontrado [aqui](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden).

![mim-privacidade-conformidade-srrc. PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

Bhold como a maioria dos sistemas ligados para o serviço de sincronização pode ser configurado para excluir uma vez o objeto de origem, como RH é removido. Este é configurado no agente de gestão. e o controlado pelas regras de eliminação de objetos, conforme descrito em funcionalidades do serviço de sincronizações.

Outra opção é remover o objeto de utilizador diretamente a partir da interface do usuário do BHOLD Core. Dependendo da configuração isso poderia funcionar bem mas tenha em atenção a lógica de aprovisionamento pode voltar a criar este utilizador se não for eliminado na origem.
![mim-privacidade-conformidade-bholdr. PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>Gestão de Certificados
Para remover um utilizador do CM, eliminar o utilizador estiver no active directory.

Gestão de certificados como ele armazenará apenas o uid de perfil de serviços de certificados com o sAMAccountName de domínio. Assim que o utilizador é eliminado do AD a cache de utilizador só é aplicável os certificados, a bruxa terem inscrito. Não recomendamos excluir nada na base de dados como isso pode causar danos geral para a operação do ambiente.

## <a name="opt-out-of-telemetry"></a>Sair da telemetria
Compilações anteriores FIM/MIM utilizado para recolhe telemetria anónimos sobre a implementação de cada e transmite esses dados através de HTTPS aos servidores da Microsoft. Estes dados foi utilizados pela Microsoft para ajudar a melhorar versões futuras do FIM/MIM no passado.

>[!Note] 
> Em versões posteriores do 4.5.x.x ou dados de maior coleção será desativada.

Para desativar os dados coleção na versão anterior, execute alterar o modo e anular a seleção de linha de comandos da seguinte:

![mim-privacidade-conformidade-ceip. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

ou editar o registo e defina o valor como 0: (componente) CEIP HKLM\SOFTWARE\Microsoft\Forefront identidade Manager\2010

![mim-privacidade-conformidade-ceip2. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>Passos Seguintes 
- [Para a documentação de orientação de privacidade SQL relacionados](https://docs.microsoft.com/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [Secção GDPR para o portal de confiança do serviço](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [Arquivo do FIM 2010: Ramp Up – implementando o Forefront Identity Manager 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)
