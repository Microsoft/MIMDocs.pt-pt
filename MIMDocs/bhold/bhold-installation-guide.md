---
title: Instalação do BHOLD SP1 | Microsoft Docs
description: Documentação de instalação do BHOLD SP1
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/11/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 05eb2afc0ddbf6104e27a5c24e121a55bd805292
ms.sourcegitcommit: a4f77aae75a317f5277d7d2a3187516cae1e3e19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2019
ms.locfileid: "68238908"
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Guia de instalação do Microsoft BHOLD Suite SP1 (6,0)

O Microsoft® BHOLD Suite Service Pack 1 (SP1) é uma coleção de aplicativos que, quando usados com o MIM (Microsoft Identity Manager 2016 SP1), adiciona gerenciamento de função efetivo, análise e atestado ao MIM. O Microsoft BHOLD Suite SP1 consiste nos seguintes módulos:

- BHOLD Core
- Conector de gerenciamento de acesso
- Integração do FIM/MIM com o BHOLD
- Gerador de modelo de BHOLD
- BHOLD Analytics
- Relatórios do BHOLD
- Atestado de BHOLD


> [!NOTE]
> **Aplica-se a**: Microsoft Identity Manager 2016 SP1

## <a name="what-this-document-covers"></a>O que este documento abrange

Este documento explica como planejar sua implantação do BHOLD para atender às suas necessidades de negócios e instalar cada módulo BHOLD. Para cada módulo, os requisitos relevantes de hardware, de infraestrutura e de software, a configuração de rede de pré-instalação, as informações necessárias durante a instalação e as etapas de pós-instalação, se houver, são detalhadas.

## <a name="pre-requisite-knowledge"></a>Conhecimento prévio do pré-requisito

Este documento pressupõe que você tenha uma compreensão básica de como instalar software em computadores servidor. Ele também pressupõe que você tenha conhecimento básico de Active Directory® serviços de domínio, Microsoft Identity Manager SP1 (FIM) e Microsoft SQL Server software de banco de dados 2012. Uma descrição de como configurar e configurar tecnologias dependentes, como AD DS e FIM, está fora do escopo desta documentação. Para obter informações sobre as funções que os módulos do Microsoft BHOLD executam, consulte [o guia de conceitos do Microsoft BHOLD Suite](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx).

## <a name="audience"></a>Audiência

Este documento destina-se a planejadores de ti, arquitetos de sistemas, tomadores de decisões de tecnologia, consultores, planejadores de infra-estrutura e pessoal de ti que planejam implantar o Microsoft BHOLD Suite.

## <a name="bhold-infrastructure-considerations"></a>Considerações sobre a infraestrutura do BHOLD

Geralmente, o BHOLD e o FIM são usados em um ambiente de infraestrutura grande. Você pode personalizar sua arquitetura BHOLD e FIM para atender às suas necessidades de negócios específicas. As seções a seguir fornecem algumas soluções arquitetônicas possíveis. Esta visão geral não é uma lista abrangente de todas as opções possíveis, mas sugere maneiras de implantar o BHOLD em sua rede.
 
Esta seção aborda os seguintes tópicos:

- Arquitetura de servidor único
- Arquitetura de servidor duplo
- Arquitetura de duas camadas
- Recomendações do SQL Server

### <a name="single-server-architecture"></a>Arquitetura de servidor único

Para implantação em pequenas organizações ou para fins de desenvolvimento, você pode instalar o BHOLD e o FIM no mesmo servidor que SQL Server e AD DS, conforme mostrado na figura a seguir.
 
![Arquitetura de servidor único](media/bhold-installation-guide/single.png)

