---
title: "Guia de topologia de implementação | Microsoft Identity Manager"
description: "Compreenda os componentes do MIM 2016 e obtenha sugestões sobre como implementá-los no seu ambiente."
keywords: 
author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 735dc357-dfba-4f68-a5b3-d66d6c018803
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: b3ab1b9376c9b613739d87c812f4b16a4e17e6de
ms.openlocfilehash: 8efb513bfe7dfb67d240a17b39535270f0c7fab6


---


# Considerações sobre a topologia
Pode implementar componentes do Microsoft Identity Manager (MIM) no mesmo servidor ou em vários servidores com várias configurações. A topologia que selecionar para a implementação afetará o desempenho que pode alcançar no MIM. Este artigo apresenta várias topologias de implementação que pode considerar implementar.

## Componentes do MIM
Ao conceber a topologia de implementação, é importante saber o que faz cada componente e como todos eles interagem.

- **Portal do MIM** – uma interface para reposições de palavras-passe, gestão de grupos e operações administrativas.
    -
- **Serviço MIM** – um serviço Web que implementa a funcionalidade da gestão de identidades do MIM 2016.
- **Serviço de Sincronização do MIM** – sincroniza os dados com outros sistemas de identidade.
- **Microsoft SQL Server** – o Serviço MIM e o Serviço de Sincronização do MIM armazenam os respetivos dados nas bases de dados do SQL.

A tabela seguinte mostra as opções para o alojamento de cada um dos componentes do MIM. Estes podem ser alojados no mesmo computador ou distribuídos em vários servidores e clusters.

| | Portal do MIM | Serviço MIM | Serviço de Sincronização do MIM | SQL Server |
| --- | --- | --- | --- | --- |
| Mesmo computador | Sim | Sim | Sim | Sim |
| Servidor separado | Sim | Sim | Sim | Sim |
| Cluster com Balanceamento de Carga na Rede | Sim | Sim | | |
| Cluster de servidores | | | | Sim |


## Topologia com múltiplas camadas
A topologia com múltiplas camadas é a topologia utilizada com maior frequência. Oferece a maior flexibilidade. O Portal do MIM, o Serviço MIM e as bases de dados são separados em camadas e implementados em vários computadores. Esta topologia adiciona flexibilidade ao dimensionar os diferentes componentes do MIM. Por exemplo, pode dimensionar o Portal do MIM horizontalmente adicionando servidores adicionais num cluster de Balanceamento de Carga na Rede (NLB). De igual modo, pode dimensionar o serviço MIM através da utilização de um cluster NLB e do aumento do número de computadores (nós) no cluster conforme necessário.

Numa topologia com várias camadas, é atribuído um computador dedicado para alojar cada base de dados do SQL (uma para o Serviço MIM e outra para o Serviço de Sincronização do MIM). A escalabilidade do desempenho dos computadores que alojam as bases de dados SQL pode ser aumentada ao adicionar ou atualizar o hardware, por exemplo, ao atualizar as CPUs, ao adicionar mais CPUs, ao aumentar a memória de acesso aleatório (RAM), ao atualizar a RAM ou ao atualizar as configurações do disco rígido para aumentar o acesso de leitura e de escrita e diminuir a latência.

![Diagrama da topologia com várias camadas do MIM](media/MIM-topo-multitier.png)

Nesta configuração, o Serviço de Sincronização do MIM e a respetiva base de dados estão alojados no mesmo computador. No entanto, deve conseguir alcançar um desempenho semelhante, se existir uma ligação de rede dedicada de um gigabit entre o Serviço de Sincronização do MIM e a respetiva base de dados quando estes estão alojados em computadores separados.


## Topologia com várias camadas e vários serviços MIM
A sincronização dos dados com sistemas externos pode demorar muito tempo e adicionar uma carga considerável ao sistema durante esse período. Se a configuração da sincronização resultar no acionamento de políticas com fluxos de trabalho, estas políticas disputam os recursos com fluxos de trabalho do utilizador final. Tais problemas podem ser pronunciados com fluxos de trabalho de autenticação, tais como reposições de palavras-passe, que são efetuadas em tempo real com um utilizador final a aguardar que o processo conclua. Ao fornecer uma instância do Serviço MIM para operações do utilizador final e um portal separado para a sincronização de dados administrativos, pode fornecer uma melhor capacidade de resposta para operações do utilizador final.

![Diagrama da topologia com várias camadas e vários serviços do MIM](media/MIM-topo-multitier-multiservice.png)

Tal como com a topologia com várias camadas padrão, pode aumentar o desempenho do Portal do MIM através da utilização de um cluster NLB e do aumento do número de nós no cluster conforme necessário.

Os computadores que executam o SQL Server e que alojam o Serviço de Sincronização do MIM e a base de dados do Serviço MIM irão influenciar significativamente o desempenho global da implementação do MIM. Portanto, siga as recomendações apresentadas na documentação do SQL Server para otimizar o desempenho da base de dados. Para mais informações, consulte os seguintes documentos:

## Consulte também
- O [Guia de Planeamento da Capacidade do Forefront Identity Manager (FIM) 2010](http://go.microsoft.com/fwlink/?LinkId=200180) transferível fornece mais detalhes sobre uma compilação de teste e os resultados de testes de desempenho.



<!--HONumber=Jul16_HO3-->


