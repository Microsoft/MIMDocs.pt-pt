---
title: Instalação do BHOLD SP1 | Documentos da Microsoft
description: Documentação de instalação do BHOLD SP1
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/11/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: e73596ea1b07814a46d638ac705edf5fdada76a2
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49334351"
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Guia de instalação do Microsoft BHOLD Suite SP1 (6.0)

Microsoft® BHOLD Suite Service Pack 1 (SP1) é uma coleção de aplicativos que, quando utilizado com o Microsoft Identity Manager 2016 SP1 (MIM), adiciona o gerenciamento de funções em vigor, análises e atestado para MIM. SP1 do Microsoft BHOLD Suite inclui os seguintes módulos:

- BHOLD Core
- Conector de gestão de acesso
- Integração do BHOLD FIM/MIM
- Gerador de modelos do BHOLD
- Do BHOLD Analytics
- BHOLD Reporting
- Atestado do BHOLD


> [!NOTE]
> **Aplica-se a**: Microsoft Identity Manager 2016 SP1

## <a name="what-this-document-covers"></a>Este documento aborda

Este documento explica como planear a implementação de BHOLD para atender às necessidades da sua empresa e instalar cada módulo BHOLD. Para cada configuração de rede de pré-instalação do módulo, hardware relevante, infraestrutura e os requisitos de software, informações necessárias durante a configuração e passos postinstallation, se houver, são detalhados.

## <a name="pre-requisite-knowledge"></a>Dados de conhecimento de pré-requisitos

Este documento parte do princípio de que tem um entendimento básico de como instalar o software em computadores de servidor. Também parte do princípio que tem um conhecimento básico do software de base de dados de serviços de domínio do Active Directory®, o Microsoft Identity Manager SP1 (FIM) e o Microsoft SQL Server 2008. Uma descrição de como definir e configurar tecnologias dependentes, tais como o AD DS e de FIM está fora do escopo desta documentação. Para obter informações sobre as funções que executam os módulos do Microsoft BHOLD, consulte [o guia de conceitos do Microsoft BHOLD suite](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx).

## <a name="audience"></a>Audiência

Este documento destina-se a planejadores de TI, arquitetos de sistemas, tomadores de decisão de tecnologia, consultores, planejadores de infraestrutura e pessoal de TI que planejam implantar o Microsoft BHOLD Suite.

## <a name="bhold-infrastructure-considerations"></a>Considerações de infraestrutura do BHOLD

Frequentemente, o BHOLD e o FIM são usados num ambiente de infra-estrutura grande. Pode personalizar sua arquitetura BHOLD e de FIM para atender às suas necessidades empresariais específicas. As secções seguintes fornecem algumas possíveis soluções de arquiteturais. Esta descrição geral não é uma lista abrangente de todas as opções possíveis, mas sugere formas que pode implementar BHOLD na sua rede.
 
Esta secção abrange os seguintes tópicos:

- Arquitetura de servidor único
- Arquitetura de servidor Dual
- Arquitetura de duas camadas
- Recomendações do SQL Server

### <a name="single-server-architecture"></a>Arquitetura de servidor único

Para implementação em pequenas organizações ou para fins de desenvolvimento, pode instalar BHOLD e FIM no mesmo servidor do SQL Server e o AD DS, conforme mostrado na figura a seguir.
 
![Arquitetura de servidor único](media/bhold-installation-guide/single.png)

