---
title: Guia do Apple SSO (REST API V2)
description: Guia do Apple SSO (REST API V2)
exl-id: 81476312-9ba4-47a0-a4f7-9a557608cfd6
source-git-commit: 0be4216ba816ddace095e557f9f61a8a42e1a1ff
workflow-type: tm+mt
source-wordcount: '3908'
ht-degree: 0%

---

# Guia do Apple SSO (REST API V2) {#apple-sso-cookbook-rest-api-v2}

>[!IMPORTANT]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

A API REST V2 de autenticação do Adobe Pass tem suporte para Logon único de parceiro (SSO) para usuários finais de aplicativos clientes em execução no iOS, iPadOS ou tvOS.

Este documento atua como uma extensão para a [Visão geral da REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) existente, que fornece uma exibição de alto nível e o documento que descreve como implementar o [Logon único usando fluxos de parceiros](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md).

## Logon único do Apple usando fluxos de parceiros {#cookbook}

### Pré-requisitos {#prerequisites}

Antes de prosseguir com o logon único da Apple usando fluxos de parceiros, verifique se os seguintes pré-requisitos foram atendidos:

* O aplicativo de streaming deve coletar todos os dados necessários exigidos pelos cabeçalhos `X-Device-Info` e/ou `User-Agent`, de modo que o back-end de Autenticação do Adobe Pass possa identificar a plataforma do dispositivo e seus recursos. Para obter mais detalhes sobre o cabeçalho `X-Device-Info`, consulte a documentação [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md).

* O aplicativo de streaming deve solicitar acesso às informações de assinatura do usuário salvas no nível do dispositivo, para o qual o usuário deve dar permissão ao aplicativo para continuar, de modo semelhante a fornecer acesso à câmera ou ao microfone do dispositivo. Esta permissão deve ser solicitada por aplicativo usando a [Estrutura de Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) da Apple, e o dispositivo salvará a seleção do usuário.

  Recomendamos incentivar os usuários que se recusam a conceder permissão para acessar informações de assinatura explicando os benefícios da experiência do usuário de logon único do Apple, mas esteja ciente de que o usuário pode alterar sua decisão acessando as configurações do aplicativo (acesso de permissão do Provedor de TV) ou *`Settings -> TV Provider`* no iOS e iPadOS ou *`Settings -> Accounts -> TV Provider`* no tvOS.

  O aplicativo de streaming pode solicitar a permissão do usuário quando o aplicativo entrar no estado de primeiro plano, pois o aplicativo pode verificar a [permissão para acessar](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) as informações de assinatura do usuário a qualquer momento antes de exigir autenticação do usuário.

>[!IMPORTANT]
>
> Suposições
>
> <br/>
>
> * O aplicativo de streaming concluiu os [pré-requisitos de integração](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md#apple-sso-prerequisites-programmer) que se aplicam a um Programador e são necessários para habilitar a experiência de usuário de logon único do Apple.

### Fluxo de trabalho (WRK) {#workflow}

Execute as etapas fornecidas para implementar o logon único do Apple usando fluxos de parceiros conforme mostrado no diagrama a seguir.

![logon único do Apple usando fluxos de parceiros](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-apple-single-sign-on-using-partner-flows.png)

*logon único do Apple usando fluxos de parceiros*

+++A. Fase de registro

1. **Recuperar credenciais do cliente:** O aplicativo de streaming reúne todos os dados necessários para recuperar credenciais do cliente chamando o ponto de extremidade de Registro do Cliente.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Recuperar credenciais do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request) para obter detalhes sobre:
   >
   > * Todos os parâmetros _necessários_, como `software_statement`
   > * Todos os cabeçalhos _necessários_, como `Content-Type`, `X-Device-Info`
   > * Todos os _parâmetros e cabeçalhos_ opcionais

1. **Retornar credenciais de cliente:** A resposta do ponto de extremidade do Registro do Cliente contém informações sobre as credenciais de cliente associadas aos parâmetros e cabeçalhos recebidos.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Recuperar credenciais do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) para obter detalhes sobre as informações fornecidas em uma resposta de credenciais do cliente.
   >
   > <br/>
   >
   > O Registro do cliente valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   >
   > <br/>
   >
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a [Recuperar credenciais do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error) documentação da API.

   >[!TIP]
   >
   > As credenciais do cliente devem ser armazenadas em cache e usadas indefinidamente.

