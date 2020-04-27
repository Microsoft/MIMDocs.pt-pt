---
title: Definir funções com privilégios para o PAM | Documentos da Microsoft
description: Decidir que funções com privilégios devem ser geridas e definir a política de gestão para cada uma.
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 1a368e8e-68e1-4f40-a279-916e605581bc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f05769a7d1db38ecde200e18e45c6ca29a75b756
ms.sourcegitcommit: a96944ac96f19018c43976617686b7c3696267d7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/21/2020
ms.locfileid: "79044043"
---
# <a name="define-roles-for-privileged-access-management"></a>Definir funções para o Privileged Access Management

Com o Privileged Access Management, pode atribuir utilizadores a funções com privilégios, que estes podem ativar conforme necessário para obterem acesso just-in-time. Estas funções são definidas manualmente e estabelecidas no ambiente bastion. Este artigo orienta-o no processo de decidir que funções gerir através do PAM e como defini-las com as restrições e permissões adequadas.

Uma abordagem simples à definição de funções para o Privileged Access Management consiste em compilar todas as informações numa folha de cálculo. Crie uma lista das funções nas funções e utilize as colunas para identificar os requisitos de governação e as permissões.

Os requisitos de governação variam consoante as políticas de identidade e acesso existentes ou requisitos de conformidade. Os parâmetros para identificar para cada função podem incluir:

- O dono do papel.
- Os utilizadores candidatos que podem estar nesse papel
- Os controlos de autenticação, aprovação ou notificação que devem estar associados à utilização da função.

As permissões de função dependem das aplicações que estão a ser geridas. Este artigo utiliza o Active Directory como uma aplicação de exemplo, dividindo as permissões em duas categorias:

- As necessárias para gerir o serviço Active Directory propriamente dito (por exemplo, configurar a topologia de replicação)

- As necessárias para gerir os dados contidos no Active Directory (por exemplo, criar utilizadores e grupos)

## <a name="identify-roles"></a>Identificar funções

Comece por identificar todas as funções que poderá querer gerir com o PAM. Na folha de cálculo, cada função potencial terá a sua própria linha.

Para encontrar as funções adequadas, considere cada aplicação no âmbito de gestão:

- A aplicação é de [nível 0, nível 1 ou nível 2?](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)
- Quais são os privilégios que afetam a confidencialidade, a integridade ou a disponibilidade da aplicação?
- A aplicação tem dependências de outros componentes do sistema? Por exemplo, tem dependências de bases de dados, networking, infraestruturas de segurança, virtualização ou plataforma de hospedagem?

Determine como agrupar essas considerações sobre aplicações. Quer funções que tenham limites claros e conceder apenas permissões suficientes para realizar tarefas administrativas comuns na aplicação.

É sempre melhor conceber funções para a atribuição de menor privilégio. Esta atribuição pode basear-se nas responsabilidades organizacionais atuais (ou planeadas) dos utilizadores e incluir o privilégio necessário aos deveres do utilizador. Também podia incluir os privilégios que simplificam as operações, sem criar risco.

Outras considerações na determinação do âmbito das permissões para incluir uma função são:

- Quantas pessoas estão a trabalhar numa função específica? Se não existirem, pelo menos, duas pessoas, significa que poderá estar definida de forma demasiado estrita para ser útil ou que definiu os deveres de uma pessoa específica.

- Quantas funções desempenha uma pessoa? Os utilizadores conseguiriam selecionar a função certa para a respetiva tarefa?

- O universo de utilizadores e a forma como interagem com as aplicações seria compatível com o Privileged Access Management?

- É possível separar a administração e a auditoria, para que um utilizador numa função administrativa não possa apagar os registos de auditoria das respetivas ações?

## <a name="establish-role-governance-requirements"></a>Estabelecer requisitos de governação de funções

À medida que identificar funções candidatas, comece a preencher a folha de cálculo. Crie colunas para os requisitos que são relevantes para a sua organização. Alguns requisitos a ter em consideração incluem:

- Quem é o proprietário de função que seria responsável pela definição adicional da função, por escolher permissões e por manter as definições de governação para a função?

- Quem são os proprietários (utilizadores) da função que irão desempenhar os deveres ou as tarefas da função?

- Que método de acesso (abordado na secção seguinte) seria apropriado para os proprietários da função?

- É preciso a aprovação manual por um proprietário de função quando um utilizador ativa a respetiva função?

- É preciso uma notificação quando um utilizador ativa a respetiva função?

- A utilização desta função deve gerar um alerta ou uma notificação num sistema SIEM, para fins de controlo?

