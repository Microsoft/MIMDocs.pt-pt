---
title: Guia de planeamento da capacidade | Documentos da Microsoft
description: Utilize este guia para compreender as variáveis que devem ser consideradas antes de implementar o 2016 MIM, incluindo os níveis de carga e as decisões de políticas.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b14066543c036eb4ec8a350843743b87902a13a1
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "73636971"
---
# <a name="capacity-planning-guide"></a>Guia de planeamento de capacidade

O Microsoft Identity Manager (MIM) permite criar, atualizar e remover contas de utilizador em toda a organização. Também proporciona aos utilizadores finais a capacidade de gerir as próprias funcionalidades de gestão personalizada de contas. Mesmo num ambiente pequeno, todas estas ações podem aumentar rapidamente.

Antes de iniciar o MIM, utilize este guia, juntamente com ambientes de teste, para compreender o âmbito adequado para a implementação. Este artigo explica vários fatores comuns que deve ter em consideração. Uma vez que cada implementação é exclusiva, testar os cenários num laboratório continua a ser a melhor forma de determinar os servidores, o hardware ou as topologias que melhor se adequam às suas necessidades.

Se ainda não estiver familiarizado com o MIM 2016 e os respetivos componentes, obtenha mais detalhes acerca do [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md) antes de continuar.

## <a name="overview"></a>Overview

Há vários fatores que podem afetar a capacidade e o desempenho geral de sua implantação de Microsoft Identity Manager:

- As maneiras pelas quais você implanta fisicamente os componentes do MIM (Topologia).
- O hardware no qual esses componentes são executados.
- O número e a complexidade dos objetos de configuração de política do MIM são fatores significativos a serem considerados ao planejar a capacidade.
- A escala esperada da implantação e a carga esperada são geralmente fatores mais óbvios que afetam o desempenho e a capacidade.

Os principais fatores que afetam a capacidade e o desempenho de uma implantação do MIM 2016 são abordados na tabela a seguir:

| Fator da Estrutura | Considerações |
| ------------- | -------------- |
| Topologia | A distribuição dos serviços MIM entre os computadores na rede. |
| Hardware | O hardware físico (físico ou virtual) para cada componente do MIM, incluindo CPU, memória, adaptador de rede e configuração de disco rígido. |
| Objetos de configuração de políticas do MIM | O número e o tipo dos objetos de configuração de políticas do MIM, incluindo conjuntos, Regras de Política de Gestão (MPRs) e fluxos de trabalho. |
| Escala | Os usuários, grupos, grupos calculados e tipos de objetos personalizados a serem gerenciados pelo MIM 2016. Além disso, considere a complexidade dos grupos dinâmicos e não se esqueça de ter em consideração o aninhamento de grupos. |
| Carga | Frequência de utilização. Operações como novo grupo ou criação de usuário, redefinições de senha ou visitas ao portal por minuto ou hora. Tenha em atenção que a carga pode variar no decorrer de uma hora, um dia, uma semana ou um ano. Consoante o componente, pode optar por conceber para o pico de carga ou a carga média. |

## <a name="hosting-microsoft-identity-manager-components"></a>Alojamento de componentes do Microsoft Identity Manager

Os componentes do Microsoft Identity Manager não têm de estar localizados no mesmo computador. Ter em consideração estes componentes e as máquinas físicas ou virtuais que os irão alojar é uma parte importante do planeamento da capacidade.

Os fatores relacionados com o hardware podem afetar o desempenho do ambiente do MIM. Por exemplo:

- Qual é a configuração de disco físico do computador que executa a Base de Dados SQL do Serviço MIM 2016? O número de spindles que constituem a configuração do disco ou a distribuição de ficheiros de dados e de registo pode afetar significativamente o desempenho do sistema.

Além disso, tenha em consideração os fatores externos na configuração. Por exemplo:

- Se estiver a utilizar uma SAN como a configuração da base de dados do Serviço MIM 2016, que outras aplicações estão a partilhar a SAN? Estas aplicações podem afetar o desempenho da base de dados se utilizarem também os recursos de disco partilhados na SAN.

## <a name="users-and-groups"></a>Utilizadores e grupos

