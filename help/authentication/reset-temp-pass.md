---
title: Redefinir aprovação temporária
description: Redefinir aprovação temporária
exl-id: ab39e444-eab2-4338-8d09-352a1d5135b6
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '478'
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
> * Obtenha as credenciais do cliente conforme descrito na documentação da API [Recuperar credenciais do cliente](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
> * Obtenha o token de acesso conforme descrito na documentação da API [Recuperar token de acesso](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Consulte a documentação [Visão Geral do Registro Dinâmico do Cliente](./dcr-api/dynamic-client-registration-overview.md) para obter mais informações sobre como criar um aplicativo registrado e baixar a instrução de software.

Para **redefinir um Temp Pass** específico, a Autenticação do Adobe Pass fornece aos Programadores uma API da Web *pública*:

* **Ambiente:** especifica o ponto de extremidade do servidor de passagem de TV por Adobe que receberá a chamada de rede de redefinição de Passagem Temporária. Valores possíveis: **Prequal** (*mgmt* prequal.auth.adobe.com*), **Release** (*mgmt.auth.adobe.com*) ou **Custom** (reservado para teste interno de Adobe).
* **Token de acesso OAuth2:** o token OAuth2 é necessário para autorizar o Programador para a autenticação de TV por Adobe. Esse token pode ser obtido conforme descrito na documentação da API [Recuperar token de acesso](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md).
* **ID da Passagem Temporária:** a ID exclusiva do MVPD da Passagem Temporária a ser redefinido.(um programador pode usar vários MVPDs Temp Pass e deseja redefinir um específico)
* **Chave Genérica:** alguns MVPDs de Passagem Temporária (ou seja, [Passagem temporária promocional](promotional-temp-pass.md)).

Todos os parâmetros acima (exceto a *Chave Genérica*) são obrigatórios. Este é um exemplo de parâmetros e a chamada de rede associada (o exemplo é na forma de um comando *curl *):

* **Ambiente:** Versão (*mgmt.auth.adobe.com*)
* **Token de acesso OAuth2:** &lt;access_token> conforme descrito na documentação da API [Recuperar token de acesso](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* **ID do Programador:** REF
* **ID da Passagem Temporária:** TempPassREF
* **Chave Genérica:** nula (nenhum valor fornecido)

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

uma solicitação HTTP DELETE será feita para o ponto de extremidade **/reset**, transmitindo o *Token de Acesso OAuth2* no cabeçalho de Autorização e a *ID do Dispositivo*, a *ID do Solicitante* e a *ID de Passagem Temporária (MVPD ID)* como parâmetros.

Se o Programador fornecer um valor para a *Chave Genérica*, outra chamada HTTP será executada (desta vez para o ponto de extremidade **/reset/generic**), transmitindo a *Chave Genérica* dentro do parâmetro de solicitação *chave*.

Por exemplo, definir a *Chave genérica* como um hash de endereço de email (para
Temp Pass (MVPDs que oferecem suporte a esse tipo de funcionalidade) resultará em
seguindo chamada HTTP (o Email é `user@domain.com` seu SHA-256
o hash é `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


Para **redefinir um Temp Pass específico para todos os dispositivos**, a Autenticação da Adobe Pass fornece aos Programadores uma API da Web *pública*:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

>[!NOTE]
>O URL acima substitui a API de redefinição anterior. A antiga API de redefinição (v1) não é mais suportada.

* **Protocolo:** HTTPS
* **Host:**
   * Versão - mgmt.auth.adobe.com
   * Pré- Igual - mgmt-prequal.auth.adobe.com
* **Caminho:** /reset-tempass/v3/reset
* **Parâmetros de consulta:** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
* **Cabeçalhos:** Autorização: Portador &lt;access_token_here>
* **Resposta:**
   * Sucesso - HTTP 204
   * Falha:
      * HTTP 400 para uma solicitação incorreta
      * HTTP 401 se o acesso for negado. O cliente DEVE solicitar um novo access_token.
      * HTTP 403 se a ID do cliente não tiver mais permissão para executar solicitações. As novas credenciais do cliente devem ser geradas.


Por exemplo:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```
