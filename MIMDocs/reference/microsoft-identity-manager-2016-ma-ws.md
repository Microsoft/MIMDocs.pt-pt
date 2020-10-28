---
title: Visão geral do conector genérico do Serviço Web ! Microsoft Docs
description: Visão geral da configuração e dos requisitos para o conector genérico do Serviço Web.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 11/27/2017
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: b0d4cc9ac9b9ac080038d632df0c02c4c244e3d6
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762371"
---
# <a name="overview-of-the-generic-web-service-connector"></a>Visão geral do conector genérico do Serviço Web

O conector do Serviço Web integra identidades através de operações de Web Service com o Microsoft Identity Manager (MIM) 2016 SP1. O conector requer que o ficheiro Do Projeto de Serviço Web se conecte com a fonte de dados correta. Este projeto pode ser descarregado do [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkID=235883) juntamente com [documentação](https://www.microsoft.com/en-us/download/details.aspx?id=29943) para usar o conector com o Oracle eBusiness, Oracle PeopleSoft e SAP. Também pode criá-lo utilizando a Ferramenta de Configuração do Serviço Web.

Quando o Serviço de Sincronização MIM invoca o conector do Serviço Web, carrega o seu ficheiro de projeto configurado (ficheiro **WsConfig).** Este ficheiro ajuda a reconhecer o Ponto Final da fonte de dados que deve ser usado para estabelecer uma ligação. O ficheiro também lhe diz o fluxo de trabalho a executar para implementar uma operação MIM. Para executar os fluxos de trabalho configurados, o conector de serviço web aproveita o motor de tempo de funcionamento da Fundação .NET 4 Workflow Foundation.

![Fluxo de trabalho](media/microsoft-identity-manager-2016-ma-ws/workflow.png)

## <a name="web-service-layers"></a>Camadas de Serviço Web

Duas camadas principais são usadas para implementar a solução de agente de gestão de serviços web (MA): 

- Ferramenta de configuração de serviço web
- Conector de tempo de execução implementado com fluxo de trabalho .NET 4.0

## <a name="supported-data-sources-for-web-service-discovery"></a>Fontes de dados suportadas para descoberta do Serviço Web

A Ferramenta de Configuração do Serviço Web implementa as seguintes funcionalidades:

- SOAP Discovery: Permite ao administrador introduzir o caminho WSDL exposto pelo serviço web alvo. A Discovery produzirá uma estrutura de árvores dos seus serviços web hospedados com o seu ponto final interno/operações, juntamente com a descrição dos dados Meta da operação. Não há limite para o número de operações de descoberta que podem ser feitas (passo a passo). As operações descobertas são posteriormente utilizadas para configurar o fluxo de operações que implementam as operações do conector contra a fonte de dados (como Importação/Exportação/Senha).

- RESTO Descoberta: Permite ao administrador introduzir detalhes de serviço restful, isto é, ponto final de serviço, caminho de recurso, método e detalhes do parâmetro. Um utilizador pode adicionar um número ilimitado de serviços Restful. A informação dos serviços de descanso será armazenada em ```discovery.xml``` arquivo do ```wsconfig``` projeto. Serão usados mais tarde pelo utilizador para configurar a atividade do Serviço Web de Repouso no fluxo de trabalho.

- Configuração do esquema do espaço do conector: Permite ao administrador configurar o esquema de espaço do conector. A configuração do esquema incluirá uma lista de Tipos de Objetos e atributos para uma implementação específica. O administrador pode especificar os tipos de objetos que serão suportados pelo Serviço Web MA. O administrador também pode escolher aqui os atributos que farão parte do Schema espacial do Conector.

- Configuração do Fluxo de Funcionamento: Workflow designer UI para configurar a implementação de operações FIM (Importação/Exportação/Palavra-passe) por tipo de objeto através de funções de operações de serviço web expostas, tais como:

    - Atribuição de parâmetros do espaço do conector às funções de serviço web.
    - Atribuição de parâmetros das funções de serviço web ao espaço do conector.

## <a name="resources-generated-by-the-web-service-configuration-tool"></a>Recursos gerados pela Ferramenta de Configuração do Serviço Web

A Ferramenta de Configuração do Serviço Web gera os recursos necessários para configurar um Serviço Web MA totalmente funcional, que inclui:

- Conector Space Schema: Um ficheiro binário que inclui a configuração do esquema. O ficheiro será importado pela MIM através da ```Get Schema``` interface quando o MA estiver configurado utilizando a UI de Sincronização FIM. É então convertido para objeto de formato ECMA2 Schema.

- Fluxos de trabalho: Uma série de definições de fluxo de trabalho. São utilizados pelo Web Service MA em tempo de execução para executar uma operação apropriada.

- Ficheiro config WCF: um ficheiro de configuração produzido pela operação de descoberta. O ficheiro inclui as informações de encadernação e pontos finais necessárias para invocar uma operação de serviço web contra a fonte de dados.

- Conjunto de contratos de dados: Uma vez que o conector do Serviço Web suporta agora tanto o serviço SOAP como o serviço REST, os contratos de dados gerados serão diferentes no ficheiro generated.dll.

- Conjunto SOAP: Ao analisar a entrada WSDL, a ferramenta de configuração do Serviço Web gera tipos de contratos de dados, que são estruturas de dados utilizadas pelas operações de serviço web para comunicar com o serviço remoto. Estes tipos de contrato também são usados para expor entidades remotas de origem de dados para o mapeamento de atributos do tipo de objeto.

- Conjunto DE REPOUSO: Ao analisar a resposta de pedido de amostra para o REST Web Service, a ferramenta de configuração gerará tipos (classes), que serão utilizados em fluxo de trabalho para comunicar com o serviço web através da atividade de Web Service Call. Cada Pedido/Resposta será definido no seu próprio espaço de nome. O espaço de nome tem uma sintaxe como \< Nome de \> Serviço. \< Nome de recursos \> . \< Nome \> metódculo .. Pedido/Resposta]. Embrulhar cada pedido/resposta em espaço de nome separado ajudará a reduzir as questões devido ao nome duplicado do tipo (classe).

