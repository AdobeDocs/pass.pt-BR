---
title: Relatórios
description: Saiba como os dados são agregados nos relatórios do painel TVE.
source-git-commit: b81cc7498a8035f4c274ba25952dcd1dcd8d71f5
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 0%

---

# Relatórios {#Reports}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso dessa API exige uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

A variável **Relatórios** do Painel TVE fornece acesso aos dados agregados para os relatórios AuthN TTL, AuthZ TTL e SSO. Esses relatórios incluem suas integrações de canal com diferentes MVPDs em todas as [plataformas](#platforms).

Os relatórios permitem filtrar dados e coletar insights [Canais específicos ou MVPDs](#selecting-specific-channels-mvpds). Você também pode exportar relatórios em um arquivo CSV para análise adicional.

## Exibir relatórios {#view-reports}

Siga estas etapas para exibir um relatório específico.

1. Selecione o **Relatórios** no painel esquerdo.
1. Selecione uma das guias a seguir para exibir e exportar dados agregados dos canais e MVPDs incluídos:
   * [Relatórios AuthN TTL](#authn-ttl-reports)
   * [Relatórios TTL AuthZ](#authz-ttl-reports)
   * [Relatórios de SSO](#sso-reports)

   ![Tipo de relatórios](assets/type-of-reports.png)

   *Tipo de relatórios*

### Relatórios AuthN TTL {#authn-ttl-reports}

Os relatórios AuthN TTL, também conhecidos como Autenticação de vida útil (TTL), exibem a duração pela qual os tokens de autenticação são configurados para suas integrações de canais com vários MVPDs em todos [plataformas](#platforms). Esses relatórios permitem inspecionar o tempo que um usuário permanece autenticado para um MVPD e plataforma específicos. Os valores de duração são apresentados em formatos amigáveis, como, **dias**, **horas**, **minutos**, e **segundos**. A tabela de Relatórios AuthN TTL apresenta rolagem horizontal e vertical para acomodar diferentes tamanhos de tela.

Também é possível visualizar e baixar dados para [canais específicos ou MVPDs](#selecting-specific-channels-mvpds).

![Exportar relatórios AuthN TTL](assets/authn-ttl-reports.png)

*Exportar relatórios AuthN TTL*

>[!IMPORTANT]
>
> A variável **Definido por MVPD** O espaço reservado é usado quando o MVPD impõe o valor AuthN TTL em vez da configuração de Autenticação do Adobe Pass.

Selecionar **Exportar relatórios** para salvar os dados como um arquivo CSV no computador local.

### Relatórios TTL AuthZ {#authz-ttl-reports}

Os relatórios TTL de AuthZ, também chamados de TTL (Time-To-Live, tempo de vida da autorização), exibem a duração do token de autorização configurado para suas integrações de canais com vários MVPDs em todos os [plataformas](#platforms). Esses relatórios permitem inspecionar o tempo que um usuário permanece autorizado a assistir ao conteúdo de um MVPD e plataforma específicos. Os valores de duração são apresentados em formatos amigáveis, como, **dias**, **horas**, **minutos**, e **segundos**. A tabela de Relatórios TTL AuthZ apresenta rolagem horizontal e vertical para acomodar diferentes tamanhos de tela.

Também é possível visualizar e baixar os dados de [canais específicos ou MVPDs](#selecting-specific-channels-mvpds).

![Exportar relatórios AuthZ TTL](assets/authz-ttl-reports.png)

*Exportar relatórios AuthZ TTL*

>[!IMPORTANT]
>
> A variável **Definido por MVPD** O espaço reservado é usado quando o MVPD impõe o valor AuthZ TTL em vez da configuração de Autenticação do Adobe Pass.

Selecionar **Exportar relatórios** para salvar os dados como um arquivo CSV no computador local.

### Relatórios de SSO {#sso-reports}

Os relatórios de SSO, também chamados de logon único, exibem o status de logon único configurado para suas integrações de Canais com vários MVPDs em todos [plataformas](#platforms). Esses relatórios permitem inspecionar a experiência de SSO de autenticação de usuário esperada para um MVPD e plataforma específicos. Os valores são apresentados em formatos amigáveis, como, **SSO Desabilitado**, **Habilitado para SSO**, e **SSO Incerto**. A tabela Relatórios de SSO apresenta rolagem horizontal e vertical para acomodar diferentes tamanhos de tela.

Também é possível visualizar e baixar dados para [canais específicos ou MVPDs](#selecting-specific-channels-mvpds).

![Exportar relatórios de SSO](assets/sso-reports.png)

*Exportar relatórios de SSO*

>[!IMPORTANT]
>
> A variável **SSO Incerto** O espaço reservado indica que o Logon Único (SSO) está habilitado e possivelmente operacional. No entanto, as configurações listadas abaixo podem inibir a autenticação SSO, conforme explicado nos exemplos a seguir:
>
> * Configurações da plataforma do usuário: a opção para bloquear cookies de terceiros.
> * Decisões do usuário: os usuários negam acesso à plataforma à assinatura do provedor de TV.
> * Configurações de MVPD: O MVPD solicita autenticação para cada canal.

Selecionar **Exportar relatórios** para salvar os dados como um arquivo CSV no computador local.

## Plataformas {#platforms}

A variável [Relatórios AuthN TTL](#authn-ttl-reports), [Relatórios TTL AuthZ](#authz-ttl-reports), e [Relatórios de SSO](#sso-reports) apresentar dados em várias plataformas, como:

* **Desktop**: exibe valores aplicados às implementações do programador por meio do SDK JavaScript de autenticação da Adobe Pass.

* **Dispositivo móvel**

  **iOS**: exibe valores aplicados usando o SDK iOS de autenticação da Adobe Pass.

  **Android**: exibe valores aplicados por meio do SDK Android de autenticação da Adobe Pass.

  **Outros**: exibe valores aplicados usando a API REST de autenticação do Adobe Pass desenvolvida para dispositivos móveis.

* **TVCD**

  **Roku**: exibe valores aplicados por meio da API REST de autenticação do Adobe Pass, identificando o Roku como um tipo de dispositivo.

  **FireTV**: exibe valores aplicados por meio do SDK FireTV de autenticação da Adobe Pass.

  **AppleTV**: exibe valores aplicados por meio do SDK tvOS de autenticação da Adobe Pass.

  **Outros**: exibe valores aplicados usando a API REST de autenticação da Adobe Pass para dispositivos conectados à TV.

* **Plataforma não identificada**: exibe valores aplicados às implementações do programador quando os serviços de autenticação da Adobe Pass detectam um tipo de dispositivo desconhecido.

Para saber mais sobre como compartilhar o tipo de dispositivo desejado, como **Roku** com as APIs REST ou SDKs de Autenticação do Adobe Pass, visualize o mecanismo de [transmitindo informações do cliente](/help/authentication/passing-client-information-device-connection-and-application.md).

>[!IMPORTANT]
>
> Os dados agregados baseiam-se na configuração específica de cada ambiente de autenticação do Adobe Pass. Ao alternar entre diferentes ambientes do Painel de controle do TVE, espere variações nos dados nos relatórios. Consulte a [Ambientes de autenticação do Adobe Pass](/help/authentication/tve-dashboard-environments.md) para saber mais.

## Seleção de canais específicos e MVPDs {#selecting-specific-channels-mvpds}

A variável [Relatórios AuthN TTL](#authn-ttl-reports), [Relatórios TTL AuthZ](#authz-ttl-reports), e [Relatórios de SSO](#sso-reports) apresentar dados para **Todos os canais** integrações com **Todos os MVPDs** por padrão.

>[!NOTE]
>
> Se você desmarcar **Todos os canais** ou **Todos os MVPDs** nos respectivos menus suspensos, uma mensagem é exibida para fazer uma seleção e exibir relatórios significativos.

Para gerar um relatório para canais específicos:

1. Selecione o **Canais incluídos** na parte superior do relatório selecionado.

   ![Menu suspenso Canais incluídos](assets/include-channels.png)

   *Menu suspenso Canais incluídos*

1. Desmarcar **Todos os canais**.
1. Selecione os canais necessários na **Canais incluídos** menu suspenso para o qual você deseja gerar dados.

>[!NOTE]
>
> Para ter opções disponíveis no **MVPDs incluídos** selecione pelo menos um canal no menu suspenso **Canais incluídos** menu suspenso.

Para gerar um relatório para MVPDs específicos:

1. Selecione o **MVPDs incluídos** na parte superior do relatório selecionado.

   ![Menu suspenso MVPDs incluídos](assets/include-mvpds.png)

   *Menu suspenso MVPDs incluídos*

1. Desmarcar **Todos os MVPDs**.
1. Selecione os MVPDs necessários na **MVPDs incluídos** menu suspenso para o qual você deseja gerar dados.
