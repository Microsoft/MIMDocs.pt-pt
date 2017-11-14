---
title: "Core BHOLD instalação | Microsoft Docs"
description: "Documento de núcleos de instalação do BHOLD suite"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 33fbe63528d5d7c543ae286f934654538782b4d5
ms.sourcegitcommit: 2be26acadf35194293cef4310950e121653d2714
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2017
---
# <a name="bhold-core-installation"></a>Instalação do núcleo do BHOLD

O módulo de núcleo do BHOLD fornece as principais funcionalidades de BHOLD Suite no seu ambiente. O módulo de núcleo do BHOLD tem de ser instalado e configurado num servidor na sua rede de área local antes de poder instalar outros módulos de BHOLD Suite.

## <a name="bhold-core-installation-requirements"></a>Requisitos de instalação do núcleo do BHOLD

O módulo de núcleo do BHOLD compõe a base de BHOLD Suite de Microsoft. Tem de instalar o módulo de núcleo do BHOLD antes de instalar outros módulos de BHOLD Suite de Microsoft.

### <a name="bhold-core-hardware-requirements"></a>Requisitos de hardware de núcleo do BHOLD

O módulo de núcleo do BHOLD compõe a base de BHOLD Suite de Microsoft. Tem de instalar o módulo de núcleo do BHOLD antes de instalar outros módulos de BHOLD Suite de Microsoft.

|          |        |          |
|----------|--------|----------|
|**Componente** |**Mínimo** | **Recomendado** |
|Processador | processador de 64 bits | Processador de 64 bits especializado |
| Memória |3 GB | 6 GB ou mais |
|Armazenamento| 30 GB disponível |Depende do tamanho de implementação |
|Adaptador de rede| 100 Mbps ligação ao servidor SQL e Forefront Identity Manager (FIM) | Ligação de 1 Gbps ao servidor SQL e de FIM|

Estas recomendações baseiam-se em implementações normais e não tem em consideração outras aplicações em execução no servidor. Poderá ter de utilizar componentes de desempenho superior, dependendo do seu ambiente particular.

### <a name="bhold-core-software-requirements"></a>Requisitos de software do núcleo do BHOLD

O módulo de núcleo do BHOLD tem de estar instalado num computador que cumpra os requisitos seguintes:

- O servidor tem de ser o Windows Server 2012 (64 bits), Windows Server 2016 
- O servidor tem de ser um membro de um domínio de serviços de domínio do Active Directory (AD DS). Em ambientes de teste, o servidor pode ser um controlador de domínio do AD DS.
- BHOLD Core tem de ser instalado por um utilizador com sessão iniciado com uma conta no mesmo domínio que o servidor e a que pertence ao grupo Admins do domínio no domínio e ao grupo de administradores no servidor.
- Microsoft informações de serviços Internet (IIS) com o ASP.NET tem de estar instalado no servidor. IIS tem de ser configurado com autenticação do Windows activada. Se o principal do BHOLD está a ser instalado no Windows Server 2012/2016, compatibilidade de gestão do IIS 6 tem de estar instalada. Se o principal do BHOLD está a ser instalado no Windows Server 2012, tem de estar instaladas as ferramentas de script do IIS 6
- O .NET 3.5 framework tem de estar instalado.
  - Uma vez BHOLD Core requer o .NET 3.5, não pode ser instalado no Server Core.
- Silverlight 4 é necessário por outros módulos BHOLD e, por isso, recomenda-se instalá-lo antes de instalar BHOLD Core.
- Microsoft SQL Server 2014 ou Microsoft SQL Server 2016 tem de estar instalado no servidor principal do BHOLD ou noutro servidor de LAN. 

Computadores de cliente Windows devem executar a versão do Microsoft Internet Explorer 6 ou posterior e versão do Microsoft Silverlight 4 ou posterior.

## <a name="before-you-begin"></a>Antes de começar

O módulo de núcleo do BHOLD requer uma conta de utilizador que é utilizada para autenticar e autorizar o serviço de núcleo do BHOLD para o servidor e outras entidades de rede. Esta secção explica como criar e configurar essa conta de utilizador e fornece uma folha de cálculo de pré-instalação ajudará a recolher as informações necessárias concluir o programa de configuração do BHOLD Core.

>{! IMPORTANTE} quando instalar o núcleo do BHOLD, pode utilizar uma base de dados existente do SQL Server como a base de dados do BHOLD núcleos ou pode permitir que o Assistente de configuração de núcleos de BHOLD criar a base de dados do BHOLD principal para si. Se optar por utilizar uma base de dados existente, certifique-se de que não existem outros utilizadores podem aceder a base de dados enquanto estiver a instalar BHOLD Core. Antes de instalar o núcleo do BHOLD, certifique-se de que os controlos de acesso da base de dados do SQL Server não permitir o acesso ao utilizador que está a instalar o núcleo do BHOLD diferente.

