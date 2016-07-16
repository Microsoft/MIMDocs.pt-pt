---
title: Planear um ambiente bastion | Microsoft Identity Manager
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/14/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: bfc7cb64-60c7-4e35-b36a-bbe73b99444b
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: b8af77d2354428da19d91d5f02b490012835f544
ms.openlocfilehash: 0ed48d43825e1a876c4d96cafcb6c17cac26610f


---

# Planear um ambiente bastion

Adicionar um ambiente bastion com uma floresta administrativa dedicada a um Active Directory permite às organizações gerir facilmente contas administrativas, estações de trabalho e grupos num ambiente que tem controlos de segurança mais fortes que o respetivo ambiente de produção existente.

Esta arquitetura ativa vários controlos que não são possíveis ou facilmente configurados numa arquitetura de floresta única. Tal inclui o aprovisionamento de contas como utilizadores não privilegiados padrão na floresta administrativa, que são altamente privilegiados no ambiente de produção, permitindo uma maior imposição técnica de governação. Esta arquitetura também permite a utilização da funcionalidade de autenticação seletiva de uma confiança como meio de restringir inícios de sessão (e exposição de credenciais) apenas a anfitriões autorizados. Em situações em que se pretende um maior nível de garantia para a floresta de produção sem incorrer no custo e na complexidade de uma reconstrução completa, uma floresta administrativa pode fornecer um ambiente que aumenta o nível de garantia do ambiente de produção.

Podem ser utilizadas técnicas adicionais para além da floresta administrativa dedicada. Estas incluem restringir os locais onde são expostas credenciais administrativas, limitar os privilégios de função de utilizadores nessa floresta e garantir que tarefas administrativas não são realizadas em anfitriões utilizados para atividades de utilizador padrão (por exemplo, e-mail e navegação na Web).

## Considerações sobre melhores práticas

Uma floresta administrativa dedicada é uma floresta do Active Directory padrão única utilizada para a gestão do Active Directory. Uma vantagem de utilizar domínios e florestas administrativas é o facto de poderem ter mais medidas de segurança do que as florestas de produção devido aos seus casos de utilização limitados. Além disso, visto que esta floresta está separada e não confia nas florestas existentes da organização, um compromisso de segurança noutra floresta não se alargaria a esta floresta dedicada.

Um design de floresta administrativa tem as seguintes considerações:

### Âmbito limitado

O valor de uma floresta administrativa é o elevado nível de garantia de segurança e a superfície de ataque reduzida. A floresta pode alojar aplicações e funções de gestão adicionais, mas cada aumento de âmbito aumentará a superfície de ataque da floresta e dos seus recursos. O objetivo é limitar as funções da floresta para manter a superfície de ataque mínima.

De acordo com o [modelo de camada](tier-model-for-partitioning-administrative-privileges.md) da criação de partições de privilégios administrativos, as contas numa floresta administrativa dedicada devem estar numa camada única, normalmente a camada 0 ou a camada 1. Se uma floresta estiver na camada 1, considere restringi-la a um âmbito específico de aplicação (por exemplo, aplicações financeiras) ou a uma comunidade de utilizadores (por exemplo, fornecedores externos de TI).

### Confiança restrita

A floresta *CORP* de produção deve confiar na floresta *PRIV* administrativa, mas não o inverso. Pode ser uma confiança de domínio ou uma confiança de floresta. O domínio de floresta de administração não precisa de confiar nas florestas e nos domínios geridos para gerir o Active Directory, embora as aplicações adicionais possam precisar de uma relação de confiança bilateral, de validação de segurança e de testes.

