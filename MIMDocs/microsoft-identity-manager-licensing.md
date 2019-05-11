---
title: Downloads e licenciamento do Microsoft Identity Manager | Documentos da Microsoft
description: Este artigo descreve as abordagens de licenciamento do Microsoft Identity Manager (MIM) 2016, com ponteiros sobre onde a baixar o software.
keywords: ''
author: markwahl-msft
ms.author: markwahl-msft
manager: femila
ms.date: 02/25/2019
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.reviewer: billmath
ms.suite: ems
ms.openlocfilehash: 7c5ab987801c63dca2457025442a35560dca3b86
ms.sourcegitcommit: 225fca802d6b59bdc98d50020b297eb6393f70eb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/26/2019
ms.locfileid: "64518738"
---
# <a name="microsoft-identity-manager-2016-licensing-and-downloads"></a>Licenciamento do Microsoft Identity Manager 2016 e downloads

Este artigo descreve as abordagens de licenciamento do Microsoft Identity Manager (MIM) 2016, com ponteiros sobre onde a baixar o software.

## <a name="licensing-mim-for-your-organization"></a>Licenciamento de MIM para a sua organização

Microsoft Identity Manager 2016 está licenciado numa base por utilizador.  Os detalhes sobre licenciamento são incluídos nos termos do produto e relacionado com documentos, o que podem ser transferidos a partir da [termos de licenciamento](https://www.microsoft.com/en-us/licensing/product-licensing/products.aspx) página.

### <a name="licensing-for-azure-ad-premium-customers"></a>Licenciamento para os clientes do Azure AD Premium

Microsoft Identity Manager 2016 está incluído no Azure Active Directory Premium (P1 e P2), o que faz parte do Enterprise Mobility + Security.

O Azure AD Premium está disponível por meio de um [contrato Enterprise da Microsoft](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), o [programa de licenciamento em Volume Open](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx)e o [fornecedores de soluções Cloud](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409) programa. Subscritores do Azure e do Office 365 também podem comprar Azure Active Directory Premium P1 e P2 online.  Leia mais sobre [preços do Azure Active Directory](https://azure.microsoft.com/en-us/pricing/details/active-directory/).

### <a name="mim-cals"></a>CALs MIM

Se não tem subscrições do Azure Active Directory Premium para os seus utilizadores e estiver a utilizar mais capacidades MIM para além de sincronização, em seguida, um [licença de acesso de cliente (CAL)](https://www.microsoft.com/en-us/licensing/product-licensing/client-access-license.aspx) é necessária para cada utilizador cuja identidade é gerida em MIM. Se pretender que os utilizadores externos — como os clientes, prestadores de serviço externos ou parceiros de negócios — para poder aceder a MIM, pode adquirir CALs para cada um dos seus utilizadores externos ou adquirir licenças External Connector (EC). CALs do Microsoft Identity Manager 2016 não são necessários para os utilizadores cuja identidade é apenas no serviço de sincronização do Microsoft Identity Manager e não for gerenciada em algum outro componente de MIM.

### <a name="licenses-for-platform-components"></a>Licenças para os componentes de plataforma

É necessário ter uma licença do Windows Server para utilizar o software de servidor do Microsoft Identity Manager 2016 como um complemento do Windows Server. E uma implementação de MIM também requer uma instalação do SQL Server.  Licenças do Windows Server e SQL Server não estão incluídas em MIM.

## <a name="obtaining-mim-software"></a>Obter o MIM software

Antes de iniciar uma instalação nova de MIM ou uma atualização de uma versão anterior, certifique-se de que tem as versões mais recentes.

Se estiver iniciando uma nova instalação, terá de transferir os ficheiros de instalação para cada componente de MIM que são relevantes para o seu cenário. Em seguida, transferir as atualizações para esses arquivos e, em seguida, transferir os componentes adicionais que são downloads separados do Centro de Download.


| Cenário | Componente | Necessário para o cenário? | Nome da pasta de ISO de DVD | Comentários |
|----------|-----------|---------------------   |-------------------|----------|--------------|
|Sincronização| (Incluindo o conector para o AD) do serviço de sincronização | Sim | `Synchronization Service` | |
| Sincronização | PCNS | Não | `Password Change Notification Service` |  Para ser instalado em controladores de domínio |
| Sincronização | Criar um gráfico de conectores para LDAP, SQL, Web Services, PowerShell, Lotus Domino | Não | N/A | Distribuído por meio do Centro de transferências |
| Privileged Access Management | Serviço MIM | Sim | `Service and Portal` | |
| Self-service | Serviço MIM, o Portal de MIM | Sim | `Service and Portal` | |
| Self-service | Suplementos e extensões | Não | `Add-ins and extensions` | Para ser instalada em PCs de utilizador final |
| Self-service | Relatórios do SCSM | Não | `Data Warehouse Support Scripts` | |
| Self-service | Agente de relatórios híbridos | Não | N/A | Distribuído por meio do Centro de transferências |
| Self-service | Pacotes de idiomas | Não | `LANGUAGE Packs` | |
| Gestão de Certificados | CM | Sim | `Certificate Management` | |
| Gestão de Certificados | Cliente CM em massa | Não | `CM Bulk Client` | |
| Gestão de Certificados | Cliente CM | Não | `CM Client`  | |
| Gestão de Certificados | Aplicação CM para Windows | Não | `FIMCMModernApp*` | | |

### <a name="obtaining-windows-installer-packages"></a>Obter pacotes de instalador do Windows

Para uma nova instalação, a maioria das organizações transferir os pacotes de instalação de MIM a [Volume Licensing Service Center](https://www.microsoft.com/licensing/servicecenter/default.aspx). 


O arquivo ISO de DVD contém uma pasta para cada componente de MIM: Sincronização Service, serviço e Portal, etc. Se pretender instalar o software num computador diferente do qual baixou, certifique-se de que copiar todo o arquivo ISO ou a pasta para o componente: não simplesmente copiar apenas um arquivo MSI fora de uma pasta sem o resto dos ficheiros e subpastas.

Se não tiver acesso ao centro de serviço de licenciamento em Volume, os clientes com uma assinatura de desenvolvedor apropriado também podem transferir MIM 2016 SP1 como um arquivo ISO de [meus benefícios transferências do Visual Studio](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%201&pgroup=).  Procure "Microsoft Identity Manager 2016 Service Pack 1".  

Se não tiver acesso para o Volume Licensing Service Center e simplesmente desejar experimentar o software MIM durante um período limitado, pode baixar uma [versão de avaliação de MIM 2016](https://www.microsoft.com/en-us/download/details.aspx?id=48244). Este software não se destina a utilização de produção e deixará de 180 dias após a primeira instalação de operar e não pode ser atualizado. A versão de avaliação requer o Windows Server 2008 R2, Windows Server 2012 ou Windows Server 2012 R2 para instalação.  Se estiver familiarizado com o MIM e a tecnologia de aprendizagem, tenha em atenção que todos os cenários MIM requerem um domínio do Active Directory, um Windows Server e o SQL Server esteja presente. Se não tiver o Windows Server ou SQL Server já está presente, pode ser útil tentar [aprovisionamento de uma VM com o SQL Server 2016 e Windows Server 2016](https://azure.microsoft.com/en-us/blog/azure-images-sql-server-2016-on-windows-server-2016/).

### <a name="obtaining-updates"></a>Obter atualizações

Depois de instalar o MIM de MSI, em seguida deve instalar as correções necessárias.

Verifique os [histórico de lançamento de versão do Identity Manager](./reference/version-history.md) para a versão de atualização mais recente, o que tem uma ligação ao site de download para os ficheiros de patch do instalador.

Para determinar quais os ficheiros de atualização são necessários, esta tabela lista os componentes e o nome do ficheiro de patch (MSP) correspondente numa atualização.

| Cenário | Componente | Nome da pasta de ISO de DVD | Nome do ficheiro de patch de atualização correspondente |
|----------|-----------|-   |-------------------|----------|--------------|
|Sincronização| Serviço de sincronização | `Synchronization Service` | `FIMSyncService_x64*.msp` |
| Self-service | Serviço MIM, o Portal de MIM | `Service and Portal` | `FIMService_x64*msp` |
| Self-service | Suplementos e extensões | `Add-ins and extensions` | `FIMAddinsExtensions*msp` |
| Self-service | Pacotes de idiomas | `LANGUAGE Packs` | `LANGUAGE Packs.zip` |
| Gestão de acesso (BHOLD) | BHOLD | `BHOLD` | `AccessManagementConnector.msi`, `BHOLD*.msi` |
| Gestão de Certificados | CM |  `Certificate Management` | `FIMCM*.msp` |
| Gestão de Certificados | Cliente CM em massa |  `CM Bulk Client` |`FIMCMBulkClient*msp` |
| Gestão de Certificados | Cliente CM | Cliente CM |`FIMCMClient*msp` |

É claro que ler qualquer notas de versão associado a atualização antes de instalar o arquivo MSP.

Atualiza para [BHOLD](https://www.microsoft.com/en-us/download/details.aspx?id=55950) não são distribuídos como arquivos MSP, apenas como programas de instalação do MSI.

### <a name="additional-downloads"></a>Downloads adicionais

Os downloads a seguir podem também ser relevantes:

- [Agente MIM Hybrid Reporting](https://www.microsoft.com/download/details.aspx?id=55112)

- [Conector LDAP genérico, conector SQL genérico, conector de gráfico, Lotus Domino Connector, conector de PowerShell, conector de serviços da Web](http://go.microsoft.com/fwlink/?LinkId=717495)

- [Conector para Store de perfil de usuário do SharePoint](https://www.microsoft.com/en-us/download/details.aspx?id=41164)

- Se ainda não tiver um domínio do Active Directory e estiver a configurar um cenário de PAM para a experimentação, consulte a [scripts de implementação da PAM de MIM 2016 SP1](sp1-deployment-scripts.md).

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais cenários no fornecidos no [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md).
- Leitura a [guia de planeamento de capacidade](capacity-planning-guide.md).
- Implemente o MIM para uma [cenário de sincronização](microsoft-identity-manager-deploy.md) ou o [cenário de gestão de acesso privilegiado](./pam/privileged-identity-management-for-active-directory-domain-services.md).

