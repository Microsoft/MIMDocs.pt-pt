---
title: Gestão de Palavras-passe do Microsoft Identity Manager 2016 | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/01/2017
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 45b46ed10f7eda506fe1fc1af94c4be06a1a37b9
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50380197"
---
# <a name="microsoft-identity-manager-2016-password-management"></a>Gestão de Palavras-passe do Microsoft Identity Manager 2016

Gerir as palavras-passe de múltiplas contas de utilizador é uma das complexidades que fazem parte da gestão de um ambiente empresarial com múltiplas origens de dados. O Microsoft Identity Manager 2016 (MIM) oferece duas soluções de gestão de palavras-passe:

-   Sincronização de palavras-passe – utiliza o serviço de notificação de alteração de palavra-passe (PCNS) para capturar alterações de palavras-passe a partir do Active Directory e propagá-las para outras origens de dados ligadas.

-   Gestão da alteração de palavra-passe baseada no utilizador – utiliza a Windows Management Instrumentation (WMI) através de aplicações de reposição personalizada de palavra-passe e de Suporte Técnico baseado na Web.

A sincronização de palavras-passe e a gestão de alteração de palavra-passe baseada no utilizador permitem-lhe:

-   Reduzir o número de palavras-passe diferentes de que os utilizadores têm de se lembrar.

-   Definir ou alterar palavras-passe em simultâneo nas múltiplas contas de um utilizador relativamente à mesma palavra-passe.

-   Permitir aos utilizadores alterar as suas próprias palavras-passe no Active Directory e enviar a alteração de palavra-passe para outros sistemas.

-   Eliminar o risco de criar um arquivo de credenciais ou palavra-passe adicionais.

-   Sincronizar palavras-passe em múltiplas origens de dados ao utilizar o Active Directory como origem autoritativa.

-   Realizar operações de gestão de palavras-passe em tempo real, independentemente das operações do MIM.

## <a name="password-extensions"></a>Extensões de palavra-passe

Os agentes de gestão para servidores de diretório suportam operações de definição e alteração de palavra-passe por predefinição. No caso dos agentes de gestão de conectividade extensível, de base de dados e baseada em ficheiros, os quais não suportam operações de definição e alteração de palavra-passe por predefinição, pode criar uma DLL (dynamic-link library) de extensão de palavra-passe em .NET.
A DLL de extensão de palavra-passe em .NET é chamada sempre que uma chamada de definição ou alteração de palavra-passe é invocada para qualquer um destes agentes de gestão. As definições da extensão de palavra-passe estão configuradas para estes agentes de gestão no Synchronization Service Manager. Para obter mais informações sobre como configurar extensões de palavra-passe, veja a Referência para Programadores do FIM.

| A gestão de palavras-passe é suportada por predefinição nos agentes de gestão para: | Se utilizar uma extensão de palavra-passe, a gestão de palavras-passe também é suportada nos agentes de gestão para: |
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Active Directory                                                          | Ficheiros de texto de par atributo-valor                                                                    |
| Serviços LDS do Active Directory (ADLDS)                   | Ficheiros de texto delimitado                                                                               |
| Servidor de Diretório IBM                                                      | Directory Services Markup Language (DSML)                                                          |
| Lotus Notes                                                               | Extensible Connectivity                                                                            |
| Novell eDirectory                                                         | Ficheiros de texto de largura fixa                                                                             |
| Servidores de diretório Sun e Netscape                                        | Base de Dados Universal IBM DB2                                                                         |
|                                                                           | LDAP Data Interchange Format (LDIF)                                                                |
|                                                                           | Microsoft SQL Server                                                                               |
|                                                                           | Base de Dados Oracle                                                                                    |

## <a name="password-synchronization"></a>Sincronização de palavras-passe


A sincronização de palavras-passe funciona com o serviço de notificação de alteração de palavra-passe (PCNS) num domínio do Active Directory e permite que as alterações de palavra-passe que têm origem no Active Directory sejam propagadas automaticamente para outras origens de dados ligadas. Para este efeito, o MIM é executado como um servidor RPC (Chamada de Procedimento Remoto) que escuta as notificações de alteração de palavra-passe a partir de um controlador de domínio do Active Directory. Quando o pedido de alteração de palavra-passe é recebido e autenticado, é processado pelo MIM e propagado para os agentes de gestão adequados.

> [!IMPORTANT]
> A sincronização de palavras-passe bidirecional não é suportada pelo MIM. Configurar a sincronização de palavras-passe bidirecional pode criar um ciclo, o que irá consumir recursos do servidor e ter um efeito potencialmente negativo tanto no Active Directory como no MIM.

