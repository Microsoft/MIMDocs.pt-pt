---
title: Implementar o Privileged Access Management do MIM com o Windows Server 2016 | Documentos da Microsoft
description: Saiba mais sobre como implementar o Privileged Access Management com o Windows Server 2016
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/18/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 521b96c3ef9cae5a5f9151ddf125cfb534ae0332
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044026"
---
# <a name="deploy-mim-pam-with-windows-server-2016"></a>Implementar o PAM do MIM com o Windows Server 2016


Este cenário permite que o MIM 2016 SP1 tire partido das funcionalidades do Windows Server 2016, como o controlador de domínio para a floresta "PRIV". Quando este cenário está configurado, a permissão Kerberos do utilizador terá um prazo limitado ao tempo restante das respetivas ativações da função. 

> [!Note]
> Não é possível utilizar pré-visualizações técnicas do Windows Server 2016 anteriores à Pré-visualização Técnica 5 com esta versão do MIM.

## <a name="preparation"></a>Preparação

São necessárias, no mínimo, duas VMs para o ambiente de laboratório:

-   A VM que aloja o Controlador de Domínio PRIV, a executar o Windows Server 2016

-   A VM que aloja o Serviço MIM, a executar o Windows Server 2016 (recomendado) ou o Windows Server 2012 R2

> [!NOTE]
> Se ainda não tiver um domínio "CORP" no seu ambiente de laboratório, é necessário um controlador de domínio adicional para esse domínio. O controlador de domínio "CORP" pode executar o Windows Server 2016 ou o Windows Server 2012 R2.


Faça a instalação tal como descrito no [Guia de introdução](privileged-identity-management-for-active-directory-domain-services.md), **exceto conforme indicado abaixo**:

- Se estiver a criar um novo domínio CORP, ao seguir as instruções no [Passo 1 – Preparar o controlador de domínio CORP](step-1-prepare-corp-domain.md), pode optar por configurar, opcionalmente, o domínio CORP para estar ao nível funcional do Windows Server 2016. **Se escolher esta opção, faça os seguintes ajustes**:

  - Se estiver a utilizar o suporte de dados do Windows Server 2016, a opção de instalação será denominada Windows Server 2016 (Servidor com Experiência de Utilização do Computador).

  - Pode especificar o nível funcional do Windows Server 2016 para a floresta e o domínio CORP ao fornecer 7 como o número de versão do domínio e da floresta no argumento para o comando Install-ADDSForest da seguinte forma:
    ```
    Install-ADDSForest –DomainMode 7 –ForestMode 7 –DomainName contoso.local –DomainNetbiosName contoso –Force –NoDnsOnNetwork
    ```
  - Em "Criar novos utilizadores e grupos", o comando final (New ADGroup-name 'CONTOSO\$\$\$' ...) **não é necessário quando os controladores de domínio CORP e PRIV estão ao nível funcional do domínio do Windows Server 2016**.

  - As alterações descritas em "Configurar a auditoria"(item n.º 8) e "Configurar as definições de registo" (item n.º 10) são **recomendadas, mas não são obrigatórias,** quando os controladores de domínio CORP e PRIV estão ao nível funcional do domínio do Windows Server 2016.

