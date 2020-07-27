---
title: Tratamento de dados do Microsoft Identity Manager Microsoft Docs
description: Compreenda o tratamento de dados do Microsoft Identity Manager para identificar e reportar dados dentro do ambiente, tome medidas num determinado sistema com base em funções operacionais e requisitos.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 12/02/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: b89f7561869e154ed5639835d1233e19e356ee76
ms.sourcegitcommit: f87be3d09cee6a8880b3a6babf32e0d064fde36b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2020
ms.locfileid: "87176749"
---
# <a name="microsoft-identity-manager-data-handling"></a>Tratamento de dados do Gestor de Identidade da Microsoft 

Este artigo fornecerá orientações sobre como as organizações podem tomar decisões que podem ser aplicadas através de muitas fontes de dados conectadas.  Isto pode ser conseguido através das operações de pesquisa, exclusão, atualização e relatório.  Antes de decidir sobre a sua abordagem de eliminar ou atualizar, é fundamental uma compreensão do design e configuração atuais do seu sistema de gestor de identidade (MIM). 

Abaixo estão alguns cenários que os clientes terão de considerar e responder às seguintes perguntas: 

- Que dados precisa para que a sua gestão de identidade ajude no processo de negócio?
- Onde é que os dados atuais vão ser armazenados na MIM?
- Como utilizará estes dados no sistema?
- Está a partilhar estes dados com quaisquer fontes de dados de parceiros externos (Exportação)
- Qual é a fonte autoritária para os dados e o seu tratamento?
- Qual será o seu plano de retenção de dados e eliminação de dados?
- Identificou toda a tecnologia de que precisa para processar e gerir dados?

