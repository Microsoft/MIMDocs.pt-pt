---
title: Instalar o Portal e o Serviço Microsoft Identity Manager | Documentos da Microsoft
description: Obter os passos para configurar e instalar o Portal e o Serviço MIM do Microsoft Identity Manager 2016
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/18/2019
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: markwahl-msft
ms.suite: ems
ms.openlocfilehash: 1f7aa8e257ef4fd1d97ee602a4e0f3f878d8c1b6
ms.sourcegitcommit: 323c2748dcc6b6991b1421dd8e3721588247bc17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/04/2019
ms.locfileid: "73568077"
---
# <a name="install-mim-2016-mim-service-and-portal"></a>Instalar o MIM 2016: Portal e Serviço MIM

> [!div class="step-by-step"]
> [«Serviço de Sincronização do MIM](install-mim-sync.md)
> [Sincronizar bases de dados»](install-mim-sync-ad-service.md)
 
> [!NOTE]
> Estas instruções utilizam valores e nomes de exemplo de uma empresa denominada Contoso. Substitua estas instruções pelas suas. Por exemplo:
> - Nome do controlador de domínio – **nomedoservidormim**
> - Nome de domínio – **contoso**
> - Palavra-passe – <strong>Pass@word1</strong>
> - Nome da conta de serviço – **ServiçoMIM**

Se não tiver configurado o pacote de instalação do MIM no último passo, retroceda e instale os componentes do Microsoft Identity Manager 2016 antes de continuar.


## <a name="configure-mim-service-and-portal-for-installation"></a>Configurar a instalação do Portal e do Serviço MIM

1. Execute o **Instalador do Portal e do Serviço MIM** a partir da subpasta **Serviço e Portal** descompactada.

2. No ecrã de boas-vindas, clique em **Seguinte**.

3. Leia o Contrato de Licença do Utilizador Final e clique em **Seguinte** se aceitar os termos de licenciamento.

4. No ecrã **Programa de Melhoramento da Experiência do Cliente de MIM**, clique em **Seguinte**.

5. Quando selecionar as funcionalidades dos componentes para esta implementação, certifique-se de que inclui o Serviço MIM (exceto para os Relatórios MIM) e as funcionalidades do Portal do MIM. De igual modo, pode selecionar o Portal de Reposição da Palavra-passe do MIM e o Serviço de Notificação de Alteração de Palavra-passe do MIM.

6. Na página **Configurar a ligação à base de dados do MIM**, escolha **Criar uma nova base de dados**.

    ![Imagem da configuração da ligação à base de dados do MIM](media/install-mim-service-portal/MIM_Install10.png)

7. Em **Configurar conexão do servidor de email**, insira o nome do servidor Exchange como **servidor de email** ou use a **caixa de correio do O365**. Se não tiver um servidor de e-mail configurado, utilize **localhost** como o nome do servidor de e-mail e desmarque as duas caixas de verificação superiores. Clique em **Seguinte**.

    ![Imagem da configuração da ligação ao servidor de e-mail](media/install-mim-service-portal/MIM_Install11.png)

8. Especifique que pretende gerar um novo certificado autoassinado ou selecione o certificado relevante.

9. Especifique o nome da Conta de Serviço a utilizar, por exemplo, *ServiçoMIM*, e a palavra-passe da Conta de Serviço, por exemplo, <em>Pass@word1</em>, o domínio da Conta de Serviço, por exemplo, *contoso*, e a Conta de E-mail do Serviço, por exemplo, *contoso*.
    >[!NOTE]
    >MIM 2016 SP2 e posterior: se você estiver usando contas de serviço gerenciado de grupo, será necessário garantir que o caractere **$** esteja no final do nome da conta de serviço, por exemplo, MIMService $, e deixe o campo de senha da conta de serviço vazio.

    ![Imagem da configuração da conta de serviço MIM](media/install-mim-service-portal/MIM_Install12.png)

10. Tenha em atenção que poderá aparecer um aviso a indicar que a Conta de Serviço não está protegida na configuração atual.

11. Aceite os padrões para o local do servidor de sincronização e especifique a conta do agente de gerenciamento do MIM como *contoso\MIMMA*.
    >[!NOTE]
    >MIM 2016 SP2 e posterior: se você planeja usar a conta de serviço gerenciado do grupo de serviço de sincronização do MIM na sincronização do MIM e habilitar o recurso ' usar conta de sincronização do MIM ', insira o nome do serviço de sincronização do MIM gMSA como a conta do MIM MA, por exemplo, *contoso\MIMSync $* .

    ![Imagem da configuração do Portal e do Serviço MIM](media/install-mim-service-portal/MIM_Install13.png)

