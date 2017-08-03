---
title: "As personalizações de Portal do Microsoft Identity Manager 2016 | Microsoft Docs"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: bebb299ece077a6ab2e0d75f3b30ad1776678303
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2017
---
# <a name="microsoft-identity-manager-2016-portal-customization"></a>Personalização de Portal do Microsoft Identity Manager 2016


>[!Warning]
Lembre-se de que limpar a cache do browser quando forem efetuadas as personalizações de CSS.

No Microsoft Identity Manager 2016 (MIM), pode personalizar elementos selecionados de portais a palavra-passe, incluindo o logótipo de faixa, recursos de cadeias e cascata as folhas de estilos.

Para tal, algumas coisas são necessárias consoante o nível de personalização. Segue-se uma lista de itens que estão envolvidos na personalizar o registo de palavra-passe de MIM 2016 e os portais de reposição.

-   Pasta de personalizações - esta é a pasta que irá verificar o MIM 2016 antes de utilizar as predefinições. Cada portal que irá ser personalizada necessita de uma pasta de personalizações. Personalizações só devem ser feitas nesta pasta porque a configuração não irá substituí-lo em atualizações, alterar modo instala ou repare a instalação de modo.

-   Strings.resources - este é um ficheiro baseado em XML que lhe permite modificar as cadeias que aparecem no portal. Este ficheiro tem de residir na pasta personalizações.

-   Style.css – esta é a folha de estilo em cascata utilizada pelos portais de personalização. Esta folha de estilos tem de ser criadas e modificadas para alterar o logótipo ou pode ser substituída totalmente com as suas próprias personalizações.

Para obter instruções passo a passo detalhadas sobre a personalização de reposição de palavra-passe e de registo de palavra-passe portais consulte o guia de laboratório de teste: demonstrar o registo de palavra-passe de MIM 2016 e personalização do Portal de reposição.

>[! WARGNING] por ordem de MIM reconhecer quaisquer alterações personalizadas, tem de reiniciar o IIS ao executar o iisreset.


## <a name="creating-the-customizations-folder"></a>A criação da pasta de personalizações

No arranque, MIM irá procurar o ficheiro de Strings.resources na pasta personalizações antes de utilizar as predefinições. Tem de criar uma pasta de personalizações sob o diretório para o portal de que pretende personalizar (ou seja, o Portal de registo de palavras-passe ou o Portal de reposição de palavra-passe). Se pretender personalizar ambos os portais, em seguida, terá de criar uma pasta de personalizações em cada um dos seguintes:

-   C:\\ficheiros de programa\\do Microsoft Forefront Identity Manager\\2010\\Portal de registo de palavras-passe

-   C:\\ficheiros de programa\\do Microsoft Forefront Identity Manager\\2010\\Portal de reposição de palavra-passe

### <a name="to-create-the-customization-folder"></a>Para criar a pasta de personalização

1.  Navegue até à unidade c:\\os ficheiros de programa\\Microsoft Forefront Identity Manager\\2010\\pasta do Portal de registo de palavras-passe.

2.  Crie uma pasta denominada personalizações.

3.  Navegue até ao nível do back um para a pasta de Portal de reposição de palavra-passe e crie uma pasta denominada personalizações.

## <a name="customizing-strings"></a>Personalizar cadeias

Muitas das cadeias na IU do portal podem ser personalizadas através da criação de um ficheiro de Strings.resources e adicionar este ficheiro para a pasta de personalizações. Terá de criar um ficheiro de Strings.resources para cada portal que pretende personalizar.

### <a name="to-customize-strings"></a>Para personalizar as cadeias

1.  A utilizar o bloco de notas, copie o seguinte código abaixo para-lo e guardá-lo para a pasta de personalizações como Strings.resources

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
2.  Sob o `<!-- Customizations begin here -->` secção alterar o nome de dados para corresponder as cadeias que pretende personalizar e introduza o valor para essa cadeia entre o <value> </value> etiquetas. Consulte a lista abaixo para as cadeias que podem ser personalizadas e os respetivos valores predefinidos.

>[!NOTE]
O **Strings.resources** ficheiro é independente de idioma. Para criar cadeias de personalizadas específicas de idioma, tem de ter esse pacote de idiomas instalado e guarde o ficheiro no formato cadeias. `<language>-<culture>.resources`, por exemplo Strings.en-us.resources.