1. **Recuperar token de acesso:** o aplicativo de streaming reúne todos os dados necessários para recuperar o token de acesso chamando o ponto de extremidade Token do Cliente.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Recuperar token de acesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#request) para obter detalhes sobre:
   >
   > * Todos os parâmetros _necessários_, como `client_id`, `client_secret` e `grant_type`
   > * Todos os cabeçalhos _necessários_, como `Content-Type`, `X-Device-Info`
   > * Todos os _parâmetros e cabeçalhos_ opcionais

1. **Retornar token de acesso:** A resposta do ponto de extremidade do Token do Cliente contém informações sobre o token de acesso associado aos parâmetros e cabeçalhos recebidos.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Recuperar token de acesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#success) para obter detalhes sobre as informações fornecidas em uma resposta de token de acesso.
   >
   > <br/>
   >
   > O token do cliente valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   >
   > <br/>
   >
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a [Recuperar token de acesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md#error) documentação da API.

   >[!TIP]
   >
   > O token de acesso deve ser armazenado em cache e usado somente durante o período especificado (por exemplo, 24 horas de vida útil). Após a expiração, o aplicativo de streaming deve solicitar um novo token de acesso.

+++

+++B. Verificar fase de autenticação

1. **Recuperar status da estrutura do parceiro:** O aplicativo de streaming chama a [Estrutura da Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) desenvolvida pela Apple para obter permissões de usuário e informações do provedor.

   >[!IMPORTANT]
   >
   > Consulte a documentação da [Estrutura da Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) para obter detalhes sobre:
   >
   > <br/>
   >
   > * O aplicativo de streaming deve verificar a [permissão para acessar](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) as informações de assinatura do usuário e continuar somente se o usuário permitir.
   > * O aplicativo de streaming deve fornecer um [delegado](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) para o `VSAccountManager`.
   > * O aplicativo de streaming deve enviar uma [solicitação](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) para obter informações sobre a conta do assinante.
   > * O aplicativo de streaming deve aguardar e processar as informações de [metadados](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > O aplicativo de streaming deve garantir que ele especifique um valor booliano igual a `false` para a propriedade [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) no objeto `VSAccountMetadataRequest`, para indicar que o usuário não pode ser interrompido nesta fase.

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
                            // Continue with the "Retrieve profiles" step.
                            ...
                        } else {
                            // The user is not authenticated at platform level, continue with the "Retrieve profiles" step.
                            ...
                        }
                    }
   
            // The user has not yet made a choice or does not allow the application to access subscription information.
            default:
                // Continue with the "Retrieve profiles" step.
                ...
            }
   }
   ...
   ```

1. **Retornar informações de status da estrutura do parceiro:** o aplicativo de streaming valida os dados de resposta para garantir que as condições básicas sejam atendidas:
   * O status de acesso da permissão do usuário é concedido.
   * O identificador de mapeamento do provedor do usuário está presente e é válido.
   * A data de expiração do perfil do provedor do usuário (se disponível) é válida.

1. **Recuperar perfis:** o aplicativo de streaming reúne todos os dados necessários para recuperar todas as informações de perfil, enviando uma solicitação ao ponto de extremidade Perfis.

   >[!IMPORTANT]
   > 
   > O ponto de extremidade da API REST v2 que você **deve** usar nesta etapa é:
   >
   > * [Recuperar perfis](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md#Request) API
   > 
   > ou
   > 
   > * [Recuperar perfis para API mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md#Request) específica
   >
   > Certifique-se de que **não** use a API [Criar e recuperar perfil usando a resposta de autenticação de parceiro](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Request) nesta etapa.

   >[!IMPORTANT]
   >
   > Consulte a [Recuperar perfis](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md#Request) API ou [Recuperar perfis para a documentação específica da API mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md#Request) para obter detalhes sobre:
   >
   > * Todos os parâmetros _necessários_, como `serviceProvider` (ou `mvpd`)
   > * Todos os cabeçalhos _necessários_, como `Authorization`, `AP-Device-Identifier` e `AP-Partner-Framework-Status`
   > * Todos os _parâmetros e cabeçalhos_ opcionais
   >
   > <br/>
   >
   > O aplicativo de transmissão deve garantir que inclua um valor válido para o status da estrutura do parceiro, de modo que a resposta recuperada possa incluir um perfil de tipo &quot;appleSSO&quot;.
   >
   > <br/>
   >
   > Para obter mais detalhes sobre o cabeçalho `AP-Partner-Framework-Status`, consulte a documentação [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Retorne informações sobre os perfis encontrados:** A resposta do ponto de extremidade Perfis contém informações sobre os perfis encontrados associados aos parâmetros e cabeçalhos recebidos.

1. **Escolha um perfil e prossiga com os fluxos de decisões:** Se a resposta do ponto de extremidade Perfis contiver perfis, o aplicativo de streaming usará sua lógica interna (eventualmente interagindo com o usuário final) para escolher um dos perfis disponíveis para continuar com os fluxos de decisões subsequentes.

1. **Continuar com o fluxo de autenticação de parceiro:** Se a resposta do ponto de extremidade Perfis não contiver um perfil, o aplicativo de streaming continuará com o fluxo de autenticação de parceiro.

+++

+++C. Fase de autenticação do parceiro

1. **Recuperar configuração:** o aplicativo de streaming reúne todos os dados necessários para recuperar a lista de MVPDs com integração ativa, enviando uma solicitação ao ponto de extremidade de Configuração.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Recuperar configuração para provedor de serviços específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Request) para obter detalhes sobre:
   >
   > * Todos os parâmetros _necessários_, como `serviceProvider`
   > * Todos os cabeçalhos _necessários_, como `Authorization`, `AP-Device-Identifier` e `X-Device-Info`
   > * Todos os _parâmetros e cabeçalhos_ opcionais

1. **Retornar configuração:** A resposta do ponto de extremidade de Configuração contém informações sobre os MVPDs que têm uma integração ativa com o provedor de serviços.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Recuperar configuração para provedor de serviços específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md#Response) para obter detalhes sobre as informações fornecidas em uma resposta de configuração.
   >
   > <br/>
   >
   > O endpoint de Configuração valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   >
   > <br/>
   >
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > O aplicativo de streaming deve garantir que ele processe os seguintes detalhes fornecidos para cada MVPD ao continuar:
   >
   > * `enablePlatformServices`: indica se a MVPD oferece suporte ao logon único da Apple no momento.
   > * `displayInPlatformPicker`: indica se a MVPD pode ser exibida no seletor de Apple.
   > * `boardingStatus`: indica se a MVPD está integrada no logon único da Apple.

1. **Recuperar status da estrutura do parceiro:** O aplicativo de streaming chama a [Estrutura da Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) desenvolvida pela Apple para obter permissões de usuário e informações do provedor.

   >[!IMPORTANT]
   >
   > Consulte a documentação da [Estrutura da Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) para obter detalhes sobre:
   >
   > <br/>
   >
   > * O aplicativo de streaming deve verificar a [permissão para acessar](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) as informações de assinatura do usuário e continuar somente se o usuário permitir.
   > * O aplicativo de streaming deve fornecer um [delegado](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) para o `VSAccountManager`.
   > * O aplicativo de streaming deve enviar uma [solicitação](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) para obter informações sobre a conta do assinante.
   > * O aplicativo de streaming deve aguardar e processar as informações de [metadados](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > O aplicativo de streaming deve garantir que ele especifique um valor booliano igual a `true` para a propriedade [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) no objeto `VSAccountMetadataRequest`, para indicar que o usuário pode ser interrompido para selecionar o provedor de TV nesta fase.

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
   
                        // This can be computed from the Configuration service response in order to filter the TV providers from the Apple picker.
                        vsaMetadataRequest.supportedAccountProviderIdentifiers = supportedAccountProviderIdentifiers;
   
                        // This can be computed from the Configuration service response in order to sort the TV providers from the Apple picker.
                        if #available(iOS 11.0, tvOS 11, *) {
                            vsaMetadataRequest.featuredAccountProviderIdentifiers = featuredAccountProviderIdentifiers;
                        }
   
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
                                // Continue with the "Retrieve partner authentication request" step.
                                ...
                            } else {
                                // The user is not authenticated at platform level.
                                if (vsaError != nil) {
                                    // The application can check to see if the user selected a provider which is present in Apple picker, but the provider is not onboarded in platform SSO.
                                    if let error: NSError = (vsaError! as NSError), error.code == 1, let appleMsoId = error.userInfo["VSErrorInfoKeyUnsupportedProviderIdentifier"] as! String? {
                                        var mvpd: Mvpd? = nil;
   
                                        // The requestor.mvpds must be computed during the "Return configuration" step. 
                                        for provider in requestor.mvpds {
                                            if provider.platformMappingId == appleMsoId {
                                                mvpd = provider;
                                                break;
                                            }
                                        }
   
                                        if mvpd != nil {
                                            // Continue with the "Proceed with basic authentication flow" step, but you can skip prompting the user with your MVPD picker and use the mvpd selection, therefore creating a better UX.
                                            ...
                                        } else {
                                            // Continue with the "Proceed with basic authentication flow" step.
                                            ...
                                        }
                                    } else {
                                        // Continue with the "Proceed with basic authentication flow" step.
                                        ...
                                    }
                                } else {
                                    // Continue with the "Proceed with basic authentication flow" step.
                                    ...
                                }
                            }
                        }
   
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Proceed with basic authentication flow" step.
                    ...
                }
    }
    ...
   ```

