---
title: Implementação do Microsoft Identity Manager Certificate Manager [ Gestor de Certificados de Identidade da Microsoft ] Microsoft Docs
description: Instale o Gestor de Certificados Microsoft Identity Manager 2016
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/19/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 35fe08363b6964bf6d264ab1e60cd9751aa7b6aa
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "79043040"
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Implementação do Microsoft Identity Manager Certificate Manager 2016 (MIM CM)

A instalação do Microsoft Identity Manager Certificate Manager 2016 (MIM CM) envolve uma série de etapas. Como forma de simplificar o processo, estamos a desfazer as coisas. Há medidas preliminares que devem ser tomadas antes de quaisquer passos reais do CM mim. Sem o trabalho preliminar, é provável que a instalação falhe.

O diagrama abaixo mostra um exemplo do tipo de ambiente que pode ser usado. Os sistemas com números estão incluídos na lista abaixo do diagrama e são obrigados a completar com sucesso os passos abrangidos por este artigo. Por fim, os Servidores do Datacenter do Windows 2016 são utilizados:

![Diagrama de ambiente](media/mim-cm-deploy/image001.png)

1. CORPDC – Controlador de Domínio
2. CORPCM - MIM CM Server
3. CORPCA – Autoridade de Certificados
4. CORPCMR – MIM CM Rest API Web – Portal CM para descanso API – Usado para mais tarde
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1 - Domínio do Windows 10 Unidos

## <a name="deployment-overview"></a>Visão geral de implantação

- Instalação do sistema operativo base

    O laboratório é composto por servidores do Datacenter windows 2016.

    >[!NOTE]
    >Para mais detalhes sobre as plataformas suportadas para mim 2016, dê uma olhada no artigo intitulado [Plataformas suportadas para MIM 2016](microsoft-identity-manager-2016-supported-platforms.md).

1. Etapas de pré-implantação

    - [Estender o esquema](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)

    - Criação de contas de serviço

    - [Criação de modelos de certificado](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)

    - IIS

    - Configurar Kerberos

    - Passos relacionados com a base de dados

        - Requisitos de configuração SQL

        - Permissões de base de dados

2. Automática

## <a name="pre-deployment-steps"></a>Etapas de pré-implantação

O assistente de configuração MIM CM requer informações a fornecer ao longo do caminho para que possa ser concluída com sucesso.

