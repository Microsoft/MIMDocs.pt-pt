---
title: Manipulação de dados de Microsoft Identity Manager | Microsoft Docs
description: Entenda Microsoft Identity Manager manipulação de dados para facilitar e relatar dados no ambiente, execute ações no sistema determinado com base em funções e requisitos operacionais.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 12/02/2018
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: 6f861c5b1984de70a91edcac89276402f289e355
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "68701488"
---
# <a name="microsoft-identity-manager-data-handling"></a>Manipulação de dados de Microsoft Identity Manager 

Este artigo fornecerá orientação sobre como as organizações podem tomar decisões que podem ser aplicadas em várias fontes de dados conectadas.  Isso pode ser obtido por meio das operações de pesquisa, exclusão, atualização e relatório.  Antes de decidir sobre sua abordagem de exclusão ou atualização, uma compreensão do design e da configuração atuais do seu sistema de identidade Manager (MIM) é essencial. 

A seguir, alguns cenários que os clientes precisarão considerar e responder às seguintes perguntas: 

- Quais dados você precisa para o gerenciamento de identidades para ajudar no processo de negócios?
- Onde os dados atuais serão armazenados no MIM?
- Como você usará esses dados no sistema?
- Você está compartilhando esses dados com qualquer fonte de dados de parceiros externos (exportação)
- Qual é a fonte autoritativa para os dados e o processamento dele?
- Qual será seu plano de retenção de dados e de exclusão de dados em vigor?
- Você identificou toda a tecnologia de que precisa para processar e gerenciar dados?

