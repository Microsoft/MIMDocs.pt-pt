---
title: Implementar o PAM passo 4 – Instalar o MIM | Documentos da Microsoft
description: Instalar e configurar o Serviço MIM e o Portal no servidor Privileged Access Management e nas estações de trabalho.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 02/09/2021
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ef605496-7ed7-40f4-9475-5e4db4857b4f
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8f81f592beff9ab952d1a42760e06a4f622316e8
ms.sourcegitcommit: 0e2b4b47a8050737c78e3b0ad088358e5de7e929
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2021
ms.locfileid: "100395474"
---
# <a name="step-4--install-mim-components-on-pam-server-and-workstation"></a>Passo 4 – Instalar os componentes do MIM num servidor e estação de trabalho de PAM

> [!div class="step-by-step"]
> [« Passo 3](step-3-prepare-pam-server.md) 
>  [Passo 5 »](step-5-establish-trust-between-priv-corp-forests.md)

No PAMSRV, inicie sessão como PRIV\Administrator para instalar o Portal e o Serviço MIM, bem como a aplicação Web do portal de exemplo.

  > [!NOTE]
  > Tem de ser um administrador de domínio; se não estiver a executar os seguintes comandos como administrador de domínio, as verificações de validação de confiança no próximo passo não serão concluídas.

Se tiver transferido o MIM, descompacte o arquivo de instalação para uma nova pasta.

## <a name="run-the-service-and-portal-install-program"></a>Execute o programa de instalação do Portal e do Serviço.

Siga as diretrizes do instalador e conclua a instalação.

1. Quando selecionar as funcionalidades dos componentes, inclua o Serviço MIM (com Gestão de Acesso Privilegiado, mas não para os Relatórios MIM) e o Portal do MIM  

   ![Configuração personalizada - captura de ecrã](./media/PAM_GS_MIM_2015_Service_Portal.png)

2. Quando configurar serviços comuns e a ligação da base de dados MIM, especifique **Criar uma nova base de dados**.

   > [!NOTE]
   > Se instalar o Serviço MIM várias vezes para elevada disponibilidade, especifique **Utilizar uma base de dados existente** para todas as instalações subsequentes.

3. Quando configurar uma ligação de servidor de correio, defina o servidor de correio para o nome de anfitrião de um servidor do Exchange ou SMTP para o ambiente CORP (utilize corpdc.contoso.local se não tiver um servidor de correio) e desmarque as caixas de verificação **Utilizar SSL** e **O Servidor de Correio é Exchange Server 2007 ou Exchange Server 2010**.

4. Escolha esta opção para gerar um novo certificado autoassinado.

5. Defina as seguintes credenciais de conta:
   - Nome da Conta de Serviço: *MIMService*  
   - Senha de conta de serviço: <em>Pass@word1</em> (ou a palavra-passe que criou no Passo 2)  
   - Domínio da Conta de Serviço: *PRIV*  
   - Conta de e-mail de serviço: <em>MIMService@priv.contoso.local</em>  

6. Aceite as predefinições para o nome de anfitrião do servidor de sincronização e especifique a conta do Agente de Gestão do MIM como *PRIV\MIMMA*. Será apresentada uma mensagem de aviso a indicar que o serviço de sincronização do MIM não existe. Este aviso é OK, uma vez que o serviço de sincronização MIM não é utilizado neste cenário.

7. Defina *PAMSRV* como endereço do servidor do Serviço MIM.

8. Definido `http://pamsrv.priv.contoso.local:82` como URL de coleção de site SharePoint.

9. Deixe o URL do portal de registo em branco.

10. Selecione a caixa de verificação para abrir as portas 5725 e 5726 na firewall e a caixa de verificação para conceder acesso ao site do portal do MIM a todos os utilizadores autenticados.

11. Deixe o nome de anfitrião da API REST de PAM em branco e defina *8086* como o número da porta.

    ![Informações de Enlace para a API REST de PAM - captura de ecrã](./media/PAM_GS_MIM_2015_Service_Portal_configure_application_pool.png)

