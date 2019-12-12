---
title: Instalação do BHOLD Core | Microsoft Docs
description: Documento principal de instalação do BHOLD Suite
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/07/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 48ff4a06233ba95288432c4cfe48e37b4d1449ab
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "64519160"
---
# <a name="bhold-core-installation"></a>Instalação do BHOLD Core

O módulo BHOLD core fornece os principais recursos do BHOLD Suite em seu ambiente. O módulo BHOLD Core deve ser instalado e configurado em um servidor em sua rede local antes que você possa instalar outros módulos do BHOLD Suite.

## <a name="bhold-core-installation-requirements"></a>Requisitos de instalação do BHOLD Core

O módulo BHOLD Core forma a base do Microsoft BHOLD Suite. Você deve instalar o módulo BHOLD Core antes de instalar outros módulos do Microsoft BHOLD Suite.

### <a name="bhold-core-hardware-requirements"></a>Requisitos de hardware do BHOLD Core

O módulo BHOLD Core forma a base do Microsoft BHOLD Suite. Você deve instalar o módulo BHOLD Core antes de instalar outros módulos do Microsoft BHOLD Suite.

|          |        |          |
|----------|--------|----------|
|**Componente** |**Máximo** | **Recomendado** |
|Processador | processador de 64 bits | Processador de vários núcleos de 64 bits |
| Memória |3 GB | 6 GB ou mais |
|Armazenamento| 30 GB disponíveis |Depende do tamanho da implantação |
|Adaptador de rede| conexão de 100 Mbps ao SQL e ao servidor do Forefront Identity Manager (FIM) | conexão do 1 Gbps com o SQL e o servidor do FIM|

Essas recomendações se baseiam em implementações típicas e não levam em consideração outros aplicativos em execução no servidor. Talvez seja necessário usar componentes de maior desempenho, dependendo de seu ambiente específico.

### <a name="bhold-core-software-requirements"></a>Requisitos de software do BHOLD Core

O módulo BHOLD Core deve ser instalado em um computador que atenda aos seguintes requisitos:

- O servidor deve estar executando o Windows Server 2012 (64 bits), Windows Server 2016 
- O servidor deve ser membro de um domínio de Active Directory Domain Services (AD DS). Em ambientes de teste, o servidor pode ser um controlador de domínio AD DS.
- O BHOLD Core deve ser instalado por um usuário conectado com uma conta no mesmo domínio que o servidor e que pertence ao grupo Admins. do domínio no domínio e ao grupo Administradores no servidor.
- O Microsoft Serviços de Informações da Internet (IIS) com ASP.NET deve ser instalado no servidor. O IIS deve ser configurado com a autenticação do Windows habilitada. Se o BHOLD Core estiver sendo instalado no Windows Server 2012/2016, a compatibilidade de gerenciamento do IIS 6 deverá ser instalada. Se o BHOLD Core estiver sendo instalado no Windows Server 2012, as ferramentas de script do IIS 6 deverão ser instaladas
- A estrutura .NET 3,5 deve ser instalada.
  - Como o BHOLD Core requer o .NET 3,5, ele não pode ser instalado no Server Core.
- O Silverlight 4 é exigido por outros módulos do BHOLD e, portanto, é recomendável instalá-lo antes de instalar o BHOLD Core.
- Microsoft SQL Server 2014 ou Microsoft SQL Server 2016 deve ser instalado no servidor BHOLD core ou em outro servidor na LAN. 

Os computadores cliente do Windows devem estar executando o Microsoft Internet Explorer versão 6 ou posterior e o Microsoft Silverlight versão 4 ou posterior.

## <a name="before-you-begin"></a>Antes de começar

O módulo BHOLD Core requer uma conta de usuário que é usada para autenticar e autorizar o serviço do BHOLD Core ao servidor e a outras entidades de rede. Esta seção explica como criar e configurar essa conta de usuário e fornece uma planilha de pré-instalação que ajudará você a coletar as informações necessárias para concluir a instalação do BHOLD Core.