- É necessário restringir os utilizadores que ativam a função para conseguirem apenas iniciar sessão em computadores nos quais seja preciso acesso para desempenhar os deveres da função, e nos quais exista verificação suficiente do anfitrião para permitir proteger os privilégios/credenciais contra utilização indevida?

- É preciso fornecer uma estação de trabalho de administrador dedicada aos proprietários de função?

- Que permissões de aplicação (veja a lista de exemplo para o AD abaixo) estão associadas a esta função?

## <a name="select-an-access-method"></a>Selecionar um método de acesso

Pode haver múltiplas funções num sistema de gestão de acesso privilegiado com as mesmas permissões que lhes são atribuídas. Isto pode acontecer se diferentes comunidades de utilizadores tiverem requisitos distintos de governação de acesso. Por exemplo, uma organização pode aplicar políticas diferentes aos seus funcionários a tempo inteiro do que as políticas que aplica a funcionários de TI externos de outra organização.

Em alguns casos, um utilizador pode ser permanentemente atribuído a um papel. Nesse caso, não precisam de solicitar ou ativar uma atribuição de funções. Exemplos de cenários de atribuição permanente incluem:

- Uma conta de serviço gerida numa floresta existente

- Uma conta de utilizador na floresta existente, com uma credencial gerida fora da PAM. Isto pode ser uma conta de "quebrar vidros". A conta de vidro de rutura pode precisar de um papel como "Manutenção de Domínio /DC" para corrigir problemas como confiança e problemas de saúde em DC. Como conta de vidro de rutura, teria o papel permanentemente atribuído com uma senha fisicamente segura)

- Uma conta de utilizador na floresta administrativa que autentica com uma senha. Isto pode ser, um utilizador que necessite de permissões administrativas permanentes de 24x7 e login de um dispositivo que não suporta autenticação forte.

- Uma conta de utilizador na floresta administrativa, com um smart card ou smart card virtual (por exemplo, uma conta com um smart card offline, necessária para tarefas de manutenção raras)

Para as organizações preocupadas com a possibilidade de roubo ou de utilização indevida de credenciais, o guia [Utilizar o MFA do Azure para ativação](use-azure-mfa-for-activation.md) inclui instruções sobre como configurar o MIM para exigir uma verificação extraordinária no momento da ativação da função.

## <a name="delegate-active-directory-permissions"></a>Delegar permissões do Active Directory

O Windows Server cria automaticamente grupos predefinidos, tais como "Admins do domínio" quando são criados novos domínios. Estes grupos simplificam a introdução e podem ser adequados para organizações mais pequenas. As organizações maiores, ou as que necessitam de um maior isolamento dos privilégios administrativos, devem esvaziar esses grupos e substituí-los por grupos que forneçam permissões de grãos finos.

Uma limitação do grupo Admins do domínio é o facto de não poder ter membros de um domínio externo. Outra limitação é o facto de conceder permissões para três funções distintas:

- Gerir o serviço do Active Directory
- Gerir os dados contidos no Active Directory
- Ativar o início de sessão remoto em computadores associados a um domínio.

Em vez de grupos padrão como domain admins, crie novos grupos de segurança que fornecem apenas as permissões necessárias. Em seguida, deve utilizar a MIM para fornecer dinamicamente contas de administradores com esses membros do grupo.

### <a name="service-management-permissions"></a>Permissões de gestão de serviços

A tabela seguinte fornece exemplos de permissões que seriam relevantes de incluir em funções para gerir o AD.

| Função | Descrição |
| ---- | ---- |
| Manutenção de Domínio/DC | A adesão ao grupo Domain\Administrators permite a resolução de problemas e a alteração do sistema operativo do controlador de domínio. Operações como promover um novo controlador de domínio para um domínio existente na floresta e delegação de papel da AD.
|Gerir DCs Virtuais | Gerir máquinas virtuais de (VMs) de controladores de domínio (DC) com o software de gestão de virtualização. Este privilégio pode ser concedido através de controlo total de todas as máquinas virtuais na ferramenta de gestão ou da funcionalidade de controlo de acesso baseado em funções (RBAC). |
| Expandir o esquema | Gerir o esquema, incluindo a adição de novas definições de objetos, alteração de permissões para objetos de esquema e alteração de permissões predefinidas de esquema para tipos de objeto |
| Cópia de Segurança da Base de Dados do Active Directory | Fazer uma cópia de segurança da Base de Dados do Active Directory na íntegra, incluindo todos os segredos conferidos ao DC e ao Domínio. |
| Gerir Confianças e Níveis Funcionais | Criar e eliminar confianças com florestas e domínios externos. |
| Gerir Sites, Sub-redes e Replicação | Gerir os objetos da topologia de replicação do Active Directory, incluindo a modificação de sites, sub-redes e objetos de ligação de sites e o início de operações de replicação |
| Gerir GPOs | Criar, eliminar e modificar objetos de política de grupo em todo o domínio |
| Gerir Zonas | Criar, eliminar e modificar Zonas DNS e objetos no Active Directory |
| Modificar UOs de Camada 0 | Modificar UOs de camada 0 e objetos contidos no Active Directory |

