---
title: Canais
description: Saiba mais sobre os canais e suas várias configurações no Painel da TVE.
source-git-commit: b81cc7498a8035f4c274ba25952dcd1dcd8d71f5
workflow-type: tm+mt
source-wordcount: '1140'
ht-degree: 0%

---


# Canais {#channels}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso dessa API exige uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

A variável **Canais** do Painel TVE permite visualizar e gerenciar as configurações dos canais associados a um programador específico. Também é possível [adicionar um novo canal](#add-new-channel) de acordo com sua necessidade.

A variável **Canais** no painel esquerdo exibe uma lista de canais vinculados com os seguintes detalhes:

* **Nome de exibição**: o nome da marca do canal usado para fins comerciais.
* **ID do canal**: um identificador exclusivo, também conhecido como ID do solicitante.
* **Integrações**: o número de conexões estabelecidas com [MVPDs](/help/authentication/glossary.md#mvpd).

![Lista de canais existentes](assets/channels-list.png)

*Lista de canais existentes*

Digite o nome do canal no campo **Pesquisar** acima da lista para saber mais sobre o canal.

## Gerenciar configurações de canal {#manage-channel-conf}

Siga as etapas para gerenciar várias configurações de um canal específico.

1. Selecione o **Canais** no painel esquerdo.
1. Selecione o canal na lista disponível.
1. Selecione uma das seguintes guias para exibir e editar as configurações correspondentes do canal selecionado:

   * [Configurações gerais](#general-settings)
   * [Integrações](#integrations)
   * [Certificados](#certificates)
   * [Domínios](#domains)
   * [Aplicativos registrados](#registered-applications)
   * [Esquemas personalizados](#custom-schemes)

   ![Configurações de canal](assets/channel-settings.png)

   *Configurações de canal*

>[!IMPORTANT]
>
> Exibir [Revisar e enviar alterações](/help/authentication/tve-dashboard-review-push-changes.md) para obter mais informações sobre como ativar as alterações de configuração.

### Configurações gerais {#general-settings}

Esta guia apresenta **Informações do canal** e **Configuração do Analytics**.

#### Informações do canal {#channel-information}

Nesta seção, você pode editar os seguintes detalhes:

* **Nome de exibição**: o nome da marca do canal usado para fins comerciais.

* **URL de redirecionamento padrão**: o URL de redirecionamento de backup para autenticação e logout.

* **Relatório de erros**: Ao selecionar **Sim**, os SDKs do Adobe Pass enviam relatórios de erros ao back-end do Adobe Pass para análise.

![Editar informações do canal](assets/channel-information.png)

*Editar informações do canal*

#### Configuração do Analytics {#analytics-configuration}

Esta seção permite configurar o encaminhamento de eventos de Autenticação do Adobe Pass para o Adobe Analytics.

Para habilitar **Configuração do Analytics**, entre em contato com o TAM (Gerente técnico de conta) para obter mais detalhes sobre como configurar a ID do conjunto de relatórios (RSID).

![Ativar configurações do Analytics](assets/channel-analytical-conf.png)

*Ativar configurações do Analytics*

Selecionar **Adicionar nova configuração de análise** para adicionar várias configurações.

Uma nova alteração de configuração foi criada e está pronta para atualização do servidor. Para usar a nova configuração de análise da variável **Configuração do Analytics** prossiga com a [revisar e enviar alterações](/help/authentication/tve-dashboard-review-push-changes.md) fluxo.

### Integrações {#integrations}

Essa guia exibe uma lista de integrações disponíveis entre o canal selecionado no momento e os MVPDs. A lista apresenta cada integração junto com seu status, indicando se está ativada ou não. Selecione uma integração específica na lista para acessar as informações detalhadas na [Integrações](tve-dashboard-integrations.md) seção.

![Lista de integrações disponíveis](assets/channel-integrations.png)

*Lista de integrações disponíveis*

### Certificados {#certificates}

Esta guia exibe uma lista de [certificados disponíveis](#available-certificates) e [certificados disponíveis herdados](#inherited-avail-certificates) usado nos fluxos de criptografia de metadados do usuário. Ela exibe detalhes sobre cada certificado que inclui:

* O status (ativado ou não para **criptografia de metadados do usuário** uso ou não)
* Número de série
* Nome da organização emissora
* Nome da organização do assunto
* Data de emissão
* Data de expiração
* Um menu suspenso para criptografar metadados do usuário (se você selecionar **Sim**, o certificado criptografa informações confidenciais do usuário, como valores de código postal).

#### Certificados disponíveis {#available-certificates}

Esses certificados servem como chaves privadas ou públicas e são usados para criptografia de metadados do usuário.
Você pode fazer as seguintes alterações na seção certificados disponíveis:

* [Adicionar novo certificado](#add-new-certificate)
* [Excluir certificado](#delete-certificate)

##### Adicionar novo certificado {#add-new-certificate}

Para adicionar um novo certificado, siga estas etapas:

1. Selecionar **Adicionar novo certificado** na parte superior do **Certificados disponíveis** seção.

   ![Adicionar um novo certificado](assets/add-new-certificate.png)

   *Adicionar um novo certificado*

1. Cole a chave pública do seu certificado no **Novo certificado** caixa de diálogo.
1. Selecionar **Adicionar certificado**.

   Uma nova alteração de configuração foi criada e está pronta para atualização do servidor. Para usar o novo certificado listado no **Certificados disponíveis** prossiga com a [revisar e enviar alterações](/help/authentication/tve-dashboard-review-push-changes.md) fluxo.

1. Localize o novo certificado na lista de **Certificados disponíveis**.

   >[!IMPORTANT]
   >
   > Verifique se seus sistemas estão atualizados e prontos para usar o novo certificado.

1. Selecionar **Sim** de **Usado para criptografar metadados de usuário** para ativar um novo certificado.

##### Excluir certificado {#delete-certificate}

Siga estas etapas para excluir um certificado.

1. Passe o mouse sobre o certificado que deseja excluir da lista de **Certificados disponíveis**.
1. Selecionar **Remover**.

   ![Remover o certificado selecionado](assets/channel-delete-certificate.png)

   *Remover o certificado selecionado*

1. Selecionar **Excluir** do **Excluir certificado ativo** caixa de diálogo.

Uma nova alteração de configuração foi criada e está pronta para atualização do servidor. O certificado será excluído do **Certificados disponíveis** seção somente depois de [revisar e enviar alterações](/help/authentication/tve-dashboard-review-push-changes.md).

#### Certificados disponíveis herdados {#inherited-avail-certificates}

As empresas de mídia definem esses certificados em seu próprio nível. Todos os canais associados à mesma empresa de mídia podem usar esses certificados.

![Certificados disponíveis herdados](assets/inherited-available-certificates.png)

*Certificados disponíveis herdados*

### Domínios {#domains}

Essa guia exibe uma lista de domínios disponíveis pelos quais o respectivo canal se comunica com a Autenticação Adobe Pass.

Você pode fazer as seguintes alterações nos domínios:

* [Adicionar um novo domínio](#add-domains)
* [Excluir domínio](#delete-domain)

>[!TIP]
>
> Evite adicionar um novo subdomínio se existir um domínio mais geral na lista.

#### Adicionar novo domínio {#add-domains}

Siga estas etapas para adicionar um domínio.

1. Selecionar **Adicionar novo domínio** no canto superior direito da **Domínios disponíveis** seção.

   ![Adicionar um novo domínio](assets/add-new-domain.png)

   *Adicionar um novo domínio*

1. Digite o nome do seu domínio no campo **Novo domínio** caixa de diálogo.

1. Selecionar **Adicionar domínio** para adicionar um novo domínio para o canal selecionado.

Uma nova alteração de configuração foi criada e está pronta para atualização do servidor. Para usar o novo domínio listado no **Domínios disponíveis** prossiga com a [revisar e enviar alterações](/help/authentication/tve-dashboard-review-push-changes.md) fluxo.

#### Excluir domínio {#delete-domain}

Siga estas etapas para excluir um domínio.

1. Passe o mouse sobre o domínio que deseja excluir da lista de **Domínios disponíveis**.
1. Selecionar **Remover**.

   ![Remover o domínio selecionado](assets/remove-domain.png)

   *Remover o domínio selecionado*

1. Selecionar **Excluir** no **Excluir domínio** caixa de diálogo.

Uma nova alteração de configuração foi criada e está pronta para atualização do servidor. O domínio será excluído do **Domínios disponíveis** seção somente depois de [revisar e enviar alterações](/help/authentication/tve-dashboard-review-push-changes.md).

O domínio selecionado não está mais disponível para uso. Como resultado, o aplicativo associado a esse domínio perde acesso aos serviços de autenticação da Adobe Pass.

### Aplicativos registrados {#registered-applications}

Esta guia fornece uma lista de registros de aplicativos. Exibir [Gerenciamento dinâmico de registros de clientes](/help/authentication/dynamic-client-registration-management.md) para obter mais informações.

### Esquemas personalizados {#custom-schemes}

Esta guia exibe uma lista de esquemas personalizados. Exibir [Registro de aplicativo iOS/tvOS](/help/authentication/iostvos-application-registration.md) e [Gerenciamento dinâmico de registros de clientes](/help/authentication/dynamic-client-registration-management.md) para obter mais informações.

## Adicionar novo canal {#add-new-channel}

Siga estas etapas para adicionar um novo canal.

1. Selecione o **Canais** no painel esquerdo.
1. Selecionar **Adicionar novo canal** no canto superior direito da **Canais** seção.

   ![Adicionar um novo canal](assets/add-new-channel.png)

   *Adicionar um novo canal*

1. Selecionar **ID do programador** no menu suspenso do **Novo canal** caixa de diálogo.

1. Digite um identificador exclusivo em **ID do canal**.
1. Digite o nome da marca do canal usado para fins comerciais na **Nome de exibição**.
1. Selecionar **Adicionar canal**.

Uma nova alteração de configuração foi criada e está pronta para atualização do servidor. Para usar o novo canal listado na variável **Canais** prossiga com a [revisar e enviar alterações](/help/authentication/tve-dashboard-review-push-changes.md) fluxo.

