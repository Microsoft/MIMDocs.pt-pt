---
title: Instalação BHOLD SP1 Microsoft Docs
description: Documentação de instalação BHOLD SP1
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/11/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: fb3cf6e5b00c1bd0c01d86aff474dc2ff28c2815
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2020
ms.locfileid: "79042258"
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Guia de instalação Microsoft BHOLD Suite SP1 (6.0)

Microsoft® BHOLD Suite Service Pack 1 (SP1) é uma coleção de aplicações que, quando utilizadas com o Microsoft Identity Manager 2016 SP1 (MIM), adiciona uma gestão eficaz de papéis, análise e atesta à MIM. O Microsoft BHOLD Suite SP1 é composto pelos seguintes módulos:

- Núcleo BHOLD
- Conector de Gestão de Acesso
- Integração BHOLD FIM/MIM
- Gerador de modelo BHOLD
- BHOLD Analytics
- Relatórios BHOLD
- Atestabção BHOLD


> [!NOTE]
> **Aplica-se a**: Microsoft Identity Manager 2016 SP1

## <a name="what-this-document-covers"></a>O que este documento cobre

Este documento explica como planear a sua implementação BHOLD para atender às necessidades do seu negócio e instalar cada módulo BHOLD. Para cada módulo, são detalhados os requisitos relevantes de hardware, infraestrutura e software, configuração da rede de pré-instalação, informação necessária durante a configuração e etapas de pós-instalação.

## <a name="pre-requisite-knowledge"></a>Conhecimento pré-requisito

Este documento pressupõe que tem uma compreensão básica de como instalar software em computadores de servidores. Também assume que tem conhecimentos básicos de Ative Directory® Domain Services, Microsoft Identity Manager SP1 (FIM) e microsoft SQL Server 2012 software de base de dados. Uma descrição de como configurar e configurar tecnologias dependentes, como AD DS e FIM, está fora do âmbito desta documentação. Para obter informações sobre as funções que os módulos Microsoft BHOLD desempenham, consulte [o guia de conceitos da suite Microsoft BHOLD](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx).

## <a name="audience"></a>Audiência

Este documento destina-se a planeadores de TI, arquitetos de sistemas, decisores tecnológicos, consultores, planeadores de infraestruturas e pessoal de TI que planeia minizar a Microsoft BHOLD Suite.

## <a name="bhold-infrastructure-considerations"></a>Considerações de infraestruturas BHOLD

Na maioria das vezes, o BHOLD e o FIM são utilizados num grande ambiente de infraestruturas. Você pode adaptar a sua arquitetura BHOLD e FIM para atender às suas necessidades de negócio particulares. As seguintes secções fornecem algumas possíveis soluções arquitetónicas. Esta visão geral não é uma lista completa de todas as opções possíveis, mas sugere formas de implementar o BHOLD na sua rede.
 
Esta secção aborda os seguintes tópicos:

- Arquitetura de servidor único
- Arquitetura de servidor duplo
- Arquitetura de dois níveis
- Recomendações do SQL Server

### <a name="single-server-architecture"></a>Arquitetura de servidor único

Para implementação em pequenas organizações ou para fins de desenvolvimento, pode instalar BHOLD e FIM no mesmo servidor que o SQL Server e o AD DS, como mostra a figura seguinte.
 
![Arquitetura de servidor único](media/bhold-installation-guide/single.png)

