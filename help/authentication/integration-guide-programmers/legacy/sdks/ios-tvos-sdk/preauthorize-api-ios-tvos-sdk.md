---
title: Pré-autorização da API do iOS/tvOS
description: Pré-autorização da API do iOS/tvOS
exl-id: 79c596a4-0e38-4b6c-bb85-f97c6af45ed8
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 0%

---

# (Herdado) Pré-autorizar {#preauthorize}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

A API pré-autorizada pode ser usada para obter uma decisão de pré-autorização para um ou mais recursos, dessa forma, o aplicativo pode implementar dicas de interface do usuário e/ou filtragem de conteúdo.

>[!IMPORTANT]
>
>A API de Autorização **deve** ser usada antes de conceder ao usuário acesso aos recursos especificados.

Se o resultado da resposta da API Pré-autorizada contiver um ou mais recursos com uma decisão de pré-autorização negada, informações adicionais sobre o erro poderão ser incluídas **(veja a observação abaixo)** para cada recurso afetado.

>[!IMPORTANT]
>
>O recurso aprimorado de relatório de erros, que adiciona informações de erro adicionais para decisões de pré-autorização negadas, está disponível mediante solicitação, pois deve ser ativado no lado da configuração da Autenticação Adobe Pass.

Caso a solicitação da API pré-autorizada não possa ser atendida devido a um erro do Adobe Pass Authentication SDK ou caso ocorra um erro do Adobe Pass Authentication services, informações de erro adicionais (independentemente da configuração acima) e nenhum recurso será incluído como parte do resultado da resposta da API pré-autorizada.

</br>

## `- (void) preauthorize:(nonnull PreauthorizeRequest *)request didCompleteWith:(nonnull AccessEnablerCallback<PreauthorizeResponse *> *)callback;`


**Disponibilidade:** v3.6.0+

**Parâmetros:**

- PreauthorizeRequest: o objeto de solicitação usado para transmitir o conteúdo da solicitação de API;
- AccessEnablerCallback: o objeto de retorno de chamada usado para retornar a resposta da API;
- PreauthorizeResponse: o objeto de resposta usado para retornar o conteúdo de resposta da API;


</br>

## `class PreauthorizeRequest`{#androidpreauthorizerequest}

### **classe PreauthorizeRequest.Builder**

```
    ///
    /// Sets the `List` of resources for which you want to obtain preauthorization decisions.
    ///
    /// Each element in the list must be a `String` representing either the resource ID value or the media RSS fragment which must be agreed with the MVPD.
    ///
    /// This function sets the information only in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// To build an actual `PreauthorizeRequest` you can have a look at `Builder`'s function:
    ///
    /// ```
    /// public func build() -> PreauthorizeRequest
    /// ```
    ///
    /// - Parameter resources: The `List` of resources for which you want to obtain preauthorization decisions.
    ///
    /// - Returns: The reference to the same `Builder` object instance which is the receiver of the function call. It does this in order to allow the creation of function chaining.
    ///
    public func setResources(resources: [String]) -> PreauthorizeRequest.Builder

 

    ///
    /// Sets the features which you want to have them disabled when obtaining preauthorization decisions.
    ///
    /// The list of available features are provided by `PreauthorizeRequest.Feature` enumeration. All features are enabled by default.
    ///
    /// This function sets the information only in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// To build an actual `PreauthorizeRequest` you can have a look at `Builder`'s function:
    ///
    /// ```
    /// public func build() -> PreauthorizeRequest
    /// ```
    ///
    /// - Parameter features: The set of features which you want to have them disabled.
    ///
    /// - Returns: The reference to the same `Builder` object instance which is the receiver of the function call. It does this in order to allow the creation of function chaining.
    ///
    public func disableFeatures(features: Set<PreauthorizeRequest.Feature>) -> PreauthorizeRequest.Builder

 

    ///
    /// Creates and retrieves the reference of a new `PreauthorizeRequest` object instance.
    ///
    /// This function instantiates a new `PreauthorizeRequest` object every time it is called.
    ///
    /// This function uses the values set in advance in the context of current `Builder` object instance which is the receiver of this function call.
    ///
    /// Bear in mind that this function does not produce any side effects, therefore it does not alter the state of the SDK or the state of the `Builder` object instance which is the receiver of this function call.
    ///
    /// It means that successive calls of this function for the same receiver will create different new `PreauthorizeRequest` object instances, but having the same information, in case the values set to the `Builder` where not modified between the calls.
    ///
    /// In case you do not need to update any of the provided information (resources, features, etc.) you may reuse the `PreauthorizeRequest` instance for multiple uses of the `preauthorize` API.
    ///
    /// - Returns: The reference to a new `PreauthorizeRequest` object instance.
    ///
    public func build() -> PreauthorizeRequest
