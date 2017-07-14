---
title: "Melhores Práticas do Microsoft Identity Manager 2016 | Documentos da Microsoft"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 05/11/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.translationtype: MT
ms.sourcegitcommit: 3bb89e2c86724e6f6d32e4043fa37da74e2b7b24
ms.openlocfilehash: a0d00c7e5d99e43d3fb0b3011a3851f7194bfdf2
ms.contentlocale: pt-pt
ms.lasthandoff: 07/10/2017


---


# Melhores Práticas do Microsoft Identity Manager 2016
<a id="microsoft-identity-manager-2016-best-practices" class="xliff"></a>

Este tópico descreve as melhores práticas para implementar e operar o Microsoft Identity Manager 2016 (MIM)

## Configuração do SQL
<a id="sql-setup" class="xliff"></a>
>[!NOTE]
As seguintes recomendações para configurar um servidor a executar o SQL presumem uma instância de SQL dedicada para o FIMService e uma instância de SQL dedicada para a base de dados FIMSynchronizationService. Se estiver a executar o FIMService num ambiente consolidado, terá de fazer os ajustes adequados para a sua configuração.

A configuração do SQL Server é fundamental para o desempenho ideal do sistema. Atingir o desempenho ideal do MIM em implementações de grande escala depende da aplicação das melhores práticas para um servidor a executar o SQL. Para obter mais informações, veja os seguintes tópicos sobre as melhores práticas do SQL:

-   [Storage Top 10 Best Practices (Dez Melhores Práticas de Armazenamento)](http://go.microsoft.com/fwlink/?LinkID=183663)

-   [Optimizing tempdb Performance (Otimizar o Desempenho de tempdb)](http://go.microsoft.com/fwlink/?LinkID=188267)

-   [SQL Server Best Practices Article (Artigo de Melhores Práticas do SQL Server)](http://go.microsoft.com/fwlink/?LinkID=188268)

-   [Reorganizing and Rebuilding Indexes (Reorganizar e Reconstruir Índices)](http://go.microsoft.com/fwlink/?LinkID=188269)

### Pré-dimensionar ficheiros de registo e dados
<a id="presize-data-and-log-files" class="xliff"></a>

Não confie no aumento automático. Em vez disso, faça a gestão do crescimento destes ficheiros manualmente. Pode deixar o aumento automático ativado por motivos de segurança, mas deve gerir proativamente o crescimento dos ficheiros de dados. Para obter tamanhos de exemplo da base de dados do MIM, veja [FIM Capacity Planning Guide (Guia de Planeamento de Capacidade do FIM)](http://go.microsoft.com/fwlink/?LinkID=185246).

### Para pré-dimensionar os ficheiros de registo e dados do SQL
<a id="to-presize-sql-data-and-log-files" class="xliff"></a>

1.  Inicie o SQL Server Management Studio.

2.  Navegue para a base de dados FIMService, clique com o botão direito do rato em FIMService e, em seguida, clique em Propriedades.

3.  Na página Ficheiros, expanda os ficheiros de base de dados para o tamanho necessário.

### Isolar o registo dos ficheiros de dados
<a id="isolate-log-from-data-files" class="xliff"></a>

Siga as melhores práticas do SQL Server para isolar os ficheiros de registo de dados e transações para as bases de dados em discos físicos separados.

Criar ficheiros tempdb adicionais

Para um desempenho ideal, recomendamos que crie um ficheiro de dados por núcleo de CPU no ficheiro tempdb.

### Para criar ficheiros tempdb adicionais
<a id="to-create-additional-tempdb-files" class="xliff"></a>

1.  Inicie o SQL Server Management Studio.

2.  Navegue para a base de dados tempdb nas Bases de Dados do Sistema, clique com o botão direito do rato em tempdb e, em seguida, clique em Propriedades.

3.  Na página Ficheiros, crie um ficheiro de dados para cada núcleo de CPU. Certifique-se de que separa os ficheiros de registo e dados tempdb em spindles e unidades diferentes.

### Garantir o espaço adequado para os ficheiros de registo
<a id="ensure-adequate-space-for-log-files" class="xliff"></a>

É importante compreender os requisitos de disco do seu modelo de recuperação. O modo de recuperação Simples pode ser adequado durante o carregamento do sistema inicial, para limitar a utilização do espaço em disco, mas os dados criados após a cópia de segurança mais recente serão expostos a perda de dados. Ao utilizar o modo de recuperação Completo, tem de gerir a utilização do disco através de cópias de segurança que incluem cópias de segurança frequentes do registo de transações para impedir a utilização elevada de espaço em disco. Para obter mais informações, veja [Recovery Model Overview (Descrição Geral do Modelo de Recuperação)](http://go.microsoft.com/fwlink/?LinkID=185370).

### Limitar a memória do SQL Server
<a id="limit-sql-server-memory" class="xliff"></a>

Dependendo da quantidade de memória que tem no SQL Server e consoante partilhe ou não o SQL Server com outros serviços (isto é, o Serviço do MIM 2016 e o Serviço de Sincronização do MIM 2016), é recomendável restringir o consumo de memória do SQL. Pode fazê-lo através dos seguintes passos.

1.  Inicie o SQL Server Enterprise Manager.

2.  Selecione Nova Consulta.

3.  Execute a seguinte consulta:

  ```SQL
  USE master

  EXEC sp_configure 'show advanced options', 1

  RECONFIGURE WITH OVERRIDE

  USE master

  EXEC sp_configure 'max server memory (MB)', 12000--- max=12G RECONFIGURE
  WITH OVERRIDE
  ```

  Este exemplo reconfigura o SQL Server para utilizar menos de 12 gigabytes (GB) de memória.

4.  Verifique a definição ao utilizar a seguinte consulta:

  ```SQL
  USE master

  EXEC sp_configure 'max server memory (MB)'--- verify the setting

  USE master

  EXEC sp_configure 'show advanced options', 0

  RECONFIGURE WITH OVERRIDE
  ```

### Configuração de cópia de segurança e recuperação
<a id="backup-and-recovery-configuration" class="xliff"></a>

Em geral, deve criar cópias de segurança da base de dados de acordo com a política de cópias de segurança da sua organização. Se as cópias de segurança de registos incrementais não forem planeadas, a base de dados deve ser definida para o modo de recuperação Simples. Certifique-se de que compreende as implicações dos diferentes modelos de recuperação antes de implementar a sua estratégia de cópia de segurança, bem como os requisitos de espaço em disco destes modelos. O modelo de recuperação completo necessita de cópias de segurança de registos frequentes para evitar a utilização elevada do espaço em disco. Para obter mais informações, veja [Recovery Model Overview (Descrição Geral do Modelo de Recuperação)](http://go.microsoft.com/fwlink/?LinkID=185370) e [FIM 2010 Backup and Restore Guide (Guia de Criação de Cópias de Segurança e de Restauro do FIM 2010)](http://go.microsoft.com/fwlink/?LinkID=165864).

## Criar uma conta de Administrador de Cópias de Segurança para o FIMService após a instalação
<a id="create-a-backup-administrator-account-for-the-fimservice-after-installation" class="xliff"></a>


>[!IMPORTANT]
Os membros do conjunto de Administradores do FIMService têm permissões exclusivas essenciais para a operação de implementação do FIM. Se não conseguir iniciar sessão como parte do conjunto de Administradores, a única resolução é reverter para uma cópia de segurança anterior do sistema. Para atenuar esta situação, recomendamos que adicione outros utilizadores ao conjunto Administrativo do FIM como parte da sua configuração de pós-instalação.

## Serviço FIM
<a id="fim-service" class="xliff"></a>


### Configurar a caixa de correio de serviço do Exchange do Serviço FIM
<a id="configuring-fim-service-service-exchange-mailbox" class="xliff"></a>

Seguem-se as melhores práticas para configurar o Microsoft Exchange Server para a conta de serviço do Serviço MIM 2016.

- Configure a conta de serviço para que possa aceitar correio apenas de endereços de e-mail internos. Especificamente, a caixa de correio de conta de serviço nunca deve conseguir receber correio de servidores SMTP externos.

#### Configurar a conta de serviço
<a id="to-configure-the-service-account" class="xliff"></a>

1.  Na Consola de Gestão do Exchange, selecione a **Conta de serviço do Serviço FIM**.

2.  Selecione Propriedades, selecione Definições de Fluxo de Correio e, em seguida, selecione **Restrições de Entrega de Correio.**

3.  Selecione a caixa de verificação **Exigir a autenticação de todos os remetentes**.

Para obter mais informações, veja [Configure Message Delivery Restrictions (Configurar Restrições de Entrega da Mensagem)](http://go.microsoft.com/fwlink/?LinkID=183625).

-   Configure a conta de serviço para que rejeite correio com um tamanho superior a 1 MB. Siga as melhores práticas para [Configure Message Size Limits (Configurar Limites de Tamanho de Mensagem)](http://go.microsoft.com/fwlink/?LinkID=183626) para uma Caixa de Correio ou uma Pasta Pública com Capacidade de Correio.

-   Configure a conta de serviço para que tenha uma quota de armazenamento de caixa de correio de 5 GB. Para obter os resultados ideais, siga as melhores práticas indicadas em [Configure Storage Quotas for a Mailbox (Configurar Quotas de Armazenamento para uma Caixa de Correio)](http://go.microsoft.com/fwlink/?LinkID=156929).

## Portal do MIM
<a id="mim-portal" class="xliff"></a>


### Desativar a indexação do SharePoint
<a id="disable-sharepoint-indexing" class="xliff"></a>

Recomendamos que desative a indexação do Microsoft Office SharePoint®. Os documentos não precisam de ser indexados e a indexação causa várias entradas de registo de erro e potenciais problemas de desempenho com o FIM 2010. Para desativar a indexação do SharePoint

1.  No servidor que aloja o Portal do MIM 2016, clique em Iniciar.

2.  Clique em Todos os Programas.

3.  Na lista Todos os Programas, clique em Ferramentas Administrativas.

4.  Em Ferramentas Administrativas, clique em Administração Central do SharePoint.

5.  Na página Administração Central, clique em Operações.

6.  Na página Operações, em Configuração Global, clique em Definições de Tarefa do Temporizador.

7.  Na página Definições de Tarefa do Temporizador, clique em Atualização da Pesquisa do SharePoint Services.

8.  Na página Editar Tarefa do Temporizador, clique em Desativar.

## Carregamento de Dados Inicial do MIM 2016
<a id="mim-2016-initial-data-load" class="xliff"></a>

Esta secção indica uma série de passos para aumentar o desempenho do carregamento de dados inicial do sistema externo para o FIM 2010. É importante compreender que vários destes passos são temporários durante o preenchimento inicial do sistema e devem ser repostos após a respetiva conclusão. Esta é uma operação única e não é uma sincronização contínua.

>[!NOTE]
Para obter mais informações sobre a sincronização de utilizadores entre o FIM 2010 e o Active Directory Domain Services (AD DS), veja [How do I Synchronize Users from Active Directory to FIM (Como Posso Sincronizar Utilizadores do Active Directory com o FIM)](http://go.microsoft.com/fwlink/?LinkID=188277) na documentação do FIM.

>[!IMPORTANT]
Certifique-se de que aplicou as melhores práticas abrangidas na secção de configuração do SQL deste guia.                                                                                                                                                      |

### Passo 1: configurar o SQL Server para o carregamento de dados inicial
<a id="step-1-configure-the-sql-server-for-initial-data-load" class="xliff"></a>
Quando planeia carregar inicialmente uma grande quantidade de dados, pode reduzir o tempo de preenchimento da base de dados ao desativar temporariamente a pesquisa em texto completo e ao ativá-la novamente após a conclusão da exportação no agente de gestão do MIM 2016 (FIM MA).

Para desativar temporariamente a pesquisa em texto completo:

1.  Inicie o SQL Server Management Studio.

2.  Selecione Nova Consulta.

3.  Execute as seguintes instruções SQL:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING =
MANUAL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = MANUAL
```

É importante que compreenda os requisitos de disco do seu modelo de recuperação do SQL Server. Dependendo da sua agenda de cópias de segurança, pode ponderar a utilização do modo de recuperação Simples durante o carregamento do sistema inicial para limitar a utilização do espaço em disco, mas tem de compreender as implicações numa perspetiva de perda de dados.
Ao utilizar o modo de recuperação Completo, tem de gerir a utilização do disco através de cópias de segurança que incluem cópias de segurança frequentes do registo de transações para impedir a utilização elevada de espaço em disco.

>[!IMPORTANT]
Não implementar estes procedimentos pode resultar na utilização elevada do espaço em disco, fazendo com que fique sem espaço em disco. Pode encontrar detalhes adicionais sobre este tópico em [Recovery Model Overview (Descrição Geral do Modelo de Recuperação)](http://go.microsoft.com/fwlink/?LinkID=185370). [O FIM Backup and Restore Guide (Guia de Criação de Cópias de Segurança e de Restauro do FIM)](http://go.microsoft.com/fwlink/?LinkID=165864) contém informações adicionais.

### Passo 2: aplicar a configuração do MIM mínima necessária durante o processo de carregamento
<a id="step-2-apply-the-minimum-necessary-mim-configuration-during-the-load-process" class="xliff"></a>

Durante o processo de carregamento inicial, só deve aplicar a configuração mínima necessária para a sua configuração do FIM para as suas regras de política de gestão (MPRs) e definições de conjunto. Após a conclusão do carregamento de dados, crie os passos adicionais necessários para a sua implementação. Utilize a definição Executar na Atualização da Política nos fluxos de trabalho da ação para aplicar essas políticas de forma retroativa aos dados carregados.

### Passo 3: configurar e preencher o Serviço FIM com dados de identidade externa
<a id="step-3-configure-and-populate-the-fim-service-with-external-identity-data" class="xliff"></a>


Neste passo, deve seguir os procedimentos descritos em How Do I Synchronize Users from Active Directory (Como Posso Sincronizar Utilizadores do Active Directory)

Guia do Domain Services para o FIM para configurar e sincronizar o seu sistema com utilizadores do Active Directory. Se precisar de sincronizar informações do grupo, os procedimentos desse processo são descritos no guia How Do I Synchronize Groups from Active Directory Domain Services to FIM (Como Posso Sincronizar Grupos do Active Directory Domain Services para o FIM).

#### Sequências de sincronização e exportação
<a id="synchronization-and-export-sequences" class="xliff"></a>

Para otimizar o desempenho, execute uma exportação após uma execução de sincronização que resulta num grande número de operações de exportação pendentes num espaço conector.

Em seguida, execute uma confirmação de execução de importação no agente de gestão associado ao espaço conector afetado. Por exemplo, quando precisar de executar perfis de execução de sincronização em vários agentes de gestão como parte de um carregamento de dados inicial, deve executar uma exportação seguida de uma importação delta após cada execução de sincronização individual.

Para cada agente de gestão de origem que faz parte do seu ciclo de inicialização, efetue os seguintes passos:

1.  Importação completa num agente de gestão de origem.

2.  Sincronização completa no agente de gestão de origem.

3.  Exportação em todos os agentes de gestão de destino afetados com operações de exportação faseadas.

4.  Importação delta em todos os agentes de gestão de destino afetados com operações de exportação faseadas.

### Passo 4: aplicar a configuração completa do MIM
<a id="step-4-apply-your-full-mim-configuration" class="xliff"></a>


Assim que concluir o seu carregamento de dados inicial, deve aplicar a configuração completa do MIM à sua implementação.

Dependendo dos cenários, isto poderá incluir a criação de conjuntos adicionais, MPRs e fluxos de trabalho. Para todas as políticas que precisa de aplicar de forma retroativa a todos os objetos existentes no sistema, utilize a definição Executar Na Atualização da Política nos fluxos de trabalho da ação para aplicar essas políticas de forma retroativa aos dados carregados.

### Passo 5: reconfigurar o SQL para as definições anteriores
<a id="step-5-reconfigure-sql-to-previous-settings" class="xliff"></a>


Lembre-se de alterar a definição do SQL para as definições normais. Isto inclui:

-   Ativar a pesquisa em texto completo

-   Atualizar a sua política de cópias de segurança de acordo com a política da sua organização

Assim que concluir o carregamento de dados inicial, tem de ativar novamente a pesquisa em texto completo. Execute as seguintes instruções SQL para ativar novamente a pesquisa em texto completo:

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = AUTO

ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = AUTO
```

Se tiver de mudar para o modo de recuperação Simples, certifique-se de que reconfigura a sua agenda de cópias de segurança de acordo com a política de cópias de segurança da sua organização. Estão disponíveis detalhes adicionais de agendas de cópias de segurança do FIM em [FIM Backup and Restore Guide (Guia de Criação de Cópias de Segurança e de Restauro do FIM)](http://go.microsoft.com/fwlink/?LinkID=165864).

## Migração de Configuração
<a id="configuration-migration" class="xliff"></a>


### Evitar alterar os nomes a apresentar
<a id="avoid-changing-display-names" class="xliff"></a>

Para muitos tipos de objeto, como os MPRs, o script syncproduction.ps1 utiliza o nome a apresentar como o único atributo âncora entre dois sistemas. Por conseguinte, uma alteração ao nome a apresentar do MPR existente resulta na eliminação do MPR existente, seguido da criação de um novo MPR. Este resultado ocorre porque o processo de migração não pode associar com êxito MPRs cujos critérios de união tenham sido alterados. Para evitar este problema, pode vincular um atributo personalizado a todos os tipos de objeto de configuração e utilizar esse atributo como o critério de união. Esta ação permite-lhe modificar os nomes a apresentar sem afetar o processo de migração.

### Evitar alterar os conteúdos dos ficheiros intermédios
<a id="avoid-changing-the-content-of-intermediate-files" class="xliff"></a>

Enquanto o formato de ficheiro e a interface de programação da aplicação (API) dos objetos de nível baixo forem públicos e as manipulações forem suportadas por programadores, não recomendamos que altere os conteúdos dos formatos intermédios durante a migração. No entanto, poderá ser necessário remover ImportObjects inteiros do ficheiro changes.xml ou efetuar operações de localização e substituição no ficheiro pilot.xml para substituir informações de Sistema de Nomes de Domínio (DNS) piloto ou números de versão para produção de informações de DNS.

### Certifique-se de que o número da versão está correto no ficheiro pilot.xml ao migrar entre versões
<a id="ensure-that-the-version-number-is-correct-in-pilotxml-when-migrating-across-versions" class="xliff"></a>

Ainda que a migração entre números de versão não seja recomendada nem suportada, pode fazê-lo ao substituir o número da versão piloto pelo número da versão de produção no ficheiro pilot.xml. Especificamente, os objetos WorkflowDefinition e

ActivityInformationConfiguration exigem o número de versão para referir precisamente as atividades de fluxo de trabalho no ambiente de produção. A não substituição do número de versão resulta no cmdlet Compare-FIMConfig ao identificar as diferenças entre os atributos XOML (Extensible Object Markup Language) em WorkflowDefinitions e ao migrar o número de versão piloto. O Serviço FIM de produção pode falhar ao iniciar atividades do fluxo de trabalho com o número de versão incorreto.

### Evitar referências cíclicas
<a id="avoid-cyclic-references" class="xliff"></a>

Em geral, as referências cíclicas não são recomendadas numa configuração do MIM.
No entanto, por vezes os ciclos ocorrem quando o Conjunto A se refere ao Conjunto B e o Conjunto B também se refere ao Conjunto A. Para evitar problemas com referências cíclicas, deve alterar a definição do Conjunto A ou do Conjunto B para não se referirem um outro. Em seguida, reinicie o processo de migração. Se tiver referências cíclicas e o cmdlet Compare-FIMConfig resultar num erro, é necessário interromper o ciclo manualmente. Como o cmdlet Compare-FIMConfig apresenta uma lista de alterações por ordem de precedência, é necessário que não exista um ciclo entre as referências de objetos de configuração.

## Segurança
<a id="security" class="xliff"></a>

### Conta de MA do MIM
<a id="mim-ma-account" class="xliff"></a>

A conta de MA do MIM não é considerada uma conta de serviço e deverá ser uma conta de utilizador normal. A conta tem de conseguir iniciar sessão localmente para que a conta de serviço do Serviço de Sincronização do FIM consiga representá-la.

Para permitir que a conta de MA do MIM inicie sessão localmente

1.  Clique em Iniciar, clique em Ferramentas Administrativas e, em seguida, clique em Política de Segurança Local.

2.  Abra o nó Políticas Locais e, em seguida, clique em Atribuição de Direitos de Utilizador.

3.  Na política Permitir início de sessão local, certifique-se de que a conta de MA do FIM é especificada explicitamente ou adicione-a a um dos grupos que já tenha acesso.

### Contas de Serviços FIM e Serviço de Sincronização do FIM
<a id="fim-synchronization-service-and-fim-services-accounts" class="xliff"></a>

Para configurar os servidores a executar componentes do servidor MIM de uma forma segura, as contas de serviço devem ser restritas. Através do procedimento anterior para ativar a conta de MA do MIM, defina as seguintes restrições nas contas de Serviço FIM e Serviço de Sincronização do FIM:

-   Negar o início de sessão como uma tarefa de lote

-   Negar o início de sessão localmente

-   Negar o acesso a este computador a partir da rede

As contas de serviço não devem ser um membro do grupo de administradores locais.

A conta de serviço do Serviço de Sincronização do FIM não deve ser um membro dos grupos de segurança utilizados para controlar o acesso ao Serviço de Sincronização do FIM (grupos que comecem por FIMSync, por exemplo, FIMSyncAdmins e por aí adiante).

>[!IMPORTANT]
 Se selecionar as opções para utilizar a mesma conta para ambas as contas de serviço e separar o Serviço FIM e o Serviço de Sincronização do FIM, não pode definir a opção Negar acesso para este computador a partir da rede no servidor do Serviço de Sincronização de MMS. Se o acesso fosse negado, isso iria proibir o Serviço FIM de contactar o Serviço de Sincronização do FIM para alterar a configuração e gerir as palavras-passe.

### A reposição de palavra-passe implementada em computadores semelhantes a quiosques deve definir segurança local para limpar o ficheiro de paginação de memória virtual
<a id="password-reset-deployed-to-kiosk-like-computers-should-set-local-security-to-clear-virtual-memory-pagefile" class="xliff"></a>

Ao implementar a reposição de palavra-passe do FIM numa estação de trabalho destinada a ser um quiosque, recomendamos que a definição de política de segurança local Encerrar: limpa o ficheiro de paginação de memória virtual esteja ativada para garantir que as informações confidenciais da memória do processo não estão disponíveis para utilizadores não autorizados.

### Implementar o SSL para o Portal do FIM
<a id="implementing-ssl-for-the-fim-portal" class="xliff"></a>

Recomendamos vivamente que utilize o SSL (Secure Sockets Layer) no servidor do Portal do FIM para proteger o tráfego entre os clientes e o servidor.

Para implementar o SSL:

1.  No servidor do Portal do MIM, abra o Gestor do IIS.

2.  Clique no nome do computador local.

3.  Clique em Certificados de Servidor.

4.  Clique em Criar Requisição de Certificado.

5.  Na caixa de texto Nome Comum, introduza o nome do servidor.

6.  Clique em Seguinte e clique novamente em Seguinte.

7.  Guarde o ficheiro numa localização. Terá de aceder a esta localização nos passos subsequentes.

8.  No Windows Internet Explorer®, navegue até https://servername/certsrv. Substitua o nome do servidor pelo nome do servidor a emitir certificados.

9.  Clique em Pedir um Novo Certificado.

10. Clique em Submeter um Pedido Avançado.

11. Clique em Submeter um Pedido de Certificado ao utilizar um ficheiro codificado com base 64.

12. Cole os conteúdos do ficheiro que guardou no passo anterior.

13. Em Modelo de Certificado, selecione Servidor Web.

14. Clique em Submeter.

15. Guarde o certificado no seu ambiente de trabalho.

16. Em Gestor do IIS, clique em Concluir Pedido de Certificação.

17. Aponte o Gestor do IIS para o certificado que acabou de guardar no ambiente de trabalho.

18. Em Nome amigável, escreva o nome do servidor.

19. Clique em Sites e, em seguida, selecione SharePoint – 80.

20. Clique em Enlaces e, em seguida, clique em Adicionar.

21. Selecione https.

22. Para obter o certificado, selecione aquele que tem o mesmo nome que o servidor (este é o certificado que acabou de importar).

23. Clique em OK.

24. Remova o enlace HTTP.

25. Clique em Definições de SSL e, em seguida, selecione Exigir SSL.

26. Guarde as definições.

27. Clique em Iniciar, clique em Tarefas Administrativas e, em seguida, clique em Administração Central do SharePoint 3.0.

28. Clique em Operações e, em seguida, clique em Mapeamentos de Acesso Alternativos.

29. Clique em http://servername.

30. Altere http://servername para https://servername e, em seguida, clique em OK.

31. Clique em Iniciar, clique em Executar, escreva iisreset e, em seguida, clique em OK.

## Desempenho
<a id="performance" class="xliff"></a>

Para obter uma configuração de desempenho ideal:

-   Aplique as melhores práticas de configuração do SQL conforme descrito na secção de configuração do SQL neste documento.

-   Desative a Indexação do SharePoint no site do Portal do FIM 2010 R2. Para obter mais informações, veja a secção Desativar a indexação do SharePoint neste documento.

## Melhores Práticas de Funcionalidades Específicas (Quero remover isto e fechar esta secção e apenas ter as funcionalidades específicas ao nível de cabeçalho 2 versus 3)
<a id="feature-specific-best-practices--i-want-to-remove-this-and-collapse-this-section-and-just-have-the-specific-features-at-header-2-level-versus-3" class="xliff"></a>


### Gestão de Pedidos
<a id="request-management" class="xliff"></a>

Por predefinição, o MIM 2016 remove objetos de sistema expirados, que inclui pedidos concluídos com aprovações associadas, respostas de aprovação e instâncias de fluxo de trabalho num intervalo de 30 dias. Se a sua organização precisar de um histórico de pedidos mais longo, deve exportar pedidos do MIM e armazená-los numa base de dados auxiliar para os preservar para além do período de 30 dias. Apesar de o período de eliminação de pedidos de 30 dias ser configurável, expandir este período poderá afetar negativamente o desempenho devido a objetos adicionais no sistema.

### Regras de Política de Gestão
<a id="management-policy-rules" class="xliff"></a>

#### Utilizar o tipo de MPR adequado
<a id="use-the-appropriate-mpr-type" class="xliff"></a>

O MIM fornece dois tipos de MPRs, Pedido e Transição de Conjunto:

-   MPR de Pedido (RMPR)

 - Utilizado para definir a política de controlo de acesso (autenticação, autorização e ação) para as operações Criar, Ler, Atualizar ou Eliminar (CRUD) relativamente aos recursos.
 - Aplicado quando uma operação CRUD é emitida relativamente a um recurso de destino no FIM.
   - Confinado pelos critérios de correspondência definidos na regra, isto é, a que pedidos CRUD se aplica a regra.


-   MPR de Transição de Conjunto (TMPR)
 - Utilize para definir políticas independentemente da forma como o objeto entrou no estado atual representado pelo Conjunto de Transição. Utilize o TMPR para políticas de direitos de modelos.
 - Aplicado quando um recurso entra ou sai de um conjunto associado.
 - Confinado aos membros do conjunto.

>[NOTA] Para obter detalhes adicionais, veja [Designing Business Policy Rules (Criar Regras de Políticas Empresariais)](http://go.microsoft.com/fwlink/?LinkID=183691).

#### Ativar MPRs apenas consoante necessário
<a id="only-enable-mprs-as-necessary" class="xliff"></a>

Utilize o princípio do menor privilégio ao aplicar a sua configuração. Os MPRs controlam a política de acesso à implementação do FIM. Permita apenas as funcionalidades utilizadas pela maioria dos seus utilizadores. Por exemplo, nem todos os utilizadores utilizam o FIM para a gestão de grupos, pelo que os MPRs de gestão de grupos associados devem ser desativados. Por predefinição, o FIM é fornecido com a maioria das permissões de não administrador desativadas.

#### Duplicar MPRs incorporados em vez de modificar diretamente
<a id="duplicate-built-in-mprs-instead-of-directly-modifying" class="xliff"></a>

Quando precisar de modificar os MPRs incorporados, deve criar um novo MPR com a configuração necessária e desativar o MPR incorporado. Esta ação garante que as alterações futuras aos MPRs incorporados que são introduzidas através do processo de atualização não afetam negativamente a configuração do seu sistema.

#### As permissões de utilizador final devem utilizar listas de atributos explícitos confinadas às necessidades empresariais dos utilizadores
<a id="end-user-permissions-should-use-explicit-attribute-lists-scoped-to-users-business-needs" class="xliff"></a>

Utilizar listas de atributos explícitos ajuda a impedir a concessão acidental de permissões a utilizadores não privilegiados quando os atributos são adicionados a objetos.
Os administradores devem ter de conceder acesso explicitamente a novos atributos em vez de tentar remover o acesso.

O acesso aos dados deve ser confinado às necessidades empresariais dos utilizadores. Por exemplo, os membros do grupo não devem ter acesso ao atributo de filtro do grupo do qual são membros. O filtro pode revelar inadvertidamente dados organizacionais a que o utilizador não teria acesso normalmente.

#### Os MPRs devem refletir permissões eficazes no sistema
<a id="mprs-should-reflect-effective-permissions-in-the-system" class="xliff"></a>

Evite conceder permissões a atributos que o utilizador nunca pode utilizar. Por exemplo, não deve conceder permissão para modificar atributos de recursos principais tal como objectType. Apesar do MPR, qualquer tentativa de modificar o tipo de recurso após a criação é negada pelo sistema.

#### As permissões Ler devem ser separadas das permissões Modificar e Criar quando utilizar atributos explícitos em MPRs
<a id="read-permissions-should-be-separate-from-modify-and-create-permissions-when-using-explicit-attributes-in-mprs" class="xliff"></a>

Ao indicar explicitamente atributos em MPRs, os atributos necessários para Criar e Modificar são normalmente diferentes dos disponíveis para Ler. Por exemplo, a permissão Ler pode ser concedida através de atributos do sistema, como Creator ou objectId, enquanto as permissões Criar ou Modificar não podem ser especificadas para atributos do sistema.

#### As permissões Criar devem ser separadas das permissões Modificar ao utilizar atributos explícitos em regras
<a id="create-permissions-should-be-separate-from-modify-permissions-when-using-explicit-attributes-in-rules" class="xliff"></a>

A operação Criar requer que o utilizador selecione objectType como parte da operação. Este é um atributo de sistema principal que não pode ser modificado após uma operação Criar.

#### Utilizar um MPR de pedido para todos os atributos com os mesmos requisitos de acesso
<a id="use-one-request-mpr-for-all-attributes-with-the-same-access-requirements" class="xliff"></a>

Para os atributos com os mesmos requisitos de acesso que não se espera que sejam alterados, pode combiná-los num único MPR de pedido para obter uma maior eficiência.

#### Evitar fornecer acesso sem restrições, mesmo a grupos principais selecionados
<a id="avoid-giving-unrestricted-access-even-to-selected-principal-groups" class="xliff"></a>

No FIM, as permissões são definidas como uma asserção positiva. Como o FIM não suporta permissões Negar, fornecer acesso sem restrições a um recurso torna mais complicado fornecer exclusões nas permissões. Como melhor prática, conceda apenas as permissões necessárias.

>[!NOTE]
Segue-se abaixo a secção de elegibilidades. Como posso intercalar para evitar criar cabeçalhos de nível 5
#### Utilizar TMPRs para definir direitos personalizados
<a id="use-tmprs-to-define-custom-entitlements" class="xliff"></a>

Utilize MPRs de Transição de Conjunto (TMPRs) em vez de RMPRs para definir direitos personalizados.
Os TMPRs fornecem um modelo baseado no estado para atribuir ou remover direitos com base na associação nos Conjuntos de Transições definidos, ou funções, e as atividades de fluxo de trabalho complementares. Os TMPRs devem ser sempre definidos em pares, um para recursos em transição para dentro e um para recursos em transição para fora. Além disso, cada MPR de transição deve conter fluxos de trabalho separados para atividades de aprovisionamento e desaprovisionamento.

>[!NOTE]
Qualquer fluxo de trabalho de desaprovisionamento deve garantir que o atributo Executar na Atualização da Política está definido para verdadeiro.

#### Ativar o MPR de Entrada de Transição de Conjunto em último lugar
<a id="enable-the-set-transition-in-mpr-last" class="xliff"></a>

Ao criar um par de TMPR, ative o MPR de Entrada de Transição de Conjunto em último lugar. Esta ordem assegura que os recursos não permanecem com os direitos se forem adicionados e removidos do conjunto enquanto o MPR de Entrada está ativado, mas antes de o MPR de Saída ser ativado.

#### Os fluxos de trabalho no TMPR devem verificar o estado do recurso de destino em primeiro lugar
<a id="workflows-in-tmpr-should-check-target-resource-state-first" class="xliff"></a>

Os fluxos de trabalho de aprovisionamento devem determinar se o recurso de destino já foi aprovisionado de acordo com os direitos. Em caso afirmativo, não deve fazer nada.

Os fluxos de trabalho de desaprovisionamento devem determinar se o recurso de destino já foi aprovisionado. Em caso afirmativo, deve desaprovisionar o recurso de destino.
Caso contrário, não deve fazer nada.

#### Selecionar Executar na Atualização da Política para TMPRs
<a id="select-run-on-policy-update-for-tmprs" class="xliff"></a>

Esta ação garante que o comportamento de aprovisionamento correto é aplicado quando as atualizações de política são implementadas e é utilizado o sinalizador Executar na Atualização da Política em fluxos de trabalho da ação associados aos TMPRs. Esta ação garante que as alterações nas definições de política aplicam os fluxos de trabalho da ação a novos membros do Conjunto de Transição.

#### Evitar associar o mesmo direito a dois Conjuntos de Transições diferentes
<a id="avoid-associating-the-same-entitlement-with-two-different-transition-sets" class="xliff"></a>

Associar os mesmos direitos a dois Conjuntos de Transições diferentes pode causar uma revogação e concessão desnecessária de direitos se o recurso for movido de um conjunto para outro. Como melhor prática, certifique-se de que um conjunto contém todos os recursos que requerem os direitos associados. Esta ação garante uma relação unívoca entre o Conjunto de Transições e os direitos a conceder o fluxo de trabalho.

#### Utilizar uma sequência adequada de operações ao remover direitos no sistema
<a id="use-an-appropriate-sequence-of-operations-when-removing-entitlements-in-the-system" class="xliff"></a>

A ordem dos passos efetuados ao remover direitos no sistema pode resultar em dois resultados operacionais diferentes. Certifique-se de que compreende que ordem se aplica ao efeito que pretende.

Para remover um direito do sistema (e revogá-lo de todos os membros que têm atualmente os direitos):

1.  Desative o TMPR de Entrada. Esta ação evita novas concessões.

2.  Elimine o filtro Conjunto de Transição ou altere-o para que o conjunto esteja vazio. Isto faz com que todos os membros existentes transitem para fora e aplica a política de transição de saída, incluindo os fluxos de trabalho de desaprovisionamento configurados associados aos direitos.

3.  Desative o TMPR de Saída.

Para remover um direito mas manter os membros atuais (por exemplo, deixar de utilizar o FIM para gerir os direitos):

1.  Desative o TMPR de Entrada. Esta ação evita novas concessões.

2.  Desative o TMPR de Saída.

3.  Elimine o filtro Conjunto de Transição ou altere-o para que o conjunto esteja vazio. Como o conjunto já não está ligado a um TMPR, os fluxos de trabalho de desaprovisionamento não são aplicados.

### Conjuntos
<a id="sets" class="xliff"></a>

Ao aplicar as melhores práticas para conjuntos, tem de considerar o impacto das otimizações na capacidade de gestão e facilidade de administração futura.
Devem ser efetuados os testes adequados à escala de produção esperada para identificar o equilíbrio certo entre o desempenho e a capacidade de gestão antes de aplicar estas recomendações.

>[!NOTE]
As seguintes diretrizes aplicam-se a conjuntos dinâmicos e grupos dinâmicos.


#### Minimizar a utilização de aninhamento dinâmico
<a id="minimize-the-use-of-dynamic-nesting" class="xliff"></a>

Isto refere-se ao filtro de um conjunto a referenciar o atributo ComputedMember de outro conjunto. Um motivo comum para aninhar conjuntos é evitar duplicar uma condição de associação em vários conjuntos. Ainda que esta abordagem possa resultar numa melhor capacidade de gestão dos conjuntos, existe um compromisso de desempenho. Pode otimizar o desempenho ao duplicar as condições de associações de um conjunto aninhado em vez de aninhar o próprio conjunto.

Poderá encontrar casos em que não pode evitar aninhar conjuntos para satisfazer um requisito funcional. Estas são as situações principais em que deve aninhar conjuntos. Por exemplo, para definir o conjunto de todos os grupos sem proprietários Empregados a Tempo Inteiro, o aninhamento de conjuntos tem de ser utilizado conforme se segue: `/Group[not(Owner =
/Set[ObjectID = ‘X’]/ComputedMember]`, em que "X" é o ObjectID do conjunto de Todos os Empregados a Tempo Inteiro.

#### Minimizar a utilização de condições negativas
<a id="minimize-the-use-of-negative-conditions" class="xliff"></a>

As condições negativas são as condições de associações que utilizam os seguintes operadores ou funções: `!=`, `not()`, `\<`, `\<=`. Para otimizar o desempenho, sempre que possível, inclua a condição que pretende com múltiplas condições positivas em vez de uma condição negativa.

#### Minimizar a utilização de condições de associações com base em atributos de referência com vários valores
<a id="minimize-the-use-of-membership-conditions-based-on-multivalued-reference-attributes" class="xliff"></a>

A utilização de condições com base em atributos de referência com vários valores deve ser minimizada porque um grande número destes conjuntos pode afetar o desempenho de operações no atributo utilizado na condição de associação.

### Reposição de Palavra-passe
<a id="password-reset" class="xliff"></a>

#### Os computadores semelhantes a quiosques que são utilizados para a reposição de palavras-passe devem definir segurança local para limpar o ficheiro de paginação de memória virtual
<a id="kiosk-like-computers-that-are-used-for-password-reset-should-set-local-security-to-clear-the-virtual-memory-pagefile" class="xliff"></a>

Ao implementar a reposição de palavra-passe do FIM 2010 numa estação de trabalho destinada a ser um quiosque, recomendamos que a definição de política de segurança local Encerrar: limpa o ficheiro de paginação de memória virtual esteja ativada para garantir que as informações confidenciais da memória do processo não estão disponíveis para utilizadores não autorizados.

#### Os utilizadores devem sempre registar-se numa reposição de palavra-passe num computador em que tenham sessão iniciada
<a id="users-should-always-register-for-a-password-reset-on-a-computer-that-they-are-logged-on-to" class="xliff"></a>

Quando um utilizador tenta registar-se na reposição de palavra-passe através de um portal Web, o FIM 2010 inicia sempre o registo em nome do utilizador com sessão iniciada, independentemente do utilizador que tem sessão iniciada no site. Os utilizadores devem sempre registar-se numa reposição de palavra-passe num computador em que tenham sessão iniciada.

#### Não defina a chave de registo AvoidPdcOnWan para verdadeiro
<a id="do-not-set-the-avoidpdconwan-registry-key-to-true" class="xliff"></a>

Ao utilizar a reposição de palavra-passe do MIM 2016, não defina a chave de registo AvoidPdcOnWan para verdadeiro.

Se esta chave de registo for definida para verdadeiro, o utilizador irá muito provavelmente passar pelas portas de palavras-passe, repor a palavra-passe no controlador de domínio primário (PDC) e tentar iniciar sessão. Por causa desta chave de registo, o controlador de domínio local não efetuará a validação secundária com o PDC, negando o pedido de início de sessão. Se for negado vezes suficientes, o utilizador poderá ficar bloqueado do domínio e ter de ligar para o suporte.

#### Não desativar o registo de palavras-passe em texto não encriptado
<a id="do-not-turn-on-logging-of-clear-text-passwords" class="xliff"></a>

É possível registar palavras-passe em texto não encriptado ao ativar o rastreio de Nível de Serviço de diagnóstico no Windows

Communication Foundation (WCF). Esta opção não está ativada por predefinição e não deve ativá-la nos ambientes de produção. Estas palavras-passe são visíveis como elementos de texto não encriptado numa mensagem de SOAP (Simple Object Access Protocol) encriptada quando os utilizadores se registam para a reposição de palavra-passe. Para obter mais informações, veja [(Configuring Message Logging) Configurar o Registo de Mensagens](http://go.microsoft.com/fwlink/?LinkID=168572).

#### Não mapeie um fluxo de trabalho de autorização para o processo de reposição de palavra-passe
<a id="do-not-map-an-authorization-workflow-to-the-password-reset-process" class="xliff"></a>

Não deve anexar um fluxo de trabalho de autorização para uma operação de reposição de palavra-passe.
A reposição de palavra-passe requer que os fluxos de trabalho de autorização e as respostas síncronas que contêm atividades como a atividade de aprovação sejam assíncronos.

#### Não mapeie múltiplas atividades de ação para a reposição de palavra-passe
<a id="do-not-map-multiple-action-activities-to-password-reset" class="xliff"></a>

Não deve anexar um fluxo de trabalho que contenha mais do que uma atividade de ação para uma operação de reposição de palavra-passe. Um cenário de exemplo seria anexar uma segunda atividade de reposição de palavra-passe do AD DS para um MPR de reposição de palavra-passe. Este cenário não é suportado.

#### Exija que seja feito um novo registo ao adicionar, remover ou alterar a ordem das atividades num fluxo de trabalho existente
<a id="require-reregistration-when-adding-removing-or-changing-the-order-of-activities-in-an-existing-workflow" class="xliff"></a>

Ao adicionar, remover ou alterar a ordem das atividades de autenticação num fluxo de trabalho existente, selecione sempre a opção para exigir que seja feito um novo registo. Os utilizadores que tentarem autenticar para reposição de palavra-passe após uma atividade ter sido adicionada ou removida de um fluxo de trabalho, mas antes de se registarem, poderão encontrar efeitos indesejados.

### Configuração do Portal e Configuração de Visualização do Controlo de Recursos
<a id="portal-configuration-and-resource-control-display-configuration" class="xliff"></a>

#### Pondere adicionar uma exclusão de responsabilidade de privacidade à página de perfil do utilizador
<a id="consider-adding-a-privacy-disclaimer-to-the-user-profile-page" class="xliff"></a>

No MIM, por predefinição, algumas informações de perfil poderão ser apresentadas para outros utilizadores. Como cortesia para os utilizadores, os administradores devem ponderar adicionar texto personalizado consistente com as políticas da empresa à página de Perfil do Utilizador. Para obter mais informações sobre como adicionar texto personalizado a uma página do portal do MIM, veja [Introduction to Configuring and Customizing the FIM Portal (Introdução para Configurar e Personalizar o Portal do FIM)](http://go.microsoft.com/fwlink/?LinkID=165848).

### Esquema
<a id="schema" class="xliff"></a>

#### Não elimine os tipos de recurso Pessoa ou Grupo
<a id="do-not-delete-person-or-group-resource-types" class="xliff"></a>

Embora os tipos de recurso Pessoa e Grupo não estejam marcados como tipos de recurso principais, os próprios recursos ou os atributos atribuídos aos mesmos não devem ser eliminados. A interface de utilizador (IU) no Portal do MIM requer que os tipos de recurso Pessoa e Grupo e os respetivos atributos estejam presentes.

#### Não modifique os atributos principais
<a id="do-not-modify-the-core-attributes" class="xliff"></a>

Existem 13 atributos principais atribuídos a todos os tipos de recurso. Não deve modificar a relação de qualquer tipo de recurso. Os 13 atributos principais são:

-   CreatedTime

-   Creator

-   DeletedTime

-   Description

-   DetectedRulesList • DisplayName

-   ExpectedRulesList

-   ExpirationTime

-   Locale

-   MVObjectID

-   ObjectID

-   ObjectType

-   ResourceTime

Não elimine o recurso de esquema com uma dependência nos requisitos de auditoria

Não deve eliminar os seus recursos de esquema enquanto ainda tiver requisitos de auditoria para estes recursos.

#### Torne as expressões regulares sensíveis a maiúsculas e minúsculas
<a id="making-regular-expressions-case-insensitive" class="xliff"></a>

No FIM, pode ser útil tornar algumas expressões regulares sensíveis a maiúsculas e minúsculas. Pode ignorar as maiúsculas e minúsculas num grupo ao utilizar ?!:. Por exemplo, para Tipo de Funcionário, utilize

`\^(?!:contractor\|full time employee)%.`

#### Cálculo do atributo de membro
<a id="calculation-of-the-member-attribute" class="xliff"></a>

O atributo Membro exposto para o motor de sincronização é mapeado atualmente para ComputedMembers. É uma combinação de membros com base em critérios e membros selecionados manualmente. Mesmo que adicione os três atributos (Filter, ExplicitMembers e ComputedMembers), o cálculo dinâmico do atributo de membro não ocorre para tipos de recurso diferentes de grupo e conjunto.

#### Os espaços à esquerda e à direita em cadeias são ignorados
<a id="leading-and-trailing-spaces-in-strings-are-ignored" class="xliff"></a>

No FIM, pode introduzir cadeias com espaços à esquerda e à direita, mas o sistema do FIM ignora os espaços. Se submeter uma cadeia sem um espaço à esquerda e à direita, o motor de sincronização e os serviços Web irão ignorar os espaços.

#### As cadeias vazias não são iguais a nulo
<a id="empty-strings-do-not-equal-null" class="xliff"></a>

As cadeias vazias não são iguais a nulo nesta versão do FIM. A entrada de cadeia vazia é considerada um valor válido. Não presente é considerado um valor nulo.

### Processamento de Pedido e Fluxo de Trabalho
<a id="workflow-and-request-processing" class="xliff"></a>

#### Não elimine fluxos de trabalho predefinidos que são fornecidos com o MIM 2016
<a id="do-not-delete-default-workflows-that-are-shipped-with-mim-2016" class="xliff"></a>

Os seguintes fluxos de trabalho são fornecidos com o FIM 2010 e não devem ser eliminados:

-   Fluxo de Trabalho de Expiração

-   Fluxo de Trabalho de Validação de Filtro para Administradores

-   Fluxo de Trabalho de Validação de Filtro para Não Administradores

-   Fluxo de Trabalho de Notificação de Expiração de Grupo

-   Fluxo de Trabalho de Validação de Grupo

-   Fluxo de Trabalho de Aprovação de Proprietário

-   Fluxo de Trabalho de Ação de Reposição de Palavra-passe

-   Fluxo de Trabalho de AuthN de Reposição de Palavra-passe

-   Validação do Requerente com Autorização do Proprietário

-   Validação do Requerente sem Autorização do Proprietário

-   Fluxo de Trabalho de Sistema Exigido para Registo

#### Não execute duas ou mais ApprovalActivities em paralelo
<a id="do-not-run-two-or-more-approvalactivities-in-parallel" class="xliff"></a>

Não deve executar duas ou mais ApprovalActivities em paralelo. Se o fizer, pode fazer com que o pedido fique bloqueado na fase de autorização. Para múltiplas aprovações, inclua uma lista de aprovadores maior na aprovação ou faça uma sequência das duas atividades.

#### As atividades de autorização não devem modificar os dados de recursos do MIM
<a id="authorization-activities-should-not-modify-mim-resources-data" class="xliff"></a>

Evite utilizar atividades que modificam os recursos do MIM, tal como a Atividade de Avaliador de Funções, como parte dos fluxos de trabalho em fluxos de trabalho de autorização. Como o pedido não foi submetido na fase de autorização do processamento, as modificações efetuadas a informações de identidade podem ser aplicadas independentemente de o pedido poder ser rejeitado.

### Compreender as Partições do Serviço FIM
<a id="understanding-fim-service-partitions" class="xliff"></a>

O objetivo do FIM é processar pedidos que podem ser iniciados por vários clientes do FIM, tal como o serviço de sincronização do FIM e os componentes de gestão personalizada, de acordo com as políticas empresariais configuradas. Por predefinição, cada instância do serviço FIM pertence a um grupo lógico que consiste numa ou mais instâncias do serviço FIM, que também é conhecida como uma partição do serviço FIM. Se apenas tiver uma instância do serviço FIM implementada para processar todos os pedidos, é possível que ocorram latências de processamento. Algumas operações podem exceder os valores de tempo limite predefinidos que são adequados para operações de gestão personalizada. As partições do serviço FIM podem ajudá-lo a resolver este problema. Para obter informações adicionais, veja Understanding FIM Service Partitions (Compreender as Partições do Serviço FIM).