## <a name="required-user-and-group"></a>Necessário de utilizador e grupo

O módulo de núcleo do BHOLD tem de ser capaz de iniciar sessão domínio com uma conta de utilizador que se encontra dedicado para essa finalidade e que é um membro de dois grupos de segurança específicos, incluindo um criado especificamente para o módulo de núcleo do BHOLD. A associação ao grupo Admins do domínio é necessário para efetuar este procedimento.
**Para criar e configurar o grupo de segurança e de utilizador principal do BHOLD**

1.  Num controlador de domínio, clique em **iniciar**, aponte para **ferramentas administrativas**e, em seguida, clique em **computadores e utilizadores do Active Directory**.

2.  Na árvore da consola, expanda o domínio onde a conta está a ser criado, clique com botão direito **utilizadores**, aponte para **novo**e, em seguida, clique em **grupo**.

3.  No **novo objeto – grupo** caixa de diálogo **nome do grupo**, escreva o nome do grupo (predefinição do BHOLD: BHOLDApplicationGroup) e, em seguida, clique em **OK**.

4.  Clique com botão direito **utilizadores**, aponte para **novo**e, em seguida, clique em **utilizador**.

5.  No **nome completo**, escreva um nome que o irão ajudar a identificar a conta, por exemplo, BHOLD conta de serviço de núcleo.

6.  No **nome de início de sessão do utilizador**, escreva o nome de utilizador da conta de serviço de núcleo do BHOLD (predefinição do BHOLD: b1user) e, em seguida, clique em **seguinte**.

7.  No **palavra-passe** e **Confirmar palavra-passe**, escreva a palavra-passe da conta de serviço.

8.  Limpar **o utilizador deve alterar a palavra-passe no próximo início de sessão**, selecione **utilizador não é possível alterar a palavra-passe** e **palavra-passe nunca expira**, clique em **seguinte**, e, em seguida, clique em **concluir**.

9.  No painel de resultados da consola, faça duplo clique a conta de utilizador e, em seguida, clique em **adicionar a um grupo**.

10. No **selecionar grupos** caixa de diálogo, escreva o nome a apresentar do grupo que criou anteriormente, escreva um ponto e vírgula (;) e, em seguida, escreva IIS_IUSRS.

11. Clique em **verificar nomes**e, em seguida, clique em **OK**.  

O procedimento seguinte deve ser executado no computador onde o módulo de núcleo do BHOLD está a ser instalado. Tem de ter sessão iniciada como membro do grupo Admins do domínio para efetuar este procedimento.

## <a name="bhold-core-installation-worksheet"></a>Folha de cálculo de instalação de núcleo do BHOLD

Antes de começar a instalar o módulo do BHOLD Core, tem de estar preparado para fornecer as informações que o Assistente de configuração de núcleos de BHOLD necessita para concluir a instalação. A folha de cálculo seguinte irão ajudá-lo, registe essa informação para estará pronto para fornecê-lo quando for necessário. Cada secção corresponde a uma página no Assistente de configuração de núcleos de BHOLD.

### <a name="account-settings"></a>Definições da conta

