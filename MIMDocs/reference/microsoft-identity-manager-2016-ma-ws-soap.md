---
title: Guia de fluxo de trabalho do conector do serviço web para | SOAP Microsoft Docs
description: Este artigo descreve como criar um novo projeto para a sua fonte de dados SOAP utilizando a Ferramenta de Configuração do Serviço Web.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/30/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 455988722b3aeb9e29b00696342e1800e9ad82c7
ms.sourcegitcommit: 78f3f18f0b7afb44fcf7444e446a4edffb1f8f12
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835976"
---
# <a name="web-service-connector-workflow-guide-for-soap"></a>Guia de fluxo de trabalho do conector do serviço web para SOAP

Este artigo descreve como criar um novo projeto para a sua fonte de dados na Ferramenta de Configuração do Serviço Web. Siga estes passos para criar um projeto.

1.  Abra a ferramenta de configuração do serviço web. Abre um projeto em branco.

    ![Ferramenta de configuração de serviço web](media/microsoft-identity-manager-2016-ma-ws-soap/web-service-configuration-tool-01.png)

2.  Selecione **SOAP Project** e, em seguida, selecione **Adicionar**.

    ![Projeto SOAP](media/microsoft-identity-manager-2016-ma-ws-soap/web-service-configuration-tool-02.png)

3.  Na página seguinte, forneça as seguintes informações e, em seguida, selecione **Seguinte**:

    - O novo nome do serviço web
    - Endereço (caminho WSDL) para recuperar os serviços expostos, pontos finais e operações
    - Espaço de Nomes
    - Modo de segurança (tipo de autenticação)
  
4.  Nesta amostra, a página **Credenciais** é mostrada com os requisitos para o modo de segurança *Básico* (o modo que foi selecionado no passo anterior). Se "Nenhum" fosse especificado para o modo de segurança, então uma página de Credenciais não seria exibida. Selecione **Seguinte**.

    ![Ecrã de serviço SOAP com nome de utilizador e senha](media/microsoft-identity-manager-2016-ma-ws-soap/soap-service.png)

5.  O caminho WSDL está a ser acedido para recuperar a informação de serviço e a lista de funções expostas é apresentada. Se o caminho WSDL introduzido estiver incorreto, a ferramenta de configuração não consegue recuperar as informações de serviço e lança um erro.

    ![serviço web baixar ecrã de progresso](media/microsoft-identity-manager-2016-ma-ws-soap/web-service-progress.png)

6.  Uma vez realizada a descoberta, então ele lista o ponto final e as operações que são descobertas. Selecione **Concluir**.

    ![Pontos finais e operações de serviço SOAP descobertos](media/microsoft-identity-manager-2016-ma-ws-soap/soap-service-endpoints.png)

7.  A compilação é realizada. A compilação é um processo de compilação da montagem do contrato de dados, o que pode ser uma operação morosa. O utilizador é informado sobre eventuais erros de compilação. Após a descoberta, a ferramenta apresenta a seguinte página:

    ![Descoberta de sabão](media/microsoft-identity-manager-2016-ma-ws-soap/soap-discovery.png)

8.  Expandir o **projeto SOAP** e selecionar o ponto final exposto fornecido abaixo do ecrã. Este ecrã lista as operações declaradas no ponto final.

    ![Operações declaradas no ponto final](media/microsoft-identity-manager-2016-ma-ws-soap/basic-http-binding.png)

9.  O ponto final de expansão apresenta a lista de operações. Uma operação é uma função declarada por um Ponto Final. Cada operação aborda um tipo de tarefa que pode ser executada dentro do serviço. Este ecrã lista os argumentos declarados para a operação. Estes argumentos são então definidos quando a operação é utilizada na configuração dos fluxos de trabalho.

    ![Pontos finais expandidos](media/microsoft-identity-manager-2016-ma-ws-soap/get-employee-byid.png)

10. O próximo passo é definir o esquema de espaço do conector, que é conseguido criando o Tipo de Objeto e definindo os seus tipos de objetos. Selecione **Os Tipos de Objetos** e, em seguida, selecione **Adicionar**. Na nova janela, adicione um novo tipo de objeto e forneça um nome. Selecione **OK**.

    ![Definição do tipo de objeto](media/microsoft-identity-manager-2016-ma-ws-soap/object-types.png)

