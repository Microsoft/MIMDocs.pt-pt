
# [Compreender e Explorar](microsoft-identity-manager-2016.md)

## [O que é o MIM 2016?](microsoft-identity-manager-2016.md)

## [Novidades no service pack 1](Microsoft-identity-manager-2016-sp1-release-notes.md)

### [Scripts de implementação da PAM do MIM2016 SP1](sp1-deployment-scripts.md)

## [Criação de relatórios híbridos no Azure](identity-manager-hybrid-reporting-azure.md)

# [Planear e Estruturar](microsoft-identity-manager-2016-supported-platforms.md)

## [Plataformas suportadas](microsoft-identity-manager-2016-supported-platforms.md)

## [Ligar a diretórios](supported-management-agents.md)

## [Planeamento da capacidade](capacity-planning-guide.md)

## [Topologia de implementação](topology-considerations.md)

# [Implementar e Utilizar](microsoft-identity-manager-deploy.md)

## [Primeira implementação](microsoft-identity-manager-deploy.md)

### [Configuração do domínio](preparing-domain.md)

### [Configuração do servidor: Windows Server](prepare-server-ws2012r2.md)

### [Configuração do servidor: SQL](prepare-server-sql2014.md)

### [Configuração do servidor: SharePoint](prepare-server-sharepoint.md)

### [Configuração do servidor: Exchange](prepare-server-exchange.md)

### [Instalar o MIM: Sincronização](install-mim-sync.md)

### [Instalar o MIM: Serviço e Portal](install-mim-service-portal.md)

### [Instalar o MIM: Sincronizar bases de dados](install-mim-sync-ad-service.md)

## [Atualização do Forefront Identity Manager 2010 R2](microsoft-identity-manager-2016-upgrade-from-fim-2010-R2.md)

## [Serviço de Notificação de Alteração de Palavra-passe](deploying-mim-password-change-notification-service-on-domain-controller.md)

## [Relatórios Híbridos do Identity Manager](working-with-identity-manager-hybrid-reporting.md)

## [Reposição Personalizada de Palavra-passe](working-with-self-service-password-reset.md)

## [Gestor de Certificados do MIM](working-with-mim-certificate-manager.md)

### [Inscrever smart cards](certificate-manager-for-non-administrators.md)

### [Criar certificados de software](certificate-manager-for-software-certificates.md)

# [Utilize o Privileged Access Management](./pam/privileged-identity-management-for-active-directory-domain-services.md)

## [Saiba mais sobre o DPM](./pam/privileged-identity-management-for-active-directory-domain-services.md)

### [Compreender os componentes](./pam/principles-of-operation.md)

## [Planear a Implementação do PAM](./pam/environment-overview.md)

### [Descrição geral do ambiente](./pam/environment-overview.md)

### [Modelo do escalão](./pam/tier-model-for-partitioning-administrative-privileges.md)

### [Plano de ambiente bastion](./pam/planning-bastion-environment.md)

### [Definir funções](./pam/defining-roles-for-pam.md)

### [Recuperação de elevada disponibilidade e após desastre](./pam/high-availability-disaster-recovery-considerations-bastion-environment.md)

### [Requisitos de hardware e software](./pam/hardware-software-requirements.md)

## [Configure o MIM para o Privileged Access Management](./pam/configuring-mim-environment-for-pam.md)

### [Passo 1 – domínio CORP](./pam/step-1-prepare-corp-domain.md)

### [Passo 2 – controlador de domínio PRIV](./pam/step-2-prepare-priv-domain-controller.md)

### [Passo 3 - servidor PAM](./pam/step-3-prepare-pam-server.md)

### [Passo 4 – instalar o MIM no servidor de PAM](./pam/step-4-install-mim-components-on-pam-server.md)

### [Passo 5 – estabelecer fidedignidade entre PRIV e a rede empresarial](./pam/step-5-establish-trust-between-priv-corp-forests.md)

### [Passo 6 – criar contas com privilégios](./pam/step-6-transition-group-to-pam.md)

### [Passo 7 – Elevar o acesso de um utilizador](./pam/step-7-elevate-user-access.md)

### [Implementar o PAM do MIM com o Windows Server 2016](./pam/deploy-pam-with-windows-server-2016.md)

### [Configurar a MFA do Azure](./pam/use-azure-mfa-for-activation.md)

## [Configurar a PAM através de scripts](./pam/sp1-pam-configure-using-scripts.md)

### [Passo 1: Configurar o domínio Priv](./pam/sp1-step1-configuring-priv-domain.md)

### [Passo 2: Configurar o domínio CORP](./pam/sp1-step2-configuring-corp-domain.md)

### [Passo 3: Configurar o SQL](./pam/sp1-step3-installing-configuring-sql.md)

### [Passo 4: Configurar o SharePoint](./pam/sp1-step4-configuring-sharepoint.md)

### [Passo 5: Instalar/configurar a PAM](./pam/sp1-step5-configuring-pam.md)

### [Passo 6: Configurar a confiança da PAM](./pam/sp1-step6-setup-pam-trust.md)

### [Passo 7: Configurar o histórico/filtragem do SID](./pam/sp1-step7-setup-sidhistory-sidfiltering.md)

### [Passo 8: Verificação de implementação da PAM](./pam/sp1-step8-pam-deployment-verification.md)

