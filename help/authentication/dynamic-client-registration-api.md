---
title: API de registro dinâmico do cliente
description: API de registro dinâmico do cliente
exl-id: 06a76c71-bb19-4115-84bc-3d86ebcb60f3
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '904'
ht-degree: 0%

---

# API de registro dinâmico do cliente {#dynamic-client-registration-api}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview}

Atualmente, há duas maneiras pelas quais a Autenticação Adobe Pass identifica e registra aplicativos:

* os clientes baseados em navegador são registrados por meio da [lista de domínios](/help/authentication/programmer-overview.md) permitida
* os clientes de aplicativos nativos, como os aplicativos iOS e Android, são registrados por meio do mecanismo solicitante assinado.

A Autenticação Adobe Pass propõe um novo mecanismo para registrar aplicativos. Esse mecanismo é descrito nos parágrafos seguintes.

## O mecanismo de registro do aplicativo {#appRegistrationMechanism}

### Motivos técnicos {#reasons}

O mecanismo de autenticação na Autenticação do Adobe Pass dependia de cookies de sessão, mas devido às [Guias Personalizadas do Android Chrome](https://developer.chrome.com/multidevice/android/customtabs){target=_blank} e ao [Controlador de Exibição do Apple Safari](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller){target=_blank}, essa meta não pode mais ser alcançada.

Dadas essas limitações, o Adobe introduz um novo mecanismo de registro para todos os seus clientes. É baseado no OAuth 2.0 RFC e consiste em
das seguintes etapas:

1. Recuperando a declaração de software da TVE
Painel
1. Obter credenciais do cliente
1. Obter o token de acesso

### Recuperando a Declaração de Software do Painel TVE {#softwareStatement}

Para cada aplicativo lançado, é necessário obter uma instrução de software. Todas as instruções de software são fornecidas através do TVE Dashboard, uma vez que os aplicativos são criados. A instrução de software deve ser implantada junto com o aplicativo no dispositivo do usuário.

>[!IMPORTANT]
>
>Ao usar uma declaração de software, o mecanismo de ID do solicitante assinado não será mais necessário.

Para obter mais detalhes sobre como criar instruções de software, visite [Registro de Cliente no Painel do TVE](/help/authentication/dynamic-client-registration.md).

### Obtendo credenciais do cliente {#clientCredentials}

Depois de recuperar uma declaração de software do Painel do TVE, é necessário registrar seu aplicativo no servidor de autorização do Adobe Pass. Faça isso executando uma chamada /register e recuperando o identificador de cliente exclusivo.

**Solicitação**

| Chamada HTTP |                    |
|-----------|--------------------|
| caminho | /o/client/register |
| método | POST |

| campos |                                                                           |           |
|--------------------|---------------------------------------------------------------------------|-----------|
| declaração_do_software | A declaração de software criada no Painel TVE. | obrigatório |
| redirect_uri | O URI que o aplicativo usa para concluir o fluxo de autenticação. | opcional |

| cabeçalhos de solicitação |                                                                                |           |
|-----------------|--------------------------------------------------------------------------------|-----------|
| Tipo de conteúdo | application/json | obrigatório |
| X-Device-Info | As informações do dispositivo conforme definidas em Passing Device and Connection Information | obrigatório |
| User-Agent | O agente do usuário | obrigatório |

**Resposta**

| cabeçalhos de resposta |                  |           |
|------------------|------------------|-----------|
| Tipo de conteúdo | application/json | obrigatório |

| campos de resposta |                 |                            |
|---------------------|-----------------|----------------------------|
| client_id | String | obrigatório |
| client_secret | String | obrigatório |
| client_id_issue_at | long | obrigatório |
| redirect_uris | lista de strings | obrigatório |
| grant_types | lista de Cadeias de Caracteres<br/> **valor aceito**<br/> `client_credentials`: usado por clientes inseguros, como o Android SDK. | obrigatório |
| erro | **valores aceitos**<ul><li>invalid_request</li><li>invalid_redirect_uri</li><li>instrução_de_software_inválida</li><li>unapproved_software_statement</li></ul> | obrigatório em um fluxo de erro |


#### Resposta de erro {#error-response}

Em caso de erro, o servidor de registro responde com um código de status HTTP 400 (Solicitação inválida) e inclui os seguintes parâmetros na resposta:

| código de status | corpo da resposta | descrição |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_request&quot;} | A solicitação não tem um parâmetro obrigatório, inclui um valor de parâmetro não compatível, repete um parâmetro ou está malformada de outra forma. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_redirect_uri&quot;} | O redirect_uri não é permitido para este cliente com base em seu aplicativo registrado. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_software_statement&quot;} | A declaração de software não é válida. |
| HTTP 400 | {&quot;error&quot;: &quot;unapproved_software_statement&quot;} | Software_id não encontrado na configuração. |

