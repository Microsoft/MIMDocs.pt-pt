---
title: Configurar o Windows Server 2016 ou 2019 para MIM 2016 SP2 | Microsoft Docs
description: Obtenha as etapas e os requisitos mínimos para preparar o Windows Server 2016 ou 2019 para trabalhar com o MIM 2016 SP2.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/18/2019
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: c6d5d5081f0e932b9c60d8f2025b54e47dc352d5
ms.sourcegitcommit: 323c2748dcc6b6991b1421dd8e3721588247bc17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/04/2019
ms.locfileid: "73568041"
---
# <a name="set-up-an-identity-management-server-windows-server-2016-or-2019"></a>Configurar um servidor de gerenciamento de identidade: Windows Server 2016 ou 2019

> [!div class="step-by-step"]
> [«Preparando um
> de domínio](preparing-domain.md) [SQL Server»](prepare-server-sql2016.md)
> 

> [!NOTE]
> O procedimento de instalação do Windows Server 2019 não difere do procedimento de instalação do Windows Server 2016.


> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio- **corpdc**
> - Nome de domínio – **contoso**
> - Nome do servidor do serviço do MIM- **corpservice**
> - Nome do servidor de sincronização do MIM- **corpsync**
> - Nome do SQL Server- **corpsql**
> - Palavra-passe – <strong>Pass@word1</strong>

## <a name="join-windows-server-2016-to-your-domain"></a>Ingresse o Windows Server 2016 em seu domínio

Comece com um computador com Windows Server 2016, com um mínimo de 8 GB de RAM. Ao instalar, especifique a edição "Windows Server 2016 Standard/datacenter (servidor com uma GUI) x64".

1. Inicie sessão no novo computador como o seu administrador.

2. Através do Painel de Controlo, atribua ao computador um endereço IP estático na rede. Configure essa interface de rede para enviar consultas DNS para o endereço IP do controlador de domínio na etapa anterior e defina o nome do computador como **CORPSERVICE**.  Esta operação exigirá uma reinicialização do servidor.

3. Abra o painel de controle e ingresse o computador no domínio que você configurou na última etapa, *contoso.com*.  Essa operação inclui fornecer o nome de usuário e as credenciais de um administrador de domínio, como *Contoso\Administrator*.  Depois de aparecer a mensagem de boas-vindas, feche a caixa de diálogo e reinicie este servidor novamente.

4. Entre no computador *CORPSERVICE* como uma conta de domínio com administrador do computador local, como *Contoso\MIMINSTALL*.


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
> Dependendo de sua configuração, servidor único (tudo em um) ou servidores distribuídos, você só precisa adicionar, com base na função do computador membro, como o servidor de sincronização. 

1. Iniciar o programa Política de Segurança Local

2. Navegue até **Políticas Locais > Atribuição de Direitos de Utilizadores**.

3. No painel de detalhes, clique com o botão direito do mouse em **fazer logon como um serviço**e selecione **Propriedades**.

    ![Imagem da Política de Segurança Local](media/MIM-DeployWS3.png)

4. Clique em **Adicionar usuário ou grupo**e, na caixa de texto, digite o seguinte com base na `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`de função, clique em **verificar nomes**e clique em **OK**.

5. Clique em **OK** para fechar a janela de **Propriedades Iniciar sessão como um serviço**.

6.  No painel de detalhes, clique com o botão direito do mouse em **negar acesso a este computador da rede**e selecione **Propriedades**. >

7. Clique em **Adicionar Utilizador ou Grupo**, no tipo de caixa de texto, escreva `contoso\MIMSync; contoso\MIMService` e clique em **OK**.

8. Clique em **OK** para fechar a janela de **Propriedades Negar acesso a este computador a partir da rede**.

9. No painel de detalhes, clique com o botão direito do mouse em **Negar logon localmente**e selecione **Propriedades**.

10. Clique em **Adicionar Utilizador ou Grupo**, no tipo de caixa de texto, escreva `contoso\MIMSync; contoso\MIMService` e clique em **OK**.

11. Clique em **OK** para fechar a janela de **Propriedades Recusar início de sessão local**.

12. Feche a janela Política de Segurança Local.

## <a name="software-prerequisites"></a>Pré-requisitos de software

Antes de instalar os componentes do MIM 2016 SP2, certifique-se de instalar todos os pré-requisitos de software:

13. Instale [os C++ pacotes redistribuíveis do Visual 2013](https://www.microsoft.com/download/details.aspx?id=40784).

14. Instale o .NET Framework 4,6.

15. No servidor que hospedará o serviço de sincronização do MIM, o serviço de sincronização do MIM exigirá [SQL Server Native Client](https://www.microsoft.com/download/details.aspx?id=50402).

16. No servidor que hospedará o serviço do MIM, o serviço do MIM exigirá .NET Framework 3,5.

17. Opcionalmente, se estiver usando o modo TLS 1,2 ou FIPS, consulte o [MIM 2016 SP2 em ambientes de modo de "tls 1,2 somente" ou FIPS](preparing-tls.md).

## <a name="change-the-iis-windows-authentication-mode-if-needed"></a>Alterar o modo de autenticação do Windows do IIS, se necessário

1.  Abra uma janela do PowerShell.

2.  Pare o IIS com o comando *iisreset /STOP*.

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

> [!div class="step-by-step"]  
> [«Preparando um
> de domínio](preparing-domain.md) [SQL Server»](prepare-server-sql2016.md)
