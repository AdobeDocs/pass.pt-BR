---
title: Guia do Apple SSO (REST API V1)
description: Guia do Apple SSO (REST API V1)
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '1473'
ht-degree: 0%

---

# Guia do Apple SSO (REST API V1) {#apple-sso-cookbook-rest-api-v1}

>[!IMPORTANT]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

A API REST V1 de autenticação da Adobe Pass tem suporte para Logon único de parceiro (SSO) para usuários finais de aplicativos clientes em execução no iOS, iPadOS ou tvOS.

Este documento atua como uma extensão para a documentação REST API V1 existente, que pode ser encontrada [aqui](/help/authentication/rest-api-reference.md).

## Guia {#apple-sso-cookbook-rest-api-v1-cookbook}

Para se beneficiar da experiência do usuário do Apple SSO, o aplicativo precisa integrar a [Estrutura da Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) desenvolvida pela Apple, enquanto para a comunicação REST API V1 de Autenticação da Adobe Pass, é necessário seguir a sequência de etapas apresentadas abaixo.

### Permissão {#apple-sso-cookbook-rest-api-v1-permission}

>[!TIP]
>
> **<u>Dica Profissional:</u>** o aplicativo de streaming deve solicitar acesso às informações de assinatura do usuário salvas no nível do dispositivo, para o qual o usuário deve dar permissão ao aplicativo para continuar, de modo semelhante a fornecer acesso à câmera ou ao microfone do dispositivo. Esta permissão deve ser solicitada por aplicativo usando a [Estrutura de Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) da Apple, e o dispositivo salvará a seleção do usuário.

>[!TIP]
>
> **<u>Dica Profissional:</u>** Recomendamos incentivar os usuários que se recusam a conceder permissão de acesso às informações de assinatura explicando os benefícios da experiência do usuário de logon único do Apple, mas esteja ciente de que o usuário pode alterar sua decisão acessando as configurações do aplicativo (acesso de permissão ao Provedor de TV) ou *`Settings -> TV Provider`* no iOS e iPadOS ou *`Settings -> Accounts -> TV Provider`* no tvOS.

>[!TIP]
>
> **<u>Dica Profissional:</u>** recomendamos solicitar a permissão do usuário quando o aplicativo entrar no estado de primeiro plano, pois o aplicativo pode verificar a [permissão para acessar](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) as informações de assinatura do usuário a qualquer momento antes de solicitar a autenticação do usuário.

### Autenticação {#apple-sso-cookbook-rest-api-v1-authentication}