| **Item**                                    | **Descrição**                                                                                                                                                                                                                                                                                             | **Valor**                                                                                                                                                          |
|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilize um fornecedor de segurança no computador/domínio** | Quando selecionada, especifica que a segurança dos serviços de domínio do Active Directory controlará o acesso para BHOLD núcleo.                                                                                                                                                                                                  | Selecione a caixa de verificação. **Importante:** a instalação falhará se esta caixa de verificação não estiver selecionada.                                                                 |
| **Domínio**                                  | Especifica o domínio que contém o servidor BHOLD, a conta de serviço e o grupo de aplicações. **Importante:** especifique o nome de domínio com o nome de NetBIOS (abreviado), não o nome de domínio completamente qualificado (FQDN). Por exemplo, se o FQDN do domínio for fabrikam.com, especifique o nome de domínio como CONTOSO. | Escreva o nome de domínio aqui:                                                                                                                                        |
| **Grupo de aplicações**                       | Especifica o nome do grupo de segurança que criou anteriormente no [utilizador e grupo necessários](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx#rug).                                                                                                                                  | Escreva o nome do grupo aqui:                                                                                                                                         |
| **Utilizador de serviço**                            | Especifica o nome de início de sessão da conta de utilizador de serviço que criou anteriormente no [utilizador e grupo necessários](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx#rug).                                                                                                                      | Escreva o nome da conta de utilizador aqui:                                                                                                                                  |
| **Palavra-passe**                                | Especifica a palavra-passe da conta de utilizador do serviço de núcleo do BHOLD.                                                                                                                                                                                                                                              | Escrever a palavra-passe aqui: **importante:** não se esqueça de manter esta palavra-passe numa localização segura, oculta.                                                                |
| **IP/porta do Web site**                         | Especifica o número de porta e o endereço IP do Web site a ser criado no servidor da intranet. Altere o valor predefinido (\*) apenas se não utilizar o mesmo endereço IP que o Web site predefinido. Alterar o número de porta para uma porta disponível apenas se a predefinição da porta (5151) já está em utilização.             | Se for utilizado um endereço IP não predefinida ao Web site predefinido, gravá-lo aqui: se o número de porta predefinido já está em utilização, escreva o número de porta de Web site BHOLD aqui: |

### <a name="database-settings"></a>Definições de base de dados

| **Item**                                       | **Descrição**                                                                                                                                                                                                                                                           | **Valor**                                                                                                                                                                                                                                                                                                                                                                                             |
|------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Utilizar a segurança integrada**                    | Especifica que a autenticação Windows é utilizada para aceder à base de dados.                                                                                                                                                                                                     | Selecione a caixa de verificação se a autenticação Windows é utilizada para ligar ao SQL Server. Desmarque a caixa de verificação se for utilizada a autenticação do SQL Server. A base de dados deve ter sido criado antes de executar BHOLD Core configuração se autenticação do SQL Server é utilizada. **Nota:** se for utilizada a autenticação do Windows, deve ter sessão iniciada com uma conta que tenha a função de servidor sysadmin no servidor de base de dados. |
| **Base de dados de utilizador** e **palavra-passe de base de dados** | Especifica o nome de utilizador e palavra-passe de um utilizador com a função de servidor sysadmin no servidor de base de dados. Estes valores são fornecidos apenas quando é utilizada a autenticação do SQL Server.                                                                                               | Escrever o nome de utilizador do SQL Server: escrever o SQL Server utilizador palavra-passe aqui: **Nota:** não se esqueça de manter esta palavra-passe numa localização segura, oculta.                                                                                                                                                                                                                                                  |
| **Servidor de base de dados** e **nome de base de dados**   | Especifica o nome NetBIOS do servidor de base de dados e o nome da base de dados (predefinição: b1) que o programa de configuração do BHOLD Core vai criar. Se não estiver a utilizar a instância de servidor de base de dados predefinida, especifique a instância de servidor de base de dados no formato * \<servidor\>*\\*\<instância\> *. | Escrita de nome de servidor (ou servidor e instância) aqui: escrever o nome de base de dados aqui:                                                                                                                                                                                                                                                                                                                   |
| **Certifique-restrições para o utilizador de base de dados**    | Obsoleto.                                                                                                                                                                                                                                                                 | Não altere o valor predefinido                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |
|                                                |                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="bhold-core-setup"></a>Configuração de núcleos do BHOLD

Para instalar o módulo do BHOLD núcleos, inicie sessão como membro do grupo Admins do domínio, transfira o ficheiro seguinte e executá-la como administrador no servidor que tenciona instalar o módulo de núcleo do BHOLD em: 

- BholdCore * \<versão\>*\_Release.msi

Substitua * \<versão\> * com o número de versão da versão do BHOLD núcleo que está a instalar.

Para executar o ficheiro de programa como administrador, clique com o botão direito do ficheiro e, em seguida, clique em **executar como administrador**.

### <a name="postinstallation-settings"></a>Definições postinstallation

Após a conclusão da configuração de núcleos de BHOLD, tem de configurar a Firewall do Windows e alterar as definições avançadas no conjunto aplicacional BHOLD Core nos serviços de informação de Internet para concluir a configuração do BHOLD Core. Se necessário, também deve alterar os atributos de sistema do BHOLD para satisfazer os seus requisitos.

#### <a name="configuring-windows-firewall"></a>Configurar a Firewall do Windows

Se os utilizadores acederão aos BHOLD através da utilização de web browsers em computadores remotos, tem de configurar a Firewall do Windows no servidor do BHOLD núcleo para permitir ligações de entrada para a porta do Web site que especificou quando instalou BHOLD Core.

Para efetuar este procedimento, tem de ser um membro do grupo Administradores no computador local.

#### <a name="to-permit-incoming-connections-to-the-bhold-website"></a>Para permitir ligações de entrada para o Web site do BHOLD

1.  Clique em **iniciar**, aponte para **ferramentas administrativas**, faça duplo clique **Firewall do Windows com definições avançadas**e, em seguida, clique em **executar como administrador**.

2.  No painel esquerdo, clique em **regras de entrada**e, em seguida, no painel direito, clique em **nova regra**.

3.  No Assistente de novas regras de entrada, clique em **porta**e, em seguida, clique em **seguinte**.

4.  Certifique-se de que **TCP** estiver selecionada, no **portas locais específicas**, escreva o número de porta predefinido BHOLD Core (5151) ou o número de porta que especificou quando instalou BHOLD núcleos e, em seguida, clique em ** Seguinte**.

5.  Certifique-se de que **permitir a ligação** está selecionada e, em seguida, clique em **seguinte**.

6.  No **perfil** página, desmarque as caixas de verificação para as localizações a partir dos quais não pretende acesso ao Web site do BHOLD e, em seguida, clique em **seguinte**.

7.  No **nome** página, escreva um nome para a regra (por exemplo, permitir ligações de entrada para BHOLD Core) e, em seguida, clique em **concluir**.  

#### <a name="enabling-32-bit-applications-for-the-bhold-core-application-pool"></a>Ativar aplicações de 32 bits para o conjunto aplicacional do núcleo do BHOLD

Para permitir que o IIS para funcionar corretamente com o módulo do BHOLD Core, tem de configurar o conjunto aplicacional BHOLD núcleos para suportar aplicações de 32 bits. Tem de ser registado com a conta utilizada para instalar o módulo do BHOLD núcleos para efetuar este procedimento.

**Para ativar o suporte da aplicação de 32 bits para o conjunto aplicacional do núcleo do BHOLD**

1.  Para abrir o Gestor de serviços de informação de Internet, clique em **iniciar**, aponte para **ferramentas de administração**e, em seguida, clique em **Gestor dos serviços de informação Internet (IIS)**.

2.  Na árvore da consola, expanda o nome do servidor e, em seguida, clique em **conjuntos aplicacionais**.

3.  No **conjuntos aplicacionais** lista, faça duplo clique **CoreAppPool**e, em seguida, clique em **definições avançadas**.

4.  No **definições avançadas** caixa de diálogo a **ativar 32-Bit aplicações** lista, selecione **verdadeiro**e, em seguida, clique em **OK**.

#### <a name="establishing-the-service-principal-name-for-the-bhold-website"></a>Estabelecer o nome do principal de serviço para o Web site do BHOLD

Se o nome de rede que é utilizado para contactar o site do BHOLD não é igual ao nome de anfitrião do servidor, tem de estabelecer um nome principal de serviço (SPN) para HTTP. Por exemplo, se utilizar um registo de recurso CNAME no DNS para especificar um alias para o servidor ou se utilizar o balanceamento de carga na rede, tem de registar estes endereços de rede adicionais no Active Directory. Se conseguir fazê-lo, o Internet Explorer não é possível utilizar o protocolo Kerberos ao contactar o site do BHOLD.

>[!IMPORTANT]
Se o módulo de núcleo do BHOLD está instalado no mesmo computador como o Portal do FIM, tem de criar registos de recursos DNS (CNAME ou A) com nomes de anfitrião diferente para os servidores que executam BHOLD Core e o servidor que executa o Portal do FIM. Apenas um SPN pode ser estabelecida para um determinado par de serviço de tipo/servidor alias, e, por isso, BHOLD Core e o Portal do FIM requerem SPNs separados porque são geralmente executadas em contas diferentes. O comando setspn reportou um erro se um SPN já tiver sido estabelecido com outra conta.

A associação **Admins do domínio**, ou equivalente, é o mínimo requerido para concluir este procedimento.

#### <a name="to-establish-the-spn-of-the-bhold-website"></a>Para estabelecer o SPN do Web site BHOLD

1.  No controlador de domínio de serviços de domínio do Active Directory, clique em **iniciar**, clique em **todos os programas**, clique em **acessórios**, faça duplo clique **linha de comandos **e, em seguida, clique em **executar como administrador**.

2.  Na linha de comandos, escreva o seguinte comando e, em seguida, prima ENTER: setspn – S HTTP / * \<networkalias\> \<domínio\> * \\ * \<accountname\> * onde:

    -   *\<networkalias\> * é o endereço que os clientes utilizam para contactar o site do BHOLD

    -   *\<domínio\>*\\*\<accountname\> * é o nome de utilizador e domínio da conta de serviço de núcleo do BHOLD que criou quando instalou BHOLD Core.

3.  Repita o passo anterior para todos os outros nomes que os clientes utilizam para contactar o site do BHOLD, por exemplo, CNAME aliases, nomes que incluem um nome de domínio completamente qualificado ou nomes que incluem um nome de domínio (abreviado) de NetBIOS.

#### <a name="setting-bhold-system-attributes"></a>Definir atributos de sistema do BHOLD

Para validar que a instalação do módulo principal do BHOLD foi concluída com êxito, abra o portal do BHOLD núcleos e ver os atributos de sistema. Além disso, para garantir que as funções do módulo de núcleo do BHOLD corretamente no seu ambiente, pode modificar os seguintes atributos de sistema do BHOLD, conforme adequado:

| **Atributo**                | **Descrição**                                                                                                                                                                                                                                                                                                      |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **NoHistory**                | Definido como Y se o Web site do BHOLD está em execução num serviço web em cluster para se certificar de que itens recentemente apresentados está a funcionar corretamente. Definido como N se o Web site do BHOLD está em execução no servidor de IIS autónomo.                                                                                                                      |
| **MoveorgunitToSameorgtype** | Definido como Y para se certificar de que as unidades organizacionais (orgunits) podem ser movidas apenas a orgunits com o mesmo tipo organizacional como o orgunit principal. Por exemplo, isto impede que um orgunit de projeto que está a ser movido para um orgunit departamento. Definido como N para permitir que um orgunit ser colocados num orgunit de um tipo diferente. |
| **Executam de dias entre ABA**     | Definido como um número inteiro dois dígitos, para especificar o intervalo (em dias), entre duas autorização baseadas em atributos (ABA) é executado. Por exemplo, para especificar que executa ABA ficarão separadas por dois dias, escreva 02.                                                                                                                     |
| **Hora de início de ABA executar**    | Definido como um número inteiro dois dígitos para especificar a hora do dia quando irá ocorrer uma execução de autorização baseadas em atributos. Por exemplo especificar que ABA executar ocorrerá às 11:00 (23:00), escreva 23.                                                                                                             |
| **Cardinalidade do sistema**       | Definido como N se não pretender que uma verificação de cardinalidade do sistema no BHOLD. O valor predefinido é Y.                                                                                                                                                                                                                             |
| **Registo**                  | Definido como N se não pretender que as alterações que será registado. O valor predefinido é Y.                                                                                                                                                                                                                                            |
| **Processamento de SystemQueue**   | Definido como N se não pretender que o processamento do sistema de fila. Não altere este valor, a menos que direcionado para fazê-lo pelo suporte técnico.                                                                                                                                                                                           |

Tem de ter sessão iniciada como membro do grupo Admins do domínio para efetuar este procedimento.

**Para definir um atributo de sistema do BHOLD**

1.  Clique em **iniciar**, clique em **todos os programas**e, em seguida, clique em **Internet Explorer**.

2.  Na caixa de endereço, escreva, em que * \<servidor\> * é o nome do servidor de Web site do BHOLD e * \<porta\> * o número de porta está vinculado ao Web site.

3.  Clique em **home page**, clique em **valores**e, em seguida, clique em **modificar**.

4.  Localize o nome do atributo que pretende alterar, escreva o novo valor na caixa junto ao nome do atributo e, em seguida, clique em **OK**.

## <a name="next-steps"></a>Próximos passos

Depois de ter instalado BHOLD núcleos e validar que a instalação foi concluída com êxito, pode instalar módulos adicionais. Neste momento, a base de dados do BHOLD serão essencialmente vazio, que contém apenas uma conta de utilizador, a conta de raiz e uma unidade organizacional (orgunit), o orgunit de raiz. Para adicionar mais utilizadores para a base de dados do BHOLD, ou pode instalar o módulo de conector de gestão de acesso ou o módulo de gerador de modelos do BHOLD, consoante as suas necessidades. Pode utilizar o módulo de conector de gestão de acesso para importar dados de utilizador do serviço de sincronização do FIM ou pode utilizar o gerador de modelo do BHOLD para importar dados de utilizador de um conjunto de ficheiros structured. Para obter mais informações sobre como utilizar o módulo de conector de gestão de acesso, consulte [Guia do laboratório de teste: conector de gestão de acesso do BHOLD](https://technet.microsoft.com/en-us/library/jj853085(v=ws.10).aspx).

Para obter mais informações sobre como utilizar o módulo do BHOLD gerador de modelos, consulte:

- [Guia do Microsoft BHOLD Suite conceitos](https://technet.microsoft.com/en-us/library/jj134102(v=ws.10).aspx)
- [Microsoft BHOLD Suite TechnicalReference](https://technet.microsoft.com/en-us/library/jj134935(v=ws.10).aspx).
