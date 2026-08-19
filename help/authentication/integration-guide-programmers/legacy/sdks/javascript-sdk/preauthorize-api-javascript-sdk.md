---
title: Pré-autorizar
description: Pré-autorização do JavaScript
exl-id: b7493ca6-1862-4cea-a11e-a634c935c86e
source-git-commit: 7208b16831e1c6c4cbb37bf925a798d931ab8ea3
workflow-type: tm+mt
source-wordcount: '1149'
ht-degree: 0%

---

# (Herdado) Pré-autorizar {#js-preauthorize}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

## Visão geral {#preauth-overview}

O método da API pré-autorizada deve ser usado por aplicativos para obter decisões de pré-autorização para um ou mais recursos. A solicitação da API pré-autorizada deve ser usada para dicas da interface do usuário e/ou filtragem de conteúdo. Uma solicitação de API de autorização real deve ser feita antes de permitir o acesso do usuário aos recursos especificados.

Caso ocorra um erro inesperado (por exemplo, problema de rede e ponto de acesso de autorização do MVPD indisponível) quando uma solicitação de API pré-autorizada for processada pelos serviços de autenticação da Adobe Pass, uma ou várias informações de erro separadas serão incluídas para os recursos afetados como parte do resultado da resposta da API pré-autorizada.

### public preauthorize(solicitação: PreauthorizeRequest, retorno de chamada: AccessEnablerCallback&lt;qualquer>): void {#preauth-method}

**Descrição:** este método deve ser usado por aplicativos para obter decisões de pré-autorização de usuário autenticado (informativas) do serviço de Autenticação da Adobe Pass para exibir recursos protegidos específicos, com o objetivo principal de decorar a interface do usuário do aplicativo (por exemplo, indicar o status de acesso com ícones de bloqueio e desbloqueio).

**Disponibilidade:** v4.4.0+

**Parâmetros:**

* `PreauthorizeRequest`: Objeto do Construtor usado para definir a solicitação
* `AccessEnablerCallback`: retorno de chamada usado para retornar a resposta da API
* `PreauthorizeResponse`: objeto usado para retornar o conteúdo de resposta da API

### classe PreauthorizeRequestBuilder {#preath-req-builder-class}

#### setResources(resources: string[]): PreauthorizeRequestBuilder {#set-res-preath-req-buildr}

* Define a Lista de recursos para os quais você deseja obter decisões de pré-autorização.
* É obrigatório defini-lo para o uso da API pré-autorizada.
* Cada elemento na lista deve ser uma String representando o valor da ID do recurso ou o fragmento de RSS de mídia que deve ser acordado com a MVPD.
* Este método define as informações somente no contexto da instância do objeto `PreauthorizeRequestBuilder` atual, que é o receptor dessa chamada de método.

* Para construir um `PreauthorizeRequest` real, você pode observar o método de `PreauthorizeRequestBuilder`:

```JavaScript
  build(): PreauthorizeRequest
```

* `@param {string[]}` recursos. A Lista de recursos para os quais você deseja obter decisões de pré-autorização.
* `@returns {PreauthorizeRequestBuilder}` A referência à mesma instância do objeto `PreauthorizeRequestBuilder`, que é o receptor da chamada de método.
* Ela faz isso para permitir a criação de encadeamento de métodos.

#### disableFeatures(...features: string[]): PreauthorizeRequestBuilder {#disabl-featres-preauth-req-buildr}

* Define os recursos que você deseja desabilitar ao obter decisões de pré-autorização.
* Esta função define as informações somente no contexto da instância do objeto `PreauthorizeRequestBuilder` atual, que é o receptor dessa chamada de função.
* Para construir um `PreauthorizeRequest` real, você pode ver a função de `PreauthorizeRequestBuilder`:

```JavaScript
public func build() -> PreauthorizeRequest
```

* `@param {string[]}` recursos. O conjunto de recursos que você deseja que eles sejam desativados.
* `@returns` A referência à mesma instância do objeto `PreauthorizeRequestBuilder`, que é o receptor da chamada de função.
* Ele faz isso para permitir a criação de encadeamento de funções.