- Se optar por utilizar o Windows Server 2012 R2 como o sistema operativo para CORPDC, tem de instalar as correções 2919442 e 2919355, [assim como a atualização 3155495](https://support.microsoft.com/kb/3156418), no CORPDC.

- Siga as instruções no [Passo 2 - Preparar o controlador de domínio PRIV](step-2-prepare-priv-domain-controller.md), exceto estes ajustes:

  -   Instale através do suporte de dados do Windows Server 2016. A opção de instalação será denominada Windows Server 2016 (Servidor com Experiência de Utilização do Computador).

  -   Nas instruções "Adicionar funções" (item n.º 4), **tem de especificar os números de versão do domínio e da floresta na quarta linha dos comandos do PowerShell como 7**, de modo a permitir que as funcionalidades do Windows Server AD descritas posteriormente sejam ativadas.

      ```
      Install-ADDSForest -DomainMode 7 -ForestMode 7 -DomainName priv.contoso.local  -DomainNetbiosName priv -Force -CreateDNSDelegation -DNSDelegationCredential $ca
      ```  

  -   Ao configurar os direitos de auditoria e início de sessão, tenha em atenção que o programa de Gestão de Políticas de Grupo estará localizado na pasta Ferramentas Administrativas do Windows.

  -   A configuração das definições de registo necessárias para a migração do histórico do SID (item n.º 8) **não é obrigatória quando o domínio PRIV está ao nível funcional do domínio do Windows Server 2016**.

  -   Depois de configurar a delegação e antes de reiniciar o servidor, ative as funcionalidades do Privileged Access Management no Active Directory do Windows Server 2016 ao iniciar uma janela do PowerShell como administrador e escrever os seguintes comandos.

  ```
  $of = get-ADOptionalFeature -filter "name -eq 'privileged access management feature'"
  Enable-ADOptionalFeature $of -scope ForestOrConfigurationSet -target "priv.contoso.local"
  ```

  - Depois de configurar a delegação e antes de reiniciar o servidor, autorize a conta do Serviço MIM e os administradores do MIM a criar e atualizar principais sombra.

    a. Inicie uma janela do PowerShell e escreva ADSIEdit.

    b. No menu Ações, clique em "Ligar a". Na Definição do ponto de ligação, altere o contexto de nomenclatura de "Contexto de nomenclatura predefinido" para "Configuração" e clique em OK.

    c. Depois de estabelecer ligação, no lado esquerdo da janela abaixo de "Editor de ADSI", expanda o nó de Configuração para ver "CN=Configuration,DC=priv,...". Expanda CN=Configuration e, em seguida, expanda CN=Services.

    d. Clique com o botão direito do rato em "CN=Shadow Principal Configuration" e clique em Propriedades. Quando for apresentada a caixa de diálogo de propriedades, mude para o separador de segurança.

    e. Clique em Adicionar. Especifique as contas "MIMService", bem como quaisquer outros administradores do MIM que mais tarde irão executar o comando New-PAMGroup para criar grupos de PAM adicionais. Para cada utilizador, na lista de permissões autorizadas, adicione "Escrever", "Criar todos os objetos subordinados" e "Eliminar todos os objetos subordinados". Adicione as permissões.

    f. Mude para as definições de Segurança Avançada. Na linha que permite o acesso ao MIMService, clique em Editar. Altere a definição "Aplica-se a" para "a este objeto e todos os objetos subordinados". Atualize esta definição de permissão e feche a caixa de diálogo de segurança.

    g. Feche o Editor de ADSI.

  - Depois de configurar a delegação e antes de reiniciar o servidor, autorize os administradores do MIM a criar e atualizar a política de autenticação.

    a.  Inicie uma **Linha de comandos** elevada e escreva os seguintes comandos, substituindo o nome da conta de administrador do MIM por "mimadmin" em cada linha:
    ```
    dsacls "CN=AuthN Policies,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:RPWPRCWD;;msDS-AuthNPolicy /i:s

    dsacls "CN=AuthN Policies,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:CCDC;msDS-AuthNPolicy

    dsacls "CN=AuthN Silos,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:RPWPRCWD;;msDS-AuthNPolicySilo /i:s

    dsacls "CN=AuthN Silos,CN=AuthN Policy
    Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
    mimadmin:CCDC;msDS-AuthNPolicySilo
    ```


- Siga as instruções no [Passo 3 - Preparar um servidor PAM](step-3-prepare-pam-server.md), com estes ajustes.

  -   Se estiver a instalar no Windows Server 2016, tenha em atenção que a função "ApplicationServer" não está disponível.

  -   Se estiver a instalar o MIM no Windows Server 2016, **não é possível instalar o SharePoint 2013**.

- Siga as instruções no [Passo 4 – Instalar componentes do MIM no servidor e estação de trabalho PAM](step-4-install-mim-components-on-pam-server.md), com estes ajustes.

  -   O utilizador que instala o Serviço MIM e os componentes do PAM **tem de ter acesso de escrita ao domínio PRIV no AD**, uma vez que a instalação do MIM cria uma nova UO do AD intitulada "Objetos de PAM".

  -   Se o SharePoint não estiver instalado, não instale o Portal do MIM.

- Siga as instruções no [Passo 5 - Estabelecer confiança](step-5-establish-trust-between-priv-corp-forests.md), com estes ajustes:

  - Ao estabelecer a confiança unidirecional, execute apenas os primeiros dois comandos do PowerShell (get-credential e New-PAMTrust), **não execute o comando New-PAMDomainConfiguration**.

  - Depois de estabelecer confiança, inicie sessão no PRIVDC como PRIV\\Administrador, inicie o PowerShell e escreva os seguintes comandos:
    ```
    netdom trust contoso.local /domain:priv.contoso.local /enablesidhistory:yes
    /usero:contoso\administrator /passwordo:Pass@word1

    netdom trust contoso.local /domain:priv.contoso.local /quarantine:no
    /usero:contoso\administrator /passwordo:Pass@word1  

    netdom trust contoso.local /domain:priv.contoso.local /enablepimtrust:yes
    /usero:contoso\administrator /passwordo:Pass@word1
    ```

- O item n.º 5 (verificação de confiança) **não é necessário quando os domínios CORP e PRIV estão ao nível funcional do domínio do Windows Server 2016**.

## <a name="more-information"></a>Mais informações

- [Privileged Access Management para Serviços de Domínio do Active Directory](privileged-identity-management-for-active-directory-domain-services.md)
- [Configuração do ambiente de MIM para Privileged Access Management](configuring-mim-environment-for-pam.md)
- [Configurar a PAM através de scripts](sp1-pam-configure-using-scripts.md)
