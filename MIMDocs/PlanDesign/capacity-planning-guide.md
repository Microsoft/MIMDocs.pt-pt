---
# required metadata

title: Guia de planeamento de capacidade | Microsoft Identity Manager
description: Utilize este guia para compreender as variáveis que devem ser consideradas antes de implementar o 2016 MIM, incluindo os níveis de carga e as decisões de políticas.
keywords:
author: kgremban
manager: stevenpo
ms.date: 05/13/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 3ac5b990-1678-4996-996d-cbd84b8426b4

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Guia de planeamento de capacidade

O Microsoft Identity Manager (MIM) permite criar, atualizar e remover contas de utilizador em toda a organização. Também proporciona aos utilizadores finais a capacidade de gerir as próprias funcionalidades de gestão personalizada de contas. Mesmo num ambiente pequeno, todas estas ações podem aumentar rapidamente.

Antes de iniciar o MIM, utilize este guia, juntamente com ambientes de teste, para compreender o âmbito adequado para a implementação. Este artigo explica vários fatores comuns que deve ter em consideração. Uma vez que cada implementação é exclusiva, testar os cenários num laboratório continua a ser a melhor forma de determinar os servidores, o hardware ou as topologias que melhor se adequam às suas necessidades.

Se ainda não estiver familiarizado com o MIM 2016 e os respetivos componentes, obtenha mais detalhes acerca do [Microsoft Identity Manager 2016](/microsoft-identity-manager/understand-explore/microsoft-identity-manager-2016) antes de continuar.

## Descrição geral
Existem muitas variáveis que podem afetar a capacidade e o desempenho globais da implementação do Microsoft Identity Manager. As formas através das quais implementa fisicamente os componentes do MIM (topologia), bem como o hardware no qual esses componentes são executados, são fatores importantes para determinar o desempenho e a capacidade que pode esperar da implementação do MIM. O número e a complexidade dos objetos de configuração de políticas do MIM podem ser menos óbvios, mas ainda são fatores significativos a considerar quando planear a capacidade. Por fim, a escala esperada da implementação, bem como a carga que se espera que seja colocada na mesma, são, normalmente, fatores mais óbvios que afetam o desempenho e capacidade.

Os principais fatores que afetam a capacidade e o desempenho que se podem esperar de uma implementação do MIM 2016 são abordados na tabela seguinte.

| Fator da Estrutura | Considerações |
| ------------- | -------------- |
| Topologia | A distribuição dos serviços MIM entre os computadores na rede. |
| Hardware | O hardware físico e quaisquer especificações de hardware virtualizadas em execução para cada componente do MIM. Isto inclui a CPU, a memória, a placa de rede e a configuração de disco rígido. |
| Objetos de configuração de políticas do MIM | O número e o tipo dos objetos de configuração de políticas do MIM, incluindo conjuntos, Regras de Política de Gestão (MPRs) e fluxos de trabalho. |
| Escala | O número de utilizadores, grupos, grupos calculados e tipos de objetos personalizados, tais como computadores que serão geridos pelo MIM 2016. Além disso, considere a complexidade dos grupos dinâmicos e não se esqueça de ter em consideração o aninhamento de grupos. |
| Carga | Frequência de utilização. Por exemplo, a frequência esperada para a criação de novos grupos ou utilizadores, a reposição das palavras-passe ou as visitas ao portal num determinado período de tempo. Tenha em atenção que a carga pode variar no decorrer de uma hora, um dia, uma semana ou um ano. Consoante o componente, pode optar por conceber para o pico de carga ou a carga média. |


## Alojamento de componentes do Microsoft Identity Manager

Os componentes do Microsoft Identity Manager não têm de estar localizados no mesmo computador. Ter em consideração estes componentes e as máquinas físicas ou virtuais que os irão alojar é uma parte importante do planeamento da capacidade.

Os fatores relacionados com o hardware podem afetar o desempenho do ambiente do MIM. Por exemplo:
- Qual é a configuração de disco físico do computador que executa a Base de Dados SQL do Serviço MIM 2016? O número de spindles que constituem a configuração do disco ou a distribuição de ficheiros de dados e de registo pode afetar significativamente o desempenho do sistema.

Além disso, tenha em consideração os fatores externos na configuração. Por exemplo:
- Se estiver a utilizar uma SAN como a configuração da base de dados do Serviço MIM 2016, que outras aplicações estão a partilhar a SAN? Estas aplicações podem afetar o desempenho da base de dados se utilizarem também os recursos de disco partilhados na SAN.


