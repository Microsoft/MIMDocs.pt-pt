---
title: "Instalação do BHOLD SP1 | Microsoft Docs"
description: "Documentação de instalação do BHOLD SP1"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: c36a9d02e90101b98ade913224e573ed21dc3d5c
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2017
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Guia de instalação do Microsoft BHOLD Suite SP1 (6.0)

Microsoft® BHOLD Suite Service Pack 1 (SP1) é uma coleção de aplicações que, quando utilizado com o SP1 do Microsoft Identity Manager 2016 (MIM), adiciona gestão de funções Efetivo, análise e atestado de MIM. Microsoft BHOLD Suite SP1 inclui os seguintes módulos:

- Núcleo do BHOLD
- Conector de gestão de acesso
- Integração do BHOLD FIM/MIM
- Gerador de modelos do BHOLD
- Do BHOLD Analytics
- Relatórios do BHOLD
- Atestado do BHOLD


>[!NOTE]
**Aplica-se a**: do Microsoft Identity Manager 2016 SP1

## <a name="what-this-document-covers"></a>Este documento abrange

Este documento explica como planear a implementação de BHOLD para satisfazer as suas necessidades e instalar cada módulo BHOLD. Para cada configuração de rede de pré-instalação do módulo, hardware relevante, infraestrutura e os requisitos de software, as informações necessárias durante a configuração e passos postinstallation, se existirem, detalhados.

## <a name="pre-requisite-knowledge"></a>Pré-requisito destinados dados de conhecimento

Este documento parte do princípio de que tem uma compreensão básica sobre como instalar o software em computadores de servidor. Também parte do princípio que tem conhecimentos básicos do software de base de dados de serviços de domínio do Active Directory®, o Microsoft Identity Manager SP1 (FIM) e o Microsoft SQL Server 2008. Uma descrição de como configurar e tecnologias dependentes, tais como o AD DS e de FIM está fora do âmbito desta documentação. Para obter informações sobre as funções que executam os módulos do Microsoft BHOLD, consulte [o Guia do Microsoft BHOLD suite conceitos](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx).

## <a name="audience"></a>Audiência

Este documento destina-se a planeadores IT, arquitetos de sistemas, decisores de tecnologia, consultores, planeadores de infraestrutura e técnicos de TI que planear a implementação de BHOLD Suite de Microsoft.

## <a name="bhold-infrastructure-considerations"></a>Considerações de infraestrutura do BHOLD

Frequentemente, do BHOLD e FIM são utilizadas num ambiente de infra-estrutura grande. Pode personalizar a sua arquitetura BHOLD e de FIM para satisfazer as suas necessidades empresariais específicas. As secções seguintes fornecem algumas possíveis soluções da arquitetura. Esta descrição geral não é uma lista completa de todas as possíveis opções, mas sugere formas que pode implementar BHOLD na sua rede.
 
Esta secção abrange os seguintes tópicos:

- Arquitetura de servidor único
- Arquitetura de Dual-servidor
- arquitetura de duas camadas
- Recomendações do SQL Server

### <a name="single-server-architecture"></a>Arquitetura de servidor único

Para implementação em pequenas organizações ou para fins de desenvolvimento, pode instalar BHOLD e de FIM no mesmo servidor do SQL Server e o AD DS, conforme mostrado na figura seguinte.
 
![Arquitetura de servidor único](media/bhold-installation-guide/single.png)

