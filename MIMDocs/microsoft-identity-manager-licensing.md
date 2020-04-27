---
title: Licenciamento e downloads do Microsoft Identity Manager / Microsoft Docs
description: Este artigo descreve as abordagens para licenciar o Microsoft Identity Manager (MIM) 2016, com indicações sobre onde descarregar o software.
keywords: ''
author: markwahl-msft
ms.author: mwahl
manager: daveba
ms.date: 10/18/2019
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.reviewer: billmath
ms.suite: ems
ms.openlocfilehash: 62a6b936dc30b8593c8758a6158244d0f6dbbc4f
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2020
ms.locfileid: "79043023"
---
# <a name="microsoft-identity-manager-2016-licensing-and-downloads"></a>Microsoft Identity Manager 2016 licenciamento e downloads

Este artigo descreve as abordagens para licenciar o Microsoft Identity Manager (MIM) 2016, com indicações sobre onde descarregar o software.

## <a name="licensing-mim-for-your-organization"></a>Licenciamento MIM para a sua organização

O Microsoft Identity Manager 2016 é licenciado por utilizador.  Os detalhes sobre o licenciamento estão incluídos nos Termos do Produto e documentos relacionados, que podem ser descarregados a partir da página de termos de [licenciamento.](https://www.microsoft.com/licensing/product-licensing/products.aspx)

### <a name="licensing-for-azure-ad-premium-customers"></a>Licenciamento para clientes Azure AD Premium

O Microsoft Identity Manager 2016 está incluído no Azure Ative Directory Premium (P1 e P2), que faz parte da Enterprise Mobility + Security.

O Azure AD Premium está disponível através de um [Acordo de Empresa da Microsoft,](https://www.microsoft.com/licensing/licensing-programs/enterprise.aspx)do Programa de Licença seletiva de Volume [Aberto](https://www.microsoft.com/licensing/licensing-programs/open-license.aspx)e do programa Cloud [Solution Providers.](https://go.microsoft.com/fwlink/?LinkId=614968&clcid=0x409) Os subscritores do Azure e do Office 365 também podem comprar online o Azure Ative Directory Premium P1 e P2.  Leia mais na [Azure Ative Directory pricing](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="mim-cals"></a>MIM CALs

Se não tiver subscrições Azure Ative Directory Premium para os seus utilizadores, e estiver a utilizar mais capacidades MIM para além da sincronização, é necessária uma Licença de [Acesso ao Cliente (CAL)](https://www.microsoft.com/licensing/product-licensing/client-access-license.aspx) para cada utilizador cuja identidade é gerida em MIM. Se quiser que utilizadores externos , como parceiros de negócio, empreiteiros externos ou clientes - possam aceder à MIM, pode adquirir CALs para cada um dos seus utilizadores externos ou adquirir licenças de Conector Externo (CE). Os CALs do Microsoft Identity Manager 2016 não são necessários para utilizadores cuja identidade se encontra apenas no serviço de sincronização do Microsoft Identity Manager e não são geridos em qualquer outro componente MIM.

### <a name="licenses-for-platform-components"></a>Licenças para componentes de plataforma

Uma licença do Windows Server é necessária para usar o software de servidor do Microsoft Identity Manager 2016 como um add-on do Windows Server. E uma implementação mim também requer uma instalação do SQL Server.  As licenças do Windows Server e do SQL Server não estão incluídas com mim.

## <a name="obtaining-mim-software"></a>Obtenção de software MIM

Antes de iniciar uma nova instalação de MIM ou uma atualização de uma versão anterior, certifique-se de que tem as versões mais recentes.

Se estiver a iniciar uma nova instalação, terá de descarregar os ficheiros de instalação de cada componente MIM que seja relevante para o seu cenário. Em seguida, descarregue quaisquer atualizações para esses ficheiros e, em seguida, descarregue quaisquer componentes adicionais que sejam downloads separados do Centro de Descarregamento.


| Cenário | Componente | Necessário para o cenário? | Nome da pasta ISO DVD | Comentários |
|----------|-----------|---------------------   |-------------------|----------|--------------|
|Sincronização| Serviço de Sincronização (incluindo conector a AD) | Sim | `Synchronization Service` | |
| Sincronização | PCNS | Não | `Password Change Notification Service` |  Para ser instalado em controladores de domínio |
| Sincronização | Conectores para LDAP, SQL, Web Services, PowerShell, Lotus Domino, Graph | Não | N/D | Distribuído via Centro de Descarregamento |
| Gestão de Acesso Privilegiado | Serviço MIM | Sim | `Service and Portal` | |
| Gestão personalizada | Serviço MIM, Portal MIM | Sim | `Service and Portal` | |
| Gestão personalizada | Suplementos e extensões | Não | `Add-ins and extensions` | Para ser instalado em Computadores de utilizador final |
| Gestão personalizada | Relatório scsm | Não | `Data Warehouse Support Scripts` | |
| Gestão personalizada | Agente de reporte híbrido | Não | N/D | Distribuído via Centro de Descarregamento |
| Gestão personalizada | Pacotes de idiomas | Não | `LANGUAGE Packs` | |
| Gestão de Certificados | CM | Sim | `Certificate Management` | |
| Gestão de Certificados | Cliente a granel CM | Não | `CM Bulk Client` | |
| Gestão de Certificados | Cliente CM | Não | `CM Client`  | |
| Gestão de Certificados | App CM para Windows | Não | `FIMCMModernApp*` | | |

### <a name="obtaining-windows-installer-packages"></a>Obtenção de pacotes de instaladores windows

Para uma nova instalação, a maioria das organizações descarrega os pacotes de instalação MIM do Centro de Serviços de [Licenciamento de Volume.](https://www.microsoft.com/licensing/servicecenter/default.aspx) 


O ficheiro DVD ISO contém uma pasta para cada componente MIM: Serviço de Sincronização, Serviço e Portal, etc. Se vai instalar o software num computador diferente do qual o descarregou, certifique-se de copiar todo o ficheiro ISO ou a pasta para o componente: não copie apenas um ficheiro MSI de uma pasta sem os restantes ficheiros e subpastas.

Se não tiver acesso ao Centro de Serviços de Licenciamento de Volume, os clientes com uma subscrição de programador apropriado também podem baixar o MIM 2016 SP2 como um ficheiro ISO do [Visual Studio My Benefits Downloads.](https://my.visualstudio.com/Downloads?q=Microsoft%20Identity%20Manager%202016%20with%20Service%20Pack%202&pgroup=)  Procure "Microsoft Identity Manager 2016 com Service Pack 2".  

Se não tiver acesso ao Centro de Serviços de Licenciamento de Volume e pretender apenas experimentar o software MIM por um período limitado, pode descarregar uma [versão de avaliação do MIM 2016](https://www.microsoft.com/en-us/download/details.aspx?id=48244). Este software não se destina a ser utilizado na produção e deixará de funcionar 180 dias após a primeira instalação e não pode ser atualizado. A versão de avaliação requer o Windows Server 2008 R2, Windows Server 2012 ou Windows Server 2012 R2 para instalação.  Se é novo na MIM e na aprendizagem da tecnologia, lembre-se que todos os cenários de MIM requerem um domínio de Diretório Ativo, um Servidor Windows e Servidor SQL para estar presente. Caso não tenha já presente o Windows Server ou o SQL Server, poderá desejar experimentar o fornecimento de [um VM com O Servidor SQL 2016 e Windows Server 2016](https://azure.microsoft.com/blog/azure-images-sql-server-2016-on-windows-server-2016/).

### <a name="obtaining-updates"></a>Obtenção de atualizações

Depois de instalar mim a partir de MSI, deverá instalar os hotfixes necessários.

Verifique o histórico de lançamento da [versão Do Gestor](./reference/version-history.md) de Identidade para obter o mais recente lançamento da atualização, que tem um link para o site de descarregamento para os ficheiros de patch do instalador.

Para determinar quais os ficheiros de atualização necessários, esta tabela lista os componentes e o nome do ficheiro patch correspondente (MSP) numa atualização.

| Cenário | Componente | Nome da pasta ISO DVD | Nome de ficheiro de patch de atualização correspondente |
|----------|-----------|-   |-------------------|----------|--------------|
|Sincronização| Serviço de Sincronização | `Synchronization Service` | `MIMSyncService_x64*.msp` |
| Gestão personalizada | Serviço MIM, Portal MIM | `Service and Portal` | `MIMService_x64*msp` |
| Gestão personalizada | Suplementos e extensões | `Add-ins and extensions` | `MIMAddinsExtensions*msp` |
| Gestão personalizada | Pacotes de idiomas | `LANGUAGE Packs` | `LANGUAGE Packs.zip` |
| Gestão de acessos (BHOLD) | BHOLD | `BHOLD` | `AccessManagementConnector.msi`, `BHOLD*.msi` |
| Gestão de Certificados | CM |  `Certificate Management` | `MIMCM*.msp` |
| Gestão de Certificados | Cliente a granel CM |  `CM Bulk Client` |`MIMCMBulkClient*msp` |
| Gestão de Certificados | Cliente CM | `CM Client` |`MIMCMClient*msp` |

Certifique-se de que lê quaisquer notas de lançamento associadas à atualização antes da instalação do ficheiro MSP.

As atualizações para [o BHOLD](https://www.microsoft.com/download/details.aspx?id=55950) não são distribuídas como ficheiros MSP, apenas como instaladores mSI.

### <a name="additional-downloads"></a>Downloads adicionais

Os seguintes downloads também podem ser relevantes:

- [Agente de reporte híbrido mim](https://www.microsoft.com/download/details.aspx?id=55112)

- [Conector LDAP genérico, Conector Genérico SQL, Conector gráfico, Conector Lotus Domino, Conector PowerShell, Conector de Serviços Web](http://go.microsoft.com/fwlink/?LinkId=717495)

- [Conector para Loja de Perfis do Utilizador SharePoint](https://www.microsoft.com/download/details.aspx?id=41164)

- Se ainda não tiver um domínio de Diretório Ativo e estiver a criar um cenário PAM para experimentação, consulte os scripts de [implementação PAM MIM 2016 .](sp1-deployment-scripts.md)

## <a name="next-steps"></a>Passos seguintes

- Saiba mais sobre os cenários entregues no [Microsoft Identity Manager 2016](microsoft-identity-manager-2016.md).
- Leia o guia de planeamento de [capacidades.](capacity-planning-guide.md)
- Implementar mim para um cenário de [sincronização](microsoft-identity-manager-deploy.md) ou o cenário de gestão de [acesso privilegiado.](./pam/privileged-identity-management-for-active-directory-domain-services.md)