#### build(): PreauthorizeRequest {#preauth-req}

* Cria e recupera a referência de uma nova instância de objeto `PreauthorizeRequest`.
* Este método instancia um novo objeto `PreauthorizeRequest` sempre que é chamado.
* Este método usa os valores definidos antecipadamente no contexto da instância do objeto `PreauthorizeRequestBuilder` atual, que é o receptor dessa chamada de método.
* Tenha em mente que este método não produz quaisquer efeitos colaterais,
* portanto, não altera o estado da SDK ou o estado da instância do objeto `PreauthorizeRequestBuilder`, que é o receptor dessa chamada de método.
* Isso significa que chamadas sucessivas desse método para o mesmo receptor criarão novas instâncias de objeto `PreauthorizeRequest` diferentes, mas com as mesmas informações, caso os valores definidos como `PreauthorizeRequestBuilder` não tenham sido modificados entre as chamadas.
* Caso não precise atualizar nenhuma das informações fornecidas (recursos e armazenamento em cache), é possível reutilizar a instância PreauthorizeRequest para vários usos da API pré-autorizada.
* `@returns {PreauthorizeRequest}`

### interface AccessEnablerCallback&lt;T> {#interface-access-enablr-callback}

#### onResponse(resultado: T); {#on-response-result}

* O retorno de chamada de resposta é chamado pelo SDK quando a solicitação da API pré-autorizada é atendida.
* O resultado é bem-sucedido ou um resultado de erro contendo um status.
* `@param {T} result`

#### onFailure(resultado: T); {#on-failure-result}

* Falha no retorno de chamada chamado pelo SDK quando a solicitação da API pré-autorizada não pôde ser atendida.
* O resultado é um resultado de falha contendo um status.
* `@param {T} result`

### classe PreauthorizeResponse {#preauth-response-class}

#### status público: Status; {#public-status}

* Retorna: informações adicionais de status (estado) em caso de falha.
* Pode conter um valor `null`.

#### decisões públicas: Decisão[]; {#public-decisions}

* Retorna: a lista de decisões de pré-autorização. Uma decisão para cada recurso.
* A lista pode estar vazia em caso de falha.

### Status da classe {#class-status}

#### estatuto público: número; {#public-status-numbr}

* O código de status de resposta HTTP conforme documentado na RFC 7231.
* Pode ser 0 caso `Status` venha da SDK em vez dos serviços de Autenticação da Adobe Pass.

#### código público: número; {#public-code-numbr}

* O código de erro padrão dos serviços de autenticação da Adobe Pass.
* Pode conter uma cadeia de caracteres vazia ou um valor `null`.

#### mensagem pública: cadeia de caracteres; {#public-msg-string}

* A mensagem detalhada que, em alguns casos, é fornecida pelos endpoints de autorização do MVPD ou pelas regras de degradação do programador.
* Pode conter uma cadeia de caracteres vazia ou um valor `null`.

#### detalhes públicos: sequência de caracteres; {#public-details-strng}

* Contém uma mensagem detalhada que, em alguns casos, é fornecida pelos endpoints de autorização do MVPD ou pelas regras de degradação do Programador.
* Pode conter uma cadeia de caracteres vazia ou um valor `null`.


#### public helpUrl: string; {#public-help-url-string}

* O URL que vincula mais informações sobre por que esse estado/erro ocorreu e possíveis soluções.
* Pode conter uma cadeia de caracteres vazia ou um valor `null`.

#### rastro público: cadeia de caracteres; {#public-trace-string}

* O identificador exclusivo para essa resposta, que pode ser usado ao entrar em contato com o suporte para identificar problemas específicos em cenários mais complexos.
* Pode conter uma cadeia de caracteres vazia ou um valor `null`.

#### ação pública: sequência de caracteres; {#public-action-string}

