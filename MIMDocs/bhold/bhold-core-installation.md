---
title: Núcleo do BHOLD instalação | Documentos da Microsoft
description: Documento de núcleo de instalação do BHOLD suite
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 48ff4a06233ba95288432c4cfe48e37b4d1449ab
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358742"
---
# <a name="bhold-core-installation"></a>Instalação do BHOLD Core

O módulo do BHOLD Core fornece as principais funcionalidades do BHOLD Suite no seu ambiente. O módulo do BHOLD Core tem de ser instalado e configurado num servidor na sua rede local antes de poder instalar outros módulos de BHOLD Suite.

## <a name="bhold-core-installation-requirements"></a>Requisitos de instalação do BHOLD Core

O módulo do BHOLD Core constitui a base do Microsoft BHOLD Suite. Tem de instalar o módulo do BHOLD Core antes de instalar outros módulos do Microsoft BHOLD Suite.

### <a name="bhold-core-hardware-requirements"></a>Requisitos de hardware do BHOLD Core

O módulo do BHOLD Core constitui a base do Microsoft BHOLD Suite. Tem de instalar o módulo do BHOLD Core antes de instalar outros módulos do Microsoft BHOLD Suite.

|          |        |          |
|----------|--------|----------|
|**Componente** |**Mínimo** | **Recomendado** |
|Processador | processador de 64 bits | Processadores de 64 bits com vários núcleos |
| Memória |3 GB | 6 GB ou mais |
|Armazenamento| 30 GB disponível |Depende do tamanho de implementação |
|Placa de rede| 100 Mbps ligação ao servidor SQL e o Forefront Identity Manager (FIM) | Ligação de 1 Gbps ao servidor SQL e de FIM|

Estas recomendações são baseadas em implementações típicas e não levam em consideração outros aplicativos em execução no servidor. Poderá ter de utilizar componentes de maior desempenho dependendo do seu ambiente específico.

### <a name="bhold-core-software-requirements"></a>Requisitos de software do BHOLD Core

O módulo do BHOLD Core tem de estar instalado num computador que cumpre os seguintes requisitos:

- O servidor tem de estar a executar Windows Server 2012 (64-bit), Windows Server 2016 
- O servidor tem de ser um membro de um domínio de serviços de domínio do Active Directory (AD DS). Em ambientes de teste, o servidor pode ser um controlador de domínio do AD DS.
- BHOLD Core tem de ser instalado por um utilizador com sessão iniciado com uma conta no mesmo domínio que o servidor e que pertence ao grupo Admins do domínio no domínio e ao grupo de administradores no servidor.
- Microsoft Internet Information Services (IIS) com o ASP.NET tem de ser instalado no servidor. IIS tem de ser configurado com a autenticação do Windows ativada. Se BHOLD Core está a ser instalado no Windows Server 2012 de Maio de 2016, a compatibilidade de gestão do IIS 6 deve ser instalada. Se BHOLD Core está a ser instalado no Windows Server 2012, tem de estar instaladas ferramentas de scripts do IIS 6
- O .NET 3.5 framework tem de estar instalado.
  - Como BHOLD Core requer o .NET 3.5, não pode ser instalado no Server Core.
- O Silverlight 4 é exigido pelo outros módulos do BHOLD e, por isso, recomenda-se que o instale antes da instalação do BHOLD Core.
- Microsoft SQL Server 2014 ou o Microsoft SQL Server 2016 tem de estar instalado no servidor do BHOLD Core ou noutro servidor na LAN. 

Computadores cliente do Windows tem de executar versão do Microsoft Internet Explorer 6 ou posterior e a versão do Microsoft Silverlight 4 ou posterior.

## <a name="before-you-begin"></a>Antes de começar

O módulo do BHOLD Core requer uma conta de utilizador que é utilizada para autenticar e autorizar o serviço do BHOLD Core para o servidor e outras entidades de rede. Esta secção explica como criar e configurar essa conta de utilizador e fornece uma folha de cálculo de pré-instalação do que o ajudará a reunir as informações necessárias concluir a instalação do BHOLD Core.