* [Há um token de autenticação de Adobe válido?](#step1)
* [O usuário está conectado por meio do SSO do parceiro?](#step2)
* [Buscar configuração de Adobe](#step3)
* [Iniciar fluxo de trabalho SSO do parceiro com configuração Adobe](#step4)
* [O logon do usuário foi bem-sucedido?](#step5)
* [Obter uma solicitação de perfil do Adobe para o MVPD selecionado](#step6)
* [Encaminhe a solicitação de Adobe para o SSO do parceiro para obter o perfil](#step7)
* [Trocar o perfil SSO do parceiro por um token de autenticação de Adobe](#step8)
* [O token de Adobe foi gerado com sucesso?](#step9)
* [Iniciar fluxo de trabalho de autenticação regular](#step10)
* [Continuar com fluxos de autorização](#step11)

![](../../../assets/rest-api-v1/apple-sso-cookbook-rest-api-v1.png)

#### Etapa: &quot;Há um token de autenticação de Adobe válido?&quot; {#step1}

>[!TIP]
>
> **<u>Dica:</u>** implemente isso por meio do serviço de API [Verificar Token de Autenticação](/help/authentication/check-authentication-token.md) da Autenticação do Adobe Pass.

#### Etapa: &quot;O usuário está conectado por meio do SSO do parceiro?&quot; {#step2}

>[!TIP]
>
> **<u>Dica:</u>** faça a implementação por meio da [Estrutura da Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount).

* O aplicativo teria que verificar a [permissão para acessar](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) as informações de assinatura do usuário e continuar somente se o usuário permitisse.
* O aplicativo teria que enviar uma [solicitação](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) para obter informações sobre a conta do assinante.
* O aplicativo teria que aguardar e processar as informações de [metadados](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).

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
                    
                    // This is going to make the Video Subscriber Account Framework to refrain from prompting the user with the providers picker at this step. 
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
                            // Continue with the "Forward the Adobe request to Partner SSO to obtain the profile" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Fetch Adobe configuration" step.
                            ...
                        }
                    }
        
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Continue with the "Initiate regular authentication workflow" step.
                ...
            }
}
...  
```

#### Etapa: &quot;Buscar configuração de Adobe&quot; {#step3}

>[!TIP]
>
> **<u>Dica:</u>** implemente isso por meio da Autenticação Adobe Pass [Forneça o serviço de API MVPD List](/help/authentication/provide-mvpd-list.md).

>[!TIP]
>
> **<u>Dica Profissional:</u>** esteja ciente das propriedades MVPD: *`enablePlatformServices`*, *`boardingStatus`*, *`displayInPlatformPicker`*, *`platformMappingId`*, *`requiredMetadataFields`* e preste atenção extra aos comentários apresentados em trechos de código de outras etapas.

#### Etapa &quot;Iniciar fluxo de trabalho SSO do parceiro com configuração Adobe&quot; {#step4}

>[!TIP]
>
> **<u>Dica:</u>** faça a implementação por meio da [Estrutura da Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount).

* O aplicativo teria que verificar a [permissão para acessar](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) as informações de assinatura do usuário e continuar somente se o usuário permitisse.
* O aplicativo teria que fornecer um [delegate](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) para o VSAccountManager.
* O aplicativo teria que enviar uma [solicitação](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) para obter informações sobre a conta do assinante.
* O aplicativo teria que aguardar e processar as informações de [metadados](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).

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
                        
                        // This is going to make the Video Subscriber Account Framework to prompt the user with the providers picker at this step. 
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
                                // Continue with the "Forward the Adobe request to Partner SSO to obtain the profile" step.
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
                                            // Continue with the "Initiate regular authentication workflow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Initiate regular authentication workflow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Initiate regular authentication workflow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Initiate regular authentication workflow" step.
                                    ...
                                }
                            }
                        }
            
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Initiate regular authentication workflow" step.
                    ...
                }
    }
    ...
```

#### Etapa: &quot;O logon do usuário foi bem-sucedido?&quot; {#step5}

>[!TIP]
>
> **<u>Dica de Profissional:</u>** esteja ciente do trecho de código da etapa [&quot;Iniciar fluxo de trabalho SSO de Parceiro com configuração Adobe&quot;](#step4). O logon do usuário é bem-sucedido caso o *`vsaMetadata!.accountProviderIdentifier`* contenha um valor válido e a data atual não tenha passado o valor *`vsaMetadata!.authenticationExpirationDate`*.

#### Etapa &quot;Obter uma solicitação de perfil do Adobe para o MVPD selecionado&quot; {#step6}

>[!TIP]
>
> **<u>Dica:</u>** implemente isso por meio do serviço de API [Solicitação de perfil](/help/authentication/retrieve-profilerequest.md) da Autenticação do Adobe Pass.

>[!TIP]
>
> **<u>Dica de Profissional:</u>** esteja ciente de que o identificador do provedor obtido da Estrutura de Conta de Assinante de Vídeo representa o *`platformMappingId`* em termos de configuração de Autenticação do Adobe Pass. Portanto, o aplicativo deve determinar o valor da propriedade de ID MVPD, usando o valor *`platformMappingId`*, por meio do serviço de API Autenticação Adobe Pass [Fornecer Lista MVPD](/help/authentication/provide-mvpd-list.md).

#### Etapa: &quot;Encaminhar a solicitação de Adobe para o SSO do parceiro para obter o perfil&quot; {#step7}

>[!TIP]
>
> **<u>Dica:</u>** faça a implementação por meio da [Estrutura da Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount).


* O aplicativo teria que verificar a [permissão para acessar](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) as informações de assinatura do usuário e continuar somente se o usuário permitisse.
* O aplicativo teria que enviar uma [solicitação](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) para obter informações sobre a conta do assinante.
* O aplicativo teria que aguardar e processar as informações de [metadados](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).

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
                        
                        // This is going to make the Video Subscriber Account Framework to refrain from prompting the user with the providers picker at this step. 
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
                                
                                // Continue with the "Exchange the Partner SSO profile for an Adobe authentication token" step.
                                ...
                            } else {
                                // Continue with the "Initiate regular authentication workflow" step.
                                ...
                            }
                        }
                        
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Initiate regular authentication workflow" step.
                    ...
                }
    }
    ...
```

#### Etapa: &quot;Trocar o perfil SSO do parceiro por um token de autenticação Adobe&quot; {#step8}

>[!TIP]
>
> **<u>Dica:</u>** implemente isso por meio do serviço de API [Token Exchange](/help/authentication/token-exchange.md) da Autenticação do Adobe Pass.

>[!TIP]
>
> **<u>Dica do Profissional:</u>** esteja ciente do trecho de código da etapa [&quot;Encaminhar a solicitação de Adobe para o SSO do Parceiro para obter o perfil&quot;](#step7). Este *`vsaMetadata!.samlAttributeQueryResponse!`* representa o *`SAMLResponse`*, que precisa ser passado no [Token Exchange](/help/authentication/token-exchange.md) e requer manipulação e codificação de cadeia de caracteres (*Base64* codificada e *URL* codificada posteriormente) antes de fazer a chamada.

#### Etapa: &quot;O token Adobe foi gerado com êxito?&quot; {#step9}

>[!TIP]
>
> **<u>Dica:</u>** implemente isso por meio da resposta bem-sucedida do [Token Exchange](/help/authentication/token-exchange.md) da Autenticação do Adobe Pass, que será um *`204 No Content`*, indicando que o token foi criado com êxito e está pronto para ser usado para os fluxos de autorização.

#### Etapa: &quot;Iniciar fluxo de trabalho de autenticação regular&quot; {#step10}

>[!TIP]
>
> Adobe Pass **<u>Dica:</u>** implemente isso por meio da [Solicitação de Código de Registro](/help/authentication/registration-code-request.md), [Iniciar Autenticação](/help/authentication/initiate-authentication.md) e [Recuperar Token de Autenticação](/help/authentication/retrieve-authentication-token.md) ou [Verificar Token de Autenticação](/help/authentication/check-authentication-token.md) para os serviços de API.

>[!TIP]
>
> **<u>Dica do profissional:</u>** siga as etapas abaixo para a implementação do tvOS.

* O aplicativo teria que [obter um código de registro](/help/authentication/registration-code-request.md) e apresentá-lo ao usuário final no primeiro dispositivo (tela).
* O aplicativo teria que iniciar a [sondagem para reconhecer o estado de autenticação](/help/authentication/retrieve-authentication-token.md) no primeiro dispositivo (tela) após a obtenção do código de registro.
* Outro aplicativo teria que [iniciar a autenticação](/help/authentication/initiate-authentication.md) em um segundo dispositivo (tela) quando o código de registro fosse usado.
* O aplicativo teria que parar a [sondagem](/help/authentication/retrieve-authentication-token.md) no primeiro dispositivo (tela) quando o token de autenticação fosse gerado.

>[!TIP]
>
> **<u>Dica dos profissionais:</u>** Siga as etapas abaixo para a(s) implementação(ões) do iOS/iPadOS.

* O aplicativo teria que [obter um código de registro](/help/authentication/registration-code-request.md), que não deve ser apresentado ao usuário final no primeiro dispositivo (tela).
* O aplicativo teria que [iniciar a autenticação](/help/authentication/initiate-authentication.md) no primeiro dispositivo (tela) usando o código de registro e um [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) ou um componente [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller).
* O aplicativo teria que iniciar a [sondagem para conhecer o estado de autenticação](/help/authentication/retrieve-authentication-token.md) no primeiro dispositivo (tela) após o fechamento do [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) ou do [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller).
* O aplicativo teria que parar a [sondagem](/help/authentication/retrieve-authentication-token.md) no primeiro dispositivo (tela) quando o token de autenticação fosse gerado.

#### Etapa: &quot;Continuar com os fluxos de autorização&quot; {#step11}

>[!TIP]
>
> **<u>Dica:</u>** implemente isso por meio da Autenticação do Adobe Pass [Iniciar Autorização](/help/authentication/initiate-authorization.md) e [Obter Token de Mídia Curta](/help/authentication/obtain-short-media-token.md) para os serviços de API.

### Sair {#apple-sso-cookbook-rest-api-v1-logout}

A [Estrutura de Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) não fornece uma API para desconectar programaticamente as pessoas que entraram na conta do provedor de TV no nível do sistema do dispositivo. Portanto, para que o logout tenha efeito total, o usuário final teria que sair explicitamente do *`Settings -> TV Provider`* no iOS/iPadOS ou do *`Settings -> Accounts -> TV Provider`* no tvOS. A outra opção que o usuário teria é retirar a permissão para acessar as informações de assinatura do usuário na seção de configurações específicas do aplicativo (acesso ao Provedor de TV).

>[!TIP]
>
> **<u>Dica:</u>** implemente isso por meio da mídia da [Chamada de Metadados do Usuário](/help/authentication/user-metadata.md) e do [Logout](/help/authentication/initiate-logout.md) da Autenticação da Adobe Pass.

>[!TIP]
>
> **<u>Dica do profissional:</u>** siga as etapas abaixo para a implementação do tvOS.

* O aplicativo teria que determinar se a autenticação ocorreu como resultado de uma entrada através do SSO parceiro, usando os &quot;*metadados de usuário](/help/authentication/user-metadata.md) do [tokenSource&quot;* do serviço de Autenticação do Adobe Pass.
* O aplicativo teria que instruir/solicitar que o usuário saia explicitamente de *`Settings -> Accounts -> TV Provider`* no tvOS **only** caso o valor *&quot;tokenSource&quot;* fosse igual a &quot;*Apple&quot;.*
* O aplicativo teria que [iniciar o logout](/help/authentication/initiate-logout.md) do serviço de Autenticação do Adobe Pass usando uma chamada HTTP direta. Isso não facilitaria a limpeza da sessão no lado do MVPD.

>[!TIP]
>
> **<u>Dica dos profissionais:</u>** Siga as etapas abaixo para a(s) implementação(ões) do iOS/iPadOS.

* O aplicativo teria que determinar se a autenticação ocorreu como resultado de uma entrada através do SSO parceiro, usando os &quot;*metadados de usuário](/help/authentication/user-metadata.md) do [tokenSource&quot;* do serviço de Autenticação do Adobe Pass.
* O aplicativo teria que instruir/solicitar que o usuário saia explicitamente de *`Settings -> TV Provider`* no iOS/iPadOS **only** caso o valor *&quot;tokenSource&quot;* fosse igual a *&quot;Apple&quot;*.
* O aplicativo teria que [iniciar o logout](/help/authentication/initiate-logout.md) do serviço de Autenticação do Adobe Pass usando um [WKWebView](https://developer.apple.com/documentation/webkit/wkwebview) ou um componente [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller). Isso facilitaria a limpeza da sessão no lado do MVPD.