O PCNS é executado em cada controlador de domínio do Active Directory. Os sistemas que recebem as notificações de palavra-passe são conhecidos como destinos. O servidor MIM tem de ser configurado como destino PCNS no Active Directory antes de serem enviadas notificações de palavra-passe. A configuração do PCNS tem de definir um grupo de inclusão e, opcionalmente, um grupo de exclusão. Estes grupos servem para restringir o fluxo de palavras-passe confidenciais do domínio. Por exemplo, para enviar palavras-passe para todos os utilizadores, mas sem enviar palavras-passe administrativas, pode optar por utilizar Utilizadores de Domínio como grupo de inclusão e Administradores de Domínio como grupo de exclusão. Para obter mais informações sobre como configurar o serviço de notificação de alteração de palavra-passe, veja [Utilizar a Sincronização de Palavras-passe](https://technet.microsoft.com/library/jj590288(v=ws.10).aspx)

Os componentes envolvidos no processo de sincronização de palavras-passe são:

-   **Serviço de notificação de alteração de palavra-passe (Pcnssvc.exe)** – o serviço de notificação de alteração de palavra-passe é executado num controlador de domínio e é responsável por receber notificações de alteração de palavra-passe do filtro de palavras-passe local, colocando essas notificações em fila para o servidor de destino que executa o MIM, e por utilizar a RPC para entregar as notificações. O serviço encripta a palavra-passe e assegura que a palavra-passe permanece em segurança até ao momento em que é entregue com êxito ao servidor de destino que executa o MIM.

-   **Nome do principal do serviço (SPN)** – o SPN é uma propriedade do objeto de conta no Active Directory que é utilizada pelo protocolo Kerberos para autenticar mutuamente o PCNS e o destino. O SPN assegura a autenticação do PCNS no servidor correto que executa o MIM e que nenhum outro serviço pode receber as notificações de alteração de palavra-passe. O SPN é criado e atribuído através da ferramenta setspn.exe. Para obter mais informações sobre como configurar o SPN, veja Utilizar a Sincronização de Palavras-passe.

-   **Filtro de notificação de alteração de palavra-passe (Pcnsflt.dll)** – o filtro de palavras-passe serve para obter palavras-passe de texto não encriptado do Active Directory. Este filtro é carregado pela Autoridade de Segurança Local (LSA) em todos os controladores de domínio do Windows Server que participem na distribuição de palavras-passe para um servidor de destino que execute o MIM. Após a instalação do filtro e do reinício do controlador de domínio, o filtro começa a receber notificações de alteração de palavra-passe relativamente a alterações de palavra-passe que tenham origem nesse controlador de domínio. O filtro de notificações de palavra-passe é executado em simultâneo com outros filtros em execução no controlador de domínio.

-   **Utilitário de configuração do serviço de notificação de alteração de palavra-passe (Pcnscfg.exe)** – o utilitário pcnscfg.exe serve para gerir e manter os parâmetros de configuração do serviço de notificação de alteração de palavra-passe armazenados no Active Directory. Estes parâmetros de configuração, tais como a definição dos servidores de destino, o intervalo entre tentativas da fila de palavras-passe e a ativação ou a desativação de um servidor de destino, são utilizados quando está a autenticar e a enviar notificações de palavra-passe para o servidor de destino que executa o MIM.
    A configuração do serviço é armazenada no Active Directory, pelo que só é necessário atualizar a configuração num único controlador de domínio. O Active Directory replica a alteração para todos os outros controladores de domínio.

-   **O servidor RPC (Chamada de Procedimento Remoto) no servidor que executa o MIM** – quando a sincronização de palavras-passe está ativada, o servidor RPC no servidor que executa o MIM é iniciado, o que permite ao mesmo receber notificações do serviço de notificação de alteração de palavra-passe. A RPC seleciona de forma dinâmica um intervalo de portas a utilizar. Se precisar que o MIM comunique com a floresta do Active Directory através de uma firewall, tem de abrir um intervalo de portas.

-   **DLL de extensão de palavra-passe** – a DLL de extensão de palavra-passe confere-lhe a capacidade de implementar operações de definição ou alteração de palavra-passe através de uma extensão de regras para qualquer agente de gestão de conectividade extensível, de base de dados ou baseada em ficheiros.
    Para o efeito, é criado um atributo encriptado apenas de exportação com o nome "export_password" que, na realidade, não existe no diretório ligado, mas a que pode aceder e o qual pode definir nas extensões de regras de aprovisionamento ou que pode utilizar durante a exportação do fluxo de atributos. Para obter mais informações sobre como configurar extensões de palavra-passe, veja a [Referência para Programadores do FIM](https://msdn.microsoft.com/library/windows/desktop/ee652263(v=vs.100).aspx).

## <a name="preparing-for-password-synchronization"></a>Preparar para a sincronização de palavras-passe

Antes de configurar a sincronização de palavras-passe para o seu ambiente do MIM e do Active Directory, verifique o seguinte:

-   O MIM está instalado de acordo com as instruções de instalação.

-   Os agentes de gestão das origens de dados ligadas a gerir para a sincronização de palavras-passe já foram criados e os objetos estão a ser associados e sincronizados com êxito.

Para configurar a sincronização de palavras-passe:

-   Expanda o esquema do Active Directory para adicionar as classes e os atributos necessários para instalar e executar o serviço de notificação de alteração de palavra-passe (PCNS).

-   Instale o PCNS em cada controlador de domínio.

-   Configure o nome do principal do serviço (SPN) no Active Directory para a conta de serviço MIM.

-   Configure o PCNS para comunicar com o serviço MIM de destino.

-   Configure os agentes de gestão das origens de dados ligadas a gerir para a sincronização de palavras-passe.

-   Ative a sincronização de palavras-passe no MIM.

Para obter mais informações sobre como configurar a sincronização de palavras-passe, veja Utilizar a Sincronização de Palavras-passe.

## <a name="password-synchronization-process"></a>Processo de sincronização de palavras-passe

No seguinte diagrama, pode ver o processo de sincronização de um pedido de alteração de palavra-passe de um controlador de domínio do Active Directory para outras origens de dados ligadas:

1.  O utilizador prime Ctrl+Alt+Delete para iniciar o pedido de alteração de palavra-passe. O pedido de alteração de palavra-passe, incluindo a nova palavra-passe, é enviado para o controlador de domínio mais próximo.

2.  O controlador de domínio regista o pedido de alteração de palavra-passe e notifica o filtro de notificação de alteração de palavra-passe (Pcnsflt.dll).

3.  O filtro de notificação de alteração de palavra-passe transmite o pedido para o serviço de notificação de alteração de palavra-passe (PCNS).

4.  O PCNS verifica o pedido de alteração de palavra-passe e, em seguida, autentica o nome do principal do serviço (SPN) através do Kerberos e reencaminha o pedido de alteração de palavra-passe em RPC encriptada para o servidor de destino MIM.

5.  O MIM valida o controlador de domínio de origem e, em seguida, utiliza o nome do domínio para localizar o agente de gestão que serve esse domínio e utiliza as informações de conta de utilizador no pedido de alteração de palavra-passe para localizar o objeto correspondente no espaço conector.

6.  Ao utilizar as informações da tabela de associação, o MIM determina os agentes de gestão que recebem a alteração de palavra-passe e envia essa alteração para os agentes de gestão.

## <a name="password-synchronization-security"></a>Segurança da sincronização de palavras-passe

As seguintes questões de segurança da sincronização de palavras-passe foram resolvidas:

-   Autenticação a partir da origem de palavra-passe – quando a notificação de alteração de palavra-passe é recebida, a autenticação Kerberos é realizada tanto pelo MIM como pelo controlador de domínio de origem para garantir que o destinatário e o remetente são válidos. Após receber uma notificação de alteração de palavra-passe, o MIM certifica-se de que o chamador tem uma conta no contentor Controladores de Domínio do domínio a que pertence.

-   Falha ao sincronizar uma palavra-passe com uma origem de dados de destino devido a uma ligação insegura. Se o agente de gestão tiver sido configurado para precisar de uma ligação segura, mas não for detetada nenhuma, a sincronização irá falhar.
    A sincronização ocorre mesmo que o agente de gestão tenha sido configurado para permitir ligações não seguras. Só deve permitir ligações não seguras depois de examinar e compreender os riscos inerentes.

-   Armazenamento seguro de palavras-passe – o MIM só armazena palavras-passe encriptadas temporariamente. Todas as palavras-passe que o MIM recebe durante uma operação de notificação de alteração de palavra-passe são encriptadas assim que entram no processo do MIM.
    A partir do momento em que as palavras-passe são enviadas com êxito para a origem de dados de destino ligada, são desencriptadas e a memória que armazena a palavra-passe é imediatamente limpa. Se a operação não conseguir efetuar a escrita na origem de dados de destino ligada, a palavra-passe encriptada será armazenada até esgotar todas as tentativas de repetição e, em seguida, será eliminada da memória.

-   Filas de palavra-passe seguras – as palavras-passe armazenadas em filas de palavra-passe do PCNS ficam encriptadas até ao momento em que são entregues.

## <a name="password-synchronization-error-recovery-scenarios"></a>Cenários de recuperação de erros de sincronização de palavras-passe

Idealmente, sempre que um utilizador alterar uma palavra-passe, a alteração será sincronizada sem erros. Os seguintes cenários descrevem como o MIM recupera de erros de sincronização comuns:

-   **Falha ao enviar a notificação de palavra-passe do Active Directory para o MIM** – este erro pode ocorrer se a rede estiver inativa ou se o servidor que executa o MIM estiver indisponível. A notificação de alteração de palavra-passe permanece em fila localmente no controlador de domínio através do PCNS. O PCNS repete o envio da notificação de acordo com a respetiva configuração do intervalo entre tentativas.

-   **Falha ao sincronizar a palavra-passe com uma origem de dados de destino** – este erro também pode ocorrer se a rede estiver inativa ou se a origem de dados de destino estiver indisponível.
    A notificação de alteração de palavra-passe é colocada em fila e repetida de acordo com a configuração do agente de gestão relativamente à tentativa de repetição e ao intervalo entre tentativas. Todas as palavras-passe permanecem encriptadas enquanto estão armazenadas para repetição e são eliminadas quando a operação é concluída com êxito ou os limites de tentativas são atingidos.

-   **Ativar um servidor de reserva semiativo que executa o MIM depois de uma falha** – em caso de falha do servidor primário que executa o MIM, pode configurar um servidor de reserva semiativo para sincronização de palavras-passe e ativá-lo sem perder alterações de palavras-passe. Para obter mais informações, veja [MIISactivate: Ferramenta de Ativação do Servidor](https://technet.microsoft.com/library/jj590194(v=ws.10).aspx)

Algumas falhas são graves o suficiente para que deixe de ser possível concluir a operação com êxito por muitas vezes que a tente repetir. Nestes casos, é registado um evento de erro e o processo é parado. Os seguintes eventos não são repetidos:

| Evento | Gravidade    | Descrição                                                                                                                                                            |
|-------|-------------|-----------|
| 6919  | Informações | Uma operação de definição de sincronização de palavra-passe não foi realizada porque o carimbo de data/hora estava desatualizado.                                                                      |
| 6921  | Error       | A operação de definição de sincronização de palavra-passe não foi processada porque a gestão de palavras-passe não está ativada no agente de gestão de destino.                                |
| 6922  | Error       | A operação de definição de sincronização de palavra-passe não foi processada porque a gestão de palavras-passe não está configurada no agente de gestão de destino.                             |
| 6923  | Aviso     | A operação de definição de sincronização de palavra-passe não foi processada porque o objeto do espaço conector de destino não foi encontrado no diretório ligado.                  |
| 6927  | Error       | A operação de definição de sincronização de palavra-passe falhou porque a palavra-passe não cumpre a política de palavras-passe do sistema de destino.                                      |
| 6928  | Error       | A operação de definição de sincronização de palavra-passe falhou porque a extensão de palavra-passe do agente de gestão de destino não está configurada para suportar operações de definição de palavra-passe. |

## <a name="user-based-password-change-management"></a>Gestão da alteração de palavra-passe baseada no utilizador

O MIM fornece duas aplicações Web que utilizam a Windows Management Instrumentation (WMI) para repor palavras-passe. Tal como acontece com a sincronização de palavras-passe, pode ativar a gestão de palavras-passe quando configura o agente de gestão no Designer do Agente de Gestão. Para obter mais informações sobre a gestão de palavras-passe e a WMI, veja a Referência para Programadores MIM.

O MIM cria dois grupos de segurança durante a instalação que suportam especificamente operações de gestão de palavras-passe:

-   FIMSyncBrowse—Os membros deste grupo têm permissão para recolher informações sobre as contas de um utilizador ao realizar operações de pesquisa com consultas de WMI.

-   FIMSyncPasswordSet—Os membros deste grupo têm permissão para realizar operações de pesquisa de contas, definição de palavras-passe e alteração de palavras-passe através das interfaces de gestão de palavras-passe com a WMI.
