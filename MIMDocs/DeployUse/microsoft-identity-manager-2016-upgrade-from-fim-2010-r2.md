---
title: "Atualizar do FIM 2010 R2 | Documentos da Microsoft"
description: "Saiba como atualizar os componentes do FIM 2010 R2 e, em seguida, instale os componentes novos no MIM 2016."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: ef272ffe665aa5753fa2bf02d26f2ce73efa025b


---

# <a name="upgrade-from-forefront-identity-manager-2010-r2"></a>Atualização do Forefront Identity Manager 2010 R2

Se tiver um ambiente do Forefront Identity Manager (FIM) 2010 R2 e pretender experimentar o Microsoft Identity Manager (MIM) 2016, utilize este artigo como guia. Existem três fases nesta atualização:

1.  Instalar o Serviço de Sincronização do MIM (Sincronização) num servidor que está associado ao domínio do Active Directory (AD). Esta ação substitui a instância do FIM 2010 R2 de Sincronização.

2.  Instale o Serviço e Portal do MIM. Nesta altura, também pode optar por instalar o portal de registo de reposição de palavra-passe (SSPR) de gestão personalizada e o portal de serviço. e, excluindo o conjunto de funcionalidades do Privileged Access Management, será então instalado.

3.  Implemente extensões e suplementos do MIM num computador cliente separado. Isto inclui o cliente integrado de início de sessão do Windows SSPR.


Este guia pressupõe que já configurou o seguinte:
- FIM 2010 R2 implementado num ambiente de teste
- Servidores que executam o Windows Server 2012, o Windows Server 2012 R2 ou o Windows Server 2008 R2
- Pré-requisitos locais e ambientais (SQL Server, Exchange Server, SharePoint Services, etc.) que estão configurados para o FIM 2010 R2.


## <a name="preparation"></a>Preparação

1.  Efetue uma cópia de segurança da base de dados do Serviço FIM, da base de dados da Sincronização do FIM, da Sincronização do FIM e do software e configuração do serviço.

2.  Em cada servidor onde os componentes do FIM 2010 R2 estejam instalados, por exemplo, *CORPIDM*, inicie sessão como Contoso\Administrador. Neste exemplo de implementação, os Direitos administrativos são necessários para atualizar o FIM 2010 R2 para o **MIM**.

3.  Transfira ou descompacte o software MIM.

## <a name="upgrade-the-synchronization-service"></a>Atualizar o Serviço de Sincronização

1.  Inicie sessão como um administrador num servidor em que o Serviço de Sincronização do FIM 2010 R2 (“Sincronizar”) esteja implementado.

2.  Certifique-se de que efetua uma cópia de segurança da base de dados antes de iniciar este procedimento.

3.  Abra a consola de **Serviços**, localize o **Serviço de Sincronização do Forefront Identity Manager** e pare-o.

    ![Imagem da consola de Serviços](media/MIM-UpgFIM1.PNG)

4.  Execute o **instalador do Serviço de Sincronização do MIM**. O instalador irá detetar a versão da Sincronização existente e sugerir uma atualização. Clique no botão **Atualizar** para continuar.

5.  Se aceitar os termos de licenciamento, clique em **Seguinte** para continuar.

6.  Introduza a palavra-passe da conta de serviço que a Sincronização está a utilizar e clique em **Seguinte**.

    ![Imagem da configuração da conta de serviço da Sincronização do MIM](media/MIM-UpgFIM3.png)

7.  Confirme se os nomes dos grupos de segurança estão corretos e clique em **Seguinte**.

    ![Imagem da configuração dos grupos de segurança da Sincronização do MIM](media/MIM-UpgFIM4.png)

8.  Não altere a caixa de verificação das regras de firewall para comunicações de RPC de entrada.

9. O instalador está pronto para atualizar a Sincronização do FIM 2010 R2 para o MIM. Clique em **Atualizar** para iniciar o processo de atualização.

10. A atualização está agora em curso. Não saia do instalador nem reinicie o computador enquanto a atualização está em curso.

    ![Imagem de estado da instalação da Sincronização do MIM](media/MIM-UpgFIM7.png)