11. A adição de um tipo de objeto fornece abaixo o ecrã.

    ![visualização do tipo de objeto recém-criado](media/microsoft-identity-manager-2016-ma-ws-soap/object-type-employee.png)

12. O painel direito correspondente ao tipo de objeto permite-lhe manter os atributos e as suas propriedades para o tipo de objeto selecionado. Selecione **Adicionar**. Abre-se uma nova janela para adicionar atributos:

    ![Atributo e tipo de dados](media/microsoft-identity-manager-2016-ma-ws-soap/object-type-firstname.png)

    ![Atributo e tipo de dados com opção Âncora selecionada](media/microsoft-identity-manager-2016-ma-ws-soap/employeeid-string.png)

13. O seguinte ecrã aparece depois de adicionar todos os atributos necessários:

    ![Tipo de objeto com informação de atributo](media/microsoft-identity-manager-2016-ma-ws-soap/soap-project.png)

14. O tipo de objeto e os atributos uma vez criados, fornecem fluxos de trabalho em branco que atendem às operações realizadas no Microsoft Identity Manager 2016 (MIM).

    ![Object Types mostra operações que o funcionário pode realizar](media/microsoft-identity-manager-2016-ma-ws-soap/object-types-operations.png)


## <a name="configure-workflows-in-the-web-service-configuration-tool"></a>Configure fluxos de trabalho na Ferramenta de Configuração do Serviço Web

O próximo passo é configurar os fluxos de trabalho para o tipo de objeto. Os ficheiros workflow são uma série de atividades que são usadas pelo Conector de Serviços Web no tempo de execução. Os fluxos de trabalho são utilizados para implementar a operação MIM adequada. A Ferramenta de configuração do Serviço Web ajuda-o a criar quatro fluxos de trabalho diferentes:

- Importação: Dados relativos às importações a partir de uma fonte de dados para os dois tipos seguintes de fluxos de trabalho:

    - Importação completa: Uma importação completa que pode ser configurada.
    - Importação Delta: Não suportado pela Ferramenta de Configuração do Serviço Web.

- Exportação: Dados de exportação da MIM para uma fonte de dados ligada. As três ações seguintes são apoiadas para a operação. Pode configurar estas ações com base nas suas necessidades.

    - Adicionar
    - Eliminar
    - Substituir

- Palavra-passe: Realizar a gestão da palavra-passe para o utilizador (tipo de objeto). Estão disponíveis duas ações para esta operação:

    - Definir palavra-passe
    - Alterar palavra-passe

- Ligação de teste: Configure um fluxo de trabalho para verificar se a ligação com o servidor de fonte de dados está estabelecida com sucesso.

