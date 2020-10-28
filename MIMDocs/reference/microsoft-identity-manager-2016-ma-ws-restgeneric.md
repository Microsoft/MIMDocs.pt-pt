---
title: Guia de fluxo de trabalho do Conector de Serviço Web para a API REST Microsoft Docs
description: Este artigo abrange como implantar uma amostra de API REST.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: e0c00972983d964a489d7c76e06e271bdf91b79e
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762402"
---
# <a name="web-service-connector-workflow-guide-for-a-rest-api-sample"></a>Guia de fluxo de fluxo de conector de serviço web para uma amostra de API REST

Este artigo cobre a implementação de uma amostra REST API para percorrer a ferramenta de Configuração do Serviço Web com uma Fonte de Dados Web REST API.

## <a name="prerequisites"></a>Pré-requisitos

São necessários os seguintes pré-requisitos para a utilização da amostra:

- A Ferramenta de Configuração do Serviço Web está instalada.
- O serviço de amostra de fonte de dados REST é implementado. Faça o download e instale a amostra a partir de (ver aqui).

<!-- No link provided for "see here" -->
<!-- Should Note go with bullet point #2 -->

>[!NOTE]
>Os dados do JSON devem conter um único objeto com uma propriedade que contenha uma matriz.

<!-- Should JSON be exactly as-is or just a sample -->
<!-- Should JSON be part of Note content -->
```JSON
{

"EmployeeList":[

{"id":"1","employee_name":"Albano","employee_salary":"22213","employee_age":"37","profile_image":""},{"id":"2","employee_name":"Albano","employee_salary":"22213","employee_age":"37","profile_image":""}

]

}
```

## <a name="configure-rest-project-discovery-in-the-web-service-configuration-tool"></a>Configure a descoberta do projeto REST na Ferramenta de Configuração do Serviço Web
Os passos a seguir mostram-lhe como criar um novo projeto para a sua fonte de dados na Ferramenta de Configuração do Serviço Web.

1. Ferramenta de configuração de serviço web aberta. Abre um projeto SOAP em branco.

   ![Ferramenta de configuração de serviço web](media/microsoft-identity-manager-2016-ma-ws-restgeneric/web-service-configuration-tool.png)

2. Selecione **Arquivo**  >  **Novo**  >  **Projeto REST** .

   ![Criar um novo projeto REST](media/microsoft-identity-manager-2016-ma-ws-restgeneric/new-project.png)
   <!-- Image shows SOAP project selected, not REST project -->

3. À esquerda, selecione **O Projeto REST** e, em seguida, selecione **Add** .

   ![Selecione o projeto REST](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-project.png)

4. Na página seguinte, forneça as seguintes informações:

   - O novo nome do serviço web
   - Endereço (caminho URL REST API)
   - Espaço de Nomes
   - Modo de segurança (tipo de autenticação)

   ![Serviço REST](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-service.png)
    
   O ecrã a seguir mostra exemplos para estes valores:
    
   ![Valores de exemplo para o Serviço REST](media/microsoft-identity-manager-2016-ma-ws-restgeneric/restsample.png)

   Definir o **modo de segurança** para _nenhum_ . Desa esta medida o **Endereço** para a amostra JSON Server que está hospedada no Azure.

5. Selecione **OK** . O projeto REST listado na Ferramenta de Configuração de Serviços Web.

   ![Projeto REST na Ferramenta de Configuração de Serviços Web](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-discovery.png)

6. O próximo passo é definir a chamada rest API e traduzir a chamada para as chamadas da Windows Communication Foundation (WCF).

   1. Expanda o **Projeto REST** e selecione o serviço _RESTSAMPLE._

   2. Selecione **Adicionar** . É solicitado a adicionar dois valores:
   
      ![Introduzir valores para o serviço REST](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-service-highlights.png)
      
      1. Insira o **Nome** . Este passo está rotulado como 3 na imagem.
      2. Insira o **Endereço** . Este passo está rotulado como 4 na imagem.
      3. Selecione **OK** . Um recurso REST é adicionado à descrição do serviço _RESTSAMPLE._