Segue-se uma lista das cadeias portais que podem ser personalizadas.

<a name="portal-strings"></a>Cadeias de portais
--------------

| Nome                                                     | Valor Predefinido                                                                                                                                                                                                                                                                                                                                         | Comentário                                                                                                                                                                                                                                            |
|---------------------------|-------------------|--------------|
| AboutLinkText                                            | Acerca de         |       |
| ButtonCancel                                             | Cancelar                                                                                 |     |
| ButtonFinish                                             | Concluir    |    |
| ButtonNext                                               | Seguinte    |    |
| ButtonOk                                                 | OK     |   |
| CancelFinishedMessage                                    | A sua sessão já não está ativa. Pode fechar a janela ou reiniciar clicando na hiperligação abaixo.         |      |
| CancelFinishedTitle                                      | Sessão terminada                                                              |      |
| ErrorDescription_3000                                    | Ocorreu um erro. Tente novamente e, se o problema persistir, contacte o administrador de suporte técnico ou o sistema de ajuda. (Erro 3000)       |          |
| ErrorDescription_3001                                    | Certifique-se de que introduza corretamente o seu nome de utilizador. Se ainda não é possível repor a palavra-passe, contacte o seu      |           |
|                                                          | suporte técnico para obter assistência. (Erro 3001)   |                   |
| ErrorDescription_3002                                    | A sessão terminou. Regressar à home page para começar de novo. (Erro 3002)                                     |                |
| ErrorDescription_3003                                    | A conta de utilizador atual não é reconhecida pelo Forefront Identity Manager.Please contacte o administrador de suporte técnico ou o sistema de ajuda. (Erro 3003)           |               |
| ErrorDescription_3004                                    | Não está autorizado a registar para a reposição de palavra-passe. Contacte o administrador de suporte técnico ou o sistema de ajuda. (Erro 3004)     |              |
| ErrorDescription_3005                                    | Um ou mais respostas fornecidas não correspondem às que indicou durante a ordem de palavra-passe Registration.In para repor a palavra-passe, as respostas que fornecer agora têm de corresponder às que indicou quando efectuou. Pode iniciar novamente a partir da home page, ou contacte o administrador de suporte técnico ou o sistema de ajuda. (Erro 3005) |           |
| ErrorDescription_3006                                    | A palavra-passe que introduziu está incorreta. Tem de introduzir a palavra-passe correta para se registar para repor a palavra-passe. (Erro 3006)            |              |
| ErrorDescription_3007                                    | Está temporariamente proibido de repor a palavra-passe. Tente novamente mais tarde, ou contacte o administrador de suporte técnico ou o sistema de ajuda para obter assistência. (Erro 3007)     |         |
| ErrorDescription_3008                                    | Ocorreu um erro. Tente novamente e, se o problema persistir, contacte o administrador de suporte técnico ou o sistema de ajuda. (Erro 3008)        |          |
| ErrorDescription_3009                                    | Os dados introduzidos contêm texto num formato que não é permitido. Tente novamente com entrada diferentes, ou contacte o administrador de suporte técnico ou o sistema de ajuda. (Erro 3009)     |             |
| ErrorDescription_3010_Registration                       | Processamento de scripts não está ativado no seu browser. Ative o scripting e regressar à home page do registo de palavra-passe ou contacte o suporte técnico para obter assistência.      |               |
| ErrorDescription_3010_Reset                              | Processamento de scripts não está ativado no seu browser. Ative o scripting e regresse à home page de reposição de palavra-passe ou contacte o suporte técnico para obter assistência.     |            |
| ErrorDescription_3011                                    | Este site utiliza cookies. Configure os seu browser para aceitar cookies e tente novamente ou contacte o suporte técnico para obter assistência.          |                                           |
| ErrorDescription_3012                                    | Os dados introduzidos não correspondeu ao código de segurança que lhe foi enviado. Pode tentar repor a palavra-passe novamente ou contacte o suporte técnico para obter assistência.      |                                                                                                                                                                                                                                                    |
| ErrorDescription_3013                                    | Não é possível enviar um código de segurança. Contacte o suporte técnico para obter assistência.                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameFormat                         | Introduza o nome de utilizador no formato correto.                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameRequired                       | Introduza um nome de utilizador para continuar.                                              |                         |
| ErrorMessagePasswordRequired                             | Introduza uma palavra-passe.              |                                                |
| ErrorMessagePasswordsDoNotMatch                          | Certifique-se de que ambas as palavras-passe corresponder.                                |           |
| ErrorPageDefaultHeading                                  | Erro da aplicação                                            |               |
| ErrorPageServerTime                                      | Hora do servidor: {0:T}                     | {0} é o tempo no qual a exceção detetada. A possível ' faz com que o tempo de transmitido a formatados como "muito tempo." Vai terminar a cópia de segurança que mostra a hora, minuto e segundo e possivelmente a designação AM/PM (dependendo da cultura atual). |
| ErrorPageTitle                                           | Microsoft Identity Management - erro de palavra-passe                     |   |
| ErrorTitle_3000                                          | Erro                                  |  |
| ErrorTitle_3001                                          | Acesso negado                          |  |
| ErrorTitle_3002                                          | Sessão terminada                          |  |
| ErrorTitle_3003                                          | Utilizador não reconhecido                      |  |
| ErrorTitle_3004                                          | Utilizador não autorizado                      |  |
| ErrorTitle_3005                                          | Respostas não correspondem                    |  |
| ErrorTitle_3006                                          | Palavra-passe incorreta                     |  |  
| ErrorTitle_3007                                          | Acesso negado temporariamente              |  |
| ErrorTitle_3008                                          | Erro de comunicação                    |  |
| ErrorTitle_3009                                          | Proibido de entrada                       |  |
| ErrorTitle_3010                                          | Erro de configuração do browser            |  |
| ErrorTitle_3011                                          | Erro de configuração do browser            |  |
| ErrorTitle_3012                                          | Falha na verificação                    |  |
| ErrorTitle_3013                                          | Não é possível enviar o código de segurança   |  |
| FinalizeRegistrationHeading1                             | Se alguma vez precisar de repor a palavra-passe:        |   |
| FinalizeRegistrationSubHeading1                          | Vá para o portal de reposição de palavra-passe   |   |
| FinalizeRegistrationSubHeading2                          | Verificar a sua identidade   |   |
| FinalizeRegistrationSubHeading3                          | Escolha a sua nova palavra-passe    |     |
| FinishingDescription                                     | Escolha a sua nova palavra-passe        |    |
| FinishingTitle                                           | Reposição de palavra-passe:      |     |
| GotoPortalPrefix                                         | Aceda a     |        |
| GotoPortalSuffix                                         | página inicial     |      |
| LabelTroubleshootingLinkText                             | Ver detalhes           |    |
| LoadingText                                              | A carregar...     |  |
| NoScriptTagErrorMessage                                  | Processamento de scripts não está ativado no seu browser. Ative o scripting e regresse à home page ou contacte o suporte técnico para obter assistência.      |   |
| PasswordResetOperationGeneralErrorMessage                | Ocorreu um erro ao tentar repor a palavra-passe.       |   |
| PasswordResetOperationPolicyViolationErrorMessage            | A palavra-passe não está em conformidade com as políticas de palavra-passe da sua organização.             |       |
| PasswordResetOperationUserCantChangePasswordErrorMessage | Erro ao repor a palavra-passe, o utilizador não é possível alterar a palavra-passe.    |   |
| PrivacyStatement                                         | Declaração de Privacidade                                                      |    |
| RegistrationDescription                                  | Registo de palavra-passe Self-Service     |    |
| RegistrationMission                                      | Se se esquecer da sua palavra-passe, pode repô-la sem chamar o suporte técnico.   |      |
| RegistrationPageTitle                                    | Microsoft Identity Management - registo de palavra-passe                                          |   |
| RegistrationSteps                                        | Clique em 'Seguinte' para iniciar o processo de registo.   |      |
| RegistrationSuccessDescription                           | Agora está registado        |   |
| RegistrationSuccessTitle                                 | Concluída:   |    |
| RegistrationWelcomeTitle                                 | Registo de palavra-passe:    | |
| ResetDescription                                         | Reposição Personalizada de Palavra-passe  |    |
| ResetEnterNamePrompt                                     | Introduza o nome de utilizador abaixo     |     |
| ResetEnterPassword                                       | Introduza uma nova palavra-passe:                                                  |   |
| ResetExample1                                            | Contoso\\mmeyers                                                                            |      |
| ResetExample2                                            | mmeyers\@contoso.com     |      |
| ResetExamples                                            | Exemplos:  |    |
| ResetPageTitle                                           | Microsoft Identity Management - reposição de palavra-passe       |     |
| ResetReenterPassword                                     | Volte a introduzir a palavra-passe:    | |
| ResetSuccessDescription                                  | A palavra-passe foi reposta    |    |
| ResetSuccessTitle                                        | Êxito:                                |    |
| ResetUseNewPassword                                      | Agora pode utilizar a sua nova palavra-passe para iniciar sessão.     |      |
| ResetUsernameTextFormat                                  | (Repor a palavra-passe para {0})       | {0} é o início de sessão do utilizador             |
| ResetWelcomeTitle                                        | Reposição de palavra-passe:     |      |
| TroubleshootingEmailSubject                              | Detalhes do erro de processamento de pedidos FIM     |       |
| TroubleshootingLabelAttributes                           | Atributos:    |    |
| TroubleshootingLabelCloseButton                          | Fechar       |    |
| TroubleshootingLabelCopyToClipboard                      | Copiar para área de transferência     |     |
| TroubleshootingLabelCorrelationId                        | Id de correlação:      |          |
| TroubleshootingLabelDetails                              | Detalhes:                                                             |    |
| TroubleshootingLabelPostCopyClipboardMessage             | As informações foi copiadas para a área de transferência.           |    |
| TroubleshootingLabelRequestId                            | Id do pedido:                  |    |
| TroubleshootingLabelSendEmail                            | Enviar informações por E-Mail                            |    |
| TroubleshootingLabelSource                               | Razão:                                                                     |    |
| TroubleshootingLabelViewRequestDetails                   | Ver detalhes do pedido        |              |                                                    
| TroubleshootingLinkText                                  | Informações de resolução de problemas          |                  | |



