---
title: Recuperar token de autenticação
description: Recuperar token de autenticação
exl-id: 7fb03854-edad-41e7-b218-1858fc071876
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 0%

---

# (Herdado) Recuperar token de autenticação {#retrieve-authentication-token}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

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

Recupera o token de autenticação (AuthN).

| Endpoint | Chamado </br>por | Entrada   </br>Parâmetros | HTTP </br>Método | Resposta | Resposta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authn</br></br>Por exemplo:</br></br>&lt;SP_FQDN>/api/v1/tokens/authn | Aplicativo de Streaming</br></br>ou</br></br>Serviço de Programador | 1. solicitante (obrigatório)</br>2.  deviceId (Obrigatório)</br>3.  device_info/X-Device-Info (Obrigatório)</br>4.  _deviceType_ (obsoleto)</br>5.  _deviceUser_ (Obsoleto)</br>6.  _appId_ (obsoleto) | GET | XML ou JSON contendo informações de autenticação ou detalhes de erro se malsucedido. | 200 - Sucesso.  </br>404 - Token Não Encontrado </br>410 - Token expirado |

{style="table-layout:auto"}


| Parâmetro de entrada | Descrição |
| --- |------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| solicitante | O requestorId do Programador para o qual esta operação é válida. |
| deviceId | Os bytes de id do dispositivo. |
| device_info/</br></br>X-Device-Info | Informações do dispositivo de transmissão.</br></br>**Observação**: isso PODE ser passado para device_info como um parâmetro de URL, mas devido ao tamanho potencial desse parâmetro e às limitações no comprimento de uma URL GET, DEVE ser passado como X-Device-Info no cabeçalho http. </br></br>Veja os detalhes completos em [Passando Informações sobre Dispositivo e Conexão](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | O tipo de dispositivo (por exemplo, Roku, PC).</br></br>**Observação**: device_info substitui este parâmetro. |
| _deviceUser_ | O identificador do usuário do dispositivo.</br></br>**Observação**: se usado, deviceUser deve ter os mesmos valores que na solicitação [Criar código de registro](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| _appId_ | O id/nome do aplicativo. </br></br>**Observação**: device_info substitui este parâmetro. Se usado, `appId` deve ter os mesmos valores que na solicitação [Criar código de registro](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |

{style="table-layout:auto"}

</br>

### Exemplo de resposta {#response}



#### Sucesso

**XML:**

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <authentication>
         <expires>1601114932000</expires>
         <userId>sampleUserId</userId>
         <mvpd>sampleMvpdId</mvpd>
         <requestor>sampleRequestor</requestor>
    </authentication>
```


**JSON:**

```JSON
    {
         "requestor": "sampleRequestor",
         "mvpd": "sampleMvpdId",
         "userId": "sampleUserId",
         "expires": "1601114932000"
    }
```





#### Token de autenticação não encontrado:

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
        "message": "Not Found"
    }
```
