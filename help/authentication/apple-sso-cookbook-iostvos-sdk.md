---
title: Guia do Apple SSO (iOS/tvOS SDK)
description: Guia do Apple SSO (iOS/tvOS SDK)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '1861'
ht-degree: 0%

---

# Guia do Apple SSO (iOS/tvOS SDK) {#apple-sso-cookbook-iostvos-sdk}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#Introduction}

O SDK iOS/tvOS do Adobe Primetime Authentication AccessEnabler pode oferecer suporte à autenticação de logon único (SSO) da plataforma para usuários finais de aplicativos clientes em execução no iOS, iPadOS ou tvOS por meio do que chamamos de fluxo de trabalho SSO do Apple.

Observe que este documento atua como uma extensão da documentação existente do SDK iOS/tvOS do AccessEnabler, que pode ser encontrada [aqui](/help/authentication/iostvos-sdk-api-reference.md).

</br>

## Guia {#Cookbook}

Para se beneficiar da experiência do usuário do Apple SSO, um aplicativo precisaria integrar o SDK do AccessEnabler iOS/tvOS e seguir a sequência de dicas apresentada abaixo.

</br>

### Pré-requisitos {#Prerequisites}

</br>

#### Permissão

>[!TIP]
>
> **<u>Dica Profissional:</u>** Para ter acesso às informações de assinatura do usuário, o usuário deve dar permissão ao aplicativo para continuar, de modo semelhante a fornecer acesso à câmera ou ao microfone do dispositivo. Essa permissão deve ser solicitada por aplicativo e o dispositivo salvará a seleção do usuário. Lembre-se de que o usuário pode alterar sua decisão acessando as configurações do aplicativo (acesso de permissão ao Provedor de TV) ou a seção de *`Settings -> TV Provider`* no iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* no tvOS.

>[!TIP]
>
> **<u>Dica Profissional:</u>** recomendamos solicitar a permissão do usuário quando o aplicativo entrar no estado de primeiro plano, mas isso é apenas uma sugestão, pois o aplicativo pode verificar a [permissão para acessar](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) as informações de assinatura do usuário a qualquer momento antes de exigir autenticação do usuário. Além disso, as APIs do SDK iOS/tvOS do AccessEnabler solicitarão automaticamente a permissão do usuário quando ele precisar.

>[!TIP]
>
> **<u>Dica Pro:</u>** caso o usuário não conceda acesso às suas informações de assinatura ou caso a comunicação com a estrutura de Conta de Assinante de Vídeo falhe, o SDK iOS/tvOS AccessEnabler fará fallback para o fluxo de autenticação regular.

>[!TIP]
>
> **<u>Dica Profissional:</u>** Recomendamos incentivar os usuários que se recusam a conceder permissão de acesso às informações de assinatura explicando os benefícios da experiência do usuário de Logon Único (SSO). Lembre-se de que o usuário pode alterar sua decisão acessando as configurações do aplicativo (acesso de permissão ao Provedor de TV) ou a seção de *`Settings -> TV Provider`* no iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* no tvOS.


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

</br>

#### Retornos de chamada

>[!TIP]
>
> **<u>Dica Profissional:</u>** Implemente a seguinte lista de [retornos de chamada](/help/authentication/iostvos-sdk-api-reference.md) que são específicos para o fluxo de trabalho SSO do Apple.

