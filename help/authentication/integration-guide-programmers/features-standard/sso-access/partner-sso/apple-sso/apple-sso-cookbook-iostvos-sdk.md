---
title: Guia do Apple SSO (iOS/tvOS SDK)
description: Guia do Apple SSO (iOS/tvOS SDK)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1811'
ht-degree: 0%

---

# Guia do Apple SSO (iOS/tvOS SDK) {#apple-sso-cookbook-iostvos-sdk}

>[!IMPORTANT]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

O SDK do iOS/tvOS do Adobe Pass Authentication AccessEnabler tem suporte para Logon único de parceiro (SSO) para usuários finais de aplicativos clientes em execução no iOS, iPadOS ou tvOS.

Este documento atua como uma extensão da documentação existente do SDK iOS/tvOS do AccessEnabler, que pode ser encontrada [aqui](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md).

## Guia {#apple-sso-cookbook-iostvos-sdk-cookbook}

Para se beneficiar da experiência do usuário do Apple SSO, o aplicativo precisa integrar o SDK do AccessEnabler iOS/tvOS e seguir a sequência de etapas apresentada abaixo.

### Pré-requisitos {#apple-sso-cookbook-iostvos-sdk-prerequisites}

#### Permissão {#apple-sso-cookbook-iostvos-sdk-permission}

>[!TIP]
>
> **<u>Dica Profissional:</u>** o aplicativo de streaming deve solicitar acesso às informações de assinatura do usuário salvas no nível do dispositivo, para o qual o usuário deve dar permissão ao aplicativo para continuar, de modo semelhante a fornecer acesso à câmera ou ao microfone do dispositivo. Esta permissão deve ser solicitada por aplicativo usando a [Estrutura de Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) da Apple, e o dispositivo salvará a seleção do usuário.

>[!TIP]
>
> **<u>Dica Profissional:</u>** Recomendamos incentivar os usuários que se recusam a conceder permissão de acesso às informações de assinatura explicando os benefícios da experiência do usuário de logon único do Apple, mas esteja ciente de que o usuário pode alterar sua decisão acessando as configurações do aplicativo (acesso de permissão ao Provedor de TV) ou *`Settings -> TV Provider`* no iOS e iPadOS ou *`Settings -> Accounts -> TV Provider`* no tvOS.

>[!TIP]
>
> **<u>Dica Profissional:</u>** o aplicativo de streaming pode solicitar a permissão do usuário quando o aplicativo entrar no estado de primeiro plano, pois o aplicativo pode verificar a [permissão para acessar](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) as informações de assinatura do usuário a qualquer momento antes de exigir autenticação do usuário.

>[!TIP]
>
> **<u>Dica Profissional:</u>** caso o usuário não conceda acesso às suas informações de assinatura ou caso a comunicação com a Estrutura de Conta de Assinante de Vídeo falhe, o SDK iOS/tvOS AccessEnabler recorrerá ao fluxo de autenticação regular.

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                   // Do nothing.
                
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                   // Incentivize users who refuse to give permission to access subscription information by explaining the benefits of the Single Sign-On (SSO) user experience. Please bear in mind that the user can change its decision by going to the application settings (TV Provider permission access) or to the section from Settings -> TV Provider on iOS/iPadOS or Settings -> Accounts -> TV Provider on tvOS.
                   ...
                }
    }
    ... 