#### Exemplo de credenciais do cliente {#client-credentials-example}

**Solicitação:**

```HTTPS
POST /o/client/register HTTP/1.1
    User-Agent: Android
    X-Device-Info: ew0KICAibW9kZWwiOiAiVFYiLA0KICAidmVuZG9yIjogIkFwcGxlIiwNCiAgIm1hbnVmYWN0dXJlciI6ICJBcHBsZSIsDQogICJvc05hbWUiOiAidHZPUyIsDQogICJvc1ZlbmRvciI6ICJBcHBsZSIsDQogICJvc1ZlcnNpb24iOiAiMTAuMiIsDQogICJicm93c2VyVmVuZG9yIjogIkFwcGxlIiwNCiAgImJyb3dzZXJOYW1lIjogIlNhZmFyaSINCn0
    Content-Type: application/json
    Accept: application/json
    Host: sp.auth.adobe.com
 {
        "software_statement": "eyJhbGciOiJSUzI1NiJ9.
    eyJzb2Z0d2FyZV9pZCI6IjROUkIxLTBYWkFCWkk5RTYtNVNNM1IiLCJjbGll
    bnRfbmFtZSI6IkV4YW1wbGUgU3RhdGVtZW50LWJhc2VkIENsaWVudCIsImNs
    aWVudF91cmkiOiJodHRwczovL2NsaWVudC5leGFtcGxlLm5ldC8ifQ.
    GHfL4QNIrQwL18BSRdE595T9jbzqa06R9BT8w409x9oIcKaZo_mt15riEXHa
    zdISUvDIZhtiyNrSHQ8K4TvqWxH6uJgcmoodZdPwmWRIEYbQDLqPNxREtYn0
    5X3AR7ia4FRjQ2ojZjk5fJqJdQ-JcfxyhK-P8BAWBd6I2LLA77IG32xtbhxY
    fHX7VhuU5ProJO8uvu3Ayv4XRhLZJY4yKfmyjiiKiPNe-Ia4SMy_d_QSWxsk
    U5XIQl5Sa2YRPMbDRXttm2TfnZM1xx70DoYi8g6czz-CPGRi4SW_S2RKHIJf
    IjoI3zTJ0Y2oe0_EJAiXbL6OyF9S5tKxDXV8JIndSA",
  "redirect_uri": "adobepass://com.programmer"  }
```

**Resposta:**

```HTTPS
HTTP/1.1 201 Created
Content-Type: application/json
Cache-Control: no-store
Pragma: no-cache
    
{
 "client_id": "s6BhdRkqt3",
 "client_secret": "t7AkePiru4",
 "client_id_issued_at": 2893256800,
 "redirect_uris": [
   "app://com.programmer.adobe#sdasdsadas"],
 "grant_types": ["client_credentials"]
}
```

**Resposta de erro:**

```HTTPS
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error": "invalid_request" }
```

### Obter o token de acesso {#accessToken}

Depois de recuperar o identificador exclusivo do cliente (ID do cliente e segredo do cliente) para o aplicativo, é necessário obter um token de acesso. O token de acesso é um token OAuth 2.0 obrigatório, usado para chamar as APIs de autenticação da Adobe Pass.

>[!NOTE]
>
>Atualmente, os tokens de acesso têm um tempo de vida de 24 horas.

**Solicitação**


| **Chamada HTTP** | |
| --- | --- |
| caminho | `/o/client/token` |
| método | POST |