```


## **enum PreauthorizeRequest.Feature**

```
    ///
    /// This feature controls whether to use the information from the AccessEnabler SDK cache or to bypass it and
    /// rely on Adobe Pass server information via a network call.
    ///
    LOCAL_CACHE

    ///
    /// This feature controls whether to use the information from the Adobe Pass server cache or to bypass it and
    /// rely on MVPD server information via a network call.
    ///
    REMOTE_CACHE
```

</br>

## `interface AccessEnablerCallback<PreauthorizeResponse>` {#accessenablercallback}

```
    /// Response callback called by the SDK when the preauthorize API request was fulfilled. The result is either a successful or an error result containing a status.
    public func onResponse(result: PreauthorizeResponse)


    /// Failure callback called by the SDK when the preauthorize API request could not be serviced. The result is a failure result containing a status. 
    public func onFailure(result: PreauthorizeResponse)
```

</br>


## `class PreauthorizeResponse` {#preauthorizeresponse}

```
    ///
    /// - Returns: Additional status (state) information in case of error or failure.
    ///   Might hold a `nil` value.
    ///
    public Status getStatus()

    ///
    /// - Returns: The list of preauthorization decisions. One decision for each resource.
    ///            The list might be empty in case of error or failure.
    ///
    public List<Decision> getDecisions()
```

### Exemplos:

Esta seção destaca a estrutura JSON de alguns objetos PreauthorizeResponse possíveis.

>[!IMPORTANT]
>
>Os JSONs apresentados pelos exemplos a seguir podem ser acessados somente por meio das classes de modelo mostradas neste documento. Você não será capaz de acessar as propriedades desses JSONs por meio de métodos públicos.

>[!IMPORTANT]
>
>A lista de possíveis erros adicionais recuperados por meio do recurso aprimorado de relatório de erros está documentada em [Relatório de erros avançado](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

#### Bem-sucedido

Todos os recursos solicitados estão tendo uma decisão de pré-autorização positiva

```JSON
    {
        "resources": [
            {
                "id": "resource1",
                "authorized": true 
            },
            {
                "id": "resource2",
                "authorized": true 
            },
            {
                "id": "resource3",
                "authorized": true 
            }
        ]
    }
```


Um ou mais recursos estão tendo uma decisão de pré-autorização negada e o recurso de relatório de erros aprimorado não está habilitado na configuração de Autenticação do Adobe Pass

```JSON
    {
        "resources": [
            {
                "id": "resource1",
                "authorized": true 
            },
            {
                "id": "resource2",
                "authorized": false,
                 
            },
            {
                "id": "resource3",
                "authorized": true 
            }
        ]
    }
```


Um ou mais recursos estão tendo uma decisão de pré-autorização negada e o recurso de relatório de erros aprimorado está habilitado na configuração de Autenticação do Adobe Pass

```JSON
    {
        "resources": [
            {
                "id": "resource1",
                "authorized": true 
            },
            {
                "id": "resource2",
                "authorized": false,
                "error" : {
                   "status" : 403,
                   "code" : "authorization_denied_by_mvpd",
                   "message" : "User not authorized",
                   "details" : "Your subscription package does not include the "TestStream3" channel.",
                   "helpUrl" : "https://experienceleague.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=pt-BR",
                   "trace" : "0453f8c8-167a-4429-8784-cd32cfeaee58",
                   "action" : "none"
                }
            },
            {
                "id": "resource3",
                "authorized": true 
            }
        ]
    }
```


#### Erro



Os serviços de Autenticação do Adobe Pass encontram um erro ao atender à solicitação da API Pré-autorizada

```JSON
    {
        "resources": [],
        "status": {
            "status": 400,
            "code" : "bad_request",
            "message": "Missing required parameter : deviceId",
            "details": "",
            "helpUrl" : "https://experienceleague.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=pt-BR",
            "trace" : "9f115e1c-0158-4a41-8805-9f68923f3646",
            "action" : "none"
        }
    }
