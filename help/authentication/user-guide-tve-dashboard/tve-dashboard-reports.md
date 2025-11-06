---
title: Relatórios
description: Saiba como os dados são agregados nos relatórios do painel TVE.
exl-id: d8ba48de-d743-4dc2-866c-7d6e3ff94773
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 0%

---

# Relatórios {#Reports}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso dessa API exige uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

A seção **Relatórios** do Painel TVE fornece acesso aos dados agregados para os relatórios AuthN TTL, AuthZ TTL e SSO. Esses relatórios incluem suas integrações de canal com diferentes MVPDs em todas as [plataformas](#platforms).

Os relatórios permitem filtrar dados e coletar insights em [Canais ou MVPDs específicos](#selecting-specific-channels-mvpds). Você também pode exportar relatórios em um arquivo CSV para análise adicional.

## Exibir relatórios {#view-reports}

Siga estas etapas para exibir um relatório específico.

1. Selecione a guia **Relatórios** no painel esquerdo.
1. Selecione uma das guias a seguir para exibir e exportar dados agregados dos canais e MVPDs incluídos:
   * [Relatórios AuthN TTL](#authn-ttl-reports)
   * [Relatórios TTL AuthZ](#authz-ttl-reports)
   * [Relatórios de SSO](#sso-reports)

   ![Tipo de relatórios](../assets/tve-dashboard/new-tve-dashboard/reports/reports-tabs-view.png)

   *Tipo de relatórios*

### Relatórios AuthN TTL {#authn-ttl-reports}

Os relatórios AuthN TTL, também chamados de TTL (Tempo de vida útil da autenticação), exibem a duração pela qual os tokens de autenticação são configurados para suas integrações de canais com vários MVPDs em todas as [plataformas](#platforms). Esses relatórios permitem inspecionar o tempo que um usuário permanece autenticado para uma MVPD e plataforma específicas. Os valores de duração são apresentados em formatos amigáveis, como **dias**, **horas**, **minutos** e **segundos**. A tabela de Relatórios AuthN TTL apresenta rolagem horizontal e vertical para acomodar diferentes tamanhos de tela.

Você também pode visualizar e baixar dados de [canais específicos ou MVPDs](#selecting-specific-channels-mvpds).

![Exportar Relatórios AuthN TTL](../assets/tve-dashboard/new-tve-dashboard/reports/reports-authn-ttl-export-button.png)

*Exportar Relatórios AuthN TTL*

>[!IMPORTANT]
>
> O espaço reservado **Set by MVPD** é usado quando o MVPD impõe o valor AuthN TTL em vez da configuração de Autenticação Adobe Pass.

Selecione **Exportar relatórios** para salvar os dados como um arquivo CSV no computador local.

### Relatórios TTL AuthZ {#authz-ttl-reports}

Os relatórios AuthZ TTL, também conhecidos como Autorização de Vida Útil (TTL), exibem a duração do token de autorização configurado para suas integrações de Canais com vários MVPDs em todas as [plataformas](#platforms). Esses relatórios permitem inspecionar o tempo que um usuário permanece autorizado a assistir ao conteúdo de uma MVPD e plataforma específicas. Os valores de duração são apresentados em formatos amigáveis, como **dias**, **horas**, **minutos** e **segundos**. A tabela de Relatórios TTL AuthZ apresenta rolagem horizontal e vertical para acomodar diferentes tamanhos de tela.

Você também pode visualizar e baixar os dados de [canais específicos ou MVPDs](#selecting-specific-channels-mvpds).

![Exportar relatórios AuthZ TTL](../assets/tve-dashboard/new-tve-dashboard/reports/reports-authz-ttl-export-button.png)

*Exportar relatórios AuthZ TTL*

>[!IMPORTANT]
>
> O espaço reservado **Set by MVPD** é usado quando o MVPD impõe o valor TTL AuthZ em vez da configuração de Autenticação Adobe Pass.

Selecione **Exportar relatórios** para salvar os dados como um arquivo CSV no computador local.

### Relatórios de SSO {#sso-reports}

Os relatórios de SSO, também chamados de logon único, exibem o status de logon único configurado para suas integrações de Canais com vários MVPDs em todas as [plataformas](#platforms). Esses relatórios permitem inspecionar a experiência de SSO de autenticação de usuário esperada para uma MVPD e plataforma específicas. Os valores são apresentados em formatos amigáveis, como **SSO Desabilitado**, **SSO Habilitado** e **SSO Incerto**. A tabela Relatórios de SSO apresenta rolagem horizontal e vertical para acomodar diferentes tamanhos de tela.

Você também pode visualizar e baixar dados de [canais específicos ou MVPDs](#selecting-specific-channels-mvpds).

![Exportar relatórios de SSO](../assets/tve-dashboard/new-tve-dashboard/reports/reports-sso-export-button.png)

*Exportar relatórios de SSO*

>[!IMPORTANT]
>
> O espaço reservado **SSO Incerto** indica que o Logon Único (SSO) está habilitado e possivelmente operacional. No entanto, as configurações listadas abaixo podem inibir a autenticação SSO, conforme explicado nos exemplos a seguir:
>
> * Configurações da plataforma do usuário: a opção para bloquear cookies de terceiros.
> * Decisões do usuário: os usuários negam acesso à plataforma à assinatura do provedor de TV.
> * Configurações do MVPD: o MVPD solicita autenticação para cada canal.

Selecione **Exportar relatórios** para salvar os dados como um arquivo CSV no computador local.

## Plataformas {#platforms}

Os [Relatórios TTL de AuthN](#authn-ttl-reports), [Relatórios TTL de AuthZ](#authz-ttl-reports) e [Relatórios SSO](#sso-reports) apresentam dados em várias plataformas, como:

* **Área de Trabalho**: exibe os valores aplicados às implementações do programador através do JavaScript SDK de Autenticação da Adobe Pass.

* **Celular**

  **iOS**: exibe os valores aplicados usando o SDK iOS de Autenticação da Adobe Pass.

  **Android**: exibe os valores aplicados por meio do SDK Android de Autenticação da Adobe Pass.

  **Outros**: exibe os valores aplicados usando a API REST de Autenticação do Adobe Pass desenvolvida para dispositivos móveis.

* **TVCD**

  **Roku**: exibe os valores aplicados por meio da API REST de Autenticação do Adobe Pass, identificando o Roku como um tipo de dispositivo.

  **FireTV**: exibe valores aplicados por meio do FireTV SDK de autenticação da Adobe Pass.

  **AppleTV**: exibe valores aplicados por meio da Autenticação da Adobe Pass tvOS SDK.

  **Outros**: exibe os valores aplicados usando a API REST de Autenticação do Adobe Pass para dispositivos conectados à TV.

* **Plataforma não identificada**: exibe os valores aplicados às implementações do programador quando os serviços de Autenticação da Adobe Pass detectam um tipo de dispositivo desconhecido.

Para saber mais sobre como compartilhar o tipo de dispositivo desejado, como **Roku** com APIs REST de Autenticação Adobe Pass ou SDKs, exiba o mecanismo de [transmissão de informações do cliente](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md).

>[!IMPORTANT]
>
> Os dados agregados baseiam-se na configuração específica de cada ambiente de autenticação do Adobe Pass. Ao alternar entre diferentes ambientes do Painel de controle do TVE, espere variações nos dados nos relatórios. Consulte os [Ambientes de autenticação do Adobe Pass](/help/authentication/user-guide-tve-dashboard/tve-dashboard-environments.md) para saber mais.

## Seleção de canais específicos e MVPDs {#selecting-specific-channels-mvpds}

Os [Relatórios TTL de AuthN](#authn-ttl-reports), [Relatórios TTL de AuthZ](#authz-ttl-reports) e [Relatórios SSO](#sso-reports) apresentam dados para **Todos os Canais** integrações com **Todos os MVPDs** por padrão.

>[!NOTE]
>
> Se você desmarcar **Todos os Canais** ou **Todos os MVPDs** nos respectivos menus suspensos, será exibida uma mensagem para fazer uma seleção e exibir relatórios significativos.

Para gerar um relatório para canais específicos:

1. Selecione o menu suspenso **Canais incluídos** na parte superior do relatório selecionado.

   ![Menu suspenso Canais incluídos](../assets/tve-dashboard/new-tve-dashboard/reports/reports-included-channels-menu.png)

   *Menu suspenso Canais incluídos*

1. Desmarque **Todos os canais**.

1. Selecione os canais necessários no menu suspenso **Canais incluídos** para os quais deseja gerar dados.

>[!NOTE]
>
> Para disponibilizar opções no menu suspenso **MVPDs** incluídos, selecione pelo menos um canal no menu suspenso **Canais incluídos**.

Para gerar um relatório para MVPDs específicos:

1. Selecione o menu suspenso **MVPDs** incluídos na parte superior do relatório selecionado.

   ![Menu suspenso MVPDs incluídos](../assets/tve-dashboard/new-tve-dashboard/reports/reports-included-mvpds-menu.png)

   *Menu suspenso MVPDs incluídos*

1. Desmarque **Todos os MVPDs**.

1. Selecione os MVPDs necessários no menu suspenso **MVPDs incluídos** para os quais deseja gerar dados.
