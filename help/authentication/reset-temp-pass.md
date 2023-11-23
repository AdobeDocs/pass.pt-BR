---
title: Redefinir aprovação temporária
description: Redefinir aprovação temporária
source-git-commit: 4ae0b17eff2dfcf0aaa5d11129dfd60743f6b467
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Redefinir aprovação temporária {#reset-temp-pass}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.
>
>Para usar a API Reset Temp Pass, é necessário:
>- solicite à equipe de suporte uma declaração de software para seu aplicativo registrado
>- obter um token de acesso com base em [Registro de cliente dinâmico](dynamic-client-registration.md)
> 

Para **redefinir um Temp Pass específico**, a Autenticação do Adobe Pass fornece aos programadores uma *público* API da Web:

- **Ambiente:** especifica o ponto de extremidade do servidor de passagem de TV por Adobe que receberá a chamada de rede de passagem temporária de redefinição. Valores possíveis: **Pré-Igual** (*mgmt-prequal.auth.adobe.com*), **Versão** (*mgmt.auth.adobe.com* ou **Personalizado** (reservado para testes internos de Adobe).
- **Token de acesso OAuth2:** o token OAuth2 é necessário para autorizar o Programador para a autenticação de TV por Adobe. Esse token pode ser obtido em [Registro de cliente dinâmico](dynamic-client-registration.md).
- **ID de passagem temporária:** o identificador exclusivo do MVPD de passagem temporária a ser redefinido.(um programador pode usar vários MVPDs Temp Pass e deseja redefinir um específico)
- **Chave genérica:** alguns MVPDs Temp Pass (ou seja, [Temp pass promocional](promotional-temp-pass.md)).

Todos os parâmetros acima (exceto o *Chave genérica*) são obrigatórios. Este é um exemplo de parâmetros e a chamada de rede associada (o exemplo é na forma de um comando *curl *):

- **Ambiente:** Versão (*mgmt.auth.adobe.com*)
- **Token de acesso OAuth2:** &lt;access_token> de [Registro de cliente dinâmico](dynamic-client-registration.md)
- **ID do programador:** REF
- **ID de passagem temporária:** TempPassREF
- **Chave genérica:** null (nenhum valor fornecido)

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>" "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=f23804a37802993fdc8e28a7f244dfe088b6a9ea21457670728e6731fa639991&requestor_id=REF&mvpd_id=TempPassREF"
```

uma solicitação HTTP DELETE será feita para o **/reset** endpoint, transmitindo o *Token de acesso OAuth2* no cabeçalho Autorização e no campo *ID do dispositivo*, *ID do Solicitante* e *ID de Aprovação Temporária (ID do MVPD)* como parâmetros.

Se o Programador fornecer um valor para a variável *Chave genérica*, outra chamada HTTP será executada (desta vez para o **/reset/generic** ponto de extremidade), transmitindo o *Chave genérica* dentro do *key* parâmetro de solicitação.

Por exemplo, definir a variável *Chave genérica* para um hash de endereço de email (para MVPDs Temp Pass que oferecem suporte a esse tipo de funcionalidade) gerará a seguinte chamada HTTP (o email é `user@domain.com` seu hash SHA-256 é `f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7`):

```curl
curl -X DELETE -H "Authorization:Bearer <access_token_here>"
"https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=TempPassREF"
```


Para **redefinir um Temp Pass específico para todos os dispositivos**, a Autenticação do Adobe Pass fornece aos programadores uma *público* API da Web:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v3/reset
```

>[!NOTE]
>O URL acima substitui a API de redefinição anterior. A antiga API de redefinição (v1) não é mais suportada.

- **Protocolo:** HTTPS
- **Host:**
   - Versão - mgmt.auth.adobe.com
   - Pré- Igual - mgmt-prequal.auth.adobe.com
- **Caminho:** /reset-tempass/v3/reset
- **Parâmetros de consulta:** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
- **Cabeçalhos:** Autorização: Portador &lt;access_token_here>
- **Resposta:**
   - Sucesso - HTTP 204
   - Falha:
      - HTTP 400 para uma solicitação incorreta
      - HTTP 401 se o acesso for negado. O cliente DEVE solicitar um novo access_token.
      - HTTP 403 se a ID do cliente não tiver mais permissão para executar solicitações. As novas credenciais do cliente devem ser geradas.


Por exemplo:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```