![Fluxo de trabalho](media/microsoft-identity-manager-2016-ma-ws/workflow2.png)

### <a name="project-file-type"></a>Tipo de arquivo de projeto

O Web Service MA é guardado em ficheiro comprimido (formato ZIP) com nome especificado pelo utilizador e extensão de ficheiro "WsConfig". A extensão de ficheiro "WsConfig" está registada e associada à ferramenta de configuração do Serviço Web por instalador. Os projetos de MESTRADO existentes podem ser abertos, modificados e salvos. Podem ser guardadas na pasta de extensões fim synchronization Service ou em qualquer outro local. As alterações relacionadas com o tipo e atributos do objeto requerem sincronização no lado FIM.  A ferramenta de configuração é uma aplicação de múltiplas instâncias concebida para criar e modificar MA(s).

## <a name="supported-security-modes"></a>Modos de segurança suportados

A aplicação de serviço web REST/SOAP pode ser protegida através de um Web Server como o IIS. A aplicação permite ao utilizador selecionar o modo de segurança, como mostra a seguinte figura. Os modos de segurança incluem Basic, Digest, Certificate, Windows ou None.

![Modos de segurança](media/microsoft-identity-manager-2016-ma-ws/security-mode.png)

## <a name="supported-data-types"></a>Tipos de dados suportados

São suportados os seguintes tipos de dados:

- SOAP (legado): O tipo de dados SOAP é suportado como descrito neste [artigo da MSDN](https://msdn.microsoft.com/library/ms995800.aspx). O suporte é fornecido apenas para a stack de Interface de Programação de Aplicações Empresariais (BAPI). Os modelos de SOAP de amostra estão disponíveis no [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=51495).
- REST (não ODATA): Um conector/web baseado em protocolo HTTP.

## <a name="next-steps"></a>Passos seguintes 

- [Instale a Ferramenta de Configuração do Serviço Web](microsoft-identity-manager-2016-ma-ws-install.md)
- [Guia de implementação soap](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Guia de implantação do REST](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Configuração MA do Serviço Web](microsoft-identity-manager-2016-ma-ws-maconfig.md)
