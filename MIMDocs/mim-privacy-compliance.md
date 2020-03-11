---
title: Tratamento de dados do Microsoft Identity Manager / Microsoft Docs
description: Compreender o tratamento de dados do Microsoft Identity Manager para identificar e reportar dados dentro do ambiente, tomar medidas em determinado sistema com base em funções e requisitos operacionais.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 12/02/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: e95cf26b62e582eaa3c07c40e551bc5930d3b1b0
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79044111"
---
# <a name="microsoft-identity-manager-data-handling"></a>Tratamento de dados do Microsoft Identity Manager 

Este artigo fornecerá orientações sobre como as organizações podem tomar decisões que podem ser aplicadas em muitas fontes de dados conectadas.  Isto pode ser conseguido através das operações de pesquisa, exclusão, atualização e reporte.  Antes de decidir sobre a sua abordagem de apagar ou atualizar, é fundamental uma compreensão do design atual e configuração do seu sistema de gestor de identidade (MIM). 

Abaixo estão alguns cenários que os clientes terão de considerar e responder às seguintes perguntas: 

- Que dados precisa para que a gestão de identidade ajude no processo de negócio?
- Onde é que os dados atuais vão ser armazenados em MIM?
- Como é que vai utilizar estes dados no sistema?
- Está a partilhar estes dados com quaisquer fontes de dados de parceiros externos (Exportação)
- Qual é a fonte autoritária para os dados e o tratamento dos mesmos?
- Qual será o seu plano de retenção de dados e eliminação de dados?
- Identificou toda a tecnologia necessária para processar e gerir dados?