12. Configure a conta da API REST de PAM do MIM para utilizar a mesma conta do SharePoint (uma vez que o Portal do MIM está colocalizado neste servidor):
    - Nome da Conta do Conjunto Aplicacional: *SharePoint*  
    - Password de conta de grupo de aplicação: <em>Pass@word1</em> (ou a palavra-passe que criou no Passo 2)  
    - Domínio da Conta do Conjunto Aplicacional: *PRIV*  

    ![Credenciais da conta do conjunto aplicacional - captura de ecrã](./media/PAM_GS_Configure_Component_Service.png)

    Pode aparecer um aviso a indicar que a Conta de Serviço não está protegida na configuração atual. Não há problema.

13. Configure o serviço do componente PAM do MIM:
    - Nome da Conta de Serviço – *MIMComponent*
    - Senha de conta de serviço: <em>Pass@word1</em> (ou a palavra-passe que criou no Passo 2)  
    - Domínio da Conta de Serviço: *PRIV*

    ![Credenciais da conta de serviço do Componente PAM - captura de ecrã](./media/PAM_GS_Configure_MIM_PAM_component_service.png)

14. Configure o Serviço de Monitorização PAM:
    - Nome da Conta de Serviço: *MIMMonitor*  
    - Senha de conta de serviço: <em>Pass@word1</em> (ou a palavra-passe que criou no Passo 2)  
    - Domínio da Conta de Serviço: *PRIV*  

    ![Credenciais da conta do Serviço de Monitorização PAM - captura de ecrã](./media/PAM_GS_Configur_PAM_Monitoring_service.png)

15. Na página Introduzir informações para Portais de Palavras-passe do MIM, deixe as caixas de verificação em branco e continue. Clique em **Seguinte** para continuar a instalação.

Depois de concluída a instalação, o servidor será reiniciado, em seguida certifique-se de que o Portal do MIM está ativo e permita que os utilizadores vejam os seus próprios recursos de objetos no MIM.

## <a name="set-up-mim-portal-management-policy-rules"></a>Configurar regras de política de gestão do Portal do MIM

1. Depois de PAMSRV ser reiniciado, inicie sessão como PRIV\Administrator.

2. Inicie o Internet Explorer e ligue-se ao Portal MIM em `http://pamsrv.priv.contoso.local:82/identitymanagement` . Pode existir um pequeno atraso quando esta página for localizada pela primeira vez.

3. Se necessário, inicie sessão como PRIV\Administrator no Internet Explorer.

4. No Internet Explorer, abra as **Opções de Internet,** altere o separador **Segurança** e adicione o site à **zona intranet local** se ainda não estiver lá. Feche a caixa de diálogo Opções da Internet.

5. Através do Internet Explorer, para ver o Portal do MIM, clique em **Regras de Política de Gestão**.

6. Procurar a regra da política de gestão **Gestão Gestão do utilizador: Os utilizadores podem ler os seus próprios atributos**.

7. Selecione esta regra de política de gestão, desmarque **A política está desativada**, clique em **OK** e, em seguida, clique em **Submeter**.

## <a name="verify-the-firewall-connections"></a>Verificar as ligações de firewall

A firewall deve permitir ligações de entrada para a porta TCP 5725, 5726, 8086 e 8090.

1.  Inicie **Firewall do Windows com Segurança Avançada** (localizado em Ferramentas Administrativas).  
2.  Clique em **Regras de Entrada**.  
3.  Verifique se estas duas regras estão listadas:  
    - Serviço de Forefront Identity Manager (STS)
    - Serviço de Forefront Identity Manager (Serviço Web)  
4.  Clique **em Nova regra**  >  **Port**  >  **TCP,** e digite as portas locais específicas *8086* e *8090*. Clique no assistente para aceitar as predefinições, dê um nome à regra e clique em **Concluir**.  
5.  Depois de concluir o assistente, feche a aplicação Firewall do Windows.

