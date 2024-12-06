---
title: Redefinir aprovação temporária
description: Redefinir aprovação temporária
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---


# Redefinir aprovação temporária {#reset-temp-pass}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Antes de usar a API Reset Temp Pass, verifique se os seguintes pré-requisitos foram atendidos:
>
> * Obtenha as credenciais do cliente conforme descrito na documentação da API [Recuperar credenciais do cliente](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
> * Obtenha o token de acesso conforme descrito na documentação da API [Recuperar token de acesso](../../rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Consulte a documentação [Visão Geral do Registro Dinâmico do Cliente](../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) para obter mais informações sobre como criar um aplicativo registrado e baixar a instrução de software.

Para **redefinir um Temp Pass específico para um dispositivo ou todos os dispositivos**, a Autenticação da Adobe Pass fornece aos Programadores uma API da Web *pública*:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

* **Protocolo:** HTTPS
* **Host:**
   * Versão - mgmt.auth.adobe.com
   * Pré- Igual - mgmt-prequal.auth.adobe.com
* **Caminho:** /reset-tempass/v3/reset
* **Parâmetros de consulta:** device_id (quando nenhum valor é fornecido, o padrão é all), requestor_id (obrigatório), mvpd_id (obrigatório), appId, deviceUser, ambiente
* **Cabeçalhos:** Autorização: Portador &lt;access_token_here>
* **Resposta:**
   * Sucesso - HTTP 204
   * Falha:xw
      * HTTP 400 para uma solicitação incorreta
      * HTTP 401 se o acesso for negado. O cliente DEVE solicitar um novo access_token.
      * HTTP 403 se a ID do cliente não tiver mais permissão para executar solicitações. As novas credenciais do cliente devem ser geradas.


Por exemplo:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

Para **redefinir a Senha Temp Flexível para uma chave genérica ou todas as chaves**, a Autenticação Adobe Pass fornece aos Programadores uma API da Web *pública*:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic
```

* **Protocolo:** HTTPS
* **Host:**
   * Versão - mgmt.auth.adobe.com
   * Pré- Igual - mgmt-prequal.auth.adobe.com
* **Caminho:** /reset-tempass/v3/reset/generic
* **Parâmetros de consulta:** chave(quando nenhum valor é fornecido, o padrão é todos), requestor_id (obrigatório), mvpd_id (obrigatório), ambiente
* **Cabeçalhos:** Autorização: Portador &lt;access_token_here>
* **Resposta:**
   * Sucesso - HTTP 204
   * Falha:
      * HTTP 400 para uma solicitação incorreta
      * HTTP 401 se o acesso for negado. O cliente DEVE solicitar um novo access_token.
      * HTTP 403 se a ID do cliente não tiver mais permissão para executar solicitações. As novas credenciais do cliente devem ser geradas.


Por exemplo, definir a *Chave genérica* como um hash de endereço de email (para
Temp Pass (MVPDs que oferecem suporte a esse tipo de funcionalidade) resultará em
seguindo chamada HTTP (o Email é `user@domain.com` seu SHA-256
o hash é `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```