>{! IMPORTANTE} quando instalar o BHOLD Core, pode utilizar uma base de dados existente do SQL Server como a base de dados do BHOLD Core ou pode permitir que o Assistente de instalação do BHOLD Core para criar a base de dados do BHOLD Core para. Se optar por utilizar uma base de dados existente, certifique-se de que nenhum outro usuário pode acessar o banco de dados enquanto estiver a instalar BHOLD Core. Antes de instalar o BHOLD Core, certifique-se de que os controlos de acesso da base de dados do SQL Server não permitir o acesso por qualquer pessoa que não seja o utilizador que está a instalar o BHOLD Core.

## <a name="required-user-and-group"></a>Necessária para o usuário e grupo

O módulo do BHOLD Core tem de ser capaz de iniciar sessão domínio com uma conta de utilizador que é dedicado a essa finalidade e que é um membro de dois grupos de segurança específicos, incluindo uma que é criada especificamente para o módulo do BHOLD Core. É necessário ter uma associação no grupo Admins do domínio para efetuar este procedimento.
**Para criar e configurar o grupo de segurança e utilizadores do BHOLD Core**

1.  No controlador de domínio, clique em **começar**, aponte para **ferramentas administrativas**e, em seguida, clique em **Active Directory Users and Computers**.

2.  Na árvore da consola, expanda o domínio onde a conta está a ser criado, clique com botão direito **usuários**, aponte para **New**e, em seguida, clique em **grupo**.

3.  Na **novo objeto – grupo** caixa de diálogo **nome do grupo**, escreva o nome do grupo (padrão do BHOLD: BHOLDApplicationGroup) e, em seguida, clique em **OK**.

4.  Com o botão direito **usuários**, aponte para **New**e, em seguida, clique em **utilizador**.

5.  Na **FullName**, escreva um nome que irá ajudá-lo a identificar a conta, por exemplo, BHOLD Core conta do serviço.

6.  Na **nome de início de sessão do utilizador**, escreva o nome de utilizador da conta de serviço do BHOLD Core (predefinição do BHOLD: b1user) e, em seguida, clique em **próxima**.

7.  Na **palavra-passe** e **Confirmar palavra-passe**, escreva a palavra-passe para a conta de serviço.

8.  Limpar **o utilizador deve alterar a palavra-passe no próximo início de sessão**, selecione **utilizador não é possível alterar a palavra-passe** e **palavra-passe nunca expira**, clique em **seguinte**, e, em seguida, clique em **concluir**.

9.  No painel de resultados da consola, faça duplo clique a conta de utilizador e, em seguida, clique em **adicionar a um grupo**.

10. Na **selecionar grupos** caixa de diálogo, escreva o nome a apresentar do grupo que criou anteriormente, escreva um ponto e vírgula (;) e, em seguida, escreva IIS_IUSRS.

11. Clique em **verificar nomes**e, em seguida, clique em **OK**.  

O procedimento a seguir deve ser executado no computador onde vai ser instalado o módulo do BHOLD Core. Tem de ter sessão iniciada como membro do grupo Admins do domínio para efetuar este procedimento.

## <a name="bhold-core-installation-worksheet"></a>Planilha de instalação do BHOLD Core

Antes de começar a instalar o módulo do BHOLD Core, precisa estar preparado para fornecer as informações que o Assistente de configuração do BHOLD Core necessita para concluir a instalação. A seguinte planilha ajudará a gravar essa informação para que estará pronto para fornecer quando for necessário. Cada secção corresponde a uma página do Assistente de configuração do BHOLD Core.

### <a name="account-settings"></a>Definições da conta

