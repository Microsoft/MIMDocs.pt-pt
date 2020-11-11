---
title: História da versão do Gestor de Identidades Microsoft Docs
description: Este artigo documenta as várias alterações feitas no âmbito de atualizações ao MIM 2016
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/18/2019
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 530229a3e5217955e974021ba42870f415e66033
ms.sourcegitcommit: 78c2d7e5ba4bec276d5a9bf8860bc126d9bd9c33
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/11/2020
ms.locfileid: "94492519"
---
# <a name="identity-manager-version-release-history"></a>Histórico de lançamento da versão do Gestor de Identidade

A equipa do Microsoft Identity Manager lança regularmente atualizações. Este artigo foi concebido para o ajudar a acompanhar as versões que foram lançadas. Pode então determinar se já se encontram na versão mais recente do pacote de serviços e da atualização do hotfix ou se precisam de atualizar. 

>[!NOTE]
>Este artigo descreve apenas os lançamentos dos componentes MIM Sync, Service e Portal, cliente, CM e PAM.  Não sabe de que componentes precisa?  Consulte [o licenciamento e downloads do Microsoft Identity Manager](../microsoft-identity-manager-licensing.md).
>
>O histórico da versão para os componentes do Microsoft BHOLD Suite pode ser encontrado no [histórico de lançamento da versão dos módulos BHOLD](version-bhold-history.md).
>
>O histórico da versão para os conectores genéricos LDAP, Generic SQL, web services, PowerShell, Graph e Lotus Domino pode ser encontrado no [Histórico de Lançamento da Versão Connector](microsoft-identity-manager-2016-connector-version-history.md).  

