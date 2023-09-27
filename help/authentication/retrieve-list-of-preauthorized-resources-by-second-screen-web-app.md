---
title: Recuperar lista de recursos pré-autorizados pelo aplicativo web de segunda tela
description: Recuperar lista de recursos pré-autorizados pelo aplicativo web de segunda tela
exl-id: 78eeaf24-4cc1-4523-8298-999c9effdb7a
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '244'
ht-degree: 0%

---

# Recuperar lista de recursos pré-autorizados pelo aplicativo web de segunda tela {#retrieve-list-of-preauthorized-resources-by-second-screen-web-app}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

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

Há dois conjuntos de APIs: um conjunto para o Aplicativo de streaming ou Serviço de programador e um conjunto para o Aplicativo Web de segunda tela. Esta página descreve a API do aplicativo AuthN.


| Endpoint | Chamado  </br>Por | Entrada   </br>Params | HTTP  </br>Método | Resposta | HTTP  </br>Resposta |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/preauthorize/{código de registro} | Módulo AuthN | 1. código de registro  </br>    (Componente do caminho)</br>2.  solicitante (Obrigatório)</br>3.  lista de recursos (Obrigatório) | GET | XML ou JSON que contém decisões individuais de pré-autorização ou detalhes de erros. Consulte os exemplos abaixo. | 200 - Sucesso</br></br>400 - Solicitação inválida</br></br>401 - Não autorizado</br></br>405 - Método não permitido  </br></br>412 - Falha na pré-condição</br></br>500 - Erro interno do servidor |



| Parâmetro de entrada | Descrição |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| código de registro | O valor do código de registro fornecido pelo usuário no início do fluxo de autenticação. |
| solicitante | O requestorId do Programador para o qual esta operação é válida. |
| lista de recursos | Uma string que contém uma lista delimitada por vírgulas de resourceIds que identifica o conteúdo que pode ser acessível a um usuário e é reconhecida por pontos de extremidade de autorização MVPD. |


### Exemplo de resposta {#sample-response}

**XML:**

```XML
HTTP/1.1 200 OK
Adobe-Request-Id : 7af28ec2-a068-45c2-8009-f5443049baf4`
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
    <resource>
</resources>
```

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
