---
title: Integrações do painel TVE
description: Saiba mais sobre as integrações entre seus canais e MVPDs e como gerenciar integrações.
exl-id: 0add340b-120c-4e82-8e3c-6c190d77cf7e
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '2093'
ht-degree: 0%

---

# Integrações

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso dessa API exige uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

A seção **Integrações** do Painel TVE permite exibir e gerenciar configurações para as integrações entre seus canais e os MVPDs. Você também pode [criar uma nova integração](#create-new-integration) de acordo com sua necessidade.

A guia **Integrações** no painel esquerdo exibe uma lista de integrações existentes com os seguintes detalhes:

* Status que indica se a integração está ativa ou inativa no momento
* Integração que vincula canais específicos aos respectivos MVPDs
* Nome do canal com ID de canal
* Nome para exibição do MVPD e ID do MVPD

![Lista de integrações existentes](../assets/tve-dashboard/new-tve-dashboard/integrations/integrations-list.png)

*Lista de integrações existentes*

Digite o nome do canal ou MVPD na barra **Pesquisa** acima da lista para saber mais sobre a integração.

## Gerenciar configurações de integração {#manage-integration-conf}

Siga estas etapas para gerenciar uma integração específica.

1. Selecione a guia **Integrações** no painel esquerdo.
1. Selecione uma integração na lista fornecida para exibir e editar várias configurações nas seguintes seções:

   * [Seleção de endpoint](#endpoint-selection)
   * [Configurações da plataforma](#platform-settings)
   * [Metadados do usuário](#user-metadata)

>[!IMPORTANT]
>
> Exiba [Revisar e enviar alterações](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md) para obter mais informações sobre como ativar as alterações de configuração.

### Seleção de ponto de extremidade {#endpoint-selection}

Esta seção permite escolher os endpoints do MVPD usados para fluxos de autenticação, autorização e logout nos respectivos menus suspensos.

![Pontos de extremidade para fluxos de autenticação, autorização e logout](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-endpoint-selection-panel-view.png)

*Pontos de extremidade para fluxos de autenticação, autorização e logout*

>[!NOTE]
>
>Os MVPDs podem fornecer um ou vários endpoints para cada fluxo. Ao integrar um novo canal, o MVPD deve especificar seu endpoint preferencial para cada fluxo.

>[!IMPORTANT]
>
>Qualquer alteração nos endpoints afetará o comportamento geral de uma integração. Essas alterações só devem ser implementadas após receber a confirmação do MVPD.

### Configurações da plataforma {#platform-settings}

Esta seção permite exibir e editar configurações de integração em todas as [plataformas](/help/authentication/user-guide-tve-dashboard/tve-dashboard-reports.md#platforms). Você pode alterar essas configurações com base em plataformas individuais. Por exemplo, é possível ajustar a duração do TTL de autorização no Android enquanto mantém um valor padrão para outra plataforma.

Cada propriedade nas configurações da plataforma herda um valor padrão definido pelo MVPD, mas pode ser ajustado se necessário.

>[!IMPORTANT]
>
>Um acordo com o MVPD é necessário para determinar valores definidos para cada propriedade nas configurações da plataforma.

>[!IMPORTANT]
>
> A herança de configurações segue uma cadeia que começa nas configurações do MVPD (que são as mais gerais), depois o endpoint do MVPD, a integração, a categoria da plataforma e a plataforma (que contém o valor mais específico).

**Configurações de plataforma** é usado para substituir configurações para cada nível na cadeia de herança. Os níveis disponíveis na cadeia são agrupados da seguinte maneira:

* **Padrão para Todos**: defina valores para propriedades aplicáveis universalmente em todas as plataformas se os valores de plataforma específicos não estiverem definidos, independentemente das implementações do Programador.

* **Dispositivos desktop**: defina valores para propriedades aplicáveis a todos os computadores desktop e laptop, independentemente do método de programação (SDK JS ou API REST).

* **Dispositivos móveis**: defina valores para propriedades aplicáveis a todos os dispositivos móveis, incluindo o **iOS**, o **Android** e outros, independentemente da abordagem de programação (SDK ou API REST).

* **Dispositivos Conectados de TV**: Defina valores para propriedades aplicáveis a todos os dispositivos conectados de TV, incluindo **tvOS**, **Roku**, **FireTV** e outros, independentemente do método de programação (SDK ou API REST).

* **Dispositivos não identificados**: defina valores para propriedades aplicáveis a todos os dispositivos nos quais o mecanismo atual não pode identificar com precisão a plataforma. Nesses casos, aplique as regras mais restritivas definidas pelo MVPD.

  ![Categoria de plataformas e seus dispositivos](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-menu.png)

  *Categoria de plataformas e seus dispositivos*

Selecionar Ícone <img alt= "ícone de cadeia de herança" src="../assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-inheritance-chain-icon.svg" width="25"> localizado à direita de cada propriedade para explorar as propriedades usadas para cada nível de herança descrito acima.

#### Fluxos de negócios mais usados {#most-used-flows}

A seção **Configurações de Plataforma** oferece um intervalo de propriedades usadas em diferentes fluxos comerciais. As propriedades reais podem variar dependendo dos MVPDs selecionados na integração específica. Abaixo estão os fluxos mais usados:

**TTL de AuthN e TTL de AuthZ em todas as plataformas**

>[!IMPORTANT]
>
>Os valores TTL de Autenticação (AuthN) e Autorização (AuthZ) devem estar consistentemente alinhados com as configurações MVPD.

Siga estas etapas para alterar a autenticação e a autorização TTL em todas as plataformas para uma integração específica.

1. Selecione a guia **Integrações** no painel esquerdo.

1. Selecione a integração cujos valores TTL de AuthN e TTL de AuthZ você deseja alterar.

1. Navegue até a seção **Configurações da plataforma**.

1. Selecione **Padrão para Todos** na guia **Configurações da plataforma**.

   >[!NOTE]
   >
   >Se você quiser alterar a duração de **AuthN TTL** e **AuthZ TTL** para uma categoria de plataforma ou uma plataforma específica, selecione a plataforma de acordo.

   ![Alterar duração de TTL AuthN TTL AuthZ em todas as plataformas](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-authn-ttl-authz-ttl-properties.png)

   *Alterar duração de TTL AuthN TTL AuthZ em todas as plataformas*

   **A.** Propriedade TTL AuthN **B.** Propriedade TTL AuthZ

1. Selecione as setas para cima e para baixo para ajustar a duração do número de dias, horas, minutos e segundos nas propriedades **AuthN TTL** e **AuthZ TTL**.

A duração de **AuthN TTL** e **AuthZ TTL** em todas as plataformas será atualizada somente após [revisar e enviar alterações](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

**Habilitar plataforma SSO**

>[!IMPORTANT]
>
>A propriedade **Habilitar Logon Único** tem suporte exclusivo em *plataformas iOS, tvOS, Roku e FireTV*. Ela só é aplicável a integrações com MVPDs que oferecem suporte a logon único para essas plataformas.

Siga estas etapas para ativar ou desativar o SSO para uma integração e plataforma específicas.

1. Selecione a guia **Integrações** no painel esquerdo.

1. Selecione a integração para a qual deseja ativar ou desativar o logon único.

1. Navegue até a seção **Configurações da plataforma**.

1. Selecione uma plataforma específica ou categoria de plataformas para a qual você deseja habilitar o logon único em **Configurações da Plataforma**.

   ![Habilitar Logon Único para uma plataforma específica](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-single-sign-on-properties.png)

   *Habilitar Logon Único para uma plataforma específica*

   **A.** Propriedade de Logon Único **B.** Impor propriedade de Permissões da Plataforma

1. Selecione **Sim** para habilitar ou **Não** para desabilitar no menu suspenso **Habilitar Logon Único**.

1. Selecione **Sim** para habilitar ou **Não** para desabilitar no menu suspenso **Impor Permissão de Plataforma**.

   A propriedade **Impor Permissão de Plataforma** controla se a decisão do usuário de **Permitir** ou **Negar** acesso à plataforma para sua assinatura do Provedor de TV é respeitada.

   Por exemplo, se o **Habilitar Logon Único** e o **Impor Permissão de Plataforma** estiverem habilitados e o usuário optar por negar acesso à plataforma à sua assinatura do Provedor de TV, o respectivo aplicativo (canal) não poderá usar o token de Autenticação do Adobe Pass obtido por outro aplicativo (canal).

A propriedade **Logon Único** de uma plataforma selecionada será habilitada ou desabilitada somente após [revisar e enviar alterações](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

**Habilitar autenticação baseada em casa**

Siga estas etapas para habilitar ou desabilitar a autenticação baseada em página inicial para MVPDs baseados em OAuth2.

1. Selecione a guia **Integrações** no painel esquerdo.

1. Selecione a integração para a qual deseja ativar ou desativar a autenticação baseada em página inicial.

1. Navegue até a seção **Configurações da plataforma**.

1. Selecione uma plataforma específica ou categoria de plataformas para as quais você deseja habilitar a autenticação baseada em página inicial em **Configurações da plataforma**.

   ![Habilitar autenticação baseada em casa para uma plataforma específica](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-attempt-hba-properties.png)

   *Habilitar autenticação baseada em casa para uma plataforma específica*

   **A.** Tentativa de propriedade HBA **B.** Propriedade HBA AuthN TTL

1. Selecione **Sim** para habilitar e **Não** para desabilitar no menu suspenso **Tentar HBA**.

>[!IMPORTANT]
>
>A alteração da duração da propriedade **HBA AuthN TTL** deve ser evitada. Pode resultar em falhas inesperadas no processo de autorização.

A propriedade **Attempt HBA** para um MVPD específico será habilitada ou desabilitada somente após [revisar e enviar alterações](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

#### Adicionar mais propriedades {#add-more-properties}

O **Adicionar mais propriedades** permite a flexibilidade de incluir propriedades específicas adicionais para integrações, principalmente para fluxos menos comuns.

É possível adicionar estas propriedades:

* Para todas as plataformas, selecione **Padrão para todas** na guia à esquerda.
* Para uma categoria de plataforma, selecione a guia **Dispositivos Desktop**, **Dispositivos Móveis** ou **Dispositivos Conectados de TV** à esquerda.
* Para um dispositivo específico, selecione a guia **iOS**, **Android**, **tvOS**, **Roku** ou **FireTV** à esquerda.

Estes são alguns exemplos de fluxos diferentes que podem ser ativados ao adicionar essas propriedades:

**Alterar o número de recursos pré-autorizados**

A maioria dos MVPDs oferece suporte a uma chamada authZ de comprovação usando até 5 IDs de recursos por padrão.
No entanto, nos casos em que os MVPDs concordam em elevar esse limite, você pode navegar até **Adicionar mais propriedades** e selecionar **Recursos máximos de comprovação** no menu de opções.

**Recursos Máximos de Comprovação** adicionará um novo atributo onde o limite acordado com o MVPD pode ser especificado.

![Adicionar a propriedade Recursos Máximos de Comprovação](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-preflight-max-resources-properties.png)

*Adicionar a propriedade Recursos Máximos de Comprovação*

A propriedade **Recursos máximos de comprovação** será adicionada somente após [revisar e enviar alterações](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

**Alterar o nome para exibição ou a URL do logotipo do MVPD**

Para aplicativos de programador que não desejam criar o seletor de MVPD e dependem das configurações fornecidas, navegue até **Adicionar mais propriedades** e selecione **Nome para Exibição** ou **URL do Logotipo** para adicionar o nome para exibição ou as URLs do logotipo necessárias para cada MVPD no menu Opções.

Valores diferentes para essas propriedades podem ser usados para o mesmo MVPD, dependendo da plataforma do dispositivo e da experiência do usuário desejada.

![Adicionar propriedade de Nome para Exibição ou URL do Logotipo](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-display-name-logo-url-properties.png)

*Adicionar propriedade de Nome para Exibição ou URL do Logotipo*

A propriedade **Nome para Exibição** ou **URL do Logotipo** será adicionada somente após [revisar e enviar alterações](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

**Solicitar um novo fluxo de autenticação após a alternância de aplicativo (canal)**

Se você quiser forçar uma nova autenticação quando os usuários alternarem entre aplicativos. Nesse caso, você pode navegar até a **Adicionar mais propriedades**, selecione a propriedade **Auth por Agregador**.

A adição de **Auth por Agregador** efetivamente interrompe o logon único para o respectivo canal.

![Adicionar Autenticação por Propriedade Agregadora](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-auth-per-aggregator-properties.png)

*Adicionar Autenticação por Propriedade Agregadora*

A propriedade **Auth por Agregador** será adicionada somente após [revisar e enviar alterações](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

Depois de adicionada, selecione **Sim** para habilitar a propriedade **Auth por Agregador** para uma integração selecionada.

#### Excluir propriedades {#delete-properties}

Selecionar Ícone <img alt= "botão excluir propriedade" src="../assets/tve-dashboard/new-tve-dashboard/integrations/integration-platform-settings-delete-property-icon.svg" width="25"> localizado à direita de cada propriedade para excluir as propriedades que não são mais necessárias.

>[!NOTE]
>
>Determinadas propriedades não podem ser removidas, pois são requisitos obrigatórios para o MVPD selecionado.

A propriedade será excluída da seção **Configurações de plataforma** somente após [revisar e enviar alterações](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

### Metadados do usuário {#user-metadata}

Esta seção permite atualizar as configurações para cada parâmetro de metadados do usuário compartilhado pelo MVPD.

>[!NOTE]
>
>Cada MVPD pode compartilhar diferentes parâmetros. Para obter mais informações sobre os parâmetros que um MVPD específico pode compartilhar, entre em contato com o representante da Adobe.

A seção de metadados do usuário exibe as seguintes colunas:

**Chave**: representa os parâmetros de metadados do usuário real a serem usados na API para extrair valores.

**Descrição**: fornece uma breve descrição de cada parâmetro de metadados do usuário.

**Criptografada**: esta coluna permite habilitar ou desabilitar parâmetros na API ao selecionar **Sim** ou **Não** no menu suspenso, respectivamente. Optar por **Sim** indica que o valor do parâmetro será criptografado na API. A criptografia é executada usando um certificado definido por um escopo **de metadados de usuário**.

>[!TIP]
>
>
> Sempre verifique se o parâmetro **ZIP** está criptografado.

Saiba mais sobre certificados disponíveis nas seções [Programadores](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#available-certificates) e [Canais](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#available-certificates).

**Habilitado**: esta coluna permite habilitar ou desabilitar os parâmetros na API ao selecionar **Sim** ou **Não**, respectivamente, no menu suspenso.

![Parâmetros disponíveis para Metadados de Usuário](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-user-metadata-panel-view.png)

*Parâmetros disponíveis para Metadados de Usuário*

## Criar nova integração {#create-new-integration}

Para criar uma nova integração com um novo MVPD na configuração atual, siga estas etapas:

1. Selecione a guia **Integrações** no painel esquerdo.

1. Selecione **Criar nova integração** no canto superior direito da seção **Integrações**.

   ![Criar uma nova integração](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-create-new-integration-button.png)

   *Criar uma nova integração*

   As seguintes seções são exibidas:

   **Selecionar canal e MVPD**

   Selecione um **Canal** no menu suspenso **Selecionar canal** para adicionar uma nova integração. Depois de selecionar o canal, selecione o **MVPD** necessário no menu suspenso **Selecionar MVPD** para ser integrado ao canal selecionado.

   ![Selecionar canal e MVPD](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-new-integration-select-channel-and-mvpd-panel-view.png)

   *Selecionar canal e MVPD*

   **Selecionar pontos de extremidade**

   Após selecionar o MVPD necessário, a seção **Selecionar ponto de extremidade** será pré-preenchida com os pontos de extremidade padrão configurados para esse MVPD específico.

   >[!IMPORTANT]
   >
   >Não altere os pontos de extremidade padrão em nenhum fluxo, a menos que seja declarado especificamente pelo MVPD.

   ![Selecionar pontos de extremidade &#x200B;](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-new-integration-select-endpoints-panel-view.png)

   *Selecionar pontos de extremidade*

   **Informações adicionais**

   Esta seção inclui várias propriedades que precisam ser configuradas para o MVPD selecionado na seção **Selecionar canal e MVPD**.

   >[!NOTE]
   >
   > As propriedades reais podem diferir dependendo dos MVPDs selecionados na seção **Selecionar canal e MVPD**.

   Por exemplo, você pode editar o **AuthN TTL** ou a **ID do Parceiro** (ID do Canal) para fins de parceria entre marcas na página de logon do MVPD na imagem a seguir.

   ![Editar informações adicionais](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-new-integration-additional-information-panel-view.png)

   *Editar informações adicionais*

   Selecione **Salvar integração** no canto superior direito da seção **Criar nova integração**.

Uma nova integração será criada somente após [revisar e enviar alterações](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).


## Desabilitar integração {#disable-integration}

Para desabilitar uma integração, siga estas etapas:

1. Selecione a guia **Integrações** no painel esquerdo.

1. Selecione a integração que deseja desativar.

1. Desative o botão de alternância disponível na parte superior direita da integração selecionada.

   ![Desabilitar integração](../assets/tve-dashboard/new-tve-dashboard/integrations/integration-enabled-disabled-button.png)

   *Desabilitar integração*

A integração será desabilitada somente após [revisar e enviar alterações](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md).

Depois que a integração for desativada, os usuários finais perderão a capacidade de se autenticar ou autorizar usando o MVPD específico.
