---
title: Instalação do núcleo BHOLD [ Instalação do núcleo BHOLD ] Microsoft Docs
description: Documento central de instalação de suite BHOLD
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: c4dfb4184292ba1b5da8c4e3e176d53e6a885ed8
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042275"
---
# <a name="bhold-core-installation"></a>Instalação BHOLD Core

O módulo BHOLD Core fornece as principais características da BHOLD Suite dentro do seu ambiente. O módulo BHOLD Core deve ser instalado e configurado num servidor na sua rede local antes de poder instalar outros módulos BHOLD Suite.

## <a name="bhold-core-installation-requirements"></a>Requisitos de instalação BHOLD Core

O módulo BHOLD Core forma a fundação da Microsoft BHOLD Suite. Tem de instalar o módulo BHOLD Core antes de instalar outros módulos Microsoft BHOLD Suite.

### <a name="bhold-core-hardware-requirements"></a>Requisitos de hardware BHOLD Core

O módulo BHOLD Core forma a fundação da Microsoft BHOLD Suite. Tem de instalar o módulo BHOLD Core antes de instalar outros módulos Microsoft BHOLD Suite.

|          |        |          |
|----------|--------|----------|
|**Componente** |**Mínimo** | **Recomendado** |
|Processador | Processador de 64 bits | Processador multicore de 64 bits |
| Memória |3 GB | 6 GB ou mais |
|Armazenamento| 30 GB disponíveis |Depende do tamanho da implantação |
|Adaptador de rede| Ligação de 100Mbps ao servidor SQL e 'Gestor de Identidade de Vanguarda' (FIM) | Ligação 1Gbps ao servidor SQL e FIM|

Estas recomendações baseiam-se em implementações típicas e não têm em conta outras aplicações em execução no servidor. Pode ser necessário utilizar componentes de maior desempenho dependendo do seu ambiente específico.

### <a name="bhold-core-software-requirements"></a>Requisitos de software BHOLD Core

O módulo BHOLD Core deve ser instalado num computador que satisfaça os seguintes requisitos:

- O servidor deve estar a executar o Windows Server 2012 (64-bit), Windows Server 2016 
- O servidor deve ser membro de um domínio de Serviços de Domínio de Diretório Ativo (AD DS). Em ambientes de teste, o servidor pode ser um controlador de domínio AD DS.
- O BHOLD Core deve ser instalado por um utilizador iniciado com uma conta no mesmo domínio que o servidor e que pertence ao grupo Dedomínio Administradores no domínio e ao grupo administradores no servidor.
- Os Serviços de Informação da Microsoft Internet (IIS) com ASP.NET devem ser instalados no servidor. O IIS deve ser configurado com a autenticação do Windows ativada. Se o BHOLD Core estiver a ser instalado no Windows Server 2012/2016, a Compatibilidade de Gestão IIS 6 deve ser instalada. Se o BHOLD Core estiver a ser instalado no Windows Server 2012, as ferramentas de script IIS 6 devem ser instaladas
- A estrutura .NET 3.5 deve ser instalada.
  - Uma vez que o BHOLD Core requer .NET 3.5, este não pode ser instalado no Núcleo do Servidor.
- Silverlight 4 é exigido por outros módulos BHOLD, pelo que é aconselhável instalá-lo antes de instalar o BHOLD Core.
- O Microsoft SQL Server 2014 ou o Microsoft SQL Server 2016 devem ser instalados quer no servidor BHOLD Core quer noutro servidor da LAN. 

Os computadores Windows Client devem estar a executar a versão 6 ou posterior do Microsoft Internet Explorer e o Microsoft Silverlight versão 4 ou posterior.

## <a name="before-you-begin"></a>Antes de começar

O módulo BHOLD Core requer uma conta de utilizador que é usada para autenticar e autorizar o serviço BHOLD Core para o servidor e outras entidades de rede. Esta secção explica como criar e configurar essa conta de utilizador e fornece uma folha de cálculo de pré-instalação que o ajudará a recolher as informações de que necessita para completar a Configuração Core BHOLD.

>{! IMPORTANTE} Quando instalar o BHOLD Core, pode utilizar uma base de dados de servidor SQL existente como base de dados BHOLD Core, ou pode permitir que o assistente de configuração core BHOLD crie a base de dados BHOLD Core para si. Se optar por utilizar uma base de dados existente, tem de garantir que nenhum outro utilizador pode aceder à base de dados enquanto estiver a instalar o BHOLD Core. Antes de instalar o BHOLD Core, verifique se os controlos de acesso da base de dados do Servidor SQL não permitem o acesso por ninguém além do utilizador que está a instalar o BHOLD Core.

