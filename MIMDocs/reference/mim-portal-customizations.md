---
title: Microsoft Identity Manager 2016 portal personalizações Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/26/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 144ce2344327f16f60269b4f505c30fd470e46da
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762381"
---
# <a name="microsoft-identity-manager-2016-portal-customization"></a>Personalização do portal Microsoft Identity Manager 2016

No Microsoft Identity Manager 2016 (MIM), pode personalizar elementos selecionados dos portais de senha, incluindo o logótipo do banner, recursos de cordas e folhas de estilo em cascata.

>[!WARNING]
>Limpe sempre a cache do navegador quando forem feitas quaisquer personalizações CSS.

Estão envolvidos os seguintes itens utilizados para personalizar o registo de senha MIM 2016 e repor portais:

- Pasta de personalização: Esta é a pasta que a MIM 2016 verifica antes da utilização das predefinições. Cada portal a ser personalizado requer uma pasta de Personalização. As personalizações só devem ser feitas nesta pasta, uma vez que o processo de configuração não substitui esta pasta em atualizações, instalações de modo de alteração ou instalações de modo de reparação.

- Strings.resources: Este é um ficheiro baseado em XML que permite modificar as cordas que aparecem no portal. Este ficheiro precisa de residir na pasta Deserdizações.

- Style.css: Esta é a folha de estilo em cascata usada pelos portais para personalização. Crie e modifique esta folha de estilo para alterar o logotipo. Também pode substituir todo o conteúdo da folha de estilo pelas suas próprias personalizações.

Para obter instruções detalhadas passo a passo sobre a personalização dos portais de registo de senha e redefinição de passwords, consulte o Guia do Laboratório de Teste: Demonstrando o Registo de Passwords MIM 2016 e a Personalização do Portal de Reset.

>[!WARNING]
>Para que a MIM reconheça quaisquer alterações personalizadas, tem de reiniciar o IIS `iisreset` executando.


## <a name="customizations-folder"></a>Pasta de personalização

No arranque, a MIM procura o ficheiro Strings.resources na pasta Personalização antes de utilizar as predefinições. Crie uma pasta de personalização sob o diretório do portal que pretende personalizar (ou seja, o portal de Registo de Palavra-Passe ou o portal De reset de palavra-passe). Se pretender personalizar ambos os portais, crie uma pasta de personalização em cada uma das seguintes localizações:

- `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Registration Portal`

- `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Reset Portal`

Para criar uma pasta de personalização para o portal de Registo de Palavras-Passe:

1. Navegue na `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Registration Portal` pasta.
   
2. Criar uma pasta chamada **Personalizações** .

Para criar uma pasta de personalização para o portal De reset de palavra-passe:

1. Navegue na `c:\Program Files\Microsoft Forefront Identity Manager\2010\Password Reset Portal` pasta.
   
2. Criar uma pasta chamada **Personalizações** .


## <a name="custom-strings-in-the-stringsresources-file"></a>Cordas personalizadas no ficheiro Strings.resources

Muitas das cordas do portal UI podem ser personalizadas criando um ficheiro Strings.resources e adicionando este ficheiro à pasta Personalização. Crie um ficheiro Strings.resources para cada portal que deseje personalizar.

Para criar cordas personalizadas no ficheiro Strings.resources:

1. No Bloco de Notas, copie e cole o seguinte código no ficheiro Strings.resources. Guarde o ficheiro na pasta Deserdizações para o portal.

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <root>
     <resheader name="resmimetype">
       <value>text/microsoft-resx</value>
     </resheader>
     <resheader name="version">
       <value>2.0</value>
     </resheader>
     <resheader name="reader">
       <value>System.Resources.ResXResourceReader, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
    </resheader>
     <resheader name="writer">
       <value>System.Resources.ResXResourceWriter, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
     </resheader>

    <!-- Customizations begin here -->
     <data name=" QAGateResetTitle " xml:space="preserve">
       <value>Contoso Question and Answer Reset</value>
     </data>
     <data name="ResetPageTitle" xml:space="preserve">
       <value>Contoso Self-Service Password Reset</value>
     </data>
   </root>

   ```

2.  De acordo com a `<!-- Customizations begin here -->` secção do código, altere o nome dos dados para corresponder às cordas que pretende personalizar. Introduza o valor da cadeia entre as `<value></value>` etiquetas. Consulte as próximas secções para as cordas que podem ser personalizadas e os seus valores predefinidos.

>[!NOTE]
>O ficheiro Strings.resources é neutro em termos linguísticos. Para criar cordas personalizadas específicas da linguagem, tem de ter esse pacote de idiomas instalado e guardar o ficheiro no formato Strings `<language>-<culture>.resources` . Por exemplo, para a cultura da língua inglesa, o nome do ficheiro é **Strings.en-us.resources** .


## <a name="portal-strings"></a>Cordas do portal
A tabela a seguir mostra as cordas do portal que podem ser personalizadas:

<!-- The default values are actual UI strings and should not copy edited. -->

| Nome da corda do portal | Valor predefinido |
|---|---|
| Sobre o LinkText                                            | Sobre  |
| BotãoCancel                                             | Cancelar |
| BotãoFinish                                             | Concluir |
| BotãoNext                                               | Seguinte   |
| ButtonOk                                                 | OK     |
| CancelFinishedMessage                                    | A sua sessão já não está ativa. Pode fechar a janela ou reiniciar clicando no link abaixo. |
| Instituto CancelFinished                                      | Sessão terminada |
| ErrorDescription_3000                                    | Ocorreu um erro. Por favor, tente novamente, e se o problema persistir, contacte o seu balcão de ajuda ou administrador do sistema. (Erro 3000) |
| ErrorDescription_3001                                    | Certifique-se de que introduz corretamente o nome do utilizador. Se ainda não conseguir repor a sua palavra-passe, contacte o seu helpdesk para obter assistência. (Erro 3001) |
| ErrorDescription_3002                                    | A sua sessão terminou. Volte à página inicial para começar de novo. (Erro 3002) |
| ErrorDescription_3003                                    | A conta de utilizador corrente não é reconhecida pelo Gestor de Identidade da Vanguarda. Por favor contacte o seu balcão de ajuda ou administrador de sistema. (Erro 3003) |
| ErrorDescription_3004                                    | Não está autorizado a registar-se para reiniciar a palavra-passe. Por favor contacte o seu balcão de ajuda ou administrador de sistema. (Erro 3004) |
| ErrorDescription_3005                                    | Uma ou mais respostas que forneceu não correspondem às respostas que forneceu durante o Registo de Passwords. Para redefinir a sua palavra-passe, as respostas que fornece agora devem corresponder às respostas que forneceu quando se registou. Pode recomeçar a partir da página inicial ou contactar o seu balcão de ajuda ou administrador do sistema. (Erro 3005) |
| ErrorDescription_3006                                    | A palavra-passe que introduziu está incorreta. Tem de introduzir a palavra-passe correta para se registar para o Reset da Palavra-Passe. (Erro 3006) |
| ErrorDescription_3007                                    | Está temporariamente proibido de redefinir a sua palavra-passe. Por favor, tente novamente mais tarde, ou contacte o seu balcão de ajuda ou administrador do sistema para obter assistência. (Erro 3007) |
| ErrorDescription_3008                                    | Ocorreu um erro. Por favor, tente novamente, e se o problema persistir, contacte o seu balcão de ajuda ou administrador do sistema. (Erro 3008) |
| ErrorDescription_3009                                    | A sua entrada contém texto num formato que não é permitido. Tente novamente com uma entrada diferente, ou contacte o seu balcão de ajuda ou administrador de sistema. (Erro 3009) |
| ErrorDescription_3010_Registration                       | A scripting não está ativada no seu navegador. Ative a script e volte à página inicial do Registo de Passwords ou contacte o seu balcão de assistência. |
| ErrorDescription_3010_Reset                              | A scripting não está ativada no seu navegador. Ative a script e volte à página inicial do Redefinidor de Palavras-Passe ou contacte o seu balcão de assistência. |
| ErrorDescription_3011                                    | Este site utiliza cookies. Configure o seu navegador para aceitar cookies e tente novamente, ou contacte o seu balcão de ajuda para obter assistência. |
| ErrorDescription_3012                                    | Os dados que introduziu não correspondem ao código de segurança que lhe foi enviado. Pode tentar redefinir a sua palavra-passe novamente ou contactar o seu balcão de ajuda para obter assistência.  |
| ErrorDescription_3013                                    | Incapaz de enviar um código de segurança. Por favor contacte o seu balcão de ajuda para obter assistência. |
| ErrorMessageDomainUsernameFormat                         | Insira o seu nome de utilizador no formato correto. |
| ErrorMessageDomainUsernameRequired                       | Insira um nome de utilizador para continuar. |
| ErrorMessagePasswordRequired                             | Introduza uma senha. |
| ErrorMessagePasswordsDoNotMatch                          | Certifique-se de que ambas as palavras-passe correspondem. |
| ErrorPageDefaultHeading                                  | Erro de Aplicação <br/>**Nota:** A posição é seguida por '=' e a mensagem de erro.
| ErrorPageServerTime                                      | Hora do servidor: {0:T} <br/>**Nota:** {0} é o momento em que a exceção foi apanhada. 'T' faz com que o tempo de passagem seja formatado como um "longo tempo" mostrando a hora, minuto e segundo. A designação AM/PM também é mostrada dependendo da cultura atual. |
| ErrorPageTitle                                           | Gestão de Identidade da Vanguarda - Erro de senha | 
| ErrorTitle_3000                                          | Erro |
| ErrorTitle_3001                                          | Acesso Negado |
| ErrorTitle_3002                                          | Sessão terminada |
| ErrorTitle_3003                                          | Utilizador não reconhecido |
| ErrorTitle_3004                                          | Utilizador não autorizado |
| ErrorTitle_3005                                          | As respostas não combinam |
| ErrorTitle_3006                                          | Senha incorreta |
| ErrorTitle_3007                                          | Acesso negado temporariamente |
| ErrorTitle_3008                                          | Erro de Comunicação |
| ErrorTitle_3009                                          | Entrada Proibida |
| ErrorTitle_3010                                          | Erro de configuração do navegador |
| ErrorTitle_3011                                          | Erro de configuração do navegador |
| ErrorTitle_3012                                          | A verificação falhou |
| ErrorTitle_3013                                          | Incapaz de enviar código de segurança |
| FinalizarRegistrationHeading1                             | Se alguma vez precisar de redefinir a sua palavra-passe: |
| FinalizarRegistrationSubHeading1                          | Vá ao portal de senha de reset |
| FinalizarRegistrationSubHeading2                          | Verifique a sua identidade |
| FinalizarRegistrationSubHeading3                          | Escolha a sua nova senha |
| AcabamentoDescrição                                     | Escolha a sua nova senha |
| FinishingTle                                           | Reset da palavra-passe: |
| GotoPortalPrefix                                         | Ir para |
| GotoPortalS sulés                                         | página inicial |
| LabelTroubleshootingLinkText                             | Ver Detalhes |
| LoadingText                                              | A carregar... |
| NoScriptTagErrorMessage                                  | A scripting não está ativada no seu navegador. Ative a script e volte à página inicial ou contacte o seu balcão de ajuda para obter assistência.  |
| PasswordResetOperationGeneralErrorMessage                | Erro enquanto tenta restabelecer a palavra-passe. |
| PasswordResetOperationPolicyViolationErrorMessage        | A palavra-passe não está em conformidade com as políticas de senha da sua organização. |
| PasswordResetOperationUserCantChangePasswordErrorMessage | Erro ao redefinir a palavra-passe, o utilizador não pode alterar a palavra-passe. |
| Estado da Privacidade                                         | Declaração de Privacidade |
| Inscrição Descrição                                  | Registo de Self-Service password |
| Inscrição                                      | Se alguma vez esquecer a sua palavra-passe, pode reiniciá-la sem ligar para o seu balcão de assistência. |
| Página de Registos                                    | Gestão de Identidades Da Vanguarda - Registo de Passwords |
| Passos de Registo                                        | Clique em 'Seguinte' para iniciar o processo de inscrição. |
| InscriçãoSDessucrição de Unidade                           | Está registado |
| RegistrOsSuccessTitle                                 | Concluído: |
| RegistroSEsese                                 | Registo de senha: |
| Reposição Dadescrição                                         | Reposição Personalizada de Palavra-passe |
| ResetEnterNamePrompt                                     | Por favor, insira o seu nome de utilizador abaixo |
| ResetEnterPassword                                       | Introduza uma nova senha: |
| ResetExample1                                            | `contoso\mmeyers` |
| ResetExample2                                            | `mmeyers\@contoso.com` |
| ResetExamples                                            | Exemplos: |
| ResetPageTitle                                           | Gestão de Identidades Da Vanguarda - Reset de Password |
| ResetReenterPassword                                     | Reintrodui a senha: |
| ReposiçãoSuccessDesscriptação                                  | A palavra-passe do utilizador foi reposta |
| ResetSuccessTitle                                        | Sucesso: |
| ResetUseNewPassword                                      | Pode agora utilizar a sua nova palavra-passe para iniciar sessão. |
| ResetUsernameTextFormat                                  | (Redefinição da palavra-passe {0} para) <br/>**Nota:** {0} é o início de sísmia do utilizador. |
| ResetWelcomeTitle                                        | Reset da palavra-passe: |
| Resolução de problemasEmailSubject                              | FIM solicita detalhes de erro de processamento |
| Resolução de problemasLabelAttributes                           | Attributes: |
| Resolução de problemasLabelCloseButton                          | Fechar |
| Resolução de problemasLabelCopyToClipboard                      | Copiar para a área de transferência |
| Resolução de problemasLabelCorrelationId                        | Id de correlação: |
| Resolução de problemasLabelDetails                              | Detalhes: |
| Resolução de problemasLabelPostCopyClipboardMessage             | A informação foi copiada para a área de transferência. |
| Resolução de problemasLabelRequestId                            | ID do Pedido: |
| Resolução de problemasLabelSendemail                            | Enviar informações por e-mail |
| Resolução de problemasLabelSource                               | Razão: |
| Resolução de problemasLabelViewRequestDetails                   | Ver Detalhes do Pedido |
| Resolução de problemasLinkText                                  | Informação sobre resolução de problemas |


## <a name="authentication-gate-strings"></a>Cadeias do Portão de Autenticação
A tabela a seguir mostra as cordas do Portão de Autenticação que podem ser personalizadas:

<!-- The default values are actual UI strings and should not copy edited. -->

| Nome da corda do portão de autenticação | Valor predefinido |
|---|---|
| OTPEmailRegistraionEmailTextboxLabel                  | Endereço de e-mail: |
| OTPEmailRegistrationEmailRequiredErrorMessage         | O campo de endereços de e-mail não pode estar vazio. |
| OTPEmailRegistrationFooterReadOnly                    | Para atualizar o seu endereço de e-mail, siga o processo definido pela sua organização ou ligue para o seu balcão de assistência. |
| OtPEmailRegistrationFooterReadWrite                   | O endereço de e-mail é armazenado pela sua organização no Gestor de Identidade da Vanguarda. |
| OtPEmailRegistrationGateTitle                         | Verificação de endereço de e-mail |
| OTPEmailRegistrationHeaderReadOnly                    | Se alguma vez precisar de redefinir a sua palavra-passe, será enviado um código de segurança de verificação para o seu email. Se o endereço de e-mail apresentado abaixo não estiver correto, terá de o atualizar para utilizar a redefinição da palavra-passe de autosserviço. |
| OtPEmailRegistrationHeaderReadWrite                   | Insira o seu endereço de e-mail abaixo. Se alguma vez precisar de redefinir a sua palavra-passe, será enviado um código de verificação para o seu email. |
| OtPEmailResetGateTitle                                | Verifique a sua identidade: e-mail |
| OtPEmailResetHeader                                   | Introduza o seu código de segurança abaixo. Foi enviado um código de segurança para o endereço de e-mail registado nesta organização. |
| OTPRegularExpressionErrorMessage                      | O valor especificado não corresponde ao formato esperado. |
| OTPResetOneTimePasswordRequiredErrorMessage           | O campo de código de segurança não pode estar vazio. |
| OTPResetVerificationLabel                             | Código de Segurança: |
| OTPSmsRegistrationFooterReadOnly                      | Para atualizar o seu número de telemóvel, siga o processo definido pela sua organização ou ligue para o seu balcão de assistência. |
| OTPSmsRegistrationFooterReadWrite                     | O número de telemóvel é armazenado pela sua organização no Gestor de Identidade da Vanguarda. |
| OTPSmsRegistrationGateTitle                           | Verificação do telemóvel |
| OTPSmsRegistrationHeaderReadOnly                      | Se alguma vez precisar de redefinir a sua palavra-passe, será enviado um código de segurança de verificação para o seu telemóvel. Se o número de telemóvel apresentado abaixo não estiver correto, terá de o atualizar para utilizar a redefinição da palavra-passe de autosserviço. |
| OTPSmsRegistrationHeaderReadWrite                     | Insira o seu número de telemóvel abaixo. Se alguma vez precisar de redefinir a sua palavra-passe, será enviado um código de verificação para o seu telemóvel. |
| OTPSmsRegistrationMobilePhoneRequiredErrorMessage     | O campo de números de telemóvel não pode estar vazio. |
| OTPSmsRegistrationSMSTextBoxLabel                     | Telemóvel: |
| OTPSmsResetGateTitle                                  | Verifique a sua identidade: Telemóvel |
| OTPSmsResetHeader                                     | Introduza o seu código de segurança abaixo. Um código de segurança foi enviado para o telemóvel registado nesta organização. |
| PasswordGateDescriptionText                           | Introduza a sua palavra-passe atual abaixo e, em seguida, clique em 'Seguinte'. |
| PasswordGateErrorMessagePasswordRequired              | Insira a sua senha atual. |
| PasswordGateGateTitle                                 | A sua senha atual |
| PasswordGatePasswordLabelText                         | Palavra-passe: |
| PasswordGateUsernameTextFormat                        | `<i>` (insaludo como: `<b>{0}</b>` ) `</i>` |
| QAGateErrororNotEnoughQuestionsAnswered                 | Deve responder pelo menos a {0} perguntas. |
| QAGateIncorrectAnswer                                 | As suas respostas não estão corretas. |
| QAGatePrivacyNotice                                   | As respostas que fornece são armazenadas pela sua organização no Gestor de Identidade da Vanguarda. |
| QAGateRegistrationNumberOfQuestionsExplanation_Format | Deve responder pelo menos {0} às perguntas a registar. |
| QAGateRegistrationOneOrMoreAnswersFailedValidation    | Uma ou mais respostas não cumprem a política. |
| QAGateRegistrationThisAnswerValidationFailed          | Esta resposta não está em conformidade com a política. |
| QAGateRegistrationTitle                               | Registar as suas respostas |
| QAGateResetNumberOfQuestionsExplanation_Format        | Deve responder {0} às seguintes {1} perguntas. |
| QAGateResetTitle                                      | Verificar a sua identidade: Submeter as suas respostas  |


## <a name="custom-logo-banners"></a>Banners de logotipo personalizado

O banner predefinido nas páginas do portal pode ser personalizado para a sua organização.

Para personalizar o banner do logotipo:

1. Crie os seus banners personalizados e guarde-os como ficheiros .png. Os ficheiros devem satisfazer as seguintes recomendações:

   - Tamanho: 490 x 50 pixels.
   - Profundidade do bit: 32 pixels.

2. Copie os ficheiros para a pasta Desativações em cada portal que pretende personalizar.

3. Crie um ficheiro Style.css em cada pasta. Aponte o ficheiro para a pasta de Personalização para o portal e para o novo logótipo. Pode alterar o nome do logotipo conforme necessário, tal como `/Customizations/contosologo.png` . O CSS deve parecer o seguinte código:

   `.title-block{background:url(../Customizations/fimlogo.png) no-repeat scroll 0 0 transparent;}`

4. Se estiver a utilizar o Internet Explorer 6.0, deve fornecer um logótipo alternativo não transparente e adicionar o seguinte código ao Style.css:

   `.ie6 .title-block{background-image:url(../Customizations/fimlogo-ie6.png);}`

   O CSS deve parecer o seguinte código:
   
   `.title-block{background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;}`


## <a name="custom-images-for-smartphones"></a>Imagens personalizadas para smartphones

Pode personalizar a imagem do logotipo para smartphones. 

Para personalizar uma imagem para um smartphone:

1. Crie as suas imagens e guarde-as como ficheiros .png. Os ficheiros devem satisfazer as seguintes recomendações:

   - Tamanho: 190 X 50 pixels.
   - Profundidade do bit: 32 pixels.

2. Copie os ficheiros para a pasta Desativações em cada portal que pretende personalizar.

3. Crie um ficheiro Style.css em cada pasta. Aponte o ficheiro para a pasta de Personalização para o portal e para o novo logótipo. Pode alterar o nome do logotipo conforme necessário, tal como `/Customizations/contosologo.png` . O CSS deve parecer o seguinte código:

   ```css
   @media only screen and (max-width: 480px)

   {

    .title-block

     {
       background: url("path_to_image/imagename.png") no-repeat scroll 0 0 transparent;
     }

   }
   ```

## <a name="custom-style-sheets"></a>Folhas de estilo personalizado

Pode modificar o layout e o estilo dos portais de senha utilizando uma folha de estilo em cascata personalizada (CSS).

Para utilizar um CSS personalizado:

1. Crie os seus ficheiros CSS personalizados e guarde-os como Style.css.

2. Copie os ficheiros para a pasta Desativações em cada portal que pretende personalizar.

O seguinte código é um exemplo básico de um ficheiro Style.css:

```css
body
{
  font: 15px Algerian;
  color: \#303030;
  background: white;
}