A autenticação seletiva deve ser utilizada para garantir que as contas na floresta de administração utilizam apenas os anfitriões de produção adequados. Para manter os controladores de domínio e delegar direitos no Active Directory, normalmente é preciso conceder o direito "Autorizado a iniciar sessão" aos controladores de domínio para as contas de administrador de camada 0 designadas na floresta de administração. Veja [Configuring Selective Authentication Settings (Configurar Definições de Autenticação Seletiva)](http://technet.microsoft.com/library/cc755844.aspx) para obter mais informações.

## Manter a separação lógica

Para garantir que o ambiente bastion não é afetado por incidentes de segurança existentes ou futuros no Active Directory organizacional, as diretrizes seguintes devem ser utilizadas ao preparar sistemas para o ambiente bastion:

- Os Windows Servers não devem estar associados a um domínio ou tirar partido de software ou de distribuição de definições do ambiente existente.

- O ambiente bastion tem de conter os seus próprios Serviços de Domínio do Active Directory, fornecendo Kerberos e LDAP, DNS e serviços de hora ao ambiente bastion.

- O MIM não deve utilizar um farm de bases de dados SQL no ambiente existente. O SQL Server deve ser implementado em servidores dedicados no ambiente bastion.

- O ambiente bastion precisa do Microsoft Identity Manager 2016 e, especificamente, o Serviço MIM e os componentes PAM têm de ser implementados.

- O software de cópia de segurança e os suportes de dados para o ambiente bastion têm de manter-se separados dos sistemas nas florestas existentes, para que um administrador na floresta existente não possa subverter uma cópia de segurança do ambiente bastion.

- Os utilizadores que gerem os servidores do ambiente bastion têm de iniciar sessão a partir de estações de trabalho que não estejam acessíveis a administradores no ambiente existente, para que não exista uma fuga das credenciais do ambiente bastion.

## Garantir a disponibilidade dos serviços de administração

Uma vez que a administração de aplicações será transitada para o ambiente bastion, tenha em conta como fornecer disponibilidade suficiente para cumprir os requisitos dessas aplicações. As técnicas incluem:

- Implementar os Serviços de Domínio do Active Directory em vários computadores no ambiente bastion. Pelo menos são necessários dois para garantir a autenticação continuada, mesmo que um servidor seja temporariamente reiniciado para manutenção agendada. Podem ser necessários computadores adicionais para uma carga maior ou para gerir recursos e administradores com base em várias regiões geográficas.

- Preparar contas «break glass» na floresta existente e na floresta administrativa dedicada, para fins de emergência.

- Implementar o SQL Server e o Serviço MIM em vários computadores no ambiente bastion.

- Manter uma cópia de segurança do AD e do SQL Server para cada alteração de utilizadores ou de definições de função na floresta administrativa dedicada.

## Configurar as permissões adequadas do Active Directory

A floresta administrativa deve ser configurada para menor privilégio com base nos requisitos de administração do Active Directory.

- Às contas na floresta administrativa que servem para administrar o ambiente de produção não devem ser concedidos privilégios administrativos para a floresta administrativa, os domínios na mesma ou as estações de trabalho na mesma.

- Os privilégios administrativos em relação à própria floresta administrativa devem ser controlados rigorosamente por um processo offline para reduzir a possibilidade de um atacante ou insider malicioso apagar registos de auditoria. Também ajuda a garantir que o pessoal com contas de administrador de produção não pode reduzir as restrições sobre as respetivas contas e aumentar o risco para a organização.

- A floresta administrativa deve seguir as configurações do Microsoft Security Compliance Manager (SCM) para o domínio, incluindo configurações seguras de protocolos de autenticação.

Ao criar o ambiente bastion, antes de instalar o Microsoft Identity Manager, identifique e crie as contas que serão utilizadas para a administração neste ambiente. Isto irá incluir:

- As **contas «break glass»** só devem conseguir iniciar sessão nos controladores de domínio do ambiente bastion.

- Os **administradores "Red Card"** aprovisionam outras contas e executam manutenção agendada. Estas contas não recebem acesso às florestas ou aos sistemas existentes fora do ambiente bastion. As credenciais, por exemplo, um smart card, devem ser fisicamente protegidas e a utilização destas contas deve ser registada.

- **Contas de serviço** necessárias ao Microsoft Identity Manager, ao SQL Server e a outro software.

## Proteção dos anfitriões

Todos os anfitriões, incluindo os controladores de domínio, os servidores e as estações de trabalho associados à floresta administrativa, devem ter os sistemas operativos e os service packs mais recentes instalados e sempre atualizados.

- As aplicações necessárias para efetuar a administração devem estar pré-instaladas nas estações de trabalho, para que as contas que as utilizam não precisem de estar no grupo de administradores locais para as instalarem. Normalmente, a manutenção do controlador de domínio pode ser feita com o RDP e as Ferramentas de Administração Remota do Servidor.

- Os anfitriões da floresta administrativa devem ser atualizados automaticamente com atualizações de segurança. Embora isto possa criar o risco de interromper as operações de manutenção do controlador de domínio, fornece uma mitigação significativa de riscos de segurança de vulnerabilidades não corrigidas.

### Identificar anfitriões administrativos

O risco de um sistema ou estação de trabalho deve ser medido pela atividade de maior risco realizada no mesmo, tal como navegação na Internet, envio e receção de e-mail, ou pela utilização de outras aplicações que processam conteúdo desconhecido ou não fidedigno.

Os anfitriões administrativos incluem os seguintes computadores:

- Um ambiente de trabalho no qual as credenciais do administrador são escritas ou introduzidas fisicamente.

- “Jump servers” administrativos nos quais são executadas ferramentas e sessões administrativas.

- Todos os anfitriões nos quais são realizadas ações administrativas, incluindo aqueles que utilizam um ambiente de trabalho de utilizador padrão com um cliente RDP para administrar remotamente servidores e aplicações.

- Servidores que alojam aplicações que precisam de ser administradas e cujo acesso não é feito através de RDP com comunicação remota de Modo de Administrador Restrito ou Windows PowerShell.

### Implementar estações de trabalho administrativas dedicadas

Embora inconveniente, podem ser precisas estações de trabalho protegidas separadas, dedicadas a utilizadores com credenciais administrativas de impacto elevado. É importante fornecer um anfitrião com um nível de segurança igual ou superior ao nível dos privilégios conferidos às credenciais. Considere incorporar as seguintes medidas proteção adicional:

- **Verifique se todos os elementos multimédia na compilação estão limpos** para atenuar os efeitos de software maligno instalado numa imagem original ou injetados num ficheiro de instalação durante a transferência ou o armazenamento.

- **Linhas de base de segurança** devem ser utilizadas como configurações de partida. O Microsoft Security Compliance Manager (SCM) pode ajudar a configurar as linhas de base em anfitriões administrativos.

- **Arranque seguro** para atenuar os efeitos de atacantes ou de software maligno que tente carregar código não assinado para o processo de arranque.

- **Restrição de software** para garantir que apenas é executado software administrativo autorizado nos anfitriões administrativos. Os clientes podem utilizar o AppLocker para esta tarefa com uma lista branca de aplicações autorizadas, para ajudar a impedir a execução de software malicioso e de aplicações não suportadas.

- **Encriptação de volume completo** para atenuar os efeitos da perda física de computadores, tais como computadores portáteis administrativos utilizados remotamente.

- **Restrições de USB** para proteger contra infeção física.

- **Isolamento de rede** para proteger contra ataques à rede e ações de administração inadvertidas. As firewalls do anfitrião devem bloquear todas as ligações de entrada, exceto aquelas explicitamente necessárias, e bloquear todo o acesso à Internet de saída desnecessário.

- **Antimalware** para proteção contra ameaças conhecidas e software maligno.

- **Explorar atenuações** para atenuar os efeitos de exploits e ameaças desconhecidas, incluindo o Enhanced Mitigation Experience Toolkit (EMET).

- **Análise da superfície de ataque** para impedir a introdução de novos vetores de ataques no Windows durante a instalação de software novo. Ferramentas como o Attack Surface Analyzer (ASA) ajudam a avaliar as definições de configuração num anfitrião e a identificar vetores de ataques introduzidos por alterações de configuração ou software.

- **Os privilégios administrativos** não devem ser concedidos aos utilizadores no respetivo computador local.

- **Modo RestrictedAdmin** para sessões RDP efetuadas, exceto quando for exigido pela função. Veja [Novidades dos Serviços de Ambiente de Trabalho Remoto no Windows Server](https://technet.microsoft.com/library/dn283323.aspx) para obter mais informações.

Algumas destas medidas poderão parecer extremas, mas revelações públicas nos últimos anos demonstraram as capacidades significativas que os adversários qualificados possuem para comprometer alvos.

## Preparar os domínios existentes para serem geridos pelo ambiente bastion

O MIM utiliza os cmdlets do PowerShell para estabelecer confiança entre os domínios de AD existentes e a floresta administrativa dedicada no ambiente bastion. Após a implementação do ambiente bastion, e antes dos utilizadores ou grupos serem convertidos para o JIT, em seguida, os cmdlets `New-PAMTrust` e `New-PAMDomainConfiguration` irão atualizar as relações de confiança dos domínios e criar artefactos necessários para o AD e o MIM.

Quando a topologia do Active Directory existente for alterada, os cmdlets `Test-PAMTrust`, `Test-PAMDomainConfiguration`, `Remove-PAMTrust` e `Remove-PAMDomainConfiguration` podem ser utilizados para atualizar as relações de confiança.

### Estabelecer confiança para cada floresta

O cmdlet `New-PAMTrust` tem de ser executado uma vez para cada floresta existente. É invocado no computador do Serviço MIM no domínio administrativo. Os parâmetros para este comando são o nome de domínio do domínio principal da floresta existente e as credenciais de um administrador desse domínio.

```
New-PAMTrust -SourceForest "contoso.local" -Credentials (get-credential)
```

Depois de estabelecer a confiança, configure cada um dos domínios para permitir a gestão a partir do ambiente bastion, conforme descrito na secção seguinte.

### Permitir a gestão de cada domínio

Existem sete requisitos para permitir a gestão de um domínio existente.

#### 1. Um grupo de segurança no domínio local

Tem de existir um grupo no domínio existente, cujo nome corresponde ao nome do domínio NetBIOS seguido de três cifrões, por exemplo, *CONTOSO$$$*. O âmbito do grupo tem de ser *local de domínio* e o tipo de grupo tem de ser *Segurança*. Isto é necessário para que os grupos sejam criados na floresta administrativa dedicada com o mesmo identificador de segurança que os grupos neste domínio. Crie este grupo com o seguinte comando do PowerShell, executado por um administrador do domínio existente e execute numa estação de trabalho associada ao domínio existente:

```
New-ADGroup -name 'CONTOSO$$$' -GroupCategory Security -GroupScope DomainLocal -SamAccountName 'CONTOSO$$$'
```

#### 2. Auditoria dos êxitos e falhas

As definições da política de grupo no controlador de domínio relativas a auditoria têm de incluir a auditoria dos êxitos e falhas para Auditar a gestão de contas e Auditar acesso ao serviço de diretórios. Isto pode ser feito com a Consola de Gestão de Políticas de Grupo, executada por um administrador do domínio existente e executada numa estação de trabalho associada ao domínio existente:

3. Aceda a **Iniciar** > **Ferramentas administrativas** > **Gestão de Políticas de Grupo**.

4. Navegue para **Floresta: contoso.local** > **Domínios** > **contoso.local** > **Controladores de Domínio** > **Política de Controladores de Domínio Predefinida**. Irá aparecer uma mensagem informativa.

    ![Política de controladores de domínio predefinida - captura de ecrã](media/pam-group-policy-management.jpg)

5. Clique com o botão direito do rato em **Política de Controladores de Domínio Predefinida** e selecione **Editar**. Será apresentada uma nova janela.

6. Na janela Editor de Gestão de Políticas de Grupo, na árvore da Política de Controladores de Domínio Predefinida, navegue para **Configuração do Computador** > **Políticas** > **Definições do Windows** > **Definições de Segurança** > **Políticas Locais** > **Política de auditoria**.

    ![Editor de gestão de políticas de grupo - captura de ecrã](media/pam-group-policy-management-editor.jpg)

5. No painel de detalhes, clique com o botão direito do rato em **Auditar a gestão de contas** e selecione **Propriedades**. Selecione **Definir estas definições de política**, marque a caixa de verificação **Êxito** e a caixa de verificação **Falha**, clique em **Aplicar** e em **OK**.

6. No painel de detalhes, clique com o botão direito do rato em **Auditar acesso ao serviço de diretórios** e selecione **Propriedades**. Selecione **Definir estas definições de política**, marque a caixa de verificação **Êxito** e a caixa de verificação **Falha**, clique em **Aplicar** e em **OK**.

    ![Definições de política com êxito e falha - captura de ecrã](media/pam-group-policy-management-editor2.jpg)

7. Feche a janela Editor de Gestão de Políticas de Grupo e a janela Gestão de Políticas de Grupo. Em seguida, aplique as definições de auditoria ao iniciar uma janela do PowerShell e ao escrever:

    ```
    gpupdate /force /target:computere
    ```

A mensagem “A atualização da Política de Computador foi concluída com êxito.” deve aparecer após alguns minutos.

#### 3. Permitir ligações à Autoridade de Segurança Local

Os controladores de domínio têm de permitir ligações RPC através de TCP/IP para a Autoridade de Segurança Local (LSA) a partir do ambiente bastion. Em versões anteriores do Windows Server, o suporte de TCP/IP na LSA tem de estar ativado no registo:

```
New-ItemProperty -Path HKLM:SYSTEM\\CurrentControlSet\\Control\\Lsa -Name TcpipClientSupport -PropertyType DWORD -Value 1
```

#### 4. Criar a configuração do domínio de PAM

O cmdlet `New-PAMDomainConfiguration` tem de ser executado no computador do Serviço MIM no domínio administrativo. Os parâmetros para este comando são o nome de domínio do domínio existente e as credenciais de um administrador desse domínio.

```
 New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials (get-credential)
```

#### 5. Conceder permissões de leitura a contas

As contas na floresta bastion utilizadas para estabelecer funções (os administradores que utilizam os cmdlets `New-PAMUser` e `New-PAMGroup`), bem como a conta utilizada pelo serviço de monitor MIM, precisam de permissões de leitura nesse domínio.

Os passos seguintes permitem o acesso de leitura do utilizador *PRIV\Administrator* ao domínio *Contoso* no controlador de domínio *CORPDC*:

1. Certifique-se de que tem sessão iniciada em CORPDC como administrador do domínio Contoso (por exemplo, Contoso\Administrator).

2. Inicie Utilizadores e Computadores do Active Directory.

3. Clique com o botão direito do rato no domínio **contoso.local** e selecione **Delegar controlo**.

4. No separador Utilizadores e grupos selecionados, clique em **Adicionar**.

5. No pop-up Selecionar Utilizadores, Computadores ou Grupos, clique em **Localizações** e altere a localização para *priv.contoso.local*. No nome do objeto, escreva *Admins do domínio* e clique em **Verificar nomes**. Quando aparecer um pop-up, para o nome de utilizador escreva *priv\administrator* e a palavra-passe.

6. A seguir a Admins do domínio, escreva *; MIMMonitor*. Depois de os nomes de Admins do domínio e MIMMonitor estarem sublinhados, clique em **OK** e, em seguida, clique em **Seguinte**.

7. Na lista de tarefas comuns, selecione **Ler todas as informações do utilizador** e, em seguida, clique em **Seguinte** e em **Concluir**.

18. Feche Computadores e Utilizadores do Active Directory.

#### 6. Uma conta «break glass»

Se o objetivo do projeto Privileged Access Management for reduzir o número de contas com privilégios de Administrador de Domínio permanentemente atribuídos ao domínio, tem de existir uma conta *break glass* no domínio, no caso de existir um problema posterior com a relação de confiança. As contas para acesso de emergência à floresta de produção devem existir em cada domínio e só devem poder iniciar sessão nos controladores de domínio. Para organizações com vários sites, podem ser precisas contas adicionais para redundância.

#### 7. Atualizar as permissões no ambiente bastion

Reveja as permissões no objeto *AdminSDHolder* no contentor do sistema nesse domínio. O objeto *AdminSDHolder* tem uma lista de controlo de acesso (ACL) exclusiva, que serve para controlar as permissões de principais de segurança que são membros de grupos privilegiados do Active Directory incorporados. Repare se foram feitas alterações às permissões predefinidas, que possam afetar os utilizadores com privilégios administrativos no domínio, uma vez que essas permissões não se aplicarão a utilizadores cuja conta esteja no ambiente bastion.

## Selecionar utilizadores e grupos para inclusão

O passo seguinte consiste em definir as funções de PAM, associando os utilizadores e grupos às quais devem ter acesso. Normalmente, será um subconjunto de utilizadores e grupos para a camada identificado como estando a ser gerida ambiente bastion. Pode encontrar mais informações em [Definir funções para o Privileged Access Management](defining-roles-for-pam.md).



<!--HONumber=Jul16_HO2-->