>{! IMPORTANTE} ao instalar o BHOLD Core, você pode usar um banco de dados SQL Server existente como o banco de dados do BHOLD core ou pode permitir que o assistente de instalação do BHOLD Core crie o banco de dados do BHOLD Core para você. Se você optar por usar um banco de dados existente, deverá garantir que nenhum outro usuário possa acessar o banco de dados enquanto você estiver instalando o BHOLD Core. Antes de instalar o BHOLD Core, verifique se os controles de acesso do banco de dados SQL Server não permitem o acesso por qualquer pessoa que não seja o usuário que está instalando o BHOLD Core.

## <a name="required-user-and-group"></a>Usuário e grupo necessários

O módulo BHOLD Core deve ser capaz de fazer logon no domínio com uma conta de usuário dedicada a essa finalidade e que é membro de dois grupos de segurança específicos, incluindo um que é criado especificamente para o módulo BHOLD Core. A associação no grupo Admins. do domínio é necessária para executar esse procedimento.
**Para criar e configurar o usuário do BHOLD Core e o grupo de segurança**

1.  Em um controlador de domínio, clique em **Iniciar**, aponte para **Ferramentas administrativas**e clique em **Active Directory usuários e computadores**.

2.  Na árvore de console, expanda o domínio em que a conta deve ser criada, clique com o botão direito do mouse em **usuários**, aponte para **novo**e clique em **grupo**.

3.  Na caixa de diálogo **novo objeto – grupo** , em **nome do grupo**, digite o nome do grupo (BHOLD padrão: BHOLDApplicationGroup) e clique em **OK**.

4.  Clique com o botão direito do mouse em **usuários**, aponte para **novo**e clique em **usuário**.

5.  Em **nome completo**, digite um nome que ajudará a identificar a conta, por exemplo, a conta de serviço do BHOLD Core.

6.  Em **nome de logon do usuário**, digite o nome de usuário da conta de serviço do BHOLD Core (BHOLD padrão: b1user) e clique em **Avançar**.

7.  Em **senha** e **Confirmar senha**, digite a senha da conta de serviço.

8.  Limpar **usuário deve alterar a senha no próximo logon**, selecionar o **usuário não pode alterar a senha** e a **senha nunca expira**, clique em **Avançar**e, em seguida, clique em **concluir**.

9.  No painel de resultados do console, clique com o botão direito do mouse na conta de usuário e clique em **Adicionar a um grupo**.

10. Na caixa de diálogo **Selecionar grupos** , digite o nome de exibição do grupo que você criou anteriormente, digite um ponto e vírgula (;) e, em seguida, digite IIS_IUSRS.

11. Clique em **verificar nomes**e em **OK**.  

O procedimento a seguir deve ser executado no computador em que o módulo BHOLD Core será instalado. Você deve estar conectado como um membro do grupo Admins. do domínio para executar esse procedimento.

## <a name="bhold-core-installation-worksheet"></a>Planilha de instalação do BHOLD Core

Antes de começar a instalar o módulo BHOLD Core, você precisa estar preparado para fornecer as informações que o assistente de instalação do BHOLD Core exige para concluir a instalação. A planilha a seguir o ajudará a registrar essas informações para que você fique pronto para fornecê-las quando necessário. Cada seção corresponde a uma página no assistente de instalação do BHOLD Core.

### <a name="account-settings"></a>Definições da conta