Quando BHOLD Suite SP1 e o Portal do FIM estiverem instalados em conjunto num único servidor, tem de criar aliases de outro anfitrião (registos CNAME ou A) no DNS para BHOLD e para FIM. Isto permite que os nomes dos principais de serviço separada (SPNs) seja criado para os serviços do BHOLD e de FIM. Para obter mais informações, consulte [BHOLD Core instalação](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).
Para obter orientações sobre a instalação de FIM numa configuração de servidor único, consulte [configurações comuns para obter guias iniciado](https://technet.microsoft.com/library/ff575965.aspx) do Microsoft TechNet Library.

### <a name="dual-server-architecture"></a>Arquitetura de Dual-servidor

Instalação do núcleo do BHOLD e FIM em servidores separados fornece maior desempenho e a flexibilidade para organizações de média dimensão que não necessitam de uma implementação mais complexa, tal como a fornecida por arquiteturas com várias camadas. A figura seguinte mostra BHOLD e FIM instalado nos respetivos servidores; o servidor do FIM também executar o SQL Server para fornecer serviços de base de dados para BHOLD e de FIM. O serviço de sincronização do FIM em execução no servidor do FIM sincroniza as alterações entre as bases de dados do FIM e BHOLD. Tenha em atenção que se for necessária personalização do utilizador final, o módulo de integração do BHOLD FIM tem de estar instalado no mesmo servidor que o serviço FIM e o Portal do FIM. O módulo de integração do BHOLD FIM requer que o serviço FIM e o módulo de integração do BHOLD FIM estão instalados no mesmo servidor.

![Arquitetura de servidor dupla](media/bhold-installation-guide/dual.png)

>[!IMPORTANT]
A funcionalidade de relatórios do módulo de integração do BHOLD FIM requer as bases de dados do BHOLD e de FIM para ser instalada na mesma instância do SQL Server e a conta de serviço do BHOLD tem de ter direitos de acesso para a base de dados do serviço FIM.

### <a name="two-tier-architecture"></a>arquitetura de duas camadas

Na maioria dos ambientes, especialmente dos que desempenho é importante, deve executar o SP1 de BHOLD Suite, o FIM e o SQL Server em servidores separados (arquitetura de duas camadas). Com uma arquitetura de duas camadas, memória e recursos da CPU estão dedicados para cada camada. A ilustração seguinte mostra uma forma de configurar uma arquitetura de duas camadas. O serviço de sincronização do FIM em execução no servidor do FIM sincroniza as alterações entre as bases de dados do FIM e BHOLD. Tenha em atenção que se for necessária personalização do utilizador final, o módulo de integração do BHOLD FIM tem de estar instalado no mesmo servidor que o serviço FIM e o Portal.

![arquitetura de duas camadas](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>Recomendações do SQL Server

Se estiver a implementar BHOLD na organização de grandes dimensões, recomenda-se vivamente que siga estas diretrizes para configurar a base de dados do Microsoft SQL Server:

- Implemente o SQL Server num servidor separado a partir de quaisquer serviços FIM ou BHOLD.
- Isole o ficheiro de registo do ficheiro de dados ao nível do disco físico.
- Se estiver a utilizar o RAID para fornecer redundância de armazenamento, utilize o RAID de nível 10 (1 + 0). Não utilize o nível RAID 5.
- Lembre-se de que configurar as definições corretas quando utilizar mais do que 2 GB de memória física para o servidor a executar o SQL Server.
- Para um desempenho ideal do BHOLD, utilize o Microsoft SQL Server 2008 R2 ou superior.

Para obter mais informações sobre as melhores práticas do SQL Server, consulte [melhores práticas do Storage primeiros 10](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) do Microsoft TechNet Library.

### <a name="trusted-certificates-list-update"></a>Atualização da lista de certificados fidedignos

Windows pode ser configurado para validar as cadeias de certificados antes de iniciar um serviço. Esses sistemas, um serviço não é possível iniciar se o código executável do serviço foi assinado com um certificado que não se encontra na lista de certificados fidedignos (TCL) do servidor. O software Microsoft BHOLD Suite SP1 é código assinado com um cadeia de certificados que têm origem com o certificado da Microsoft 2010 de autoridade de certificado de raiz de assinatura de código.
Windows pode ser configurado para obter certificados de raiz da Microsoft através de uma ligação à Internet. No entanto, num sistema desligado, o Windows Server inclui apenas os certificados que estavam presentes no programa de raiz de cada vez antes de lançamento do Windows. Em versões do Windows Server antes de Windows Server 2010, estes certificados não irão incluir o certificado de raiz necessário para validar o cadeia de certificados de assinatura de código de BHOLD Suite SP1. Se pretender instalar um ou mais módulos de Microsoft BHOLD Suite SP1 num sistema que não pode ter um TCL atualizada, deve transferir e instalar o pacote de atualização de raiz ou utilizar a política de grupo para instalar o pacote de atualização de raiz, antes de instalar um SP1 de BHOLD Suite módulo. Para obter mais informações, consulte [os membros do programa de certificado de raiz do Windows](http://support.microsoft.com/kb/931125).

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>A instalação de BHOLD Suite SP1 no Windows Server 2012/2016 necessárias passo 

![Instalar o IIS BHOLD](media/bhold-installation-guide/iis-install-bhold.png)

Se instalar BHOLD Suite SP1 no Windows Server 2012 ou 2016, as páginas web do BHOLD não estará disponível depois de modificar o ficheiro applicationHost. config, localizado na ```C:\Windows\System32\inetsrv\config```. No ```<globalModules>``` secção, adicione ```preCondition="bitness64``` para a entrada que comece ```<add name="SPNativeRequestModule"``` para que o se lê da seguinte forma:

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

Depois de editar e guardar o ficheiro, execute o comando iisreset para repor o servidor IIS.


## <a name="upgrading-bhold-suite"></a>Atualizar BHOLD Suite

Não é possível atualizar uma instalação existente do BHOLD Suite. Em vez disso, tem de desinstalar uma instalação existente do BHOLD Suite antes de poder atualizar módulos do BHOLD. Se tiver um modelo de função do BHOLD existente, pode atualizar a base de dados do BHOLD e utilizá-lo quando instala o módulo de BHOLD Core atualizado. Para obter mais informações, consulte [substituir BHOLD Suite com SP1 de BHOLD Suite](https://technet.microsoft.com/en-us/library/jj874043(v=ws.10).aspx).


## <a name="next-steps"></a>Próximos passos

- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