7. Na caixa **Recursos,** selecione o recurso REST que acaba de adicionar. Adicione o seguinte método:

   ![Adicione um método REST ao recurso](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-method.png)
   <!-- How does this dialog appear, by selecting Edit? -->

8. Selecione o método REST. Note que é possível criar múltiplos métodos no mesmo recurso e definir as consultas passadas durante a execução.

9. Para o método GETALL, não são necessárias consultas. Deixe os valores dos parâmetros em branco. Ao exportar ou importar a API REST, deve definir o Pedido de Amostra /ou Resposta dependendo da função. Copie e cole o retorno JSON ao navegar para esta amostra.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/rest-samples.png)

10. Selecione **Guardar** . Guarde o projeto `C:\Program Files\Microsoft Forefront Identity Manager\2010\Synchronization Service\Extensions` para. 

>[!NOTE]
>Após a salvação do projeto, o ficheiro WsConfig é gerado. O ficheiro de configuração contém vários ficheiros que são definidos anteriormente na visão geral do Serviço Web.


## <a name="configure-object-types-in-the-web-service-configuration-tool"></a>Configurar tipos de objetos na Ferramenta de Configuração de Serviço Web
Os seguintes passos mostram-lhe como configurar tipos de objetos para a sua fonte de dados na Ferramenta de Configuração do Serviço Web.

1. O próximo passo é definir o esquema espacial do conector. Isto é conseguido criando o Tipo de Objeto e definindo os seus tipos de objetos. Clique em **'Tipos de Objectos'** no painel esquerdo e clique no botão **Adicionar.** Fazer isto abre abaixo do ecrã. Adicione um novo tipo de objeto e forneça um nome. Clique no botão **OK.**

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/object-types.png)

2. A adição de um tipo de objeto fornece abaixo o ecrã.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/object-type-employee.png)

3. O painel direito correspondente ao tipo de objeto permite-lhe manter os atributos e as suas propriedades para o tipo de objeto selecionado. Clicar no botão Adicionar fornece abaixo o ecrã onde se pode adicionar atributos.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employeeid-string.png)

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-id-string.png)

4. Abaixo o ecrã aparece depois de adicionar todos os atributos necessários.

   ![](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-object-type-02.png)

