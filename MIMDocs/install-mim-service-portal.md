---
title: Instalar o Portal e o Serviço Microsoft Identity Manager | Documentos da Microsoft
description: Obter os passos para configurar e instalar o Portal e o Serviço MIM do Microsoft Identity Manager 2016
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldiwn
ms.date: 04/30/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f975bf659f285418380d52db5724bcbb46bbcf46
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333739"
---
# <a name="install-mim-2016-mim-service-and-portal"></a>Instalar o MIM 2016: Portal e Serviço MIM

> [!div class="step-by-step"]
> [«Serviço de Sincronização do MIM](install-mim-sync.md)
> [Sincronizar bases de dados»](install-mim-sync-ad-service.md)
> 
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

7. Sobre o **configurar ligação do servidor de correio**, introduza o nome do servidor Exchange como **servidor de email** ou pode utilizar **caixa de correio do Office 365**. Se não tiver um servidor de e-mail configurado, utilize **localhost** como o nome do servidor de e-mail e desmarque as duas caixas de verificação superiores. Clique em **Seguinte**.

    ![Imagem da configuração da ligação ao servidor de correio](media/install-mim-service-portal/MIM_Install11.png)

8. Especifique que pretende gerar um novo certificado autoassinado ou selecione o certificado relevante.

9. Especifique o nome da Conta de Serviço a utilizar, por exemplo, *ServiçoMIM*, e a palavra-passe da Conta de Serviço, por exemplo, <em>Pass@word1</em>, o domínio da Conta de Serviço, por exemplo, *contoso*, e a Conta de E-mail do Serviço, por exemplo, *contoso*.

    ![Imagem da configuração da conta de serviço MIM](media/install-mim-service-portal/MIM_Install12.png)

10. Tenha em atenção que poderá aparecer um aviso a indicar que a Conta de Serviço não está protegida na configuração atual.

11. Aceite as predefinições para a localização do servidor de sincronização e especifique a conta de agente de gestão de MIM como *contoso\MIMMA*.

    ![Imagem da configuração do Portal e do Serviço MIM](media/install-mim-service-portal/MIM_Install13.png)

12. Especifique *CORPIDM* (o nome deste computador) como o endereço do servidor do Serviço MIM para o Portal do MIM.

13. Especifique *http://mim.contoso.com* como o SharePoint URL de coleção de sites.

14. Especifique *http://passwordregistration.contoso.com* como o URL de registo de palavras-passe de porta 80, recomendamos a atualizar mais tarde com o certificado SSL em 443.

15. Especifique *http://passwordreset.contoso.com* como o URL de reposição de palavra-passe de porta 80, recomendamos a atualizar mais tarde com o certificado SSL em 443.

16. Selecione a caixa de verificação para abrir as portas 5725 e 5726 na firewall e a caixa de verificação para conceder acesso ao Portal do MIM a todos os utilizadores autenticados.

## <a name="configure-mim-password-registration-portal"></a>Configurar o Portal de Registo de Palavras-passe do MIM

1. Defina o nome da conta de serviço do Registo SSPR para *contoso\MIMSSPR* e a respetiva palavra-passe para <em>Pass@word1</em>.

2. Especifique *passwordregistration.contoso.com* como o nome de anfitrião para o registo de palavra-passe de MIM e defina a porta para **80**. Ative a opção **Abrir porta na firewall**.

   ![Imagem da introdução das informações de configuração utilizadas pelo IIS](media/install-mim-service-portal/MIM_Install14.png)

3. Será apresentado um aviso. Leia-o e clique em **Seguinte**.

4. No seguinte ecrã de configuração do Portal de registo de palavra-passe de MIM, especifique *mim.contoso.com* como o endereço de servidor do serviço de MIM para o Portal de registo de palavra-passe.

## <a name="configure-mim-password-reset-portal"></a>Configurar o Portal de Reposição de Palavras-passe do MIM

1. Defina o nome de conta de serviço de registo SSPR para *Contoso\MIMSSPR* e a respetiva palavra-passe para <em>Pass@word1</em>.

2. Especifique *passwordreset.contoso.com* como o nome de anfitrião para o Portal de reposição de palavra-passe de MIM e defina a porta para **80**. Ative a opção **Abrir porta na firewall**.

   ![Imagem da introdução das informações de configuração utilizadas pelo IIS](media/install-mim-service-portal/MIM_Install15.png)

3. Será apresentado um aviso. Leia-o e clique em **Seguinte**.

4. No seguinte ecrã de configuração do Portal de registo de palavra-passe de MIM, especifique *mim.contoso.com* como o endereço de servidor do serviço de MIM para o Portal de reposição de palavra-passe.

## <a name="install-mim-service-and-portal"></a>Instalar o Portal e o Serviço MIM

Quando todas as definições de pré-instalação estiverem prontas, clique em **Instalar** para começar a instalar os componentes do **Serviço e Portal** selecionados.

Depois de concluída a instalação, verifique se o Portal do MIM está ativo.

1. Inicie o Internet Explorer e ligue-se ao Portal de MIM no *http://mim.contoso.com/identitymanagement*. Tenha em atenção que pode existir um curto atraso na primeira visita a esta página.

    - Se necessário, efetue a autenticação como *contoso\miminstall* para o Internet Explorer.

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
> 
> [!div class="step-by-step"]  
> [«Serviço de Sincronização do MIM](install-mim-sync.md)
> [Sincronizar bases de dados»](install-mim-sync-ad-service.md)
