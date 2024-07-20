---
title: Obter o token do Short Media
description: obter token de mídia curto
exl-id: 667eaaba-423e-4d54-9dbe-084b3c049e1f
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 0%

---

# Obter o token do Short Media {#obtain-short-media-token}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!NOTE]
>
> A implementação da REST API é limitada por [Mecanismo de limitação](/help/authentication/throttling-mechanism.md)

## Endpoints da REST API {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produção - [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* Estágios - [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produção - [`api.auth.adobe.com`](http://api.auth.adobe.com/)
* Estágios - [`api.auth-staging.adobe.com`](http://api.auth-staging.adobe.com/)

</br>

## Descrição {#description}

Obtém O Token De Mídia Curta.

| Endpoint | Chamado </br>por | Entrada   </br>Parâmetros | HTTP </br>Método | Resposta | Resposta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/mediatoken</br></br> ou</br></br>&lt;SP_FQDN>/api/v1/tokens/media</br></br>Por exemplo:</br></br>&lt;SP_FQDN>/api/v1/tokens/media | Aplicativo de Streaming</br></br>ou</br></br>Serviço de Programador | 1. solicitante (obrigatório)</br>2.  deviceId (Obrigatório)</br>3.  recurso (obrigatório)</br>4.  device_info/X-Device-Info (Obrigatório)</br>5.  _deviceType_</br> 6.  _deviceUser_ (Obsoleto)</br>7.  _appId_ (obsoleto) | GET | XML ou JSON contendo um token de mídia codificado na Base64 ou detalhes de erro, se malsucedido. | 200 - Êxito </br>403 - Sem Êxito |

{style="table-layout:auto"}

<!--
| Endpoint | Called  </br>By | Input   </br>Params | HTTP  </br>Method | Response | HTTP  </br>Response |
| --- | --- | --- | --- | --- | --- |
| `<SP_FQDN>/api/v1/mediatoken`</br></br>  or</br></br>`<SP_FQDN>/api/v1/tokens/media`</br></br>For example:</br></br>`<SP_FQDN>/api/v1/tokens/media` | Streaming App</br></br>or</br></br>Programmer Service | <ol><li>requestor (Mandatory)</l><li>deviceId (Mandatory)</li><li>resource (Mandatory)</li><li>device_info/X-Device-Info (Mandatory)</li><li>_deviceType_</li><li>_deviceUser_ (Deprecated)</li><li>_appId_ (Deprecated)</li></ol> | GET | XML or JSON containing an Base64 encoded media token or error details if unsuccessful. | 200 - Success  </br>403 - No Success |
-->

</br>

| Parâmetro de entrada | Descrição |
| --- | --- |
| solicitante | O requestorId do Programador para o qual esta operação é válida. |
| deviceId | Os bytes de id do dispositivo. |
| recurso | Uma cadeia de caracteres que contém um resourceId (ou fragmento MRSS), identifica o conteúdo solicitado por um usuário e é reconhecida por pontos de extremidade de autorização MVPD. |
| device_info/</br></br>X-Device-Info | Informações do dispositivo de transmissão.</br></br>**Observação**: isso PODE ser passado para device_info como um parâmetro de URL, mas devido ao tamanho potencial desse parâmetro e às limitações no comprimento de uma URL GET, DEVE ser passado como X-Device-Info no cabeçalho http. </br></br>Veja os detalhes completos em [Passando Informações sobre Dispositivo e Conexão](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | O tipo de dispositivo (por exemplo, Roku, PC).</br></br>Se este parâmetro for definido corretamente, o ESM oferecerá métricas que são [analisadas por tipo de dispositivo]/(help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) ao usar o Clientless, para que diferentes tipos de análise possam ser executados para. Por exemplo, Roku, Apple TV e Xbox.</br></br>Consulte [Vantagens de usar o parâmetro devicetype sem cliente ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Observação**: device_info substituirá esse parâmetro. |
| _deviceUser_ | O identificador do usuário do dispositivo.</br></br>**Observação**: se usado, deviceUser deve ter os mesmos valores que na solicitação [Criar código de registro](/help/authentication/registration-code-request.md). |
| _appId_ | O id/nome do aplicativo. </br></br>**Observação**: device_info substitui este parâmetro. Se usado, `appId` deve ter os mesmos valores que na solicitação [Criar código de registro](/help/authentication/registration-code-request.md). |

{style="table-layout:auto"}

### Exemplo de resposta {#response}

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <play>
        <expires>1348649569000</expires>
        <mvpdId>sampleMvpdId</mvpdId>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <serializedToken>PHNpZ25hdH[...]</serializedToken>
        <userId>sampleScrambledUserId</userId>
    </play>
```



**JSON:**

```JSON
    {
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348649614000",
        "serializedToken": "PHNpZ25hdH[...]",
        "userId": "sampleScrambledUserId",
        "mvpdId": "sampleMvpdId"
    }
```



### Compatibilidade da biblioteca de verificação de mídia

O campo `serializedToken` da chamada &quot;Obter token de mídia curta&quot; é um token codificado em Base64 que pode ser verificado em relação à Biblioteca de Verificação de Adobe Medium.
