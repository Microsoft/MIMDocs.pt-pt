---
title: Licenciamento e downloads do Microsoft Identity Manager | Microsoft Docs
description: Este artigo descreve as abordagens de licenciamento de Microsoft Identity Manager (MIM) 2016, com ponteiros sobre onde baixar o software.
keywords: ''
author: markwahl-msft
ms.author: mwahl
manager: femila
ms.date: 10/18/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.reviewer: billmath
ms.suite: ems
ms.openlocfilehash: e0bfd868345b8e7dcc6a02e745d3ccbf632a6c58
ms.sourcegitcommit: b09a8c93983d9d92ca4871054650b994e9996ecf
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2019
ms.locfileid: "73329292"
---
# <a name="microsoft-identity-manager-2016-licensing-and-downloads"></a>Licenciamento e downloads do Microsoft Identity Manager 2016

Este artigo descreve as abordagens de licenciamento de Microsoft Identity Manager (MIM) 2016, com ponteiros sobre onde baixar o software.

## <a name="licensing-mim-for-your-organization"></a>Licenciamento do MIM para sua organização

O Microsoft Identity Manager 2016 é licenciado por usuário.  Os detalhes sobre o licenciamento estão incluídos nos termos do produto e documentos relacionados, que podem ser baixados na página [termos de licenciamento](https://www.microsoft.com/licensing/product-licensing/products.aspx) .

### <a name="licensing-for-azure-ad-premium-customers"></a>Licenciamento para clientes Azure AD Premium

Microsoft Identity Manager 2016 está incluído com Azure Active Directory Premium (P1 e P2), que faz parte do Enterprise Mobility + Security.

O Azure AD Premium está disponível por meio de um [Microsoft Enterprise Agreement](https://www.microsoft.com/licensing/licensing-programs/enterprise.aspx), o [programa Open Volume License](https://www.microsoft.com/licensing/licensing-programs/open-license.aspx)e o programa [provedores de soluções na nuvem](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409) . Os assinantes do Azure e do Office 365 também podem comprar Azure Active Directory Premium P1 e P2 online.  Leia mais em [preços de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="mim-cals"></a>CALs do MIM

Se você não tiver assinaturas do Azure Active Directory Premium para seus usuários e estiver usando mais recursos do MIM além da sincronização, uma [Cal (licença de acesso para cliente)](https://www.microsoft.com/licensing/product-licensing/client-access-license.aspx) será necessária para cada usuário cuja identidade seja gerenciada no mim. Se você quiser que usuários externos — como parceiros de negócios, prestadores de trabalho externos ou clientes — possam acessar o MIM, poderá adquirir CALs para cada um dos usuários externos ou adquirir licenças do conector externo (EC). Microsoft Identity Manager as CALs do 2016 não são necessárias para os usuários cuja identidade é apenas no serviço de sincronização Microsoft Identity Manager e não é gerenciada em nenhum outro componente do MIM.

### <a name="licenses-for-platform-components"></a>Licenças para componentes da plataforma

Uma licença do Windows Server é necessária para usar o software de servidor do Microsoft Identity Manager 2016 como um complemento do Windows Server. E uma implantação do MIM também requer uma instalação SQL Server.  As licenças do Windows Server e SQL Server não estão incluídas no MIM.

## <a name="obtaining-mim-software"></a>Obtendo o software MIM

Antes de iniciar uma nova instalação do MIM ou uma atualização de uma versão anterior, verifique se você tem as versões mais recentes.

Se você estiver iniciando uma instalação nova, será necessário baixar os arquivos de instalação para cada componente do MIM relevante para seu cenário. Em seguida, baixe todas as atualizações para esses arquivos e, em seguida, baixe os componentes adicionais que são downloads separados do centro de download.


| Cenário | Componente | Necessário para o cenário? | Nome da pasta ISO do DVD | Comentários |
|----------|-----------|---------------------   |-------------------|----------|--------------|
|Sincronização| Serviço de sincronização (incluindo conector para AD) | Sim | `Synchronization Service` | |
| Sincronização | PCNS | Não | `Password Change Notification Service` |  Para ser instalado em controladores de domínio |
| Sincronização | Conectores para LDAP, SQL, Web Services, PowerShell, Lotus Domino, Graph | Não | N/A | Distribuído por meio do centro de download |
| Gestão de Acesso Privilegiado | Serviço MIM | Sim | `Service and Portal` | |
| Self-service | Serviço do MIM, portal do MIM | Sim | `Service and Portal` | |
| Self-service | Suplementos e extensões | Não | `Add-ins and extensions` | Para ser instalado em computadores de usuários finais |
| Self-service | Relatórios do SCSM | Não | `Data Warehouse Support Scripts` | |
| Self-service | Agente de relatório híbrido | Não | N/A | Distribuído por meio do centro de download |
| Self-service | Pacotes de idiomas | Não | `LANGUAGE Packs` | |
| Gestão de Certificados | CM | Sim | `Certificate Management` | |
| Gestão de Certificados | Cliente em massa CM | Não | `CM Bulk Client` | |
| Gestão de Certificados | CM Client | Não | `CM Client`  | |
| Gestão de Certificados | Aplicativo CM para Windows | Não | `FIMCMModernApp*` | | |

### <a name="obtaining-windows-installer-packages"></a>Obtendo pacotes do Windows Installer

Para uma nova instalação, a maioria das organizações baixa os pacotes de instalação do MIM do [centro de serviços de licenciamento por volume](https://www.microsoft.com/licensing/servicecenter/default.aspx). 


O arquivo de DVD ISO contém uma pasta para cada componente do MIM: serviço de sincronização, serviço e portal, etc. Se você for instalar o software em um computador diferente do qual o baixou, certifique-se de copiar o arquivo ISO inteiro ou a pasta do componente: não copie apenas um arquivo MSI de uma pasta sem o restante dos arquivos e subpastas.

Se você não tiver acesso ao centro de serviços de licenciamento por volume, os clientes com uma assinatura de desenvolvedor apropriada também poderão baixar o MIM 2016 SP2 como um arquivo ISO do [Visual Studio meus benefícios downloads](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%202&pgroup=).  Pesquise "Microsoft Identity Manager 2016 com Service Pack 2".  

Se você não tiver acesso ao centro de serviços de licenciamento por volume e quiser apenas experimentar o software do MIM por um período limitado, poderá baixar uma [versão de avaliação do MIM 2016](https://www.microsoft.com/en-us/download/details.aspx?id=48244). Este software não se destina ao uso em produção e deixará de operar 180 dias após a primeira instalação e não poderá ser atualizado. A versão de avaliação requer o Windows Server 2008 R2, o Windows Server 2012 ou o Windows Server 2012 R2 para instalação.  Se você for novo no MIM e estiver aprendendo a tecnologia, tenha em mente que todos os cenários do MIM exigem um Active Directory domínio, um Windows Server e SQL Server estar presentes. Se você não tiver o Windows Server ou o SQL Server já presente, talvez queira tentar [provisionar uma VM com o SQL Server 2016 e o Windows Server 2016](https://azure.microsoft.com/blog/azure-images-sql-server-2016-on-windows-server-2016/).

### <a name="obtaining-updates"></a>Obtendo atualizações

Depois de instalar o MIM do MSI, você deve instalar os hotfixes necessários.

Verifique o [histórico de lançamento de versão do Identity Manager](./reference/version-history.md) para obter a versão de atualização mais recente, que tem um link para o site de download dos arquivos de patch do instalador.

Para determinar quais arquivos de atualização são necessários, esta tabela lista os componentes e o nome do arquivo de patch (MSP) correspondente em uma atualização.

| Cenário | Componente | Nome da pasta ISO do DVD | Nome do arquivo do patch de atualização correspondente |
|----------|-----------|-   |-------------------|----------|--------------|
|Sincronização| Serviço de sincronização | `Synchronization Service` | `MIMSyncService_x64*.msp` |
| Self-service | Serviço do MIM, portal do MIM | `Service and Portal` | `MIMService_x64*msp` |
| Self-service | Suplementos e extensões | `Add-ins and extensions` | `MIMAddinsExtensions*msp` |
| Self-service | Pacotes de idiomas | `LANGUAGE Packs` | `LANGUAGE Packs.zip` |
| Gerenciamento de acesso (BHOLD) | BHOLD | `BHOLD` | `AccessManagementConnector.msi`, `BHOLD*.msi` |
| Gestão de Certificados | CM |  `Certificate Management` | `MIMCM*.msp` |
| Gestão de Certificados | Cliente em massa CM |  `CM Bulk Client` |`MIMCMBulkClient*msp` |
| Gestão de Certificados | CM Client | `CM Client` |`MIMCMClient*msp` |

Certifique-se de ler as notas de versão associadas à atualização antes de instalar o arquivo MSP.

As atualizações para [BHOLD](https://www.microsoft.com/download/details.aspx?id=55950) não são distribuídas como arquivos MSP, somente como instaladores msi.

### <a name="additional-downloads"></a>Downloads adicionais

Os seguintes downloads também podem ser relevantes:

- [Agente de relatório híbrido do MIM](https://www.microsoft.com/download/details.aspx?id=55112)

- [Conector LDAP genérico, conector do SQL genérico, conector do Graph, conector do Lotus Domino, conector do PowerShell, conector de serviços Web](http://go.microsoft.com/fwlink/?LinkId=717495)

- [Conector para repositório de perfis de usuário do SharePoint](https://www.microsoft.com/download/details.aspx?id=41164)

- Se você ainda não tiver um domínio Active Directory e estiver configurando um cenário do PAM para experimentação, consulte os [scripts de implantação do Pam do MIM 2016 SP1](sp1-deployment-scripts.md).

## <a name="next-steps"></a>Próximos passos

- Saiba mais sobre os cenários fornecidos no [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md).
- Leia o [Guia de planejamento de capacidade](capacity-planning-guide.md).
- Implante o MIM em um [cenário de sincronização](microsoft-identity-manager-deploy.md) ou no cenário de gerenciamento de [acesso privilegiado](./pam/privileged-identity-management-for-active-directory-domain-services.md).