* A ação recomendada para corrigir a situação.
  * **nenhum**: infelizmente não há nenhuma ação predefinida para corrigir esse problema. Isso pode indicar uma invocação incorreta da API pública
  * **configuração**: é necessária uma alteração na configuração por meio do painel TVE ou contatando o suporte.
  * **application-registration**: o aplicativo deve se registrar novamente.
  * **autenticação**: o usuário deve autenticar ou autenticar novamente.
  * **autorização**: o usuário deve obter autorização para o recurso específico.
  * **degradação**: alguma forma de degradação deve ser aplicada.
  * **repetir**: tentar novamente a solicitação pode resolver o problema
  * **repetir-após**: tentar novamente a solicitação após o período indicado pode resolver o problema.
* Pode conter uma cadeia de caracteres vazia ou um valor `null`.

### Decisão de classe {#class-decision}

#### public id: string; {#public-id-string}

* A ID do recurso para a qual a decisão foi obtida.

#### público autorizado: booleano; {#public-auth-boolean}

* O valor do sinalizador que indica se a decisão foi bem-sucedida ou não.

#### erro público: Status; {#public-error-status}

* Informações adicionais de status (estado) caso ocorra algum erro. Pode conter um valor `null`.

## Exemplo de implementação de cliente {#client-imp-example}

```JavaScript
let accessEnablerApi = new window.AccessEnabler.AccessEnabler("software statement");
let accessEnablerModels = window.AccessEnabler.models;



// Build request
let requestBuilder = new accessEnablerModels.PreauthorizeRequest.getBuilder();
let request = requestBuilder
    .setResources(["RES01", "RES02", "RES03"])
    .disableFeatures("LOCAL_CACHE")
    .build();



// Create callback
let callback = {
    onResponse(response) {
        // Handle onResponse
    },
    onFailure(response) {
        // Handle onFailure
    }
};

// Invoke call
accessEnablerApi.preauthorize(request, callback);
```


## Exemplos de cenário {#scenario-examples}

### Cenário 1: todos os recursos solicitados foram autorizados {#all-req-res-auth}

<table>
<thead>
  <tr>
    <th>Sinalizador de código de erro aprimorado</th>
    <th>Resposta</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Desabilitado</td>
    <td>

```JavaScript
        {
    "decisions": [
        {
        "id": "RES01",
        "authorized": true
        },
        {
        "id": "RES02",
        "authorized": true
        },
        {
        "id": "RES03",
        "authorized": true
        }
    ]
    }    
```

</td>
  </tr>
</tbody>


### Cenário 2: alguns recursos solicitados foram autorizados. {#sm-req-res-auth}

<table>
<thead>
  <tr>
    <th>Sinalizador de código de erro aprimorado</th>
    <th>Resposta</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Desabilitado</td>
    <td>

```JavaScript
        {
    "decisions": [
        {
        "id": "RES01",
        "authorized": true
        },
        {
        "id": "RES02",
        "authorized": false
        },
        {
        "id": "RES03",
        "authorized": true
        }
    ]
    }
       
```

</td>
  </tr>

<tr>
    <td>Ativado</td>
    <td>

```JavaScript
    {
      "decisions": [
        {
        "id": "RES01",
        "authorized": true
        },
        {
        "id": "RES02",
        "authorized": false,
        "error": {
            "status": 403,
            "code": "preauthorization_denied_by_mvpd",
            "message": "The MVPD has returned a \"Deny\" decision when requesting pre-authorization for the specified resource.",
            "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
            "action": "none"
        }
        },
        {
        "id": "RES03",
        "authorized": true
        },
    ]
    }
    
```

</td>
  </tr>
</tbody>


### Cenário 3: nenhum dos recursos solicitados foi autorizado. {#none-req-res-auth}

<table>
<thead>
  <tr>
    <th>Sinalizador de código de erro aprimorado</th>
    <th>Resposta</th>
  </tr>
</thead>
<tbody>
<tr>
    <td>Desabilitado</td>
    <td>

```JavaScript
        {
    "decisions": [
        {
        "id": "RES01",
        "authorized": false
        },
        {
        "id": "RES02",
        "authorized": false
        },
        {
        "id": "RES03",
        "authorized": false
        }
    ]
    }
       
```

</td>
  </tr>

<tr>
    <td>Ativado</td>
    <td>