6.  Inicie o **Painel de Controlo**.  
7.  Em Rede e Internet, selecione **Ver o estado da rede e as tarefas**.
8.  Verifique se existe uma rede ativa, listada como sendo priv.contoso.local, e uma rede de domínio.
9. Painel **de controlo de perto**.

## <a name="optional-set-up-the-sample-web-application"></a>Opcional: Configurar a aplicação web da amostra

Nesta secção, pode instalar e configurar a aplicação web da amostra para a API MIM PAM REST.  Este componente só é necessário se desejar aprender a utilizar a API MIM PAM REST. Se pretender utilizar o PowerShell para solicitar e aprovar o acesso, continue com a secção seguinte para instalar os cmdlets do requestor MIM PAM.

1. A partir do arquivo da aplicação Web de exemplo, transfira os [exemplos de Gestão de Identidades](https://github.com/Azure/identity-management-samples) como um ficheiro zip.

2. Descompacte o conteúdo da pasta **identity-management-samples-master\Privileged-Access-Management-Portal\src** para uma nova pasta **C:\Programas\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal**.

3. Crie o novo site no IIS com um nome de site do Portal de Exemplo da Gestão de Acesso Privilegiado do MIM, caminho físico C:\Programas\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal e porta 8090.  Esta criação do site pode ser feita usando o seguinte comando PowerShell:

   ```PowerShell
   New-WebSite -Name "MIM Privileged Access Management Example Portal" -Port 8090   -PhysicalPath "C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\"
   ```

4. Configure a aplicação Web de exemplo para conseguir redirecionar os utilizadores para a API REST de PAM do MIM. Utilizando um editor de texto como o Notepad, edite o ficheiro **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management REST API\web.config**. Na secção **<system.webServer>,** adicione as seguintes linhas:

   ```XML
   <httpProtocol>
   <customHeaders>
     <add name="Access-Control-Allow-Credentials" value="true"  />
     <add name="Access-Control-Allow-Headers" value="content-type" />
     <add name="Access-Control-Allow-Origin" value="http://pamsrv:8090" />  
   </customHeaders>
   </httpProtocol>
   ```

5. Configure a aplicação Web de exemplo. Utilizando um editor de texto, como o Bloco de Notas, edite o ficheiro **C:\Programas\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\js\utils.js**. Desagure o valor do **pamRespApiUrl** para `http://pamsrv.priv.contoso.local:8086/api/pamresources/` .

6. Reinicie o IIS com o seguinte comando para estas alterações serem aplicadas.

   ```cmd
   iisreset
   ```

7. (Opcional) Verifique se o utilizador pode autenticar para a API REST. Abra um browser como administrador em PAMSRV.  Navegue para o URL do `http://pamsrv.priv.contoso.local:8086/api/pamresources/pamroles/` site, autense se necessário, e certifique-se de que ocorre um download.

## <a name="install-the-mim-pam-requestor-cmdlets"></a>Instalar os cmdlets do requerente de PAM do MIM

Instale os cmdlets do requerente de PAM do MIM na estação de trabalho configurada no Passo 1.

1.  Inicie sessão em CORPWKSTN como administrador.

2.  Transfira os **Suplementos e extensões** para o computador CORPWKSTN, se ainda não existirem.

3.  Descompacte a pasta **Suplementos e extensões** do arquivo para uma nova pasta.

4.  Execute o instalador **setup.exe**.

5.  Na configuração personalizada, especifique onde o **Cliente PAM** será instalado, mas não o **Suplemento do MIM para o Outlook** ou as **Extensões de Autenticação e Palavra-passe do MIM**.

6.  No endereço DO Servidor PAM, especifique como nome de anfitrião do servidor PRIV MIM `pamsrv.priv.contoso.local` .

Depois de concluída a instalação, reinicie CORPWKSTN para concluir o registo do novo módulo do PowerShell.

No próximo passo, irá estabelecer confiança entre florestas PRIV e CORP.

> [!div class="step-by-step"]
> [« Passo 3](step-3-prepare-pam-server.md) 
>  [Passo 5 »](step-5-establish-trust-between-priv-corp-forests.md)