```


#### Falha

O SDK de autenticação da Adobe Pass encontra um erro ao atender à solicitação da API pré-autorizada

```JSON
    {
        "status": 0,
        "code": "requestor_not_configured",
        "message": "The requestor is not yet configured which is a prerequisite for using any API apart from the setRequestor API.",
        "action": "retry"
    }
    
    {
        "status": 0,
        "code": "authentication_session_expired",
        "message": "The current authentication session has expired. The user must re-authenticate with a supported MVPD in order to continue.",
        "action": "authentication"
    }
    
    {
        "status": 0,
        "code": "authentication_session_missing",
        "message": "The authentication session associated with this request could not be retrieved. The user must re-authenticate with a supported MVPD in order to continue.",
        "action": "authentication"
    }
    
    {
        "status": 0,
        "code": "access_token_unavailable",
        "message": "The request failed due to an unexpected error while retrieving the access token. Please check the validity of your software statement and custom scheme.",
        "action": "none"
    }
    
    {
        "status": 0,
        "code": "server_response_format_unknown",
        "message": "The request failed due to an unknown server response format. Please capture the device console logs and contact support.",
        "action": "none"
    }
    
    {
        "status": 0,
        "code": "network_error",
        "message": "The request failed due to a network error. If the issue persists over several retries, please capture the device console logs and contact support.",
        "action": "none"
    }
```

</br>

## **Status da classe** {#status}

```
    ///
    /// - Returns: The HTTP response status code as documented in RFC 7231.
    ///            Might be 0 in case the `Status` comes from the SDK instead of Adobe Pass Authentication services.
    ///
    public int getStatus()

    ///
    /// - Returns: The standard Adobe Pass Authentication services error code.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getCode()

    ///
    /// - Returns: The human readable message which can be displayed to the end user.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getMessage()

    ///
    /// - Returns: The detailed message which in some cases is provided by the MVPD authorization endpoints or by Programmer degradation rules.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getDetails()

    ///
    /// - Returns: The URL that links to more information about why this state/error occurred and possible solutions.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getHelpUrl()

    ///
    /// - Returns: The unique identifier for this response, which can be used when contacting support to identify specific issues in more complex scenarios.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getTrace()

    ///
    /// - Returns: The recommended action to remediate the situation.
    ///             - none: Unfortunately there is no predefined action to remediate this issue. This might indicate an improper invocation of the public API
    ///             - configuration: A configuration change is needed through TVE dashboard or by contacting support.
    ///             - application-registration: The application must register itself again.
    ///             - authentication: The user must authenticate or re-authenticate.
    ///             - authorization: The user must obtain authorization for the specific resource.
    ///             - degradation: Some form of degradation should be applied.
    ///             - retry: Retrying the request might solve the issue
    ///             - retry-after: Retrying the request after the indicated period of time might solve the issue.
    ///            Might hold an empty string or a `nil` value.
    ///
    public String getAction()
```

<br>

## **Decisão de classe** {#decision}

```
    ///
    /// This is a getter function.
    ///
    /// - Returns: The resource id for which the decision was obtained.
    ///
    public Status getId()

    ///
    /// This is a getter function.
    ///
    /// - Returns: The value of the flag indicating if the decision is successful or not.
    ///
    public boolean isAuthorized()

    ///
    /// This is a getter function.
    ///
    /// - Returns: Additional status (state) information in case some error has occurred.
    ///            Might hold a `nil` value.
    ///
    public Status getError()
```

</br>


## **Código de exemplo** {#sample}

```
let resources: [String] = ["resource_1", "resource_2", "resource_3"];

let disabledFeatures: Set<PreauthorizationRequest.Feature> = [PreauthorizationRequest.Feature.LOCAL_CACHE];

// Build the Preauthorization API request using the builder from PreauthorizationRequest class`

let request: PreauthorizationRequest = PreauthorizationRequest.Builder()

                  .setResources(resources: resources)


                  .disableFeatures(features: disabledFeatures)  // It is **optional** to disable features. If not used all features are enabled by default.

                  .build();

// Build the AccessEnablerCallback by providing the constructor two callbacks for onResponse and onFailure handling  
func onResponseCallback(result: PreauthorizeResponse) -> Void {  //
TODO };

func onFailureCallback(result: PreauthorizeResponse) -> Void {

// TODO

};

let callback: AccessEnablerCallback<PreauthorizeResponse> = AccessEnablerCallback<PreauthorizeResponse>(onResponse: onResponseCallback, onFailure: onFailureCallback);

// Use the preauthorize API
accessEnabler.preauthorize(request, callback);
```