| **Item**                                    | **Descrição**                                                                                                                                                                                                                                                                                             | **Valor**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilize um fornecedor de segurança num computador/domínio** | Quando selecionada, especifica que a segurança de serviços de domínio do Active Directory irá controlar o acesso a BHOLD Core.                                                                                                                                                                                                  | Selecione a caixa de verificação. **Importante:** a instalação falhará se esta caixa de verificação não estiver selecionada.                                                                 |
| **Domínio**                                  | Especifica o domínio que contém o servidor BHOLD, a conta de serviço e o grupo de aplicações. **Importante:** especifique o nome de domínio com o nome NetBIOS do (curto), não o nome de domínio completamente qualificado (FQDN). Por exemplo, se o FQDN do domínio fabrikam.com, especifique o nome de domínio como CONTOSO. | Escreva o nome de domínio aqui:                                                                                                                                        |
| **Grupo de aplicações**                       | Especifica o nome do grupo de segurança que criou anteriormente no [utilizador e grupo necessários](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                                  | Escreva o nome do grupo aqui:                                                                                                                                         |
| **Utilizador do serviço**                            | Especifica o nome de início de sessão da conta de utilizador de serviço que criou anteriormente no [utilizador e grupo necessários](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                      | Escreva o nome da conta de utilizador aqui:                                                                                                                                  |
| **Palavra-passe**                                | Especifica a palavra-passe da conta de utilizador de serviço do BHOLD Core.                                                                                                                                                                                                                                              | Escrever a palavra-passe aqui: **importante:** Certifique-se de que tenha esta palavra-passe num local seguro oculto.                                                                |
| **IP/porta do Web site**                         | Especifica o número de porta e o endereço IP do Web site a ser criada no servidor de intranet. Altere o valor predefinido (\*) apenas se não pretender utilizar o mesmo endereço IP como o Web site predefinido. Altere o número de porta para uma porta disponível apenas se a predefinição da porta (5151) já está em utilização.             | Se utilizar um endereço IP não predefinidas do Web site predefinido, escreva-o aqui: se o número de porta predefinido já está em utilização, escreva o número de porta de Web site BHOLD aqui: |

### <a name="database-settings"></a>Definições de base de dados

| **Item**                                       | **Descrição**                                                                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilizar a segurança integrada**                    | Especifica que a autenticação do Windows é usada para acessar o banco de dados.                                                                                                                                                                                                     | Selecione a caixa de verificação se a autenticação do Windows é utilizada para ligar ao SQL Server. Desmarque a caixa de verificação se for utilizada a autenticação do SQL Server. A base de dados deve ter sido criada antes de executar o BHOLD Core configuração se autenticação do SQL Server é utilizada. **Nota:** se é utilizada a autenticação do Windows, deve fazer logon com uma conta que tenha a função de servidor sysadmin no servidor de base de dados. |
| **Utilizador de base de dados** e **palavra-passe de base de dados** | Especifica o nome de utilizador e palavra-passe de um utilizador com a função de servidor sysadmin no servidor de base de dados. Estes valores são fornecidos apenas quando é utilizada a autenticação do SQL Server.                                                                                               | Escrever o nome de utilizador do SQL Server: escrever a senha de usuário do SQL Server: **Nota:** Certifique-se de que tenha esta palavra-passe num local seguro oculto.                                                                                                                                                                                                                                                  |
| **Servidor de base de dados** e **nome de base de dados**   | Especifica o nome NetBIOS do servidor de base de dados e o nome da base de dados (predefinição: b1) que irá criar o programa de configuração do BHOLD Core. Se não estiver a utilizar a instância de servidor de base de dados predefinida, especifique a instância de servidor de base de dados no formato  *\<server\>*\\*\<instância\>* . | Escrita do servidor (ou servidor e instância) nome aqui: escrever aqui o nome da base de dados:                                                                                                                                                                                                                                                                                                                   |
| **Tornar restrições para o utilizador de base de dados**    | Obsoleto.                                                                                                                                                                                                                                                                 | Não altere o valor predefinido                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>O programa de configuração do BHOLD Core

Para instalar o módulo do BHOLD Core, iniciar sessão como membro do grupo Admins do domínio, transfira o ficheiro seguinte e executá-lo como administrador no servidor que pretende instalar o módulo do BHOLD Core num: 

- BholdCore  *\<versão\>*\_Release.msi

Substitua *\<versão\>* com o número de versão da versão principal do BHOLD que está a instalar.

Para executar o ficheiro de programa como administrador, o ficheiro com o botão direito e, em seguida, clique em **executar como administrador**.

### <a name="postinstallation-settings"></a>Definições de postinstallation

Depois de concluída a instalação do BHOLD Core, tem de configurar a Firewall do Windows e alterar as definições avançadas no pool de aplicativos do BHOLD Core nos serviços de informação de Internet para concluir a configuração do BHOLD Core. Se necessário, também deve alterar atributos do sistema BHOLD para satisfazer os seus requisitos.

#### <a name="configuring-windows-firewall"></a>Configurar a Firewall do Windows

Se os utilizadores irão aceder BHOLD com navegadores da web em computadores remotos, tem de configurar o Firewall do Windows no servidor do BHOLD Core para permitir ligações de entrada para a porta do Web site que especificou quando instalou o BHOLD Core.

Para efetuar este procedimento, tem de ser membro do grupo Administradores no computador local.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>Para permitir ligações de entrada para o site do BHOLD

1.  Clique em **começar**, aponte para **ferramentas administrativas**, clique com botão direito **Firewall do Windows com definições avançadas**e, em seguida, clique em **executar como administrador**.

2.  No painel esquerdo, clique em **regras de entrada**e, em seguida, no painel da direita, clique em **nova regra**.

3.  No Assistente de nova regra de entrada, clique em **porta**e, em seguida, clique em **próxima**.

4.  Certifique-se de que **TCP** estiver selecionada, na **portas locais específicas**, escreva o número de porta do BHOLD Core predefinido (5151) ou o número de porta que especificou quando instalou o BHOLD Core e, em seguida, clique em  **Próxima**.

5.  Certifique-se de que **permitir a conexão** está selecionada e, em seguida, clique em **próxima**.

6.  Sobre o **perfil** página, desmarque as caixas de verificação de localizações a partir do qual não deseja acessar o site do BHOLD e, em seguida, clique em **próxima**.

7.  Sobre o **Name** página, escreva um nome para a regra (por exemplo, permitir ligações de entrada BHOLD Core) e, em seguida, clique em **concluir**.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>Permitindo que os aplicativos de 32 bits para o conjunto aplicacional do BHOLD Core

Para permitir que o IIS para funcionar corretamente com o módulo do BHOLD Core, tem de configurar o conjunto aplicacional do BHOLD Core para oferecer suporte a aplicativos de 32 bits. Deve estar conectado com a conta utilizada para instalar o módulo do BHOLD Core para efetuar este procedimento.

**Para ativar o suporte técnico da aplicação de 32 bits para o conjunto aplicacional do BHOLD Core**

1.  Para abrir o Gestor de serviços de informação de Internet, clique em **começar**, aponte para **ferramentas de administração**e, em seguida, clique em **Gestor dos serviços de informação Internet (IIS)**.

2.  Na árvore da consola, expanda o nome do servidor e, em seguida, clique em **Pools de aplicativos**.

3.  Na **Pools de aplicativos** lista, clique com botão direito **CoreAppPool**e, em seguida, clique em **definições avançadas**.

4.  Na **definições avançadas** caixa de diálogo a **aplicativos ativar 32 bits** lista, selecione **True**e, em seguida, clique em **OK**.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>Estabelecer o nome do principal de serviço para o site do BHOLD

Se o nome de rede que serve para contactar o site do BHOLD não é igual ao nome de anfitrião do servidor, tem de estabelecer um nome de principal de serviço (SPN) para HTTP. Por exemplo, se usar um registo de recursos CNAME no DNS para especificar um alias para o servidor, ou se utilizar o balanceamento de carga na rede, tem de registar estes endereços de rede adicionais no Active Directory. Se não conseguir fazê-lo, o Internet Explorer não pode utilizar o protocolo Kerberos ao contactar o site do BHOLD.

> [!IMPORTANT]
> Se o módulo do BHOLD Core está instalado no mesmo computador que o Portal do FIM, tem de criar registos de recursos DNS (CNAME ou A) com nomes de anfitrião diferente para os servidores que executam BHOLD Core e o servidor que executa o Portal do FIM. Apenas um SPN pode ser estabelecida para um determinado par de serviço-tipo/servidor-alias e então BHOLD Core e o Portal do FIM requerem SPNs separadas porque eles normalmente são executados sob contas diferentes. O comando setspn relata um erro, se já tiver sido estabelecido um SPN em outra conta.

A associação **Admins do domínio**, ou equivalente, é o mínimo requerido para concluir este procedimento.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>Para estabelecer o SPN do site do BHOLD

1.  No controlador de domínio de serviços de domínio do Active Directory, clique em **começar**, clique em **todos os programas**, clique em **acessórios**, clique com botão direito **linha de comandos** e, em seguida, clique em **executar como administrador**.

2.  Na linha de comandos, escreva o seguinte comando e, em seguida, prima ENTER: setspn – S HTTP / *\<networkalias\> \<domínio\>* \\ *\<accountname\>* onde:

    -   *\<networkalias\>*  é o endereço que os clientes utilizam para contactar o site do BHOLD

    -   *\<domínio\>*\\*\<accountname\>*  é o nome de utilizador e domínio da conta de serviço do BHOLD Core que criou quando instalou o BHOLD Core.

3.  Repita o passo anterior para todos os outros nomes de que os clientes utilizam para contactar o site do BHOLD, por exemplo, CNAME aliases, nomes, que incluem um nome de domínio completamente qualificado ou nomes que incluem um nome de domínio do (curto) do NetBIOS.

#### <a name="setting-bhold-system-attributes"></a>Definição de atributos de sistema do BHOLD

Para validar que a instalação do módulo do BHOLD Core foi concluída com êxito, abra o portal do BHOLD Core e ver os atributos do sistema. Além disso, para garantir que as funções de módulo do BHOLD Core corretamente no seu ambiente, pode modificar os seguintes atributos de sistema do BHOLD, conforme apropriado:

| **Atributo**                | **Descrição**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **NoHistory**                | Defina como Y, se o Web site do BHOLD está em execução num serviço web em cluster para garantir que os itens exibidos recentemente funcionem corretamente. Definido como N se o Web site do BHOLD está em execução num servidor IIS autônomo.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Defina como Y para garantir que as unidades organizacionais (orgunits) podem ser movidas apenas a orgunits com o mesmo tipo organizacional, como o orgunit principal. Por exemplo, isto impede que um orgunit de projeto que está a ser movido para um departamento orgunit. Definido como N para permitir que um orgunit sejam posicionadas num orgunit de um tipo diferente. |
| **Executam de dias entre ABA**     | Definido como um inteiro de dois dígitos, para especificar o intervalo (em dias), entre duas execuções de autorização baseada em atributo (ABA). Por exemplo, para especificar que as execuções de ABA ficarão separadas por dois dias, digite 02.                                                                                                                     |
| **Hora de início da ABA executar**    | Definido como um inteiro de dois dígitos para especificar a hora do dia quando ocorrerá uma execução de autorização baseadas em atributos. Por exemplo especificar que o ABA executar terá lugar em 11 meio-dia (23:00), escreva a 23.                                                                                                             |
| **Cardinalidade de sistema**       | Definido como N se não pretender que uma verificação de cardinalidade do sistema no BHOLD. O valor predefinido é Y.                                                                                                                                                                                                                             |
| **Registro em log**                  | Definido como N se não pretender que as alterações de ter sessão iniciada. O valor predefinido é Y.                                                                                                                                                                                                                                            |
| **Processamento de SystemQueue**   | Definido como N se não pretender que o processamento do sistema de fila. Não altere este valor, a menos que direcionado a fazer isso, o suporte do produto.                                                                                                                                                                                           |

Tem de ter sessão iniciada como membro do grupo Admins do domínio para efetuar este procedimento.

**Para definir um atributo de sistema do BHOLD**

1.  Clique em **começar**, clique em **todos os programas**e, em seguida, clique em **Internet Explorer**.

2.  Na caixa de endereço, digite, em que *\<server\>* é o nome do servidor de Web site do BHOLD e *\<porta\>* é o número da porta é vinculado ao Web site.

3.  Clique em **home page**, clique em **valores**e, em seguida, clique em **modificar**.

4.  Localize o nome do atributo que pretende alterar, escreva o novo valor na caixa junto ao nome do atributo e, em seguida, clique em **OK**.

## <a name="next-steps"></a>Passos Seguintes

Depois de ter instalado o BHOLD Core e validar que a instalação foi concluída com êxito, pode instalar módulos adicionais. Neste momento, a base de dados do BHOLD estará essencialmente vazio, que contém apenas uma conta de utilizador, a conta de raiz e uma unidade organizacional (orgunit), o orgunit de raiz. Para adicionar mais utilizadores para a base de dados do BHOLD, pode instalar o módulo de conector de gestão de acesso ou o módulo do BHOLD Model Generator, consoante as suas necessidades. Pode utilizar o módulo de conector de gestão de acesso para importar dados de utilizador do serviço de sincronização do FIM ou pode utilizar o gerador de modelo do BHOLD para importar dados de utilizador de um conjunto de ficheiros estruturados. Para obter mais informações sobre como utilizar o módulo de conector de gestão de acesso, consulte [guia de laboratório de teste: conector do BHOLD Access Management](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

Para obter mais informações sobre como utilizar o módulo do BHOLD Model Generator, consulte:

- [Guia de conceitos do Microsoft BHOLD Suite](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx)
- [Microsoft BHOLD Suite TechnicalReference](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx).