```

#### Retornos de chamada {#apple-sso-cookbook-iostvos-sdk-callbacks}

>[!TIP]
>
> **<u>Dica Profissional:</u>** Implemente a seguinte lista de [retornos de chamada](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md) que são específicos para o fluxo de trabalho SSO do Apple.

* [*presentTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Retorno de chamada disparado quando o seletor de MVPD do Apple será aberto.
* [*dismissTVProviderDialog*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Retorno de chamada disparado quando o seletor de MVPD do Apple vai fechar.

#### Relatório de erros {#apple-sso-cookbook-iostvos-sdk-error-reporting}

>[!TIP]
>
> **<u>Dica Profissional:</u>** Implemente a seguinte lista de [códigos de erro avançados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md) que são específicos para o fluxo de trabalho SSO do Apple.

* ***N003*** - O usuário selecionou a opção &quot;Outro Provedor de TV&quot; no seletor Apple MVPD.
* ***N004*** - O usuário selecionou um Provedor de TV no seletor de MVPD do Apple, para o qual não há suporte (integração ou Logon Único desabilitado) pelo solicitante atual.
* ***N005*** - O usuário decidiu cancelar o seletor de MVPD normal ou o seletor de MVPD do Apple.
* ***VSA403*** - Permissão de Provedor de TV do usuário negada para o aplicativo.
* ***VSA404*** - A permissão Provedor de TV do usuário não foi determinada para o aplicativo.
* ***VSA503*** - Falha na solicitação de metadados da Conta do Assinante do Vídeo. Mais contexto é fornecido no campo *message*.
* ***AAPL / APPL_ERROR*** - A solicitação de metadados da Conta do Assinante do Vídeo falhou, mais contexto é fornecido no campo *details*.

### Autenticação {#apple-sso-cookbook-iostvos-sdk-authentication}

>[!TIP]
>
> **<u>Dica:</u>** siga as etapas abaixo para as implementações do iOS/iPadOS/tvOS.

1. O aplicativo teria que [inicializar](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) o SDK iOS/tvOS do AccessEnabler.


1. O aplicativo teria que [definir o identificador do solicitante atual](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3).

   **Importante:** esta segunda etapa pode disparar um [código de erro avançado](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md) que é específico para o fluxo de trabalho SSO do Apple, no caso de **uma das seguintes opções ser verdadeira**:

   * ***VSA403*** - Permissão de Provedor de TV do usuário negada para o aplicativo.
   * ***VSA404*** - A permissão Provedor de TV do usuário não foi determinada para o aplicativo.
   * ***APPL*** - A comunicação entre o SDK iOS/tvOS do AccessEnabler e a Estrutura de Conta de Assinante de Vídeo encontrou um erro.

   Esta segunda etapa tentaria trocar silenciosamente o perfil de SSO do Apple por um token de autenticação Adobe, caso **todas as opções acima sejam falsas** e **todas as opções a seguir sejam verdadeiras**:

   * A permissão Provedor de TV do usuário é concedida para o aplicativo.
   * O usuário está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo.
   * O SDK iOS/tvOS do AccessEnabler recebeu o identificador do provedor de TV do usuário da Estrutura da conta do assinante de vídeo.
   * A integração do Provedor de TV do usuário com o aplicativo é habilitada por meio do Painel do Adobe Primetime TVE.
   * O Logon único do provedor de TV do usuário com o aplicativo é ativado por meio do painel do Adobe Primetime TVE.
   * O provedor de TV do usuário não é degradado pelo painel do Adobe Primetime TVE.
   * O SDK iOS/tvOS do AccessEnabler recebeu a resposta SAML do Provedor de TV do usuário da Estrutura de conta do assinante de vídeo.

   **<u>Dica Pro:</u>** Esta segunda etapa não acionará nenhum outro retorno de chamada, além do retorno de chamada [setRequestorComplete](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete), pois a autenticação não foi iniciada explicitamente pelo aplicativo.


1. O aplicativo teria que [verificar o status de autenticação](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkauthentication-checkauthn).

   **Importante:** esta terceira etapa pode disparar um [código de erro avançado](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md) que é específico para o fluxo de trabalho SSO do Apple, no caso de **uma das seguintes opções ser verdadeira**:

   * ***VSA403** - O usuário está conectado à sua conta do Provedor de TV em
o nível do sistema do dispositivo, mas a permissão do Provedor de TV do usuário é
negado para o aplicativo.
   * ***VSA404** - O usuário está conectado à sua conta do Provedor de TV em
o nível do sistema do dispositivo, mas a permissão do Provedor de TV do usuário
é indeterminado para o aplicativo.
   * ***APPL\_ERROR** - O usuário está conectado ao seu Provedor de TV
conta a nível do sistema do dispositivo, mas a comunicação entre os
o SDK iOS/tvOS do AccessEnabler e a conta do assinante de vídeo
a estrutura encontrou um erro.

   **Importante:** esta terceira etapa disparará o retorno de chamada [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) com *status* igual a 0, no caso de **uma das seguintes opções ser verdadeira**:

   * O usuário não está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo ou por meio de um fluxo de autenticação regular.
   * O usuário está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo ou por meio de um fluxo de autenticação regular, mas o token de autenticação do Provedor de TV TTL do usuário foi aprovado.
   * O usuário faz logon em sua conta do Provedor de TV no nível do sistema do dispositivo ou por meio de um fluxo de autenticação regular, mas a integração do Provedor de TV do usuário com o aplicativo é desativada por meio do Painel do Adobe Primetime TVE.
   * O usuário faz logon em sua conta do Provedor de TV no nível do sistema do dispositivo, mas o Logon único do Provedor de TV com o aplicativo é desativado por meio do Painel do Adobe Primetime TVE.
   * O usuário está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo, mas a permissão do Provedor de TV do usuário é negada para o aplicativo.
   * O usuário está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo, mas a permissão do Provedor de TV do usuário não foi determinada para o aplicativo.
   * O usuário está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo, mas a comunicação entre o SDK iOS/tvOS do AccessEnabler e a Estrutura da Conta do Assinante de Vídeo encontrou um erro.

   **Importante:** esta terceira etapa disparará o retorno de chamada [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) com *status* igual a 1, no caso de **todas as etapas acima serem falsas.**


1. O aplicativo teria que [inicializar a autenticação](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) caso a verificação de status de autenticação anterior disparasse o retorno de chamada [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) com *status* igual a 0.

   **<u>Dica de Profissional:</u>** Implemente uma das seguintes API de SDK do AccessEnabler iOS/tvOS [getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) ou [getAuthentication:filter](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN_filter).

   **Importante:** esta quarta etapa pode disparar um [código de erro avançado](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md) que é específico para o fluxo de trabalho SSO do Apple, no caso de **uma das seguintes opções ser verdadeira**:

   * ***VSA403*** - Permissão de Provedor de TV do usuário negada para o aplicativo.
   * ***VSA404*** - A permissão Provedor de TV do usuário não foi determinada para o aplicativo.
   * ***VSA503*** - A comunicação entre o SDK iOS/tvOS do AccessEnabler e a Estrutura de Conta de Assinante de Vídeo encontrou um erro.
   * ***N003*** - O usuário selecionou a opção &quot;Outro Provedor de TV&quot; no seletor Apple MVPD.
   * ***N004*** - O usuário selecionou um Provedor de TV no seletor de MVPD do Apple, para o qual não há suporte (integração ou Logon Único desabilitado) pelo solicitante atual.
   * ***N005*** - O usuário decidiu cancelar o seletor de MVPD normal ou o seletor de MVPD do Apple.

   **Importante:** esta quarta etapa retornará ao fluxo de autenticação regular, acionando o retorno de chamada [displayProviderDialog](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#dispProvDialog) e **one** dos [códigos de erro avançados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md) acima, caso **uma das opções acima seja verdadeira**.

   **Importante:** esta quarta etapa retornará ao fluxo de autenticação regular, acionando o [navigateToUrl](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2url) ou o [navigateToUrl:useSVC](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#nav2urlSVC) de retorno e **nenhum** dos [códigos de erro avançados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md) acima, caso o usuário tenha selecionado um provedor de TV, que não dá suporte ao Apple SSO, mas está presente no seletor Apple MVPD.

   **<u>Dica Pro:</u>** o SDK iOS/tvOS do AccessEnabler chama silenciosamente a API [setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv), caso o usuário tenha selecionado um provedor de TV, que não dá suporte ao SSO do Apple, mas está presente no seletor de MVPD do Apple.

   **Importante:** esta quarta etapa tentaria trocar silenciosamente o perfil de SSO do Apple por um token de autenticação Adobe, caso **todas as opções acima sejam falsas** e **todas as opções a seguir sejam verdadeiras**:

   * A permissão Provedor de TV do usuário é concedida para o aplicativo.
   * O usuário está conectado/no momento faz logon em sua conta do Provedor de TV no nível do sistema do dispositivo.
   * O SDK iOS/tvOS do AccessEnabler recebeu o identificador do provedor de TV do usuário da Estrutura da conta do assinante de vídeo.
   * A integração do Provedor de TV do usuário com o aplicativo é habilitada por meio do Painel do Adobe Primetime TVE.
   * O Logon único do provedor de TV do usuário com o aplicativo é ativado por meio do painel do Adobe Primetime TVE.
   * O provedor de TV do usuário não é degradado pelo painel do Adobe Primetime TVE.
   * O SDK iOS/tvOS do AccessEnabler recebeu a resposta SAML do Provedor de TV do usuário da Estrutura de conta do assinante de vídeo.

   **<u>Dica de Profissional:</u>** Esta quarta etapa acionará o retorno de chamada [*setAuthenticationStatus*](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setAuthNStatus), independentemente do resultado de *status*, pois a autenticação foi iniciada explicitamente pelo aplicativo.

### Metadados {#apple-sso-cookbook-iostvos-sdk-metadata}

O aplicativo tem a opção de determinar se a autenticação ocorreu como resultado de uma entrada por meio do SSO do Parceiro ou não, usando a API de &quot;*metadados de [usuário* do &quot;](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta)tokenSource&quot; do SDK iOS/tvOS do AccessEnabler.

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

### Sair {#apple-sso-cookbook-iostvos-sdk-logout}

A [Estrutura de Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) não fornece uma API para desconectar programaticamente as pessoas que entraram na conta do provedor de TV no nível do sistema do dispositivo. Portanto, para que o logout tenha efeito total, o usuário final teria que sair explicitamente do *`Settings -> TV Provider`* no iOS/iPadOS ou do *`Settings -> Accounts -> TV Provider`* no tvOS. A outra opção que o usuário teria é retirar a permissão para acessar as informações de assinatura do usuário na seção de configurações específicas do aplicativo (acesso de permissão ao Provedor de TV).

>[!TIP]
>
> **<u>Dica:</u>** implemente-a por meio da API [logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) do SDK do iOS/tvOS do AccessEnabler.

>[!TIP]
>
> **<u>Dica do profissional:</u>** siga as etapas abaixo para a implementação do tvOS.

* O aplicativo teria que [iniciar o logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) do SDK iOS/tvOS do AccessEnabler. Isso não facilitaria a limpeza da sessão no lado do MVPD.
* O aplicativo teria que instruir/solicitar que o usuário saia explicitamente de *`Settings -> Accounts -> TV Provider`* no tvOS somente no caso de o código de status [*VSA203* ser acionado](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md).

>[!TIP]
>
> **<u>Dica dos profissionais:</u>** Siga as etapas abaixo para a(s) implementação(ões) do iOS/iPadOS.

* O aplicativo teria que [iniciar o logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) do SDK iOS/tvOS do AccessEnabler. Isso facilitaria a limpeza da sessão no lado do MVPD.
* O aplicativo teria que instruir/solicitar que o usuário saia explicitamente de *`Settings -> TV Provider`* no iOS/iPadOS somente se o código de status [*VSA203* for acionado](/help/authentication/integration-guide-programmers/features-standard/error-reporting/error-reporting.md).