![diagrama](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>Estender o esquema

O processo de extensão do esquema é simples, mas deve ser abordado com cautela devido à sua natureza irreversível.

>[!NOTE]
>Este passo requer que a conta utilizada tenha direitos de administração de esquemas.

1. Navegue na localização dos meios MIM e navegue para \\Gestão de Certificados\\x64.

2. Copie a pasta Schema para a CORPDC e navegue até ela.

    ![diagrama](media/mim-cm-deploy/image005.png)

3. Executar o script recursosForestModifySchema.vbs cenário único Forest. Para o cenário da floresta de recursos executar os scripts:
   - DomainA – Utilizadores localizados (userForestModifySchema.vbs)
   - ResourceForestB – Localização da instalação CM (recursosForestModifySchema.vbs).

     >[!NOTE]
     >As alterações de schema são uma operação de ida e exigem uma recuperação florestal para recuar, por isso certifique-se de que tem cópias de segurança necessárias. Para mais detalhes sobre as alterações feitas ao esquema através da realização desta operação, reveja o artigo Gestor de [Identidade de Vanguarda 2010 Gestão de Certificados Schema Changes](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx)

     ![diagrama](media/mim-cm-deploy/image007.png)

4. Execute o script e receberá uma mensagem de sucesso assim que o script estiver concluído.

    ![Mensagem de êxito](media/mim-cm-deploy/image009.png)

O esquema em AD é agora estendido para apoiar mim CM.

### <a name="creating-service-accounts-and-groups"></a>Criação de contas de serviços e grupos

O quadro seguinte resume as contas e permissões exigidas pela MIM CM. Pode permitir que o CM MIM crie automaticamente as seguintes contas ou pode criá-las antes da instalação. Os nomes reais da conta podem ser alterados. Se criar as contas por si mesmo, considere nomear as contas de utilizador de tal forma que é fácil comparar o nome da conta do utilizador com a sua função.

Utilizadores:

![Diagrama](media/mim-cm-deploy/image010.png)

![Diagrama](media/mim-cm-deploy/image012.png)

| **Função**                   | **Registo do utilizador no nome** |
|----------------------------|---------------------|
| Agente MIM CM               | AGENTE MIMCM          |
| Agente de recuperação chave mim CM  | AGENTE MIMCMKR        |
| Agente de autorização mim CM | MIMCMAuthAgent      |
| Agente gestor do MIM CM CA    | AGENTE MIMCMManager   |
| Agente da piscina web mim CM      | AGENTE MIMCMWebAgent       |
| Agente de inscrição mim CM    | Agente mimcminscrito    |
| Serviço de Atualização MIM CM      | Serviço MIMCM        |
| Conta de instalação mim        | MIMINSTALL          |
| Agente de mesa de ajuda            | CMHelpdesk1-2       |
| Gestor cm                 | CMManager1-2        |
| Utilizador Assinante            | CMUser1-2           |

Grupos:

| **Função**               | **Grupo**         |
|------------------------|-------------------|
| Membros do helpdesk cm    | MIMCM-Helpdesk    |
| Membros do Gestor cm     | MIMCM-Managers    |
| Membros dos Assinantes da CM | MIMCM-Subscritores |

Powershell: Contas de agente:

```powershell
import-module activedirectory
## Agent accounts used during setup
$cmagents = @{
"MIMCMKRAgent" = "MIM CM Key Recovery Agent"; 
"MIMCMAuthAgent" = "MIM CM Authorization Agent"
"MIMCMManagerAgent" = "MIM CM CA Manager Agent";
"MIMCMWebAgent" = "MIM CM Web Pool Agent";
"MIMCMEnrollAgent" = "MIM CM Enrollment Agent";
"MIMCMService" = "MIM CM Update Service";
"MIMCMAgent" = "MIM CM Agent";
}
##Groups Used for CM Management
$cmgroups = @{
"MIMCM-Managers" = "MIMCM-Managers"
"MIMCM-Helpdesk" = "MIMCM-Helpdesk"
"MIMCM-Subscribers" = "MIMCM-Subscribers" 
}
##Users Used during testlab
$cmusers = @{
"CMManager1" = "CM Manager1"
"CMManager2" = "CM Manager2"
"CMUser1" = "CM User1"
"CMUser2" = "CM User2"
"CMHelpdesk1" = "CM Helpdesk1"
"CMHelpdesk2" = "CM Helpdesk2"
}

## OU Paths
$aoupath = "OU=Service Accounts,DC=contoso,DC=com" ## Location of Agent accounts
$oupath = "OU=CMLAB,DC=contoso,DC=com" ## Location of Users and Groups for CM Lab


#Create Agents – Update UserprincipalName
$cmagents.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -UserPrincipalName ($_.Name + "@contoso.com")  -Path $aoupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}


#Create Users
$cmusers.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -Path $oupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}
```

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>Atualizar política local do servidor **CORPCM** para contas de agente 

| **Registo do utilizador no nome** | **Descrição e permissões**   |
|------|---------------------|
| AGENTE MIMCM          | Presta os seguintes serviços: </br>- Recupera chaves privadas encriptadas da AC. </br>- Protege a informação PIN do cartão inteligente na base de dados FIM CM. </br>- Protege a comunicação entre o FIM CM e a AC. </br></br> Esta conta de utilizador requer as seguintes definições de controlo de acesso:</br>-   Permitir o **logon localmente** direito do utilizador.</br>-   **Emitir e Gerir certificados** direito. </br>- Ler e escrever permissão na pasta Temp do sistema no seguinte local: %WINDIR%\\Temp.</br>- Um certificado de assinatura digital e encriptação emitido e instalado na loja de utilizadores.
|AGENTE MIMCMKR        | Recupera chaves privadas arquivadas da AC. Esta conta de utilizador requer as seguintes definições de controlo de acesso:</br> -   Permitir o **logon localmente** direito do utilizador.</br>- Adesão ao grupo de **administradores locais.** </br>- Inscreva a permissão no modelo de certificado **KeyRecoveryAgent.** </br>- O certificado do Agente de Recuperação chave é emitido e instalado na loja do utilizador. O certificado deve ser adicionado à lista dos principais agentes de recuperação da AC. </br>- Ler permissão e escrever permissão na pasta Temp do sistema no seguinte local: ```%WINDIR%\\Temp.```                                                                                                                     |
| MIMCMAuthAgent      | Determina os direitos e permissões dos utilizadores para utilizadores e grupos. Esta conta de utilizador requer as seguintes definições de controlo de acesso: </br>- Adesão ao grupo de domínio de acesso compatível pré-Windows 2000. </br> - Concedido o direito de utilizador de auditorias de **segurança Generate.**             |
| AGENTE MIMCMManager   | Realiza atividades de gestão de CA. </br> Este utilizador deve ser atribuído à permissão Manage CA.        |
| AGENTE MIMCMWebAgent       | Fornece a identidade para o conjunto de aplicações IIS. O FIM CM funciona dentro de um processo de interface de programação de aplicações ® Microsoft Win32 que utiliza as credenciais deste utilizador. </br> Esta conta de utilizador requer as seguintes definições de controlo de acesso:</br> - Adesão à **IIS_WPG local, janelas 2016 = IIS_IUSRS** grupo. </br>- Adesão ao grupo de **administradores locais.**</br>- Concedido o direito de utilizador de auditorias de **segurança Generate.** </br>- Concedida a Lei como parte do direito do utilizador **do sistema operativo.** </br>- Concedido o direito de substituição do utilizador do **nível de processo.**</br>- Atribuído como identidade do conjunto de aplicações IIS, **CLMAppPool**. </br>- Autorização de leitura concedida no **software HKEY_LOCAL_MACHINE\\\\Microsoft\\CLM\\v1.0\\Server\\WebUser.** </br>- (OS) deve também ser confiado a esta conta para a delegação.|
| Agente mimcminscrito    | Realiza a inscrição em nome de um utilizador. Esta conta de utilizador requer as seguintes definições de controlo de acesso:</br>- Um certificado de Agente de Inscrição emitido e instalado na loja de utilizadores.</br>-   Permitir o **logon localmente** direito do utilizador. </br>- Inscrever a permissão no modelo de certificado do **Agente de Inscrição** (ou no modelo personalizado, se um for utilizado).                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>Criação de modelos de certificado para contas de serviço MIM CM

Três das contas de serviço utilizadas pela MIM CM requerem um certificado e o assistente de configuração exige que forneça o nome dos modelos de certificados que deve utilizar para solicitar certificados para os mesmos.

As contas de serviço que requerem certificados são:

- MIMCMAgent: Esta conta precisa de um certificado de utilizador

- MIMCMEnrollAgent: Esta conta precisa de um certificado de agente de inscrição

- MIMCMKRAgent: Esta conta precisa de um certificado de agente de **recuperação chave**

Existem modelos já presentes em AD, mas precisamos criar as nossas próprias versões para trabalhar com mim CM. Como precisamos fazer modificações a partir dos modelos originais de base.

Todas as três contas acima terão direitos elevados dentro da sua organização e devem ser tratadas cuidadosamente.

#### <a name="create-the-mim-cm-signing-certificate-template"></a>Crie o modelo de certificado de assinatura MIM CM

1. A partir de **Ferramentas Administrativas,** Autoridade de **Certificação**Aberta.

2. Na consola da Autoridade de **Certificação,** na árvore das consolas, expanda **Contoso-CorpCA,** e depois clique em **Modelos**de Certificado .

3. Clique com botão direito do rato em **Modelos de Certificado** e, em seguida, clique em **Gerir**.

4. Na consola de modelos de **certificado,** no painel de **detalhes,** selecione e clique no direito **do utilizador,** e, em seguida, clique no **Modelo Duplicado**.

5. Na caixa de diálogo **do modelo duplicado,** selecione **Windows Server 2003 Enterprise**, e, em seguida, clique EM **OK**.

    ![Mostrar alterações resultantes](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    >Mim CM não trabalha com certificados baseados em modelos de certificado sé3. Tem de criar um modelo de certificado ® Windows Server® 2003 Enterprise (versão 2). Consulte [os detalhes da V3](https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade) para mais informações.

6. Nas propriedades da caixa de diálogo **do novo modelo,** no separador **Geral,** na caixa de **nome** si, digite mim **CM Signing**. Alterar o **Período de Validade** para 2 **anos**e, em seguida, limpar o certificado Publicar na caixa de verificação **de Diretório Ativo.**

7. No separador **'Manipulação de Pedidos',** certifique-se de que a **chave privada Permitir a exportação** da caixa de verificação é selecionada e, em seguida, clique no **separador Cryptography**.

8. Na caixa de diálogo **Cryptography Selection,** desative o **Fornecedor Criptográfico Melhorado da Microsoft v1.0,** ative o **Fornecedor Criptográfico Melhorado**da Microsoft e o AES, e depois clica **em OK**.

9. No separador Nome do **Assunto,** limpe o nome do e-mail incluir o nome do **e-mail no nome do assunto** e as caixas de verificação de nome seleções por **e-mail.**

10. No separador **Extensões,** nas **Extensões incluídas nesta** lista de modelos, certifique-se de que as Políticas de **Aplicação** são selecionadas e, em seguida, clique em **Editar**.

11. Na caixa de diálogo de extensão de políticas de **aplicação de edição,** selecione tanto o Sistema de **Ficheiros encriptando** como as políticas de aplicação **de email seguro.** Clique em **Remover** e, em seguida, clique em **OK**.

12. No separador **Segurança** efetuar os seguintes passos:

    - Remover **administrador**.

    - Remover administradores de **domínio**.

    - Remover **utilizadores de domínio**.

    - Atribuir apenas permissões **de leitura** e **escrita** aos administradores **da Empresa**.

    - Adicione **mimcmagent.**

    - Atribuir permissões **de leitura** e **inscrição** ao **MIMCMAgent**.

13. Nas propriedades da caixa de diálogo **de novo modelo,** clique **OK**.

14. Deixe a consola de **modelos** de certificado aberta.

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>Crie o modelo de certificado do agente de inscrição MIM CM

1. Na consola de modelos de **certificado,** no painel de **detalhes,** selecione e clique direito agente de **inscrição,** e, em seguida, clique no **Modelo Duplicado**.

2. Na caixa de diálogo **do modelo duplicado,** selecione **Windows Server 2003 Enterprise**, e, em seguida, clique EM **OK**.

3. Nas propriedades da caixa de diálogo **do novo modelo,** no separador **Geral,** na caixa de **nome** si, digite o agente de **inscrição MIM CM**. Certifique-se de que o Período de **Validade** é de **2 anos.**

4. No separador **De Manuseamento de Pedidos,** ative **permitir a exportação da chave privada**e, em seguida, clique em **CSPs ou Cryptography Tab.**

5. Na caixa de diálogo **CSP Selection,** desative o **Fornecedor Criptográfico da Microsoft Base v1.0**, desative o **Fornecedor Criptográfico Melhorado da Microsoft v1.0,** ative o **Fornecedor DeRSA melhorado da Microsoft e o Fornecedor Criptográfico AES,** e depois clique em **OK**.

6. No separador **Segurança** efetuar o seguinte:

    - Remover **administrador**.

    - Remover administradores de **domínio**.

    - Atribuir apenas permissões **de leitura** e **escrita** aos administradores **da Empresa**.

    - Adicione **mimcmenrollagent**.

    - Atribuir permissões **de leitura** e **inscrição** ao **MIMCMEnrollAgent**.

7. Nas propriedades da caixa de diálogo **de novo modelo,** clique **OK**.

8. Deixe a consola de **modelos** de certificado aberta.

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>Crie o modelo de certificado do agente de recuperação da chave MIM CM

1. Na consola de **modelos** de certificado, no painel de **detalhes,** selecione e clique direito no Agente de Recuperação da **Chave,** e, em seguida, clique no **Modelo Duplicado**.

2. Na caixa de diálogo **do modelo duplicado,** selecione **Windows Server 2003 Enterprise**, e, em seguida, clique EM **OK**.

3. Nas propriedades da caixa de diálogo **do novo modelo,** no separador **Geral,** na caixa de **nome** si, digite o agente de **recuperação da chave MIM CM**. Certifique-se de que o **Período** de Validade é de **2 anos** no separador de **criptografia.**

4. Na caixa de diálogo **De seleção de Fornecedores,** desative o Fornecedor **Criptográfico Melhorado da Microsoft v1.0,** ative o **Fornecedor Criptográfico RSA melhorado da Microsoft e aes,** e, em seguida, clique em **OK**.

5. No separador Requisitos de **Emissão,** certifique-se de que a aprovação do gestor de **certificados ca** está **desativada**.

6. No separador **Segurança** efetuar o seguinte:

    - Remover **administrador**.

    - Remover administradores de **domínio**.

    - Atribuir apenas permissões **de leitura** e **escrita** aos administradores **da Empresa**.

    - Adicione **mimcmkragent**.

    - Atribuir permissões **de leitura** e **inscrição** ao **KRAgent**.

7. Nas propriedades da caixa de diálogo **de novo modelo,** clique **OK**.

8. Feche a **Consola de Modelos de Certificado**.

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>Publique os modelos de certificado seletos exigidos na Autoridade de Certificação

1. Restaurar a consola da Autoridade de **Certificação.**

2. Na consola da Autoridade de **Certificação,** na árvore da consola, os **modelos**de certificado de clique direito, apontam para **Novo,** e depois clique no Modelo de **Certificado para Emitir**.

3. Na caixa de diálogo de modelos de **certificado ativa,** selecione MIM CM Agente de **inscrição,** agente de **recuperação de chaves MIM CM**e assinatura MIM **CM**. Clique em **OK**.

4. Na árvore da consola, clique em **Modelos de Certificado**.

5. Verifique se os três novos modelos aparecem no painel de **detalhes** e, em seguida, feche a Autoridade de **Certificação**.

    ![Assinatura MIM CM](media/mim-cm-deploy/image016.png)

6. Feche todas as janelas abertas e sais.

### <a name="iis-configuration"></a>Configuração IIS

Para hospedar o site para CM, instale e configure o IIS.

#### <a name="install-and-configure-iis"></a>Instalar e configurar o IIS

1. Iniciar sessão no **CORLog na** conta **MIMINSTALL**

    >[!IMPORTANT]
    >A conta de instalação mim deve ser um administrador local

2. Open PowerShell e executar o seguinte comando

    `Install-WindowsFeature –ConfigurationFilePath`

>[!NOTE]
>Um site chamado Default Web Site é instalado por padrão com o IIS 7. Se esse site foi renomeado ou removido um site com o nome 'Web Site' predefinido deve estar disponível antes de o CM MIM poder ser instalado.

#### <a name="configuring-kerberos"></a>Configurar Kerberos

A conta MIMCMWebAgent estará a executar o portal MIM CM. Por predefinição no IIS e a autenticação do modo kernel é utilizada no IIS por defeito. Irá desativar a autenticação do modo Kernel Kerberos e configurar SPNs na conta MIMCMWebAgent. Algum comando requer permissão elevada no diretório ativo e no servidor CORPCM.

![Diagrama](media/mim-cm-deploy/image020.png)

```powershell
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}
```

**Atualizar iIS no CORPCM**

![diagrama](media/mim-cm-deploy/image022.png)

```powershell
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true
```

>[!NOTE]
>Você precisará adicionar um DNS A Record para o "cm.contoso.com" e apontar para CORPCM IP

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>Requerendo SSL no portal MIM CM

É altamente recomendável que necessite de SSL no portal MIM CM. Se não o fizeres, o feiticeiro vai avisá-lo.

1. Inscreva-se em certificado web para **cm.contoso.com** atribuir ao site predefinido

2. Abra **o Gestor IIS** e navegue para **a Gestão de Certificados**

3. Na Vista de Funcionalidades, faça duplo clique em Definições de SSL.

4. Na página Definições SSL, selecione **RequireS SSL**.

5. No painel de Ações, clique **em Aplicar.**

### <a name="database-configuration-corpsql-for-mim-cm"></a>Configuração de base de dados **CORPSQL** para MIM CM

1. Certifique-se de que está ligado ao Servidor CORPSQL01.

2. Certifique-se de que está ligado como SQL DBA.

3. Executar o seguinte script T-SQL para permitir que a conta DECONSEAÇÃO\\CONTOSO crie a base de dados quando formos para o passo de configuração

    >[!NOTE]
    >Teremos de voltar à SQL quando estivermos prontos para o módulo de saída e política

    ```sql
    create login [CONTOSO\\MIMINSTALL] from windows;
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
    exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
    ```

![Mensagem de erro do assistente de configuração MIM CM](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Implementação da Microsoft Identity Manager 2016 Certificate Management

1. Certifique-se de que está ligado ao Servidor CORPCM e que a conta **MIMINSTALL** é membro do grupo de **administradores locais.**

2. Certifique-se de que está ligado como Contoso\\MIMINSTALL.

3. Monte o Microsoft Identity Manager SP1 ISO.

4. **Abra** o diretório **de Gestão de Certificados\\x64.**

5. Na janela **x64,** clique à direita **Configuração,** e clique em **Executar como administrador**.

6. Na página Welcome to the Microsoft Identity Manager Certificate Management Wizard, clique em **Next.**

7. Na página do Contrato de Licença de Utilizador Final, leia o acordo, permita que o I aceite os termos na caixa de **verificação**do contrato de licença e, em seguida, clique em Seguinte.

8. Na página de Configuração Personalizada, certifique-se de que os componentes do **Portal MIM CM** e do Serviço de Atualização MIM CM estão **definidos** para serem instalados e, em seguida, **clique em Next**.

9. Na página da Pasta Web Virtual, certifique-se de que o nome da pasta Virtual é **CertificateManagement**, e, em seguida, **clique em Seguinte**.

10. Na página de Gestão de Certificados do Gestor de Identidade da Microsoft, **clique em Instalar**.

11. Na **página completa da** configuração do Assistente de Gestão de Certificados de Identidade da Microsoft, **clique em Terminar**.

![Mim CM assistente concluído](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Assistente de Configuração do Microsoft Identity Manager 2016 Gestão de Certificados

Antes de iniciar sessão no CORPCM, adicione mimINSTALL a **administrações de domínio, administrações de Schema e grupo de administradores locais** para o assistente de configuração . Isto pode ser removido mais tarde assim que a configuração estiver completa.

![Mensagem de erro](media/mim-cm-deploy/image028.png)

1. A partir do menu **Iniciar,** clique em **Certificado Management Config Wizard**. E correr como **administrador**

2. Na página **Welcome to the Configuration Wizard,** clique em **Next**.

3. Na página de **Configuração CA,** certifique-se de que o CA selecionado é **Contoso-CORPCA-CA, certifique-se**de que o servidor selecionado é **CORPCA. CONTOSO.COM,** e depois clique em **Seguinte**.

4. Na configuração da página de base de dados **® Microsoft® SQL®,** no nome da caixa **do Servidor SQL,** tipo **CORPSQL1,** ative a utilização das **minhas credenciais para criar a** caixa de verificação da base de dados e, em seguida, clique em **Next**.

5. Na página Definições da Base de **Dados,** aceite o nome de base de dados predefinido da **FIMCertificateManagement,** certifique-se de que a **autenticação integrada SQL** é selecionada e, em seguida, clique em **Seguinte**.

6. Na página **'Diretório Activo',** aceite o nome predefinido previsto para o ponto de ligação de serviço e, em seguida, clique em **Next**.

7. Na página do **método de Autenticação,** confirme que é selecionada **a Autenticação Integrada do Windows** e, em seguida, clique em **Seguinte**.

8. Na página **Agentes – FIM CM,** limpe a caixa de verificação de **definições padrão** DO FIM CM e, em seguida, clique em **Contas Personalizadas**.

9. Na caixa de diálogo multi-tabed **DE Agentes – CM,** em cada separador, escreva as seguintes informações:

   - Nome do utilizador: **Atualização**

   - Palavra-passe: **Passe\@palavra1**

   - Confirmar Palavra-passe: **Passe\@palavra1**

   - Utilize um utilizador existente: **Ativado**

     >[!NOTE]
     >Criámos estas contas mais cedo. Certifique-se de que os procedimentos no passo 8 são repetidos para os seis separadores de conta de agente.

     ![Contas MIM CM](media/mim-cm-deploy/image030.png)

10. Quando todas as informações da conta do agente estiverem completas, clique em **OK**.

11. Na página **Agentes – MIM CM,** clique **em Next**.

12. Na página **de configurar os certificados** do servidor, ative os seguintes modelos de certificado:

    - Modelo de certificado a utilizar para o certificado do agente de recuperação chave de recuperação: **MIMCMKeyRecoveryAgent**.

    - Modelo de certificado a utilizar para o certificado de agente FIM CM: **MIMCMSigning**.

    - Modelo de certificado a utilizar para o certificado do agente de matrícula: **FIMCMEnrollmentAgent**.

13. Na página de **certificados de servidor de configuração,** clique em **Next**.

14. No Servidor de **E-mail de Configuração,** página de Impressão de Documentos, na **especificação do nome do servidor SMTP que pretende utilizar para** a caixa de notificações de registo de e-mail e, em seguida, clique em **Seguinte.**

15. Na página **Ready to configure,** clique em **Configurar**.

16. Na caixa de diálogo de aviso Do Assistente de **Configuração – Microsoft Forefront Identity Manager 2010 R2,** clique em **OK** para reconhecer que o SSL não está ativado no diretório virtual IIS.

    ![meios/imagem17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    >Não clique no botão 'Terminar' até que a execução do assistente de configuração esteja completa. O registo do assistente pode ser consultado aqui: **%programfiles%\\Microsoft Forefront Identity Management\\2010\\Gestão de Certificados\\config.log**

17. Clique em **Concluir**.

    ![Mim CM assistente concluído](media/mim-cm-deploy/image033.png)

18. Feche todas as janelas abertas.

19. Adicione `https://cm.contoso.com/certificatemanagement` à zona intranet local no seu navegador.

20. Visite o site do servidor CORPCM `https://cm.contoso.com/certificatemanagement`  

    ![diagrama](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>Verifique o Serviço de Isolamento da Chave CNG

1. De **Ferramentas Administrativas,** **serviços abertos.**

2. No painel de **detalhes,** clique duas vezes no isolamento da **chave cng**.

3. No separador **Geral,** altere o Tipo de **Arranque** para **Automático**.

4. No separador **Geral,** inicie o serviço se não estiver em estado de arranque.

5. No separador **Geral,** clique **OK**.

### <a name="installing-and-configuring-the-ca-modules-"></a>Instalação e configuração dos módulos CA:

Neste passo, vamos instalar e configurar os módulos FIM CM CA na autoridade de certificação.

1. Configure FIM CM para apenas inspecionar permissões dos utilizadores para operações de gestão

2. No **C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Gestão de Certificados\\** janela web, faça uma cópia do **web.config** nomeando a **cópia web.1.config**.

3. Na janela **web,** clique na **web.config,** clique em **Abrir**.

    >[!Note]
    >O ficheiro Web.config é aberto no bloco de notas

4. Quando o ficheiro abrir, prima CTRL+F.

5. Na caixa de diálogo **Find and Replace,** na **caixa Find what** box, type **UseUser**, e, em seguida, clique em **Encontrar O Próximo** três vezes.

6. Feche a caixa de diálogo **Find and Replace.**

7. Deve estar na linha **\<adicionar key="Clm.RequestSecurity.Flags" valor="UseUser,UseGroups" /\>** . Altere a linha para ler **\<adicione key="Clm.RequestSecurity.Flags" valor="UseUser" /\>** .

8. Feche o ficheiro, guardando todas as alterações.

9. Criar uma conta para o computador CA no servidor SQL \<nenhum\> de script

10. Certifique-se de que está ligado ao servidor **CORPSQL01.**

11. Certifique-se de que está ligado como **DBA**

12. A partir do menu **Iniciar,** lance o **Estúdio de Gestão de Servidores SQL**.

13. Na caixa de diálogo **Connect to Server,** na caixa de nome sinuosa, digite **CORPSQL01 e,** em seguida, clique em **Connect**.

14. Na árvore da consola, expanda **a Segurança**e, em seguida, clique **em Logins**.

15. Clique no **login si**mesmo e, em seguida, clique em **New Login**.

16. Na página **Geral,** na caixa de **nomelogina Login,** **digite contoso\\\$CORPCA** . **Selecione A autenticação do Windows**. A base de dados predefinida é **FIMCertificateManagement**.

17. No painel esquerdo, selecione **User Mapping**. No painel direito, clique na caixa de verificação na coluna **do Mapa** ao lado da **FIMCertificateManagement**. Na base de **dados de subscrição de funções para: LISTA FIMCertificateManagement,** ative a função **clmApp.**

18. Clique em **OK**.

19. Feche o **Microsoft SQL Server Management Studio**.

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>Instalar os módulos FIM CM CA na Autoridade de Certificação

1. Certifique-se de que está ligado ao servidor **CORPCA.**

2. Nas janelas **X64,** clique à direita **Setup.exe**, e, em seguida, clique em **Executar como administrador**.

3. Na **página Welcome to the Microsoft Identity Manager Certificate Management Wizard,** clique em **Next**.

4. Na página do Contrato de **Licença de Utilizador Final,** leia o acordo. Selecione os **termos i aceito os termos na** caixa de verificação do contrato de licença e, em seguida, clique em **Seguinte**.

5. Na página de **Configuração Personalizada,** selecione **MIM CM Portal**, e clique em esta funcionalidade não estará **disponível**.

6. Na página de **Configuração Personalizada,** selecione **MIM CM Update Service**, e, em seguida, clique nesta funcionalidade não estará **disponível**.

    >[!Note]
    >Isto deixará os Ficheiros CA MIM CM como a única funcionalidade ativada para a instalação.

7. Na página de **Configuração Personalizada,** clique em **Seguinte**.

8. Na página **de Gestão** de Certificados do Gestor de Identidade da Microsoft, clique em **Instalar**.

9. Na **página completa da configuração do Assistente** de Gestão de Certificados de Identidade da Microsoft, clique em **Terminar**.

10. Feche todas as janelas abertas.

### <a name="configure-the-mim-cm-exit-module"></a>Configure o módulo de saída MIM CM

1. A partir de **Ferramentas Administrativas,** Autoridade de **Certificação**Aberta.

2. Na árvore da consola, clique direito **contoso-CORPCA-CA,** e depois clique em **Propriedades**.

3. No separador **Módulo de Saída,** selecione Módulo de **Saída FIM CM**, e, em seguida, clique em **Propriedades**.

4. Na caixa de cadeias de **ligação de base de dados CM,** escreva **Tempo de Ligação=15; Persistir informações de segurança=verdadeiras; Segurança Integrada=sspi;Catálogo Inicial=FIMCertificateManagement;Data Source=CORPSQL01**. Deixe ativada a caixa de verificação **de fios de ligação Encrypt e,** em seguida, clique em **OK**.
5. Na caixa de mensagens **Microsoft FIM Certificate Management,** clique em **OK**.

6. Na caixa de diálogo **contoso-CORPCA-CA Properties,** clique em **OK**.

7. Clique direito **contoso-CORPCA-CA** *,* aponte para todas as **tarefas,** e, em seguida, clique no Serviço **stop**. Aguarde até que deixa de serviços de certificados do Active Directory.

8. Clique direito **contoso-CORPCA-CA** *,* aponte para todas as **tarefas,** e, em seguida, clique no Serviço **iniciar**.

9. Minimize a consola da Autoridade de **Certificação.**

10. De **Ferramentas Administrativas,** espectador de **eventos**abertos.

11. Na árvore da consola, expanda registos de **aplicações e serviços,** e depois clique na **Gestão de Certificados FIM.**

12. Na lista de eventos, verifique se os eventos mais recentes *não* incluem quaisquer eventos de **Aviso** ou **Erro** desde o último reinício dos Serviços de Certificados.

    >[!NOTE] 
    >O último evento deve indicar que o Módulo de Saída carregou com definições de: `SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit`

13. Minimize o Espectador de **Eventos.**

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>Copie a impressão digital do Certificado MIMCMAgent para o Windows® clipboard

1. Restaurar a consola da Autoridade de **Certificação.**

2. Na árvore da consola, expanda **contoso-CORPCA-CA,** e depois clique em **Certificados Emitidos**.

3. No painel de **detalhes,** clique duas vezes no certificado com **CONTOSO\\MIMCMAgent** na coluna Nome do Solicitado récis e com a assinatura DO **CM** na coluna modelo do **certificado.**

4. No separador **Detalhes**, selecione o campo **Thumbprint**.

5. Selecione a impressão digital e, em seguida, prima CTRL+C.

    >[!NOTE]
    >**Não** inclua o espaço principal na lista de caracteres de impressão digital.

6. Na caixa de diálogo **do Certificado,** clique **ok**.

7. A partir do menu **Iniciar,** na caixa de **programas de pesquisa e ficheiros,** digite o Bloco de **Notas**e, em seguida, prima ENTER.

8. No Bloco de **Notas**, a partir do menu **Editar,** clique em **Paste**.

9. A partir do menu **Editar,** clique **em Substituir**.

10. Na caixa **Find what** box, digite um personagem de espaço e, em seguida, clique em **Substituir Tudo**.

    >[!Note]
    >Isto remove todos os espaços entre os caracteres na impressão digital.

11. Na caixa de diálogo **Substituir,** clique **em Cancelar**.

12. Selecione a cadeia de *impressão digital*convertida e, em seguida, prima CTRL+C.

13. Fechar **o Bloco** de Notas sem guardar alterações.

### <a name="configure-the-fim-cm-policy-module"></a>Configure o Módulo político FIM CM

1. Restaurar a consola da Autoridade de **Certificação.**

2. Clique direito **contoso-CORPCA-CA,** e, em seguida, clique **em Propriedades**.

3. Na caixa de diálogo **contoso-CORPCA-CA Properties,** no separador **Módulo Político,** clique em **Propriedades**.

    - No separador **Geral,** certifique-se de que o **Passe CM não-FIM solicita ao módulo de política padrão para processamento.**

    - No separador **Certificados de Assinatura,** clique em **Adicionar**.

    - Na caixa de diálogo do Certificado, clique à direita na caixa de **hash do certificado codificado por hexás** e, em seguida, clique em **Paste**.

    - Na caixa de diálogo **do Certificado,** clique **ok**.

        >[!Note]
        >Se o botão **OK** não estiver ativado, incluiu acidentalmente um personagem oculto na cadeia de impressão digital quando copiou a impressão digital do certificado clmAgent. Repita todos os passos a partir da **Tarefa 4: Copie a impressão digital do Certificado MIMCMAgent para a Clipboard windows** neste exercício.

4. Na caixa de diálogo **Configuração Properties,** certifique-se de que a impressão digital aparece na lista de Certificados de **Assinatura Válidas** e, em seguida, clique em **OK**.

5. Na caixa de mensagens **FIM Certificate Management,** **clique**OK .

6. Na caixa de diálogo **contoso-CORPCA-CA Properties,** clique em **OK**.

7. Clique direito **contoso-CORPCA-CA** *,* aponte para todas as **tarefas,** e, em seguida, clique no Serviço **stop**.

8. Aguarde até que deixa de serviços de certificados do Active Directory.

9. Clique direito **contoso-CORPCA-CA** *,* aponte para todas as **tarefas,** e, em seguida, clique no Serviço **iniciar**.

10. Feche a consola da Autoridade de **Certificação.**

11. Feche todas as janelas abertas e depois sais.

**O último passo na implementação** é que queremos garantir que o CONTOSO\\MIMCM-Managers possa implementar e criar modelos e configurar o sistema sem ser schema e Administração de Domínio. O próximo script irá ACL as permissões para os modelos de certificado usando dsacls. Por favor, corra com conta que tenha permissão total para alterar a segurança Ler e Escrever permissões para cada modelo de certificado existente na floresta.

Primeiros passos: Configurar o ponto de **ligação ao serviço e as permissões do grupo alvo e delegar a gestão** do modelo de perfil

1. Configure permissões no ponto de ligação de serviço (SCP).

2. Configure a gestão do modelo de perfil delegado.

3. Configure permissões no ponto de ligação de serviço (SCP). **\<nenhum guião\>**

4.   Certifique-se de que está ligado ao servidor virtual **CORPDC.**

5. Inicie sessão como **contoso\\corpadmin**

6. A partir de **Ferramentas Administrativas,** abra **utilizadores e computadores de diretório ativo.**

7. No **Ative Directory Users and Computers,** no menu **'Ver',** certifique-se de que **as Funcionalidades Avançadas** estão ativadas.

8. Na árvore da consola, expanda **Contoso.com** **sistema** \| \| **Microsoft** \| **Certificate Lifecycle Manager**, e, em seguida, clique em **CORPCM**.

9. Clique à direita **CORPCM**, e depois clique em **Propriedades**.

10. Na caixa de diálogo **CORPCM Properties,** no separador **Segurança,** adicione os seguintes grupos com as permissões correspondentes:

    | Grupo          | Permissões      |
    |----------------|------------------|
    | mimcm-Managers | Ler </br> Auditoria FIM CM</br> Agente de inscrição fim CM</br> Inscrição de Pedido de Pedido fim CM</br> Pedido de recuperação de CM fim</br> Pedido de Cm fim Renovar</br> Pedido de C.C. fim revogação </br> Pedido de CM fim desbloquear cartão inteligente |
    | mimcm-HelpDesk | Ler</br> Agente de inscrição fim CM</br> Pedido de C.C. fim revogação</br> Pedido de CM fim desbloquear cartão inteligente |

11. Na caixa de diálogo **CORPDC Properties,** **clique**OK .

12. Deixe **os utilizadores e computadores de diretório ativo abertos.**

**Configure permissões nos objetos de utilizador descendentes**

1. Certifique-se de que ainda se encontra na consola **Ative Directory Users and Computers.**

2. Na árvore da consola, clique à direita **Contoso.com**, e depois clique em **Propriedades**.

3. No separador **Segurança**, clique em **Avançadas**.

4. Nas **Definições avançadas** de segurança para a caixa de diálogo Contoso, clique em **Adicionar**.

5. Na caixa de diálogo **Select User, Computer, Service Account ou Group,** **insira o nome do objeto para selecionar** a caixa, digite **mimcm-Managers**e, em seguida, clique em **OK**.

6. Na caixa de diálogo **De Supor Contoso,** na lista **Apply,** selecione **Objetos De utilizador descendentes** e, em seguida, ative a caixa de verificação **de autorização** para as **seguintes Permissões:**

    - **Ler todas as propriedades**

    - **Ler permissões**

    - **Auditoria FIM CM**

    - **Agente de inscrição fim CM**

    - **Inscrição de Pedido de Pedido fim CM**

    - **Pedido de recuperação de CM fim**

    - **Pedido de Cm fim Renovar**

    - **Pedido de C.C. fim revogação**

    - **Pedido de CM fim desbloquear cartão inteligente**

7. Na caixa de diálogo **De Smpermissão para Contoso,** clique em **OK**.

8. Nas **Definições avançadas** de segurança para a caixa de diálogo Contoso, clique em **Adicionar**.

9. Na caixa de diálogo **Select User, Computer, Service Account ou Group,** **insira o nome do objeto para selecionar** a caixa, digite **mimcm-HelpDesk,** e, em seguida, clique em **OK**.

10. Na caixa de diálogo **De Smpermissão para Contoso,** na lista **Apply,** selecione **Objetos De Utilizador Descendentee,** em seguida, selecione a caixa de verificação **de autorização** para as **seguintes Permissões:**

    - **Ler todas as propriedades**

    - **Ler permissões**

    - **Agente de inscrição fim CM**

    - **Pedido de C.C. fim revogação**

    - **Pedido de CM fim desbloquear cartão inteligente**

11. Na caixa de diálogo **De Smpermissão para Contoso,** clique em **OK**.

12. Nas **Definições avançadas** de segurança para a caixa de diálogo Contoso, clique em **OK**.

13. Na caixa de diálogo **contoso.com Properties,** clique em **OK**.

14. Deixe **os utilizadores e computadores de diretório ativo abertos.**

**Configure permissões nos objetos de utilizador descendentes \<nenhum\> de script**

1. Certifique-se de que ainda se encontra na consola **Ative Directory Users and Computers.**

2. Na árvore da consola, clique à direita **Contoso.com**, e depois clique em **Propriedades**.

3. No separador **Segurança**, clique em **Avançadas**.

4. Nas **Definições avançadas** de segurança para a caixa de diálogo Contoso, clique em **Adicionar**.

5. Na caixa de diálogo **Select User, Computer, Service Account ou Group,** **insira o nome do objeto para selecionar** a caixa, digite **mimcm-Managers**e, em seguida, clique em **OK**.

6. Na caixa de autorização para a caixa de diálogo **CONTOSO,** na lista **Apply,** selecione **Objetos de Utilizador Descendente** e, em seguida, ative a caixa de verificação **de autorização** para as **seguintes Permissões:**

    - **Ler todas as propriedades**

    - **Ler permissões**

    - **Auditoria FIM CM**

    - **Agente de inscrição fim CM**

    - **Inscrição de Pedido de Pedido fim CM**

    - **Pedido de recuperação de CM fim**

    - **Pedido de Cm fim Renovar**

    - **Pedido de C.C. fim revogação**

    - **Pedido de CM fim desbloquear cartão inteligente**

7. Na entrada de autorização para caixa de diálogo **CONTOSO,** clique **OK**.

8. Nas **Definições avançadas de segurança para** a caixa de diálogo CONTOSO, clique em **Adicionar**.

9. Na caixa de diálogo **Select User, Computer, Service Account ou Group,** **insira o nome do objeto para selecionar** a caixa, digite **mimcm-HelpDesk,** e, em seguida, clique em **OK**.

10. Na caixa de **permissão para CONTOSO,** na lista **Apply,** selecione **Objetos de Utilizador Descendente** e, em seguida, selecione a caixa de verificação **de autorização** para as **seguintes Permissões:**

    - **Ler todas as propriedades**

    - **Ler permissões**

    - **Agente de inscrição fim CM**

    - **Pedido de C.C. fim revogação**

    - **Pedido de CM fim desbloquear cartão inteligente**

11. Na entrada de autorização para caixa de diálogo **contoso,** clique **OK**.

12. Nas **Definições avançadas** de segurança para a caixa de diálogo Contoso, clique em **OK**.

13. Na caixa de diálogo **contoso.com Properties,** clique em **OK**.

14. Deixe **os utilizadores e computadores de diretório ativo abertos.**

Segundas Etapas: **Delegar Permissões de gestão de modelo sinuosos \<script\>**

- Delegar permissões no recipiente de modelos de certificado.

- Delegar permissões no recipiente OID.

- Delegar permissões nos modelos de certificado existentes.

Definir permissões no recipiente de modelos de certificado:

1. Restaurar a consola **Ative Directory Sites and Services.**

2. Na árvore da consola, expandir **Serviços,** expandir **Serviços de Chave Pública,** e, em seguida, clicar em **Modelos de Certificado**.

3. Na árvore da consola, clique direito **nos modelos**de certificado, e, em seguida, clique no **Controlo de Delegados**.

4. Na **Delegação do** Feiticeiro de Controlo, clique em **Next**.

5. Na página **Utilizadores ou Grupos,** clique em **Adicionar**.

6. Na caixa de diálogo **Select Users, Computers ou Groups,** **insira os nomes do objeto para selecionar** a caixa, digite **mimcm-Managers**e, em seguida, clique em **OK**.

7. Na página **Utilizadores ou Grupos,** clique em **Seguinte**.

8. Na página **Tarefas para Delegar,** clique **em Criar uma tarefa personalizada para delegar,** e depois clique em **Seguinte**.

9.  Na página **Tipo de Objeto de Diretório Ativo,** certifique-se de que esta **pasta, objetos existentes nesta pasta e criação de novos objetos nesta pasta** são selecionados e, em seguida, clique em **Seguinte**.

10. Na página **Permissões,** na lista de **Permissões,** selecione a caixa de verificação **de Controlo Completo** e, em seguida, clique em **Next**.

11. Na página **De completar a Delegação de Assistente de Controlo,** clique em **Terminar**.

Defina permissões no recipiente OID:

1. Na árvore da consola, clique no **OID**direito, e depois clique em **Propriedades**.

2. Na caixa de diálogo **OID Properties,** no separador **Segurança,** clique **em Advanced**.

3. Nas **Definições avançadas de segurança para** a caixa de diálogo OID, clique em **Adicionar**.

4. Na caixa de diálogo **Select User, Computer, Service Account ou Group,** **insira o nome do objeto para selecionar** a caixa, digite **mimcm-Managers**e, em seguida, clique em **OK**.

5. Na caixa de diálogo De **Acesso a Permissões,** certifique-se de que as permissões se aplicam a **Este objeto e a todos os objetos descendentes,** clique em **Controlo Completo**e, em seguida, clique em **OK**.

6. Nas **Definições avançadas de segurança para** a caixa de diálogo OID, clique em **OK**.

7. Na caixa de diálogo **OID Properties,** clique em **OK**.

8. Fechar **sites e serviços de diretório ativo.**

**Scripts: Permissões no recipiente de modelos de perfil e modelos de certificado**

![diagrama](media/mim-cm-deploy/image021.png)

```powershell
import-module activedirectory
$adace = @{
"OID" = "AD:\\CN=OID,CN=Public Key Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"CT" = "AD:\\CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com";
"PT" = "AD:\\CN=Profile Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=contoso,DC=com"
}
$adace.GetEnumerator() | **Foreach-Object** {
$acl = **Get-Acl** *-Path* $_.Value
$sid=(**Get-ADGroup** "MIMCM-Managers").SID
$p = **New-Object** System.Security.Principal.SecurityIdentifier($sid)
##https://msdn.microsoft.com/library/system.directoryservices.activedirectorysecurityinheritance(v=vs.110).aspx
$ace = **New-Object** System.DirectoryServices.ActiveDirectoryAccessRule
($p,[System.DirectoryServices.ActiveDirectoryRights]"GenericAll",[System.Security.AccessControl.AccessControlType]::Allow,
[DirectoryServices.ActiveDirectorySecurityInheritance]::All)
$acl.AddAccessRule($ace)
**Set-Acl** *-Path* $_.Value *-AclObject* $acl
}
```

**Scripts: Delegar permissões nos modelos de certificado existentes.**  

![diagrama](media/mim-cm-deploy/image039.png)

```shell
dsacls "CN=Administrator,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CAExchange,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CEPEncryption,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ClientAuth,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CodeSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CrossCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CTLSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DirectoryEmailReplication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainController,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainControllerAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFS,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFSRecovery,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgentOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMKeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOnline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KerberosAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Machine,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=MachineEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OCSPResponseSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OfflineRouter,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=RASAndIASServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardLogon,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SubCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=User,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=UserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=WebServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Workstation,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO
```