### <a name="data-management-permissions"></a>Permissões de gestão de dados

O quadro seguinte dá exemplos de permissões que seriam relevantes para incluir em funções de gestão ou utilização dos dados detidos em AD.

| Função | Descrição |
| ---- | ---- |
| Modificar UO de Administrador de Camada 1                 | Modificar UOs que contêm objetos de Administrador de Camada 1 no Active Directory |
| Modificar UO de Administrador de Camada 2                 | Modificar UOs que contêm objetos de Administrador de Camada 2 no Active Directory |
| Gestão de Contas: Criar/Eliminar/Mover | Modificar contas de utilizador padrão                                      |
| Gestão de Contas: Repor/Desbloquear       | Repor palavras-passe e desbloquear contas                                  |
| Grupo de Segurança: Criar/Modificar          | Criar e modificar grupos de segurança no Active Directory              |
| Grupo de Segurança: Eliminar                 | Eliminar grupos de segurança no Active Directory                         |
| Gestão de GPOs                         | Gerir todos os GPOs no domínio/floresta que não afetam servidores de camada 0             |
| Associar Admin Local/PC                    | Direitos administrativos locais para todas as estações de trabalho                               |
| Associar Admin Local/Srv                   | Direitos administrativos locais para todos os servidores                                    |

## <a name="example-role-definitions"></a>Exemplo de definições de funções

A escolha das definições de papéis depende do nível de gestão dos servidores. Isto também depende da escolha das candidaturas que estão a ser geridas. Aplicações como o Exchange ou produtos empresariais de terceiros, como o SAP, muitas vezes trarão as suas próprias definições de funções adicionais para a administração delegada.

As secções seguintes fornecem exemplos para cenários típicos de empresas.

### <a name="tier-0---administrative-forest"></a>Camada 0 - Floresta administrativa

As funções adequadas a contas no ambiente bastion poderão incluir:

- Acesso de emergência à floresta administrativa
- Administradores "Red Card": utilizadores que são administradores da floresta administrativa
- Utilizadores que são administradores da floresta de produção
- Utilizadores a quem são delegados direitos administrativos a aplicações na floresta de produção

### <a name="tier-0---enterprise-production-forest"></a>Camada 0 - Floresta de produção empresarial

As funções adequadas à gestão das contas e recursos da floresta de produção de camada 0 poderão incluir:

- Acesso de emergência à floresta de produção
- Administradores de Política de Grupo
- Administradores de DNS
- Administradores de PKI
- Administradores de replicação e topologia do AD
- Administradores de virtualização para servidores de camada 0
- Administradores de armazenamento
- Administradores de antimalware para servidores de camada 0
- Administradores do SCCM para o SCCM de camada 0
- Administradores do SCOM para o SCOM de camada 0
- Administradores de cópia de segurança para a camada 0
- Utilizadores de controladores BMC e fora de banda (para gestão integral ou de KVM) ligados a anfitriões de camada 0

### <a name="tier-1"></a>Nível 1

As funções para a gestão e cópia de segurança de servidores na camada 1 poderão incluir:

- Manutenção de servidores
- Administradores de virtualização para servidores de camada 1
- Conta de Analista de Segurança
- Administradores de antimalware para servidores de camada 1
- Administradores do SCCM para o SCCM de camada 1
- Administradores do SCOM para o SCOM de camada 1
- Administradores de cópia de segurança para servidores de camada 1
- Utilizadores de controladores BMC e fora de banda (para gestão integral ou de KVM) ligados a anfitriões de camada 1

Além disso, as funções para a gestão de aplicações empresariais na camada 1 poderão incluir:

- Administradores do DHCP
- Administradores do Exchange
- Administradores do Skype para Empresas
- Administradores de farms do SharePoint
- Administradores de um serviço em nuvem, por exemplo, um site de uma empresa ou DNS público
- Administradores de sistemas HCM, Financeiros e Jurídicos

### <a name="tier-2"></a>Camada 2

As funções para a gestão de utilizadores e computadores não administrativos poderão incluir:

- Administradores de contas
- Suporte técnico
- Administradores de grupos de segurança
- Suporte de deskside de estações de trabalho

## <a name="next-steps"></a>Passos seguintes

- [Proteger Material de Referência de Acesso Privilegiado](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)
- [Utilizar o MFA do Azure para ativação](use-azure-mfa-for-activation.md)
