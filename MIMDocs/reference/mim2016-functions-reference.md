---
title: "Referência do Microsoft Identity Manager 2016 de função | Microsoft Docs"
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
ms.openlocfilehash: 8f36cf981971db0d6c55fc17cce874a8faf0ecaf
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2017
---
# <a name="functions-reference-for-microsoft-identity-manager-2016"></a>Referência das funções do Microsoft Identity Manager 2016


No Microsoft Identity Manager (MIM) 2016, as funções permitem-lhe modificar os valores de atributo antes de flui-las para um destino de uma actividade de função ou declarativos de aprovisionamento. O objetivo deste documento é para lhe dar uma descrição geral das funções disponíveis e uma descrição de como pode utilizá-los.

Configurar os mapeamentos de fluxo de atributos é uma tarefa básicas quando configurar as regras de sincronização. A forma mais simples de um mapeamento de fluxo de atributos é um mapeamento direto. Conforme indicado pelo nome, um mapeamento direto aceita o valor de um atributo de origem e aplica-se-lo para o atributo de destino configurado. Existem casos onde a de valores de atributo existentes a modificação ou novos valores de atributo deve ser calculado antes do sistema aplica-as para um destino.

As funções são um método incorporado utilizado para definir o tipo de modificação que terá o motor de sincronização para aplicar ao gerar um valor de atributo para um destino.

Em MIM, pode agrupar as funções existentes nas seguintes categorias:

-   **As funções de manipulação de dados**. Funções para efetuar uma variedade de operações de manipulação de cadeias.

-   **Funções de obtenção de dados**. Funções para extrair dados de valores de atributo.

-   **As funções do dados geração**. Funções para gerar valores.

-   **Funções de lógica**. Funções para executar operações com base nas condições.

As secções seguintes fornecem mais detalhes sobre as funções em cada categoria.

## <a name="data-manipulation-functions"></a>Funções de manipulação de dados

As funções de manipulação de dados são utilizadas para efetuar uma variedade de operações de manipulação de cadeias.

| Concatenar        |   |
|--------------------|-------------------------|
| Descrição        | A função Concatenate é utilizada para concatenar duas ou mais cadeias.                                                                                                       |
| Assinatura de função | string1 + string2...                                                                                                                                                     |
| Entradas             | Dois ou mais cadeias                                                                                                                                                        |
| Operações         | Todos os parâmetros de cadeia de entrada são concatenados entre si.                                                                                                              |
| Saída             | Uma cadeia de carateres        |


| Em maiúsculas         |         |
|-------------------|---------|
| Descrição        | A função maiúsculas converte todos os carateres existentes numa cadeia de maiúsculas.         |
| Assinatura de função | Cadeia UpperCase(string)                                                                                                                                                   |
| Entradas             | Uma cadeia de carateres                                                                                                                                                                 |
| Operações         | Todos os carateres minúsculos do parâmetro de entrada são convertidos em carateres maiúsculos. Exemplo: UpperCase("test") resulta numa "Teste".                                     |
| Saída             | Uma cadeia de carateres                                                              |


| Em minúsculas          |                                 |
|--------------------|---------------------------------|
| Descrição        | A função em minúsculas converte todos os carateres existentes numa cadeia caso inferior.                                                                                                  |
| Assinatura de função | Cadeia LowerCase(string)                                                                                                                                                   |
| Entradas             | Uma cadeia de carateres                                                                                                                                                                 |
| Operações         | Todos os carateres maiúsculos do parâmetro de entrada são convertidos em carateres minúsculos. Exemplo: LowerCase("TeSt") resulta numa "teste".                                     |
| Saída             | Uma cadeia de carateres               |


| ProperCase        |                                                          |
|-------------------|----------------------------------------------------------|
| Descrição        | O ProperCase funcionar coverts o primeiro caráter de cada palavra delimitada por espaços de uma cadeia de maiúsculas e todos os outros carateres são convertidos em maiúsculas e minúsculas inferior.           |
| Assinatura de função | Cadeia ProperCase(string)                                                                                                                                                  |
| Entradas             | Uma cadeia de carateres                                                                                                                                                                 |
| Operações         | O primeiro caráter de cada palavra delimitada por espaços no parâmetro de entrada é convertido em maiúsculas, e são convertidos todos os carateres maiúsculos para carateres minúsculos. Se uma palavra no parâmetro de entrada for iniciado com um caráter não alfabeto, o primeiro caráter da palavra não é convertido para maiúscula. <br/> Exemplos: <br/> -ProperCase("TEsT") resulta numa "Teste". <br/> -ProperCase("britta simon") resulta numa "Britta Simon". <br/>-ProperCase("TEsT") resulta numa "Teste". <br/> -ProperCase("\$TEsT") resulta em "\$teste".|
| Saída             | Uma cadeia de carateres      |


