---
title: História da versão do Gestor de Identidade BHOLD Microsoft Docs
description: Este artigo documenta as várias alterações feitas no âmbito de atualizações ao BHOLD no âmbito do MIM 2016
keywords: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 09/14/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 1ff755aef697b92206745426e7820526db0666d3
ms.sourcegitcommit: 7e8c3b85dd3c3965de9cb407daf74521e4cc5515
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/10/2020
ms.locfileid: "92762502"
---
# <a name="bhold-modules-version-release-history"></a>Histórico de lançamento de versão de módulos BHOLD

A equipa do Microsoft Identity Manager lança regularmente atualizações que estão listadas no histórico da [versão.](version-history.md) Este artigo ajuda-o a acompanhar as versões que foram lançadas para o BHOLD, um subcomponente do Gestor de Identidade da Microsoft. Pode então decidir se precisa de atualizar para a versão mais recente ou não.

## <a name="version-60360"></a>Versão 6.0.36.0

- Estado: 7 de setembro de 2017
- [Transferência](https://www.microsoft.com/en-us/download/details.aspx?id=55950)

### <a name="enhancements"></a>Melhoramentos  
A versão BHOLD 6.0.36.0 inclui as seguintes melhorias:

- Atualização da plataforma x86 a x64.

### <a name="fixed-issues"></a>Problemas corrigidos
As seguintes questões foram corrigidas na versão BHOLD 6.0.36.0.

#### <a name="bhold-core"></a>Núcleo BHOLD

- Instalador atualizado para x86 a x64.
- A interface web não mostra todas as permissões.
- Correções para a biblioteca B1Common.
- O utilizador não pode atribuir papel à OrgUnit quando o Role tem poucas permissões com cardinalidade.
- A permissão cardinalício não funciona para Max. números de utilizadores.

#### <a name="access-management-connector"></a>Conector de Gestão de Acessos

- n/a

#### <a name="bhold-integration-module"></a>Módulo de integração BHOLD

- Instalador atualizado para x86 a x64.
- Localização incorreta da pasta LAYOUT.

#### <a name="bhold-model-generator"></a>Gerador de modelos BHOLD

- As funções-modelo dos departamentos-mãe não são herdadas.

#### <a name="bhold-analytics"></a>BHOLD analíticos

- n/a

#### <a name="bhold-attestation"></a>Atestado BHOLD

- n/a

#### <a name="bhold-reporting"></a>Relatórios BHOLD

- Atributo personalizado 'employeeID' não disponível.
- Não é possível carregar corretamente dados grandes utilizando um conjunto de 3 ficheiros.

## <a name="next-steps"></a>Passos seguintes

- [Guia de conceitos do BHOLD](../bhold/bhold-concepts-guide.md)
- [Guia de instalação da BHOLD](../bhold/bhold-installation-guide.md)
- [Referência para programadores do BHOLD](mim2016-bhold-developer-reference.md)
- [Histórico de versões do MIM](version-history.md)

