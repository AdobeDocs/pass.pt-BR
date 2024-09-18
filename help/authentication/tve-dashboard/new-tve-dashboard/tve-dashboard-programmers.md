---
title: Programadores
description: Saiba mais sobre os programadores e suas configurações no painel TVE.
exl-id: b450d7cc-d5b5-4454-8f95-8047856bfb98
source-git-commit: acff285f7db1bdd32d5da3e01a770d9581d3ba75
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 0%

---

# Programadores {#programmers}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso dessa API exige uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

A seção **Programadores** do Painel do TVE permite exibir e gerenciar as configurações dos [programadores](/help/authentication/glossary.md#programmer) vinculados aos direitos da sua conta. Você também pode [adicionar um novo programador](#add-new-programmer) de acordo com sua necessidade.

A guia **Programadores** no painel esquerdo exibe uma lista de programadores existentes com os seguintes detalhes:

* **ID do Programador**: Um identificador de empresa de mídia no sistema.
* **Canais**: o número de canais associados vinculados a um programador.

![Lista de programadores existentes](../../assets/tve-dashboard/new-tve-dashboard/programmers/programmers-list-view.png)

*Lista de programadores existentes*

Digite o nome do programador na barra **Pesquisa** acima da lista para saber mais sobre um programador.

## Gerenciar configurações do programador {#manage-programmer-conf}

Siga estas etapas para gerenciar várias configurações de um programador específico.

1. Selecione a guia **Programadores** no painel esquerdo.
1. Selecione um programador na lista.
1. Selecione uma das guias a seguir para exibir e editar as configurações correspondentes do programador selecionado:

   * [Canais](#channels)
   * [Certificados](#certificates)
   * [Aplicativos registrados](#registered-applications)
   * [Esquemas personalizados](#custom-schemes)

   ![Configurações do programador](../../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-tabs-view.png)

   *Configurações do programador*

>[!IMPORTANT]
>
> Exiba [Revisar e enviar alterações](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-review-push-changes.md) para obter mais informações sobre como ativar as alterações de configuração.

### Canais {#channels}

Esta guia exibe uma lista de canais vinculados a um programador atual. Selecione um canal específico na lista para acessar informações detalhadas na seção [Canais](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md).

Para adicionar um novo canal para o programador selecionado, selecione **Adicionar novo canal** no canto superior direito da seção **Canais disponíveis**. Saiba [como adicionar um novo canal](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md#add-new-channel).

![Adicionar um novo canal](../../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-channel-button.png)

*Adicionar um novo canal*

### Certificados {#certificates}

Esta guia exibe uma lista de [certificados disponíveis](#available-certificates) usados nos fluxos de criptografia de metadados do usuário. Ela exibe detalhes sobre cada certificado que inclui:

* O status (habilitado para uso de **criptografia de metadados do usuário** ou não)
* Número de série
* Nome da organização emissora
* Nome da organização do assunto
* Data de emissão
* Data de expiração
* Um menu suspenso para criptografar metadados do usuário (Se você selecionar **Sim**, o certificado criptografará informações confidenciais do usuário, como valores de código postal).

#### Certificados disponíveis {#available-certificates}

Esses certificados servem como chaves privadas ou públicas e são usados para criptografia de metadados do usuário. Todos os canais associados à mesma empresa de mídia podem usar esses certificados.

Você pode fazer as seguintes alterações nos certificados disponíveis:

* [Adicionar novo certificado](#add-new-certificate)
* [Excluir certificado](#delete-certificate)

##### Adicionar novo certificado {#add-new-certificate}

Siga estas etapas para adicionar um novo certificado.

1. Selecione **Adicionar novo certificado** no canto superior direito da seção **Certificados disponíveis**.

   ![Adicionar um novo certificado](../../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-certificate-button.png)

   *Adicionar um novo certificado*

1. Cole a chave pública do seu certificado na caixa de diálogo **Novo certificado**.

1. Selecione **Adicionar certificado**.

1. Localize o novo certificado na lista de **Certificados Disponíveis**.

   >[!IMPORTANT]
   >
   > Verifique se seus sistemas estão atualizados e prontos para usar o novo certificado.

1. Selecione **Sim** no menu suspenso **Usado para criptografar metadados de usuário** para ativar um novo certificado.

Uma nova alteração de configuração foi criada e está pronta para atualização do servidor. Para usar o novo certificado listado na seção **Certificados Disponíveis**, prossiga com o fluxo de [alterações de revisão e envio por push](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-review-push-changes.md).

##### Excluir certificado {#delete-certificate}

Siga estas etapas para excluir um certificado.

1. Passe o mouse sobre o certificado que deseja excluir da lista de **Certificados disponíveis**.

1. Selecione **Remover**.

   ![Remover o certificado selecionado](../../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-remove-certificate-button.png)

   *Remover o certificado selecionado*

1. Selecione **Excluir** na caixa de diálogo **Excluir certificado**.

Uma nova alteração de configuração foi criada e está pronta para atualização do servidor. O certificado será excluído da seção **Certificados disponíveis** somente após [revisar e enviar alterações](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-review-push-changes.md).

### Aplicativos registrados {#registered-applications}

Esta guia fornece uma lista de registros de aplicativos.

### Esquemas personalizados {#custom-schemes}

Esta guia exibe uma lista de esquemas personalizados. Exibir [registro de aplicativo iOS/tvOS](/help/authentication/iostvos-application-registration.md).

## Adicionar novo programador {#add-new-programmer}

Siga estas etapas para adicionar uma nova entidade programadora.

1. Selecione a guia **Programadores** no painel esquerdo.

1. Selecione **Adicionar novo programador** no canto superior direito da seção **Programadores**.

   ![Adicionar um novo programador](../../assets/tve-dashboard/new-tve-dashboard/programmers/programmer-add-new-programmer-button.png)

   *Adicionar um novo programador*

1. Digite o identificador da empresa de mídia em **ID do Programador** na caixa de diálogo **Novo programador**.

1. Digite o nome de uma marca comercial que você deseja mostrar no console em **Nome de exibição**.

1. Selecione **Adicionar programador**.

Uma nova alteração de configuração foi criada e está pronta para atualização do servidor. Para usar o novo programador listado na seção **Programadores**, prossiga com o fluxo de [alterações de revisão e envio por push](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-review-push-changes.md).