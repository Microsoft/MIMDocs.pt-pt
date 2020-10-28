---
title: Amostra do Serviço de Aplicações API do Serviço de Aplicações da Web Service REST Microsoft Docs
description: Guia ajudando-o a implementar uma amostra de servidor REST JSON em Azure
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/28/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: deb743fcbe4bdd155c1b0c4a31e24af0e8da8a4e
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762401"
---
# <a name="web-service-connector-rest-api-app-service-sample"></a>Amostra do Serviço de Aplicações API do Serviço de Aplicações do Serviço de Aplicações do Serviço de Aplicações DO Serviço de Serviços de API do

Este guia de implementação irá ajudá-lo a implementar o servidor REST JSON da amostra para Azure. Pode utilizar esta amostra para o ajudar na configuração e compreensão do Conector de Serviço Web.

- A amostra requer Microsoft Visual Studio 2017.
- Os pacotes Native Module Path (NMP) na amostra utilizam este [servidor JSON](https://github.com/typicode/JSON-server) no GitHub.
- Descarregue o código de [amostra](https://github.com/fimguy/SAMPLEREST) do GitHub e transfira o código de amostra para o Serviço de Aplicações Azure.

## <a name="deploy-the-json-server-sample"></a>Implementar a amostra do servidor JSON

1. Depois de o código ser descarregado, abra o ficheiro de solução no [Visual Studio 2017](https://www.visualstudio.com/downloads/).

2. Implemente a solução selecionando o projeto, em seguida, clique à direita e **selecione Publicar** :

    ![Publicar a solução](media/microsoft-identity-manager-2016-ma-ws-restsample/publish-project.png)

3. Selecione o Serviço de Aplicações a utilizar para a implementação:

    ![Selecione o Serviço de Aplicações](media/microsoft-identity-manager-2016-ma-ws-restsample/app-service.png)

4. Selecione um Grupo de Recursos existente ou crie um novo Grupo de Recursos:

    ![Selecione um Grupo de Recursos](media/microsoft-identity-manager-2016-ma-ws-restsample/resource-group.png)

5. Utilize os nomes existentes para o Serviço de Aplicações e selecione **Criar** :

    ![Criar o Serviço de Aplicações](media/microsoft-identity-manager-2016-ma-ws-restsample/create.png)

    O Serviço de Aplicações é criado.

6. Para publicar o Serviço de Aplicações, **selecione Publicar:**

    ![Publicar o Serviço de Aplicações](media/microsoft-identity-manager-2016-ma-ws-restsample/publish.png)

7. Após a publicação do Serviço de Aplicações, a amostra de API REST e o site correspondente são lançados no seu navegador padrão:

    ![Amostra REST API e website](media/microsoft-identity-manager-2016-ma-ws-restsample/sample-rest-api.png)

Agora pode configurar a sua implantação, conforme descrito no [guia de implantação REST](microsoft-identity-manager-2016-ma-ws-restgeneric.md).


## <a name="modify-the-sample"></a>Modificar a amostra

Para modificar os dados JSON e a amostra DE API REST, faça as suas alterações no ficheiro **ONdb.JS** e, em seguida, atualize a implementação:

![Atualizar o ficheiro on db.JS](media/microsoft-identity-manager-2016-ma-ws-restsample/db-json.png)


## <a name="next-steps"></a>Passos seguintes

- [Visão geral do conector genérico do serviço web](microsoft-identity-manager-2016-ma-ws.md)
- [Instale a Ferramenta de Configuração do Serviço Web](microsoft-identity-manager-2016-ma-ws-install.md)
- [Guia de implementação soap](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Guia de implantação do REST](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Configuração MA do Serviço Web](microsoft-identity-manager-2016-ma-ws-maconfig.md)
