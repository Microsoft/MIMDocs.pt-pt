---
title: Utilizar o Azure MFA para ativar o PAM | Documentos da Microsoft
description: "Configure o MFA do Azure como uma segunda camada de segurança quando os utilizadores ativarem funções de Privileged Access Management."
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: MT
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: b937b30da2dff9bbfeabf7dceb43fcaca99a1b63
ms.contentlocale: pt-pt
ms.lasthandoff: 07/10/2017


---

# Utilizar o MFA do Azure para ativação
<a id="using-azure-mfa-for-activation" class="xliff"></a>
Quando configurar uma função de PAM, pode escolher como autorizar os utilizadores que pedem para ativar a função. As opções que a atividade de autorização de PAM implementa são:

- Aprovação do proprietário da função
- Azure Multi-Factor Authentication (MFA)

Se nenhuma verificação estiver ativada, os utilizadores candidatos são automaticamente ativados para a respetiva função.

O Multi-Factor Authentication do Microsoft Azure (MFA) é um serviço de autenticação que requer que os utilizadores verifiquem as tentativas de início de sessão com uma aplicação móvel, uma chamada telefónica ou uma mensagem de texto. Está disponível para utilizar com o Microsoft Azure Active Directory e como um serviço para aplicações empresariais na nuvem e no local. Para o cenário de PAM, o MFA do Azure fornece um mecanismo de autenticação adicional que pode ser utilizado na autorização, independentemente da forma como um utilizador candidato foi autenticado anteriormente para o domínio PRIV do Windows.

## Pré-requisitos
<a id="prerequisites" class="xliff"></a>

Para utilizar o MFA do Azure com o MIM, é necessário:

- Acesso à Internet a partir de cada Serviço MIM que fornece o PAM, para contactar o serviço MFA do Azure
- Uma subscrição do Azure
- Licenças do Azure Active Directory Premium para utilizadores candidatos ou um meio alternativo de licenciamento do MFA do Azure
- Números de telefone para todos os utilizadores candidatos

## Criar um Fornecedor de MFA do Azure
<a id="creating-an-azure-mfa-provider" class="xliff"></a>

Nesta secção, irá configurar o fornecedor de MFA do Azure no Microsoft Azure Active Directory.  Se já estiver a utilizar o MFA do Azure, de forma autónoma ou configurado com o Azure Active Directory Premium, avance para a secção seguinte.