1. **Retornar informações de status da estrutura do parceiro:** o aplicativo de streaming valida os dados de resposta para garantir que as condições básicas sejam atendidas:
   * O status de acesso da permissão do usuário é concedido.
   * O identificador de mapeamento do provedor do usuário está presente e é válido.
   * A data de expiração do perfil do provedor do usuário (se disponível) é válida.

1. **Recuperar solicitação de autenticação do parceiro:** o aplicativo de streaming reúne todos os dados necessários para iniciar uma sessão de autenticação chamando o ponto de extremidade Parceiro de Sessões.

   >[!IMPORTANT]
   >
   > Consulte a [Recuperar solicitação de autenticação do parceiro](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Request) documentação da API para obter detalhes sobre:
   >
   > * Todos os parâmetros _necessários_, como `serviceProvider` e `partner`
   > * Todos os cabeçalhos _necessários_ como `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` e `AP-Partner-Framework-Status`
   > * Todos os cabeçalhos e parâmetros _opcionais_
   >
   > <br/>
   >
   > O aplicativo de streaming deve garantir que inclua um valor válido para o status da estrutura do parceiro, de modo que a resposta recuperada possa incluir uma solicitação de autenticação do parceiro (solicitação SAML).
   >
   > <br/>
   >
   > Para obter mais detalhes sobre o cabeçalho `AP-Partner-Framework-Status`, consulte a documentação [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Indique a próxima ação:** A resposta do ponto de extremidade do Parceiro de Sessões contém os dados necessários para orientar o aplicativo de streaming em relação à próxima ação.

   >[!IMPORTANT]
   >
   > Consulte a [Recuperar solicitação de autenticação do parceiro](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md#Response) documentação da API para obter detalhes sobre as informações fornecidas na resposta da sessão.
   >
   > <br/>
   >
   > O endpoint do Parceiro de sessões valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve estar ativa.
   >
   > <br/>
   >
   > Se a validação básica falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a [documentação de Códigos de erro aprimorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   >
   > O endpoint do Parceiro de sessões valida os dados da solicitação para garantir que as condições de logon único do parceiro sejam atendidas:
   >
   >  * A configuração de logon único do parceiro no servidor do Adobe Pass deve ser válida e ativada.
   >  * A carga de status da estrutura do parceiro recebida por meio do cabeçalho [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) deve ser válida.
   >
   > <br/>
   >
   > Se a validação do logon único do parceiro falhar, a resposta assumirá como padrão o fluxo de autenticação básico.

1. **Continuar com fluxos de decisões:** A resposta do ponto de extremidade do Parceiro de Sessões contém os seguintes dados:
   * O atributo `actionName` está definido como &quot;autorize&quot;.
   * O atributo `actionType` está definido como &quot;direto&quot;.

   Se o back-end do Adobe Pass identificar um perfil válido, o aplicativo de transmissão não precisará reautenticar com o MVPD selecionado, pois já há um perfil que pode ser usado para fluxos de decisões subsequentes.

1. **Continuar com o fluxo de autenticação básico:** A resposta do ponto de extremidade do Parceiro de Sessões contém os seguintes dados:
   * O atributo `actionName` está definido como &quot;autenticar&quot; ou &quot;retomar&quot;.
   * O atributo `actionType` está definido como &quot;interativo&quot; ou &quot;direto&quot;.

   Se o back-end do Adobe Pass não identificar um perfil válido e a validação do logon único do parceiro falhar, o servidor do Adobe Pass voltará ao fluxo de autenticação básico.

   Para obter mais detalhes sobre o fluxo de autenticação básico, consulte os seguintes documentos:
   * [Executar autenticação no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Realizar autenticação no aplicativo secundário com mvpd pré-selecionado](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Executar autenticação no aplicativo secundário sem mvpd pré-selecionado](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

1. **Continue com a criação e recuperação de perfil usando o fluxo de resposta de autenticação de parceiro:** A resposta do ponto de extremidade do Parceiro de Sessões contém os seguintes dados:
   * O atributo `actionName` está definido como &quot;partner_profile&quot;.
   * O atributo `actionType` está definido como &quot;direto&quot;.
   * O atributo `authenticationRequest - type` inclui o protocolo de segurança usado pela estrutura do parceiro para logon do MVPD (atualmente definido somente como SAML).
   * O atributo `authenticationRequest - request` inclui a solicitação SAML passada para a estrutura do parceiro.
   * O atributo `authenticationRequest - attributesNames` inclui os atributos SAML passados para a estrutura do parceiro.

   Se o back-end do Adobe Pass não identificar um perfil válido e a validação de logon único de parceiro passar, o aplicativo de transmissão receberá uma resposta com ações e dados para transmitir à estrutura do parceiro para iniciar o fluxo de autenticação com a MVPD.

1. **Conclua a autenticação do MVPD com a estrutura do parceiro:** Encaminhe a solicitação de autenticação do parceiro (solicitação SAML) obtida na etapa anterior para a [Estrutura de Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount). Se o fluxo de autenticação for bem-sucedido, a interação da [Estrutura de Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) com a MVPD produzirá uma resposta de autenticação de parceiro (resposta SAML) que será retornada junto com as informações de status da estrutura de parceiro.

   >[!IMPORTANT]
   >
   > Consulte a documentação da [Estrutura da Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) para obter detalhes sobre:
   >
   > <br/>
   >
   > * O aplicativo de streaming deve verificar a [permissão para acessar](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) as informações de assinatura do usuário e continuar somente se o usuário permitir.
   > * O aplicativo de streaming deve fornecer um [delegado](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) para o `VSAccountManager`.
   > * O aplicativo de streaming deve enviar uma [solicitação](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) para informações de conta de assinante e incluir a solicitação de autenticação de parceiro (solicitação SAML) obtida na etapa anterior.
   > * O aplicativo de streaming deve aguardar e processar as informações de [metadados](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > O aplicativo de streaming deve garantir que ele especifique um valor booliano igual a `true` para a propriedade [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) no objeto `VSAccountMetadataRequest`, para indicar que o usuário pode ser interrompido para autenticação com o provedor de TV selecionado nesta fase.

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
   
                        // This are the user metadata fields expected to be available on a successful login and are determined from the Sessions SSO service. Look for the authenticationRequest > attributesNames associated with the provider determined in a previous step.
                        vsaMetadataRequest.attributeNames = attributesNames;
   
                        // This is the authenticationRequest > request field from Sessions SSO service.
                        vsaMetadataRequest.verificationToken = authenticationRequestPayload;
   
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
   
                                // Continue with the "Create and retrieve profile using partner authentication response" step.
                                ...
                            } else {
                                // Continue with the "Proceed with basic authentication flow" step.
                                ...
                            }
                        }
   
                // The user has not yet made a choice or does not allow the application to access subscription information.
                default:
                    // Continue with the "Proceed with basic authentication flow" step.
                    ...
                }
    }
    ...
   ```

1. **Resposta de autenticação de parceiro de retorno:** o aplicativo de streaming valida os dados de resposta para garantir que as condições básicas sejam atendidas:
   * O status de acesso da permissão do usuário é concedido.
   * O identificador de mapeamento do provedor do usuário está presente e é válido.
   * A data de expiração do perfil do provedor do usuário (se disponível) é válida.
   * A resposta de autenticação de parceiro (resposta SAML) está presente e é válida.

1. **Criar e recuperar perfil usando a resposta de autenticação de parceiro:** o aplicativo de streaming reúne todos os dados necessários para criar e recuperar um perfil, chamando o ponto de extremidade Parceiro de Perfis.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Criar e recuperar perfil usando a resposta de autenticação de parceiro](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Request) para obter detalhes sobre:
   >
   > * Todos os parâmetros _necessários_, como `serviceProvider`, `partner` e `SAMLResponse`
   > * Todos os cabeçalhos _necessários_, como `Authorization`, `AP-Device-Identifier`, `Content-Type`, `X-Device-Info` e `AP-Partner-Framework-Status`
   > * Todos os cabeçalhos e parâmetros _opcionais_
   >
   > <br/>
   >
   > O aplicativo de transmissão deve garantir que inclua valores válidos para o status da estrutura do parceiro e a resposta de autenticação do parceiro (resposta SAML), de modo que a resposta recuperada possa incluir um perfil de tipo &quot;appleSSO&quot;.
   >
   > <br/>
   >
   > Para obter mais detalhes sobre o cabeçalho `AP-Partner-Framework-Status`, consulte a documentação [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Retornar informações sobre o perfil do parceiro:** A resposta do ponto de extremidade Perfis contém informações sobre o perfil do parceiro, incluindo o atributo `type` definido como &quot;appleSSO&quot;.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Criar e recuperar perfil usando a resposta de autenticação de parceiro](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md#Response) para obter detalhes sobre as informações fornecidas em uma resposta de perfil.
   >
   > <br/>
   >
   > O ponto de extremidade Profiles Partner valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve estar ativa.
   >
   > <br/>
   >
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).
   >
   > <br/>
   >
   > O ponto de extremidade do Parceiro de perfis valida os dados da solicitação para garantir que as condições de logon único do parceiro sejam atendidas:
   >
   >  * A configuração de logon único do parceiro no servidor do Adobe Pass deve ser válida e ativada.
   >  * A carga de status da estrutura do parceiro recebida por meio do cabeçalho [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md) deve ser válida.
   >
   > <br/>
   >
   > Se a validação do logon único do parceiro falhar, a resposta assumirá como padrão o fluxo básico de recuperação do perfil.

1. **Continuar com fluxos de decisões:** o aplicativo de streaming pode continuar com fluxos de decisões subsequentes.

+++

+++ D. Fase das decisões

1. **Recuperar status da estrutura do parceiro:** O aplicativo de streaming chama a [Estrutura da Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) desenvolvida pela Apple para obter permissões de usuário e informações do provedor.

   >[!IMPORTANT]
   > 
   > O aplicativo de transmissão pode ignorar essa etapa se o tipo de perfil de usuário selecionado não for &quot;appleSSO&quot;.

   >[!IMPORTANT]
   >
   > Consulte a documentação da [Estrutura da Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) para obter detalhes sobre:
   >
   > <br/>
   >
   > * O aplicativo de streaming deve verificar a [permissão para acessar](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) as informações de assinatura do usuário e continuar somente se o usuário permitir.
   > * O aplicativo de streaming deve fornecer um [delegado](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) para o `VSAccountManager`.
   > * O aplicativo de streaming deve enviar uma [solicitação](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) para obter informações sobre a conta do assinante.
   > * O aplicativo de streaming deve aguardar e processar as informações de [metadados](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > O aplicativo de streaming deve garantir que ele especifique um valor booliano igual a `false` para a propriedade [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) no objeto `VSAccountMetadataRequest`, para indicar que o usuário não pode ser interrompido nesta fase.

   >[!TIP]
   >
   > O aplicativo de transmissão pode usar um valor em cache para as informações de status da estrutura do parceiro, que recomendamos atualizar quando o aplicativo passa do estado de segundo plano para o primeiro plano. Nesse caso, o aplicativo de transmissão deve garantir que ele armazene em cache e use apenas valores válidos para o status da estrutura do parceiro, conforme descrito pela etapa &quot;Retornar informações de status da estrutura do parceiro&quot;.

1. **Retornar informações de status da estrutura do parceiro:** o aplicativo de streaming valida os dados de resposta para garantir que as condições básicas sejam atendidas:
   * O status de acesso da permissão do usuário é concedido.
   * O identificador de mapeamento do provedor do usuário está presente e é válido.
   * A data de expiração do perfil do provedor de usuário é válida.

   >[!IMPORTANT]
   >
   > O aplicativo de transmissão pode ignorar essa etapa se o tipo de perfil de usuário selecionado não for &quot;appleSSO&quot;.

1. **Recuperar decisões de pré-autorização:** O aplicativo de streaming reúne todos os dados necessários para obter decisões de pré-autorização para uma lista de recursos, chamando o ponto de extremidade de Pré-autorização de Decisões.

   >[!IMPORTANT]
   >
   > Consulte a [Recuperar decisões de pré-autorização usando a documentação da API do mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#request) específica para obter detalhes sobre:
   >
   > * Todos os parâmetros _necessários_, como `serviceProvider`, `mvpd` e `resources`
   > * Todos os cabeçalhos _necessários_, como `Authorization` e `AP-Device-Identifier`
   > * Todos os _parâmetros e cabeçalhos_ opcionais
   >
   > <br/>
   >
   > O aplicativo de transmissão deve garantir que inclua um valor válido para o status da estrutura do parceiro antes de fazer uma solicitação adicional, quando o perfil escolhido for um perfil de tipo &quot;appleSSO&quot;. No entanto, essa etapa poderá ser ignorada se o tipo de perfil de usuário selecionado não for &quot;appleSSO&quot;.
   >
   > <br/>
   >
   > Para obter mais detalhes sobre o cabeçalho `AP-Partner-Framework-Status`, consulte a documentação [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Retornar decisões de pré-autorização:** A resposta de Ponto de Extremidade de Pré-autorização de Decisões contém uma decisão `Permit` ou `Deny` para cada recurso:
   * Uma decisão `Permit` significa que o recurso é reproduzível. A resposta não inclui um token de mídia, pois o fluxo de pré-autorização não deve ser usado para reproduzir recursos.
   * Uma decisão `Deny` significa que o recurso não pode ser reproduzido. A resposta inclui uma carga de erro que segue a documentação de [Códigos de erro aprimorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Consulte a [Recuperar decisões de pré-autorização usando a documentação específica da API mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md#response) para obter detalhes sobre as informações fornecidas em uma resposta de decisão.
   >
   > <br/>
   >
   > O endpoint de pré-autorização de decisões valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve estar ativa.
   >
   > <br/>
   >
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

1. **Recuperar status da estrutura do parceiro:** O aplicativo de streaming chama a [Estrutura da Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) desenvolvida pela Apple para obter permissões de usuário e informações do provedor.

   >[!IMPORTANT]
   >
   > O aplicativo de transmissão pode ignorar essa etapa se o tipo de perfil de usuário selecionado não for &quot;appleSSO&quot;.

   >[!IMPORTANT]
   >
   > Consulte a documentação da [Estrutura da Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) para obter detalhes sobre:
   >
   > <br/>
   >
   > * O aplicativo de streaming deve verificar a [permissão para acessar](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) as informações de assinatura do usuário e continuar somente se o usuário permitir.
   > * O aplicativo de streaming deve fornecer um [delegado](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanagerdelegate) para o `VSAccountManager`.
   > * O aplicativo de streaming deve enviar uma [solicitação](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest) para obter informações sobre a conta do assinante.
   > * O aplicativo de streaming deve aguardar e processar as informações de [metadados](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadata).
   >
   > <br/>
   >
   > O aplicativo de streaming deve garantir que ele especifique um valor booliano igual a `false` para a propriedade [`isInterruptionAllowed`](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmetadatarequest/1771708-isinterruptionallowed) no objeto `VSAccountMetadataRequest`, para indicar que o usuário não pode ser interrompido nesta fase.

   >[!TIP]
   >
   > O aplicativo de transmissão pode usar um valor em cache para as informações de status da estrutura do parceiro, que recomendamos atualizar quando o aplicativo passa do estado de segundo plano para o primeiro plano. Nesse caso, o aplicativo de transmissão deve garantir que ele armazene em cache e use apenas valores válidos para o status da estrutura do parceiro, conforme descrito pela etapa &quot;Retornar informações de status da estrutura do parceiro&quot;.

1. **Retornar informações de status da estrutura do parceiro:** o aplicativo de streaming valida os dados de resposta para garantir que as condições básicas sejam atendidas:
   * O status de acesso da permissão do usuário é concedido.
   * O identificador de mapeamento do provedor do usuário está presente e é válido.
   * A data de expiração do perfil do provedor de usuário é válida.

   >[!IMPORTANT]
   >
   > O aplicativo de transmissão pode ignorar essa etapa se o tipo de perfil de usuário selecionado não for &quot;appleSSO&quot;.

1. **Recuperar decisão de autorização:** o aplicativo de streaming reúne todos os dados necessários para obter uma decisão de autorização para um recurso específico, chamando o ponto de extremidade de Autorização de Decisões.

   >[!IMPORTANT]
   >
   > Consulte a [Recuperar decisões de autorização usando a documentação da API do mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#request) específica para obter detalhes sobre:
   >
   > * Todos os parâmetros _necessários_, como `serviceProvider`, `mvpd` e `resources`
   > * Todos os cabeçalhos _necessários_, como `Authorization` e `AP-Device-Identifier`
   > * Todos os _parâmetros e cabeçalhos_ opcionais
   >
   > <br/>
   >
   > O aplicativo de transmissão deve garantir que inclua um valor válido para o status da estrutura do parceiro antes de fazer uma solicitação adicional, quando o perfil escolhido for um perfil de tipo &quot;appleSSO&quot;. No entanto, essa etapa poderá ser ignorada se o tipo de perfil de usuário selecionado não for &quot;appleSSO&quot;.
   >
   > <br/>
   >
   > Para obter mais detalhes sobre o cabeçalho `AP-Partner-Framework-Status`, consulte a documentação [AP-Partner-Framework-Status](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md).

1. **Decisão de autorização de retorno:** A resposta do ponto de extremidade de Autorização de Decisões contém uma decisão `Permit` ou `Deny` para o recurso específico:
   * Uma decisão `Permit` significa que o recurso é reproduzível. A resposta inclui um token de mídia.
   * Uma decisão `Deny` significa que o recurso não pode ser reproduzido. A resposta inclui uma carga de erro que segue a documentação de [Códigos de erro aprimorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Consulte a [Recuperar decisões de autorização usando a documentação específica da API mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md#response) para obter detalhes sobre as informações fornecidas em uma resposta de decisão.
   >
   > <br/>
   >
   > O endpoint de autorização de decisões valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve estar ativa.
   >
   > <br/>
   >
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

+++

+++ D. Fase de saída

1. **Iniciar logout do Adobe Pass:** o aplicativo de streaming reúne todos os dados necessários para iniciar o fluxo de logout chamando o ponto de extremidade de Logout do Adobe Pass.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Iniciar logout para mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#request) específica para obter detalhes sobre:
   >
   > * Todos os parâmetros _necessários_, como `serviceProvider`, `mvpd` e `redirectUrl`
   > * Todos os cabeçalhos _necessários_, como `Authorization`, `AP-Device-Identifier`
   > * Todos os _parâmetros e cabeçalhos_ opcionais

1. **Indique a próxima ação:** A resposta do ponto de extremidade de logout do Adobe Pass contém os dados necessários para orientar o aplicativo de streaming em relação à próxima ação:
   * O atributo `url` está ausente, pois o usuário precisa interagir com o nível do parceiro (sistema) para concluir o fluxo de logout.
   * O atributo `actionName` está definido como &quot;partner_logout&quot;.
   * O atributo `actionType` está definido como &quot;partner_interative&quot;.

   >[!IMPORTANT]
   >
   > O aplicativo de streaming deve solicitar que o usuário conclua o processo de logout no nível do parceiro, conforme especificado pelos atributos `actionName` e `actionType`, quando o tipo de perfil de usuário removido for &quot;appleSSO&quot;.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Iniciar logout para mvpd](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md#response) específica para obter detalhes sobre as informações fornecidas em uma resposta de logout.
   >
   > <br/>
   >
   > O endpoint de logout do Adobe Pass valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve estar ativa.
   >
   > <br/>
   >
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

+++
