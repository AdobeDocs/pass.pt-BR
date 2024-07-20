---
title: Guia do Apple SSO (REST API)
description: Guia do Apple SSO (REST API)
exl-id: cb27c4b7-bdb4-44a3-8f84-c522a953426f
source-git-commit: 1b8371a314488335c68c82882c930b7c19aa64ad
workflow-type: tm+mt
source-wordcount: '1344'
ht-degree: 0%

---

# Guia do Apple SSO (REST API) {#apple-sso-cookbook-rest-api}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#Introduction}

A API REST de autenticação da Adobe Pass pode oferecer suporte à autenticação de logon único (SSO) da plataforma para usuários finais de aplicativos clientes em execução no iOS, iPadOS ou tvOS por meio do que chamamos de fluxo de trabalho SSO do Apple.

Observe que este documento atua como uma extensão à documentação da API REST existente, que pode ser encontrada [aqui](/help/authentication/rest-api-reference.md).

## Cookbooks {#Cookbooks}

Para se beneficiar da experiência do usuário do Apple SSO, um aplicativo precisaria integrar a estrutura [Conta de assinante de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) desenvolvida pela Apple, enquanto em relação à comunicação da API REST de autenticação da Adobe Pass, ela precisaria seguir a sequência de dicas apresentada abaixo.

### Autenticação {#Authentication}