## <a name="required-user-and-group"></a>Utilizador e grupo necessários

O módulo BHOLD Core deve ser capaz de iniciar sessão no domínio com uma conta de utilizador dedicada a esse fim e que é membro de dois grupos de segurança específicos, incluindo um que é criado especificamente para o módulo BHOLD Core. A adesão ao grupo Dedomínio Admins é necessária para realizar este procedimento.
**Para criar e configurar o grupo de utilizador e segurança BHOLD Core**

1.  Num controlador de domínio, clique em **Iniciar**, apontar para **Ferramentas Administrativas,** e, em seguida, clique em **Utilizadores e Computadores**de Diretório Ativo .

2.  Na árvore da consola, expanda o domínio onde a conta está a ser criada, clique à direita **Utilizadores,** aponte para **Novo**e clique no **Grupo**.

3.  No **Novo Objeto –** Caixa de diálogo de grupo, em **nome de grupo,** digite o nome do grupo (padrão BHOLD: BHOLDApplicationGroup) e, em seguida, clique EM **OK**.

4.  Clique no direito **Utilizadores,** aponte para **Novo**e, em seguida, clique no **Utilizador**.

5.  Em **nome completo**, escreva um nome que o ajude a identificar a conta, por exemplo, a Conta de Serviço Core BHOLD.

6.  No nome de início de **sessão do Utilizador,** escreva o nome de utilizador da conta de serviço BHOLD Core (padrão BHOLD: b1user) e, em seguida, clique em **Seguinte**.

7.  Na **palavra-passe** e confirmar a **palavra-passe,** escreva a palavra-passe para a conta de serviço.

8.  Claro **O Utilizador tem de alterar a palavra-passe no próximo início de sessão,** selecione User não pode alterar a **palavra-passe** e **a palavra-passe nunca expira,** clique em **Seguinte**, e depois clique em **Terminar**.

9.  No painel de resultados da consola, clique na conta de utilizador e clique em **Adicionar a um grupo**.

10. Na caixa de diálogo **Select Groups,** digite o nome de exibição do grupo que criou anteriormente, escreva um ponto e vírgula (;) e, em seguida, escreva IIS_IUSRS.

11. Clique em **'Verificar Nomes'** e, em seguida, clique **em OK**.  

O seguinte procedimento deve ser realizado no computador onde o módulo BHOLD Core deve ser instalado. Deve ser registado como membro do grupo Dedomínio Administradores para realizar este procedimento.

## <a name="bhold-core-installation-worksheet"></a>Folha de cálculo de instalação BHOLD Core

Antes de começar a instalar o módulo BHOLD Core, tem de estar preparado para fornecer a informação que o assistente de configuração core BHOLD necessita para completar a instalação. A seguinte folha de cálculo irá ajudá-lo a registar essa informação para que esteja pronto para fornecê-la quando for necessária. Cada secção corresponde a uma página no assistente de configuração BHOLD Core.

### <a name="account-settings"></a>Definições da conta

