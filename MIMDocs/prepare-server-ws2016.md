---
title: Configure o Windows Server 2016 ou 2019 para MIM 2016 SP2 ; Microsoft Docs
description: Obtenha os passos e requisitos mínimos para preparar o Windows Server 2016 ou 2019 para trabalhar com o MIM 2016 SP2.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 10/18/2019
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: cf8261c4e6f6529fd82760206b62b689a75d0acb
ms.sourcegitcommit: 78c2d7e5ba4bec276d5a9bf8860bc126d9bd9c33
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/11/2020
ms.locfileid: "94492417"
---
# <a name="set-up-an-identity-management-server-windows-server-2016-or-2019"></a>Configurar um servidor de gestão de identidade: Windows Server 2016 ou 2019

> [!div class="step-by-step"]
> [« Preparar um domínio](preparing-domain.md) 
>  [SQL Server »](prepare-server-sql2016.md)
> 

> [!NOTE]
> O procedimento de configuração do Windows Server 2019 não difere do procedimento de configuração do Windows Server 2016.


> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio - **corpdc**
> - Nome de domínio – **contoso**
> - Nome do Servidor de Serviço MIM - **corpservice**
> - Nome do Servidor MIM Sync - **corpsync**
> - Nome do Servidor SQL - **corpsql**
> - Senha - <strong>Pass@word1</strong>

## <a name="join-windows-server-2016-to-your-domain"></a>Junte-se ao Windows Server 2016 ao seu domínio

Comece com uma máquina Windows Server 2016, com um mínimo de 8-12GB de RAM. Ao instalar especificar a edição "Windows Server 2016 Standard/Datacenter (Servidor com gui) x64".

1. Inicie sessão no novo computador como o seu administrador.

2. Através do Painel de Controlo, atribua ao computador um endereço IP estático na rede. Configure essa interface de rede para enviar consultas de DNS para o endereço IP do controlador de domínio no passo anterior, e definir o nome do computador para **CORPSERVICE**.  Esta operação requer um reinício do servidor.

3. Abra o Painel de Controlo e junte o computador ao domínio que configura no último passo, *contoso.com*.  Esta operação inclui o nome de utilizador e credenciais de um administrador de domínio como *Contoso\Administrator*.  Depois de aparecer a mensagem de boas-vindas, feche a caixa de diálogo e reinicie este servidor novamente.

4. Inscreva-se no computador *CORPSERVICE* como uma conta de domínio com o administrador de máquinas local, como *Contoso\MIMINSTALL*.


5. Inicie uma janela do PowerShell como administrador e escreva o seguinte comando para atualizar o computador com as definições da política de grupos.

    ```
    gpupdate /force /target:computer
    ```

    Após menos de um minuto, a atualização estará concluída com a mensagem “A atualização da Política de Computador foi concluída com êxito”.

6. Adicione as funções **Servidor Web (IIS)** e **Servidor de Aplicações** , as funcionalidades do **.NET Framework** 3.5, 4.0 e 4.5 e o **Módulo Active Directory para Windows PowerShell**.

    ![Imagem das funcionalidades do PowerShell](media/MIM-DeployWS2.png)

7. No PowerShell, escreva os seguintes comandos. Tenha em atenção que poderá ser necessário especificar uma localização diferente para os ficheiros de origem das funcionalidades do **.NET Framework** 3.5. Normalmente, estas funcionalidades não estão presentes quando o Windows Server é instalado, mas estão disponíveis na pasta lado a lado (SxS) na pasta de origens do disco de instalação do SO, por exemplo, “*d:\Fontes\SxS\*”.

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## <a name="configure-the-server-security-policy"></a>Configurar a política de segurança do servidor

Configure a política de segurança do servidor para permitir que as contas recentemente criadas sejam executadas como serviços.
> [!NOTE] 
> Dependendo da configuração, servidor único (tudo-em-um) ou servidores distribuídos que só precisa de adicionar, com base no papel da máquina membro, como o servidor de sincronização. 

1. Iniciar o programa Política de Segurança Local

2. Navegue até **Políticas Locais > Atribuição de Direitos de Utilizadores**.

3. No painel de detalhes, clique com o botão direito **no Log on como um serviço,** e selecione **Propriedades**.

    ![Imagem da Política de Segurança Local](media/MIM-DeployWS3.png)

4. Clique em **Adicionar Utilizador ou Grupo** , e no tipo de caixa de texto seguindo com base na função, clique em Verificar `contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR` **Nomes** e clique **em OK**.

5. Clique em **OK** para fechar a janela de **Propriedades Iniciar sessão como um serviço**.

6.  No painel de detalhes, clique à direita no **Acesso a Negar a este computador a partir da rede** , e selecione **Properties**.>

7. Clique em **Adicionar Utilizador ou Grupo** , no tipo de caixa de texto, escreva `contoso\MIMSync; contoso\MIMService` e clique em **OK**.

8. Clique em **OK** para fechar a janela de **Propriedades Negar acesso a este computador a partir da rede**.

9. No painel de detalhes, clique à direita no **registo deny localmente,** e selecione **Propriedades**.

10. Clique em **Adicionar Utilizador ou Grupo** , no tipo de caixa de texto, escreva `contoso\MIMSync; contoso\MIMService` e clique em **OK**.

11. Clique em **OK** para fechar a janela de **Propriedades Recusar início de sessão local**.

12. Feche a janela Política de Segurança Local.

## <a name="software-prerequisites"></a>Pré-requisitos de software

Antes de instalar os componentes MIM 2016 SP2, certifique-se de que instala todos os pré-requisitos do software:

13. Instalar [Pacotes Redistribuíveis Visual C++ 2013](https://www.microsoft.com/download/details.aspx?id=40784).

14. Instale o quadro .NET 4.6.

15. No servidor que irá acolher o Serviço de Sincronização MIM, o Serviço de Sincronização MIM requer [cliente nativo do servidor SQL](https://www.microsoft.com/download/details.aspx?id=50402).

16. No servidor que irá acolher o Serviço MIM, o Serviço MIM requer .NET Framework 3.5.

17. Opcionalmente, se utilizar o modo TLS 1.2 ou FIPS, consulte [MIM 2016 SP2 em ambientes "TLS 1.2 apenas" ou modo FIPS](preparing-tls.md).

## <a name="change-the-iis-windows-authentication-mode-if-needed"></a>Alterar o modo de autenticação do IIS Windows, se necessário

1.  Abra uma janela do PowerShell.

2.  Pare o IIS com o comando *iisreset /STOP*.

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

> [!div class="step-by-step"]  
> [« Preparar um domínio](preparing-domain.md) 
>  [SQL Server »](prepare-server-sql2016.md)