Para ajudá-lo a entender um ambiente MIM atual, pode utilizar a seguinte ferramenta para documentar o seu ambiente MIM, ou adiar para os seus documentos de design de implementação.
- [Documentor MIM - Permite exportar configuração atual](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Procurar e identificar dados pessoais
A pesquisa de dados dentro do MIM será dependente da configuração e configuração. A maioria dos ambientes estão interligados, mas para a clareza, eliminámo-los por componentes de alto nível.

### <a name="synchronization-service"></a>Serviço de Sincronização

Todos os dados em MIM que se relacionam com os utilizadores são derivados de fontes de Dados Ativos (AD) e HR Data. Ao procurar dados pessoais, o primeiro lugar que deve considerar procurar é AD ou fontes de dados conectadas. 

Se não tiver a certeza de que a fonte de autoridade pode rastrear este utilizador a partir da consola Mim Synchronization Service Manager, clique na barra de Pesquisa Metaverse para ver os dados pessoais identificáveis que estão armazenados na base de dados. Os utilizadores podem procurar um utilizador ou atributo específico.

- Para realizar uma análise ou pesquisa de dados de objetos de utilizador
    - Abra o cliente do serviço de sincronização
        - A utilização do designer metaverso permite-lhe ver importações de fluxo de atributos e precedência.
![mim-privacy-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - A utilização da pesquisa de metaverso permite-lhe pesquisar qualquer objeto e atribuir dentro da base de dados ![mim-privacy-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
Depois de encontrar o objeto, clicar no objeto abrirá a página do perfil do utilizador. Os detalhes do objeto fornecem-lhe os detalhes abrangentes sobre o objeto, seus atributos, última modificação e fonte de autoridade, e fonte de dados conectada relacionada derivada do exemplo de configuração do agente de gestão abaixo.

![mim-privacy-compliance.PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Serviço e Portal / PAM
É importante pesquisar por utilizadores uma instância do Serviço e portal ou PAM instalada. 

Se instalou o Portal, pode utilizar o UI para pesquisar qualquer atributo ou consulta para um determinado utilizador.

Se tiver apenas o servidor de serviço (sem Portal UI) instalado, pode executar uma sintaxe de pesquisa com base na [FIMAutomation PSSnapin], Exemplo encontrado [aqui](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx).

A PAM pode utilizar a mesma sintaxe acima ou pode utilizar o [Módulo MIMPAM](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) especificamente o cmdlet get-pamuser para procurar o utilizador dentro do ambiente PAM.

Outras opções de reporte para pesquisar os dados disponíveis estão no serviço e portal.
- [Reportagem híbrida](https://docs.microsoft.com/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Reportagem com a SCSM](https://docs.microsoft.com/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
O serviço Bhold Core tem uma UI que lhe permite pesquisar por um utilizador ou atributos. 

![pesquisa bhold](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Se estiver a sincronizar o BHOLD com [o conector de gestão de acesso](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-access-management-connector-install) para o serviço de sincronização, poderá ver os objetos do utilizador ligados e os atributos que envia para o núcleo BHOLD.

Também pode carregar o módulo BHOLD Reporting.

- [Relatório bhold](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Gestão de Certificados
A pesquisa do serviço de gestão de certificados está incorporada na UI. O administrador lançará e selecionará o "Localizar utilizador e visualizar ou gerir as suas informações"  

![pesquisa cm](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Exportar dados pessoais
Como os dados relacionados com entidades na MIM são derivados de múltiplas fontes, a maioria dos dados é armazenada na base de dados do Serviço de Sincronização. Por esta razão, deverá exportar dados relacionados com objetos da MIM Sync ou pode determinar o proprietário destes dados.

### <a name="synchronization-service"></a>Serviço de Sincronização
Os serviços de sincronização para exportação de dados simplesmente selecionam os dados da UI de pesquisa e copiam e colam num formato csv ou preferencial. Outra forma de exportar estes dados é criar um MA baseado em ficheiros para deixar cair os dados atuais necessários sobre um utilizador sinalizado de interesse. Um exmaple de utilização de MA baseado em ficheiros pode ser encontrado [aqui](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/).


### <a name="service-and-portal--pam"></a>Serviço e Portal / PAM
Serviço e portal juntamente com a PAM você pode exportar estes dados executar uma sintaxe de pesquisa com base no [FIMAutomation PSSnapin], Exemplo encontrado [aqui](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) e tubo-lo para [csv](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6).

A PAM pode utilizar a mesma sintaxe acima ou pode utilizar o [Módulo MIMPAM](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) especificamente o get-pamuser para procurar o utilizador dentro do ambiente PAM e encaná-lo a um csv.

- [Exemplo consulta do serviço MIM usando powershell](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
Os dados bhold podem ser exportados usando o módulo de relatório Bhold para o seu formato preferido.

### <a name="certificate-management"></a>Gestão de Certificados
Os dados de gestão de certificados relacionados com dados pessoais estão ligados ao diretório ativo. Um administrador pode exportar estes dados usando o Ative Directory PowerShell.

## <a name="updating-personal-data"></a>Atualizar dados pessoais

Os dados pessoais sobre utilizadores ou objetos em SOLUÇÕES MIM normalmente são derivados do objeto do utilizador nas fontes de dados conectadas da sua organização. Porque quaisquer alterações feitas ao perfil do utilizador na fonte de RH, ou outro sistema autoritário de registo, tais como AD são então refletidas no Serviço de Sincronização MIM.

### <a name="synchronization-service"></a>Serviço de Sincronização

Para realizar operações de gestão, os administradores devem fazer parte de operações de sincronização ou administrador [aqui](https://docs.microsoft.com/previous-versions/mim/jj590183(v%3dws.10))definidas.

A atualização dos dados é feita através da definição de regras a partir da fonte de autoridade. A consola de gestão ajuda a identificar a fonte de autoridade para atualizá-la na fonte. Outra opção é criar a regra de sincronização ou a extensão de regra para controlar a atualização de dados se a fonte como os dados de RH ainda precisar permanecer. Estas são opções suportadas disponíveis.

Para obter mais informações sobre diferentes formas de atualizar o atributo, consulte abaixo. 

- [Utilização de extensões de regras](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Compreender a Sincronização de Dados com Sistemas Externos](https://docs.microsoft.com/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Serviço e Portal / PAM

O serviço e o Portal para incluir dados PAM podem ser atualizados através dos cmdlets FIMAutomation ou PAM. Se tiver o Portal, também pode atualizar diretamente, pesquisando e modificando o objeto. Uma coisa a notar e dependendo da configuração simplesmente atualizar a partir do portal não significa que permanecerá. Como a fonte de autoridade é altamente dependente da configuração geral.

### <a name="bhold"></a>BHOLD

Os utilizadores podem ser atualizados diretamente com a interface do utilizador BHOLD Core ou com o conector de gestão de acesso.

### <a name="certificate-management"></a>Gestão de Certificados

Os utilizadores do serviço de gestão de certificados são todos uma reflexão do diretório ativo. Para atualizar, utilize o Ative Directory para alterar detalhes do objeto.

## <a name="deleting-personal-data"></a>Eliminar dados pessoais

>[!Note] 
> Este artigo fornece orientações sobre formas de eliminar dados pessoais do Microsoft Identity Manager e pode ser usado para suportar as suas obrigações ao abrigo do RGPD. Se estiver à procura de informações gerais sobre o GDPR, veja a [secção GDPR do Portal de Confiança do Serviço](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

Os dados na MIM são sincronizados e sempre atualizados a partir da sua fonte de dados conectada. Quando um objeto é eliminado no alvo, os dados do objeto em MIM podem ser mantidos para fins de investigação de segurança. A eliminação de objetos está configurada de acordo com as regras de origem de dados ligadas ou regras de extensão de regras (código) e/ou regras de eliminação de objetos.

### <a name="synchronization-service"></a>Serviço de Sincronização
Serviço de Sincronização tantas formas de lidar com dados ou eliminar dados dependendo dos processos de negócio. Para ajudar a entender, abaixo estão alguns artigos para ajudar a entender opções sobre a eliminação e atualização de atributos: 

- [Compreender o Desaprovisionamento](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Utilização de extensões de regras](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Melhores Práticas da MIM](https://docs.microsoft.com/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Serviço e Portal / PAM

Recomenda-se para o Portal & Serviço que mantenha a configuração de retenção de recursos do sistema de 30 dias padrão. Isto indica ao serviço quando irá apagar, não só solicitar dados, mas também qualquer objeto que precise de ser retirado do sistema. Uma vez que o processo ocorre, todos os dados ligados a este objeto são eliminados isto inclui todos os dados de registo SSPR. Isto reproduz-se na configuração de eliminação de objetos acima. Temos uma mesa onde guardamos o guia dos objetos. Para reduzir o tamanho total da tabela na construção 4.4.1459, adicionámos um processo chamado FIM_DeleteExpiredSystemObjectsJob detalhes sobre este processo podem ser [encontrados aqui.](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden)

![mim-privacy-compliance-srrc.PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

Bhold como a maioria dos sistemas ligados ao serviço de sincronização pode ser configurado para eliminar uma vez que o objeto de origem como HR é removido. Isto está configurado no agente de gestão. e controlado pelas regras de eliminação de objetos, conforme descrito nas funções de serviço de sincronização.

Outra opção é remover o objeto do utilizador diretamente da interface do Utilizador BHOLD Core. Dependendo da configuração, isto pode funcionar bem, mas a lógica de provisionamento de notas pode recriar este utilizador se não for eliminado na fonte.
![mim-privacy-compliance-bholdr.PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>Gestão de Certificados
Para remover um utilizador de CM, elimine o utilizador em diretório ativo.

A gestão de certificados, uma vez que apenas armazenará o perfil uid dos serviços de certificados com o domínio sAMAccountName. Uma vez eliminado o utilizador da AD, a cache do utilizador só está presente nos certificados que tenham matriculado. Não recomendamos a eliminação de nada na base de dados, uma vez que isso pode causar danos globais ao funcionamento do ambiente.

## <a name="opt-out-of-telemetry"></a>Opt-out da telemetria
Construções anteriores FIM/MIM usadas para recolher telemetria anonimizada sobre cada implementação e transmite estes dados através de HTTPS para servidores da Microsoft. Estes dados foram utilizados pela Microsoft para ajudar a melhorar futuras versões do FIM/MIM no passado.

>[!Note] 
> Em lançamentos posteriores de 4.5.x.x ou maior recolha de dados será desativada.

Para desativar a recolha de dados na versão anterior, executar o modo de alteração e desmarcar o seguinte aviso:

![mim-privacy-compliance-ceip.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

ou editar o registo e definir o valor para 0: (Componente)CEIP HKLM\SOFTWARE\Microsoft\Microsoft\Forefront Identity Manager\2010

![mim-privacy-compliance-ceip2.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>Próximos Passos 
- [Para orientação de privacidade relacionada com o SQL](https://docs.microsoft.com/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [Secção rGPD do portal Service Trust](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [FIM 2010 Archive: Ramp Up - Implementação do Gestor de Identidade da Vanguarda 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)