.pad
{
  padding: 30px;
  padding-top: 50px;
  background: white;
}

.backgroundWhite
{
  border: \#e9e9e9 2px solid;
} .

title-block
{
background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;
}
```

>[!IMPORTANT]
>Para que a MIM reconheça quaisquer alterações personalizadas, tem de reiniciar o IIS `iisreset` executando.

O código a seguir é um exemplo mais avançado de um ficheiro Style.css. Este ficheiro fornece informações específicas para um smartphone ou um iPad da Apple para exibir os portais nestes dispositivos.

```css
/****************
BASE
*****************/

body {
    font-size: 14px; /*Customizeable- Body Font Size */
    background-color: #ced5ec; /*Customizeable- Backgound Color behind the product */
}

body, button, input, select, textarea {
    font-family: Segoe UI, Arial, Verdana, Sans-Serif, Helvetica; /*Customizeable- Body Font Family */
    color: #595959; /*Customizeable- Body Font Color */
}

/****************
LINKS
*****************/

a { color: #396faf; text-decoration: none; } /*Customizeable- Link Color and Underline */
a:visited { color: #396faf; text-decoration: none; } /*Customizeable- After Link is clicked color and underline */
a:hover { color: #6486ae; text-decoration: none; } /*Customizeable- Hover mouse over Link color and underline */
a:focus { outline: thin dotted; } /*Customizeable- Keyboard event to Link and Link is in focus outline*/
a:hover, a:active { outline: 0; } /*Customizeable- Hover and Active Link outline */

/****************
Typography
*****************/

hr { border-top: 1px solid #acd9ec; } /*Customizeable- Horizontal Rule Color Above the Footer */

/****************
Layout
*****************/

#wrapper {
    background: url(../images/bg-top-slice.png) repeat-x 0 0; /*Customizeable-remove this line to remove top gradient */
}

#container {
    background: url(../images/bg-bottom-slice.png) repeat-x 100% 100% transparent;  /*Customizeable-remove this line to remove bottom gradient */
}

.title-block {
    background: url("../images/fimlogo.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 600px or less in width. Logo must be 50px or less in height. */
    border-bottom: 2px solid #acd9ec;/*Customizeable- 2px border color under logo */
}

.ie6 .title-block {
    background-image: url(../images/fimlogo-ie6.png);   /*Customizeable- Can make a non-transparent image for IE6 only */
}

h2 {
    color: #578e4c; /*Customizeable- h2 page header color */
}

h3 {
    color: #999; /*Customizeable- h3 page header color */
}

input[type=text]:focus, input[type=password]:focus {
    border: #82bd3b 2px solid; /*Customizeable- Highlight color around textbox when cursor is inside */
}

.chromeButton, .chromeButton:visited {
    background-color: #333; /*Customizeable- Color of button */
    color: #fff; /*Customizeable- Color of text on the button */
    border: 1px solid #666; /*Customizeable- Border color of button */
}

.chromeButton:hover {
    background-color: #666; /*Customizeable- Hover color of button */
    border: 1px solid #999; /*Customizeable- Hover border color of button */
}

.qcol /*Style from QAgate.css */ {
    color: #7a7a7a; /*Customizeable- Font color of Q&A container */
    background-color:#e6e7e9; /*Customizeable- Background color of Q&A container */
}

/****************
Media Queries
*****************/

/* Smartphones ----------- */
@media only screen and (max-width: 480px) {
    body {
        font-size:12px; /*Customizeable- Body Font Size for devices */
    }

    .title-block {
        background: url("../images/fim-logo-portrait.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 190px (landscape) or less in width. Logo must be 50px or less in height. */
    }
    h2, h3 {
        font-size:14px; /*Customizeable- H2 and H3 Heading Size for devices */
    }
}


/* iPads (landscape) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : landscape)
{
}

/* iPads (portrait) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : portrait)
{
}
```


## <a name="common-customization-issues"></a>Questões comuns de personalização

A tabela que se segue lista questões comuns que podem ocorrer ao atualizar o Serviço FIM e o Portal MIM.

| Problema | Resolução |
|---|---|
| Fiz uma personalização de cordas, mas não se refletiu na UI.                       | As personalizações de cordas em cadeias.os recursos requerem o reinício do IIS executando `iisreset` . |
| Depois de fazer uma mudança de cordas.recursos, não vejo nenhuma das minhas mudanças de corda.          | O formato Strings.resources é provavelmente mal formado, e pode ser ignorado pelo portal. Verifique o registo de eventos no **Windows Logs**  >  **Application and Services Logs**  >  **Front Identity Manager** . |
| A primeira vez que adicionei Style.css, não vi as mudanças de estilo no portal.     | A primeira vez que introduz um ficheiro Style.css, tem de ser `iisreset` executado. |
| Novos estilos são adicionados ou modificados em Style.css, mas as alterações não são vistas no navegador. | Limpe a cache do navegador e refresque a página. Verifique a sintaxe CSS. |
| Mudei diretamente o conteúdo da pasta CSS `<path_to_sspr_portal>\css\*.css` ou o logótipo do `<path_to_sspr_portal>\images\fimlogo.png` banner. Perdi estas mudanças na atualização. | Recomenda-se aos utilizadores que não alterem diretamente estes ficheiros. Utilize apenas a pasta Deserdizações para fornecer um logotipo de banner, e faça apenas personalizações de estilo CSS em Style.css. A pasta Personalização não é substituída deliberadamente por grandes atualizações. Não utilize ferramentas como ILSpy e Refletor para alterar cordas nos conjuntos do portal. Use strings.recursos para anular as cordas predefinidos. Os conjuntos são substituídos na atualização.  |
| O logótipo do banner não é apresentado nos portais. Ainda vejo o logótipo fim.                  | O nome de imagem/caminho em Style.css não é válido, ou a cache do navegador não foi limpa.  |
| O logotipo do banner parece feio no Internet Explorer 6.                                          | Forneça uma imagem não transparente para o Internet Explorer 6 com um estilo correspondente para a imagem em style.css. |
