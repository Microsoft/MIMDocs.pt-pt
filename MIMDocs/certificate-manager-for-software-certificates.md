---
title: "Pedir certificados no Gestor de Certificados através de modelos | Documentos da Microsoft"
description: Saiba como utilizar o Gestor de Certificados para criar e renovar certificados de software com modelos de perfil.
keywords: 
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fed5ada9-d80f-4825-aad7-4172ac5d71d3
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: aebf5af709f4f775ce13be49d8f9075a94e864a2
ms.sourcegitcommit: f077508b5569e2a96084267879c5b6551e1e0905
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/12/2017
---
# <a name="create-software-certificates-with-certificate-manager"></a>Criar certificados de software com o Gestor de Certificados
Para se inscrever e renovar certificados de software, não tem de ser um administrador e não precisa de um smart card virtual. É importante salientar que, a determinada altura, será pedido que permita uma operação de certificado, mas isto é normal.

## <a name="create-a-software-certificate-profile-template-in-mim-2016-certificate-manager"></a>Criar um Modelo de Perfil de certificado de software no Gestor de Certificados do MIM 2016

1.  Crie um modelo para o certificado que irá pedir para o smart card virtual. Abra a mmc.

2.  Clique em **Ficheiro** e, em seguida, em **Adicionar/Remover Snap-in**.

3.  Na lista Snap-ins disponíveis, clique em **Modelos de Certificado** e, em seguida, em **Adicionar**.

4.  Agora, os **Modelos de Certificado** encontram-se na Raiz da Consola na MMC. Faça duplo clique para ver todos os modelos de certificado disponíveis.

5.  Clique com o botão direito do rato em **Modelo de utilizador** e clique em **Duplicar Modelo**.

6.  No separador **Compatibilidade**, em Autoridade de Certificação, Selecione Windows Server 2008 e, em Destinatário do Certificado, selecione Windows 8.1/Windows Server 2012 R2.

    1.  No separador **Geral**, no campo nome a apresentar, escreva **Modelo de Certificado Arquivado**.

    2.  b.  No separador **Processamento de Pedidos**

        1.  Defina o **Objetivo** para a Assinatura e encriptação.

        2.  Marque **Incluir algoritmos simétricos permitidos pelo requerente**.

        3.  Se pretende arquivar a chave, marque **Arquivar chave privada de encriptação do requerente**.

        4.  Em Faça o seguinte…, selecione **Perguntar ao utilizador durante a inscrição**.

    3.  No separador **Criptografia**

        1.  Em Categoria de Fornecedor, selecione **Fornecedor de Armazenamento de Chaves**

        2.  Selecione **Pedidos podem usar um fornecedor disponível no comput. do requerente**.

    4.  No separador **Segurança**, adicione o grupo de segurança ao qual pretende conceder o acesso de **Inscrição**. Por exemplo, se pretende conceder acesso a todos os utilizadores, selecione o grupo de utilizadores **Autenticados** e, em seguida, selecione as permissões de **Inscrição**.

    5.  No separador **Nome do Requerente**

        1.  Desmarque **Incluir nome de correio eletrónico no nome de requerente**.

        2.  Em **Incluir estas informações no nome do requerente alternativo**, desmarque **Nome de e-mail**.

    6.  Clique em **OK** para finalizar as alterações e criar o novo modelo. O novo modelo deverá agora aparecer na lista de Modelos de Certificado.

    7.  Selecione **Ficheiro** e, em seguida, clique em **Adicionar/Remover Snap-in** para adicionar o snap-in Autoridade de Certificação à consola MMC. Quando lhe for pedido o computador que pretende gerir, selecione **Computador Local**.

    8.  No painel esquerdo da MMC, expanda a **Autoridade de Certificação (Local)** e, em seguida, expanda a AC na lista de Autoridades de Certificação.

    9. Clique com botão direito do rato em **Modelos de Certificado**, clique em **Novo** e, em seguida, em **Modelo de Certificado a Emitir**.

    10. Na lista, selecione o novo modelo que acabou de criar (**Modelo de Certificado Arquivado**) e, em seguida, clique em **OK**.

## <a name="create-the-profile-template"></a>Criar o Modelo de Perfil

1.  Inicie sessão no portal do CM como um utilizador com privilégios administrativos.

2.  Aceda a **Administração &gt; Gerir Modelos de Perfil**, certifique-se de que marca a caixa junto a **Modelo de Perfil de Início de Sessão do Smart Card de Exemplo MIM CM** e, em seguida, clique em **Copiar um modelo de perfil selecionado**.

3.  Escreva o nome do modelo de perfil e clique em **OK**.

4.  No ecrã seguinte, clique em **Adicionar novo modelo de certificado** e certifique-se de que marca a caixa junto ao nome da AC.

5.  Marque a caixa junto ao nome do Certificado de Software Arquivado e clique em **Adicionar**.

6.  Remova o Modelo de utilizador ao marcar a caixa junto ao mesmo, clique em **Eliminar modelos de certificado selecionados** e, em seguida, em **OK**.

7.  Clique em **Alterar definições gerais**.

8.  Marque as caixas à esquerda de **Gerar chaves de encriptação no servidor** e clique em **OK**. No painel da esquerda, clique em **Política de Recuperação**.

9. Clique em **Alterar definições gerais**.

10. Se pretender voltar a emitir certificados arquivados, marque as caixas à esquerda de **Voltar a emitir certificados arquivados** e clique em **OK**.

11. Se estiver a utilizar o CM do Smart Card Virtual, terá de desativar os itens de recolha de dados, pois não funciona com a recolha de dados ativada. Desative a recolha de dados para todas as políticas ao clicar na política no painel esquerdo e ao desmarcar a caixa junto a **Item de dados de exemplo** e, em seguida, clique em **Eliminar itens de recolha de dados**. Em seguida, clique em **OK**.
