---
title: Opções de configuração do Conector de Serviço Web Microsoft Docs
description: Este artigo cobre os passos necessários para instalar a Ferramenta de Configuração do Serviço Web.
keywords: ''
author: EugeneSergeev
ms.author: esergeev
manager: aashiman
ms.date: 3/27/2020
ms.topic: conceptual
ms.prod: microsoft-identity-manager
ms.reviewer: markwahl-msft
ms.assetid: ''
ms.openlocfilehash: 34c83427b6dfb3084976aebf29c019d8228f8247
ms.sourcegitcommit: d21963c1fba6dc908bec5eaadc54e3395a8ef8c3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/10/2020
ms.locfileid: "92762552"
---
# <a name="web-service-connector-configuration-options"></a>Opções de configuração do Web Service Connector
Este artigo descreve os passos para configurar um novo Conector de Serviço Web ou para fazer alterações num Conector de Serviço Web existente através do Microsoft Identity Manager (MIM) Synchronization Service UI.

>[!IMPORTANT]
>Descarregue e instale o [Conector de Serviço Web](https://www.microsoft.com/download/details.aspx?id=51495) antes de tentar os passos neste artigo.

## <a name="configure-the-web-service-connector-in-the-synchronization-service"></a>Configure o Conector de Serviço Web no Serviço de Sincronização

Pode criar um novo Conector de Serviço Web utilizando o designer de agentes de gestão. Depois de criar o Conector, pode definir vários Perfis de Execução para executar diferentes tarefas. Ao configurar um Conector existente, pode alterar uma tarefa clicando na página apropriada no Designer de Agente de Gestão. Siga os passos abaixo para configurar um novo Conector de Serviço Web.

1. Abra o Serviço de Sincronização do Microsoft Identity Manager 2016. No menu **Ferramentas,** selecione **Management Agents** .

2. No menu **Ações,** selecione **Criar** . O Designer de Gerentes abre.

3. In **Management Agent Designer** , under Management Agent **for** , select Web **Service (Microsoft)** . Em seguida, selecione **Seguinte** .

    ![Criar um agente de gestão](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma.png)

4. No ecrã **de Conectividade,** selecione o projeto padrão do **Conector de Serviço Web** . Fornecer valores para o **Anfitrião** e **Porto.** Em seguida, selecione **Seguinte** .

    ![Criar conectividade para o agente de gestão](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma-connectivity.png)

5. Defina os **Parâmetros Globais.** Utilize a credencial de login obtida do Administrador de Serviço Web para ligar ao Anfitrião. Em seguida, selecione **Seguinte** .

    ![Definir parâmetros globais para o agente de gestão](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma-global-parameters.png)

    - Se a localização da fonte de dados observar a Daylight Saving e a fonte de dados estiver configurada para ajustar automaticamente as definições de poupança de luz do dia, verifique se a Fonte de **Dados está configurada para ajustar automaticamente** o relógio para a opção Horário de verão.
    - Se pretender ativar o fluxo de trabalho da ligação de teste a partir deste conector, verifique a opção **De Ligação de Teste.**

6. No ecrã seguinte, selecione **predefinição** para **Selecionar divisórias de diretório .** Em seguida, selecione **Seguinte** .

    ![Criar divisórias para o agente de gestão](media/microsoft-identity-manager-2016-ma-ws-maconfig/create-ma-partitions.png)

7. No ecrã **Select Object Types,** selecione o tipo de objeto com o quais pretende trabalhar. Por predefinição, o Conector de Serviço Web suporta dois tipos de objetos: **Empregado** e **Utilizador** . Em seguida, selecione **Seguinte** .

    ![Selecione o tipo de objeto](media/microsoft-identity-manager-2016-ma-ws-maconfig/select-object-types.png)

8. Na página **'Selecionar Atributos',** selecione todos os atributos obrigatórios para os objetos e atributos selecionados com os quais necessita de trabalhar. Em seguida, selecione **Seguinte** .

    ![Selecione os atributos para os objetos](media/microsoft-identity-manager-2016-ma-ws-maconfig/select-attributes.png)

9. Na página **Configure Anchors, especifique** os atributos da âncora. Em seguida, selecione **Seguinte** .

    ![Configure as âncoras](media/microsoft-identity-manager-2016-ma-ws-maconfig/configure-anchors.png)

10. Na página **Configure Connector Filter,** especifique o **filtro de conector** . Em seguida, selecione **Seguinte** .

    ![Especificar o filtro do conector](media/microsoft-identity-manager-2016-ma-ws-maconfig/configure-connector-filter.png)

11. Na página **'Configurar Regras de Junção e Projeção',** especifique as regras de junção e projeção. Pode criar uma nova regra de união e regra de projeção selecionando **Nova Regra de União** e **Nova Regra de Projeção,** respectivamente. Em seguida, selecione **Seguinte** .

    ![Especificar as regras de junção e projeção](media/microsoft-identity-manager-2016-ma-ws-maconfig/join-projection.png)

12. Na página seguinte, configuure o fluxo de atributos. Tem de especificar o **Tipo de Mapeamento** e **a Direção de Fluxo** para os atributos para os tipos de objetos selecionados. Em seguida, selecione **Seguinte** .

    ![Configure o fluxo de atributos](media/microsoft-identity-manager-2016-ma-ws-maconfig/attribute-flow.png)

13. Especifique o tipo de desprovisionamento para aplicar aos objetos. Em seguida, selecione **Seguinte** .

    ![Especificar o tipo de desprovisionamento](media/microsoft-identity-manager-2016-ma-ws-maconfig/deprovisioning.png)

14. No caso de um fluxo de importação, a página **de extensões de configuração** é desativada. Pode configurar extensões para fluxos de exportação selecionando primeiro o tipo de mapeamento **avançado** na página **Configure Attribute Flow.**

    ![Configurar extensões](media/microsoft-identity-manager-2016-ma-ws-maconfig/extensions.png)

15. Clique em **Concluir** .

O seu Conector está agora configurado:

![Configuração do conector completa](media/microsoft-identity-manager-2016-ma-ws-maconfig/sync-manager.png)

Depois de configurar um Conector, pode configurar os Perfis de Execução selecionando **Perfis de Execução configurados** .

## <a name="additional-steps"></a>Passos adicionais

Quando a autenticação baseada em certificados é utilizada, é necessária uma alteração adicional após a Ferramenta de Configuração do Serviço Web gerar um ficheiro WSConfig, antes que esse ficheiro possa ser importado para um projeto de Conector de Serviço Web no Serviço de Sincronização MIM.

Para permitir a autenticação baseada em certificados:

- Configure o seu projeto para utilizar a Autenticação Básica na Ferramenta de Configuração do Serviço Web
- Crie uma cópia do ficheiro my_project.wsconfig e rebatize-o para my_project.zip
- Abra este arquivo e modifique generated.config ficheiro para substituir a autenticação básica por autenticação baseada em certificados (um exemplo fornecido abaixo)
- Substitua generated.config ficheiro em my_project.zip e rebatize-o para my_project_updated.wsconfig
- Selecione my_project_updated.wsconfig ao criar um agente de gestão no Servidor de Sincronização MIM

Encontre generated.config ficheiro de amostra com autenticação baseada em certificados abaixo:

```xml
<?xml version="1.0" encoding="utf-8"?>
    <configuration>
        <appSettings>
            <add key="SoapAuthenticationType" value="Certificate"/>
        </appSettings>
        <system.serviceModel>
            <bindings>
                <wsHttpBinding>
                    <binding name="binding">
                        <security mode="Transport">
                            <transport clientCredentialType="Certificate"/>
                        </security>
                    </binding>
                </wsHttpBinding>
            </bindings>
            <client>
                <endpoint address="https://myserver.local.net:8011/sap/bc/srt/scs/sap/zsapconnect?sap-client=800"
                    binding="wsHttpBinding" bindingConfiguration="binding"
                    contract="SAPCONNECTOR.ZSAPConnect" name="binding"/>
            </client>
            <behaviors>
                <endpointBehaviors>
                    <behavior name="endpointCredentialBehavior">
                        <clientCredentials>
                            <clientCertificate findValue="my.certificate.name.local.net"
                                storeLocation="LocalMachine"
                                storeName="My"
                                x509FindType="FindBySubjectName"/>
                        </clientCredentials>
                    </behavior>
                </endpointBehaviors>
            </behaviors>
        </system.serviceModel>
    </configuration>
```

## <a name="next-steps"></a>Passos seguintes

- [Instale a Ferramenta de Configuração do Serviço Web](microsoft-identity-manager-2016-ma-ws-install.md)
- [Guia de implementação soap](microsoft-identity-manager-2016-ma-ws-soap.md)
- [Guia de implantação do REST](microsoft-identity-manager-2016-ma-ws-restgeneric.md)
- [Configuração MA do Serviço Web](microsoft-identity-manager-2016-ma-ws-maconfig.md)