## Utilizadores e grupos
O número de utilizadores e grupos no seu ambiente é uma consideração típica quando tem em conta a escala de uma implementação. No entanto, existem muitas outras considerações relacionadas que também deve ter em conta no planeamento.

- Os utilizadores podem criar grupos? Em caso afirmativo, considere fazer uma estimativa de como os utilizadores que criam novos grupos poderão afetar o crescimento de grupos no seu ambiente.

- Está a considerar implementar grupos dinâmicos? Determine o número e os tipos de grupos dinâmicos que são esperados no seu ambiente.


## Níveis de carga esperados
De igual modo, deve considerar o tipo de carga a colocar nos componentes do MIM. Provavelmente, estas informações podem ser estimadas observando as aplicações atuais no seu ambiente. Veja a seguir algumas questões relevantes a ter em consideração:

- Com que frequência espera receber pedidos para aderir ou sair de um grupo?

- Com que frequência espera que um utilizador crie um grupo estático ou dinâmico?

- Quantas operações não controladas pelo utilizador espera que ocorram, tais como a sincronização das alterações de sistemas externos? Certifique-se de que tem em conta a carga gerada pela sincronização das informações de identidade com os sistemas externos.

- Que tipo de cenários planeia implementar? Cenários diferentes contribuem para padrões diferentes de carga. Por exemplo, os computadores cliente que tenham o cliente MIM 2016 instalado validam periodicamente se o registo é necessário no início de sessão, o que aumenta a carga do sistema.

- Espera grandes variações nos níveis de carga, desde a carga normal ao pico de carga? Por exemplo, tende existir uma grande quantidade de reposições de palavras-passe após os períodos de férias. Certifique-se de que os horários de sincronização e de manutenção do sistema são fora dos picos de utilização previsíveis. Quando pensar no planeamento da capacidade, certifique-se de que tem em conta os períodos de pico de carga.


## Objetos de configuração de políticas

Os objetos de configuração de políticas do Microsoft Identity Manager incluem as MPRs, os conjuntos, os fluxos de trabalho e as regras de sincronização de uma implementação específica. As implementações do MIM são exclusivas para cada cliente, porque a configuração de políticas é alterada para se ajustar às necessidades de cada implementação. As principais considerações sobre o desempenho relacionadas com os objetos de configuração de políticas do MIM incluem o seguinte:

- **Conjuntos** – Todas as operações no sistema têm de ser avaliadas tendo em conta as atualizações e as associações ao conjunto existentes que causam alterações na associação ao conjunto. Por exemplo, uma alteração simples como, por exemplo, para o número do edifício do escritório de um indivíduo, pode não ter um grande impacto. No entanto, existem outras alterações que podem ter um impacto em cascata, tais como a alteração de um gestor, que pode afetar vários objetos em vários níveis.

- **Regras de Política de Gestão** – As MPRs gerem as regras de controlo de acesso e acionam os fluxos de trabalho. Quando cria MPRs, pode constatar que precisa de aumentar o número de conjuntos para poder capturar vários estados de transição de objetos. Estes conjuntos adicionais podem acionar fluxos de trabalho adicionais, com cada fluxo de trabalho a ser mapeado para pedidos exclusivos no sistema. Deste modo, isto torna-se outro item a incluir quando planear a capacidade.

A configuração de políticas do MIM também inclui as decisões sobre o aprovisionamento no seu ambiente. Certifique-se de que tem em consideração o seguinte:

- Pensa aprovisionar princípios de segurança externos em várias florestas dos Serviços de Domínio do Active Directory (AD DS)? Se o fizer, irá gerar mais fluxos de trabalho e pedidos, o que resultará numa carga adicional no sistema.

- Pensa utilizar o aprovisionamento sem código? Se o fizer, isso afetará o número de entradas de regras esperadas, bem como os fluxos de trabalho e os pedidos associados no sistema.


## Consulte também
- [Considerações sobre a topologia da implementação do MIM](topology-considerations.md)
- O [Guia de Planeamento da Capacidade do Forefront Identity Manager (FIM) 2010](http://go.microsoft.com/fwlink/?LinkId=200180) transferível fornece mais detalhes sobre uma compilação de teste e os resultados de testes de desempenho.


<!--HONumber=Jun16_HO1-->