11. Durante a atualização, é apresentado um aviso sobre a atualização da base de dados da Sincronização. Recomenda-se que efetue uma cópia de segurança da base de dados antes de começar.

12. Quando a atualização for concluída com êxito, clique em **Concluir**.

    ![Imagem de êxito da instalação da Sincronização do MIM](media/MIM-UpgSP1.png)

13. Tenha em atenção que o **Serviço de Sincronização** reiniciou.

## <a name="upgrade-the-service-and-portal"></a>Atualizar o Serviço e o Portal

1.  Inicie sessão como um administrador num servidor em que o Portal e o Serviço do FIM 2010 R2 estejam implementados.

2.  Abra a consola de **Serviços**, localize o **Serviço de Forefront Identity Manager** e pare-o.

    ![Imagem da consola de Serviços](media/MIM-UpgFIM9.PNG)

3.  Execute o instalador do Portal e do Serviço MIM. Clique no botão **Instalar** para continuar.

    ![Imagem da instalação do Portal e do Serviço MIM](media/MIM-UpgSP2.png)

4.  Se aceitar os termos de licenciamento, clique em **Seguinte** para continuar.

5.  No ecrã Programa de Melhoramento da Experiência do Cliente do MIM, clique em **Seguinte** para continuar.

6.  Selecione as funcionalidades e os componentes do MIM que pretende instalar e clique em **Seguinte** quando terminar.

    ![Imagem da Configuração Personalizada](media/MIM-UpgSP4.png)

    1.  **Serviço MIM:** esta funcionalidade é obrigatória em, pelo menos, um servidor e necessita de um servidor da base de dados do SQL Server colocalizado ou noutro servidor.

    2.  **Portal do MIM:** esta funcionalidade é obrigatória em, pelo menos, um servidor e necessita do SharePoint 2013 Foundation.

    3.  **Portal de Registo de Palavras-passe do MIM:** esta funcionalidade é necessária para a reposição personalizada de palavra-passe.

    4.  **Portal de Reposição da Palavra-passe do MIM:** esta funcionalidade é necessária para a reposição da palavra-passe.

7.  Forneça detalhes do SQL Server que está a ser utilizado para a base de dados do Serviço FIM. Selecione a opção para utilizar novamente a base de dados existente e manter os dados. Clique em **Seguinte** para continuar.

8. Se a opção para utilizar novamente a base de dados existente tiver sido selecionada, é apresentado um lembrete para efetuar uma cópia de segurança da base de dados.

9. Introduza os detalhes do servidor de correio. Se o servidor de correio estiver localizado no servidor atual, introduza “localhost” como a localização do servidor de correio. Clique em **Seguinte** para continuar.

    ![Imagem da configuração da ligação ao servidor de correio](media/MIM-UpgSP6.png)

10. Selecione um certificado para o Serviço a utilizar para validar os clientes. Deve utilizar o certificado existente do arquivo de certificados local que foi utilizado anteriormente pelo Serviço FIM.

    ![Imagem de configuração do certificado de serviço](media/MIM-UpgSP7.png)

    1.  Se a opção de arquivo de certificados local estiver selecionada, clique no botão **Selecionar Cert** e selecione um certificado na lista na janela de pop-up. Cliquem em **OK** e em **Seguinte**.

        ![Imagem da janela de pop-up Selecionar um Certificado](media/MIM-UpgSP8.PNG)

11. Configure as credenciais da Conta de Serviço do Serviço MIM. Tenha em atenção que a conta de serviço não pode ser a mesma conta de serviço utilizada pelo serviço de Sincronização. Deve ser a mesma conta que foi utilizada pelo Serviço FIM.

    ![Imagem da configuração da conta de serviço MIM](media/MIM-UpgSP9.png)

12. Configure os detalhes do Servidor de Sincronização do MIM de acordo com a implementação do Serviço MIM configurado num passo anterior.

    ![Imagem da configuração da sincronização do Portal e do Serviço MIM](media/MIM-UpgSP10.png)

13. Ao instalar o Portal do MIM, forneça o endereço do servidor do Serviço MIM. Clique em **Seguinte**.