| **Item**                                    | **Descrição**                                                                                                                                                                                                                                                                                             | **Valor**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Usar o provedor de segurança em domínio/computador** | Quando selecionado, especifica que Active Directory Domain Services segurança controlará o acesso ao BHOLD Core.                                                                                                                                                                                                  | Selecione a caixa de verificação. **Importante:** A instalação falhará se essa caixa de seleção não estiver marcada.                                                                 |
| **Domínio**                                  | Especifica o domínio que contém o servidor BHOLD, a conta de serviço e o grupo de aplicativos. **Importante:** Especifique o nome de domínio usando o nome NetBIOS (curto), não o FQDN (nome de domínio totalmente qualificado). Por exemplo, se o FQDN do domínio for fabrikam.com, especifique o nome de domínio como CONTOSO. | Escreva o nome de domínio aqui:                                                                                                                                        |
| **Grupo de aplicativos**                       | Especifica o nome do grupo de segurança que você criou anteriormente em [usuário e grupo necessários](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                                  | Escreva o nome do grupo aqui:                                                                                                                                         |
| **Usuário de serviço**                            | Especifica o nome de logon da conta de usuário de serviço que você criou anteriormente em [usuário e grupo necessários](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx#rug).                                                                                                                      | Escreva o nome da conta de usuário aqui:                                                                                                                                  |
| **Palavra-passe**                                | Especifica a senha da conta de usuário do serviço do BHOLD Core.                                                                                                                                                                                                                                              | Escreva a senha aqui: **importante:** Lembre-se de manter essa senha em um local oculto e seguro.                                                                |
| **IP/porta do site**                         | Especifica o endereço IP e o número da porta do site a ser criado no servidor de intranet. Altere o valor padrão (\*) somente se você não usar o mesmo endereço IP que o site da Web padrão. Altere o número da porta para uma porta disponível somente se a porta padrão (5151) já estiver em uso.             | Se um endereço IP não padrão for usado pelo site padrão, escreva-o aqui: se o número da porta padrão já estiver em uso, escreva o número da porta do site BHOLD aqui: |

### <a name="database-settings"></a>Configurações do banco de dados

| **Item**                                       | **Descrição**                                                                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Usar segurança integrada**                    | Especifica que a autenticação do Windows é usada para acessar o banco de dados.                                                                                                                                                                                                     | Marque a caixa de seleção se a autenticação do Windows for usada para se conectar ao SQL Server. Desmarque a caixa de seleção se SQL Server autenticação for usada. O banco de dados deve ter sido criado antes da execução da instalação do BHOLD Core se SQL Server autenticação for usada. **Observação:** Se a autenticação do Windows for usada, você deverá estar conectado com uma conta que tenha a função de servidor sysadmin no servidor de banco de dados. |
| **Usuário do banco de dados** e  **senha do banco de dados** | Especifica o nome de usuário e a senha de um usuário com a função de servidor sysadmin no servidor de banco de dados. Esses valores são fornecidos somente quando SQL Server autenticação é usada.                                                                                               | Escreva o nome de usuário do SQL Server aqui: escreva a senha do usuário do SQL Server aqui: **Observação:** Lembre-se de manter essa senha em um local oculto e seguro.                                                                                                                                                                                                                                                  |
| **Servidor de banco de dados** e  **nome do banco de dados**   | Especifica o nome NetBIOS do servidor de banco de dados e o nome do banco de dados (padrão: B1) que a instalação do BHOLD Core criará. Se você não estiver usando a instância padrão do servidor de banco de dados, especifique a instância do servidor de banco de dados no formulário *\<server\>* \\*instância\<\>* . | Escreva o nome do servidor (ou servidor e instância) aqui: escreva o nome do banco de dados aqui:                                                                                                                                                                                                                                                                                                                   |
| **Fazer restrições para o usuário do banco de dados**    | Obsoleto.                                                                                                                                                                                                                                                                 | Não altere o valor padrão                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>Configuração do BHOLD Core

Para instalar o módulo BHOLD Core, faça logon como um membro do grupo Admins. do domínio, baixe o arquivo a seguir e execute-o como administrador no servidor no qual você pretende instalar o módulo BHOLD Core: 

- BholdCore *\<versão\>* \_Release. msi

Substitua *\<versão\>* pelo número de versão da versão principal do BHOLD que você está instalando.

Para executar o arquivo de programa como administrador, clique com o botão direito do mouse no arquivo e clique em **Executar como administrador**.

### <a name="postinstallation-settings"></a>Configurações de pós-instalação

Após a conclusão da instalação do BHOLD Core, você deve configurar o Firewall do Windows e alterar as configurações avançadas no pool de aplicativos do BHOLD Core em Serviços de Informações da Internet para concluir a configuração do BHOLD Core. Se necessário, você também deve alterar os atributos do sistema BHOLD para atender às suas necessidades.

#### <a name="configuring-windows-firewall"></a>Configuração do Firewall do Windows

Se os usuários acessarão o BHOLD usando navegadores da Web em computadores remotos, será necessário configurar o Firewall do Windows no servidor do BHOLD Core para permitir conexões de entrada para a porta do site que você especificou quando instalou o BHOLD Core.

Para executar esse procedimento, você deve ser um membro do grupo Administradores no computador local.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>Para permitir conexões de entrada para o site do BHOLD

1.  Clique em **Iniciar**, aponte para **Ferramentas administrativas**, clique com o botão direito do mouse em **Firewall do Windows com configurações avançadas**e clique em **Executar como administrador**.

2.  No painel esquerdo, clique em **regras de entrada**e, no painel direito, clique em **nova regra**.

3.  No Assistente para nova regra de entrada, clique em **porta**e, em seguida, clique em **Avançar**.

4.  Verifique se **TCP** está selecionado, em **portas locais específicas**, digite o número da porta padrão do BHOLD Core (5151) ou o número da porta que você especificou quando instalou o BHOLD Core e clique em **Avançar**.

5.  Certifique-se de que **permitir a conexão** esteja selecionado e clique em **Avançar**.

6.  Na página **perfil** , desmarque as caixas de seleção dos locais dos quais você não deseja acessar o site do BHOLD e clique em **Avançar**.

7.  Na página **nome** , digite um nome para a regra (como permitir conexões de entrada para o BHOLD Core) e clique em **concluir**.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>Habilitando aplicativos de 32 bits para o pool de aplicativos do BHOLD Core

Para permitir que o IIS funcione corretamente com o módulo BHOLD Core, você deve configurar o pool de aplicativos do BHOLD Core para dar suporte a aplicativos de 32 bits. Você deve estar conectado com a conta usada para instalar o módulo BHOLD Core para executar esse procedimento.

**Para habilitar o suporte a aplicativos de 32 bits para o pool de aplicativos do BHOLD Core**

1.  Para abrir o Gerenciador de Serviços de Informações da Internet, clique em **Iniciar**, aponte para **Ferramentas do administrador**e clique em **Gerenciador do serviços de informações da Internet (IIS)** .

2.  Na árvore de console, expanda o nome do servidor e clique em **pools de aplicativos**.

3.  Na lista **pools de aplicativos** , clique com o botão direito do mouse em **CoreAppPool**e clique em **Configurações avançadas**.

4.  Na caixa de diálogo **Configurações avançadas** , na lista **habilitar aplicativos de 32 bits** , selecione **verdadeiro**e clique em **OK**.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>Estabelecendo o nome da entidade de serviço para o site BHOLD

Se o nome de rede usado para contatar o site BHOLD não for o mesmo que o nome de host do servidor, você deverá estabelecer um SPN (nome da entidade de serviço) para HTTP. Por exemplo, se você usar um registro de recurso CNAME no DNS para especificar um alias para o servidor ou se usar o balanceamento de carga de rede, deverá registrar esses endereços de rede adicionais no Active Directory. Se você não conseguir fazer isso, o Internet Explorer não poderá usar o protocolo Kerberos ao entrar em contato com o site BHOLD.

> [!IMPORTANT]
> Se o módulo BHOLD Core estiver instalado no mesmo computador que o portal do FIM, você deverá criar registros de recurso DNS (CNAME ou A) com nomes de host diferentes para os servidores que executam o BHOLD Core e o servidor que executa o portal do FIM. Somente um SPN pode ser estabelecido para um par de Service-Type/Server-alias específico e, portanto, o BHOLD Core e o portal do FIM exigem SPNs separados, pois normalmente são executados em contas diferentes. O comando SetSPN relatará um erro se um SPN já tiver sido estabelecido em outra conta.

A associação em **Admins**. do domínio, ou equivalente, é o mínimo necessário para concluir este procedimento.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>Para estabelecer o SPN do site do BHOLD

1.  Na Active Directory Domain Services controlador de domínio, clique em **Iniciar**, **todos os programas**, **acessórios**, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.

2.  No prompt de comando, digite o seguinte comando e pressione ENTER: SetSPN – S HTTP/ *\<networkalias\> \<domínio\>*  \\ *\<AccountName\>* em que:

    -   *\<networkalias\>* é o endereço que os clientes usam para entrar em contato com o site do BHOLD

    -   *\<\>de domínio*\\ *\<AccountName\>* é o nome de usuário e domínio da conta de serviço do BHOLD Core que você criou quando instalou o BHOLD Core.

3.  Repita a etapa anterior para todos os outros nomes que os clientes usam para entrar em contato com o site BHOLD, por exemplo, aliases CNAME, nomes que incluem um nome de domínio totalmente qualificado ou nomes que incluem um nome de domínio NetBIOS (curto).

#### <a name="setting-bhold-system-attributes"></a>Definindo atributos do sistema BHOLD

Para validar que a instalação do módulo BHOLD Core foi bem-sucedida, abra o portal do BHOLD Core e exiba os atributos do sistema. Além disso, para garantir que o módulo BHOLD Core funcione corretamente em seu ambiente, você pode modificar os seguintes atributos do sistema BHOLD, conforme apropriado:

| **Attribute**                | **Descrição**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nohistory**                | Defina como Y se o site do BHOLD estiver em execução em um serviço Web clusterizado para garantir que os itens exibidos recentemente funcionem corretamente. Defina como N se o site do BHOLD estiver em execução em um servidor IIS autônomo.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Defina como Y para garantir que as unidades organizacionais (orgunits) possam ser movidas somente para orgunits com o mesmo tipo organizacional que o orgunit pai. Por exemplo, isso impede que um projeto orgunit seja movido para um orgunit de departamento. Defina como N para permitir que um orgunit seja colocado em um orgunit de um tipo diferente. |
| **Dias entre a execução do ABA**     | Defina como um inteiro de dois dígitos para especificar o intervalo (em dias) entre duas execuções de ABA (autorização baseada em atributo). Por exemplo, para especificar que as execuções de ABA serão separadas por dois dias, digite 02.                                                                                                                     |
| **Hora de início da execução do ABA**    | Defina como um inteiro de dois dígitos para especificar a hora do dia em que uma execução de autorização baseada em atributo ocorrerá. Por exemplo, para especificar que a execução de ABA ocorrerá às 11:00 p.m. (23:00), tipo 23.                                                                                                             |
| **Cardinalidade do sistema**       | Defina como N se você não quiser uma verificação de cardinalidade do sistema em BHOLD. O valor padrão é Y.                                                                                                                                                                                                                             |
| **Registro em log**                  | Defina como N se você não quiser que as alterações sejam registradas. O valor padrão é Y.                                                                                                                                                                                                                                            |
| **Processamento de SystemQueue**   | Defina como N se você não quiser o processamento de fila do sistema. Não altere esse valor, a menos que seja instruído a fazer isso pelo suporte do produto.                                                                                                                                                                                           |

Você deve estar conectado como um membro do grupo Admins. do domínio para executar esse procedimento.

**Para definir um atributo de sistema BHOLD**

1.  Clique em **Iniciar**, **todos os programas**e, em seguida, clique em **Internet Explorer**.

2.  Na caixa endereço, digite, em que *\<server\>* é o nome do servidor do site BHOLD e *\<porta\>* é o número da porta associada ao site.

3.  Clique em **início**, clique em **valores**e, em seguida, clique em **Modificar**.

4.  Localize o nome do atributo que você deseja alterar, digite o novo valor na caixa ao lado do nome do atributo e clique em **OK**.

## <a name="next-steps"></a>Próximos passos

Depois de instalar o BHOLD Core e validar que a instalação foi bem-sucedida, você pode instalar módulos adicionais. Neste ponto, o banco de dados BHOLD estará essencialmente vazio, contendo apenas uma conta de usuário, a conta raiz e uma unidade organizacional (orgunit), o orgunit raiz. Para adicionar mais usuários ao banco de dados BHOLD, você pode instalar o módulo do conector de gerenciamento de acesso ou o módulo gerador de modelos do BHOLD, dependendo de suas necessidades. Você pode usar o módulo do conector de gerenciamento de acesso para importar dados de usuário do serviço de sincronização do FIM ou pode usar o gerador de modelo BHOLD para importar dados de usuário de um conjunto de arquivos estruturados. Para obter mais informações sobre como usar o módulo do conector de gerenciamento de acesso, consulte [Guia de laboratório de teste: conector de gerenciamento de acesso do BHOLD](https://technet.microsoft.com/library/jj853085(v=ws.10).aspx).

Para obter mais informações sobre como usar o módulo gerador de modelos BHOLD, consulte:

- [Guia de conceitos do Microsoft BHOLD Suite](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx)
- [Microsoft BHOLD Suite TechnicalReference](https://technet.microsoft.com/library/jj134935(v=ws.10).aspx).