Quando o SP1 do BHOLD Suite e o Portal do FIM sendo instalados juntos num único servidor, tem de criar aliases de outro anfitrião (registos CNAME ou A) no DNS para BHOLD e para o FIM. Isso permite que os nomes de principal de serviço separado (SPNs) a ser criado para os serviços do BHOLD e de FIM. Para obter mais informações, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).
Para obter orientações sobre como instalar o FIM numa configuração de servidor único, veja [configurações comuns para guias de introdução](https://technet.microsoft.com/library/ff575965.aspx) da Microsoft TechNet Library.

### <a name="dual-server-architecture"></a>Arquitetura de servidor Dual

Instalação do BHOLD Core e de FIM em servidores separados fornece maior desempenho e flexibilidade para organizações de médio porte que não necessitam de uma implementação mais complexa, tal como a fornecida por arquiteturas de várias camadas. A figura seguinte mostra BHOLD e de FIM instalado nos seus próprios servidores; o servidor do FIM também executar o SQL Server para fornecer serviços de base de dados para BHOLD e de FIM. O serviço de sincronização do FIM em execução no servidor do FIM sincroniza alterações entre as bases de dados do FIM e BHOLD. Tenha em atenção que, se for necessário o Self-Service do utilizador final, o módulo de integração do BHOLD FIM deve ser instalado no mesmo servidor que o serviço FIM e o Portal do FIM. O módulo de integração do BHOLD FIM requer que o serviço FIM e o módulo de integração do BHOLD FIM estão instalados no mesmo servidor.

![Arquitetura de servidor duplo](media/bhold-installation-guide/dual.png)

> [!IMPORTANT]
> A funcionalidade de relatórios do módulo de integração do BHOLD FIM requer as bases de dados do BHOLD e de FIM para ser instalado na mesma instância do SQL Server e a conta de serviço do BHOLD tem de ter direitos de acesso para a base de dados do serviço FIM.

### <a name="two-tier-architecture"></a>Arquitetura de duas camadas

Na maioria dos ambientes, especialmente aqueles em que o desempenho é importante, deve executar o SP1 do BHOLD Suite, a FIM e o SQL Server em servidores separados (arquitetura de duas camadas). Com uma arquitetura de duas camadas, memória e recursos da CPU são dedicados para cada camada. A ilustração seguinte mostra uma forma possível de configurar uma arquitetura de duas camadas. O serviço de sincronização do FIM em execução no servidor do FIM sincroniza alterações entre as bases de dados do FIM e BHOLD. Tenha em atenção que, se for necessário o Self-Service do utilizador final, o módulo de integração do BHOLD FIM deve ser instalado no mesmo servidor que o serviço FIM e o Portal.

![Arquitetura de duas camadas](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>Recomendações do SQL Server

Se estiver implantando BHOLD numa grande organização, é altamente recomendável que siga estas diretrizes para a configuração da base de dados do Microsoft SQL Server:

- Implemente o SQL Server num servidor separado a partir de quaisquer serviços FIM ou BHOLD.
- Isole o ficheiro de registo a partir do ficheiro de dados ao nível do disco físico.
- Se estiver a utilizar o RAID para fornecer redundância de armazenamento, utilize o nível RAID 10 (1 + 0). Não utilize o nível RAID 5.
- Certifique-se de que configurar as definições corretas quando utilizar mais de 2 GB de memória física para o servidor a executar o SQL Server.
- Para um desempenho ideal do BHOLD, utilize o Microsoft SQL Server 2008 R2 ou superior.

Para obter mais informações sobre as melhores práticas do SQL Server, consulte [melhores práticas do Storage Top 10](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) da Microsoft TechNet Library.

### <a name="trusted-certificates-list-update"></a>Atualização da lista de certificados fidedignos

Windows podem ser configurado para validar o certificado encadeia antes de iniciar um serviço. Nesses sistemas, um serviço não é possível iniciar se o código executável do serviço foi assinado com um certificado que não esteja na lista de certificados fidedignos (TCL) do servidor. O software do Microsoft BHOLD Suite SP1 é o respetivo código assinado com um cadeia de certificados que tem origem com o Microsoft 2010 de autoridade de certificado de raiz de certificado de assinatura de código.
Windows podem ser configurado para obter certificados de raiz da Microsoft através de uma ligação de Internet. No entanto, num sistema desligado, o Windows Server inclui apenas os certificados que estavam presentes no programa de raiz de cada vez antes de lançamento do Windows. Em versões do Windows Server antes do Windows Server 2010, estes certificados não incluirá o certificado de raiz necessário para validar o cadeia de certificados de assinatura de código do SP1 do BHOLD Suite. Se pretende instalar um ou mais módulos do Microsoft BHOLD Suite SP1 num sistema que não pode ter um TCL atualizado, deve transferir e instalar o pacote de atualização de raiz ou usar a diretiva de grupo para instalar o pacote de atualização de raiz, antes de instalar um pacote Bhold SP1 módulo. Para obter mais informações, consulte [os membros do programa de certificado de raiz do Windows](http://support.microsoft.com/kb/931125).

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>Instalação do BHOLD Suite SP1 no Windows Server 2012/2016 necessário passo 

![Instalar o IIS BHOLD](media/bhold-installation-guide/iis-install-bhold.png)

Se instalar o SP1 do BHOLD Suite no Windows Server 2012 ou 2016, as páginas da web BHOLD não estarão disponíveis até que modificar o ficheiro applicationHost. config localizado na ```C:\Windows\System32\inetsrv\config```. Na ```<globalModules>``` secção, adicione ```preCondition="bitness64``` para a entrada que começa ```<add name="SPNativeRequestModule"``` para que ele lê da seguinte forma:

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

Depois de editar e guardar o ficheiro, execute o comando iisreset para repor o servidor IIS.


## <a name="upgrading-bhold-suite"></a>Atualizar pacote Bhold

Não é possível atualizar uma instalação existente do BHOLD Suite. Em vez disso, tem de desinstalar uma instalação do BHOLD Suite existente antes de poder atualizar módulos do BHOLD. Se tiver um modelo de função do BHOLD existente, pode atualizar a base de dados do BHOLD e utilizá-lo ao instalar o módulo do BHOLD Core atualizado. Para obter mais informações, consulte [substituindo o BHOLD Suite com o SP1 do BHOLD Suite](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx).


## <a name="next-steps"></a>Passos Seguintes

- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