### [Adenda](./pam/sp1-pam-deployment-addendum.md)

# Infraestrutura de Gestão

## [Analisador de Melhores Práticas do Identity Manager](https://technet.microsoft.com/library/jj203402)

## [Serviço de Notificação de Alteração de Palavra-passe](https://technet.microsoft.com/library/e27c0bc6-c808-4fdb-9e59-58feeb419308)

## Gestão de Certificados

### [Ferramenta de Linhas de Comando CLMUtil](https://technet.microsoft.com/library/cc720647)

### [Modelos de Perfil de Configuração](https://technet.microsoft.com/library/cc708656)

### [Utilizar o site de gestão de certificados](https://technet.microsoft.com/library/cc720560)

### [Gerir Aplicações para Smart Cards](https://technet.microsoft.com/library/cc708681)

### [Criar Cópias de Segurança e Restaurar](https://technet.microsoft.com/library/dd883245)

## Reposição Personalizada de Palavra-passe

### [Registo de Utilizadores Programático](https://technet.microsoft.com/library/jj134294)

### [Personalizações](https://technet.microsoft.com/library/jj134312)

## Serviço e Portal

### [Kerberos](https://technet.microsoft.com/library/jj134299)

### [Registo dinâmico](./infrastructure/mim-service-dynamic-logging.md)

### [Guia de Desempenho da Exportação](https://technet.microsoft.com/library/hh322883)

## Relatórios

### [Relatórios Personalizados e Extensibilidade](https://technet.microsoft.com/library/jj133861)

## [Software do Microsoft Identity: versões de compilação públicas](https://blogs.technet.microsoft.com/iamsupport/idmbuildversions/)

# [Referência para Programadores](./reference/microsoft-identity-manager-2016-developer-reference.md)

## [Referência para Programadores do Microsoft Identity Manager 2016](./reference/microsoft-identity-manager-2016-developer-reference.md)

### [Referência à API REST da Gestão de Certificados](./reference/certificate-management-rest-api-reference.md)

#### [Detalhes do Serviço da API REST da Gestão de Certificados](./reference/certificate-management-rest-api-service-details.md)

#### [Instruções de Inscrição de Exemplo](./reference/sample-enrollment-walkthrough.md)

#### [Obter Modelos de Perfil](./reference/get-profile-templates.md)

#### [Operações de Políticas](./reference/policy-operations.md)

##### [Obter Política de Fluxo de Trabalho](./reference/get-workflow-policy.md)

##### [Obter Política de Smart Card](./reference/get-smartcard-policy.md)

#### [Operações de Pedidos](./reference/request-operations.md)

##### [Criar Pedido](./reference/create-request.md)

##### [Obter Pedido](./reference/get-request.md)

##### [Cancelar, Abandonar ou Concluir um Pedido](./reference/cancel-abandon-complete-request.md)

#### [Operações de Pedidos de Certificado](./reference/certificate-request-operations.md)

##### [Obter Opções de Geração de Pedido de Certificado](./reference/get-certificate-request-generation-options.md)

##### [Obter Respostas de Certificado](./reference/get-certificate-responses.md)

#### [Operações de Smart Card](./reference/smartcard-operations.md)

##### [Atribuir Smart Card a um Pedido](./reference/assign-smartcard-to-request.md)

##### [Obter Dados de Smart Card](./reference/get-smartcard-data.md)

##### [Obter Resposta de Autenticação de Smart Card](./reference/get-smartcard-authentication-response.md)

##### [Obter Chave de Administração Diversificada de Smart Card](./reference/get-smartcard-diversified-admin-key.md)

##### [Obter PIN Proposto de Smart Card](./reference/get-smartcard-proposed-pin.md)

##### [Atualizar Estado de Smart Card](./reference/update-smartcard-status.md)

#### [Operações de Perfil](./reference/profile-operations.md)

##### [Obter Dados de Perfil](./reference/get-profile-data.md)

##### [Obter Operações de Estado de Perfil](./reference/get-profile-state-operations.md)

#### [Operações de Certificado](./reference/certificate-operations.md)

##### [Obter Certificados de Perfil ou Smart Card](./reference/get-smartcard-profile-certificates.md)

##### [Obter Certificados de Utilizador](./reference/get-user-certificates.md)

### [Referência à API REST do Privileged Access Management](./reference/privileged-access-management-rest-api-reference.md)

#### [Detalhes do Serviço da API REST do PAM](./reference/privileged-access-management-rest-api-service-details.md)

#### [Obter Funções de PAM](./reference/privileged-access-management-get-roles.md)

#### [Criar Pedido de PAM](./reference/privileged-access-management-create-request.md)

#### [Criar Pedidos de PAM](./reference/privileged-access-management-get-requests.md)

#### [Fechar Pedido de PAM](./reference/privileged-access-management-close-request.md)

#### [Obter Pedidos de PAM Pendentes](./reference/privileged-access-management-get-pending-requests.md)

#### [Aprovar ou Rejeitar um Pedido de PAM Pendente](./reference/privileged-access-management-approve-reject-pending-request.md)

#### [Obter Informações da Sessão de PAM](./reference/privileged-access-management-get-session-info.md)

## [Referência Técnica]

### [Referência XML de Configuração de Visualização do Controlo de Recursos](./reference/rcd-configuration-xml-reference.md)
