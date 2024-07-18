---
title: Iniciar autorização
description: Iniciar autorização
exl-id: 2f8a5499-e94f-40dd-9fb0-aac8e080de66
source-git-commit: 1ad2a4e75cd64755ccbde8f3b208148b7d990d82
workflow-type: tm+mt
source-wordcount: '423'
ht-degree: 0%

---

# Iniciar autorização {#initiate-authorization}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!NOTE]
>
> A implementação da REST API é limitada por [Mecanismo de limitação](/help/authentication/throttling-mechanism.md)

## Endpoints da REST API {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Preparo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Preparo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descrição {#description}

Obtém a resposta de autorização.

| Endpoint | Chamado </br>por | Entrada   </br>Parâmetros | HTTP </br>Método | Resposta | Resposta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authorize | Aplicativo de Streaming</br></br>ou</br></br>Serviço de Programador | 1. solicitante (obrigatório)</br>2.  deviceId (Obrigatório)</br>3.  recurso (obrigatório)</br>4.  device_info/X-Device-Info (Obrigatório)</br>5.  _deviceType_</br> 6.  _deviceUser_ (Obsoleto)</br>7.  _appId_ (obsoleto)</br>8.  parâmetros extras (opcional) | GET | XML ou JSON que contém detalhes de autorização ou detalhes de erro, se malsucedido. Consulte os exemplos abaixo. | 200 - Êxito </br>403 - Sem Êxito |

{style="table-layout:auto"}

</br>


| Parâmetro de entrada | Descrição |
| --- | --- |
| solicitante | O requestorId do Programador para o qual esta operação é válida. |
| deviceId | Os bytes de id do dispositivo. |
| recurso | Uma cadeia de caracteres que contém um resourceId (ou fragmento MRSS), identifica o conteúdo solicitado por um usuário e é reconhecida por pontos de extremidade de autorização MVPD. |
| device_info/</br></br>X-Device-Info | Informações do dispositivo de transmissão.</br></br>**Observação**: isso PODE ser passado para device_info como um parâmetro de URL, mas devido ao tamanho potencial desse parâmetro e às limitações no comprimento de uma URL GET, DEVE ser passado como X-Device-Info no cabeçalho http. </br></br>Veja os detalhes completos em [Passando Informações sobre Dispositivo e Conexão](/help/authentication/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | O tipo de dispositivo (por exemplo, Roku, PC).</br></br>Se este parâmetro estiver definido corretamente, o ESM oferecerá métricas que são [analisadas por tipo de dispositivo](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) ao usar o sem cliente, para que diferentes tipos de análise possam ser executados para, por exemplo, Roku, Apple TV, Xbox etc.</br></br>Consulte [Vantagens do parâmetro de tipo de dispositivo sem cliente em métricas de passagem ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Observação**: device_info substituirá esse parâmetro. |
| _deviceUser_ | O identificador do usuário do dispositivo. |
| _appId_ | O id/nome do aplicativo. </br></br>**Observação**: device_info substitui este parâmetro. |
| parâmetros extras | A chamada também pode conter parâmetros opcionais que habilitam outras funcionalidades como:</br></br>* generic_data - habilita o uso de [Promotional TempPass](/help/authentication/promotional-temp-pass.md)</br></br>Exemplo: `generic_data=("email":"email@domain.com")` |

{style="table-layout:auto"}

>[!CAUTION]
>
>**Endereço IP do Dispositivo de Streaming**</br>
>Para implementações de Cliente para servidor, o Endereço IP do dispositivo de transmissão é implicitamente enviado com esta chamada.  Para implementações de Servidor para Servidor, em que a chamada **regcode** é feita pelo Serviço do Programador e não pelo Dispositivo de Streaming, o seguinte cabeçalho é necessário para passar o Endereço IP do Dispositivo de Streaming:</br></br>
>
>```
>X-Forwarded-For : <streaming\_device\_ip>
>```
>
>onde `<streaming\_device\_ip>` é o endereço IP público do Dispositivo de Streaming.</br></br>
>Exemplo: </br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1
>X-Forwarded-For:203.45.101.20
>```
>


### Exemplo de resposta {#sample-response}

* **Caso 1: Sucesso**
</br>
  * **XML:**
  </br>

    &quot;XML
    &lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;yes&quot;?>
    &lt;authorization>
    &lt;expires>1348148289000&lt;/expires>
    &lt;mvpd>sampleMvpdId&lt;/mvpd>
    &lt;requestor>sampleRequestorId&lt;/requestor>
    &lt;resource>sampleResourceId&lt;/requestor>
    &lt;/authorization>
    &quot;



* **JSON:**

  ```JSON
  {
    "mvpd": "sampleMvpdId",
    "resource": "sampleResourceId",
    "requestor": "sampleRequestorId",
    "expires": "1348148289000"
  }
  ```

>[!IMPORTANT]
>
>Quando a resposta vem de um MVPD de Proxy, ela pode incluir um elemento adicional chamado `proxyMvpd`.



* **Caso 2: autorização negada**


  ```JSON
  <error>
    <status>403</status>
    <message>User not authorized</message>
    <details>Your subscription package does not include the "ASFAFD" channel.
    Please go to http://www.ca.ble/upgrade in order to upgrade your subscription.</details>
  </error>
  ```
