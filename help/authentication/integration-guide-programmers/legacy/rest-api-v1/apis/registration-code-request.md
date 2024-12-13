---
title: Página de registro
description: Página de registro
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '494'
ht-degree: 1%

---

# Página de registro (herdada) {#registration-page}

## Endpoints da REST API {#clientless-endpoints}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!NOTE]
>
> A implementação da REST API é limitada por [Mecanismo de limitação](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

&lt;REGGIE_FQDN>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Preparo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Preparo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

<br>

## Descrição {#create-reg-code-svc}

Retorna o Código de registro gerado aleatoriamente e o URI da página de logon.

| Endpoint | Chamado <br>por | Entrada   <br>Parâmetro | HTTP <br>Método | Resposta | Resposta HTTP <br> |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestor}/regcode<br>Por exemplo:<br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | Aplicativo de Streaming<br>ou<br>Serviço de Programador | 1. solicitante <br>    (Componente do caminho)<br>2.  deviceId (com hash)   <br>    (Obrigatório)<br>3.  device_info/X-Device-Info (Obrigatório)<br>4.  mvpd (Opcional)<br>5.  ttl (Opcional)<br> | POST | XML ou JSON que contém um código de registro e informações ou detalhes de erro, caso seja malsucedido. Consulte os exemplos abaixo. | 201 |

{style="table-layout:auto"}