- [*presentTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Retorno de chamada disparado quando o seletor de MVPD do Apple será aberto.
- [*dismissTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Retorno de chamada disparado quando o seletor de MVPD do Apple vai fechar.

</br>

#### Relatório de erros

>[!TIP]
>
> **<u>Dica Profissional:</u>** Implemente a seguinte lista de [códigos de erro avançados](/help/authentication/error-reporting.md) que são específicos para o fluxo de trabalho SSO do Apple.

- ***N003*** - O usuário selecionou a opção &quot;Outro Provedor de TV&quot; no seletor Apple MVPD.
- ***N004*** - O usuário selecionou um Provedor de TV no seletor de MVPD do Apple, para o qual não há suporte (integração ou Logon Único desabilitado) pelo solicitante atual.
- ***N005*** - O usuário decidiu cancelar o seletor de MVPD normal ou o seletor de MVPD do Apple.
- ***VSA403*** - Permissão de Provedor de TV do usuário negada para o aplicativo.
- ***VSA404*** - A permissão Provedor de TV do usuário não foi determinada para o aplicativo.
- ***VSA503*** - Falha na solicitação de metadados da Conta do Assinante do Vídeo. Mais contexto é fornecido no campo *message*.
- ***AAPL / APPL_ERROR*** - A solicitação de metadados da Conta do Assinante do Vídeo falhou, mais contexto é fornecido no campo *details*.

</br>

### Autenticação {#Authentication}

>[!TIP]
>
> **<u>Dica:</u>** siga as etapas abaixo para as implementações do iOS/iPadOS/tvOS.

1. O aplicativo teria que [inicializar](/help/authentication/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) o SDK iOS/tvOS do AccessEnabler.
1. O aplicativo teria que [definir o identificador do solicitante atual](/help/authentication/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3).

   **Importante:** esta segunda etapa pode disparar um [código de erro avançado](/help/authentication/error-reporting.md) que é específico para o fluxo de trabalho SSO do Apple, no caso de **uma das seguintes opções ser verdadeira**:

   - ***VSA403*** - Permissão de Provedor de TV do usuário negada para o aplicativo.
   - ***VSA404*** - A permissão Provedor de TV do usuário não foi determinada para o aplicativo.
   - ***APPL*** - A comunicação entre o SDK iOS/tvOS do AccessEnabler e a estrutura da Conta do Assinante de Vídeo encontrou um erro.

   Esta segunda etapa tentaria trocar silenciosamente o perfil de SSO do Apple por um token de autenticação Adobe, caso **todas as opções acima sejam falsas** e **todas as opções a seguir sejam verdadeiras**:

   - A permissão Provedor de TV do usuário é concedida para o aplicativo.
   - O usuário está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo.
   - O SDK iOS/tvOS do AccessEnabler recebeu o identificador do provedor de TV do usuário da estrutura da conta do assinante de vídeo.
   - A integração do Provedor de TV do usuário com o aplicativo é habilitada por meio do Painel do Adobe Primetime TVE.
   - O Logon único do provedor de TV do usuário com o aplicativo é ativado por meio do painel do Adobe Primetime TVE.
   - O provedor de TV do usuário não é degradado pelo painel do Adobe Primetime TVE.
   - O SDK iOS/tvOS do AccessEnabler recebeu a resposta SAML do Provedor de TV do usuário da estrutura da Conta do assinante de vídeo.

   **<u>Dica Pro:</u>** Esta segunda etapa não acionará nenhum outro retorno de chamada, além do retorno de chamada [setRequestorComplete](/help/authentication/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete), pois a autenticação não foi iniciada explicitamente pelo aplicativo.

1. O aplicativo teria que [verificar o status de autenticação](/help/authentication/iostvos-sdk-api-reference.md#checkauthentication-checkauthn).

   **Importante:** esta terceira etapa pode disparar um [código de erro avançado](/help/authentication/error-reporting.md) que é específico para o fluxo de trabalho SSO do Apple, no caso de **uma das seguintes opções ser verdadeira**:

   - ***VSA403** - O usuário está conectado à sua conta do Provedor de TV em
o nível do sistema do dispositivo, mas a permissão do Provedor de TV do usuário é
negado para o aplicativo.
   - ***VSA404** - O usuário está conectado à sua conta do Provedor de TV em
o nível do sistema do dispositivo, mas a permissão do Provedor de TV do usuário
é indeterminado para o aplicativo.
   - ***APPL\_ERROR** - O usuário está conectado ao seu Provedor de TV
conta a nível do sistema do dispositivo, mas a comunicação entre os
o SDK iOS/tvOS do AccessEnabler e a conta do assinante de vídeo
a estrutura encontrou um erro.

   **Importante:** esta terceira etapa disparará o retorno de chamada [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) com *status* igual a 0, no caso de **uma das seguintes opções ser verdadeira**:

   - O usuário não está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo ou por meio de um fluxo de autenticação regular.
   - O usuário está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo ou por meio de um fluxo de autenticação regular, mas o token de autenticação do Provedor de TV TTL do usuário foi aprovado.
   - O usuário faz logon em sua conta do Provedor de TV no nível do sistema do dispositivo ou por meio de um fluxo de autenticação regular, mas a integração do Provedor de TV do usuário com o aplicativo é desativada por meio do Painel do Adobe Primetime TVE.
   - O usuário faz logon em sua conta do Provedor de TV no nível do sistema do dispositivo, mas o Logon único do Provedor de TV com o aplicativo é desativado por meio do Painel do Adobe Primetime TVE.
   - O usuário está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo, mas a permissão do Provedor de TV do usuário é negada para o aplicativo.
   - O usuário está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo, mas a permissão do Provedor de TV do usuário não foi determinada para o aplicativo.
   - O usuário está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo, mas a comunicação entre o SDK do AccessEnabler iOS/tvOS e a estrutura da Conta do Assinante de Vídeo encontrou um erro.

   **Importante:** esta terceira etapa disparará o retorno de chamada [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) com *status* igual a 1, no caso de **todas as etapas acima serem falsas.**


1. O aplicativo teria que [inicializar a autenticação](/help/authentication/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) caso a verificação de status de autenticação anterior disparasse o retorno de chamada [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) com *status* igual a 0.

   **<u>Dica de Profissional:</u>** Implemente uma das seguintes API de SDK do AccessEnabler iOS/tvOS [getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) ou [getAuthentication:filter](/help/authentication/iostvos-sdk-api-reference.md#getAuthN_filter).

   **Importante:** esta quarta etapa pode disparar um [código de erro avançado](/help/authentication/error-reporting.md) que é específico para o fluxo de trabalho SSO do Apple, no caso de **uma das seguintes opções ser verdadeira**:

   - ***VSA403*** - Permissão de Provedor de TV do usuário negada para o aplicativo.
   - ***VSA404*** - A permissão Provedor de TV do usuário não foi determinada para o aplicativo.
   - ***VSA503*** - A comunicação entre o SDK iOS/tvOS do AccessEnabler e a estrutura da Conta do Assinante de Vídeo encontrou um erro.
   - ***N003*** - O usuário selecionou a opção &quot;Outro Provedor de TV&quot; no seletor Apple MVPD.
   - ***N004*** - O usuário selecionou um Provedor de TV no seletor de MVPD do Apple, para o qual não há suporte (integração ou Logon Único desabilitado) pelo solicitante atual.
   - ***N005*** - O usuário decidiu cancelar o seletor de MVPD normal ou o seletor de MVPD do Apple.

   **Importante:** esta quarta etapa pode reverter para o fluxo de autenticação regular, disparando o retorno de chamada [displayProviderDialog](/help/authentication/iostvos-sdk-api-reference.md#dispProvDialog) e **one** dos [códigos de erro avançados](/help/authentication/error-reporting.md) acima, caso **uma das opções acima seja verdadeira**.

   **Importante:** esta quarta etapa pode reverter para o fluxo de autenticação regular, acionando o [navigateToUrl](/help/authentication/iostvos-sdk-api-reference.md#nav2url) ou o [navigateToUrl:useSVC](/help/authentication/iostvos-sdk-api-reference.md#nav2urlSVC) de retorno de chamada e **nenhum** dos [códigos de erro avançados](/help/authentication/error-reporting.md) acima, caso o usuário tenha selecionado um provedor de TV, que não dá suporte ao Apple SSO, mas está presente no seletor de MVPD do Apple.

   **<u>Dica Pro:</u>** o SDK iOS/tvOS do AccessEnabler chama silenciosamente a API [setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv), caso o usuário tenha selecionado um provedor de TV, que não dá suporte ao SSO do Apple, mas está presente no seletor de MVPD do Apple.

   **Importante:** esta quarta etapa tentaria trocar silenciosamente o perfil de SSO do Apple por um token de autenticação Adobe, caso **todas as opções acima sejam falsas** e **todas as opções a seguir sejam verdadeiras**:

   - A permissão Provedor de TV do usuário é concedida para o aplicativo.
   - O usuário está conectado/no momento faz logon em sua conta do Provedor de TV no nível do sistema do dispositivo.
   - O SDK iOS/tvOS do AccessEnabler recebeu o identificador do provedor de TV do usuário da estrutura da conta do assinante de vídeo.
   - A integração do Provedor de TV do usuário com o aplicativo é habilitada por meio do Painel do Adobe Primetime TVE.
   - O Logon único do provedor de TV do usuário com o aplicativo é ativado por meio do painel do Adobe Primetime TVE.
   - O provedor de TV do usuário não é degradado pelo painel do Adobe Primetime TVE.
   - O SDK iOS/tvOS do AccessEnabler recebeu a resposta SAML do Provedor de TV do usuário da estrutura da Conta do assinante de vídeo.



>**<u>Dica de Profissional:</u>** Esta quarta etapa acionará o retorno de chamada [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setAuthNStatus), independentemente do resultado de *status*, pois a autenticação foi iniciada explicitamente pelo aplicativo.


</br>

### Metadados {#Metadata}

O aplicativo tem a opção de determinar se a autenticação ocorreu como resultado de uma entrada através do SSO da plataforma, usando a API de &quot;*tokenSource&quot;* [metadados de usuário](/help/authentication/iostvos-sdk-api-reference.md#getMeta) do SDK iOS/tvOS do AccessEnabler.

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

</br>

### Sair {#Logout}

A estrutura da [Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) não fornece uma API para fazer logoff programaticamente de pessoas que entraram na conta do provedor de TV no nível do sistema do dispositivo. Portanto, para que o logout tenha efeito total, o usuário final teria que sair explicitamente do *`Settings -> TV Provider`* no iOS/iPadOS ou do *`Settings -> Accounts -> TV Provider`* no tvOS. A outra opção que o usuário teria é retirar a permissão para acessar as informações de assinatura do usuário na seção de configurações específicas do aplicativo (acesso de permissão ao Provedor de TV).

>[!TIP]
>
> **<u>Dica:</u>** implemente-a por meio da API [logout](/help/authentication/iostvos-sdk-api-reference.md#logout) do SDK do iOS/tvOS do AccessEnabler.


>[!TIP]
>
> **<u>Dica do profissional:</u>** siga as etapas abaixo para a implementação do tvOS.

- O aplicativo teria que [iniciar o logout](/help/authentication/iostvos-sdk-api-reference.md#logout) do SDK iOS/tvOS do AccessEnabler. Isso não facilitaria a limpeza da sessão no lado do MVPD.
- O aplicativo teria que instruir/solicitar que o usuário saia explicitamente de *`Settings -> Accounts -> TV Provider`* no tvOS somente no caso de o código de status [*VSA203* ser acionado](/help/authentication/error-reporting.md).

>[!TIP]
>
> **<u>Dica dos profissionais:</u>** Siga as etapas abaixo para a(s) implementação(ões) do iOS/iPadOS.

- O aplicativo teria que [iniciar o logout](/help/authentication/iostvos-sdk-api-reference.md#logout) do SDK iOS/tvOS do AccessEnabler. Isso facilitaria a limpeza da sessão no lado do MVPD.
- O aplicativo teria que instruir/solicitar que o usuário saia explicitamente de *`Settings -> TV Provider`* no iOS/iPadOS somente se o código de status [*VSA203* for acionado](/help/authentication/error-reporting.md).


<!--
## Resources {#Resources}

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [AccessEnabler iOS/tvOS SDK Overview](/help/authentication/iostvos-sdk-overview.md)
- [AccessEnabler iOS/tvOS SDK Cookbook](/help/authentication/iostvos-sdk-cookbook.md)
- [AccessEnabler iOS/tvOS SDK API Reference](/help/authentication/iostvos-sdk-api-reference.md)
- [Error Reporting](/help/authentication/error-reporting.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