Para ajudá-lo a compreender um ambiente MIM atual, pode utilizar a seguinte ferramenta para documentar o seu ambiente MIM, ou adiar para os seus documentos de design de implementação.
- [MIM Documentor - Permite exportar configuração atual](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Procurar e identificar dados pessoais
A pesquisa de dados dentro da MIM dependerá da configuração e configuração. A maioria dos ambientes estão interligados, mas para clareza, quebrámo-los por componentes de alto nível.

### <a name="synchronization-service"></a>Serviço de Sincronização

Todos os dados em MIM relacionados com os utilizadores são derivados de fontes de Dados de Ative (AD) e HR. Ao procurar dados pessoais, o primeiro local onde deve considerar procurar é a AD ou fontes de dados conectadas. 

Se não tiver a certeza da origem da autoridade, pode rastrear este utilizador a partir da consola MIM Synchronization Service Manager, clique na barra metaverse search para ver os dados pessoais identificáveis que estão armazenados na base de dados. Os utilizadores podem procurar um utilizador ou atributo específico.

- Para efetuar uma revisão ou pesquisa de dados de objetos de utilizador
    - Abra o cliente do serviço de sincronização
        - A utilização do designer metaverso permite-lhe ver importações e precedência do fluxo de atributos.
![mim-privacy-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - A utilização da pesquisa metaverso permite-lhe pesquisar qualquer objeto e atribuir dentro da base de dados ![mim-privacy-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
Depois de encontrar o objeto, clicar no objeto abrirá a página do perfil do utilizador. Os detalhes do objeto fornecem-lhe os detalhes abrangentes sobre o objeto, os seus atributos, a última modificação e fonte de autoridade, e fonte de dados conectada relacionada derivada do exemplo de configuração do agente de gestão abaixo.

![mim-privacidade-compliance. PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Serviço e Portal / PAM
Se tiver uma instância do Serviço e portal ou PAM instalada seleção de pesquisa para os utilizadores é importante. 

Se instalou o Portal, pode utilizar o UI para pesquisar qualquer atributo ou consulta para um determinado utilizador.

Se tiver apenas o servidor de serviço (sem portal UI) instalado, pode executar uma sintaxe de pesquisa com base no [FIMAutomation PSSnapin], exemplo [aqui](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx)encontrado .

O PAM pode utilizar a mesma sintaxe acima ou pode utilizar o [Módulo MIMPAM](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) especificamente o cmdlet get-pamuser para procurar o utilizador dentro do ambiente PAM.

Outras opções de reporte para pesquisar os dados disponíveis estão no serviço e no portal.
- [Reportagem Híbrida](https://docs.microsoft.com/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Reportagem com SCSM](https://docs.microsoft.com/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
O serviço Bhold Core tem um UI que lhe permite procurar um utilizador ou atributos. 

![pesquisa de bhold](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Se estiver a sincronizar o BHOLD com [o conector](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-access-management-connector-install) de gestão de acesso para o serviço de sincronização, poderá ver os objetos de utilizador conectados e os atributos que envia para o núcleo BHOLD.

Também pode carregar o módulo BHOLD Reporting.

- [Relatórios BHOLD](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Gestão de Certificados
A pesquisa de serviçode gestão de certificados está incorporada na UI. O administrador lançará e selecionará o 'Localizar utilizador e visualizar ou gerir as suas informações'  

![pesquisa cm](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Exportar os dados pessoais
Como os dados relacionados com entidades em MIM são derivados de várias fontes, a maioria dos dados é armazenado na base de dados do Serviço de Sincronização. Por esta razão, deve exportar dados relacionados com objetos a partir do MIM Sync ou pode determinar o proprietário destes dados.

### <a name="synchronization-service"></a>Serviço de Sincronização
Os serviços de sincronização para dados de exportação simplesmente selecionam os dados da UI de pesquisa e copiam e colam num formato csv ou preferido. Outra forma de exportar estes dados é criar um MA baseado em Ficheiros para deixar cair os dados atuais necessários sobre um utilizador sinalizado de interesse. Um exmaple de usar MA baseado em ficheiros pode ser encontrado [aqui](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/).


### <a name="service-and-portal--pam"></a>Serviço e Portal / PAM
Serviço e portal juntamente com PAM pode exportar estes dados executar uma sintaxe de pesquisa com base no [FIMAutomation PSSnapin], Exemplo encontrado [aqui](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) e encaná-lo para [csv](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6).

O PAM pode utilizar a mesma sintaxe acima ou pode utilizar o [Módulo MIMPAM](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) especificamente o get-pamuser para procurar o utilizador dentro do ambiente PAM e encaná-lo para um csv.

- [Exemplo consulta do serviço MIM usando powershell](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
Os dados de isolo podem ser exportados utilizando o módulo de relatório de retenção para o seu formato preferido.

### <a name="certificate-management"></a>Gestão de Certificados
Os dados de gestão de certificados relacionados com dados pessoais estão ligados ao diretório ativo. Um administrador pode exportar estes dados usando powershell de Diretório Ativo.

## <a name="updating-personal-data"></a>A atualizar os dados pessoais

Os dados pessoais sobre utilizadores ou objetos em Soluções MIM são normalmente derivados do objeto do utilizador nas fontes de dados conectadas da sua organização. Uma vez que quaisquer alterações feitas ao perfil do utilizador na fonte de RH, ou noutro sistema de registo sinuoso, como a AD são então refletidas no Serviço de Sincronização MIM.

### <a name="synchronization-service"></a>Serviço de Sincronização

Para realizar operações de gestão, os administradores devem fazer parte de operações de sincronização ou de administradores [aqui](https://docs.microsoft.com/previous-versions/mim/jj590183(v%3dws.10))definidos.

A atualização dos dados é feita através da definição de regras a partir da fonte de autoridade. A consola de gestão ajuda a identificar a fonte de autoridade para atualizá-la na fonte. Outra opção é criar regra de sincronização ou a extensão da regra para controlar a atualização de dados se os dados de origem como os dados de RH ainda precisarem de permanecer. Estas são opções apoiadas avializadas.

Para obter mais informações sobre diferentes formas de atualizar o atributo, consulte abaixo. 

- [Utilização de extensões de regras](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Compreender a Sincronização de Dados com Sistemas Externos](https://docs.microsoft.com/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Serviço e Portal / PAM

O serviço e o Portal para incluir dados PAM podem ser atualizados utilizando os cmdlets FIMAutomation ou PAM. Se tiver o Portal, também pode atualizar diretamente pesquisando e modificando o objeto. Uma coisa a notar e dependendo da configuração simplesmente atualizar a partir do portal não significa que permanecerá. Como fonte de autoridade é altamente dependente da configuração geral.

### <a name="bhold"></a>BHOLD

Os utilizadores podem ser atualizados diretamente com a interface de utilizador BHOLD Core ou com o conector de gestão de acesso.

### <a name="certificate-management"></a>Gestão de Certificados

Os utilizadores do serviço de gestão de certificados são uma reflexão do diretório ativo. Para atualizar utilize o Ative Directory para alterar os detalhes do objeto.

## <a name="deleting-personal-data"></a>A eliminar os dados pessoais

>[!Note] 
> Este artigo fornece orientações sobre formas de eliminar dados pessoais do Microsoft Identity Manager e pode ser usado para suportar as suas obrigações ao abrigo do RGPD. Se quiser obter informações gerais sobre o RGPD, veja a [secção RGPD do Portal de Confiança do Serviço](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

Os dados em MIM são sincronizados e sempre atualizados a partir da sua fonte de dados conectada. Quando um objeto é eliminado no alvo, os dados do objeto em MIM podem ser mantidos para efeitos de investigação de segurança. A eliminação de objetos é configurada de acordo com as regras de origem de dados ligadas ou as regras de extensão de regras de extensão de regras de eliminação de regras de eliminação de objetos.ou/ou objetos.

### <a name="synchronization-service"></a>Serviço de Sincronização
Serviço de Sincronização tantas formas de lidar com dados ou apagar dados dependendo dos processos de negócio. Para ajudar a compreender, abaixo estão alguns artigos para ajudar a entender opções sobre alocamento e atualização: 

- [Compreender o Desaprovisionamento](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Utilização de extensões de regras](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Boas Práticas mim](https://docs.microsoft.com/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Serviço e Portal / PAM

Recomenda-se ao Serviço e Portal que mantenha a configuração de retenção de recursos do sistema padrão de 30 dias. Isto indica ao serviço quando irá apagar, não só solicitar dados, mas também qualquer objeto que precise de ser retirado do sistema. Uma vez que o processo ocorre, todos os dados ligados a este objeto são eliminados, isto inclui todos os dados de registo SSPR. Isto reproduz-se na configuração de eliminação do objeto acima. Temos uma mesa onde armazenamos o guia dos objetos. Para reduzir o tamanho total da tabela na construção 4.4.1459 adicionámos um processo chamado FIM_DeleteExpiredSystemObjectsJob detalhes sobre este processo podem ser encontrados [aqui](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden).

![mim-privacy-compliance-srrc. PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

A maior parte dos sistemas ligados ao serviço de sincronização pode ser configurada para eliminar assim que o objeto de origem como o RH for removido. Isto está configurado no agente de gestão. e controlado pelas regras de eliminação de objetos, conforme descrito nas funcionalidades de serviço de sincronização.

Outra opção é remover o objeto do utilizador diretamente da interface De utilizador Core BHOLD. Dependendo da configuração, esta lógica de provisionamento de nota poderia recriar este utilizador se não fosse eliminado na fonte.
![mim-privacy-compliance-bholdr.](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG) do PNG


### <a name="certificate-management"></a>Gestão de Certificados
Para remover um utilizador de CM, elimine o utilizador no diretório ativo.

Gestão de certificados, uma vez que apenas armazenará o perfil uid dos serviços de certificado com domínio sAMAccountName. Uma vez que o utilizador é eliminado de AD, a cache do utilizador só está presente para os certificados, bruxa que eles inscreveram. Não recomendamos a apagar nada na base de dados, pois isso pode causar danos globais ao funcionamento do ambiente.

## <a name="opt-out-of-telemetry"></a>Opt-out da telemetria
Anteriores construções FIM/MIM usadas para recolher telemetria anoonizada sobre cada implementação e transmite estes dados em HTTPS para servidores da Microsoft. Estes dados foram utilizados pela Microsoft para ajudar a melhorar futuras versões de FIM/MIM no passado.

>[!Note] 
> Em lançamentos posteriores de 4.5.x.x ou maior recolha de dados serão desativados.

Para desativar a recolha de dados no modo de alteração de execução da versão anterior e desseleccionar a seguinte solicitação:

![mim-privacy-compliance-ceip. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

ou editar o registo e definir o valor para 0: (Componente)CEIP HKLM\SOFTWARE\Microsoft\Forefront Identity Manager\2010

![mim-privacy-compliance-ceip2. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>Passos Seguintes 
- [Para orientação de privacidade relacionada com SQL](https://docs.microsoft.com/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [Secção rGPD do portal Service Trust](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [ARQUIVO FIM 2010: Ramp up - Implementação de Gestor de Identidade de Vanguarda 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)
