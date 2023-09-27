---
title: Guia do Apple SSO (iOS/tvOS SDK)
description: Guia do Apple SSO (iOS/tvOS SDK)
exl-id: 2d59cd33-ccfd-41a8-9697-1ace3165bc44
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '1867'
ht-degree: 0%

---

# Guia do Apple SSO (iOS/tvOS SDK) {#apple-sso-cookbook-iostvos-sdk}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#Introduction}

O SDK iOS/tvOS do Adobe Primetime Authentication AccessEnabler pode oferecer suporte à autenticação de logon único (SSO) da plataforma para usuários finais de aplicativos clientes em execução no iOS, iPadOS ou tvOS por meio do que chamamos de fluxo de trabalho SSO do Apple.

Observe que este documento atua como uma extensão da documentação existente do SDK do AccessEnabler iOS/tvOS, que pode ser encontrada [aqui](/help/authentication/iostvos-sdk-api-reference.md).

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
> **<u>Dica Profissional:</u>** Recomendamos solicitar a permissão do usuário quando o aplicativo entrar no estado de primeiro plano, mas isso é apenas uma sugestão, pois o aplicativo pode verificar [permissão para acessar](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) as informações de assinatura do usuário a qualquer momento antes da solicitação de autenticação do usuário. Além disso, as APIs do SDK iOS/tvOS do AccessEnabler solicitarão automaticamente a permissão do usuário quando ele precisar.

>[!TIP]
>
> **<u>Dica Profissional:</u>** Caso o usuário não conceda acesso a suas informações de assinatura ou caso a comunicação com a estrutura da conta do assinante de vídeo falhe, o SDK do AccessEnabler iOS/tvOS fará fallback para o fluxo de autenticação regular.

>[!TIP]
>
> **<u>Dica Profissional:</u>** Recomendamos incentivar os usuários que se recusam a dar permissão para acessar informações de assinatura explicando os benefícios da experiência do usuário de Logon único (SSO). Lembre-se de que o usuário pode alterar sua decisão acessando as configurações do aplicativo (acesso de permissão ao Provedor de TV) ou a seção de *`Settings -> TV Provider`* no iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* no tvOS.


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
> **<u>Dica Profissional:</u>** Implemente a seguinte lista de [callbacks](/help/authentication/iostvos-sdk-api-reference.md) que são específicos para o fluxo de trabalho SSO do Apple.