O número de utilizadores e grupos no seu ambiente é uma consideração típica quando tem em conta a escala de uma implementação. No entanto, existem muitas outras considerações relacionadas que também deve ter em conta no planeamento.

- Os utilizadores podem criar grupos? Em caso afirmativo, considere fazer uma estimativa de como os utilizadores que criam novos grupos poderão afetar o crescimento de grupos no seu ambiente.

- Está a considerar implementar grupos dinâmicos? Determine o número e os tipos de grupos dinâmicos que são esperados no seu ambiente.

## <a name="expected-load-levels"></a>Níveis de carga esperados

De igual modo, deve considerar o tipo de carga a colocar nos componentes do MIM. Você precisará estimar a carga examinando os aplicativos atuais em seu ambiente. Veja a seguir algumas questões relevantes a ter em consideração:

- Com que frequência espera receber pedidos para aderir ou sair de um grupo?

- Com que frequência espera que um utilizador crie um grupo estático ou dinâmico?

- Quantas operações não controladas pelo utilizador espera que ocorram, tais como a sincronização das alterações de sistemas externos? Certifique-se de que tem em conta a carga gerada pela sincronização das informações de identidade com os sistemas externos.

- Que tipo de cenários planeia implementar? Diferentes cenários contribuem para diferentes padrões de carga. Por exemplo, os computadores cliente que têm o cliente MIM 2016 instalado periodicamente validam se o registro é necessário na entrada.

- Espera grandes variações nos níveis de carga, desde a carga normal ao pico de carga? Por exemplo, tendem a ser muitas redefinições de senha após os períodos de feriado. Certifique-se de que os horários de sincronização e de manutenção do sistema são fora dos picos de utilização previsíveis. Quando pensar no planeamento da capacidade, certifique-se de que tem em conta os períodos de pico de carga.

## <a name="policy-configuration-objects"></a>Objetos de configuração de políticas

Os objetos de configuração de política do MIM incluem MPRs, conjuntos, fluxos de trabalho e regras de sincronização para uma implantação. As implementações do MIM são exclusivas para cada cliente, porque a configuração de políticas é alterada para se ajustar às necessidades de cada implementação. As principais considerações de desempenho incluem os seguintes objetos de configuração de política do MIM:

- **Conjuntos** – Todas as operações no sistema têm de ser avaliadas tendo em conta as atualizações e as associações ao conjunto existentes que causam alterações na associação ao conjunto. Por exemplo, uma alteração no número de edifício do escritório de um indivíduo pode não ter um grande impacto. No entanto, existem outras alterações que podem ter um impacto em cascata, tais como a alteração de um gestor, que pode afetar vários objetos em vários níveis.

- **Regras de Política de Gestão** – As MPRs gerem as regras de controlo de acesso e acionam os fluxos de trabalho. A criação de MPRs pode criar a necessidade de aumentar o número de conjuntos para que você possa capturar vários Estados de transição de objetos. Estes conjuntos adicionais podem acionar fluxos de trabalho adicionais, com cada fluxo de trabalho a ser mapeado para pedidos exclusivos no sistema. Deste modo, isto torna-se outro item a incluir quando planear a capacidade.

A configuração de políticas do MIM também inclui as decisões sobre o aprovisionamento no seu ambiente. Certifique-se de que tem em consideração o seguinte:

- Pensa aprovisionar princípios de segurança externos em várias florestas dos Serviços de Domínio do Active Directory (AD DS)? Se o fizer, irá gerar mais fluxos de trabalho e pedidos, o que resultará numa carga adicional no sistema.

- Pensa utilizar o aprovisionamento sem código? Se o fizer, isso afetará o número de entradas de regras esperadas, bem como os fluxos de trabalho e os pedidos associados no sistema.

## <a name="next-steps"></a>Próximos passos

- [Considerações sobre a topologia da implementação do MIM](topology-considerations.md)
- O guia de [planejamento de capacidade do fim (Forefront Identity Manager) 2010](https://www.microsoft.com/en-us/download/details.aspx?id=7437) que pode ser baixado apresenta mais detalhes sobre uma compilação de teste e resultados de testes de desempenho.
