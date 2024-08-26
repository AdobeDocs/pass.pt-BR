---
title: Canais
description: Saiba mais sobre os canais e suas várias configurações no Painel da TVE.
exl-id: bbddeccb-6b6f-4a8f-87ab-d4af538eee1d
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '1124'
ht-degree: 0%

---

# Canais {#channels}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso dessa API exige uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

A seção **Channels** do Painel TVE permite que você exiba e gerencie configurações para os canais associados a um programador específico. Você também pode [adicionar um novo canal](#add-new-channel) de acordo com sua necessidade.

A guia **Canais** no painel esquerdo exibe uma lista de canais vinculados com os seguintes detalhes:

* **Nome para exibição**: o nome da marca do canal usado para fins comerciais.
* **ID do canal**: um identificador exclusivo, também conhecido como ID do Solicitante.
* **Integrações**: o número de conexões estabelecidas com [MVPDs](/help/authentication/glossary.md#mvpd).

![Lista de canais existentes](assets/channels-list.png)

*Lista de canais existentes*

Digite o nome do canal na barra **Pesquisa** acima da lista para saber mais sobre o canal.

## Gerenciar configurações de canal {#manage-channel-conf}

Siga as etapas para gerenciar várias configurações de um canal específico.

1. Selecione a guia **Canais** no painel esquerdo.
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
> Exiba [Revisar e enviar alterações](/help/authentication/tve-dashboard-review-push-changes.md) para obter mais informações sobre como ativar as alterações de configuração.

### Configurações gerais {#general-settings}

Esta guia apresenta **Informações do Canal** e **Configuração do Analytics**.

#### Informações do canal {#channel-information}

Nesta seção, você pode editar os seguintes detalhes:

* **Nome para exibição**: o nome da marca do canal usado para fins comerciais.

* **URL de redirecionamento padrão**: a URL de redirecionamento de backup para autenticação e logout.

* **Relatório de erros**: ao selecionar **Sim**, os SDKs da Adobe Pass enviam relatórios de erros ao back-end da Adobe Pass para análise.

![Editar informações do canal](assets/channel-information.png)

*Editar informações do canal*

#### Configuração do Analytics {#analytics-configuration}

Esta seção permite configurar o encaminhamento de eventos de Autenticação do Adobe Pass para o Adobe Analytics.

Para habilitar a **Configuração do Analytics**, entre em contato com o TAM (Gerente técnico de conta) para obter mais detalhes sobre como configurar a RSID (ID de conjunto de relatórios).

![Habilitar configurações do Analytics](assets/channel-analytical-conf.png)

*Habilitar configurações do Analytics*

Selecione **Adicionar nova configuração de análise** para adicionar várias configurações.

Uma nova alteração de configuração foi criada e está pronta para atualização do servidor. Para usar a nova configuração de análise da seção **Configuração de análise**, continue com o fluxo de [alterações de revisão e push](/help/authentication/tve-dashboard-review-push-changes.md).

### Integrações {#integrations}

Essa guia exibe uma lista de integrações disponíveis entre o canal selecionado no momento e os MVPDs. A lista apresenta cada integração junto com seu status, indicando se está ativada ou não. Selecione uma integração específica na lista para acessar informações detalhadas na seção [Integrações](tve-dashboard-integrations.md).

![Lista de Integrações Disponíveis](assets/channel-integrations.png)

*Lista de Integrações Disponíveis*

### Certificados {#certificates}

Esta guia exibe uma lista de [certificados disponíveis](#available-certificates) e [certificados disponíveis herdados](#inherited-avail-certificates) usados nos fluxos de criptografia de metadados do usuário. Ela exibe detalhes sobre cada certificado que inclui:

* O status (habilitado para uso de **criptografia de metadados do usuário** ou não)
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

1. Selecione **Adicionar novo certificado** na parte superior da seção **Certificados disponíveis**.

   ![Adicionar um novo certificado](assets/add-new-certificate.png)

   *Adicionar um novo certificado*

1. Cole a chave pública do seu certificado na caixa de diálogo **Novo certificado**.
1. Selecione **Adicionar certificado**.

   Uma nova alteração de configuração foi criada e está pronta para atualização do servidor. Para usar o novo certificado listado na seção **Certificados Disponíveis**, prossiga com o fluxo de [alterações de revisão e envio por push](/help/authentication/tve-dashboard-review-push-changes.md).

1. Localize o novo certificado na lista de **Certificados Disponíveis**.

   >[!IMPORTANT]
   >
   > Verifique se seus sistemas estão atualizados e prontos para usar o novo certificado.

1. Selecione **Sim** no menu suspenso **Usado para criptografar metadados de usuário** para ativar um novo certificado.

##### Excluir certificado {#delete-certificate}

Siga estas etapas para excluir um certificado.

1. Passe o mouse sobre o certificado que deseja excluir da lista de **Certificados disponíveis**.
1. Selecione **Remover**.

   ![Remover o certificado selecionado](assets/channel-delete-certificate.png)

   *Remover o certificado selecionado*

1. Selecione **Excluir** da caixa de diálogo **Excluir certificado ativo**.

Uma nova alteração de configuração foi criada e está pronta para atualização do servidor. O certificado será excluído da seção **Certificados disponíveis** somente após [revisar e enviar alterações](/help/authentication/tve-dashboard-review-push-changes.md).

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

1. Selecione **Adicionar novo domínio** no canto superior direito da seção **Domínios disponíveis**.

   ![Adicionar um novo domínio](assets/add-new-domain.png)

   *Adicionar um novo domínio*

1. Digite o nome do seu domínio na caixa de diálogo **Novo domínio**.

1. Selecione **Adicionar domínio** para adicionar um novo domínio para o canal selecionado.

Uma nova alteração de configuração foi criada e está pronta para atualização do servidor. Para usar o novo domínio listado na seção **Domínios Disponíveis**, continue com o fluxo de [alterações de revisão e envio por push](/help/authentication/tve-dashboard-review-push-changes.md).

#### Excluir domínio {#delete-domain}

Siga estas etapas para excluir um domínio.

1. Passe o mouse sobre o domínio que deseja excluir da lista de **Domínios disponíveis**.
1. Selecione **Remover**.

   ![Remover o domínio selecionado](assets/remove-domain.png)

   *Remover o domínio selecionado*

1. Selecione **Excluir** na caixa de diálogo **Excluir domínio**.

Uma nova alteração de configuração foi criada e está pronta para atualização do servidor. O domínio será excluído da seção **Domínios Disponíveis** somente após [revisar e enviar alterações](/help/authentication/tve-dashboard-review-push-changes.md).

O domínio selecionado não está mais disponível para uso. Como resultado, o aplicativo associado a esse domínio perde acesso aos serviços de autenticação da Adobe Pass.

### Aplicativos registrados {#registered-applications}

Esta guia fornece uma lista de registros de aplicativos.

### Esquemas personalizados {#custom-schemes}

Esta guia exibe uma lista de esquemas personalizados. Exibir [registro de aplicativo iOS/tvOS](/help/authentication/iostvos-application-registration.md).

## Adicionar novo canal {#add-new-channel}

Siga estas etapas para adicionar um novo canal.

1. Selecione a guia **Canais** no painel esquerdo.
1. Selecione **Adicionar novo canal** no canto superior direito da seção **Canais**.

   ![Adicionar um novo canal](assets/add-new-channel.png)

   *Adicionar um novo canal*

1. Selecione **ID do Programador** no menu suspenso da caixa de diálogo **Novo canal**.

1. Digite um identificador exclusivo em **ID do canal**.
1. Digite o nome da marca do canal usado para fins comerciais no **Nome de exibição**.
1. Selecione **Adicionar canal**.

Uma nova alteração de configuração foi criada e está pronta para atualização do servidor. Para usar o novo canal listado na seção **Canais**, prossiga com o fluxo de [alterações de revisão e envio por push](/help/authentication/tve-dashboard-review-push-changes.md).