Para ajudá-lo a entender um ambiente atual do MIM, você pode utilizar a seguinte ferramenta para documentar seu ambiente do MIM ou adiar para seus documentos de design de implementação.
- [Documentador do MIM – permite exportar a configuração atual](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>Procurar e identificar dados pessoais
A pesquisa de dados no MIM dependerá da configuração e da instalação. A maioria dos ambientes é interconectada, mas para fins de clareza, nós os dividimos por um componente de alto nível.

### <a name="synchronization-service"></a>Serviço de Sincronização

Todos os dados no MIM relacionados a usuários são derivados de fontes de dados Active Directory (AD) e HR. Ao pesquisar dados pessoais, o primeiro lugar que você deve considerar a pesquisa é o AD ou as fontes de dados conectadas. 

Se você não tiver certeza de que a fonte de autoridade pode controlar esse usuário no console do MIM Synchronization Service Manager, clique na barra de pesquisa do metaverso para exibir os dados pessoais identificáveis armazenados no banco de dados. Os usuários podem procurar um usuário ou atributo específico.

- Para executar uma revisão ou uma pesquisa de dados de objetos de usuário
    - Abrir o cliente do serviço de sincronização
        - Usar o designer de metaverso permite que você veja a precedência e as importações de fluxo de atributo.
![mim-Privacy-compliance_1. PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - Usar a pesquisa de metaverso permite pesquisar em qualquer objeto e atributo no banco de dados ![mim-Privacy-compliance_2. PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
Depois de localizar o objeto, clicar no objeto abrirá a página de perfil do usuário. Os detalhes do objeto fornecem os detalhes abrangentes sobre o objeto, seus atributos, última modificação e fonte de autoridade e fonte de dados conectada relacionada, derivada do exemplo de configuração do agente de gerenciamento abaixo.

![mim-privacidade-conformidade. PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>Serviço e portal/PAM
Se você tiver uma instância do serviço e do portal ou do PAM instalado, a capacidade de Pesquisar usuários será importante. 

Se você instalou o portal, poderá usar a interface do usuário para pesquisar qualquer atributo ou consulta para um usuário específico.

Se você tiver apenas o servidor de serviço (sem interface do usuário do Portal) instalado, poderá executar uma sintaxe de pesquisa baseada no [FIMAutomation PSSnapin], exemplo encontrado [aqui](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx).

O PAM pode usar a mesma sintaxe acima ou você pode usar o [módulo MIMPAM](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) especificamente o cmdlet Get-pamuser para pesquisar o usuário no ambiente do Pam.

Outras opções de relatório para pesquisar dados disponíveis estão no portal e no serviço.
- [Relatório híbrido](https://docs.microsoft.com/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [Relatórios com SCSM](https://docs.microsoft.com/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
O serviço Bhold Core tem uma interface do usuário que permite pesquisar um ou mais atributos. 

![pesquisa do bhold](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

Se você estiver sincronizando o BHOLD com o [Access Management Connector](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-access-management-connector-install) para o serviço de sincronização, poderá ver os objetos de usuário conectados e os atributos que você envia para o BHOLD Core.

Além disso, você pode carregar o módulo relatório do BHOLD.

- [Relatórios do BHOLD](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>Gestão de Certificados
A pesquisa do serviço de gerenciamento de certificados é incorporada à interface do usuário. O administrador será iniciado e selecionará "localizar usuário e exibir ou gerenciar suas informações"  

![pesquisa cm](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>Exportar os dados pessoais
Como os dados relacionados às entidades no MIM são derivados de várias fontes, a maioria dos dados é armazenada no banco de dado do serviço de sincronização. Por esse motivo, você deve exportar dados relacionados ao objeto da sincronização do MIM ou pode determinar o proprietário desses dados.

### <a name="synchronization-service"></a>Serviço de Sincronização
Os serviços de sincronização para exportar dados simplesmente selecionam os dados da interface do usuário de pesquisa e copiam e colam em um formato CSV ou preferencial. Outra maneira de exportar esses dados é criar um MA baseado em arquivo para remover os dados atuais necessários sobre um usuário sinalizado de interesse. Um exbordo do uso de MA baseada em arquivo pode ser encontrado [aqui](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/).


### <a name="service-and-portal--pam"></a>Serviço e portal/PAM
Serviço e portal juntamente com o PAM, você pode exportar esses dados executar uma sintaxe de pesquisa baseada no [FIMAutomation PSSnapin], exemplo encontrado [aqui](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx) e redirecioná-lo para [CSV](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6).

O PAM pode usar a mesma sintaxe acima ou você pode usar o [módulo MIMPAM](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) especificamente o Get-pamuser para pesquisar o usuário no ambiente do Pam e redirecioná-lo a um CSV.

- [Exemplo consultando o serviço do MIM usando o PowerShell](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
Os dados do Bhold podem ser exportados usando o módulo relatórios do Bhold para seu formato preferido.

### <a name="certificate-management"></a>Gestão de Certificados
Os dados de gerenciamento de certificados relacionados a dados pessoais são conectados ao Active Directory. Um administrador pode exportar esses dados usando Active Directory PowerShell.

## <a name="updating-personal-data"></a>A atualizar os dados pessoais

Os dados pessoais sobre usuários ou objetos em soluções do MIM normalmente são derivados do objeto do usuário nas fontes de dados conectadas da sua organização. Como as alterações feitas no perfil de usuário na origem de RH ou outro sistema autoritativo de registro, como o AD, são refletidas no serviço de sincronização do MIM.

### <a name="synchronization-service"></a>Serviço de Sincronização

Para executar operações de gerenciamento, os administradores devem fazer parte das operações de sincronização ou do administrador definido [aqui](https://docs.microsoft.com/previous-versions/mim/jj590183(v%3dws.10)).

A atualização de dados é feita pela definição de regras da origem da autoridade. O console de gerenciamento ajuda a identificar a origem de autoridade para atualizá-la na origem. Outra opção é criar regra de sincronização ou extensão de regra para controlar a atualização de dados se a origem, como dados de RH, ainda precisar permanecer. Essas são opções com suporte avialible.

Para obter mais informações sobre diferentes maneiras de atualizar o atributo, consulte abaixo. 

- [Usando extensões de regras](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Compreender a Sincronização de Dados com Sistemas Externos](https://docs.microsoft.com/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>Serviço e portal/PAM

O serviço e o portal para incluir dados do PAM podem ser atualizados usando os cmdlets FIMAutomation ou PAM. Se você tiver o portal, também poderá atualizar diretamente pesquisando e modificando o objeto. Uma coisa a observar e, dependendo da configuração, basta atualizar do portal não significa que ela permanecerá. Como a origem da autoridade é altamente dependente da configuração geral.

### <a name="bhold"></a>BHOLD

Os usuários podem ser atualizados diretamente com a interface do usuário do BHOLD core ou com o conector de gerenciamento de acesso.

### <a name="certificate-management"></a>Gestão de Certificados

Os usuários no serviço de gerenciamento de certificados são uma reflexão do Active Directory. Para atualizar, use Active Directory para alterar detalhes do objeto.

## <a name="deleting-personal-data"></a>A eliminar os dados pessoais

>[!Note] 
> Este artigo fornece orientação sobre maneiras de excluir dados pessoais de Microsoft Identity Manager e pode ser usado para dar suporte às suas obrigações no GDPR. Se quiser obter informações gerais sobre o RGPD, veja a [secção RGPD do Portal de Confiança do Serviço](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

Os dados no MIM são sincronizados e sempre atualizados a partir de sua fonte de dados conectada. Quando um objeto é excluído no destino, os dados do objeto no MIM podem ser mantidos para fins de investigação de segurança. A exclusão de objetos é configurada por regras de fonte de dados conectadas ou por extensão de regra (código) e/ou regras de exclusão de objeto.

### <a name="synchronization-service"></a>Serviço de Sincronização
O serviço de sincronização tem várias maneiras de lidar com dados ou excluir dados, dependendo dos processos de negócios. Para ajudar a entender, veja abaixo alguns artigos para ajudar a entender as opções de exclusão e atualização de atributos: 

- [Compreender o Desaprovisionamento](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [Usando extensões de regras](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)
- [Práticas recomendadas do MIM](https://docs.microsoft.com/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>Serviço e portal/PAM

É recomendável para o portal de & de serviço que você mantenha a configuração padrão de retenção de recursos do sistema de 30 dias. Isso informa ao serviço quando ele será excluído, não apenas solicita dados, mas também qualquer objeto que precise ser limpo do sistema. Quando o processo ocorre, todos os dados vinculados a esse objeto são excluídos isso inclui todos os dados de registro do SSPR. Isso é reproduzido na configuração de exclusão de objeto acima. Temos uma tabela que armazenamos o GUID dos objetos. Para reduzir o tamanho geral da tabela no Build 4.4.1459, adicionamos um processo chamado FIM_DeleteExpiredSystemObjectsJob detalhes sobre esse processo podem ser encontrados [aqui](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden).

![mim-privacidade-conformidade-SRRC. PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

Bhold como a maioria dos sistemas conectados ao serviço de sincronização podem ser configurados para excluir uma vez que o objeto de origem, como HR, é removido. Isso é configurado no agente de gerenciamento. e controladas pelas regras de exclusão de objetos, conforme descrito nos recursos do serviço de sincronizações.

Outra opção é remover o objeto de usuário da interface do usuário do BHOLD Core. Dependendo da configuração, isso pode funcionar bem, mas a lógica de provisionamento poderá recriar esse usuário se não for excluído na origem.
![mim-Privacy-Compliance-bholdr.](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG) PNG


### <a name="certificate-management"></a>Gestão de Certificados
Para remover um usuário do CM, exclua o usuário que está no Active Directory.

Gerenciamento de certificados como ele armazenará apenas a UID do perfil dos serviços de certificados com o domínio sAMAccountName. Depois que o usuário é excluído do AD, o cache do usuário está presente apenas para os certificados, a bruxa que eles registraram. Não recomendamos a exclusão de nada no banco de dados, pois isso pode causar danos gerais à operação do ambiente.

## <a name="opt-out-of-telemetry"></a>Recusa de telemetria
As compilações anteriores do FIM/MIM usadas para coletar telemetria anônima sobre cada implantação e transmite esses dados por HTTPS para servidores Microsoft. Esses dados foram usados pela Microsoft para ajudar a aprimorar versões futuras do FIM/MIM no passado.

>[!Note] 
> Em versões posteriores de 4.5. x. x ou mais, a coleta de dados será desabilitada.

Para desabilitar a coleta de dados na versão anterior, execute o modo de alteração e desmarque o seguinte prompt:

![mim-privacidade-conformidade-CEIP. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

ou edite o registro e defina o valor para 0: (componente) CEIP HKLM\SOFTWARE\Microsoft\Forefront identidade Manager\2010

![mim-privacidade-conformidade-ceip2. PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>Próximos Passos 
- [Diretrizes de privacidade relacionadas ao SQL](https://docs.microsoft.com/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [Seção GDPR do portal de confiança do serviço](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [Arquivo do FIM 2010: crescimento-implementação do Forefront Identity Manager 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)