12. Especifique *CORPIDM* (o nome deste computador) como o endereço do servidor do Serviço MIM para o Portal do MIM.

13. Especifique `http://mim.contoso.com` como a URL do conjunto de sites do SharePoint.

14. Especifique `http://passwordregistration.contoso.com` como a porta de URL de registro de senha 80, recomenda atualizar mais tarde com o certificado SSL em 443.

15. Especifique `http://passwordreset.contoso.com` como a porta de URL de redefinição de senha 80, recomendamos a atualização mais tarde com o certificado SSL no 443.

16. Selecione a caixa de verificação para abrir as portas 5725 e 5726 na firewall e a caixa de verificação para conceder acesso ao Portal do MIM a todos os utilizadores autenticados.

## <a name="configure-mim-password-registration-portal"></a>Configurar o Portal de Registo de Palavras-passe do MIM

1. Defina o nome da conta de serviço do Registo SSPR para *contoso\MIMSSPR* e a respetiva palavra-passe para <em>Pass@word1</em>.

2. Especifique *passwordregistration.contoso.com* como o nome do host para o registro de senha do mim e defina a porta como **80**. Ative a opção **Abrir porta na firewall**.

   ![Imagem da introdução das informações de configuração utilizadas pelo IIS](media/install-mim-service-portal/MIM_Install14.png)

3. Será apresentado um aviso. Leia-o e clique em **Seguinte**.

4. Na tela de configuração do próximo portal de registro de senha do MIM, especifique *mim.contoso.com* como o endereço do servidor do serviço do mim para o portal de registro de senha.

## <a name="configure-mim-password-reset-portal"></a>Configurar o Portal de Reposição de Palavras-passe do MIM

1. Defina o nome da conta de serviço para o registro SSPR como *Contoso\MIMSSPR* e sua senha como <em>Pass@word1</em>.

2. Especifique *PasswordReset.contoso.com* como o nome do host para o portal de redefinição de senha do mim e defina a porta como **80**. Ative a opção **Abrir porta na firewall**.

   ![Imagem da introdução das informações de configuração utilizadas pelo IIS](media/install-mim-service-portal/MIM_Install15.png)

3. Será apresentado um aviso. Leia-o e clique em **Seguinte**.

4. Na tela de configuração do próximo portal de registro de senha do MIM, especifique *mim.contoso.com* como o endereço do servidor do serviço do mim para o portal de redefinição de senha.

## <a name="install-mim-service-and-portal"></a>Instalar o Portal e o Serviço MIM

Quando todas as definições de pré-instalação estiverem prontas, clique em **Instalar** para começar a instalar os componentes do **Serviço e Portal** selecionados.

Depois de concluída a instalação, verifique se o Portal do MIM está ativo.

1. Inicie o Internet Explorer e ligue-se ao Portal de MIM no *http://mim.contoso.com/identitymanagement* . Observe que pode haver um pequeno atraso na primeira visita a essa página.
    - Se necessário, autentique como *contoso\miminstall* no Internet Explorer.

2. No Internet Explorer, abra as **Opções da Internet**, mude para o separador **Segurança** e adicione o site à zona **Intranet local** se ainda não estiver lá.  Feche a caixa de diálogo **Opções da Internet**.

3. Permita que os utilizadores vejam as respetivas entradas no MIM.

    1.  Através do Internet Explorer, no **Portal do MIM**, clique nas **Regras de Política de Gestão**.

    2.  Procure a regra de política de gestão, **Gestão de utilizadores: os utilizadores podem ler os seus próprios atributos**.

    3.  Selecione esta regra de política de gestão e desmarque **A política está desativada**.

    4.  Clique em **OK** e em **Submeter**.

4.  Verifique se a firewall permite ligações de entrada para a porta TCP 5725 e 5726.

    1.  Inicie **Ferramentas Administrativas » Firewall do Windows** com **Segurança Avançada**.

    2.  Clique em **Regras de Entrada**.

    3.  Verifique se são apresentadas as duas regras seguintes:

    -   Serviço de Forefront Identity Manager (STS).
    -   Serviço de Forefront Identity Manager (Serviço Web).

    4.  Conclua o assistente e feche a aplicação **Firewall do Windows**.

    5.  Inicie o **Painel de Controlo » Rede e Internet » Ver estado e tarefas da rede**.

    6.  Verifique se existe uma Rede ativa listada como contoso.local como uma Rede de domínio.

    7.  Feche o **Painel de Controlo**.

> [!NOTE]
> Opcional: nesta fase, pode instalar os suplementos e as extensões do MIM.
 
> [!div class="step-by-step"]  
> [«Serviço de Sincronização do MIM](install-mim-sync.md)
> [Sincronizar bases de dados»](install-mim-sync-ad-service.md)