14. Ao instalar o Portal do MIM, forneça o URL da coleção de sites do SharePoint no qual o Portal do FIM está alojado atualmente. Clique em **Seguinte**.

## <a name="install-the-mim-password-registration-portal"></a>Instalar o Portal de Registo de Palavras-passe do MIM

1. Se estiver a instalar o Portal de Registo de Palavras-passe do MIM, forneça o URL solicitado para o Portal de Registo de Palavras-passe. Clique em **Seguinte**.

2. Configure a capacidade para os clientes e os utilizadores finais utilizarem o Serviço e o Portal.

    1.  Verifique se pretende **Abrir as portas 5725 e 5726 na firewall**.

    2.  Verifique se pretende **Conceder aos utilizadores autenticados o acesso ao site do Portal do MIM**.

    3.  Clique em **Seguinte**.

3. Forneça os detalhes de acesso e as credenciais do Registo de Palavras-passe do MIM.

    ![Imagem da configuração do Portal de Registo de Palavras-passe do MIM](media/MIM-UpgSP15.png)

    1.  Forneça o nome da conta de serviço (incluindo o domínio) e a palavra-passe do Registo de Palavras-passe do MIM.

    2.  Forneça os detalhes do anfitrião, o nome e a porta (por exemplo, 8080), do Portal de Registo de Palavras-passe.

    3.  Ative a opção **abrir porta na firewall**.

    4.  Clique em **Seguinte**.

4. No seguinte ecrã de configuração Registo de Palavras-passe do MIM:

    1.  Indique o Registo de Palavras-passe do MIM onde está instalado o Serviço MIM, normalmente, o mesmo sistema.

    2.  Determine se este portal pode ser acedido pelos utilizadores da extranet e da intranet, ou apenas pelos utilizadores da intranet, como previamente configurado para a reposição da palavra-passe do FIM.

## <a name="install-the-mim-password-reset-portal"></a>Instalar o Portal de Reposição da Palavra-passe do MIM

1. Se estiver a instalar o Portal de Reposição da Palavra-passe do MIM, forneça os detalhes de acesso e as credenciais para a Reposição da Palavra-passe do MIM.

    ![Configurar a imagem do Portal de Reposição da Palavra-passe do MIM](media/MIM-UpgSP17.png)

    1.  Forneça o nome da conta de serviço (incluindo o domínio) e a palavra-passe da Reposição da Palavra-passe do MIM.

    2.  Forneça os detalhes do anfitrião, o nome e a porta (por exemplo, 8088), do Portal de Reposição da Palavra-passe.

    3.  Ative a opção **abrir porta na firewall**.

    4.  Clique em **Seguinte**.

2. No seguinte ecrã de configuração Reposição da Palavra-passe do MIM:

    1.  Indique a Reposição da Palavra-passe do MIM onde está instalado o Serviço MIM.

    2.  Especifique se este portal pode ser acedido pelos utilizadores da extranet e da intranet ou apenas pelos utilizadores da intranet.

## <a name="finish-installation-and-upgrade"></a>Concluir a instalação e a atualização

1. Depois de concluídas todas as definições de configuração com êxito, será apresentada uma página de instalação. Clique em **Instalar** para iniciar a instalação e a atualização do Portal e do Serviço MIM.

2. A instalação e a atualização do Portal e do Serviço MIM estão agora em curso. Não cancele o instalador nem reinicie o computador durante a instalação.

3. Depois de a instalação (atualização) do Portal e do Serviço MIM estar concluída com êxito, é apresentado um ecrã de confirmação. Clique em **Concluir** para concluir a instalação e sair do instalador.

4. Tenha em atenção que o **Serviço de Forefront Identity Manager** reiniciou.

Nota: se, atualmente, os Suplementos e as Extensões do FIM estiverem implementados nos computadores do utilizador para SSPR, não configure as novas portas do telefone MFA para a reposição da palavra-passe antes da atualização de todos os Suplementos e as Extensões do FIM para o MIM 2016.  Uma vez que os Suplementos e as Extensões do FIM 2010 e do FIM 2010 R2 não reconhecem as novas portas, tal resultará num erro e o utilizador não poderá concluir a reposição da palavra-passe.



<!--HONumber=Nov16_HO2-->