```JavaScript
    {
    "decisions": [
        {
        "id": "RES01",
        "authorized": false,
        "error": {
            "status": 403,
            "code": "preauthorization_denied_by_mvpd",
            "message": "The MVPD has returned a \"Deny\" decision when requesting pre-authorization for the specified resource.",
            "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
            "action": "none"
            }
        },
        {
            "id": "RES02",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "preauthorization_denied_by_mvpd",
                "message": "The MVPD has returned a \"Deny\" decision when requesting pre-authorization for the specified resource.",
                "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
                "action": "none"
            }
        },
        {
        "id": "RES03",
        "authorized": false,
        "error": {
            "status": 403,
            "code": "maximum_execution_time_exceeded",
            "message": "The request did not complete in the maximum allowed time. Retrying the request might solve the issue.",
            "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
            "action": "retry"
                }
            }
        ]
    }
    
```

</td>
  </tr>
</tbody>


### Cenário 4: solicitação do cliente incorreta - nenhum recurso especificado. {#bad-cl-req-no-res-sp}

<table>
<thead>
  <tr>
    <th>Sinalizador de código de erro aprimorado</th>
    <th>Resposta</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Desativado/Ativado</td>
    <td>

```JavaScript
    {
    "status": {
    "status": 400,
    "code": "internal_error",
    "message": "The request failed due to an internal error.",
    "details": "Required String[] parameter 'resource' is not present",
    "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
    "action": "none"
    },
    "decisions": []
    }
```

</td>
  </tr>
</tbody>
</table>

### Cenário 5: solicitação de cliente incorreta - recursos vazios especificados. {#bad-cl-req-empt-res-sp}

<table>
<thead>
  <tr>
    <th>Sinalizador de código de erro aprimorado</th>
    <th>Resposta</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Desativado/Ativado</td>
    <td>

```JavaScript
    {
    "status": {
    "status": 412,
    "code": "missing_resource",
    "message": "The resource parameter is missing",
    "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
    "action": "none"
    },
    "decisions": []
    }
```

</td>
  </tr>
</tbody>
</table>

### Cenário 6: erro de rede. {#ntwrk-error}

<table>
<thead>
  <tr>
    <th>Sinalizador de código de erro aprimorado</th>
    <th>Resposta</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Ativado</td>
    <td>

```JavaScript
    {
    "decisions": [
        {
        "id": "RES01",
        "authorized": false,
        "error": {
            "status": 403,
            "code": "network_received_error",
            "message": "There was a read error while retrieving the response from the associated partner service. Retrying the request might solve the issue.",
            "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
            "action": "retry"
            }
        },
        {
            "id": "RES02",
            "authorized": false,
            "error": {
                "status": 403,
                "code": "network_received_error",
                "message": "There was a read error while retrieving the response from the associated partner service. Retrying the request might solve the issue.",
                "helpUrl": "https://experienceleague.adobe.com/docs/primetime/authentication/home.html",
                "action": "retry"
                }   
        }
    ]
    }
```

</td>
  </tr>
</tbody>
</table>

### Cenário 7: o fluxo de pré-autorização foi chamado sem uma sessão AuthN válida.

<table>
<thead>
  <tr>
    <th>Sinalizador de código de erro aprimorado</th>
    <th>Resposta</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Desativado/Ativado</td>
    <td>

```JavaScript
    {
    "status": {
    "status": 0,
    "code": "authentication_session_missing",
    "message": "The authentication session associated with this request could not be retrieved. The user must re-authenticate with a supported MVPD in order to continue.",
    "action": "authentication"
    },
    "decisions": []
    }
```

</td>
  </tr>
</tbody>
</table>



### Cenário 8: o fluxo de pré-autorização foi chamado antes da chamada setRequestor ser concluída

<table>
<thead>
  <tr>
    <th>Sinalizador de código de erro aprimorado</th>
    <th>Resposta</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>Desativado/Ativado</td>
    <td>

```JavaScript
    {
    "status": {
    "status": 0,
    "code": "requestor_not_configured",
    "message": "The requestor is not yet configured which is a prerequisite for using any API apart from the setRequestor API.",
    "action": "retry"
    },
    "decisions": []
    }
```

</td>
  </tr>
</tbody>
</table>