| **Item**                                    | **Descrição**                                                                                                                                                                                                                                                                                             | **Valor**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilize o Fornecedor de Segurança no Domínio/Máquina** | Quando selecionado, especifica que a segurança dos Serviços de Domínio do Diretório Ativo controlará o acesso ao BHOLD Core.                                                                                                                                                                                                  | Selecione a caixa de verificação. **Importante:** A instalação falhará se esta caixa de verificação não for selecionada.                                                                 |
| **Domínio**                                  | Especifica o domínio que contém o servidor BHOLD, a conta de serviço e o grupo de aplicações. **Importante:** Especifique o nome de domínio utilizando o nome NetBIOS (curto) e não o nome de domínio totalmente qualificado (FQDN). Por exemplo, se o FQDN do domínio for fabrikam.com, especifique o nome de domínio como CONTOSO. | Escreva o nome de domínio aqui:                                                                                                                                        |
| **Grupo de candidaturas**                       | Especifica o nome do grupo de segurança que criou anteriormente no [utilizador e grupo exigidos.](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug)                                                                                                                                  | Escreva o nome de grupo aqui:                                                                                                                                         |
| **Utilizador de serviço**                            | Especifica o nome de início de sessão da conta de utilizador de serviço que criou anteriormente no [utilizador e grupo exigidos](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                      | Escreva aqui o nome da conta de utilizador:                                                                                                                                  |
| **Palavra-passe**                                | Especifica a palavra-passe da conta de utilizador do serviço BHOLD Core.                                                                                                                                                                                                                                              | Escreva a palavra-passe aqui: **Importante:** Certifique-se de manter esta palavra-passe num local escondido e seguro.                                                                |
| **Site IP/Porto**                         | Especifica o endereço IP e o número de porta do website a criar no servidor intranet. Altere o\*valor padrão () apenas se não utilizar o mesmo endereço IP que o website predefinido. Mude o número da porta para uma porta disponível apenas se a porta padrão (5151) já estiver em uso.             | Se um endereço IP não predefinido for utilizado pelo website predefinido, escreva-o aqui: Se o número de porta padrão já estiver em uso, escreva o número da porta do site BHOLD aqui: |

### <a name="database-settings"></a>Definições de base de dados

| **Item**                                       | **Descrição**                                                                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilizar segurança integrada**                    | Especifica que a Autenticação do Windows é utilizada para aceder à base de dados.                                                                                                                                                                                                     | Selecione a caixa de verificação se a autenticação do Windows for utilizada para se ligar ao Servidor SQL. Limpe a caixa de verificação se for utilizada a autenticação do servidor SQL. A base de dados deve ter sido criada antes de executar a configuração do núcleo BHOLD se for utilizada a autenticação do servidor SQL. **Nota:** Se for utilizada a Autenticação do Windows, deve iniciar-se o seu login com uma conta que tenha a função de servidor de sisadmina no servidor de base de dados. |
| **Palavra-passe** **do utilizador** e da base de dados | Especifica o nome de utilizador e a palavra-passe de um utilizador com a função do servidor sysadmin no servidor de base de dados. Estes valores só são fornecidos quando for utilizada a autenticação do servidor SQL.                                                                                               | Escreva aqui o nome de utilizador do SQL Server: Escreva aqui a palavra-passe do utilizador do SQL Server: **Nota:** Certifique-se de manter esta palavra-passe num local oculto e seguro.                                                                                                                                                                                                                                                  |
| **Nome do servidor** de base de dados e **da base de dados**   | Especifica o nome NetBIOS do servidor de base de dados e o nome da base de dados (predefinido: b1) que a Configuração core bHOLD criará. Se não estiver a utilizar a instância de servidor de base de dados predefinida, especifique a instância do servidor de base de dados na\\*\<instância\>* do * \<servidor\>* de formulários . | Escreva aqui o nome do servidor (ou servidor e instância): Escreva o nome da base de dados aqui:                                                                                                                                                                                                                                                                                                                   |
| **Fazer restrições ao utilizador da base de dados**    | Obsoleto.                                                                                                                                                                                                                                                                 | Não alterar o valor padrão                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>Configuração do núcleo BHOLD

Para instalar o módulo BHOLD Core, inicie sessão como membro do grupo Domain Admins, descarregue o seguinte ficheiro e execute-o como administrador no servidor que pretende instalar o módulo BHOLD Core em: 

- \_ * \<Versão\>* BholdCore Release.msi

Substitua * \<\> * a Versão pelo número de versão da versão do lançamento BHOLD Core que está a instalar.

Para executar o ficheiro do programa como administrador, clique no ficheiro à direita e, em seguida, clique em **Executar como administrador**.

### <a name="postinstallation-settings"></a>Definições de pós-instalação

Depois de concluída a Configuração do Núcleo BHOLD, tem de configurar o Firewall do Windows e alterar as definições avançadas no pool de aplicações BHOLD Core nos Serviços de Informação da Internet para completar a configuração BHOLD Core. Se necessário, também deve alterar os atributos do sistema BHOLD para satisfazer os seus requisitos.

#### <a name="configuring-windows-firewall"></a>Configurar firewall do Windows

Se os utilizadores acederem ao BHOLD utilizando navegadores web em computadores remotos, tem de configurar o Windows Firewall no servidor BHOLD Core para permitir a entrada de ligações à porta do site que especificou quando instalou o BHOLD Core.

Para realizar este procedimento, deve ser membro do grupo administradores no computador local.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>Para permitir a entrada de ligações ao website da BHOLD

1.  Clique em **Iniciar,** apontar para **ferramentas administrativas,** clique no windows **firewall com definições avançadas**e, em seguida, clique em **Executar como administrador**.

2.  No painel esquerdo, clique em **Regras de Entrada**, e depois, no painel direito, clique em Nova **Regra**.

3.  No novo assistente de regra de entrada, clique em **Porta**, e, em seguida, clique **em Seguinte**.

4.  Certifique-se de que o **TCP** é selecionado, em **portas locais específicas,** escreva o número de porta padrão BHOLD Core (5151) ou o número de porta que especificou quando instalou bHOLD Core, e depois clique **em Seguinte**.

5.  Certifique-se de que **a ligação** é selecionada e, em seguida, clique em **Seguinte**.

6.  Na página **Profile,** verifique as caixas de verificação de locais a partir dos quais não quer aceder ao website da BHOLD e, em seguida, clique em **Next**.

7.  Na página **Nome,** digite um nome para a regra (como permitir ligações de entrada ao BHOLD Core) e, em seguida, clique em **Terminar**.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>Habilitar aplicações de 32 bits para o conjunto de aplicações BHOLD Core

Para permitir que o IIS funcione corretamente com o módulo BHOLD Core, tem de configurar o conjunto de aplicações BHOLD Core para suportar aplicações de 32 bits. Tem de ser iniciado com a conta utilizada para instalar o módulo BHOLD Core para realizar este procedimento.

**Para ativar suporte de aplicação de 32 bits para o conjunto de aplicações BHOLD Core**

1.  Para abrir o Gestor de Serviços de Informação na Internet, clique em **Iniciar,** apontar para **Ferramentas de Administrador,** e, em seguida, clicar em **Internet Information Services (IIS) Manager**.

2.  Na árvore da consola, expanda o nome do servidor e, em seguida, clique em **Application Pools**.

3.  Na lista de Pools de **Aplicações,** clique no CoreAppPool, clique no clique direito no **CoreAppPool,** e depois clique em **Definições Avançadas**.

4.  Na caixa de diálogo **Definições Avançadas,** na lista **de aplicações Ativar 32-Bits,** selecione **True**, e, em seguida, clique **EM OK**.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>Estabelecer o nome principal do serviço para o website da BHOLD

Se o nome de rede utilizado para contactar o website bHOLD não for o mesmo que o nome do servidor, deve estabelecer um nome principal de serviço (SPN) para HTTP. Por exemplo, se utilizar um registo de recursos CNAME no DNS para especificar um pseudónimo para o servidor, ou se utilizar o equilíbrio de carga de rede, deve registar estes endereços de rede adicionais no Diretório Ativo. Se não o fizer, o Internet Explorer não pode utilizar o protocolo Kerberos ao contactar o website da BHOLD.

> [!IMPORTANT]
> Se o módulo BHOLD Core estiver instalado no mesmo computador que o Portal FIM, deve criar registos de recursos DNS (CNAME ou A) com diferentes nomes de anfitriões para os servidores que executam o BHOLD Core e o servidor que executa o Portal FIM. Apenas um SPN pode ser estabelecido para um par de tipos de serviço/servidor-pseudónimo, e assim o BHOLD Core e o Portal FIM requerem SPNs separados porque normalmente funcionam sob contas diferentes. O comando setspn relata um erro se um SPN já tiver sido estabelecido sob outra conta.

A adesão aos Administradores de **Domínio**, ou equivalente, é o mínimo necessário para completar este procedimento.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>Para estabelecer o SPN do website bHOLD

1.  No controlador de domínio de domínio do Diretório Ativo, clique em **Iniciar,** clique em **Todos os programas,** clique em **Acessórios,** clique no motivo de **comando**do clique direito e, em seguida, clique em Executar **como administrador**.

2.  No pedido de comando, digite o seguinte comando e, em seguida, prima ENTER: setspn –S HTTP/ \\ * \<networkalias\> \<\> nome* * \<\> * de conta de domínio onde:

    -   *networkalias\> é o endereço que os clientes usam para contactar o website da BHOLD \<*

    -   *\<o\>*\\*\<nome\> * de conta de domínio é o domínio e o nome de utilizador da conta de serviço BHOLD Core que criou quando instalou o BHOLD Core.

3.  Repita o passo anterior para todos os outros nomes que os clientes usam para contactar o website da BHOLD, por exemplo, pseudónimos CNAME, nomes que incluem um nome de domínio totalmente qualificado, ou nomes que incluam um nome de domínio NetBIOS (curto).

#### <a name="setting-bhold-system-attributes"></a>Definição de atributos do sistema BHOLD

Para validar que a instalação do módulo BHOLD Core foi bem sucedida, abra o portal BHOLD Core e veja os atributos do sistema. Além disso, para garantir que o módulo BHOLD Core funciona corretamente no seu ambiente, pode modificar os seguintes atributos do sistema BHOLD, conforme apropriado:

| **Atributo**                | **Descrição**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **NoHistory**                | Configurado para Y se o website BHOLD estiver a funcionar num serviço web agrupado para garantir que os itens recentemente apresentados funcionam corretamente. Configurado para N se o website BHOLD estiver a funcionar num servidor IIS autónomo.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Definir para Y para garantir que as unidades organizacionais (orgunits) podem ser movidas apenas para orgunits com o mesmo tipo organizacional que a orgunit principal. Por exemplo, isto impede que uma unidade orgunit do projeto seja transferida para uma unidade de departamento. Definir em N para permitir que uma orgunit seja colocada numa orgunit de um tipo diferente. |
| **Dias entre a Corrida da ABA**     | Detete to a um inteiro de dois dígitos para especificar o intervalo (em dias) entre duas execuções de autorização baseada no atributo (ABA). Por exemplo, para especificar que as corridas de ABA serão separadas por dois dias, tipo 02.                                                                                                                     |
| **Hora de início da corrida da ABA**    | Detete to a um inteiro de dois dígitos para especificar a hora do dia em que ocorrerá uma execução de autorização baseada no atributo. Por exemplo, para especificar que a corrida da ABA terá lugar às 23h. (23:00), tipo 23.                                                                                                             |
| **Cardeal do Sistema**       | Definido para N se não quiser um controlo de cardinalidade do sistema em BHOLD. O valor padrão é Y.                                                                                                                                                                                                                             |
| **Registo**                  | Set to N se não quiser que as alterações sejam registadas. O valor padrão é Y.                                                                                                                                                                                                                                            |
| **Processamento de fila de sistemas**   | Set para N se não quiser o processamento da fila do sistema. Não altere este valor a menos que seja direcionado para o fazer através do suporte do produto.                                                                                                                                                                                           |

Deve ser registado como membro do grupo Dedomínio Administradores para realizar este procedimento.

**Para definir um atributo do sistema BHOLD**

1.  Clique em **Iniciar,** clique em **Todos os Programas,** e depois clique no **Internet Explorer**.

2.  Na caixa de endereços, escreva, onde * \<\> * o servidor é o nome do servidor do website BHOLD e * \<\> * a porta é o número de porta ligado ao website.

3.  Clique em **Casa,** clique em **Valores,** e clique em **Modificar**.

4.  Localize o nome do atributo que pretende alterar, digite o novo valor na caixa ao lado do nome do atributo e, em seguida, clique em **OK**.

## <a name="next-steps"></a>Passos seguintes

Depois de ter instalado o BHOLD Core e validado que a instalação foi bem sucedida, pode instalar módulos adicionais. Neste ponto, a base de dados BHOLD ficará essencialmente vazia, contendo apenas uma conta de utilizador, a conta raiz e uma unidade organizacional (orgunit), a orgunit raiz. Para adicionar mais utilizadores à base de dados BHOLD, pode instalar o módulo de Conector de Gestão de Acesso ou o módulo Gerador de Modelos BHOLD, dependendo das suas necessidades. Pode utilizar o módulo de Conector de Gestão de Acesso para importar dados dos utilizadores do Serviço de Sincronização FIM, ou pode utilizar o Gerador de Modelos BHOLD para importar dados dos utilizadores a partir de um conjunto de ficheiros estruturados. Para mais informações sobre a utilização do módulo de conector de gestão de acesso, consulte o Guia do [Laboratório de Teste: Conector de Gestão](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx)de Acesso BHOLD .

Para obter mais informações sobre a utilização do módulo BHOLD Model Generator, consulte:

- [Guia de conceitos da Suite BHOLD da Microsoft](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx)
- [Referência técnica da Suite Microsoft BHOLD](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx).