| LTrim              |      |
|--------------------|------|
| Descrição        | A função de LTrim remove espaços em branco à esquerda de uma cadeia.                                                                                                             |
| Assinatura de função | Cadeia LTrim(string)                                                                                                                                                       |
| Entradas             | Uma cadeia de carateres                                                                                                                                                                 |
| Operações         | Os carateres de espaço em branco à esquerda contidos no parâmetro de entrada são removidos. <br/><br/>Exemplo: LTrim ("Test") resulta em "Teste".                                              |
| Saída             | Uma cadeia de carateres      |



| RTrim              |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Descrição        | A função de RTrim remove espaços em branco à direita de uma cadeia.                                                                 |
| Assinatura de função | Cadeia RTrim(string)                                                                                                            |
| Entradas             | Uma cadeia de carateres                                                                                                                      |
| Operações         | Os carateres de espaço em branco à direita contidos no parâmetro de entrada são removidos. Exemplo: RTrim ("Test") resulta em "Teste".  |
| Saída             | Uma cadeia de carateres                                                                                                                      |


| Cortar               |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Descrição        | A função de compactação remove espaços em branco de uma cadeia de direita e à esquerda.                                                      |
| Assinatura de função | Cadeia Trim(string)                                                                                                             |
| Entradas             | Uma cadeia de carateres                                                                                                                      |
| Operações         | Os carateres de espaço em branco à direita e à esquerda contidos na cadeia são removidos. Exemplo: Compactar ("Test") resulta em "Teste". |
| Saída             | Uma cadeia de carateres                                                                                                                      |




| RightPad           |                                                                                                                                 |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Descrição        | Os RightPad função direito-pads uma cadeia para um comprimento especificado utilizando um caráter de preenchimento fornecido.                          |
| Assinatura de função | Cadeia RightPad (cadeia, comprimento, padCharacter)                                                                                   |
| Operações         | Se o comprimento da cadeia é inferior ao comprimento, em seguida, padCharacter é repetidamente acrescentado ao fim da cadeia até ter um comprimento igual ao comprimento. <br/> Exemplos: <br/> -RightPad("User", 10, "0") resultaria em "User000000". <br/> -RightPad(RandomNum(1,10), 5, "0") pode resultar em "9000".   |
| Saída                                                                                                                                                          | Se a cadeia tem um comprimento maior que ou igual ao comprimento, é devolvida uma cadeia idêntica à cadeia. Se o comprimento da cadeia é inferior ao comprimento, é devolvida uma nova cadeia do comprimento pretendida cadeia que contém será preenchida com um padCharacter. Se a cadeia for nula, a função devolve uma cadeia vazia. |   |   |
>[!NOTE]
**padCharacter** pode ser um caráter de espaço, mas não pode ser um valor nulo. Se o comprimento de **cadeia** é igual ou maior do que **comprimento**, **cadeia** é devolvido inalterado.


| LeftPad      |     |
|----|-------|
| Descrição  | Os LeftPad função left-pads uma cadeia para um comprimento especificado utilizando um caráter de preenchimento fornecido.    |
| Assinatura de função      | Cadeia LeftPad (cadeia, comprimento, padCharacter)     |
| Entradas |  - **Cadeia.** A cadeia a preencher. <br/> - **comprimento.** Um número inteiro que representa o comprimento pretendido da cadeia. <br/> - **padCharacter.** Uma cadeia que consistem de um caráter único a ser utilizado como um caráter de preenchimento. |
| Operações  | Se o comprimento da cadeia é inferior ao comprimento, em seguida, padCharacter é repetidamente acrescentado ao início da cadeia até ter um comprimento igual ao comprimento. <br/> Exemplos: <br/> -LeftPad("User", 10, "0") resultaria em "000000User". <br/> LeftPad(RandomNum(1,10), 5, "0") pode resultar em "0009". |  
|Saída | Se a cadeia tem um comprimento maior que ou igual ao comprimento, é devolvida uma cadeia idêntica à cadeia. <br/> Se o comprimento da cadeia é inferior ao comprimento, é devolvida uma nova cadeia do comprimento pretendida cadeia que contém será preenchida com um padCharacter. <br/>  Se **cadeia** é nulo, a função devolve uma cadeia vazia.                                                   |