- [*presentTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#presenttvproviderdialog-presenttvdialog) - Retorno de chamada acionado quando o seletor de MVPD do Apple for aberto.
- [*dismissTVProviderDialog*](/help/authentication/iostvos-sdk-api-reference.md#dismisstvproviderdialog-dismisstvdialog) - Retorno de chamada acionado quando o seletor de MVPD do Apple for fechado.

</br>

#### Relatório de erros

>[!TIP]
>
> **<u>Dica Profissional:</u>** Implemente a seguinte lista de [códigos de erro avançados](/help/authentication/error-reporting.md) que são específicos para o fluxo de trabalho SSO do Apple.

- ***N003*** - O usuário selecionou a opção &quot;Outro provedor de TV&quot; no seletor Apple MVPD.
- ***N004*** - O usuário selecionou um Provedor de TV no seletor de MVPD do Apple, que não é compatível (integração ou Logon único desabilitado) pelo solicitante atual.
- ***N005*** - O usuário decidiu cancelar o seletor de MVPD regular ou o seletor de MVPD do Apple.
- ***VSA403*** - A permissão do Provedor de TV do usuário foi negada para o aplicativo.
- ***VSA404*** - A permissão do Provedor de TV do usuário não foi determinada para o aplicativo.
- ***VSA503*** - A solicitação de metadados da conta do assinante de vídeo falhou. Mais contexto é fornecido em *mensagem* campo.
- ***AAPL / APPL_ERROR*** - A solicitação de metadados da conta do assinante de vídeo falhou. Mais contexto é fornecido em *detalhes* campo.

</br>

### Autenticação {#Authentication}

>[!TIP]
>
> **<u>Dica:</u>** Siga as etapas abaixo para as implementações do iOS/iPadOS/tvOS.

1. O aplicativo teria que [inicializar](/help/authentication/iostvos-sdk-api-reference.md#initsoftwarestatement-initwithsoftwarestatement) o SDK AccessEnabler iOS/tvOS.
1. O aplicativo teria que [definir o identificador do solicitante atual](/help/authentication/iostvos-sdk-api-reference.md#setrequestorrequestorid-setrequestorrequestoridserviceproviders-setreqv3).

   **Importante:** Esta segunda etapa pode desencadear uma [código de erro avançado](/help/authentication/error-reporting.md) que é específico para o fluxo de trabalho de SSO do Apple, no caso de **uma das seguintes opções é verdadeira**:

   - ***VSA403*** - A permissão do Provedor de TV do usuário foi negada para o aplicativo.
   - ***VSA404*** - A permissão do Provedor de TV do usuário não foi determinada para o aplicativo.
   - ***APPL*** - A comunicação entre o SDK iOS/tvOS do AccessEnabler e a estrutura da conta do assinante de vídeo encontrou um erro.

   Essa segunda etapa tentaria trocar silenciosamente o perfil de SSO do Apple por um token de autenticação Adobe, caso **todas as opções acima são falsas** e **todos os itens a seguir são verdadeiros**:

   - A permissão Provedor de TV do usuário é concedida para o aplicativo.
   - O usuário está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo.
   - O SDK iOS/tvOS do AccessEnabler recebeu o identificador do provedor de TV do usuário da estrutura da conta do assinante de vídeo.
   - A integração do Provedor de TV do usuário com o aplicativo é habilitada por meio do Painel do Adobe Primetime TVE.
   - O Logon único do provedor de TV do usuário com o aplicativo é ativado por meio do painel do Adobe Primetime TVE.
   - O provedor de TV do usuário não é degradado pelo painel do Adobe Primetime TVE.
   - O SDK iOS/tvOS do AccessEnabler recebeu a resposta SAML do Provedor de TV do usuário da estrutura da Conta do assinante de vídeo.

   **<u>Dica Profissional:</u>** Essa segunda etapa não acionará outros retornos de chamada, além da [setRequestorComplete](/help/authentication/iostvos-sdk-api-reference.md#setrequestorcomplete-setreqcomplete) retorno de chamada, pois a autenticação não foi iniciada explicitamente pelo aplicativo.

1. O aplicativo teria que [verifique o status de autenticação](/help/authentication/iostvos-sdk-api-reference.md#checkauthentication-checkauthn).

   **Importante:** Esta terceira etapa pode desencadear uma [código de erro avançado](/help/authentication/error-reporting.md) que é específico para o fluxo de trabalho de SSO do Apple, no caso de **uma das seguintes opções é verdadeira**:

   - ***VSA403** - O usuário está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo, mas a permissão do Provedor de TV do usuário é negada para o aplicativo.
   - ***VSA404** - O usuário está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo, mas a permissão do Provedor de TV do usuário não foi determinada para o aplicativo.
   - ***APPL\_ERROR** - O usuário está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo, mas a comunicação entre o SDK do AccessEnabler iOS/tvOS e a estrutura da Conta do Assinante de Vídeo encontrou um erro.

   **Importante:** Esta terceira etapa acionará a [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) retorno de chamada com *status* igual a 0, no caso **uma das seguintes opções é verdadeira**:

   - O usuário não está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo ou por meio de um fluxo de autenticação regular.
   - O usuário está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo ou por meio de um fluxo de autenticação regular, mas o token de autenticação do Provedor de TV TTL do usuário foi aprovado.
   - O usuário faz logon em sua conta do Provedor de TV no nível do sistema do dispositivo ou por meio de um fluxo de autenticação regular, mas a integração do Provedor de TV do usuário com o aplicativo é desativada por meio do Painel do Adobe Primetime TVE.
   - O usuário faz logon em sua conta do Provedor de TV no nível do sistema do dispositivo, mas o Logon único do Provedor de TV com o aplicativo é desativado por meio do Painel do Adobe Primetime TVE.
   - O usuário está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo, mas a permissão do Provedor de TV do usuário é negada para o aplicativo.
   - O usuário está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo, mas a permissão do Provedor de TV do usuário não foi determinada para o aplicativo.
   - O usuário está conectado à sua conta do Provedor de TV no nível do sistema do dispositivo, mas a comunicação entre o SDK do AccessEnabler iOS/tvOS e a estrutura da Conta do Assinante de Vídeo encontrou um erro.

   **Importante:** Esta terceira etapa acionará a [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) retorno de chamada com *status* igual a 1, no caso de **todos os itens acima são falsos.**


1. O aplicativo teria que [inicializar a autenticação](/help/authentication/iostvos-sdk-api-reference.md#getauthentication-getauthenticationwithdata-getauthn) caso a verificação anterior do estado de autenticação tenha [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setauthenticationstatuserrorcode-setauthnstatus) retorno de chamada com *status* igual a 0.

   **<u>Dica Profissional:</u>** Implemente uma das seguintes API de SDK do AccessEnabler iOS/tvOS [getAuthentication](/help/authentication/iostvos-sdk-api-reference.md#getAuthN) ou [getAuthentication:filtro](/help/authentication/iostvos-sdk-api-reference.md#getAuthN_filter).

   **Importante:** Esta quarta etapa pode desencadear uma [código de erro avançado](/help/authentication/error-reporting.md) que é específico para o fluxo de trabalho de SSO do Apple, no caso de **uma das seguintes opções é verdadeira**:

   - ***VSA403*** - A permissão do Provedor de TV do usuário foi negada para o aplicativo.
   - ***VSA404*** - A permissão do Provedor de TV do usuário não foi determinada para o aplicativo.
   - ***VSA503*** - A comunicação entre o SDK iOS/tvOS do AccessEnabler e a estrutura da conta do assinante de vídeo encontrou um erro.
   - ***N003*** - O usuário selecionou a opção &quot;Outro provedor de TV&quot; no seletor Apple MVPD.
   - ***N004*** - O usuário selecionou um Provedor de TV no seletor de MVPD do Apple, que não é compatível (integração ou Logon único desabilitado) pelo solicitante atual.
   - ***N005*** - O usuário decidiu cancelar o seletor de MVPD regular ou o seletor de MVPD do Apple.

   **Importante:** Essa quarta etapa pode fazer o fallback para o fluxo de autenticação normal, acionando o [displayProviderDialog](/help/authentication/iostvos-sdk-api-reference.md#dispProvDialog) retorno de chamada e **um** das opções acima [códigos de erro avançados](/help/authentication/error-reporting.md), no caso de **uma das situações acima é verdadeira**.

   **Importante:** Essa quarta etapa pode fazer o fallback para o fluxo de autenticação normal, acionando o [navigateToUrl](/help/authentication/iostvos-sdk-api-reference.md#nav2url) ou [navigateToUrl:useSVC](/help/authentication/iostvos-sdk-api-reference.md#nav2urlSVC) retorno de chamada e **nenhum** das opções acima [códigos de erro avançados](/help/authentication/error-reporting.md), caso o usuário tenha selecionado um provedor de TV, que não é compatível com o Apple SSO, mas está presente no seletor de MVPD do Apple.

   **<u>Dica Profissional:</u>** O SDK iOS/tvOS do AccessEnabler chama silenciosamente a [setSelectedProvider](/help/authentication/iostvos-sdk-api-reference.md#setSelProv) API, caso o usuário tenha selecionado um provedor de TV, que não oferece suporte ao Apple SSO, mas está presente no seletor de MVPD do Apple.

   **Importante:** Essa quarta etapa tentaria trocar silenciosamente o perfil de SSO do Apple por um token de autenticação Adobe, caso **todas as opções acima são falsas** e **todos os itens a seguir são verdadeiros**:

   - A permissão Provedor de TV do usuário é concedida para o aplicativo.
   - O usuário está conectado/no momento faz logon em sua conta do Provedor de TV no nível do sistema do dispositivo.
   - O SDK iOS/tvOS do AccessEnabler recebeu o identificador do provedor de TV do usuário da estrutura da conta do assinante de vídeo.
   - A integração do Provedor de TV do usuário com o aplicativo é habilitada por meio do Painel do Adobe Primetime TVE.
   - O Logon único do provedor de TV do usuário com o aplicativo é ativado por meio do painel do Adobe Primetime TVE.
   - O provedor de TV do usuário não é degradado pelo painel do Adobe Primetime TVE.
   - O SDK iOS/tvOS do AccessEnabler recebeu a resposta SAML do Provedor de TV do usuário da estrutura da Conta do assinante de vídeo.



>**<u>Dica Profissional:</u>** Esta quarta etapa acionará o [*setAuthenticationStatus*](/help/authentication/iostvos-sdk-api-reference.md#setAuthNStatus) retorno de chamada, independentemente de *status* resultado, já que a autenticação foi iniciada explicitamente pelo aplicativo.


</br>

### Metadados {#Metadata}

O aplicativo tem a opção de determinar se a autenticação aconteceu como resultado de um logon por meio do SSO da plataforma, usando o &quot;*tokenSource&quot;* [metadados do usuário](/help/authentication/iostvos-sdk-api-reference.md#getMeta) da API do SDK AccessEnabler iOS/tvOS.

```swift
    ...
    accessEnabler.getMetadata([METADATA_OPCODE_KEY:Int(METADATA_USER_META), METADATA_USER_META_KEY: "tokenSource"])
    ...
```

</br>

### Sair {#Logout}

A variável [Conta do assinante do vídeo](https://developer.apple.com/documentation/videosubscriberaccount) A estrutura não fornece uma API para fazer logoff programaticamente de pessoas que entraram na conta do provedor de TV no nível do sistema do dispositivo. Portanto, para que o logout tenha efeito total, o usuário final teria que sair explicitamente do *`Settings -> TV Provider`* no iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* no tvOS. A outra opção que o usuário teria é retirar a permissão para acessar as informações de assinatura do usuário na seção de configurações específicas do aplicativo (acesso de permissão ao Provedor de TV).

>[!TIP]
>
> **<u>Dica:</u>** Implemente isso por meio do SDK iOS/tvOS do AccessEnabler [logout](/help/authentication/iostvos-sdk-api-reference.md#logout) API.


>[!TIP]
>
> **<u>Dica Profissional:</u>** Siga as etapas abaixo para a implementação do tvOS.

- O aplicativo teria que [iniciar o logout](/help/authentication/iostvos-sdk-api-reference.md#logout) no SDK AccessEnabler iOS/tvOS. Isso não facilitaria a limpeza da sessão no lado do MVPD.
- O aplicativo teria que instruir/solicitar que o usuário se desconecte explicitamente do *`Settings -> Accounts -> TV Provider`* no tvOS somente em caso de [*VSA203* o código de status é acionado](/help/authentication/error-reporting.md).

>[!TIP]
>
> **<u>Dica Profissional:</u>** Siga as etapas abaixo para as implementações do iOS/iPadOS.

- O aplicativo teria que [iniciar o logout](/help/authentication/iostvos-sdk-api-reference.md#logout) no SDK AccessEnabler iOS/tvOS. Isso facilitaria a limpeza da sessão no lado do MVPD.
- O aplicativo teria que instruir/solicitar que o usuário se desconecte explicitamente do *`Settings -> TV Provider`* no iOS/iPadOS somente em caso de [*VSA203* o código de status é acionado](/help/authentication/error-reporting.md).


<!--
## Resources {#Resources}

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [AccessEnabler iOS/tvOS SDK Overview](/help/authentication/iostvos-sdk-overview.md)
- [AccessEnabler iOS/tvOS SDK Cookbook](/help/authentication/iostvos-sdk-cookbook.md)
- [AccessEnabler iOS/tvOS SDK API Reference](/help/authentication/iostvos-sdk-api-reference.md)
- [Error Reporting](/help/authentication/error-reporting.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
