---
title: "Recuperação após desastre do PAM | Documentos da Microsoft"
description: "Saiba como configurar Privileged Access Management para uma elevada disponibilidade e para a recuperação após desastre."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 03e521cd-cbf0-49f8-9797-dbc284c63018
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2fab9af837ed11b1f2f7f32c9ced6d79c8cc9d00
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2017
---
# Considerações para elevada disponibilidade e recuperação após desastre do ambiente bastion
<a id="high-availability-and-disaster-recovery-considerations-for-the-bastion-environment" class="xliff"></a>
Este artigo descreve considerações de elevada disponibilidade e recuperação após desastre ao implementar os Serviços de Domínio do Active Directory (AD DS) e o Microsoft Identity Manager 2016 (MIM) para Gestão de Acesso Privilegiado (PAM).

As empresas concentram-se na elevada disponibilidade e na recuperação após desastre para cargas de trabalho no Windows Server, SQL Server e do Active Directory. No entanto, a disponibilidade fiável do ambiente bastion para Gestão de Acesso Privilegiado também é importante. O ambiente bastion é uma parte crítica da sua organização de infraestrutura de TI, uma vez que os utilizadores interagem com os respetivos componentes para assumirem funções administrativas. Para obter mais informações sobre a disponibilidade elevada em geral, pode descarregar o documento técnico [Descrição Geral da Disponibilidade Elevada da Microsoft](http://download.microsoft.com/download/3/B/5/3B51A025-7522-4686-AA16-8AE2E536034D/Microsoft%20High%20Availability%20Strategy%20White%20Paper.doc).

## Cenários de elevada disponibilidade e recuperação após desastre
<a id="high-availability-and-disaster-recovery-scenarios" class="xliff"></a>

Ao planear a elevada disponibilidade e recuperação após desastre, tenha em conta as seguintes perguntas:

- Que funções podem ser afetadas por uma falha?
- Que funções são essenciais para as empresas e/ou essenciais para as operações de TI?
- Quais são os riscos que podem conduzir a uma falha destes sistemas?

O âmbito destas considerações provoca um impacto sobre o custo total da implementação e das operações, para que as organizações possam atribuir mais ou menos prioridades a determinadas funções e aceitar o risco de falhas temporárias de funções de prioridade inferior. A seguinte tabela destaca uma classificação de prioridade potencial que uma organização pode utilizar:

| **Função de floresta bastion** | **Prioridade relativa durante a recuperação** | **Mitigação se a função estiver indisponível** |
| --------------------------- | --------------------- | -------------- |
| Estabelecimento de confiança         | Baixa | Aguarde até que o ambiente bastion seja restaurado |
| Mitigação de utilizadores e de grupos   | Baixa | Aguarde até que o ambiente bastion seja restaurado |
| Administração do MIM          | Baixa | Aguarde até que o ambiente bastion seja restaurado |
| Ativação de função com privilégios  | Média | Contas de segurança com smartcard dedicadas para adicionar manualmente os utilizadores a grupos administrativos |
| Gestão de recursos         | Alto | Contas de segurança com smartcard dedicadas para adicionar manualmente os utilizadores a grupos administrativos |
| Monitorização dos utilizadores e grupos na floresta existente | Baixa | Aguarde até que o ambiente bastion seja restaurado |

Agora vamos ver cada uma destas funções de floresta bastion por sua vez.

### Estabelecimento de confiança
<a id="trust-establishment" class="xliff"></a>

Tem de existir uma confiança de floresta entre os domínios da floresta existente e a floresta do ambiente bastion. Isto é para que os utilizadores que estão a autenticar para o ambiente bastion possam administrar recursos nas florestas existentes. Pode ser necessária uma configuração adicional, por exemplo, para permitir a migração de utilizadores dos domínios existentes em versões anteriores do Windows Server.

O estabelecimento de confiança exige que os controladores de domínio de floresta existentes estejam online, assim como os componentes de MIM e AD do ambiente bastion.  Se existir uma falha de qualquer uma das partes durante o estabelecimento de confiança, o administrador pode tentar novamente após a falha estar resolvida.  No caso dos controladores de domínio de floresta existentes ou o ambiente bastion ter sido recuperado após uma falha, o MIM também inclui cmdlets do PowerShell `Test-PAMTrust` e `Test-PAMDomainConfiguration` que podem ser utilizados para verificar se uma confiança ainda está no local.

### Migração de utilizadores e de grupos
<a id="user-and-group-migration" class="xliff"></a>

Assim que a confiança tiver sido estabelecida, podem ser criados grupos de sombra no ambiente bastion, bem como contas de utilizador para os membros desses grupos e aprovadores. Isto permite aos utilizadores ativarem funções com privilégios e obterem novamente associações de grupo de forma eficaz.

A migração de grupos e de utilizadores exige que os controladores de domínio de floresta existentes estejam online, assim como os componentes de MIM e AD do ambiente bastion.   Se os controladores de domínio de floresta existentes estiverem inacessíveis, não podem ser adicionados outros utilizadores e grupos ao ambiente bastion, mas os utilizadores e os grupos existentes não são afetados. Se uma falha de qualquer um dos componentes ocorrer durante a migração, o administrador pode tentar novamente após a falha estar resolvida.

### Administração do MIM
<a id="mim-administration" class="xliff"></a>
Assim que os utilizadores e os grupos migrarem, um administrador pode configurar ainda mais no MIM as atribuições de função, ligando os utilizadores como candidatos para ativação em funções.  Pode também configurar as políticas de MIM para aprovação e o MFA do Azure.  

A administração de MIM requer que os componentes de MIM e AD do ambiente bastion estejam online.

### Ativação de função com privilégios
<a id="privileged-role-activation" class="xliff"></a>
Quando um utilizador pretende ativar uma função com privilégios, este deve autenticar para o domínio de ambiente bastion e submeter um pedido para o MIM.  O MIM inclui APIs SOAP e REST, bem como interfaces de utilizador no PowerShell e numa página Web.

A ativação de função privilegiada requer que os componentes de MIM e AD do ambiente bastion estejam online.  Além disso, se o MIM estiver configurado para utilizar o [MFA do Azure para ativação](use-azure-mfa-for-activation.md) da função selecionada, é necessário acesso à Internet para contactar o serviço de MFA do Azure.

### Gestão de Recursos
<a id="resource-management" class="xliff"></a>
Assim que um utilizador for ativado com êxito para a função, o controlador de domínio pode gerar um pedido de suporte do Kerberos para o mesmo, que pode ser utilizado por controladores de domínios nos domínios existentes e reconhecerá as novas associações de grupo temporário do utilizador.

A gestão de recursos requer que um controlador de domínio para o domínio de recurso esteja online, assim como um controlador de domínio no ambiente bastion.  Assim que um utilizador estiver ativado, emitir os pedidos de suporte do Kerberos não necessita do MIM ou do SQL para estar online no ambiente bastion.  (Tenha em atenção que, com o Windows Server 2012 R2 como o nível funcional para o ambiente bastion, o MIM tem de estar online, para terminar a associação ao grupo temporário.)

### Monitorização dos utilizadores e grupos na floresta existente
<a id="monitoring-of-users-and-groups-in-the-existing-forest" class="xliff"></a>
O MIM também inclui um serviço de monitorização do PAM que verifica regularmente os utilizadores e os grupos nos domínios existentes, e atualiza a base de dados do MIM e do AD em conformidade.  Este serviço não tem de estar online para a ativação de função ou durante a gestão de recursos.

A monitorização exige que os controladores de domínio de floresta existentes estejam online, assim como os componentes de MIM e AD do ambiente bastion.  

## Opções de implementação
<a id="deployment-options" class="xliff"></a>
A [Descrição geral do ambiente](environment-overview.md) ilustra uma topologia básica adequada para aprender a tecnologia que não se destina a ser de elevada disponibilidade. Esta secção descreve como expandir mediante essa topologia para fornecer elevada disponibilidade, para organizações com um único local ou com vários.

### Rede
<a id="networking" class="xliff"></a>

O tráfego de rede entre os computadores no ambiente bastion deve ser isolado das redes existentes, ao utilizar uma rede física ou virtual diferente.  Consoante os riscos para o ambiente bastion, também poderá ser necessário ter interconnects físicas independentes entre os computadores.  Algumas tecnologias de cluster de ativação pós-falha têm requisitos adicionais em interfaces de rede.

Os computadores que alojam Serviços de Domínio do Active Directory e os que alojam os Serviços de MIM no ambiente bastion necessitam de conetividade bidirecional nos recursos da floresta existente para que:
- os utilizadores sejam autenticados pelos controladores de domínio de floresta PRIV
- os utilizadores peçam ativação
- os utilizadores tenham os pedidos de suporte do Kerberos consumíveis pelos recursos numa floresta existente
- o MIM monitorize os domínios de floresta existentes
- o MIM envie servidores de e-mail localizados numa floresta existente.

### Topologias de elevada disponibilidade mínimas
<a id="minimal-high-availability-topologies" class="xliff"></a>
Uma organização pode selecionar as funções do respetivo ambiente bastion que necessitam de elevada disponibilidade, com as seguintes restrições:

- A elevada disponibilidade para qualquer função fornecida pelo ambiente bastion requer, pelo menos, dois controladores de domínio.  
- A elevada disponibilidade para pedidos de ativação necessita de, pelo menos, dois computadores que alojem o Serviço de MIM e também necessita de elevada disponibilidade para o SQL Server.
- A elevada disponibilidade do SQL Server com clusters de ativação pós-falha requer, pelo menos, dois servidores que fornecem o SQL Server e estes não podem ser o mesmo que um controlador de domínio.
- O Serviço de MIM não deve ser instalado no controlador de domínio, de forma a minimizar a superfície de ataque de cada servidor.

A topologia de elevada disponibilidade mais pequena para todas as funções num ambiente bastion é composta por, pelo menos, quatro servidores e armazenamento partilhado. Dois dos servidores devem estar configurados como controladores de domínio, fornecendo Serviços de Domínio do Active Directory. Os outros dois servidores podem ser configurados como um cluster de ativação pós-falha, fornecendo o SQL Server e o serviço de MIM.

Além disso, uma implementação típica do ambiente bastion também inclui uma estação de trabalho com privilégios de administração para a gestão desses servidores, bem como um componente de monitorização

O diagrama seguinte ilustra uma arquitetura possível:

![topologia de bastion - diagrama](media/bastion1.png)

Os servidores adicionais podem ser configurados para cada uma destas funções, para fornecer um desempenho superior em condições de carga ou para a redundância geográfica conforme descrito abaixo.

### Implementações de suporte de vários sites
<a id="deployments-supporting-multiple-sites" class="xliff"></a>
Escolher a topologia de implementação correta para recursos que estão implementados através de vários locais depende de três fatores:  
- Objetivos e riscos para elevada disponibilidade e recuperação após desastre  
- A capacidade de hardware para alojar o ambiente bastion  
- O modelo de trabalho administrativo para cada site.

Uma das abordagens mais simples seria alojar o ambiente bastion num determinado site.  Em condições normais, os utilizadores ligar-se-iam à implementação de MIM na ativação de pedido e ambiente bastion desse site e as ativações teriam efeito em recursos de cada site.  Se a ligação de rede for interrompida ou o site que aloja o ambiente bastion não estiver disponível, as credenciais offline não podem ser acedidas noutro site, para efetuar a administração temporária até a rede estar novamente ligada.  Esta abordagem poderá ser adequada para situações em que é antecipada administração local de um site específico, como uma sucursal, para ser rara e limitada para restabelecer a ligação desse site com a rede restante de uma organização.

![Bastion único para topologia de vários sites - diagrama](media/bastion2.png)

Para obter elevada disponibilidade e recuperação após desastre em todos os sites, também é possível implementar os componentes do ambiente bastion em cada site, partilhando um diretório de PRIV comum e uma base de dados SQL comum.  Nesta topologia, deve ser interrompida a ligação de rede e os utilizadores de cada site podem continuar a funcionar de forma independente.

![Multi-bastion para topologia de vários sites - diagrama](media/bastion3.png)

Uma restrição nesta abordagem de implementação é o facto de o SQL Server exigir um cluster que abranja ambos os sites, o que pode ser complexo para implementar. Nesta situação, considere como alternativa replicar apenas o Active Directory (floresta PRIV) do ambiente bastion.  No caso de existir uma quebra de rede entre sites, os utilizadores do site B, que já ativou anteriormente as respetivas funções com privilégios conseguiriam continuam a administrar recursos no site B.

![Bastion replicada para topologia de vários sites - diagrama](media/bastion4.png)

Se cada site representa um limite separado administrativo, também é possível implementar vários ambientes bastion independentes.  Enquanto cada ambiente bastion teria o mesmo software, os nomes de domínio de cada seriam diferentes e teria existiriam aspetos comuns entre os diretórios e as bases de dados de cada ambiente bastion. Um utilizador que pretende gerir os recursos num site específico prefere ativar uma conta de utilizador no ambiente bastion desse site.

![Bastions independentes para topologia de vários sites - diagrama](media/bastion5.png)

Por fim, as implementações mais complexas são possíveis, uma vez que os vários ambientes bastion podem ser configurados de forma independente para gerir os recursos num determinado domínio.

![Bastion complexo para topologia de vários sites - diagrama](media/bastion6.png)

### Ambiente bastion alojado
<a id="hosted-bastion-environment" class="xliff"></a>
Algumas organizações também consideraram estabelecer o ambiente bastion separado a partir de qualquer um dos respetivos sites existentes. O software de ambiente bastion pode ser alojado numa plataforma de virtualização das redes da sua organização ou num fornecedor de alojamento externo.  Ao avaliar esta abordagem, tenha em atenção que:

- Para se proteger contra ataques com origem nos domínios existentes, a administração do ambiente bastion tem de ser isolada das contas administrativas do domínio existente.
- O ambiente bastion necessita de conetividade de TCP/IP para os controladores de domínio no domínio existente.  É possível encontrar uma lista de portas em [Como configurar uma firewall de domínios e confianças](https://support.microsoft.com/kb/179442).
- Uma implementação virtualizada de Serviços de Domínio do Active Directory requer funcionalidades específicas da plataforma de virtualização, conforme descrito em [Configuração e Implementação do Controlador de Domínio Virtualizado](https://technet.microsoft.com/library/jj574223.aspx).
- Uma implementação de elevada disponibilidade do SQL Server para o serviço MIM requer configuração do armazenamento especializada, descrita na secção [Armazenamento da base de dados do SQL Server](#sql-server-database-storage) abaixo.  Nem todos os fornecedores de alojamento podem oferecer atualmente alojamento do Windows Server com configurações de disco adequadas para clusters de ativação pós-falha do SQL Server.

## Procedimentos de recuperação e preparação de implementação
<a id="deployment-preparation-and-recovery-procedures" class="xliff"></a>
Preparar para uma elevada disponibilidade ou implementação pronta para recuperação após desastre do ambiente bastion requer considerações sobre como instalar o Windows Server Active Directory, o SQL Server, a respetiva base de dados no armazenamento partilhado, o serviço MIM e os respetivos componentes de PAM.

### Windows Server
<a id="windows-server" class="xliff"></a>
O Windows Server contém uma funcionalidade incorporada para elevada disponibilidade, permitindo que vários computadores trabalhem em conjunto como um cluster de ativação pós-falha. Os servidores em cluster são ligados por cabos físicos e software. Se um ou mais nós de cluster falharem, outros nós começam a fornecer serviço (um processo conhecido como ativação pós-falha).   Pode encontrar mais detalhes em [Descrição geral do Clustering de Ativação Pós-falha](https://technet.microsoft.com/library/hh831579.aspx).

Certifique-se de que o sistema operativo e as aplicações no ambiente bastion recebem atualizações para problemas de segurança. Algumas destas atualizações podem exigir um reinício do servidor, por isso, coordene os tempos em que as atualizações são aplicadas entre os servidores para evitar falhas expandidas. Uma abordagem possível é utilizar a [Atualização com Suporte para Cluster](https://technet.microsoft.com/library/hh831694.aspx) nos servidores do cluster de ativação pós-falha do Windows Server.

Os servidores no ambiente bastion serão associados a um domínio e dependente dos serviços de domínio. Certifique-se de que não foram inadvertidamente configurados com uma dependência num controlador de domínio específico para serviços como o DNS.

### Ambiente bastion do Active Directory
<a id="bastion-environment-active-directory" class="xliff"></a>
Os Serviços de Domínio do Active Directory do Windows Server incluem suporte nativo para a elevada disponibilidade e recuperação após desastre.

#### Preparação
<a id="preparation" class="xliff"></a>
Uma implementação de produção normal da gestão de acesso privilegiado inclui, pelo menos, dois controladores de domínio no ambiente bastion. As instruções para configurar o primeiro controlador de domínio no ambiente bastion estão incluídas no passo 2 dos artigos de implementação, [Preparar o controlador de domínio PRIV](step-2-prepare-priv-domain-controller.md).

É possível encontrar o procedimento para adicionar um controlador de domínio adicional em [Instalar uma Réplica do Controlador de Domínio do Windows Server 2012 num Domínio Existente (Nível 200)](https://technet.microsoft.com/library/jj574134.aspx).  

>[!NOTE]
> Se o controlador de domínio está a ser alojado numa plataforma de virtualização, como o Hyper-V, reveja as advertências na [Configuração e Implementação do Controlador de Domínio Virtualizado](https://technet.microsoft.com/library/jj574223.aspx).

#### Recuperação
<a id="recovery" class="xliff"></a>
Após uma falha, certifique-se de que se encontra disponível, pelo menos, um controlador de domínio no ambiente bastion antes de reiniciar outros servidores.

Dentro de um domínio, o Active Directory, distribui as funções de Operação Mestre Única Flexível (FSMO) nos controladores de domínio, conforme descrito em [Como Funcionam as Operações Mestres](https://technet.microsoft.com/library/cc780487.aspx).  Se um controlador de domínio falhou, poderá ser necessário descarregar uma ou mais [Funções de Controlador de Domínio](https://technet.microsoft.com/library/cc786438.aspx) em que esse controlador de domínio foi atribuído.

Após determinar que um controlador de domínio não será devolvido à produção, certifique-se de que verifica se foram atribuídas quaisquer funções a esse controlador de domínio e reatribua-os conforme necessário. Pode encontrar instruções em [Ver os Titulares da Função de Mestre das Operações](https://technet.microsoft.com/library/cc816893.aspx) e os respetivos artigos relacionados.

Também é recomendável verificar as definições de DNS de computadores associados a um ambiente bastion, bem como os controladores de domínio em domínios CORP que têm uma relação de confiança com esse controlador de domínio, para garantir que nenhum disco rígido está codificado com uma dependência no endereço IP desse computador de controlador de domínio.

### Armazenamento da base de dados do SQL Server
<a id="sql-server-database-storage" class="xliff"></a>
Uma implementação de elevada disponibilidade necessita de clusters de ativação pós-falha do SQL Server e da resposta de instâncias do cluster de ativação pós-falha do SQL Server no armazenamento partilhado entre todos os nós da base de dados e do armazenamento de registos. O armazenamento partilhado pode estar sob a forma de discos de cluster de Clustering de Ativação Pós-falha do Windows Server, de discos numa Rede de Área de Armazenamento (SAN) ou partilhas de ficheiros num servidor do SMB.  Tenha em atenção que estas têm de estar dedicadas ao ambiente bastion; o armazenamento de partilha com outras cargas de trabalho fora do ambiente de bastion não é recomendado, uma vez que pode comprometer a integridade do ambiente bastion.

### SQL Server
<a id="sql-server" class="xliff"></a>
O Serviço MIM necessita de uma implementação do SQL Server no ambiente bastion.   Para Elevada Disponibilidade, o SQL pode ser implementado com uma instância de cluster de ativação pós-falha (FCI). Ao contrário das instâncias autónomas, em FCIs a elevada disponibilidade do SQL Server está protegida pela presença de nós redundantes do FCI. Em caso de falha ou atualização planeada, o proprietário do grupo de recursos é movido para outro nó do Cluster de Ativação Pós-falha do Windows Server.

Se apenas necessitar de suporte para a recuperação após desastre, e não de elevada disponibilidade, então o envio de registos, a replicação de transações, a replicação de instantâneos ou o espelhamento da base de dados pode ser utilizado, em vez de clustering de ativação pós-falha.   

#### Preparação
<a id="preparation" class="xliff"></a>
Quando instala o SQL Server no ambiente bastion, tem de ser independente de qualquer servidor do SQL Server que já esteja presente nas florestas CORP.  Além disso, é recomendado que o SQL Server seja implementado num servidor dedicado, diferente do controlador de domínio.
Estão documentadas mais informações no guia do SQL Server para [Instâncias do Cluster de Ativação Pós-falha do AlwaysOn](https://msdn.microsoft.com/library/ms189134.aspx).

#### Recuperação
<a id="recovery" class="xliff"></a>
Se o SQL Server foi configurado para recuperação após-desastre com envio de registo, tem de atualizar o SQL Server durante a recuperação após desastre.  Além disso, tem de reiniciar cada instância de serviço MIM.

Se o SQL Server falhou ou a conetividade entre o SQL Server e o serviço MIM foi perdida, após o restauro do SQL Server, é recomendado reiniciar cada serviço MIM.  Esta ação irá garantir que o Serviço MIM restabeleça a ligação ao SQL Server.

### Serviço MIM
<a id="mim-service" class="xliff"></a>
O Serviço MIM é necessário para processar os pedidos de ativação.  Para que um computador que aloja o Serviço MIM possa ser desativado para manutenção enquanto ainda estão a ser recebidos pedidos de ativação, podem ser implementados vários computadores de Serviço MIM.  Tenha em atenção que o Serviço MIM não está envolvido nas operações do Kerberos assim que um utilizador é adicionado a um grupo.  

#### Preparação
<a id="preparation" class="xliff"></a>
Recomenda-se que implemente o Serviço MIM em vários servidores associados ao domínio PRIV.
Para elevada disponibilidade, consulte os documentos do Windows Server para [Opções de Armazenamento e Requisitos de Hardware de Clustering de Ativação Pós-falha](https://technet.microsoft.com/library/jj612869.aspx) e [Criar um Cluster de Ativação Pós-falha do Windows Server 2012](http://blogs.msdn.com/b/clustering/archive/2012/05/01/10299698.aspx).

Para implementação de produção por vários servidores, pode utilizar o Balanceamento de Carga na Rede (NLB) para distribuir a carga de processamento.  Também deverá ter um alias único (por exemplo, registos A ou CNAME) para que um nome comum seja exposto ao utilizador.

>[!IMPORTANT]
> Se utilizar uma tecnologia de balanceamento de carga que não seja a funcionalidade NLB no Windows Server 2012 R2, certifique-se de que a sua solução irá redirecionar uma sessão para o mesmo servidor e não para um servidor aleatório.

Numa implementação MIM com vários servidores, cada Serviço MIM tem um nome de anfitrião externo, um nome de serviço e um nome de partição de serviço.  O valor predefinido do nome do serviço é o nome do computador e o valor predefinido do nome de partição de serviço e nome de anfitrião externo são configurados durante a instalação do Serviço MIM no ecrã que lhe pede o endereço do Servidor do Serviço MIM. Estes três nomes estão armazenados no ficheiro %ProgramFiles%\Microsoft Forefront Identity Manager\Service\Microsoft.ResourceManagementService.exe.config como atributos `externalHostName`, `serviceName` e `servicePartitionName` do nó configuração do `resourceManagementService`.  

Quando um Serviço MIM recebe um pedido, o nome da partição de serviço é armazenado como um atributo desse pedido.   Posteriormente, apenas outras instalações do Serviço MIM que têm o mesmo nome de partição de serviço estão autorizadas a interagir com esse pedido.  Como resultado, se o cenário de PAM incluir aprovações manuais ou outro processamento de pedidos de longa duração, certifique-se de que cada Serviço MIM tem o mesmo atributo `servicePartitionName` nesse ficheiro de configuração.

#### Recuperação
<a id="recovery" class="xliff"></a>
Após uma falha, certifique-se de que, pelo menos, um controlador de domínio do Active Directory e do SQL Server estão disponíveis no ambiente do bastion antes de reiniciar o Serviço MIM.  

Uma instância de fluxo de trabalho só pode ser concluída por um servidor do Serviço MIM que tenha o mesmo nome de partição de serviço e o nome do serviço como o servidor do Serviço MIM que o iniciou.  Se um determinado computador falha ao alojar um Serviço MIM que estava processar pedidos e esse computador não será devolvido ao serviço, será necessário instalar o Serviço MIM num novo computador. No novo Serviço MIM após a instalação, edite o ficheiro *resourcemanagementservice.exe.config* e defina os atributos `serviceName` e `servicePartitionName` da nova implementação de MIM para que sejam os mesmos que o nome do anfitrião e o nome de partição de serviço do computador que falhou.

### Componentes PAM do MIM
<a id="mim-pam-components" class="xliff"></a>
O instalador do Serviço MIM e do Portal incorpora também componentes de PAM adicionais, incluindo módulos do PowerShell e dois serviços.

#### Preparação
<a id="preparation" class="xliff"></a>
Os componentes de Gestão de Acesso Privilegiado devem ser instalados em cada computador no ambiente bastion onde o serviço MIM está a ser instalado.  Estes não podem ser adicionados posteriormente.

#### Recuperação
<a id="recovery" class="xliff"></a>
Após a recuperação a partir de uma falha, certifique-se de que o Serviço MIM está em execução em, pelo menos, um servidor.  Certifique-se de que o serviço de monitorização PAM de MIM também está em execução nesse servidor, com o `net start "PAM Monitoring service"`.

Se o nível funcional do ambiente de floresta de bastion for o Windows Server 2012 R2, certifique-se de que o serviço do componente PAM de MIM também está em execução nesse servidor, com o comando `net start "PAM Component service"`.