| **solicitar parâmetros** | |
| --- | --- |
| `grant_type` | Recebido no processo de registro de cliente.<br/> **Valor aceito**<br/>`client_credentials`: usado para clientes inseguros, como o SDK do Android. |
| `client_id` | Identificador de cliente obtido no processo de registro do cliente. |
| `client_secret` | Identificador de cliente obtido no processo de registro do cliente. |

**Resposta**

| campos de resposta | | |
| --- | --- | --- |
| `access_token` | O valor do token de acesso que você deve usar para chamar as APIs do Adobe Pass | obrigatório |
| `expires_in` | O tempo em segundos até o access_token expirar | obrigatório |
| `token_type` | O tipo do token **portador** | obrigatório |
| `created_at` | A hora de emissão do token | obrigatório |
| **cabeçalhos de resposta** | | |
| `Content-Type` | application/json | obrigatório |

**Resposta de erro**

Em caso de erro, o servidor de autorização responde com um código de status HTTP 400 (Solicitação inválida) e inclui os seguintes parâmetros na resposta:

| código de status | corpo da resposta | descrição |
| --- | --- | --- |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_request&quot;} | A solicitação não tem um parâmetro obrigatório, inclui um valor de parâmetro sem suporte (diferente do tipo de concessão), repete um parâmetro, inclui várias credenciais, usa mais de um mecanismo para autenticar o cliente ou está malformada. |
| HTTP 400 | {&quot;error&quot;: &quot;invalid_client&quot;} | Falha na autenticação do cliente porque o cliente era desconhecido. O SDK DEVE se registrar no servidor de autorização novamente. |
| HTTP 400 | {&quot;error&quot;: &quot;unauthorized_client&quot;} | O cliente autenticado não está autorizado a usar este tipo de concessão de autorização. |

#### Obtendo Exemplo De Token De Acesso: {#obt-access-token}

**Solicitação:**

```HTTPS
POST o/client/token HTTP/1.1
Content-Type: application/x-www-form-urlencoded
    
grant_type=client_credentials&client_id=AAA&client_secret=SSS
```

**Resposta:**

```JSON
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{
  "access_token":"2YotnFZFEjr1zCsicMWpAA",
  "token_type":"bearer",
  "expires_in":3600,
  "created_at":123456789
}
```

**Resposta de erro:**

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error": "invalid_request" }
```

## Executando solicitações de autenticação {#autheticationRequests}

Use o token de acesso para executar [Chamadas de API de autenticação](/help/authentication/initiate-authentication.md) do Adobe Pass. Para fazer isso, o token de acesso precisa ser adicionado à solicitação de API de uma das seguintes maneiras:

* adicionando um novo parâmetro de consulta à solicitação. Esse novo parâmetro é chamado **access_token**.

* adicionando um novo cabeçalho HTTP à solicitação: Autorização: Portador. Recomendamos que você use o cabeçalho HTTP, já que as cadeias de caracteres de consulta tendem a estar visíveis nos logs do servidor.

Em caso de erro, as seguintes respostas de erro podem ser retornadas:

| Respostas de erro |     |                                                                                                        |
|-----------------|-----|--------------------------------------------------------------------------------------------------------|
| invalid_request | 400 | A solicitação está malformada. |
| invalid_client | 403 | A ID do cliente não tem mais permissão para executar solicitações. As novas credenciais do cliente devem ser geradas. |
| access_denied | 401 | O access_token não é válido (expirado ou inválido). O cliente DEVE solicitar um novo access_token. |

### Exemplos de execução de solicitações de autenticação:

**Enviando token de acesso como parâmetro de solicitação:**

```HTTPS
GET adobe-services/config?access_token=<access_token>&requestor_id=... HTTP/1.1
    
Host: sp.auth.adobe.com
```

**Enviando token de acesso como cabeçalho HTTP:**

```HTTPS
POST adobe-services/sessionDevice?device_id=platformDeviceId HTTP/1.1
    
Authorization: Bearer <access_token>
    
Host: sp.auth.adobe.com
```

**Respostas de erro como corpo da resposta:**

```HTTPS
HTTP/1.1 401 Unauthorized
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache
    
{ "error":"invalid_client" }
```

**Respostas de erro como parâmetros de URL:**

```HTTPS
HTTP/1.1 302 Found
    
Location: adobepass://com.programmer.adobe?error=access_denied
```