- [Há um token de autenticação de Adobe válido?](#Is_there_a_valid_Adobe_authentication_token)
- [O usuário está conectado por meio do Platform SSO?](#Is_the_user_logged_in_via_Platform_SSO)
- [Buscar configuração de Adobe](#Fetch_Adobe_configuration)
- [Iniciar fluxo de trabalho SSO da plataforma com configuração Adobe](#Initiate_Platform_SSO_workflow_with_Adobe_config)
- [O logon do usuário foi bem-sucedido?](#Is_user_login_successful)
- [Obter uma solicitação de perfil do Adobe para o MVPD selecionado](#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD)
- [Encaminhe a solicitação de Adobe para o SSO da Platform para obter o perfil](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile)
- [Troque o perfil SSO da Platform por um token de autenticação de Adobe](#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token)
- [O token de Adobe foi gerado com sucesso?](#Is_Adobe_token_generated_successfully)
- [Iniciar fluxo de trabalho de autenticação da segunda tela](#Initiate_second_screen_authentication_workflow)
- [Continuar com fluxos de autorização](#Proceed_with_authorization_flows)


![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/qu/platform-sso.jpeg)


#### Etapa: &quot;Há um token de autenticação de Adobe válido?&quot; {#Is_there_a_valid_Adobe_authentication_token}

>[!TIP]
>
> **<u>Dica:</u>** implemente isso por meio do serviço de [Autenticação da Adobe Pass](/help/authentication/check-authentication-token.md).


#### Etapa: &quot;O usuário está conectado por meio do SSO da Platform?&quot; {#Is_the_user_logged_in_via_Platform_SSO}

>[!TIP]
>
> **<u>Dica:</u>** faça a implementação por meio da estrutura de [Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount).

- O aplicativo teria que verificar a [permissão para acessar](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) as informações de assinatura do usuário e continuar somente se o usuário permitisse.
- O aplicativo teria que enviar uma [solicitação](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) para obter informações sobre a conta do assinante.
- O aplicativo teria que aguardar e processar as informações de [metadados](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).



>[!TIP]
>
> **<u>Dica Profissional:</u>** Siga o trecho de código e preste mais atenção aos comentários.

```swift
...
let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();

videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
            switch (accessStatus) {
            // The user allows the application to access subscription information.
            case VSAccountAccessStatus.granted:
                    // Construct the request for subscriber account information.
                    let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();

                    // This is actually the SAML Issuer not the channel ID.
                    vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAccountProviderIdentifier = true;
                    
                    // This is the subscription account information needed at this step.
                    vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                    
                    // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
                    vsaMetadataRequest.isInterruptionAllowed = false;
                    
                    // Submit the request for subscriber account information - accountProviderIdentifier.
                    videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in        
                        if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                            // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                            // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                            ...
                            // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                            // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                            ...
                            // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                            // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                            ...
                            // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                            ...
                            // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Fetch Adobe configuration" step.
                            ...
                        }
                    }
        
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Fallback to regular authentication workflow.
                ...
            }
}
...  
```

#### Etapa: &quot;Buscar configuração de Adobe&quot; {#Fetch_Adobe_configuration}

>[!TIP]
>
> **<u>Dica:</u>** implemente isso por meio do serviço de [Autenticação da Adobe Pass](/help/authentication/provide-mvpd-list.md).


>[!TIP]
>
> **<u>Dica Profissional:</u>** esteja ciente das propriedades MVPD: *`enablePlatformServices`*, *`boardingStatus`*, *`displayInPlatformPicker`*, *`platformMappingId`*, *`requiredMetadataFields`* e preste atenção extra aos comentários apresentados em trechos de código de outras etapas.

#### Etapa &quot;Iniciar fluxo de trabalho de SSO da plataforma com configuração Adobe&quot; {#Initiate_Platform_SSO_workflow_with_Adobe_config}

>[!TIP]
>
> **<u>Dica:</u>** faça a implementação por meio da estrutura de [Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount).

- O aplicativo teria que verificar a [permissão para acessar](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) as informações de assinatura do usuário e continuar somente se o usuário permitisse.
- O aplicativo teria que fornecer um [delegate](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) para o VSAccountManager.
- O aplicativo teria que enviar uma [solicitação](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) para obter informações sobre a conta do assinante.
- O aplicativo teria que aguardar e processar as informações de [metadados](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).



>[!TIP]
>
> **<u>Dica Profissional:</u>** Siga o trecho de código e preste mais atenção aos comentários.


```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    // This must be a class implementing the VSAccountManagerDelegate protocol.
    let videoSubscriberAccountManagerDelegate: VideoSubscriberAccountManagerDelegate = VideoSubscriberAccountManagerDelegate();
    
    videoSubscriberAccountManager.delegate = videoSubscriberAccountManagerDelegate;
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is the subscription account information needed at this step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account framework to prompt the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = true;
                        
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to filter the TV providers from the Apple picker.
                        vsaMetadataRequest.supportedAccountProviderIdentifiers = supportedAccountProviderIdentifiers;
    
                        // This can be computed from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service response in order to sort the TV providers from the Apple picker.
                        if #available(iOS 11.0, tvOS 11, *) {
                            vsaMetadataRequest.featuredAccountProviderIdentifiers = featuredAccountProviderIdentifiers;
                        }
                        
                        // Submit the request for subscriber account information - accountProviderIdentifier.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in                        
                            // This represents the checks for the "Is user login successful?" step.
                            if (vsaMetadata != nil && vsaMetadata!.accountProviderIdentifier != nil) {
                                // The vsaMetadata!.authenticationExpirationDate will contain the expiration date for current authentication session.
                                // The vsaMetadata!.authenticationExpirationDate should be compared against current date.
                                ...
                                // The vsaMetadata!.accountProviderIdentifier will contain the provider identifier as it is known for the platform configuration.
                                // The vsaMetadata!.accountProviderIdentifier represents the platformMappingId in terms of Adobe Pass Authentication configuration.
                                ...
                                // The application must determine the MVPD id property value based on the platformMappingId property value obtained above.
                                // The application must use the MVPD id further in its communication with Adobe Pass Authentication services.
                                ...
                                // Continue with the "Obtain a profile request from Adobe for the selected MVPD" step.
                                ...
                                // Continue with the "Forward the Adobe request to Platform SSO to obtain the profile" step.
                                ...
                            } else {
                                // The user is not authenticated at platform level.
                                if (vsaError != nil) {
                                    // The application can check to see if the user selected a provider which is present in Apple picker, but the provider is not onboarded in platform SSO.
                                    if let error: NSError = (vsaError! as NSError), error.code == 1, let appleMsoId = error.userInfo["VSErrorInfoKeyUnsupportedProviderIdentifier"] as! String? {
                                        var mvpd: Mvpd? = nil;
    
                                        // The requestor.mvpds must be computed during the "Fetch Adobe configuration" step. 
                                        for provider in requestor.mvpds {
                                            if provider.platformMappingId == appleMsoId {
                                                mvpd = provider;
                                                break;
                                            }
                                        }
                                        
                                        if mvpd != nil {
                                            // Continue with the "Initiate second screen authentcation workflow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Initiate second screen authentcation workflow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Initiate second screen authentcation workflow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Initiate second screen authentcation workflow" step.
                                    ...
                                }
                            }
                        }
            
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### Etapa: &quot;O logon do usuário foi bem-sucedido?&quot; {#Is_user_login_successful}

>[!TIP]
>
> **<u>Dica Profissional:</u>** esteja ciente do trecho de código da etapa [&quot;Iniciar fluxo de trabalho SSO da plataforma com configuração Adobe&quot;](#Initiate_Platform_SSO_workflow_with_Adobe_config). O logon do usuário é bem-sucedido caso o *`vsaMetadata!.accountProviderIdentifier`* contenha um valor válido e a data atual não tenha passado o valor *`vsaMetadata!.authenticationExpirationDate`*.

#### Etapa &quot;Obter uma solicitação de perfil do Adobe para o MVPD selecionado&quot; {#Obtain_a_profile_request_from_Adobe_for_the_selected_MVPD}

>[!TIP]
>
> **<u>Dica:</u>** implemente isso por meio do serviço [Solicitação de perfil](/help/authentication/retrieve-profilerequest.md) da Autenticação do Adobe Pass.

>[!TIP]
>
> **<u>Dica de Profissional:</u>** esteja ciente de que o identificador do provedor obtido da estrutura da Conta do Assinante do Vídeo representa o *`platformMappingId`* em termos de configuração de Autenticação do Adobe Pass. Portanto, o aplicativo deve determinar o valor da propriedade de ID MVPD, usando o valor *`platformMappingId`*, por meio do serviço de Autenticação Adobe Pass [Fornecer Lista MVPD](/help/authentication/provide-mvpd-list.md).

#### Etapa: &quot;Encaminhe a solicitação de Adobe para o SSO da plataforma para obter o perfil&quot; {#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile}

>[!TIP]
>
> **<u>Dica:</u>** faça a implementação por meio da estrutura de [Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount).


- O aplicativo teria que verificar a [permissão para acessar](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) as informações de assinatura do usuário e continuar somente se o usuário permitisse.
- O aplicativo teria que enviar uma [solicitação](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) para obter informações sobre a conta do assinante.
- O aplicativo teria que aguardar e processar as informações de [metadados](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).



>[!TIP]
>
> **<u>Dica Profissional:</u>** Siga o trecho de código e preste mais atenção aos comentários.

```swift
    ...
    let videoSubscriberAccountManager: VSAccountManager = VSAccountManager();
    
    videoSubscriberAccountManager.checkAccessStatus(options: [VSCheckAccessOption.prompt: true]) { (accessStatus, error) -> Void in
                switch (accessStatus) {
                // The user allows the application to access subscription information.
                case VSAccountAccessStatus.granted:
                        // Construct the request for subscriber account information.
                        let vsaMetadataRequest: VSAccountMetadataRequest = VSAccountMetadataRequest();
    
                        // This is actually the SAML Issuer not the channel ID.
                        vsaMetadataRequest.channelIdentifier = "https://saml.sp.auth.adobe.com";
        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAccountProviderIdentifier = true;
                        
                        // This is going to include subscription account information which should match the provider determined in a previous step.
                        vsaMetadataRequest.includeAuthenticationExpirationDate = true;
                        
                        // This is going to make the Video Subscriber Account framework to refrain from prompting the user with the providers picker at this step. 
                        vsaMetadataRequest.isInterruptionAllowed = false;
    
                        // This are the user metadata fields expected to be available on a successful login and are determined from the [Adobe Pass Authentication](/help/authentication/provide-mvpd-list.md) service. Look for the requiredMetadataFields associated with the provider determined in a previous step.
                        vsaMetadataRequest.attributeNames = requiredMetadataFields;
    
                        // This is the payload from [Adobe Pass Authentication](/help/authentication/retrieve-profilerequest.md) service.
                        vsaMetadataRequest.verificationToken = profileRequestPayload;
                        
                        // Submit the request for subscriber account information.
                        videoSubscriberAccountManager.enqueue(vsaMetadataRequest) { vsaMetadata, vsaError in
                            if (vsaMetadata != nil && vsaMetadata!.samlAttributeQueryResponse != nil) {
                                var samlResponse: String? = vsaMetadata!.samlAttributeQueryResponse!;
                                
                                // Remove new lines, new tabs and spaces.
                                samlResponse = samlResponse?.replacingOccurrences(of: "[ \\t]+", with: " ", options: String.CompareOptions.regularExpression);
                                samlResponse = samlResponse?.components(separatedBy: CharacterSet.newlines).joined(separator: "");
                                samlResponse = samlResponse?.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines);
                                
                                // Base64 encode.
                                samlResponse = samlResponse?.data(using: .utf8)?.base64EncodedString(options: []);
                                
                                // URL encode. Please be aware not to double URL encode it further.
                                samlResponse = samlResponse?.addingPercentEncoding(withAllowedCharacters: CharacterSet.init(charactersIn: "!*'();:@&=+$,/?%#[]").inverted);
                                
                                // Continue with the "Exchange the Platform SSO profile for an Adobe authentication token" step.
                                ...
                            } else {
                                // Fallback to regular authentication workflow.
                                ...
                            }
                        }
                        
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Fallback to regular authentication workflow.
                    ...
                }
    }
    ...
```

</br>

#### Etapa: &quot;Trocar o perfil SSO da Platform por um token de autenticação de Adobe&quot; {#Exchange_the_Platform_SSO_profile_for_an_Adobe_authentication_token}

>[!TIP]
>
> **<u>Dica:</u>** implemente isso por meio do serviço [Token Exchange](/help/authentication/token-exchange.md) de Autenticação do Adobe Pass.


>[!TIP]
>
> **<u>Dica Profissional:</u>** esteja ciente do trecho de código da etapa [&quot;Encaminhar a solicitação de Adobe para o SSO da Plataforma para obter o perfil&quot;](#Forward_the_Adobe_request_to_Platform_SSO_to_obtain_the_profile). Este *`vsaMetadata!.samlAttributeQueryResponse!`* representa o *`SAMLResponse`*, que precisa ser passado no [Token Exchange](/help/authentication/token-exchange.md) e requer manipulação e codificação de cadeia de caracteres (*Base64* codificada e *URL* codificada posteriormente) antes de fazer a chamada.

</br>

#### Etapa: &quot;O token Adobe foi gerado com êxito?&quot; {#Is_Adobe_token_generated_successfully}

>[!TIP]
>
> **<u>Dica:</u>** implemente isso por meio da resposta bem-sucedida da autenticação de mídia do Adobe Pass [Token Exchange](/help/authentication/token-exchange.md), que será um *`204 No Content`*, indicando que o token foi criado com êxito e está pronto para ser usado para os fluxos de autorização.

</br>

#### Etapa: &quot;Iniciar fluxo de trabalho de autenticação da segunda tela&quot; {#Initiate_second_screen_authentication_workflow}

**Importante:** a terminologia &quot;Fluxo de trabalho de autenticação da segunda tela&quot; é adequada para AppleTVs, enquanto a terminologia &quot;Fluxo de trabalho de autenticação da primeira tela&quot; / &quot;Fluxo de trabalho de autenticação regular&quot; seria mais apropriada para iPhones e iPads.


>[!TIP]
>
> **<u>Dica:</u>** implemente isso através da mídia de Autenticação Adobe Pass

[Solicitação de Código de Registro](/help/authentication/registration-code-request.md), [Iniciar Autenticação](/help/authentication/initiate-authentication.md) e [Serviços de Token de Autenticação de Recuperação da API REST](/help/authentication/retrieve-authentication-token.md) ou [Verificar Token de Autenticação](/help/authentication/check-authentication-token.md).


>[!TIP]
>
> **<u>Dica do profissional:</u>** siga as etapas abaixo para a implementação do tvOS.

- O aplicativo teria que [obter um código de registro](/help/authentication/registration-code-request.md) e apresentá-lo ao usuário final no primeiro dispositivo (tela).
- O aplicativo teria que iniciar a [sondagem para reconhecer o estado de autenticação](/help/authentication/retrieve-authentication-token.md) no primeiro dispositivo (tela) após a obtenção do código de registro.
- Outro aplicativo teria que [iniciar a autenticação](/help/authentication/initiate-authentication.md) em um segundo dispositivo (tela) quando o código de registro fosse usado.
- O aplicativo teria que parar a [sondagem](/help/authentication/retrieve-authentication-token.md) no primeiro dispositivo (tela) quando o token de autenticação fosse gerado.



>[!TIP]
>
> **<u>Dica dos profissionais:</u>** Siga as etapas abaixo para a(s) implementação(ões) do iOS/iPadOS.

- O aplicativo teria que [obter um código de registro](/help/authentication/registration-code-request.md), que não deve ser apresentado ao usuário final no primeiro dispositivo (tela).
- O aplicativo teria que [iniciar a autenticação](/help/authentication/initiate-authentication.md) no primeiro dispositivo (tela) usando o código de registro e um [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) ou um componente [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller).
- O aplicativo teria que iniciar a [sondagem para conhecer o estado de autenticação](/help/authentication/retrieve-authentication-token.md) no primeiro dispositivo (tela) após o fechamento do [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) ou do [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller).
- O aplicativo teria que parar a [sondagem](/help/authentication/retrieve-authentication-token.md) no primeiro dispositivo (tela) quando o token de autenticação fosse gerado.

</br>

#### Etapa: &quot;Continuar com os fluxos de autorização&quot; {#Proceed_with_authorization_flows}

>[!TIP]
>
> **<u>Dica:</u>** implemente isso por meio da mídia de Autenticação Adobe Pass [Iniciar Autorização](/help/authentication/initiate-authorization.md) e [Obter serviços de Token de Mídia Curta](/help/authentication/obtain-short-media-token.md).

</br>

### Sair {#Logout}

A estrutura da [Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) não fornece uma API para fazer logoff programaticamente de pessoas que entraram na conta do provedor de TV no nível do sistema do dispositivo. Portanto, para que o logout tenha efeito total, o usuário final teria que sair explicitamente do *`Settings -> TV Provider`* no iOS/iPadOS ou do *`Settings -> Accounts -> TV Provider`* no tvOS. A outra opção que o usuário teria é retirar a permissão para acessar as informações de assinatura do usuário na seção de configurações específicas do aplicativo (acesso ao Provedor de TV).

>[!TIP]
>
> **<u>Dica:</u>** implemente isso por meio da mídia de Autenticação Adobe Pass [Chamada de Metadados de Usuário](/help/authentication/user-metadata.md) e serviços de [Logout](/help/authentication/initiate-logout.md).


>[!TIP]
>
> **<u>Dica do profissional:</u>** siga as etapas abaixo para a implementação do tvOS.


- O aplicativo teria que determinar se a autenticação ocorreu como resultado de uma entrada através do SSO da plataforma, usando os &quot;*metadados de usuário](/help/authentication/user-metadata.md) do [tokenSource&quot;* do serviço de Autenticação do Adobe Pass.
- O aplicativo teria que instruir/solicitar que o usuário saia explicitamente de *`Settings -> Accounts -> TV Provider`* no tvOS **only** caso o valor *&quot;tokenSource&quot;* fosse igual a &quot;*Apple&quot;.*
- O aplicativo teria que [iniciar o logout](/help/authentication/initiate-logout.md) do serviço de Autenticação do Adobe Pass usando uma chamada HTTP direta. Isso não facilitaria a limpeza da sessão no lado do MVPD.



>[!TIP]
>
> **<u>Dica dos profissionais:</u>** Siga as etapas abaixo para a(s) implementação(ões) do iOS/iPadOS.

- O aplicativo teria que determinar se a autenticação ocorreu como resultado de uma entrada através do SSO da plataforma, usando os &quot;*metadados de usuário](/help/authentication/user-metadata.md) do [tokenSource&quot;* do serviço de Autenticação do Adobe Pass.
- O aplicativo teria que instruir/solicitar que o usuário saia explicitamente de *`Settings -> TV Provider`* no iOS/iPadOS **only** caso o valor *&quot;tokenSource&quot;* fosse igual a *&quot;Apple&quot;*.
- O aplicativo teria que [iniciar o logout](/help/authentication/initiate-logout.md) do serviço de Autenticação do Adobe Pass usando um [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) ou um componente [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller). Isso facilitaria a limpeza da sessão no lado do MVPD.

<!--

## Resources

- [Apple SSO Overview](/help/authentication/apple-sso-overview.md)
- [REST API Overview](/help/authentication/rest-api-overview.md)
- [REST API Cookbook (Server-to-Server)](/help/authentication/rest-api-cookbook-servertoserver.md)
- [REST API Cookbook (Client-to-Server)](/help/authentication/rest-api-cookbook-clienttoserver.md)
- [REST API Reference](/help/authentication/rest-api-reference.md)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
