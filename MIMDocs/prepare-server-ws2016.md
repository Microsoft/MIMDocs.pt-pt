---
title: Configurar o Windows Server 2016 para o MIM 2016 SP1 | Documentos da Microsoft
description: Obter os passos e os requisitos mínimos para preparar o Windows Server 2016 para trabalhar com o MIM 2016 SP1.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/26/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: a0fa1e837fd73872043748ee73f19a29d1d1412f
ms.sourcegitcommit: 3b514aba69af203f176b40cdb7c2a51c477c944a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/16/2018
ms.locfileid: "51718330"
---
# <a name="set-up-an-identity-management-server-windows-server-2016"></a>Configurar um servidor de gestão de identidades: Windows Server 2016

> [!div class="step-by-step"]
> [«Preparar um domínio](preparing-domain.md)
> [SQL Server 2016»](prepare-server-sql2016.md)
> 
> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio - **corpdc**
> - Nome de domínio – **contoso**
> - Nome do servidor de serviço de MIM - **corpservice**
> - Nome do servidor de sincronização de MIM - **corpsync**
> - Nome do SQL Server - **corpsql**
> - Palavra-passe – <strong>Pass@word1</strong>

## <a name="join-windows-server-2016-to-your-domain"></a>Associar o Windows Server 2016 ao seu domínio

Comece com um computador Windows Server 2016, com um mínimo de 8 a 12GB de RAM. Quando instalar, especifique a edição de "Windows Server 2016 Standard/Datacenter (servidor com uma GUI) x64".

1. Inicie sessão no novo computador como o seu administrador.

2. Através do Painel de Controlo, atribua ao computador um endereço IP estático na rede. Configure essa interface de rede para enviar consultas DNS para o endereço IP do controlador de domínio no passo anterior e defina o nome do computador **CORPSERVICE**.  Esta operação requer um reinício do servidor.

3. Abra o painel de controlo e associe o computador ao domínio que configurou no passo anterior, *contoso.com*.  Esta operação inclui fornecer o nome de utilizador e as credenciais de um administrador de domínio, tal como *contoso\administrador*.  Depois de aparecer a mensagem de boas-vindas, feche a caixa de diálogo e reinicie este servidor novamente.

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
> Dependendo da sua configuração, server(all-in-one) único ou servidores distribuídos, que só precisa de adicionar, com base na função da máquina membro, como o servidor de sincronização. 

1. Iniciar o programa Política de Segurança Local

2. Navegue até **Políticas Locais > Atribuição de Direitos de Utilizadores**.

3. No painel de detalhes, faça duplo clique em **iniciar sessão como um serviço**e selecione **propriedades**.

    ![Imagem da Política de Segurança Local](media/MIM-DeployWS3.png)

4. Clique em **adicionar utilizador ou grupo**, e na caixa de texto a seguir de tipo com base na função `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`, clique em **verificar nomes**e clique em **OK**.

5. Clique em **OK** para fechar a janela de **Propriedades Iniciar sessão como um serviço**.

6.  No painel de detalhes, faça duplo clique em **negar o acesso a este computador a partir da rede**e selecione **propriedades**. >

[!NOTE] Separação dos servidores de função irá interromper a algumas funcionalidades, como o SSPR.

7. Clique em **Adicionar Utilizador ou Grupo**, no tipo de caixa de texto, escreva `contoso\MIMSync; contoso\MIMService` e clique em **OK**.

8. Clique em **OK** para fechar a janela de **Propriedades Negar acesso a este computador a partir da rede**.

9. No painel de detalhes, faça duplo clique em **Recusar início de sessão localmente**e selecione **propriedades**.

10. Clique em **Adicionar Utilizador ou Grupo**, no tipo de caixa de texto, escreva `contoso\MIMSync; contoso\MIMService` e clique em **OK**.

11. Clique em **OK** para fechar a janela de **Propriedades Recusar início de sessão local**.

12. Feche a janela Política de Segurança Local.


## <a name="change-the-iis-windows-authentication-mode-if-needed"></a>Alterar o modo de autenticação do Windows do IIS, se necessário

1.  Abra uma janela do PowerShell.

2.  Pare o IIS com o comando *iisreset /STOP*.

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

> [!div class="step-by-step"]  
> [«Preparar um domínio](preparing-domain.md)
> [SQL Server 2016»](prepare-server-sql2016.md)
