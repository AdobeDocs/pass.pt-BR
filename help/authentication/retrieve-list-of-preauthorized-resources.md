---
title: Recuperar lista de recursos pré-autorizados
description: Recuperar lista de recursos pré-autorizados
exl-id: 3821378c-bab5-4dc9-abd7-328df4b60cc3
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 0%

---

# Recuperar lista de recursos pré-autorizados {#retrieve-list-of-preauthorized-resources}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!NOTE]
>
> A implementação da REST API é limitada por [Mecanismo de limitação](/help/authentication/throttling-mechanism.md)

## Endpoints da REST API {#clientless-endpoints}

&lt;reggie_fqdn>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Estágios - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Estágios - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descrição {#description}

Uma solicitação para que a Autenticação do Adobe Pass obtenha a lista de recursos pré-autorizados.

Há dois conjuntos de APIs: um conjunto para o Aplicativo de streaming ou Serviço de programador e um conjunto para o Aplicativo Web de segunda tela. Esta página descreve a API do aplicativo de streaming ou do serviço de programador.


| Endpoint | Chamado  </br>Por | Entrada   </br>Params | HTTP  </br>Método | Resposta | HTTP  </br>Resposta |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/preauthorize | Aplicativo de transmissão</br></br>ou</br></br>Serviço de programador | 1. solicitante (obrigatório)</br>2.  deviceId (Obrigatório)</br>3.  lista de recursos (Obrigatório)</br>4.  device_info/X-Device-Info (Obrigatório)</br>5.  _deviceType_</br> 6.  _deviceUser_ (Obsoleto)</br>7.  _appId_ (Obsoleto) | GET | XML ou JSON que contém decisões individuais de pré-autorização ou detalhes de erros. Consulte os exemplos abaixo. | 200 - Sucesso</br></br>400 - Solicitação inválida</br></br>401 - Não autorizado</br></br>405 - Método não permitido  </br></br>412 - Falha na pré-condição</br></br>500 - Erro interno do servidor |


| Parâmetro de entrada | Descrição |
| --- | --- |
| solicitante | O requestorId do Programador para o qual esta operação é válida. |
| deviceId | Os bytes de id do dispositivo. |
| lista de recursos | Uma string que contém uma lista delimitada por vírgulas de resourceIds que identifica o conteúdo que pode ser acessível a um usuário e é reconhecida por pontos de extremidade de autorização MVPD. |
| device_info/</br></br>X-Device-Info | Informações do dispositivo de transmissão.</br></br>**Nota**: isso PODE ser passado para device_info como um parâmetro de URL, mas devido ao tamanho potencial desse parâmetro e às limitações no comprimento de um URL do GET, ele DEVE ser passado como X-Device-Info no cabeçalho http. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | O tipo de dispositivo (por exemplo, Roku, PC).</br></br>Se esse parâmetro estiver definido corretamente, o ESM oferecerá métricas que são [detalhado por tipo de dispositivo](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) ao usar sem cliente, para que diferentes tipos de análise possam ser executados, por exemplo, Roku, Apple TV e Xbox.</br></br>Consulte [benefícios de usar o parâmetro de tipo de dispositivo sem cliente nas métricas de passagem ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota**: a variável `device_info` substituirá este parâmetro. |
| _deviceUser_ | O identificador do usuário do dispositivo. |
| _appId_ | O id/nome do aplicativo. </br></br>**Nota**: device_info substitui esse parâmetro. |



### Exemplo de resposta {#sample-response}



**XML:**

```XML
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/xml; charset=utf-8

<resources>
  <resource>
    <id>TestStream1</id>
    <authorized>true</authorized>
  </resource>
  <resource>
    <id>TestStream2</id>
    <authorized>false</authorized>
    <error>
      <status>403</status>
      <code>authorization_denied_by_mvpd</code>
      <message>User not authorized</message>
      <details>Your subscription package does not include the "TestStream3" channel.</details>
      <helpUrl>https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes</helpUrl>
      <trace>0453f8c8-167a-4429-8784-cd32cfeaee58</trace>
      <action>none</action>
    </error>
  </resource>
</resources>
```

</br>

**JSON:**

```JSON
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4
Adobe-Response-Confidence : full
Content-Type: application/json; charset=utf-8
 
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream3",
            "authorized" : false,
            "error" : {
               "status" : 403,
               "code" : "authorization_denied_by_mvpd",
               "message" : "User not authorized",
               "details" : "Your subscription package does not include the "TestStream3" channel.",
               "helpUrl" : "https://experienceleague-review.corp.adobe.com/docs/primetime/authentication/auth-features/error-reportn/enhanced-error-codes.html#error-codes",
               "trace" : "0453f8c8-167a-4429-8784-cd32cfeaee58",
               "action" : "none"
            }
        } 
    ]
}
```