## <a name="mim-version-463550"></a>Versão MIM 4.6.355.0
- Estado: 6 de novembro de 2020
- [Download de hotfix](https://www.microsoft.com/download/details.aspx?id=102301)
- [Artigo KB 4585922](https://support.microsoft.com/help/4585922)

Este hotfix contém atualizações para os componentes MIM Synchronization Manager, MIM Service e MIM Portal, e também contém atualizações cumulativas aos componentes MIM dos hotfixes anteriores para MIM 2016 SP2.


## <a name="mim-version-462630"></a>Versão MIM 4.6.263.0
- Estado: 7 de agosto de 2020
- [Download de hotfix](https://www.microsoft.com/download/details.aspx?id=101612)
- [Artigo KB 4576473](https://support.microsoft.com/help/4576473)

Este hotfix contém atualizações para os componentes MIM CM, Mim Synchronization Manager e PAM.


## <a name="mim-version-462580"></a>Versão MIM 4.6.258.0
- Estado: 22 de maio de 2020
- [Download de hotfix](https://www.microsoft.com/download/details.aspx?id=101305)
- [Artigo KB 4560335](https://support.microsoft.com/help/4560335)

Este hotfix contém atualizações para os componentes MIM Service, MIM Portal e PAM.


## <a name="mim-version-46340"></a>Versão MIM 4.6.34.0
* Estado: PACOTE DE SERVIÇOS MIM 2016 2 (SP2) de outubro de 2019
* Número correspondente da versão BHOLD: 6.0.62.0
- [Download de hotfix](https://www.microsoft.com/download/details.aspx?id=100412)
- [Artigo KB 4512924](https://support.microsoft.com/help/4512924)
- O ficheiro ISO pode ser descarregado [a partir de VLSC](https://www.microsoft.com/Licensing/servicecenter/default.aspx) ou através de  [downloads do Visual Studio](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%202)

> [!IMPORTANT]
>- .Net Quadro 4.6 é necessário<br>
>- [Pacote Redistribuable Visual C++ 2013 (vcredist_x64.exe ou vcredist_x86.exe)](https://www.microsoft.com/download/details.aspx?id=40784) é necessário<br>

>[!NOTE]
> Certifique-se de que lê o guia de [implementação da MIM](../microsoft-identity-manager-deploy.md) para obter uma lista atualizada de pré-requisitos e limitações relacionadas com apenas ambientes TLS 1.2, suporte de contas de serviço geridos pelo grupo e o [caminho de upgrade das versões mim e FIM anteriores.](../microsoft-identity-manager-2016-service-pack-2-upgrade-path.md)

Um pacote rollup do Service Pack 2 (SP2) (build 4.6.34.0) está disponível para o Microsoft Identity Manager (MIM) 2016. Trata-se de uma atualização cumulativa que substitui as atualizações anteriores do MIM 2016 SP1 4.4.1302.0 através da construção 4.5.412.0.
### <a name="updates-in-mim-2016-service-pack-2"></a>Atualizações no Pacote de Serviços MIM 2016 2

#### <a name="mim-client-addons"></a>Addons do Cliente MIM
- Suporte adicional para o addon MIM Outlook a ser carregado no Outlook para a versão Click-To-Run do Microsoft 365.

#### <a name="service-and-portal"></a>Serviço e Portal
- Suporte adicional para serviço MIM e Portal a instalar no Windows Server 2019 e utilizar SQL Server 2017, Exchange Server 2019, SharePoint 2019, System Center Service Manager Data Warehouse 2019
- Serviço MIM habilitado e instalação do Portal em ambientes TLS 1.2.
- Instalação ativada para o Serviço MIM, Reset de Password e Registo de Passwords, Serviço de Monitorização PAM, Serviço de Componentes PAM para utilizar contas de serviço geridas pelo grupo.
- Parâmetro de instalação "keepSQLjobs".
- Os trabalhos temporais do Agente do Servidor MIM SQL já não começam nas réplicas secundárias do SQL Always-On Availability Group.
- Atributos virtuais 'ExplicitMember.Add' e 'ExplicitMember.Remove' habilitados para tipos de objetos personalizados em formulários RCDC para trabalhar com alterações delta.

#### <a name="synchronization-service"></a>Serviço de Sincronização
- Suporte adicional para o Serviço de Sincronização MIM a instalar no Windows Server 2019 e utilizar o SQL Server 2017, Exchange Server 2019
- Instalação do Serviço de Sincronização MIM ativada apenas em ambientes TLS 1.2.
- Instalação ativada para o Serviço de Sincronização MIM para utilizar uma conta de serviço gerida pelo grupo.
- Opção "Use a conta MIMSync" para o Agente de Gestão de Serviços MIM.
 
#### <a name="privilege-access-management"></a>Gestão de Acesso privilegiado 
- PowerShell cmdlet 'Get-PAMRequest' devolve uma propriedade adicional 'FIMRequestID'.


## <a name="mim-version-454120"></a>Versão MIM 4.5.412.0
> [!IMPORTANT]
>- A forma RCDC para objetos de grupo não consegue renderizar quando o valor do atributo 'displayOwner' não é povoado. Não poderá editar/visualizar grupos quando ocorrer tal erro.<br>

- Data de disponibilidade: 10 de maio de 2019
- [Download de hotfix](https://www.microsoft.com/en-us/download/details.aspx?id=58213)
- [Artigo KB 4489646](https://support.microsoft.com/en-us/help/4489646/hotfix-rollup-4-5-412-0-available-for-mim-2016-sp1)

Este hotfix contém atualizações para os componentes MIM Sync, MIM, Portal MIM, MIM CM e PAM.  Trata-se de uma atualização cumulativa que substitui as anteriores atualizações DO MIM 2016 SP1 4.4.1302.0 através da construção 4.5.286.0.

> [!IMPORTANT]
>- .NET Framework 4.6 também é necessário para o instalador <br>
>- [Visual C++ 2013 x64 Pacotes Redistribuíveis (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe) são necessários<br>

## <a name="mim-hybrid-reporting-agent-version-31410"></a>Mim Hybrid Reporting Agent Versão 3.1.41.0
- Data de disponibilidade: 22 de abril de 2019
- [Download de agente de relatórios híbrido](https://www.microsoft.com/en-us/download/details.aspx?id=55112)

A versão 3.1.41.0 do agente de reporte híbrido MIM inclui atualizações para TLS 1.2 e uma correção de erros.  O agente de reporte híbrido pode ser utilizado com as versões MIM 2016 SP1 4.4.1302.0 ou posterior e uma assinatura Azure AD Premium.


## <a name="mim-version-452860"></a>Versão MIM 4.5.286.0
- Estado: 19 de novembro de 2018
- [Download de hotfix](https://www.microsoft.com/en-us/download/details.aspx?id=57596)
- [Artigo KB 4469694](https://support.microsoft.com/en-us/help/4469694/hotfixrolluppackagebuild452860isavailableformicrosoftidentitymanager20)


Este hotfix contém atualizações para os componentes MIM Service, MIM Portal e PAM.  .NET Quadro 4.6 é necessário para o instalador.


## <a name="version-452020"></a>Versão 4.5.202.0
- Estado: 30 de agosto de 2018
- [KB Lançamento KB](https://support.microsoft.com/en-us/help/4346632)

> [!IMPORTANT]
>- .NET Framework 4.6 também é necessário para o instalador <br>
>- [Visual C++ 2013 x64 Pacotes Redistribuíveis (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe) são necessários<br>
>- Locais apoiados atualizados para novas normas ISO[(aqui)](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-language-support)<br>
>- *Denota novos melhoramentos 

#### <a name="mim-service"></a>Serviço MIM
- Integração do Servidor Azure MFA -Fornecedor alternativo de autenticação multi-factor

#### <a name="privilege-access-management"></a>Gestão de Acesso privilegiado 
- PAM REST API não pôde ser iniciada porque não podia carregar arquivo ou montagem

#### <a name="microsoft-identity-portal"></a>Portal de Identidade da Microsoft
- Portal são apresentados com um comprimento de mesa incorreto
- Diálogo avançado de pesquisa do Portal, as barras de deslocamento não são apresentadas corretamente
- Language Pack de linguagem de pacote de nome forte verificação de assinatura falhou

#### <a name="certificate-management"></a>Gestão de Certificados
- Declaração de redirecionamento vinculativo para REST API

## <a name="version-45260"></a>Versão 4.5.26.0
- Estado: 30 de junho de 2018
- [KB Release KB4073679](https://support.microsoft.com/en-us/help/4073679)

> [!IMPORTANT]
>- .NET Framework 4.6 também é necessário para o instalador <br>
>- [Visual C++ 2013 x64 Pacotes Redistribuíveis (vcresist_x64.exe)](https://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe) são necessários<br>
>- Locais apoiados atualizados para novas normas ISO[(aqui)](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016-language-support)<br>
>- *Denota novos melhoramentos 

#### <a name="synchronization-service"></a>Serviço de sincronização
- *Suporte para contas de serviço geridas pelo grupo
- *Visual Studio Support (Visual Studio 2013,Visual Studio 2015,Visual Studio 2017)
- Atualizações para MIISACTIVATE.EXE, suporte gMSA adicionado 
    - non-gMSA: Miisactivate.exe c:\configBU\miiserver_01.bin "contoso\mimSyncService" *
    - gMSA: Miisactivate.exe c:\configBU\miiserver_01.bin "contoso\mimSyncService"
- Atualizações para MIISKMU.exe, suporte gMSA adicionado 
    - non-gMSA:MIISKMU.exe /e c:\configBU\miiserver_02.bin" /u:"contoso\mimSyncService"
    - gMSA:MIISKMU.exe /e c:\configBU\miiserver_02.bin" /u:"contoso\mimSyncService" *
- As informações atualizadas de partilha são guardadas como esperado quando os botões De Atualização então OK são clicados
- Ao indexar um atributo de string indexável é demasiado longo um erro inesperado foi devolvido, uma mensagem de erro mais descritiva é agora devolvida
- A criação de um agente de gestão de ficheiros de texto quando o Serviço de Sincronização MIM é instalado no Windows Server 2016, algumas opções de codificação de texto, incluindo o Unicode, não estavam disponíveis
- MIM Service MA Se uma mensagem de erro de exportação contiver um carácter inválido, isto causa corrupção nas entradas do histórico de execução. Esta construção removemos da mensagem de erro antes de sermos guardados no objeto do espaço do conector e executar o histórico

#### <a name="mim-service"></a>Serviço MIM
- *Suporte para contas de serviço geridas pelo grupo
- *Melhor suporte linguístico a um novo padrão definido
- *FIMAutomation Export-FIMConfig PowerShell cmdlet o argumento "-PamConfig" está disponível para forçar a exportação dos objetos de configuração PAM
- *FIMAutomation Export-FIMConfig PowerShell cmdlet o parâmetro "pedido" foi adicionado
- *Os atributos booleanos são sempre definidos para NU APÓS a criação de ligação, O Boolean anterior antes de hotfix não será atualizado
> [!IMPORTANT]
>Isto pode ser uma mudança de rutura se pré-formar uma migração de configuração. A configuração deve ser avaliada e atualizada para nova funcionalidade, uma vez que a migração de configuração é considerada uma nova 
    - Iniciação implementada de novos atributos MIM Boolean a falsos na criação de novo objeto
    - Iniciação implementada de novos atributos MIM Boolean a falsos na adição de novo atributo Boolean vinculativo ao recurso
- A definição do Programa de Melhoria da Experiência do Cliente é mantida em falso 
- A instalação do Serviço MIM falhou com o erro de atualização da base de dados: Não é possível inserir o valor NULO na coluna 'Nome' se não for utilizado o nome da base de dados padrão
- Nos casos de hotfix, a definição microsoft 365 seria limpa, a palavra-passe encriptada para a caixa de correio online do Serviço MIM não é alterada
- *Não havia limite para o ficheiro de registo do Serviço MIM criado, a definição de registo padrão de registo atualizado e a capacidade de registo circular implementada

#### <a name="privileged-access-management"></a>Gestão de Acesso Privilegiado 
- *Suporte para contas de serviço geridas pelo grupo
- *Melhor suporte linguístico a um novo padrão definido
- Os objetos que utilizam recursos não geridos não são limpos a tempo.  estes objetos serão devidamente limpos
- *New-PAMRole PowerShell cmdlet o "-disableAutoApproveIfOwner" nega auto-aprovação para o papel
- * Get-PamRequest PowerShell cmdlet o "-CreatedFrom" permite a filtragem do pedido específico da OD PAM
- *Adições do módulo PAM
    - Get-PAMSet
    - Add-PAMSetMember
    - Remove-PAMSetMember
- O aviso (Exceção: System.ObjectDisposedException: Não pode aceder a um objeto eliminado) deixará de aparecer no registo de eventos PAM
- Set-PAMUser cmdlet é capaz de alterar o Nome PrivAccount sem a eliminação
- New-PamRole valida agora que a data "disponível para" é maior do que a data "disponível a partir"
- Os valores "Disponíveis de" e "Disponível Para" são devolvidos pelo Get-PAMRole PowerShell cmdlet
- O filtro de Get-PamRequest cmdlet está agora corretamente
- *O cmdlet Set-PamGroup é agora capaz de atualizar o objeto principal do grupo sombra do Diretório Ativo
- Remove-PamUser o cmdlet PowerShell falha com uma mensagem de erro pouco clara, se o utilizador estiver ligado a uma Função como candidato. Agora a validação do lado do cliente foi adicionada ao cmdlet, e a exceção devolvida foi clarificada
- As contas PAM do modo de alteração não estão expostas para configuração
    - Conta API PAM Rest
    - Conta de serviço pam component
    - Conta de serviço de monitorização PAM

#### <a name="microsoft-identity-portal"></a>Portal de Identidade da Microsoft
- *Suporte para contas de serviço geridas pelo grupo
- *Melhor suporte linguístico a um novo padrão definido
- Controlo de Escolha de Identidade, o controlo parece aumentar dinamicamente a sua largura em vez de embrulhar o texto
- Portal, os diálogos popup não são apresentados corretamente ao visualizar no Internet Explorer (IE) 10
- Os símbolos cirílicos no texto da barra de título são exibidos corretamente
- As janelas popup já não têm a barra de pergaminho extra exibida, quando vistas no Internet Explorer
- Falha na "Definição de Fluxo de Trabalho de Importação" lança corretamente uma exceção e recupera, permitindo que uma atividade da Regra de Sincronização seja adicionada à definição de fluxo de trabalho
- <httpRuntime enableVersionHeader="false" /> adicionado ao web.config padrão
- Os caracteres especiais no nome distinto já não impedem Self-Service redefinição da palavra-passe do utilizador no Diretório Ativo
- As melhorias nas frases estão devidamente localizadas no ecrã
- O Add-in MIM para o Outlook inclui uma cópia dos interop binaries do Outlook em falta

#### <a name="certificate-management"></a>Gestão de Certificados
- Renovando um cartão inteligente virtual através da App Moderna MIM CM, o utilizador recebe exceção Proibida
- *Melhor suporte linguístico a um novo padrão definido
- Pin Utility "CLM encontrou um erro ao tentar alterar o Smart Card PIN.  Número errado de Argumentos ou Atribuição de Imóveis Inválidos."
- Atualização para os Módulos da Autoridade de Certificados MIM de 4.4.1302.0 para uma construção posterior a 4.4.1459, a configuração falha
- Aplicação moderna para renovar, inscrever e substituir operações, o histórico de pedidos não contém todos os itens de estado de pedido registados como são gravados
- A Atualização Online não completa e devolve a exceção "O registo foi atualizado ou eliminado por outro utilizador."  
- O link "Certificado de Descarregamento" no Portal de Gestão de Certificados, o download do certificado (ficheiro.cer) era demasiado grande
- O Cliente A granel de Gestão de Certificados MIM trabalhará com os TLS 1.1 e TLS 1.2.  

 
## <a name="version-4417490"></a>Versão 4.4.1749.0

- Estado: 30 de novembro de 2017
- [Transferência](https://www.microsoft.com/download/details.aspx?id=56271)

### <a name="fixed-issues"></a>Problemas corrigidos
As seguintes questões foram corrigidas na versão MIM 4.4.1749.0.

#### <a name="synchronization-service"></a>Serviço de sincronização

- A rotina de reset de palavra-passe falha quando o domínio do servidor de sincronização não tem uma relação de confiança com o domínio alvo.

#### <a name="mim-service"></a>Serviço MIM

- Falha na atualização da base de dados. Uma exceção de violação de restrição de chave estrangeira é registada no registo de atualização da base de dados.
- Quando executa pedidos de redefinição de senha de autosserviço, o Serviço MIM para aleatoriamente.
- O cálculo definido não reflete a correta adesão. Não é possível eliminar uma ligação para um atributo se for referenciado num conjunto dinâmico ou num filtro de grupo.
- O Serviço MIM não funcionou para o cenário de Aprovação de Pedido com o Exchange Online onde as aprovações são respondidas através do ADD-in MIM para o Outlook.
- O atributo msidmPhoneGatePhonePhoneNumber sem um Código de País não utiliza o valor DefaultCountryCode em MFASettings.xml.
- Os membros definidos podem ser atualizados dinamicamente sem ter que depender do FIM_TemporalEventsJob.
- As Regras de Sincronização não suportam a criação de regras de fluxo de atributos para atributos cujos nomes incluam o símbolo de hash ou libra (#).
  
#### <a name="privilege-access-management"></a>Gestão de Acesso privilegiado 

- New-PAMDomainConfiguration o cmdlet PowerShell define um valor incorreto para a configuração da confiança do domínio, resultando em erro (este parâmetro de pedido desconhecido não pode ser processado).

#### <a name="microsoft-identity-portal"></a>Portal de Identidade da Microsoft

- Uma exceção é exibida no ecrã principal do Portal de Gestão de Identidade, e um botão Close também aparece.
- Botões apresentados incorretamente na janela Eliminar Item.  Este problema ocorreu no Internet Explorer, Firefox e Chrome. 
- O botão 'Procurar' sobrepõe-se ao botão 'Picker' de recursos numa janela de atividade de aprovação no fluxo de trabalho de autorização. Este problema ocorreu no Internet Explorer, Firefox e Chrome. 
- No popup de propriedades do grupo, a área do botão sobrepõe-se aos controlos de navegação listview no controlo delete Members.  Este problema ocorreu no Internet Explorer, Firefox e Chrome.
- Vários elementos de UI não são apresentados corretamente. São fixados os seguintes elementos:

    - Setas para cima e para baixo em alguns lençóis de propriedade.
    - Espaço vazio na parte inferior de algumas páginas e caixas de diálogo.
    - Sobreposições de popup desaparecidas.

- Quando utilizar o construtor de filtros em várias áreas do produto (como a Advanced Search), o construtor de filtros ficaria "preso" se o botão OK numa caixa de diálogo de valor selecionado fosse clicado sem um objeto selecionado na área de declaração de adição.
- O pop up de fluxo de novo atributo num diálogo de edição de regras de sincronização não funcionou como esperado no Chrome.
- Num ecrã de gestão de objetos (como grupos de distribuição), se vários objetos forem selecionados utilizando a caixa de verificação e os objetos tiverem nomes de visualização longos. Agora, os tamanhos do diálogo verticalmente para que o controlo não se estenda para além da extremidade do ecrã do navegador.
- Num ecrã de gestão ou lista de objetos (como Grupos de Distribuição), o controlo de itens selecionados pode subir o ecrã para estar diretamente no último objeto listado nas listas de tabelas.
- O construtor de filtros (como pesquisa avançada) no navegador Safari não é funcionacional.
- caixas de diálogo portal que exibem valores de atributos, as palavras mais curtas são distribuídas por toda a célula com muito espaço em branco entre e não sendo alinhadas à esquerda. 
- Em algumas versões do navegador, os Itens Selecionados não são atualizados quando a seleção de artigos é alterada.
- Os separadores de diálogo e o destaque da Copy to Clipboard quando navegado utilizando a tecla do separador.  
- No Internet Explorer 10, quando vê um ecrã de grelha de objetos (como grupos de distribuição), o banner "Encontre os grupos de distribuição que deseja usar a pesquisa acima" sobrepõe parte da fita do botão em vez de exibir no meio da caixa de diálogo.  
- Depois de instalar uma atualização no Portal MIM, a exibição do Portal no Internet Explorer falha.
- Quando utiliza a Pesquisa Avançada no navegador Firefox, premir a tecla de introdução num campo de valor de atributos retorna um erro.  

#### <a name="certificate-management"></a>Gestão de Certificados

- Um pedido de autoria (gestor de certificados) não pode abandonar um pedido duplicado ou esquecido por um utilizador que tenha permissão de Execução.
- Quando tenta renovar o TPM Virtual Smart Card da Aplicação Moderna, é devolvida uma exceção proibida.
- Durante algumas atividades de cartões inteligentes, as ligações existentes à base de dados CertificateManagement são deixadas abertas inesperadamente.  
- Se for experimenta uma instalação de uma atualização à Gestão de Certificados MIM (CM) antes da execução do Assistente de Configuração MIM CM, a atualização falha com uma exceção que parece não estar relacionada com o problema.
- O Assistente de Configuração MIM CM tem informações incorretas da versão do produto apresentadas e o logótipo não é apresentado corretamente.  
- Os dados exportados para um relatório de gestão de certificados mim difere dos dados do relatório.  Os dados da coluna nem sempre correspondem às rubricas das colunas.

## <a name="version-4416420"></a>Versão 4.4.1642.0

- Estado: 29 de agosto de 2017
- [Transferência](https://www.microsoft.com/download/details.aspx?id=55794)

### <a name="fixed-issues"></a>Problemas corrigidos
As seguintes questões foram corrigidas na versão MIM 4.4.1642.0.

#### <a name="synchronization-service"></a>Serviço de Sincronização

- A rotina de reset de palavra-passe falha quando o domínio do servidor de sincronização não tem uma relação de confiança com o domínio alvo.
- O filtro de importação declarado (verificando o nome distinto) não funciona corretamente quando um objeto é movido de uma unidade organizacional onde deve ser filtrado para outro onde não deve ser devido ao uso do nome antigo de um objeto fantasma.
- O Designer de Agentes de Gestão está na página "Configure Partitions and Hierarquias".
- A precedência não é transferida para o próximo objeto quando o anterior com precedência é desligado.
- Agente de gestão Sun One Procurar recipientes para crianças no servidor LDAP usando paging causa um erro se o servidor não suportar a paging.
- O tipo de objeto metaverso é alterado dinamicamente causando a colisão.

#### <a name="mim-service"></a>Serviço MIM

- A função palavra não devolve a corda vazia se a corda contiver menos do que o número de palavras.
- Ver membros UI mostra adesão incorreta quando os critérios contêm desrespeição e se a desreferencagem acontecer abaixo de outra peça de critério. 
- A AuthZ Workflow nega pedido com mensagem de erro "fluxo de trabalho não encontrado na loja de persistência do Estado".
- O fluxo de trabalho executa uma atividade de recursos enumerado para consultar a MIM e está a falhar intermitentemente.

#### <a name="privilege-access-management"></a>Gestão de Acesso privilegiado

- Get-PAMRequestToApprove comando não devolve candidatos se o aprovador não estiver no papel de aprovação.
- Os MPRs relacionados e o nó da barra de navegação PAM estão ativados mesmo quando a PAM não está instalada.
- O Serviço MIM lança exceções e regista o aviso do sincronizador de configuração do domínio para o PAM.

#### <a name="microsoft-identity-portal"></a>Portal de Identidade da Microsoft

- Ao utilizar o Firefox para aceder ao construtor de filtros no portal MIM, não é possível utilizar os controlos internos (caixas de entrega, caixas de texto, etc.). Não podem ser selecionados até que clique à direita (isto é, clique para exibir o menu de contexto).
- A Pesquisa do Portal torna-se incorretamente em algumas resoluções de ecrã. 
- O controlo do calendário em Busca Avançada é truncado.
- O uI do construtor de filtros foi quebrado - em modo de edição (elementos deslocados).
- Popup tinha tamanho fixo e os controlos de edição não eram de tamanho adequado.
- Cortes de cordas para a Noruega e algumas outras línguas no menu principal.
- URL copiado do popup não funcionando.

#### <a name="certificate-management"></a>Gestão de Certificados

- Portais de senha de autosserviço MIM.

#### <a name="reporting"></a>Relatórios

- Import-FIMReportingSchemaDefinition cmdlet para reportar falha com erro.

#### <a name="client-add-in"></a>Complemento de cliente

- A UI Language não verifica a igualdade no código do país.

### <a name="new-features-and-improvements"></a>Novas funcionalidades e melhorias
As seguintes funcionalidades e melhorias foram adicionadas na versão MIM 4.4.1642.0.

#### <a name="mim-service"></a>Serviço MIM

- Voltou a tentar nas operações de processamento de pedido mais longas (fase de validação). Não garante que o processamento de pedidos esteja concluído, mas torna os pedidos mais estáveis. A correção é desativada por defeito. Para ativar a correção, adicione sempreOnRetryRequestProcessingTransaction="true" in resourceManagementService secção de ficheiros configurar FIMService

#### <a name="certificate-management"></a>Gestão de Certificados

- As versões mais recentes do CM Server são capazes de trabalhar com bulkClient mais antigo (mas não mais do que 4.4.xxxx.0). 

#### <a name="mim-self-service-password-portals"></a>Portais de senha de autosserviço MIM 

- Mostrar aviso para QAGate se usado fornece uma resposta que contém caráter de duplo byte
- Adicionar propriedade configurável para permitir permitir/desativar a utilização do IME no formulário de registo SSPR 
- SSPR mascara as perguntas de registo de senha no Cliente Portal

## <a name="version-4415490"></a>Versão 4.4.1549.0
 
* Estado: anterior MIM 2016 SP1 Hotfix Update, de 27 de março de 2017
* Número correspondente da versão BHOLD: 6.0.36.0
* Ler mais em [https://support.microsoft.com/en-us/help/4012498](https://support.microsoft.com/en-us/help/4012498)


## <a name="version-4413020"></a>Versão 4.4.1302.0

* Estado: PACOTE DE SERVIÇOS MIM 2016 1 (SP1) de 9 de novembro de 2016
* Número correspondente da versão BHOLD: 5.0.3355.0

### <a name="updates-in-this-service-pack"></a>Atualizações neste service pack


#### <a name="mim"></a>MIM

- **Compatibilidade entre browsers do Portal do MIM para o utilizador final self-service:** neste service pack, introduzimos suporte para a maioria dos browsers principais. Os utilizadores podem agora aceder e interagir com o Portal do MIM no grupo self-service e na gestão de perfis a partir do Microsoft Edge, Chrome e Safari.

- **Suporte do Serviço MIM para Exchange Online:** o serviço MIM há muito tempo que suporta o envio e a receção de e-mails de aprovações e notificações. Antes do SP1, o MIM apenas suportava o Exchange Server ou SMTP. Com o pack de serviços 1, o Serviço MIM pode enviar e receber pedidos, bem como notificações de e-mail usando uma conta online Microsoft 365 Exchange.

- **Validação do formato de ficheiros de imagem ao carregar:** o MIM agora é capaz de validar o formato de ficheiros de imagem quando são carregados para o portal.

#### <a name="privileged-access-managementpam"></a>Privileged Access Management (PAM)

- **Suporte da floresta (bastion) “PRIV” da PAM para o nível funcional do Windows Server 2016:** o Serviço PAM do MIM pode ser configurado num ambiente com controladores de domínio em execução no nível funcional da floresta dos Serviços de Domínio do Active Directory do Windows Server 2016. Quando configurado, a permissão Kerberos do utilizador terá um prazo limitado ao tempo restante da respetiva ativação da função.

    >[!Note]
    Se optar por manter o nível funcional da floresta do Windows Server 2012 R2 no domínio CORP, recomenda-se instalar [KB 2919442](https://support.microsoft.com/en-us/kb/2919442) e [ 2919355](https://support.microsoft.com/en-us/kb/2919355) no controlador de domínio CORP.

- **Elevação da conta com privilégios para grupos exclusivos da floresta (bastion) “PRIV”:** agora, os administradores podem informar o Serviço MIM relativamente a grupos e utilizadores exclusivos da floresta “PRIV”. Este procedimento permite a estes grupos e utilizadores serem incluídos nas funções de PAM.  Em seguida, podem ser ativados para uma função e associados a grupos na floresta “PRIV”.

- **Scripts de Implementação da PAM:** os Scripts de Implementação da PAM permitem aos administradores otimizarem a instalação do ambiente de PAM.

- **Cmdlets da PAM para a configuração Silo de Políticas de Autenticação:** o service pack 1 introduz Cmdlets novos para reforçar a segurança da floresta bastion. Estes Cmdlets criam automaticamente um Silo de Políticas de Autenticação, vinculado a um Modelo de Políticas de Autenticação.

    >[!Note]
    Estes Cmdlets são executados automaticamente como parte dos scripts de implementações.



### <a name="platform-support"></a>Suporte da plataforma 

Pode encontrar informações atualizadas sobre as plataformas suportadas no documento denominado [Plataformas suportadas para o MIM 2016](../microsoft-identity-manager-2016-supported-platforms.md).  As novas plataformas suportadas neste pacote de serviços incluem o SQL Server 2016, o SharePoint 2016.


### <a name="issues-fixed-in-this-release"></a>Problemas corrigidos nesta versão


#### <a name="pam"></a>PAM
- O cmdlet New-PAMGroup não criava objetos MIM para grupos locais de domínio na floresta PRIV
- O cmdlet New-PAMDomainConfiguration falhava com uma mensagem de erro “netdom”
- O Serviço de Monitorização de PAM registava avisos de grupos na floresta PRIV


### <a name="how-to-upgrade"></a>Como atualizar

Os clientes que atualizarem para o Microsoft Identity Manager 2016 Service Pack 1 devem seguir as seguintes instruções em todos os serviços aplicáveis à sua implementação.

>[!Note]
>Os clientes com o Forefront Identity Manager 2010 R2 SP1 ou anterior têm primeiro de atualizar o respetivo ambiente para o Microsoft Identity Manager 2016, lançado em agosto de 2015, e seguir os passos abaixo.

Antes de começar

Tem de atualizar o motor de Sincronização do MIM antes de atualizar o portal e o serviço MIM.
Tem de criar uma cópia de segurança das bases de dados de Sincronização do MIM e MIMService.

1. Desinstale o componente do Microsoft Identity Manager que está a atualizar
2. Depois de concluída a desinstalação, abra a página inicial localizada no suporte de dados de instalação “FIMSplash.htm”
3. Selecione o componente do MIM a atualizar
4. Siga os avisos para continuar com a instalação
   * Instalação do Portal e do Serviço MIM: ao escolher o Exchange Online como a conta de correio, introduza o endereço de e-mail e as credenciais da conta do Exchange Online no ecrã seguinte.

## <a name="version-4322660"></a>Versão 4.3.2266.0


* Estado: MIM 2016 Hotfix de 15 de julho de 2016; clientes devem fazer upgrade para uma versão mais recente para permanecer suportado
* Ler mais em [https://support.microsoft.com/en-us/kb/3171342](https://support.microsoft.com/en-us/kb/3171342)

## <a name="version-4321950"></a>Versão 4.3.2195.0

* Status: MIM 2016 Hotfix de 20 de março de 2016, os clientes devem fazer upgrade para uma versão mais recente para se manterem apoiados
* Número correspondente da versão BHOLD: 5.0.3355.0
* Ler mais em [https://support.microsoft.com/kb/3134725](https://support.microsoft.com/kb/3134725)
    
## <a name="version-4320640"></a>Versão 4.3.2064.0

* Status MIM 2016 Hotfix de 11 de dezembro de 2015; clientes devem fazer upgrade para uma versão mais recente para permanecer suportado
* Número correspondente da versão BHOLD: 5.0.3176.0
* Ler mais em [https://support.microsoft.com/kb/3092179](https://support.microsoft.com/kb/3092179)

## <a name="version-4319350"></a>Versão 4.3.1935.0

* Status MIM 2016 GA de 6 de agosto de 2015; clientes devem fazer upgrade para uma versão mais recente para permanecer suportado
* Número correspondente da versão BHOLD: 5.0.3079.0

