---
title: Instalação BHOLD SP1 / Microsoft Docs
description: Documentação de instalação BHOLD SP1
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/11/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 848bdbb793de97eb9512de93febd939bb45a52d3
ms.sourcegitcommit: 78c2d7e5ba4bec276d5a9bf8860bc126d9bd9c33
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/11/2020
ms.locfileid: "94492298"
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Guia de instalação microsoft BHOLD Suite SP1 (6.0)

Microsoft® BHOLD Suite Service Pack 1 (SP1) é uma coleção de aplicações que, quando usadas com o Microsoft Identity Manager 2016 SP1 (MIM), adiciona uma gestão eficaz de funções, análise e atestado à MIM. O Microsoft BHOLD Suite SP1 é composto pelos seguintes módulos:

- Núcleo BHOLD
- Conector de Gestão de Acessos
- Integração BHOLD FIM/MIM
- Gerador de modelos BHOLD
- BHOLD Analytics
- Relatório bhold
- Atestado BHOLD


> [!NOTE]
> **Aplica-se a** : Microsoft Identity Manager 2016 SP1 ou posterior

## <a name="what-this-document-covers"></a>O que este documento cobre

Este documento explica como planear a sua implementação BHOLD para atender às necessidades do seu negócio e instalar cada módulo BHOLD. Para cada módulo, os requisitos de hardware, infraestrutura e software relevantes, configuração da rede de pré-instalação, informações necessárias durante a configuração e etapas de pós-instalação, caso existam, são detalhadas.

## <a name="pre-requisite-knowledge"></a>Conhecimentos necessários

Este documento pressupõe que tem uma compreensão básica de como instalar software em computadores de servidor. Também assume que tem conhecimentos básicos de Ative Directory® Domain Services, Fronte ou Microsoft Identity Manager (FIM) e software de base de dados Microsoft SQL Server 2012. Uma descrição de como configurar e configurar tecnologias dependentes, como a AD DS e a FIM, está fora do âmbito desta documentação. Para obter informações sobre as funções que os módulos Microsoft BHOLD desempenham, consulte [o guia de conceitos da suite Microsoft BHOLD](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx).

## <a name="audience"></a>Audiência

Este documento destina-se a planeadores de TI, arquitetos de sistemas, decisores tecnológicos, consultores, planeadores de infraestruturas e pessoal de TI que planeiam implantar a Microsoft BHOLD Suite.

## <a name="bhold-infrastructure-considerations"></a>Considerações de infraestrutura da BHOLD

Na maioria das vezes, o BHOLD e o FIM são utilizados num grande ambiente de infraestruturas. Você pode personalizar a sua arquitetura BHOLD e FIM para atender às suas necessidades de negócio particulares. As seguintes secções fornecem algumas soluções arquitetónicas possíveis. Esta visão geral não é uma lista completa de todas as opções possíveis, mas sugere formas de implementar o BHOLD na sua rede.
 
Esta secção abrange os seguintes tópicos:

- Arquitetura de servidor único
- Arquitetura de servidor duplo
- Arquitetura de dois níveis
- Recomendações do SQL Server

### <a name="single-server-architecture"></a>Arquitetura de servidor único

Para implementação em pequenas organizações ou para fins de desenvolvimento, pode instalar O BHOLD e o FIM no mesmo servidor que o SQL Server e o AD DS, como mostra o seguinte valor.
 
![Arquitetura de servidor único](media/bhold-installation-guide/single.png)

