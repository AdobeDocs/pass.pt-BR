---
title: Recuperar token de autenticação
description: Recuperar token de autenticação
exl-id: 7fb03854-edad-41e7-b218-1858fc071876
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# Recuperar token de autenticação {#retrieve-authentication-token}

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

Recupera o token de autenticação (AuthN).

| Endpoint | Chamado  </br>Por | Entrada   </br>Params | HTTP  </br>Método | Resposta | HTTP  </br>Resposta |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/tokens/authn</br></br>Por exemplo:</br></br>&lt;sp_fqdn>/api/v1/tokens/authn | Aplicativo de transmissão</br></br>ou</br></br>Serviço de programador | 1. solicitante (obrigatório)</br>2.  deviceId (Obrigatório)</br>3.  device_info/X-Device-Info (Obrigatório)</br>4.  _deviceType_ (Obsoleto)</br>5.  _deviceUser_ (Obsoleto)</br>6.  _appId_ (Obsoleto) | GET | XML ou JSON contendo informações de autenticação ou detalhes de erro se malsucedido. | 200 - Sucesso.  </br>404 - Token Não Encontrado  </br>410 - Token expirado |

{style="table-layout:auto"}


| Parâmetro de entrada | Descrição |
| --- | --- |
| solicitante | O requestorId do Programador para o qual esta operação é válida. |
| deviceId | Os bytes de id do dispositivo. |
| device_info/</br></br>X-Device-Info | Informações do dispositivo de transmissão.</br></br>**Nota**: isso PODE ser passado para device_info como um parâmetro de URL, mas devido ao tamanho potencial desse parâmetro e às limitações no comprimento de um URL do GET, ele DEVE ser passado como X-Device-Info no cabeçalho http. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | O tipo de dispositivo (por exemplo, Roku, PC).</br></br>**Nota**: device_info substitui esse parâmetro. |
| _deviceUser_ | O identificador do usuário do dispositivo.</br></br>**Nota**: Se usado, deviceUser deve ter os mesmos valores que no [Criar código de registro](/help/authentication/registration-code-request.md) solicitação. |
| _appId_ | O id/nome do aplicativo. </br></br>**Nota**: device_info substitui esse parâmetro. Se usado, `appId` deve ter os mesmos valores que no [Criar código de registro](/help/authentication/registration-code-request.md) solicitação. |

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