5. O tipo de objeto e os atributos uma vez criados, fornecem fluxos de trabalho em branco que atendem às operações realizadas no Microsoft Identity Manager (MIM).


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
>Pode configurar estes fluxos de trabalho para o seu projeto ou descarregar o projeto padrão a partir do [Microsoft Download Center.](http://www.microsoft.com/download/details.aspx?id=29944)


### <a name="workflow-designer"></a>Designer de fluxo de trabalho
O Workflow Designer abre a área de trabalho para configurar o fluxo de trabalho conforme o requisito. Para cada tipo de objeto (novo /existente), a ferramenta de configuração fornece os nós para fluxos de trabalho que são suportados pela ferramenta. 

![Designer de fluxo de trabalho](media/microsoft-identity-manager-2016-ma-ws-restgeneric/full-import-configuration-workflow.png)

O Workflow Designer é composto pelos seguintes elementos de UI:

   - **Nós no painel esquerdo** : Estes ajudam-no a selecionar qual o fluxo de trabalho que pretende conceber.

   - **Central Workflow Designer** : Aqui pode deixar cair as atividades para configurar os fluxos de trabalho. Para realizar várias operações MIM (Exportação, Importação, Gestão de Passwords), pode utilizar as atividades de fluxo de trabalho padrão e personalizado do Quadro de Fluxo de Trabalho .NET 4. A ferramenta de configuração do serviço web utiliza atividades de fluxo de trabalho padrão e personalizadas. Para obter mais informações sobre as atividades padrão, consulte [utilizando designers de atividades.](http://msdn.microsoft.com/library/ee829528.aspx)

      - No Central Workflow Designer, um círculo vermelho com ponto de exclamação ao lado de qualquer atividade indica que a operação caiu e não está definida corretamente e completamente. Passe sobre o círculo vermelho para descobrir o erro exato. Após a definição correta da atividade, o círculo vermelho altera-se na marca de informação amarela.
      
      - No Central Workflow Designer, uma marca de informação de triângulo amarelo ao lado de qualquer atividade indica que a atividade está definida, mas há mais que pode fazer para completar a atividade. Paire sobre o triângulo amarelo para ver mais informações.

   - **Toolbox** : Embala todas as ferramentas, incluindo sistemas e atividades personalizadas e declarações predefinidas para conceber o fluxo de trabalho. Para mais informações, consulte [Toolbox](http://msdn.microsoft.com/library/aa480213.aspx).
   
   - **Secções Toolbox** : A Caixa de Ferramentas tem as seguintes secções e categorias:
   
      - **Descrição** : O cabeçalho da caixa de ferramentas. Um separador acede à Caixa de Ferramentas e às propriedades da atividade de fluxo de trabalho selecionado. 

      - **Fluxo de trabalho de importação** : Atividades personalizadas para configurar fluxos de trabalho de importação.
      
      - **Fluxo de trabalho de exportação** : Atividades personalizadas para configurar fluxos de trabalho de exportação.
      
      - **Comum** : Atividades personalizadas para configurar qualquer fluxo de trabalho.
      
      - **Debug** : Atividades de fluxo de trabalho do sistema para depurar definido no Fluxo de Trabalho 4. Estas atividades permitem o rastreio de emissão para um fluxo de trabalho.
      
      - **Declarações** : Atividades de fluxo de trabalho do sistema definidas no Fluxo de Trabalho 4. Para obter mais informações, consulte [utilizando designers de atividades.](http://msdn.microsoft.com/library/ee829528.aspx)

   - **Propriedades** : O separador de propriedades exibe as propriedades de uma determinada atividade de fluxo de trabalho que é largada na área do designer e selecionada. A figura à esquerda mostra as propriedades da atividade **de Atribuição.** Para cada atividade, as propriedades diferem e são utilizadas ao configurar o fluxo de trabalho personalizado. Este separador permite-lhe definir os atributos da ferramenta selecionada que foi deixada no designer central de fluxos de trabalho. Para mais informações, consulte [propriedades.](http://msdn.microsoft.com/library/ee342461.aspx)

   - **Barra de Tarefa:** A barra de tarefas inclui três elementos: **Variáveis,** **Argumentos** e **Importações.** Estes elementos são utilizados juntamente com atividades de fluxo de trabalho. Para obter mais informações, consulte [a introdução de um desenvolvedor à Windows Workflow Foundation (WF) em .NET 4](http://msdn.microsoft.com/library/ee342461.aspx).



## <a name="configure-a-full-import-workflow-in-the-web-service-configuration-tool"></a>Configure um fluxo de trabalho de importação completo na Ferramenta de Configuração do Serviço Web
Os passos a seguir mostram como configurar fluxos de trabalho completos de importação para a API REST utilizando a Ferramenta de Configuração do Serviço Web.

>[!WARNING]
>Esta amostra só cria um fluxo de trabalho. Podem ser necessárias modificações no fluxo de trabalho, tais como a utilização de lógica personalizada na API.

1. Selecione o fluxo de trabalho de Importação Completa para configurar. Os **Argumentos** e Importações já estão **definidos** e são específicos das atividades. Consulte os seguintes ecrãs para obter mais informações.

   ![Argumentos completos do fluxo de trabalho das importações](media/microsoft-identity-manager-2016-ma-ws-restgeneric/arguments.png)

   ![Espaços de nome importados](media/microsoft-identity-manager-2016-ma-ws-restgeneric/imported-name-spaces.png)

   Após a reconfiguração das chamadas, é necessário alterar os nomes dos atributos que mudam ou adicionar o espaço de nome a variáveis que se referem à estrutura de retorno da API e tipos de objetos que se referem ao antigo espaço de nome. A caixa de ferramentas no painel direito contém todas as atividades personalizadas específicas do fluxo de trabalho que necessita para a configuração. Atribua os valores às variáveis que vai utilizar para a sua lógica. Vá para a secção inferior do designer central de fluxos de trabalho e declare as variáveis. As variáveis são declaradas no próximo passo.

2. Adicione uma atividade de sequência. Arraste o designer de **atividades de sequência** da caixa de **ferramentas** e deixe-o cair na superfície do Windows Workflow Designer. Consulte os seguintes ecrãs. A atividade [sequência](https://msdn.microsoft.com/library/system.activities.statements.sequence.aspx) contém uma coleção ordenada de atividades infantis que executa por ordem.
   
    ![Atividade de sequência](media/microsoft-identity-manager-2016-ma-ws-restgeneric/imports.png)

3. Para adicionar uma variável, localizar **Criar Variável** . Tipo _wsResponse_ para o **Nome,** selecione o **tipo de paragem variável** e, em seguida, selecione Procurar para **tipos** . É apresentado um diálogo. Selecione **generated**  >  **GETALL**  >  **getall response** gerado . Mantenha os valores **de Âmbito** e **Predefinidos** não selecionados. Em alternativa, desa esta medida utilizando a vista **Propriedades.**

   ![Resposta padrão](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-list.png)

4. Arraste mais um designer de **atividades** de sequência da **Toolbox** dentro da atividade de sequência já adicionada.

5. Arraste um **WebServiceCallActivity** apresentado em **Common.** Esta atividade é usada para invocar a operação de serviço Web disponível após o Discovery. Esta é uma atividade personalizada e é comum em diferentes cenários de operação. 

    ![Operação de nome de serviço](media/microsoft-identity-manager-2016-ma-ws-restgeneric/full-import-operation-workflow.png)

   Para utilizar a operação de serviço Web, desemote as seguintes propriedades:
   
      - **Nome de serviço** : Introduza um nome para o serviço web.
      - **Nome do ponto final** : Especifique um nome de ponto final para o serviço selecionado.
      - **Operação Nome** : Especifique a respetiva operação para o serviço.
      - **Argumento** : Selecione **Argumentos** . No diálogo seguinte, atribua os valores do argumento, como mostrado na figura seguinte:
      
         ![Atribuir argumentos](media/microsoft-identity-manager-2016-ma-ws-restgeneric/get-all.png)

         >[!IMPORTANT]
         >Não altere o **Nome,** **Direção** ou **Tipo** para um argumento utilizando este diálogo. Se algum destes valores for alterado, a atividade torna-se inválida. Apenas definir o **Valor** para o argumento. Como mostrado neste valor, o valor *wsResponse* é definido.

6. Adicione uma atividade **ForEach** logo abaixo **do WebServiceCallActivity.** Esta atividade é usada para iterar sobre todos os atributos (âncoras e não-âncoras) do tipo de objeto. Ao arrastar esta atividade para a sua superfície workflow Designer, ele enumera automaticamente todos os nomes de atributos para o seu objeto. De definir os valores exigidos de acordo com o seguinte ecrã:

   ![Atividade de chamada de serviço web](media/microsoft-identity-manager-2016-ma-ws-restgeneric/foreach.png)

7. Em alguns casos, pode precisar de abrir o generated.dll que está dentro do ficheiro WsConfig. Copie este ficheiro WsConfig e rebatize-o com a extensão .zip. Abra e extraia o generated.dll utilizando a sua ferramenta refletora .NET preferida.

   ![Arquivo Config](media/microsoft-identity-manager-2016-ma-ws-restgeneric/config-files.png)

8. Identifique o espaço de nome público para a _Lista de Empregados:_

    ![Código da lista de empregados](media/microsoft-identity-manager-2016-ma-ws-restgeneric/employee-list-code.png)

    Em seguida, adicione este retorno ao fluxo de trabalho **ForEach** :

    ![Adicione a lista de empregados ao fluxo de trabalho forEach](media/microsoft-identity-manager-2016-ma-ws-restgeneric/foreach-employee-list.png)

9. Arraste uma atividade **CreateCSEntryChangeScope** dentro do corpo **forEach.** Esta atividade é usada para criar uma instância de objeto CSEntryChange no domínio do fluxo de trabalho para cada registo respetivo enquanto recupera dados da fonte de dados-alvo. Arrastar esta atividade fornece abaixo do ecrã. As atividades **createAnchorAttribute** são herdadas automaticamente. Atualize o valor **DN** para o seu nome de domínio preferido.

    ![Criar atividade de mudança de entrada CS](media/microsoft-identity-manager-2016-ma-ws-restgeneric/createcsentry.png)

    >[!NOTE]
    >Os valores de âncora e os nomes dos objetos variam de acordo com o serviço web exposto. A figura mostra um exemplo.

10. Arraste uma atividade **CreateAttributeChange** abaixo da atividade **CreateAnchorAttribute.** O número de atividades a arrastar é igual ao número de atributos não-âncora. Consulte a seguinte figura para referência.

    ![Criar âncora](media/microsoft-identity-manager-2016-ma-ws-restgeneric/create-anchor-attribute.png)

    >[!NOTE]
    >Para utilizar esta atividade, escolha e atribua os respetivos campos da queda e atribua os valores. Para atributos multivalorizados, deixe cair várias atividades **createValueChangeActivity** dentro de uma atividade **CreateAttributeChangeActivity.**

11. Guarde este projeto no `%FIM_INSTALL_FOLDER%\Synchronization Service\Extensions` local. Em seguida, configuure o Agente de Gestão como descrito na [configuração MA do Serviço Web](microsoft-identity-manager-2016-ma-ws-maconfig.md).

    ![Salve o projeto REST](media/microsoft-identity-manager-2016-ma-ws-restgeneric/sample-rest.png)
    
    Os projetos predefinidos devem ser descarregados e guardados no local `%FIM_INSTALL_DIR\2010\Synchronization Service\Extensions` do sistema alvo. Os projetos são então visíveis no assistente de conector de serviço web.
    
    Ao executar o ficheiro executável, é-lhe pedido que especifique a localização da instalação. Introduza o local de salvamento.
    
    >[!IMPORTANT]
    >O ficheiro do projeto pode ser guardado e aberto a partir de qualquer local (com os privilégios de acesso adequados do seu executor). Apenas os ficheiros de projeto que são guardados `Synchronization Service\Extension` na pasta podem ser selecionados no assistente de conector do Serviço Web que é acedido através da UI de sincronização MIM.
    
    O utilizador que está a executar a ferramenta de Configuração do Serviço Web requer os seguintes privilégios:
    
       - Controlo total da pasta de extensão do serviço de sincronização.
       - Leia o acesso à chave de registo `HKLM\System\CurrentControlSet\Services\FIMSynchronizationService\Parameters` através da qual se encontra o caminho da pasta de extensão.


## <a name="next-steps"></a>Passos seguintes

- [Visão geral do conector genérico do serviço web](microsoft-identity-manager-2016-ma-ws.md)
- [Instale a Ferramenta de Configuração do Serviço Web](microsoft-identity-manager-2016-ma-ws-install.md)
- [Guia de implementação soap](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Guia de implantação do REST](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Configuração MA do Serviço Web](microsoft-identity-manager-2016-ma-ws-maconfig.md)