>[!NOTE]
>Pode configurar estes fluxos de trabalho para o seu projeto ou descarregar o projeto padrão a partir do [Microsoft Download Center.](https://www.microsoft.com/download/details.aspx?id=29944)


### <a name="workflow-designer"></a>Designer de fluxo de trabalho
O Workflow Designer abre a área de trabalho para configurar o fluxo de trabalho conforme o requisito. Para cada tipo de objeto (novo /existente), a ferramenta de configuração fornece os nós para fluxos de trabalho que são suportados pela ferramenta. 

![Designer de fluxo de trabalho](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-workflow.png)

O Workflow Designer é composto pelos seguintes elementos de UI:

   - **Nós no painel esquerdo**: Estes ajudam-no a selecionar qual o fluxo de trabalho que pretende conceber.

   - **Central Workflow Designer**: Aqui pode deixar cair as atividades para configurar os fluxos de trabalho. Para realizar várias operações MIM (Exportação, Importação, Gestão de Passwords), pode utilizar as atividades de fluxo de trabalho padrão e personalizado do Quadro de Fluxo de Trabalho .NET 4. A ferramenta de configuração do serviço web utiliza atividades de fluxo de trabalho padrão e personalizadas. Para obter mais informações sobre as atividades padrão, consulte [utilizando designers de atividades.](https://msdn.microsoft.com/library/ee829528.aspx)

      - No Central Workflow Designer, um círculo vermelho com ponto de exclamação ao lado de qualquer atividade indica que a operação caiu e não está definida corretamente e completamente. Passe sobre o círculo vermelho para descobrir o erro exato. Após a definição correta da atividade, o círculo vermelho altera-se na marca de informação amarela.
      
      - No Central Workflow Designer, uma marca de informação de triângulo amarelo ao lado de qualquer atividade indica que a atividade está definida, mas há mais que pode fazer para completar a atividade. Paire sobre o triângulo amarelo para ver mais informações.

   - **Toolbox**: Embala todas as ferramentas, incluindo sistemas e atividades personalizadas e declarações predefinidas para conceber o fluxo de trabalho. Para mais informações, consulte [Toolbox](https://msdn.microsoft.com/library/aa480213.aspx).
   
   - **Secções Toolbox**: A Caixa de Ferramentas tem as seguintes secções e categorias:
   
      - **Descrição**: O cabeçalho da caixa de ferramentas. Um separador acede à Caixa de Ferramentas e às propriedades da atividade de fluxo de trabalho selecionado. 

      - **Fluxo de trabalho de importação**: Atividades personalizadas para configurar fluxos de trabalho de importação.
      
      - **Fluxo de trabalho de exportação**: Atividades personalizadas para configurar fluxos de trabalho de exportação.
      
      - **Comum**: Atividades personalizadas para configurar qualquer fluxo de trabalho.
      
      - **Debug**: Atividades de fluxo de trabalho do sistema para depurar definido no Fluxo de Trabalho 4. Estas atividades permitem o rastreio de emissão para um fluxo de trabalho.
      
      - **Declarações**: Atividades de fluxo de trabalho do sistema definidas no Fluxo de Trabalho 4. Para obter mais informações, consulte [utilizando designers de atividades.](https://msdn.microsoft.com/library/ee829528.aspx)            

   - **Propriedades**: O separador de propriedades exibe as propriedades de uma determinada atividade de fluxo de trabalho que é largada na área do designer e selecionada. A figura à esquerda mostra as propriedades da atividade **de Atribuição.** Para cada atividade, as propriedades diferem e são utilizadas ao configurar o fluxo de trabalho personalizado. Este separador permite-lhe definir os atributos da ferramenta selecionada que foi deixada no designer central de fluxos de trabalho. Para mais informações, consulte [propriedades.](https://msdn.microsoft.com/library/ee342461.aspx)

   - **Barra de Tarefa:** A barra de tarefas inclui três elementos: **Variáveis,** **Argumentos** e **Importações.** Estes elementos são utilizados juntamente com atividades de fluxo de trabalho. Para obter mais informações, consulte [a introdução de um desenvolvedor à Windows Workflow Foundation (WF) em .NET 4](https://msdn.microsoft.com/library/ee342461.aspx).


<h2 id="full-import-workflows">Configure um fluxo de trabalho de importação completo na Ferramenta de Configuração do Serviço Web</h2>
Os passos a seguir mostram como configurar fluxos de trabalho de importação completos para SOAP utilizando a Ferramenta de Configuração do Serviço Web.

>[!WARNING]
>Esta amostra só cria um fluxo de trabalho. Podem ser necessárias modificações no fluxo de trabalho, tais como a utilização de lógica personalizada na API.

1. Selecione o fluxo de trabalho de Importação Completa para configurar. Os **Argumentos** e Importações já estão **definidos** e são específicos das atividades. Consulte os seguintes ecrãs para obter mais informações.

   ![Argumentos completos do fluxo de trabalho das importações](media/microsoft-identity-manager-2016-ma-ws-soap/arguments.png)
 
   ![Espaços de nome importados](media/microsoft-identity-manager-2016-ma-ws-soap/imports.png)

   Após a reconfiguração das chamadas, altere os nomes dos atributos que mudam, adicionam ou alteram o espaço de nome para variáveis que se referem à estrutura de retorno da API e tipos de objetos que se referem ao antigo espaço de nome. A caixa de ferramentas no painel direito contém todas as atividades personalizadas específicas do fluxo de trabalho que necessita para a configuração. Atribua os valores às variáveis que vai utilizar para a sua lógica. Vá para a secção inferior do designer central de fluxos de trabalho e declare as variáveis. As variáveis são declaradas no próximo passo.

2. Adicione uma atividade de sequência. Arraste o designer de **atividades de sequência** da caixa de **ferramentas** e deixe-o cair na superfície do Windows Workflow Designer. Consulte os seguintes ecrãs. A atividade [sequência](https://msdn.microsoft.com/library/system.activities.statements.sequence.aspx) contém uma coleção ordenada de atividades infantis que executa por ordem.
   
    ![Atividade de sequência](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-sequence.png)

3. Para adicionar uma variável, localizar **Criar Variável**. Tipo _wsResponse_ para o **Nome,** selecione o **tipo de paragem variável** e, em seguida, selecione Procurar para **tipos**. É apresentado um diálogo. Selecione resposta   >  **padrão** gerada  >  . Mantenha os valores **de Âmbito** e **Predefinidos** não selecionados. Em alternativa, desa esta medida utilizando a vista **Propriedades.**

   ![Resposta padrão](media/microsoft-identity-manager-2016-ma-ws-soap/default-response.png)

   ![Propriedades de importação completas](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-properties.png)

4. Agora adicione todas as outras variáveis e abaixo está o ecrã final.

   ![Variáveis de importação completas](media/microsoft-identity-manager-2016-ma-ws-soap/full-import-variables.png)

5. Arraste mais um designer de **atividades** de sequência da **Toolbox** dentro da atividade de sequência já adicionada.

6. Arraste um **WebServiceCallActivity** apresentado em **Common.** Esta atividade é usada para invocar a operação de serviço Web disponível após o Discovery. Esta é uma atividade personalizada e é comum em diferentes cenários de operação. 

    ![Operação de nome de serviço](media/microsoft-identity-manager-2016-ma-ws-soap/service-name-operation.png)

   Para utilizar a operação de serviço Web, desemote as seguintes propriedades:
   
      - **Nome de serviço**: Introduza um nome para o serviço web.
      - **Nome do ponto final**: Especifique um nome de ponto final para o serviço selecionado.
      - **Operação Nome**: Especifique a respetiva operação para o serviço.
      - **Argumento**: Selecione **Argumentos**. No diálogo seguinte, atribua os valores do argumento, como mostrado na figura seguinte:
      
         ![Atribuir argumentos](media/microsoft-identity-manager-2016-ma-ws-soap/get-employeebyid.png)

         >[!IMPORTANT]
         >Não altere o **Nome,** **Direção** ou **Tipo** para um argumento utilizando este diálogo. Se algum destes valores for alterado, a atividade torna-se inválida. Apenas definir o **Valor** para o argumento. Como mostrado neste valor, o valor *wsResponse* é definido.

7. Adicione uma atividade **ForEach** logo abaixo **do WebServiceCallActivity.** Esta atividade é usada para iterar sobre todos os atributos (âncoras e não-âncoras) do tipo de objeto. Ao arrastar esta atividade para a sua superfície workflow Designer, ele enumera automaticamente todos os nomes de atributos para o seu objeto. De definir os valores exigidos de acordo com o seguinte ecrã:

   ![Atividade de chamada de serviço web](media/microsoft-identity-manager-2016-ma-ws-soap/webservicecallactivity.png)

8. Arraste uma atividade **CreateCSEntryChangeScope** dentro do corpo **forEach.** Esta atividade é usada para criar uma instância de objeto CSEntryChange no domínio do fluxo de trabalho para cada registo respetivo enquanto recupera dados da fonte de dados-alvo. Arrastar esta atividade fornece abaixo do ecrã. As atividades **createAnchorAttribute** são herdadas automaticamente.

    ![Criar atividade de mudança de entrada CS](media/microsoft-identity-manager-2016-ma-ws-soap/createcsentrychangescope.png)

9.  Desateia o valor da expressão DN como `‘string.Concat ("Employee",item.EmployeeID)’` . Desaprova o **AnchorValue** para o _EmployeeID_ para **'Converter.tostring(item. EmployeeID)'.** Desajei o **Nome de ObjectType** como _Empregado._ Depois de escamar estas modificações, verá o seguinte ecrã:

    ![Obter a iD do empregado](media/microsoft-identity-manager-2016-ma-ws-soap/get-employeebyid.png)

    >[!NOTE]
    >Os valores de âncora e os nomes dos objetos variam de acordo com o serviço web exposto. A figura mostra um exemplo.

10. Arraste uma atividade **CreateAttributeChange** abaixo da atividade **CreateAnchorAttribute.** O número de atividades a arrastar é igual ao número de atributos não-âncora. Consulte a seguinte figura para referência.

    ![Criar âncora](media/microsoft-identity-manager-2016-ma-ws-soap/create-anchor.png)

11. Arraste **a CreateValueChangeActivity** dentro da atividade **CreateAttributeChange** e desempate o valor do atributo de acordo com o ecrã abaixo.

    ![Alterar um atributo](media/microsoft-identity-manager-2016-ma-ws-soap/attribute-change.png)

    >[!NOTE]
    >Para utilizar esta atividade, escolha e atribua os respetivos campos da queda e atribua os valores. Para atributos multivalorizados, deixe cair várias atividades **createValueChangeActivity** dentro de uma atividade **CreateAttributeChangeActivity.**

12. Para adicionar condições para um atributo, adicione uma atividade **Se** como mostrado na seguinte figura:

    ![Se a atividade](media/microsoft-identity-manager-2016-ma-ws-soap/if.png)

13. Por fim, adicione uma atividade **de Atribuição** e desatrei a expressão, como mostra a seguinte figura:

    ![Atribua atividade e definir a expressão](media/microsoft-identity-manager-2016-ma-ws-soap/attribute-change-email.png)

14. Guarde este projeto no `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` local.
    
    Os projetos predefinidos devem ser descarregados e guardados no local `%FIM_INSTALL_DIR\2010\Synchronization Service\Extensions` do sistema alvo. Os projetos são então visíveis no assistente de conector de serviço web.
    
    Ao executar o ficheiro executável, é-lhe pedido que especifique a localização da instalação. Introduza o local de salvamento.
    
    >[!IMPORTANT]
    >O ficheiro do projeto pode ser guardado e aberto a partir de qualquer local (com os privilégios de acesso adequados do seu executor). Apenas os ficheiros de projeto que são guardados `Synchronization Service\Extension` na pasta podem ser selecionados no assistente de conector do Serviço Web que é acedido através da UI de sincronização MIM.
    
    O utilizador que está a executar a ferramenta de Configuração do Serviço Web requer os seguintes privilégios:
    
       - Controlo total da pasta de extensão do serviço de sincronização.
       - Leia o acesso à chave de registo `HKLM\System\CurrentControlSet\Services\FIMSynchronizationService\Parameters` através da qual se encontra o caminho da pasta de extensão.


## <a name="configure-export-workflows-in-the-web-service-configuration-tool"></a>Configure fluxos de trabalho de exportação na Ferramenta de Configuração do Serviço Web
As secções seguintes mostram como exportar os seus fluxos de trabalho utilizando a Ferramenta de Configuração do Serviço Web.

<h3 id="attribute-change-anchor">Adicionar fluxos de trabalho</h3>
Adicione fluxos de trabalho de exportação seguindo estes passos na Ferramenta de Configuração do Serviço Web.

1. Selecione o fluxo de trabalho de exportação para configurar. Em **Exportação**, **selecione Add**. Os **Argumentos** e Importações já estão **definidos** e são específicos das atividades. Consulte os seguintes ecrãs para obter referência.

    ![Adicionar](media/microsoft-identity-manager-2016-ma-ws-soap/add.png)

2. Adicione uma atividade **de sequência.** Arraste o designer de **atividades de sequência** da caixa de **ferramentas** e deixe-o cair na superfície do Windows Workflow Designer. A atividade [sequência](https://msdn.microsoft.com/library/system.activities.statements.sequence.aspx) contém uma coleção ordenada de atividades infantis que executa por ordem. Selecione **Criar Variável**. Atribua os valores às variáveis que vai usar para a sua lógica.

    ![Exportar](media/microsoft-identity-manager-2016-ma-ws-soap/export-add.png)

    >[!NOTE]
    >Os passos para adicionar uma variável são descritos na secção para a <a href="#full-import-workflows">criação de fluxos de trabalho completos de importação</a>.

3. Arraste uma atividade **ForEach** dentro da atividade de **sequência** já adicionada para iterar sobre valores de atributos de âncora.

4. Selecione **Propriedades** e desa esta **medida** de acordo com o ecrã abaixo. Aqui **o objectToExport** é argumento.

    ![Definir as propriedades para a atividade ForEach](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-sequence.png)

5. Definir **DisplayName** como **\<AnchorAttribute\> ForEach**

   ![Definir o nome do visor](media/microsoft-identity-manager-2016-ma-ws-soap/add-sequence.png)

6. Definir **TypeArgument** como `Microsoft.MetadirectoryServices.AnchorAttribute` .

   ![Definir o argumento do tipo](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-anchor.png)

7. Adicione uma atividade **switch** dentro do corpo **forEach** do **AnchorAttribute**.

   ![Adicione uma atividade switch](media/microsoft-identity-manager-2016-ma-ws-soap/select-net-types.png)

8. Adicione uma expressão de acordo com o ecrã abaixo.

   ![Adicione uma expressão](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch.png)

9. **Selecione Adicione um novo caso** e introduza um valor para o **EmployeeId**. Arraste uma atividade **de sequência** e dentro dela adicione uma atividade **De atribuir.**

    ![Adicione um novo caso e atribua-o à sequência](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-employeeid.png)

10. Atribuir as propriedades **de To** **and Value** para a atividade **Atribuir.**

    ![Atribuir as propriedades de To and Value](media/microsoft-identity-manager-2016-ma-ws-soap/anchor-attribute-name.png)

11. A atividade **ForEach** é utilizada para valores de ancoragem. Adicione outra atividade **ForEach** para atribuir valores não-âncora. Neste exemplo, é utilizada a âncora **AttributeChange.**

    ![Adicione mais uma atividade forEach com a âncora AttributeChange](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-change.png)

12. Adicione uma atividade **switch** dentro do corpo **forEach** da âncora **AttributeChange.**   

    ![Adicionar atividade switch para a âncora AttributeChange](media/microsoft-identity-manager-2016-ma-ws-soap/attribute-name-wrapper.png)

13. Adicione uma expressão de acordo com o ecrã abaixo.

    ![Adicione uma expressão para a atividade do interruptor](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-expression.png)

14. **Selecione Adicione um novo caso** e introduza um valor para o Primeiro **Nome**. Arraste uma atividade **de sequência** e dentro dela adicione uma atividade **De atribuir.** Atribuir as propriedades **de To** **and Value** para a atividade **Atribuir.**

    ![Adicione um novo caso para a sequência](media/microsoft-identity-manager-2016-ma-ws-soap/switch-firstname.png)

15. Adicione valores para os atributos necessários como **LastName**, **Email,** e assim por diante. 

    ![Adicionar valores para atributos necessários](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch-anchor.png)

16. Em **Comum,** arraste um **WebServiceCallActivity** e desconte **valores** para os seus **argumentos.**

    ![Adicione uma atividade de chamada de Serviço Web e desa estalem os valores](media/microsoft-identity-manager-2016-ma-ws-soap/add-employee-attribute.png)

    >[!IMPORTANT]
    >Não altere o **Nome,** **Direção** ou **Tipo** para um argumento utilizando este diálogo. Se algum destes valores for alterado, a atividade torna-se inválida. Apenas definir o **Valor** para o argumento. Como mostrado neste valor, o valor *wsResponse* é definido.

17.  Por fim, adicione uma atividade **Se** para verificar as respostas que são devolvidas da operação de serviço web.

A criação do fluxo de trabalho de exportação com a operação **Add** está completa:

![Fluxo de trabalho de exportação concluído](media/microsoft-identity-manager-2016-ma-ws-soap/create-csentry-change.png)

Guarde este projeto no `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` local.


### <a name="delete-workflows"></a>Eliminar fluxos de trabalho
Elimine os fluxos de trabalho de exportação seguindo estes passos na Ferramenta de Configuração do Serviço Web.

1. Selecione o fluxo de trabalho de exportação para configurar. Em **Exportação**, **selecione Delete**. Os **Argumentos** e Importações já estão **definidos** e são específicos das atividades. Consulte os seguintes ecrãs para obter referência.

   ![Exportação eliminar fluxos de trabalho](media/microsoft-identity-manager-2016-ma-ws-soap/export-delete.png)

2. Adicione uma atividade **de sequência.** Selecione **Criar Variável**. Atribua os valores às variáveis que vai utilizar para a sua lógica.

   ![Adicione uma atividade de sequência](media/microsoft-identity-manager-2016-ma-ws-soap/sequence-variables.png)

   >[!NOTE]
   >Os passos para adicionar uma variável são descritos na secção para a <a href="#full-import-workflows">criação de fluxos de trabalho completos de importação</a>.

3. Arraste uma atividade **ForEach** dentro da atividade de **sequência** já adicionada para iterar sobre valores de atributos de âncora.

4. Selecione **Propriedades** e desa estale os **Valores** por ecrã abaixo. Aqui **o objectToExport** é argumento.

   ![Definir as propriedades para a atividade ForEach](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-object-to-export.png)

5. Desabine o **Nome do Visor** como `ForEach\<AnchorAttribute\>` :

   ![Definir o nome do visor](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-anchor-type.png)

6. Desagure o **TipoArgument** `Microsoft.MetadirectoryServices.AnchorAttribute` como: 

   ![Definir o argumento do tipo](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-type-argument.png)

7. Adicione uma atividade **switch** dentro do corpo **forEach** do **AnchorAttribute**.

   ![Adicione uma atividade switch](media/microsoft-identity-manager-2016-ma-ws-soap/select-net-type.png)

8. Adicione uma expressão de acordo com o ecrã abaixo.

   ![Adicione uma expressão](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch.png)

9. **Selecione Adicione um novo caso** e introduza um valor para o **EmployeeId**. Arraste uma atividade **de sequência** e dentro dela adicione uma atividade **De atribuir.**

   ![Adicione um novo caso e atribua-o à sequência](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-default.png)

10. Atribuir as propriedades **de To** **and Value** para a atividade **Atribuir.**

    ![Atribuir as propriedades de To and Value](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-attribute-flow.png)

11. Em **Comum,** arraste um **WebServiceCallActivity** e desconte **valores** para os seus **argumentos.**

    ![Adicione uma atividade de chamada de Serviço Web e desa estalem os valores](media/microsoft-identity-manager-2016-ma-ws-soap/delete-employee.png)

    >[!IMPORTANT]
    >Não altere o **Nome,** **Direção** ou **Tipo** para um argumento utilizando este diálogo. Se algum destes valores for alterado, a atividade torna-se inválida. Apenas definir o **Valor** para o argumento. Como mostrado neste valor, o *empregado de valorID* é definido.

12. Por fim, adicione uma **atividade Se** verificar as respostas devolvidas da operação de serviço web.

A supressão do fluxo de trabalho de exportação com a operação **Eliminar** está concluída:

![Fluxo de trabalho de exportação eliminado](media/microsoft-identity-manager-2016-ma-ws-soap/create-csentry-change.png)

<!-- Image of completed Delete operation is missing from document -->

Guarde este projeto no `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` local.


### <a name="replace-workflows"></a>Substituir fluxos de trabalho
Substitua os fluxos de trabalho de exportação seguindo estes passos na Ferramenta de Configuração do Serviço Web.

1. Selecione o fluxo de trabalho de exportação para configurar. Em **Exportação**, selecione **Substitua.** Os **Argumentos** e Importações já estão **definidos** e são específicos das atividades. Consulte abaixo o ecrã para obter referência.

   ![Substitua um fluxo de trabalho](media/microsoft-identity-manager-2016-ma-ws-soap/replace.png)

2. Adicione uma atividade **de sequência.**

3. Arraste uma atividade **forEach** para o **\<AnchorAttribute> .**

4. Adicione outra atividade **ForEach \<AttributeChange>** para atribuir valores não-âncora.

5. Finalmente, o ecrã parece a seguinte figura. As instruções para a configuração desta atividade são fornecidas na secção para <a href="#attribute-change-anchor">a adição de fluxos de trabalho de exportação</a>.

   ![ForEach com uma atividade switch e atributo âncora](media/microsoft-identity-manager-2016-ma-ws-soap/foreach-switch-anchor.png)

6. Em **Comum,** arraste um **WebServiceCallActivity** e desconte **valores** para os seus **argumentos.**

   ![Adicione uma atividade de chamada de Serviço Web e desa estalem os valores](media/microsoft-identity-manager-2016-ma-ws-soap/wsresponse.png)

   >[!IMPORTANT]
   >Não altere o **Nome,** **Direção** ou **Tipo** para um argumento utilizando este diálogo. Se algum destes valores for alterado, a atividade torna-se inválida. Apenas definir o **Valor** para o argumento. Como mostrado neste valor, o *trabalhador de* valor é definido.

7. Por fim, adicione uma atividade **Se** para verificar as respostas que são devolvidas da operação de serviço web.

A substituição do fluxo de trabalho de exportação pela operação **Substituir** está concluída:

![Substituir o fluxo de trabalho de exportação](media/microsoft-identity-manager-2016-ma-ws-soap/operation-name-update-employee.png)

Guarde este projeto no `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` local.


## <a name="debug-activities"></a>Atividades de depurar
As seguintes atividades personalizadas estão disponíveis para ajudar a depurar o modelo de fluxo de trabalho.

### <a name="log-activity"></a>Atividade de log
A atividade **'Log'** é utilizada para escrever mensagens de texto no ficheiro de registo. Para mais informações, consulte ['Registar'.](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx)

>[!NOTE]
>Se não conseguir depurar facilmente o seu fluxo de trabalho, tente depurar o fluxo de trabalho no ambiente de produção.  

Para utilizar a atividade **Log,** desa estale as seguintes propriedades. As propriedades são visíveis quando seleciona a atividade no Workflow Designer e vê as **Propriedades** para a atividade.
<!-- Properties missing from document -->
<!-- Image of Log activity GUI missing from document -->

### <a name="writeline-activity"></a>Atividade writeLine
A atividade **WriteLine** é usada para escrever mensagens de texto ao escritor de um fornecedor. Se não houver nenhum escritor disponível, a atividade [WriteLine](http://127.0.0.1:47873/help/1-7016/ms.help?method=page&id=T%3ASYSTEM.ACTIVITIES.STATEMENTS.WRITELINE&product=VS&productVersion=100&topicVersion=100&locale=EN-US&topicLocale=EN-US&embedded=true) escreve o texto para a janela da consola.

<!-- Image of WriteLine activity GUI missing from document -->

Na caixa de texto, escreva a mensagem de que quer ser visível no alvo do escritor.

>[!IMPORTANT]
>A janela da consola não pode ser usada para esta atividade. Use outro autor de saída de janela para esta tarefa.

Para utilizar a atividade **WriteLine,** descreva as seguintes propriedades. As propriedades são visíveis quando seleciona a atividade no Workflow Designer e vê as **Propriedades** para a atividade.

- **Nível de registo**: Especifica a quantidade de conteúdo para escrever no valor do registo. Os valores possíveis são:

    - Alto: Escreva a mensagem **LogText** no ficheiro de registo se a gravidade do registo estiver definida para Alta.
    - Verbose: Escreva a mensagem **LogText** no ficheiro de registo se a gravidade do registo estiver definida para Verbose.
    - Desativado: Não escreva no ficheiro de registo.
- **LogText**: Especifica o conteúdo do texto para escrever no registo.
- **Tag**: Adiciona uma etiqueta ao texto para identificar o tipo de conteúdo que está a ser escrito no registo. Os valores possíveis são: Erro, rastreio ou aviso.

<!-- log severity is not defined in this document -->


## <a name="next-steps"></a>Passos seguintes

- [Visão geral do conector genérico do serviço web](microsoft-identity-manager-2016-ma-ws.md)
- [Instale a Ferramenta de Configuração do Serviço Web](microsoft-identity-manager-2016-ma-ws-install.md)
- [Guia de implementação soap](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Guia de implantação do REST](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Configuração MA do Serviço Web](microsoft-identity-manager-2016-ma-ws-maconfig.md)