| Parâmetro de entrada | Tipo | Descrição |
| --- |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Autorização | Valor do cabeçalho <br>: Portador &lt;access_token> | Token de acesso DCR |
| Aceitar | Cabeçalho <br> Valor: application/json | indicar que tipo de conteúdo o cliente deve entender |
| solicitante | Parâmetro da consulta | O requestorId do Programador para o qual esta operação é válida. |
| deviceId | Parâmetro da consulta | Os bytes de id do dispositivo. |
| device_info/<br>X-Device-Info | device_info: Corpo <br> X-Device-Info: Cabeçalho | Informações do dispositivo de transmissão.<br>**Observação**: isso PODE ser passado para device_info como um parâmetro de URL, mas devido ao tamanho potencial desse parâmetro e às limitações no comprimento de uma URL GET, DEVE ser passado como X-Device-Info no cabeçalho http. <br>Veja os detalhes completos em [Passando Informações sobre Dispositivo e Conexão](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| mvpd | Parâmetro da consulta | A MVPD ID para a qual esta operação é válida. |
| ttl | Parâmetro da consulta | Por quanto tempo esse regcode deve durar em segundos.<br>**Observação**: o valor máximo permitido para ttl é de 36.000 segundos (10 horas). Valores mais altos resultam em uma resposta HTTP 400 (solicitação incorreta). Se `ttl` for deixado em branco, a Autenticação Adobe Pass definirá um valor padrão de 30 minutos. |
| _deviceType_ | Parâmetro da consulta | Obsoleto, não deve ser usado. |
| _deviceUser_ | Parâmetro da consulta | Obsoleto, não deve ser usado. |
| _appId_ | Parâmetro da consulta | Obsoleto, não deve ser usado. |

{style="table-layout:auto"}

>[!CAUTION]
>
>**Endereço IP do Dispositivo de Streaming**
><br>
>Para implementações de Cliente para servidor, o Endereço IP do dispositivo de transmissão é implicitamente enviado com esta chamada.  Para implementações de Servidor para Servidor, em que a chamada **regcode** é feita pelo Serviço de Programador e não pelo Dispositivo de Streaming, o seguinte cabeçalho é necessário para passar o Endereço IP do Dispositivo de Streaming:
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>onde `<streaming\_device\_ip>` é o endereço IP público do Dispositivo de Streaming.
><br><br>
>Exemplo: <br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1<br>X-Forwarded-For:203.45.101.20
>```
>
<br>

### Resposta JSON


#### Código de registro JSON SAMPLES

```JSON
{
  "id": "ef5a79e8-7c8a-41d6-a45a-e378c6c7c8b5",
  "code": "IYQD5JQ",
  "requestor": "sampleRequestorId",
  "mvpd": "sampleMvpdId",
  "generated": 1704963921144,
  "expires": 1704965721144,
  "info": {
    "deviceId": "c28tZGV2aWQtMDAz",
    "deviceInfo": "eyJ0eXBlIjoiU2V0VG9wQm94IiwibW9kZWwiOiJBRlRNTSIsInZlcnNpb24iOnsibWFqb3IiOjAsIm1pbm9yIjowLCJwYXRjaCI6MCwicHJvZmlsZSI6IiJ9LCJoYXJkd2FyZSI6eyJuYW1lIjoiQUZUTU0iLCJ2ZW5kb3IiOiJVbmtub3duIiwidmVyc2lvbiI6eyJtYWpvciI6MCwibWlub3IiOjAsInBhdGNoIjowLCJwcm9maWxlIjoiIn0sIm1hbnVmYWN0dXJlciI6IlJva3UifSwib3BlcmF0aW5nU3lzdGVtIjp7Im5hbWUiOiJBbmRyb2lkIiwiZmFtaWx5IjoiQW5kcm9pZCIsInZlbmRvciI6IkFtYXpvbiIsInZlcnNpb24iOnsibWFqb3IiOjcsIm1pbm9yIjoxLCJwYXRjaCI6MiwicHJvZmlsZSI6IiJ9fSwiYnJvd3NlciI6eyJuYW1lIjoiQ2hyb21lIiwidmVuZG9yIjoiR29vZ2xlIiwidmVyc2lvbiI6eyJtYWpvciI6MTEyLCJtaW5vciI6MCwicGF0Y2giOjU2MTUsInByb2ZpbGUiOiIifSwidXNlckFnZW50IjoiTW96aWxsYS81LjAgKExpbnV4OyBBbmRyb2lkIDcuMS4yOyBBRlRNTSBCdWlsZC9OUzYyOTc7IHd2KSBBcHBsZVdlYktpdC81MzcuMzYgKEtIVE1MLCBsaWtlIEdlY2tvKSBWZXJzaW9uLzQuMCBDaHJvbWUvMTEyLjAuNTYxNS4xOTcgTW9iaWxlIFNhZmFyaS81MzcuMzYgQWRvYmVQYXNzTmF0aXZlRmlyZVRWLzMuMC44Iiwib3JpZ2luYWxVc2VyQWdlbnQiOiJNb3ppbGxhLzUuMCAoTGludXg7IEFuZHJvaWQgNy4xLjI7IEFGVE1NIEJ1aWxkL05TNjI5Nzsgd3YpIEFwcGxlV2ViS2l0LzUzNy4zNiAoS0hUTUwsIGxpa2UgR2Vja28pIFZlcnNpb24vNC4wIENocm9tZS8xMTIuMC41NjE1LjE5NyBNb2JpbGUgU2FmYXJpLzUzNy4zNiBBZG9iZVBhc3NOYXRpdmVGaXJlVFYvMy4wLjgifSwiZGlzcGxheSI6eyJ3aWR0aCI6MCwiaGVpZ2h0IjowLCJwcGkiOjAsIm5hbWUiOiJESVNQTEFZIiwidmVuZG9yIjpudWxsLCJ2ZXJzaW9uIjpudWxsLCJkaWFnb25hbFNpemUiOm51bGx9LCJhcHBsaWNhdGlvbklkIjpudWxsLCJjb25uZWN0aW9uIjp7ImlwQWRkcmVzcyI6IjE5My4xMDUuMTQwLjEzMSIsInBvcnQiOiI5OTM0Iiwic2VjdXJlIjpmYWxzZSwidHlwZSI6bnVsbH19",
    "userAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "originalUserAgent": "Mozilla/5.0 (Linux; Android 7.1.2; AFTMM Build/NS6297; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/112.0.5615.197 Mobile Safari/537.36 AdobePassNativeFireTV/3.0.8",
    "authorizationType": "OAUTH2",
    "sourceApplicationInformation": {
      "id": "14138364-application-id",
      "name": "application name",
      "version": "1.0.0"
    }
  }
}
```

<br>

| Nome do elemento | Descrição |
|-----------------------------------|------------------------------------------------------------------------------------------------------------------|
| id | UUID gerado pelo Serviço de Código de Registro |
| código | Código de registro gerado pelo Serviço de código de registro |
| solicitante | ID do Solicitante |
| mvpd | ID do Mvpd |
| gerado | Carimbo de data e hora de criação do Código de registro (em milissegundos desde 1° de janeiro de 1970 GMT) |
| expira em | Carimbo de data e hora quando o código de registro expira (em milissegundos desde 1° de janeiro de 1970 GMT) |
| deviceId | ID de Dispositivo Exclusiva Base64 |
| info:deviceId | Tipo de dispositivo Base64 |
| info:deviceInfo | Informações de Dispositivo Normalizado Base64 criadas com base em informações recebidas do Agente do Usuário, Informações do Dispositivo X ou informações do dispositivo |
| info:userAgent | Agente do usuário enviado pelo aplicativo |
| info:originalUserAgent | Agente do usuário enviado pelo aplicativo |
| info:authorizationType | OAUTH2 para chamadas usando DCR |
| info:sourceApplicationInformation | Informações do aplicativo conforme configurado no DCR |

{style="table-layout:auto"}


<br>

### Mensagem de erro Amostra de resposta JSON (#error-sample-response)

```JSON
{
  "status": 400,
  "message": "Required '<>' is not present"
}
```