Quando o BHOLD Suite SP1 e o portal do FIM são instalados juntos em um único servidor, você deve criar diferentes aliases de host (registros CNAME ou A) no DNS para o BHOLD e para o FIM. Isso permite que nomes de entidade de serviço (SPNs) separados sejam criados para os serviços BHOLD e FIM. Para obter mais informações, consulte [instalação do BHOLD Core](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx).
Para obter orientação sobre como instalar o FIM em uma configuração de servidor único, consulte [configuração comum para introdução guias](https://technet.microsoft.com/library/ff575965.aspx) na biblioteca do Microsoft TechNet.

### <a name="dual-server-architecture"></a>Arquitetura de servidor duplo

A instalação do BHOLD Core e do FIM em servidores separados fornece maior desempenho e flexibilidade para organizações de médio porte que não exigem uma implantação mais complexa, como as fornecidas por arquiteturas de várias camadas. A figura a seguir mostra o BHOLD e o FIM instalados em seus próprios servidores; o servidor do FIM também está executando SQL Server para fornecer serviços de banco de dados para BHOLD e FIM. O serviço de sincronização do FIM em execução no servidor do FIM sincroniza as alterações entre os bancos de dados do FIM e do BHOLD. Observe que, se o autoatendimento do usuário final for necessário, o módulo integração de FIM com o BHOLD deverá ser instalado no mesmo servidor que o serviço FIM e o portal do FIM. O módulo integração de FIM com o BHOLD requer que o serviço FIM e o módulo integração de FIM do BHOLD sejam instalados no mesmo servidor.

![Arquitetura de servidor dual](media/bhold-installation-guide/dual.png)

> [!IMPORTANT]
> O recurso de relatório do módulo integração de FIM com o BHOLD exige que os bancos de dados BHOLD e FIM sejam instalados na mesma instância de SQL Server, e a conta de serviço do BHOLD deve ter direitos de acesso ao banco de dados do serviço FIM.

### <a name="two-tier-architecture"></a>Arquitetura de duas camadas

Na maioria dos ambientes, especialmente aqueles em que o desempenho é importante, você deve executar o BHOLD Suite SP1, o FIM e o SQL Server em servidores separados (arquitetura de duas camadas). Com uma arquitetura de duas camadas, os recursos de memória e CPU são dedicados para cada camada. A ilustração a seguir mostra uma maneira possível de configurar uma arquitetura de duas camadas. O serviço de sincronização do FIM em execução no servidor do FIM sincroniza as alterações entre os bancos de dados do FIM e do BHOLD. Observe que, se o autoatendimento do usuário final for necessário, o módulo integração do FIM com o BHOLD deverá ser instalado no mesmo servidor que o serviço e o portal do FIM.

![arquitetura de duas camadas](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>Recomendações do SQL Server

Se você estiver implantando o BHOLD em uma grande organização, é altamente recomendável que você siga estas diretrizes para configurar o banco de dados de Microsoft SQL Server:

- Implante SQL Server em um servidor separado de qualquer serviço FIM ou BHOLD.
- Isole o arquivo de log do arquivo de dados no nível do disco físico.
- Se você estiver usando RAID para fornecer redundância de armazenamento, use o nível de RAID 10 (1 + 0). Não use o RAID nível 5.
- Certifique-se de definir as configurações corretas ao usar mais de 2 GB de memória física para o servidor que executa o SQL Server.

Para obter mais informações sobre as práticas recomendadas SQL Server, consulte as [10 melhores práticas recomendadas de armazenamento](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx) na biblioteca do Microsoft TechNet.

### <a name="trusted-certificates-list-update"></a>Atualização da lista de certificados confiáveis

O Windows pode ser configurado para validar cadeias de certificados antes de iniciar um serviço. Nesses sistemas, um serviço não poderá ser iniciado se o código executável do serviço tiver sido assinado com um certificado que não está na TCL (lista de certificados confiáveis) do servidor. O software Microsoft BHOLD Suite SP1 é assinado por código usando uma cadeia de certificados de assinatura de código que se origina com o certificado 2010 da autoridade de certificação raiz da Microsoft.
O Windows pode ser configurado para recuperar certificados raiz da Microsoft por meio de uma conexão com a Internet. No entanto, em um sistema desconectado, o Windows Server inclui apenas os certificados presentes no programa raiz de cada vez antes do lançamento do Windows. Em versões do Windows Server anteriores ao Windows Server 2010, esses certificados não incluirão o certificado raiz necessário para validar a cadeia de certificados de assinatura de código do BHOLD Suite SP1. Se você pretende instalar um ou mais módulos do Microsoft BHOLD Suite SP1 em um sistema que pode não ter um TCL atualizado, você deve baixar e instalar o pacote de atualização raiz, ou usar Política de Grupo para instalar o pacote de atualização raiz, antes de instalar um BHOLD Suite SP1 modulo. Para obter mais informações, consulte [membros do programa de certificado raiz do Windows](http://support.microsoft.com/kb/931125).

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>Instalando o BHOLD Suite SP1 no Windows Server 2012/2016 etapa necessária 

![BHOLD de instalação do IIS](media/bhold-installation-guide/iis-install-bhold.png)

Se você instalar o BHOLD Suite SP1 no Windows Server 2012 ou 2016, as páginas da Web do BHOLD não estarão disponíveis até que você modifique o arquivo applicationHost. config localizado em ```C:\Windows\System32\inetsrv\config```. Na seção ```<globalModules>```, adicione ```preCondition="bitness64``` à entrada que começa ```<add name="SPNativeRequestModule"``` para que ele se leia da seguinte maneira:

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

Depois de editar e salvar o arquivo, execute o comando iisreset para redefinir o servidor IIS.


## <a name="upgrading-bhold-suite"></a>Atualizando o BHOLD Suite

Não é possível atualizar uma instalação existente do BHOLD Suite. Em vez disso, você deve desinstalar uma instalação existente do BHOLD Suite para poder atualizar os módulos BHOLD. Se você tiver um modelo de função BHOLD existente, poderá atualizar o banco de dados BHOLD e usá-lo quando instalar o módulo atualizado do BHOLD Core. Para obter mais informações, consulte [substituindo o BHOLD Suite pelo BHOLD Suite SP1](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx).


## <a name="next-steps"></a>Próximos passos

- [Referência para programadores do BHOLD](../reference/mim2016-bhold-developer-reference.md)
- [Histórico de versões do BHOLD](../reference/version-bhold-history.md)
