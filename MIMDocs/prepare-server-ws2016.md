---
title: Configurar o Windows Server 2016 para o MIM 2016 SP1 | Microsoft Docs
description: Obter os passos e requisitos mínimos para preparar o Windows Server 2016 para trabalhar com o MIM 2016 SP1.
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 04/26/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 7c77ed0ceb541b9b00ebb9954ce65a53f0f44442
ms.sourcegitcommit: 32d9a963a4487a8649210745c97a3254645e8744
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/27/2018
---
# <a name="set-up-an-identity-management-servers-windows-server-2016"></a>Configurar servidores de gestão de identidades: Windows Server 2016

>[!div class="step-by-step"]
[«Preparar um domínio](preparing-domain.md)
[SQL Server 2016»](prepare-server-sql2016.md)

> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio - **corpdc**
> - Nome de domínio – **contoso**
> - Nome do servidor do serviço MIM - **corpservice**
> - Nome do servidor de sincronização de MIM - **corpsync**
> - Nome do SQL Server - **corpsql**
> - Palavra-passe – **Pass@word1**

## <a name="join-windows-server-2016-to-your-domain"></a>Associar o Windows Server 2016 ao seu domínio

Começar com uma máquina de Windows Server 2016, com um mínimo de 8-12GB de RAM. Quando instalar, especifique a edição "Windows Server 2016 Padrão/Centro de dados (servidor com uma GUI) x64".

1. Inicie sessão no novo computador como o seu administrador.

2. Através do Painel de Controlo, atribua ao computador um endereço IP estático na rede. Configure essa interface de rede para enviar consultas DNS para o endereço IP do controlador de domínio no passo anterior e defina o nome do computador **CORPSERVICE**.  Esta ação exige reiniciar o servidor.

3. Abra o painel de controlo e associe o computador ao domínio que configurou no passo anterior, *contoso.com*.  Isto inclui fornecer o nome de utilizador e as credenciais de um administrador do domínio, tal como *Contoso\Administrador*.  Depois de aparecer a mensagem de boas-vindas, feche a caixa de diálogo e reinicie este servidor novamente.

4. Inicie sessão computador *CORPSERVICE* como uma conta de domínio com o administrador do computador local, tal como *Contoso\MIMINSTALL*.


5. Inicie uma janela do PowerShell como administrador e escreva o seguinte comando para atualizar o computador com as definições da política de grupos.

    ```
    gpupdate /force /target:computer
    ```

    Após menos de um minuto, a atualização estará concluída com a mensagem “A atualização da Política de Computador foi concluída com êxito”.

6. Adicione as funções **Servidor Web (IIS)** e **Servidor de Aplicações**, as funcionalidades do **.NET Framework** 3.5, 4.0 e 4.5 e o **Módulo Active Directory para Windows PowerShell** .

    ![Imagem das funcionalidades do PowerShell](media/MIM-DeployWS2.png)

7. No PowerShell, escreva os seguintes comandos. Tenha em atenção que poderá ser necessário especificar uma localização diferente para os ficheiros de origem das funcionalidades do **.NET Framework** 3.5. Normalmente, estas funcionalidades não estão presentes quando o Windows Server é instalado, mas estão disponíveis na pasta lado a lado (SxS) na pasta de origens do disco de instalação do SO, por exemplo, “*d:\Fontes\SxS\*”.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## <a name="configure-the-server-security-policy"></a>Configurar a política de segurança do servidor

Configure a política de segurança do servidor para permitir que as contas recentemente criadas sejam executadas como serviços.
> [!NOTE] 
> Consoante a configuração server(all-in-one) única ou de servidor distribuído, que só tem de adicionar com base na função da máquina membro como servidor de sincronização. 

1. Iniciar o programa Política de Segurança Local

2. Navegue até **Políticas Locais > Atribuição de Direitos de Utilizadores**.

3. No painel de detalhes, clique com o botão direito do rato em **Iniciar sessão como um serviço** e selecione **Propriedades**.

    ![Imagem da Política de Segurança Local](media/MIM-DeployWS3.png)

4. Clique em **adicionar utilizador ou grupo**, e na caixa de texto seguinte do tipo com base na função `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`, clique em **verificar nomes**e clique em **OK**.

5. Clique em **OK** para fechar a janela de **Propriedades Iniciar sessão como um serviço**.

6.  No painel de detalhes, clique em **negar o acesso a este computador a partir da rede**e selecione **propriedades**. >

[!NOTE] Se os servidores de função separada este passo irá interromper a algumas funtionality como o SSPR funcionalidade.

7. Clique em **Adicionar Utilizador ou Grupo**, no tipo de caixa de texto, escreva `contoso\MIMSync; contoso\MIMService` e clique em **OK**.

8. Clique em **OK** para fechar a janela de **Propriedades Negar acesso a este computador a partir da rede**.

9. No painel de detalhes, clique com o botão direito do rato em **Recusar início de sessão local** e selecione **Propriedades**.

10. Clique em **Adicionar Utilizador ou Grupo**, no tipo de caixa de texto, escreva `contoso\MIMSync; contoso\MIMService` e clique em **OK**.

11. Clique em **OK** para fechar a janela de **Propriedades Recusar início de sessão local**.

12. Feche a janela Política de Segurança Local.


## <a name="change-the-iis-windows-authentication-mode-if-needed"></a>Alterar o modo de autenticação do Windows do IIS, se for necessário

1.  Abra uma janela do PowerShell.

2.  Pare o IIS com o comando *iisreset /STOP*.

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

>[!div class="step-by-step"]  
[«Preparar um domínio](preparing-domain.md)
[SQL Server 2016»](prepare-server-sql2016.md)