Quando o BHOLD Suite SP1 e o Portal FIM estiverem instalados juntos num único servidor, deve criar diferentes pseudónimos de hospedeiro (CNAME ou A) em DNS para BHOLD e para o FIM. Isto permite a criação de nomes principais de serviço separados (SPNs) para os serviços BHOLD e FIM. Para mais informações, consulte [a Instalação BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).
Para obter orientações sobre a instalação do FIM numa configuração de um único servidor, consulte a [Configuração Comum para Iniciar Guias](https://technet.microsoft.com/library/ff575965.aspx) na Biblioteca Microsoft TechNet.

### <a name="dual-server-architecture"></a>Arquitetura de servidor duplo

A instalação do BHOLD Core e do FIM em servidores separados proporciona um maior desempenho e flexibilidade para organizações de tamanho médio que não requerem uma implementação mais complexa, como a fornecida por arquiteturas multimais. A figura que se segue mostra bHOLD e FIM instalados nos seus próprios servidores; o servidor FIM também está a executar o SQL Server para fornecer serviços de base de dados para BHOLD e FIM. O Serviço de Sincronização FIM em execução no servidor FIM sincroniza as alterações entre as bases de dados FIM e BHOLD. Note que se for necessário self-service de utilizador final, o módulo de integração BHOLD FIM deve ser instalado no mesmo servidor que o Serviço FIM e o Portal FIM. O módulo de integração BHOLD FIM requer que o Serviço FIM e o módulo de Integração BHOLD FIM sejam instalados no mesmo servidor.

![Arquitetura de servidor duplo](media/bhold-installation-guide/dual.png)

> [!IMPORTANT]
> A funcionalidade de reporte do módulo de integração BHOLD FIM requer que as bases de dados BHOLD e FIM sejam instaladas na mesma instância do Servidor SQL, e a conta de serviço BHOLD deve ter direitos de acesso à base de dados do Serviço FIM.

### <a name="two-tier-architecture"></a>Arquitetura de dois níveis

Na maioria dos ambientes, especialmente aqueles onde o desempenho é importante, você deve executar o BHOLD Suite SP1, FIM e SQL Server em servidores separados (arquitetura de dois níveis). Com uma arquitetura de dois níveis, os recursos de memória e CPU são dedicados para cada nível. A seguinte ilustração mostra uma maneira possível de configurar uma arquitetura de dois níveis. O Serviço de Sincronização FIM em execução no servidor FIM sincroniza as alterações entre as bases de dados FIM e BHOLD. Note que se for necessário self-service de utilizador final, o módulo de integração BHOLD FIM deve ser instalado no mesmo servidor que o Serviço FIM e portal.

![arquitetura de dois níveis](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>Recomendações do SQL Server

Se estiver a implementar o BHOLD numa grande organização, é altamente recomendável que siga estas diretrizes para a configuração da base de dados do Microsoft SQL Server:

- Implemente o Servidor SQL num servidor separado de quaisquer serviços FIM ou BHOLD.
- Isole o ficheiro de registo do ficheiro de dados ao nível do disco físico.
- Se estiver a utilizar o RAID para fornecer redundância de armazenamento, utilize o nível RAID 10 (1+0). Não utilize o nível RAID 5.
- Certifique-se de configurar as definições corretas quando utilizar mais de 2 GB de memória física para o servidor que executa o Servidor SQL.

Para obter mais informações sobre as melhores práticas do SQL Server, consulte o [Storage Top 10 Best Practices](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) na Microsoft TechNet Library.

### <a name="trusted-certificates-list-update"></a>Atualização da lista de certificados fidedignos

O Windows pode ser configurado para validar cadeias de certificados antes de iniciar um serviço. Nesses sistemas, um serviço não pode ser iniciado se o código executável do serviço foi assinado com um certificado que não esteja na lista de certificados fidedignos (TCL) do servidor. O software Microsoft BHOLD Suite SP1 é um código assinado utilizando uma cadeia de certificados de assinatura de código que tem origem no certificado Microsoft Root Certificate Authority 2010.
O Windows pode ser configurado para recuperar certificados de raiz da Microsoft através de uma ligação à Internet. No entanto, num sistema desligado, o Windows Server inclui apenas os certificados que estavam presentes no programa raiz numa altura antes de o Windows ser lançado. Nos lançamentos do Windows Server antes do Windows Server 2010, estes certificados não incluirão o certificado raiz necessário para validar a cadeia de certificados de assinatura de código BHOLD Suite SP1. Se pretender instalar um ou mais módulos Microsoft BHOLD Suite SP1 num sistema que possa não ter um TCL atualizado, tem de descarregar e instalar o pacote de atualização de raízes, ou utilizar a Política do Grupo para instalar o pacote de atualização de raiz, antes de instalar um módulo BHOLD Suite SP1. Para mais informações, consulte [os membros do programa de certificados](https://support.microsoft.com/kb/931125)de raiz do Windows .

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>Instalação bhold Suite SP1 no Windows Server 2012/2016 Passo Obrigatório 

![IIS instalar BHOLD](media/bhold-installation-guide/iis-install-bhold.png)

Se instalar o BHOLD Suite SP1 no Windows Server 2012 ou 2016, as páginas web do BHOLD não estarão disponíveis até modificar o ficheiro applicationHost.config localizado em ```C:\Windows\System32\inetsrv\config```. Na ```<globalModules>``` secção, ```preCondition="bitness64``` adicione à entrada ```<add name="SPNativeRequestModule"``` que começa de modo a que leia o seguinte:

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

Depois de editar e guardar o ficheiro, executar o comando iisreset para redefinir o servidor IIS.


## <a name="upgrading-bhold-suite"></a>Upgrade BHOLD Suite

Não é possível atualizar uma instalação bhold suite existente. Em vez disso, tem de desinstalar uma instalação BHOLD Suite existente antes de atualizar os módulos BHOLD. Se tiver um modelo de função BHOLD existente, pode atualizar a base de dados BHOLD e utilizá-la quando instalar o módulo BHOLD Core atualizado. Para mais informações, consulte Substituir a [BHOLD Suite por BHOLD Suite SP1](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx).


## <a name="next-steps"></a>Passos seguintes

- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
