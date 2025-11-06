---
title: Recuperar token de autorização
description: Recuperar token de autorização
exl-id: 0b010958-efa8-4dd9-b11b-5d10f51f5680
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '373'
ht-degree: 0%

---

# (Herdado) Recuperar token de autorização {#retrieve-authorization-token}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

>[!NOTE]
>
> A implementação da REST API é limitada por [Mecanismo de limitação](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## Endpoints da REST API {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Preparo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Preparo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descrição {#description}

Recupera o token de autorização (AuthZ).


| Endpoint | Chamado </br>por | Entrada   </br>Parâmetros | HTTP </br>Método | Resposta | Resposta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authz</br></br>Por exemplo:</br></br>&lt;SP_FQDN>/api/v1/tokens/authz | Aplicativo de Streaming</br></br>ou</br></br>Serviço de Programador | &#x200B;1. solicitante (obrigatório)</br>2.  deviceId (Obrigatório)</br>3.  recurso (obrigatório)</br>4.  device_info/X-Device-Info (Obrigatório)</br>5.  _deviceType_</br> 6.  _deviceUser_ (Obsoleto)</br>7.  _appId_ (obsoleto) | GET | &#x200B;1. Êxito</br>2.  Token de autenticação </br>    não encontrado ou expirado:   </br>    Motivo explicativo XML </br>    token de autenticação não encontrado</br>3.  Token de autorização </br>    não encontrado: </br>    Explicação XML</br>4.  Token de autorização </br>    expirado: </br>    Explicação de XML | 200 - Êxito </br>412 - Sem AuthN</br></br>404 - Sem AuthZ</br></br>410 - AuthZ Expirado |

{style="table-layout:auto"}

</br>

| Parâmetro de entrada | Descrição |
| --- | --- |
| solicitante | O requestorId do Programador para o qual esta operação é válida. |
| deviceId | Os bytes de id do dispositivo. |
| recurso | Uma string que contém um resourceId (ou fragmento MRSS), identifica o conteúdo solicitado por um usuário e é reconhecida pelos endpoints de autorização do MVPD. |
| device_info/</br></br>X-Device-Info | Informações do dispositivo de transmissão.</br></br>**Observação**: isso PODE ser passado para device_info como um parâmetro de URL, mas devido ao tamanho potencial desse parâmetro e às limitações no comprimento de uma URL GET, ELE DEVE ser passado como X-Device-Info no cabeçalho http. </br></br>Veja os detalhes completos em [Passando Informações sobre Dispositivo e Conexão](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | O tipo de dispositivo (por exemplo, Roku, PC).</br></br>Se este parâmetro estiver definido corretamente, o ESM oferecerá métricas que são [analisadas por tipo de dispositivo](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) ao usar o sem cliente, para que diferentes tipos de análise possam ser executados, por exemplo, Roku, Apple TV e Xbox.</br></br>Consulte, [Vantagens de usar o parâmetro de tipo de dispositivo sem cliente em métricas de passagem ](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Observação**: device_info substituirá esse parâmetro. |
| _deviceUser_ | O identificador do usuário do dispositivo. |
| _appId_ | O id/nome do aplicativo. </br></br>**Observação**: device_info substitui este parâmetro. |

{style="table-layout:auto"}


### Exemplo de resposta {#response}



#### Sucesso

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authorization>
        <expires>1348148289000</expires>
        <mvpd>sampleMvpdId</mvpd>
        <requestor>sampleRequestorId</requestor>
        <resource>sampleResourceId</resource>
        <proxyMvpd>sampleProxyMvpdId</proxyMvpd>
    </authorization>
```



**JSON:**

```JSON
    {
        "mvpd": "sampleMvpdId",
        "resource": "sampleResourceId",
        "requestor": "sampleRequestorId",
        "expires": "1348148289000",
        "proxyMvpd": "sampleProxyMvpdId"
    }
```

</br>


#### Token de autenticação não encontrado ou expirado:

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>412</status>
        <message>User not authenticated</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 412,
        "message": "User not authenticated",
        "details": null
    }
```

</br>


#### Token de autorização não encontrado:

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>404</status>
        <message>Not found</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 404,
        "message": "Not Found",
        "details": null
    }
```

</br>



#### Token de autorização expirado:

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <error>
        <status>410</status>
        <message>Gone</message>
    </error>
```



**JSON:**

```JSON
    {
        "status": 410,
        "message": "Gone",
        "details": null
    }
```