Quando o BHOLD Suite SP1 e o Portal FIM estiverem instalados juntos num único servidor, deve criar diferentes pseudónimos de anfitrião (CNAME ou registos A) em DNS para BHOLD e para FIM. Isto permite criar nomes principais de serviço separados (SPNs) para os serviços BHOLD e FIM. Para obter mais informações, consulte [a Instalação do Núcleo BHOLD.](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)
Para obter orientações sobre a instalação do FIM numa configuração de um único servidor, consulte [a Configuração Comum para Obter Guias Iniciados](https://technet.microsoft.com/library/ff575965.aspx) na Biblioteca Microsoft TechNet.

### <a name="dual-server-architecture"></a>Arquitetura de servidor duplo

A instalação do BHOLD Core e fim em servidores separados proporciona um maior desempenho e flexibilidade para organizações de tamanho médio que não requerem uma implementação mais complexa, como a fornecida por arquiteturas multi-mais multi-diferentes. A seguinte figura mostra BHOLD e FIM instalados nos seus próprios servidores; o servidor FIM também está a executar o SQL Server para fornecer serviços de base de dados ao BHOLD e FIM. O Serviço de Sincronização FIM em execução no servidor FIM sincroniza as alterações entre as bases de dados FIM e BHOLD. Note que, se for necessário o autosserviço do utilizador final, o módulo de integração BHOLD FIM deve ser instalado no mesmo servidor que o Serviço FIM e o Portal FIM. O módulo de Integração BHOLD FIM requer que o Serviço FIM e o módulo de Integração BHOLD FIM estejam instalados no mesmo servidor.

![Arquitetura de servidor duplo](media/bhold-installation-guide/dual.png)

> [!IMPORTANT]
> A funcionalidade de reporte do módulo de integração BHOLD FIM requer que as bases de dados BHOLD e FIM sejam instaladas na mesma instância do SQL Server, e a conta de serviço BHOLD deve ter direitos de acesso à base de dados do Serviço FIM.

### <a name="two-tier-architecture"></a>Arquitetura de dois níveis

Na maioria dos ambientes, especialmente aqueles onde o desempenho é importante, você deve executar o BHOLD Suite SP1, FIM e SQL Server em servidores separados (arquitetura de dois níveis). Com uma arquitetura de dois níveis, a memória e os recursos da CPU são dedicados a cada nível. A seguinte ilustração mostra uma maneira possível de configurar uma arquitetura de dois níveis. O Serviço de Sincronização FIM em execução no servidor FIM sincroniza as alterações entre as bases de dados FIM e BHOLD. Note que, se for necessário o autosserviço do utilizador final, o módulo de integração BHOLD FIM deve ser instalado no mesmo servidor que o Serviço FIM e portal.

![arquitetura de dois níveis](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>Recomendações do SQL Server

Se estiver a implementar o BHOLD numa grande organização, é altamente recomendável que siga estas diretrizes para a configuração da base de dados do Microsoft SQL Server:

- Implemente o SQL Server num servidor separado de quaisquer serviços FIM ou BHOLD.
- Isolar o ficheiro de registo do ficheiro de dados ao nível do disco físico.
- Se estiver a utilizar o RAID para fornecer redundância de armazenamento, utilize o raid nível 10 (1+0). Não utilize o nível RAID 5.
- Certifique-se de que configura as definições corretas quando utilizar mais de 2 GB de memória física para o servidor que executa o SQL Server.

Para obter mais informações sobre as melhores práticas do SQL Server, consulte [o Storage Top 10 Best Practices](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) na Microsoft TechNet Library.

### <a name="trusted-certificates-list-update"></a>Atualização da lista de certificados fidedignos

O Windows pode ser configurado para validar cadeias de certificados antes de iniciar um serviço. Nestes sistemas, um serviço não pode iniciar-se se o código executável do serviço tiver sido assinado com um certificado que não está na lista de certificados fidedignos (TCL) do servidor. O software Microsoft BHOLD Suite SP1 é assinado através de uma cadeia de certificados de assinatura de código que se origina com o certificado Microsoft Root Certificate Authority 2010.
O Windows pode ser configurado para obter certificados de raiz da Microsoft através de uma ligação à Internet. Num sistema desligado, no entanto, o Windows Server inclui apenas os certificados que estavam presentes no programa raiz numa altura antes do Windows ser lançado. Nas versões do Windows Server antes do Windows Server 2010, estes certificados não incluirão o certificado raiz necessário para validar a cadeia de certificados de assinatura de código BHOLD Suite SP1. Se pretender instalar um ou mais módulos Microsoft BHOLD Suite SP1 num sistema que possa não ter um TCL atualizado, tem de descarregar e instalar o pacote de atualização de raiz, ou utilizar a Política de Grupo para instalar o pacote de atualização de raiz, antes de instalar um módulo BHOLD Suite SP1. Para obter mais informações, consulte [os membros do programa de certificados de raiz do Windows](https://support.microsoft.com/kb/931125).

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>Instalação da Suíte BHOLD SP1 no Windows Server 2012/2016 Passo Obrigatório 

![IIS instalar BHOLD](media/bhold-installation-guide/iis-install-bhold.png)

Se instalar o BHOLD Suite SP1 no Windows Server 2012 ou 2016, as páginas web do BHOLD não estarão disponíveis até modificar o ficheiro applicationHost.config localizado em ```C:\Windows\System32\inetsrv\config``` . Na ```<globalModules>``` secção, adicione ```preCondition="bitness64``` à entrada que começa de modo a que leia o ```<add name="SPNativeRequestModule"``` seguinte:

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

Depois de editar e guardar o ficheiro, executar o comando iisreset para reiniciar o servidor IIS.


## <a name="upgrading-bhold-suite"></a>Upgrade BHOLD Suite

Não é possível atualizar uma instalação BHOLD Suite existente. Em vez disso, tem de desinstalar uma instalação BHOLD Suite existente antes de poder atualizar os módulos BHOLD. Se tiver um modelo BHOLD existente, pode atualizar a base de dados BHOLD e usá-la quando instalar o módulo BHOLD Core atualizado. Para mais informações, consulte [substituir a Suíte BHOLD pela BHOLD Suite SP1](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx).


## <a name="next-steps"></a>Passos seguintes

- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