<[!NOTE]
**padCharacter** pode ser um caráter de espaço, mas não pode ser um valor nulo. Se o comprimento de **cadeia** é igual ou maior do que **comprimento**, **cadeia** é devolvido inalterado.

| BitOr    |  |
|----- |------|
| Descrição  | A função de BitOr define um bit especificado de um sinalizador para 1.     |
| Assinatura de função  | Int BitOr(mask, flag)       |  
| Entradas     | 1. **máscara.** Um valor hexadecimal que especifica o bit para definir o sinalizador. <br/> 2. **sinalizador.** Um valor hexadecimal que consiste num bit específico modificado.    |   
| Operações         | Esta função converte ambos os parâmetros de representação binária e compara-as com: <br/> -Define um pouco para 1 se uma ou ambas bits correspondente na máscara e o sinalizador 1 e 0 se o bits correspondente são 0. <br/> -Devolve 1 em todos os casos, exceto em que o bits correspondente de ambos os parâmetros são 0. <br/> -O padrão de bit resultante é o "definir" (1 ou verdadeiro) bits de qualquer um dos dois operandos. Bits de sinalizador vários podem ser definidas se vários bits tem o valor 1 na máscara.  |
| Saída             | Uma nova versão do **sinalizador**, com os bits especificados no **máscara** definido como 1.                   |


| BitAnd             |                                                                                    |
|--------------------|------------------------------------------------------------------------------------|
| Descrição        | A função de BitAnd define um bit especificado de um sinalizador como 0.                           |
| Assinatura de função | Int BitOr(mask, flag)                                                              |
| Entradas             | 1. **máscara.** Um valor hexadecimal que especifica o bit modificar no sinalizador. <br/> 2. **sinalizador.** Um valor hexadecimal que consiste num bit específico modificado   |
| Operações         | Esta função converte ambos os parâmetros de representação binária e compara-as com: <br/> -Um pouco define como 0, se um ou ambos bits correspondente na **máscara** e **sinalizador** são 0 e para 1 se o bits correspondente são 1. <br/> -Devolve 0 em todos os casos, exceto em que o bits correspondente de ambos os parâmetros são 1. Bits de sinalizador vários podem ser definidos como 0, se vários bits tem o valor 0 **máscara.** |
| Saída             | Uma nova versão do **sinalizador** com bits especificados no **máscara** definido como 0.                    |

| DateTimeFormat                                 |    |
|---------------------------------------|------------|
| Descrição       | A função de DateTimeFormat é utilizada para formatar um DateTime no formato de cadeia para um formato especificado.     |
| Assinatura de função   | Cadeia DateTimeFormat (dateTime, formato)      |
| Entradas   | 1. dateTime. Uma cadeia que representa a DateTime para formatar.  <br/> 2. **formato.** Uma cadeia representando o formato a converter.  |   
| Operações           | A cadeia de formato especificada no formato é aplicada a DateTime na cadeia de dateTime. <br/> A cadeia especificada no formato tem de ser um formato de DateTime válido. Se não for, é devolvido um erro que indica que o formato não é um formato de DateTime válido. <br/> Exemplo: DateTime ("25/12/2007", "aaaa-MM-dd") resulta em "2007-12-25".|   
| Saída     | Uma cadeia resultante da aplicar **formato** para **dateTime.**   |