1.  Abra um browser e aceda ao [Portal clássico do Azure](https://manage.windowsazure.com) como administrador de subscrição do Azure.

2.  No canto inferior esquerdo, clique em **Novo**.

3.  Clique em **Serviços Aplicacionais > Active Directory > Fornecedor de Autenticação Multifator > Criação Rápida**.

4.  No campo **Nome**, introduza **PAM** e, no campo Modelo de Utilização, selecione Por Utilizador Ativado. Se já tem um diretório do Azure AD, selecione esse directório. Por fim, clique em **Criar**.

## Transferir as Credenciais do Serviço MFA do Azure
<a id="downloading-the-azure-mfa-service-credentials" class="xliff"></a>

Em seguida, irá gerar um ficheiro que inclui o material de autenticação do PAM para contactar o MFA do Azure.

1. Abra um browser e aceda ao [Portal clássico do Azure](https://manage.windowsazure.com) como administrador de subscrição do Azure.

2.  Clique em **Active Directory**, no menu do Portal do Azure e, em seguida, no separador **Fornecedores de Autenticação Multifator**.

3.  Clique no fornecedor de MFA do Azure que irá utilizar para o PAM e, em seguida, clique em **Gerir**.

4.  Na nova janela, no painel esquerdo, em **Configurar**, clique em **Definições**.

5.  Na janela **Multi-Factor Authentication do Azure**, clique em **SDK**, em **Transferências**.

6.  Clique na ligação **Transferir** na coluna ZIP do ficheiro com o idioma **SDK para ASP.net 2.0 C\#**.

![Transferir o SDK do Multi-Factor Authentication - captura de ecrã](media/PAM-Azure-MFA-Activation-Image-1.png)

7.  Copie o ficheiro ZIP resultante para todos os sistemas onde o serviço MIM estiver instalado. 

>[!NOTE]
> O ficheiro ZIP contém material para chaves, que é utilizado para efetuar a autenticação no serviço MFA do Azure.

## Configurar o serviço MIM para o MFA do Azure
<a id="configuring-the-mim-service-for-azure-mfa" class="xliff"></a>

1.  No computador onde o Serviço MIM está instalado, inicie sessão como um utilizador ou o utilizador que instalou o MIM.

2.  Crie uma nova pasta de diretório no diretório onde o Serviço MIM foi instalado, como `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service\\MfaCerts`.

3.  Através do Explorador do Windows, navegue para a pasta **pf\\certs** do ficheiro ZIP transferido na secção anterior e copie o ficheiro **cert\_key.p12** para o novo diretório.

4.  Através do Explorador do Windows, navegue para a pasta **pf** do ZIP e abra o ficheiro **pf\_auth.cs** num editor de texto, como o Wordpad.

5.  Localize estes três parâmetros: **LICENSE\_KEY**, **GROUP\_KEY**, **CERT\_PASSWORD**.

![Copiar os valores do ficheiro pf\_auth.cs - captura de ecrã](media/PAM-Azure-MFA-Activation-Image-2.png)

6.  Com o Bloco de Notas, abra **MfaSettings.xml** localizado em `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service`.

7.  Copie os valores dos parâmetros LICENSE\_KEY, GROUP\_KEY e CERT\_PASSWORD no ficheiro pf\_auth.cs para os respetivos elementos xml no ficheiro MfaSettings.xml.

8.  No elemento XML **<CertFilePath>**, especifique o nome de caminho completo do ficheiro cert\_key.p12 extraído anteriormente.

9.  No elemento **<username>**, introduza qualquer nome de utilizador.

10.  No elementos **<DefaultCountryCode>**, introduza o código de país para marcar os seus utilizadores, como 1 para os Estados Unidos e Canadá. Este valor é utilizado no caso dos utilizadores estarem registados com números de telefone que não tenham um código de país. Se o número de telefone de um utilizador tiver um código de país internacional distinto do configurado para a organização, esse código de país tem de ser incluído no número de telefone que será registado.

11.  Guarde e substitua o ficheiro **MfaSettings.xml** na pasta do Serviço MIM `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service`. 

> [!NOTE]
> No final do processo, certifique-se de que o ficheiro **MfaSettings.xml**, ou quaisquer cópias do mesmo, ou o ficheiro ZIP não são legíveis publicamente.

## Configurar utilizadores do PAM para o MFA do Azure
<a id="configure-pam-users-for-azure-mfa" class="xliff"></a>

Para um utilizador ativar uma função que requeira o MFA do Azure, o respetivo número de telefone tem de ser armazenado no MIM. Existem duas formas de definir este atributo.

Primeiro, o comando `New-PAMUser` copia um atributo de número de telefone da entrada de diretório do utilizador no domínio CORP para a base de dados do Serviço MIM. Tenha em atenção que se trata de uma operação única.

Segundo, o comando `Set-PAMUser` atualiza o atributo de número de telefone na base de dados do Serviço MIM. Por exemplo, o seguinte substitui o número de telefone de um utilizador do PAM existente no Serviço MIM. A respetiva entrada de diretório permanece inalterada.

```
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```


## Configurar funções de PAM para o MFA do Azure
<a id="configure-pam-roles-for-azure-mfa" class="xliff"></a>

Depois de todos os utilizadores candidatos a uma função de PAM terem os números de telefone armazenados na base de dados do Serviço MIM, a função pode ser configurada para requerer o MFA do Azure. Isto é feito utilizando os comandos `New-PAMRole` ou `Set-PAMRole`. Por exemplo,

```
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

O MFA do Azure pode ser desativado para uma função especificando o parâmetro "-MFAEnabled 0" no comando `Set-PAMRole`.

## Resolução de Problemas
<a id="troubleshooting" class="xliff"></a>

Os eventos seguintes podem ser encontrados no registo de eventos da Gestão de Acesso Privilegiado:

| ID  | Gravidade | Gerado por | Descrição |
|-----|----------|--------------|-------------|
| 101 | Erro       | Serviço MIM            | O utilizador não concluiu o MFA do Azure (por exemplo, não atendeu o telefone) |
| 103 | Informações | Serviço MIM            | O utilizador concluiu o MFA do Azure durante a ativação                       |
| 825 | Aviso     | Serviço de Monitorização de PAM | O número de telefone foi alterado                                |

Para saber mais informações sobre as chamadas de telefone falhadas (evento 101), também pode ver ou transferir um relatório do MFA do Azure.

1.  Abra um browser e aceda ao [Portal clássico do Azure](https://manage.windowsazure.com) como administrador global do Azure AD.

2.  Selecione **Active Directory** no menu do Portal do Azure e, em seguida, selecione o separador **Fornecedores de Autenticação Multifator**.

3.  Selecione o fornecedor de MFA do Azure que está a utilizar para o PAM e, em seguida, clique em **Gerir**.

4.  Na nova janela, clique em **Utilização** e, em seguida, clique em **Detalhes do Utilizador**.

5.  Selecione o intervalo de tempo e a caixa junto a **Nome** na coluna do relatório adicional. Clique em **Exportar para CSV**.

6.  Quando o relatório for gerado, pode visualizá-lo no portal ou, se o relatório do MFA for extenso, transfira-o para um ficheiro CSV. Os valores **SDK** na coluna **AUTH TYPE** indicam as linhas relevantes como pedidos de ativação de PAM: são eventos com origem no MIM ou outro software no local. O campo **USERNAME** é a GUID do objeto de utilizador na base de dados do serviço MIM. Se uma chamada não tiver êxito, o valor na coluna **AUTHD** será **Não** e o valor da coluna **CALL RESULT** irá conter os detalhes do motivo da falha.