Segue-se uma lista das cadeias de porta de autenticação que podem ser personalizadas.

<a name="authentication-gate-strings"></a>Cadeias de porta de autenticação
---------------------------

| Nome                                          | Valor Predefinido                                                                                                                                                                                                                | Commnent |
|-----------|--------------|----------|
| OTPEmailRegistraionEmailTextboxLabel          | Endereço de correio eletrónico:             |          |
| OTPEmailRegistrationEmailRequiredErrorMessage | O campo de endereço de correio eletrónico não pode estar vazio.     |          |
| OTPEmailRegistrationFooterReadOnly            | Para atualizar o seu endereço de e-mail, siga o processo definido pela sua organização ou contacte o suporte técnico.                   |          |
| OTPEmailRegistrationFooterReadWrite           | O endereço de correio eletrónico foi armazenado pela sua organização no Forefront Identity Manager.                       |          |
| OTPEmailRegistrationGateTitle                 | Verificação do endereço de e-mail   |          |
| OTPEmailRegistrationHeaderReadOnly            | Se alguma vez precisar de repor a palavra-passe, será enviado um código de segurança de verificação ao seu e-mail. Se o endereço de e-mail mostrado abaixo não estiver correto, terá de o actualizar para utilizar a reposição de palavra-passe self-service. |          |
| OTPEmailRegistrationHeaderReadWrite           | Introduza o seu endereço de correio eletrónico abaixo. Se alguma vez precisar de repor a palavra-passe, será enviado um código de verificação ao seu e-mail.                 |          |
| OTPEmailResetGateTitle                        | Verificar a sua identidade: E-Mail         |          |
| OTPEmailResetHeader                           | Introduza o código de segurança abaixo. Foi enviado um código de segurança para o endereço de e-mail registado nessa organização.                                                                                                             |          |
| OTPRegularExpressionErrorMessage              | O valor especificado não corresponde ao formato esperado.                   |          |
| OTPResetOneTimePasswordRequiredErrorMessage   | O campo de código de segurança não pode estar vazio.        |          |
| OTPResetVerificationLabel                     | Código de segurança:                    |          |
| OTPSmsRegistrationFooterReadOnly                      | Para atualizar o seu número de telemóvel, siga o processo definido pela sua organização ou contacte o suporte técnico.   ||
| OTPSmsRegistrationFooterReadWrite                     | O número de telemóvel foi armazenado pela sua organização no Forefront Identity Manager.                                                                                                                                                     |   |
| OTPSmsRegistrationGateTitle                           | Verificação do telemóvel                      |   |
| OTPSmsRegistrationHeaderReadOnly                      | Se alguma vez precisar de repor a palavra-passe, será enviado um código de segurança de verificação para o seu telemóvel. Se o número de telemóvel mostrado abaixo não estiver correto, terá de o actualizar para utilizar a reposição de palavra-passe self-service. |   |
| OTPSmsRegistrationHeaderReadWrite                     | Introduza o seu número de telemóvel abaixo. Se alguma vez precisar de repor a palavra-passe, será enviado um código de verificação para o seu telemóvel.                                                                                                     |   |
| OTPSmsRegistrationMobilePhoneRequiredErrorMessage     | O campo do número de telemóvel não pode estar vazio.      |   |
| OTPSmsRegistrationSMSTextBoxLabel                     | Telemóvel:                    |   |
| OTPSmsResetGateTitle                                  | Verificar a sua identidade: telemóvel         |   |
| OTPSmsResetHeader                                     | Introduza o código de segurança abaixo. Foi enviado um código de segurança para o telemóvel registado nessa organização.                                                                                                                           |   |
| PasswordGateDescriptionText                           | A palavra-passe actual abaixo e, em seguida, clique em 'Seguinte'.        |   |
| PasswordGateErrorMessagePasswordRequired              | Introduza a palavra-passe atual.                  |   |
| PasswordGateGateTitle                                 | A palavra-passe atual               |   |
| PasswordGatePasswordLabelText                         | Palavra-passe:                 |   |
| PasswordGateUsernameTextFormat                        | `<i>`(com sessão iniciada como:`<b>{0}</b>)</i>`                                                          |   |
| QAGateErrorNotEnoughQuestionsAnswered                 | Tem de responder a pelo menos {0} perguntas.        |   |
| QAGateIncorrectAnswer                                 | As suas respostas não estão corretas.       |   |
| QAGatePrivacyNotice                                   | As respostas fornecidas são armazenadas pela sua organização no Forefront Identity Manager.                                                                                                                                                  |   |
| QAGateRegistrationNumberOfQuestionsExplanation_Format | Tem de responder a pelo menos {0} perguntas para se registar.     |   |
| QAGateRegistrationOneOrMoreAnswersFailedValidation    | Uma ou mais respostas não estão em conformidade com a política.      |   |
| QAGateRegistrationThisAnswerValidationFailed          | Esta resposta não está em conformidade com a política.      |   |
| QAGateRegistrationTitle                               | Registar as suas respostas         |   |
| QAGateResetNumberOfQuestionsExplanation_Format        | Tem de responder {0} das perguntas {1} seguinte.   |   |
| QAGateResetTitle                                      | Verificar a sua identidade: Submeter as suas respostas                                  |   |



## <a name="customizing-the-logo-banner"></a>Personalizar a faixa de logótipo

A faixa predefinida nas páginas do portais pode ser personalizada para a sua organização.
Para personalizar a faixa de logótipo:

1.  Crie o seu faixas personalizadas e guardá-los como ficheiros PNG. Os ficheiros devem cumprir as seguintes recomendações:

 - Tamanho: 490 X 50 pixéis.

 - Profundidade de bits: 32

2.  Copie os ficheiros para o \\pasta personalizações no cada portal que pretende personalizar.

3.  Crie um ficheiro Style.css cada pasta. Ter ponto para a pasta de personalizações e o logótipo de novo... Pode alterar o nome de logótipo, se aplicável (ou seja, /Customizations/contosologo.png). O código deve ter o seguinte aspeto:

   **Exemplo 1:**

  `.title-block{background:url(../Customizations/fimlogo.png) no-repeat scroll 0 0 transparent;}`

4.  Se estiver a utilizar o Internet Explorer 6.0, terá de fornecer um logótipo não transparente alternativo e adicione o seguinte código para Style.css:

  `.ie6 .title-block{background-image:url(../Customizations/fimlogo-ie6.png);}`

  **Exemplo 2:**

  `.title-block{background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;}`

Se estiver a utilizar o Internet Explorer 6.0, terá de fornecer um logótipo não transparente alternativo e adicione o seguinte código para Style.css:

  `.ie6 .title-block{background-image:url(../Customizations/contosologo-ie6.png);}`

## <a name="customizing-image-for-smartphones"></a>Personalizar a imagem para SmartPhones

Pode personalizar a imagem de SmartPhones, utilizando o seguinte. Para personalizar a imagem de SmartPhone:

1.  Criar a sua imagem e guardá-los como ficheiros PNG. Os ficheiros devem cumprir as seguintes recomendações:

    - Tamanho: 190 X 50 pixéis.
    - Profundidade de bits: 32

2.  Copie os ficheiros para o \\pasta personalizações no cada portal que pretende personalizar.

2.  Crie um ficheiro Style.css cada pasta. Ter ponto para a pasta de personalizações e o logótipo de novo... Pode alterar o nome de logótipo, se aplicável (ou seja, /Customizations/contosologo.png). O código deve ter o seguinte aspeto:

  **Exemplo 1:**

```css
@media only screen and (max-width: 480px)

   {

    .title-block

     {
       background: url("path_to_image/imagename.png") no-repeat scroll 0 0 transparent;
     }

   }
   ```

## <a name="customizing-style-sheets"></a>Personalizar as folhas de estilos

Pode modificar o esquema e o estilo de portais a palavra-passe através da utilização de uma folha de estilo em cascata (CSS) personalizadas. Para utilizar um CSS personalizados:

1.  Criar os seus ficheiros CSS personalizados e guardá-los como Style.css.

2.  Copie os ficheiros para o \\pasta personalizações no cada portal que pretende personalizar.

Segue-se um exemplo de um ficheiro Style.css básico:

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
Ordem de MIM reconhecer quaisquer alterações personalizadas, tem de reiniciar o IIS ao executar o iisreset.                                                                                                                                                                                       

Segue-se um exemplo mais avançado de um ficheiro Style.css. Este ficheiro fornece smartphone e ipad informações específicas para apresentar os portais nestes dispositivos.

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


## <a name="common-customization-issues"></a>Problemas comuns de personalização

A tabela seguinte é uma lista dos problemas conhecidos comuns que podem ocorrer com a atualização do Portal e serviço FIM. Esta tabela inclui também resoluções para estes problema

|Problema |Resolução                                                                    |
|------|------------------------------|
| Posso efetuar uma personalização de cadeia, mas não foi refletido na IU         | Cadeia personalizações strings.resources necessitam sempre de iisreset         |
| Depois de efetuar uma alteração de strings.resources, não vejo qualquer uma das alterações a minha cadeia já     | Strings.resources formato está incorreto, provavelmente e, por conseguinte, é ignorado pelo portal. Verifique o registo de eventos em registos do Windows – registos de serviços e aplicações – Forefront Identity Manager                        |
| A primeira que posso adicionar Style.css, não vejo o meu alterações de estilo no portal do            | O tempo relativo ao primeiro que introduz um ficheiro Style.css, precisa de fazer uma iisreset     |
| Estilos novos são adicionadas/modificada Style.css, mas as alterações não são visualizadas no browser      | Limpar a cache do browser e atualize a página <br/> Verifique a sintaxe do CSS    |
| Posso alterar diretamente o conteúdo da pasta CSS no path_to_sspr_portal\\css\\\*. css ou o logótipo de faixa path_to_sspr_portal\\imagens\\fimlogo.png e perdida estas alterações na atualização | Nunca deve alterar estes ficheiros lugar. Utilize apenas a pasta de personalização como um meio para fornecer que um logótipo de faixa e CSS estilo personalizações no Style.css. Pasta de personalizações deliberadamente não é substituída pelos principais atualizações <br/><br/>Não tente utilizar ferramentas como ILSpy e Refletor alterar cadeias no portais assemblagens. Utilize strings.resources para substituir as cadeias de predefinição. As assemblagens são substituídas actualização  |
| Logótipo de faixa não é apresentado na portais / vejo ainda o logótipo FIM     | O nome/caminho de imagem no Style.css não é válido ou não foi limpar a cache do browser          |
| Logótipo de faixa procura ugly no IE6       | Terá de fornecer uma imagem não transparente para IE6 e um estilo associado especial no style.css        |