>[!Note]                                                                                                                                                                             
Para os carateres aceites para criar formatos definido pelo utilizador, consulte [formatos de data/hora definidas pelo utilizador](http://go.microsoft.com/fwlink/?LinkId=195182)


| ConvertSidToString       |    |   
|--------------------------|----|
| Descrição       | O ConvertSidToString converte uma matriz de bytes que contém um identificador de segurança para uma cadeia.         |
| Assinatura de função      | Cadeia ConvertSidToString(ObjectSID)    |
| Entradas  | **Sidobjeto.** Uma matriz de bytes que contém um identificador de segurança (SID).   |
| Operações    | O SID de binário especificado é convertido numa cadeia.    |
| Saída              | Uma representação de cadeia do SID.   |  

| ConvertStringToGuid |        |
|---------------------|--------|
| Descrição         | **O ConvertStringToGuid** função converte a representação de cadeia de um GUID para uma representação binária do GUID.      |
| Função            | Byte [] ConvertStringToGuid(stringGuid)  |  
| Entradas              | **GUID.** Uma cadeia formatada neste padrão: **xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx**, onde o valor do GUID é representado como uma série de dígitos hexadecimais em grupos de 8, 4, 4, 4 e 12 dígitos e separado por hífenes. Um exemplo de um valor de retorno é "382c74c3-721d-4f34-80e557657b6cbc27".  |
| Operações          | A cadeia **Guid** especificado no parâmetro 1 é convertido respetiva representação binária. <br/> Se a cadeia não é uma representação de um **Guid**, a função rejeita o argumento com o seguinte erro: <br/> **O parâmetro da função de ConvertStringToGuid tem de ser uma cadeia representando um Guid válido.**  |
| Saída              | Uma representação binária do Guid.           |                                                                           


| ReplaceString       |     |
|--------------------|-------|
| Descrição         | A função de ReplaceString substitui todas as ocorrências de uma cadeia de outra cadeia.  |   
| Função            | Cadeia ReplaceString (cadeia, OldValue, NewValue)    |                                                                          
| Entradas              | 1. **Cadeia.** Uma cadeia na qual pretende substituir valores. <br/> 2. **OldValue.** A cadeia para procurar e substituir. <br/> 3. NewValue. A cadeia para substituir a. |
| Operações          | Todas as ocorrências de OldValue na cadeia são substituídas por NewValue. A função tem de ser capaz de lidar com os seguintes carateres especiais: <br/> - **\n.** Nova linha. <br/> - **\r.** Devolva de avanço. <br/> - **\t.** Separador. <br/> Exemplo: ReplaceString ("One\n\rMicrosoft\n\r\Way", "\n\r","") devolve "One Microsoft Way". |   
| Saída              | Uma cadeia com todas as ocorrências de **OldValue** na cadeia de substituída **NewValue.**      |

## <a name="data-retrieval-functions"></a>Funções de obtenção de dados

Funções de obtenção de dados são utilizadas para efetuar operações de obter os carateres pretendidos a partir de uma cadeia.

| Word       |        |
|--------------------|---------------|
| Descrição        | A função do Word devolve uma palavra contida uma cadeia, com base nos parâmetros que descrevem os delimitadores a utilizar e o número de palavras para devolver.                                                                |
| Assinatura de função | Palavra de cadeia (cadeia, número, delimitadores)                                                                                                                                                                        |
| Entradas             | 1. **cadeia.** A cadeia da qual a devolver uma palavra. <br/> 2. **número.** Deve ser devolvido um número que identifica o número da palavra. <br/> 3. **delimeters.** Uma cadeia representando o delimeters que devem ser utilizadas para identificar palavras. |
| Operações         | Cada cadeia de carateres existentes numa cadeia que é separado por um dos carateres no delimitadores é identificada como uma palavra. É devolvido o word que foi encontrado na posição especificada no parâmetro 3 (número): <br/> -Se number < 1, devolve uma cadeia vazia. <br/> -Se a cadeia é nula, devolva uma cadeia vazia. <br/><br/> Exemplos: <br/> 1. Word ("Testar; de % função;", 3, "; $& %") devolve "função". <br/> 2. Word ("Testar; A função", 2,";") Devolve "" (uma cadeia vazia). 3. Word ("Testar; de % função;", 0,"; $& %") devolve ""(uma cadeia vazia).
| Saída             | Uma cadeia contendo o word na posição pedido ao utilizador para. Se **cadeia** conter menos do que o número de palavras, ou **cadeia** não contém quaisquer palavras identificadas por **delimeters,** é devolvida uma cadeia vazia. |  


| Esquerda               |   |
|-------|-------|
| Descrição        | A função à esquerda devolve um número especificado de carateres do lado esquerdo de uma cadeia.       |
| Assinatura de função | À esquerda de cadeia (cadeia, numChars)     |
| Entradas             | 1. **cadeia.** A cadeia da qual a devolver carateres. 2. **numChars.** Um número que identifica o número de carateres a devolver a partir do início de uma cadeia.         |
| Operações         | **numChars** carateres são devolvidos pela primeira posição da cadeia. <br/> Exemplo: À esquerda ("Britta Simon", 3) devolve "Bri".   |
| Saída             | Uma cadeia que contém os carateres numChars primeiro na cadeia.  <br/> -Se numChars = 0, devolver uma cadeia vazia. <br/> -Se numChars < 0, devolver uma cadeia de entrada. <br/> -Se a cadeia é nula, devolva uma cadeia vazia. |




| Direita       |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Descrição | A função à direita devolve um número especificado de carateres à direita (fim) de uma cadeia.                                 |
| Assinatura de função   | Cadeia direita (cadeia, numChars)   |
| Entradas      | 1. **Cadeia.** A cadeia da qual a devolver carateres. <br/> 2. **numChars.** Um número que identifica o número de carateres para devolver a partir do fim da cadeia.  |
| Operações  | **numChars.** Carateres são devolvidos a partir do final de uma cadeia. <br/> Exemplo: Direita ("Britta Simon", 3) devolve "mon".                  |
| Saída      | Uma cadeia que contém os carateres numChars último numa cadeia. Se numChars = 0, devolver uma cadeia vazia. <br/> -Se **numChars** < 0, devolver uma cadeia de entrada. <br/> -Se a cadeia é nula, devolva uma cadeia vazia. <br/> -Se a cadeia contém menos carateres do que o numChars especificado no número, é devolvida uma cadeia idêntica à cadeia. |




| Mid         |                                                                                                                               |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| Descrição | A função de Mid devolve um número especificado de carateres da posição especificada numa cadeia.                              |
| Assinatura de função    | Cadeia Mid(string, pos, numChars)                                                                                             |
| Entradas      | 1. **cadeia.** A cadeia da qual a devolver carateres.   <br/> 2. **pos.** Um número que identifica a posição inicial de uma cadeia de carateres a devolver. <br/> 3. **numChars.** Um número que identifica o número de carateres para devolver a partir de uma posição na cadeia.  |
| Operações  | Devolver **numChars** carateres a partir da posição **pos** na cadeia. <br/>Exemplo: Mid ("Britta Simon", 3, 5) devolvam "itta". |
| Saída      | Uma cadeia que contém **numChars** carateres da posição **pos** numa cadeia: <br/> -Se **numChars** = 0, devolver uma cadeia vazia. <br/> -Se **numChars** < 0, devolver uma cadeia vazia. <br/> -Se **pos** > o comprimento da cadeia, devolver uma cadeia de entrada. <br/> -Se **pos** ≤ 0, devolver uma cadeia de entrada. <br/> -Se **cadeia** for nulo, devolver uma cadeia vazia. <br/> Se existirem não **numChar** carateres restante no **cadeia** da posição **pos**, como o número de carateres que podem ser devolvidos é devolvido.

## <a name="data-generation-functions"></a>Funções de geração de dados

Funções de geração de dados são utilizadas para gerar valores para os tipos de dados específica.

| CRLF               |                                                                                          |
|--------------------|------------------------------------------------------------------------------------------|
| Descrição        | O CRLF gera um Feed de retorno/linha de avanço. Utilize esta função para adicionar uma nova linha. |
| Assinatura de função | Cadeia CRLF                                                                              |
| Entradas             | Sem parâmetros                                                                            |
| Operações         | É devolvido um CRLF no.                                                                      |
|                    | Exemplo: AddressLine1 + CRLF() + AddressLine2 resulta em: <br/> -AddressLine1 <br/> -AddressLine2 |
| Saída             | Um CRLF é o resultado.                                                                                             |

| RandomNum          |                                                                                                                   |  
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Descrição        | A função de RandomNum devolve um número aleatório dentro de um intervalo especificado.                                       |   
| Assinatura de função | Int RandomNum(start, end)                                                                                         |   
| Entradas             | - **Iniciar**. Um número que identifica o limite inferior do valor aleatório para gerar.   <br/> - **end**. Um número que identifica o limite superior do valor aleatório para gerar.  |
| Operações         | Um número aleatório maior ou igual a **iniciar** e inferior ou igual a **final** é gerado. <br/>  Exemplo: Random(0,999) devolver 100.                      |
| Saída             | Um número aleatório dentro do intervalo especificado pelo **iniciar** e **final**.                                                      |  

| EscapeDNComponent  |                                                                                                                   |   
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| Descrição        | O *EscapeDNComponent* método processa a cadeia de entrada com base no tipo de agente de gestão que está a ser utilizado. |
| Assinatura de função | Cadeia EscapeDNComponent(string)                                                                                  |
| Entradas     | **cadeia**. Uma cadeia que é utilizada para processar um nome exclusivo. A cadeia não deve conter carateres de escape. |
| Operações | O método de EscapeDNComponent dos MIISUtils é utilizado para efetuar esta operação. Este método processa a cadeia de entrada com base no tipo de agente de gestão que está a ser utilizado. <br/> Porque os agentes de gestão diferentes necessitam de formatos de nome único diferente, este método processa as cadeias de entrada com base no tipo de agente de gestão. Os tipos são Lightweight Directory Access Protocol LDAP) nome único, como os serviços de domínio do asActive Directory®, o servidor de diretório Sun (anteriormente iPlanet servidor de diretório), o Microsoft Exchange Server; nonLDAP hierárquica, tais como Microsoft Lotus notas; e extrinsic, tais como a base de dados e XML sem LDAP único nomes. <br/> * * Nome único do LDAP: * * <br/> -Qualquer carateres XML inválidos na parte do valor de uma determinada parte são com codificação hexadecimal. <br/>-Qualquer carateres ilegais (incluindo os carateres XML inválidos) na parte do nome de uma determinada parte geram um erro. <br/> -São escape os seguintes carateres: <br/> &nbsp;&nbsp;&nbsp;-Vírgulas (',') <br/> &nbsp;&nbsp;&nbsp;-Sinal de igual do ('=') <br/> &nbsp;&nbsp;&nbsp;-Sinal de adição ('+') <br/> &nbsp;&nbsp;&nbsp;-Menos-ao início de sessão ('< ') <br/> &nbsp;&nbsp;&nbsp;-Maior-ao início de sessão ('> ') <br/> &nbsp;&nbsp;&nbsp;-Sinal ('#') <br/> &nbsp;&nbsp;&nbsp;-Ponto e vírgula (';') <br/> &nbsp;&nbsp;&nbsp;-Barra invertida ('\') <br/> &nbsp;&nbsp;&nbsp;-Aspas (""') <br/> -Se o último caráter na cadeia de um espaço, em seguida, esse espaço é caráter de escape correto. <br/> -Qualquer supérfluas esquerda ou à direita são removidos os espaços em torno de um nome de parte. <br/> -Para o agente de gestão de XML, se existirem várias partes, em seguida, as partes são alphabetized. <br/> -Se forem especificadas várias partes, a cadeia de nome único composto é a concatenação das cadeias individuais separados por sinais de sinal de adição. <br/> -Um erro é gerado se a cadeia de entrada não é bem formado, estilo LDAP único cadeia do nome. <br/><br/> **Não LDAP hierárquica** <br/> -Estes agentes de gestão não suportam componentes com várias partes. Se vários cadeias são transmitidas para EscapeDNComponent, é emitida uma ArgumentException. <br/> -Se qualquer um dos carateres na cadeia de entrada são carateres XML inválidos, é emitida uma ArgumentException. <br/> -Todas as vírgulas e barras invertidas na cadeia de entrada são caráter de escape correto. <br/> -Se o último caráter na cadeia de um espaço, em seguida, esse espaço é caráter de escape correto. <br/><br/> **Extrinsic:** <br/> 1. Se qualquer parte binária ou contém um caráter XML inválido, parte é armazenado como uma versão com codificação hexadecimal dos dados não processados por um caráter '#' prefixo para a frente da cadeia. Por exemplo, se tiver sido uma parte 'AxC' (em que x representa um caráter XML ilegal, como '0x10'), essa parte é codificado como '#410010004300'. <br/> 2. Caso contrário, são escape todas as instâncias dos seguintes carateres: <br/> &nbsp;&nbsp;&nbsp;-Barra invertida ('\') <br/> &nbsp;&nbsp;&nbsp;-Vírgulas (',') <br/> &nbsp;&nbsp;&nbsp;-Sinal de adição ('+') <br/> &nbsp;&nbsp;&nbsp;-Sinal ('#') <br/> 3. Se o último caráter numa cadeia parte fornecido é um espaço, esse espaço é caráter de escape correto. <br/> 4. Se forem especificadas várias partes, a cadeia de nome único composto é a concatenação das cadeias individuais separados por sinais de sinal de adição.
| Saída      | Uma cadeia que contém um nome de domínio válida.                                                                                                                  |   

>[!NOTE]
A validação de nomes único é menos restrita do que a sintaxe definida nas especificações LDAP. EscapeDNComponent(String[]) permite um nome de parte para conter qualquer combinação de um ou mais carateres 'a'-'z', 'A'-'Z', '0'-'9', '-', e '.'. <br/>
Não é possível especificar uma parte binária com este método. No entanto, é possível ter uma parte binária **CommitNewConnector** se o nome único é construído a partir de atributos de âncora e um dos atributos âncora é um tipo binário.


| Valor nulo        |                                                                                                                                                           |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| Descrição | A função de Null é utilizada para definir que esta MÁ não tem um atributo contribuir e que precedência de atributos deve continuar com o seguinte MA. |   
| Assinatura de função    | Cadeia nula    |
| Entradas      | Sem parâmetros                                                                                                                                             |   
| Operações  | É devolvido um valor nulo. <br/> Exemplo: IIF(Eq(domain), "desconhecido", Null())                                                                                           |   
| Saída      | O resultado de é um valor nulo.                                                                                                                                         |   |   |


## <a name="logic-functions"></a>Funções de lógica
Funções de lógica são utilizadas para efetuar uma operação com base nas condições avaliadas pelo sistema.

| IIF        |  |
|-------------|---|
| Descrição | A função ' IIF devolve um de um conjunto de valores possíveis com base numa condição especificada.    |
| Assinatura de função   | Objeto IIF (condição valueIfTrue, valueIfFalse)   |                                                 |
| Entradas      | 1. **Condição**. Qualquer valor ou expressão que pode ser avaliado como true ou false. 2. **valueIfTrue** um valor que é devolvido se a condição for avaliada como true. <br/> 3. **valueIfFalse** um valor que é devolvido se a condição for avaliada como falsa. <br/><br/> Seguem-se as funções disponíveis para utilização como expressões na função IIF como **condição:** <br/> **EQ.** Esta função compara dois argumentos de igualdade. <br/> **NotEquals.** Esta função compara dois argumentos para inequality, devolver VERDADEIRO se não forem igual e false caso estes sejam iguais.<br/> Exemplo: NotEquals (EmployeeType, "Contratantes")<br/> **LessThan.** Esta função compara dois números, devolver VERDADEIRO se o primeiro é menor que o segundo e false caso contrário.<br/>Exemplo: LessThan (salário, 100000) <br/>**GreaterThan.** Esta função compara dois números, devolver true se o primeiro é maior que o segundo e false caso contrário.<br/> Exemplo: GreaterThan (salário, 100000) <br/> **LessThanOrEquals.** Esta função compara dois números, devolver true se o primeiro é menor ou igual ao segundo e false caso contrário.<br/>Exemplo: LessThanOrEquals (salário, 100000) <br/> **GreaterThanOrEquals.* Esta função compara dois números, devolver VERDADEIRO se a primeira é maior que ou é igual para o segundo e false caso contrário. <br/>Exemplo: GreaterThanOrEquals (salário, 100000)<br/> IsPresent. Este guia de função como um atributo no esquema ILM de entrada e devolve true se o atributo não for nulo e false se o atributo é nulo.|
| Operações  | Se **condição** avalia como VERDADEIRO, devolver **valueIfTrue.** Caso contrário, devolve **valueIfFalse.** <br/>Exemplo: IIF (Eq (EmployeeType, "Estagiário"), "t-" + Alias, Alias) devolve o alias de um utilizador com "t-" adicionado no início do mesmo se o utilizador for um estágio. Caso contrário, devolve o alias do utilizador conforme está. |
| Saída      | O resultado é **valueIfTrue** se a condição for true ou **valueIfFalse** se a condição for false. |      
