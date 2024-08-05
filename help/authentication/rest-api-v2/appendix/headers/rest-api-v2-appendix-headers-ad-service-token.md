---
title: Cabeçalho - AD-Service-Token
description: REST API V2 - Cabeçalho - AD-Service-Token
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 1%

---


# Cabeçalho - AD-Service-Token {#header-ad-service-token}

>[!NOTE]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview}

O cabeçalho de solicitação <b>AD-Service-Token</b> contém o identificador de usuário exclusivo como `JWS` obtido de um serviço de identidade em execução fora dos sistemas de Autenticação Adobe Pass.

Esse cabeçalho foi projetado para uso em fluxos habilitados para logon único (SSO) que usam o método de token de serviço.

Para obter mais detalhes sobre os fluxos habilitados para o logon único (SSO) que usam o método Token de Serviço, consulte a documentação do [Logon único usando fluxos de token de serviço](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md).

## Sintaxe {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AD-Service-Token</b>: &lt;identificador_usuário_exclusivo&gt;</td>
   </tr>
   <tr>
      <td>Tipo de cabeçalho</td>
      <td>Cabeçalho da solicitação</td>
   </tr>
   <tr>
      <td>Padrão</td>
      <td>Não</td>
   </tr>
</table>

## Diretivas {#directives}

<b>identificador_usuário_exclusivo</b>

A Assinatura JSON Web (`JWS`), que é um JSON Web Token (`JWT`) assinado contendo informações de identificador de usuário exclusivas.

O `JWT` tem os seguintes atributos:

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Atributo</th>
      <th style="background-color: #EFF2F7;">Descrição</th>
   </tr>
   <tr>
      <td>iss</td>
      <td>O identificador exclusivo associado à entidade que oferece ao aplicativo um serviço de identidade externo para obter logon único (SSO).</td>
   </tr>
   <tr>
      <td>sub</td>
      <td>O identificador exclusivo do usuário conforme retornado pelo serviço de identidade externo.</td>
   </tr>
   <tr>
      <td>aud</td>
      <td>O público-alvo, que deve ser "Adobe".</td>
   </tr>
   <tr>
      <td>iat</td>
      <td>O emitido no carimbo de data e hora do JWT atual.</td>
   </tr>
   <tr>
      <td>exp</td>
      <td>O carimbo de data e hora de expiração do JWT atual.</td>
   </tr>
</table>

O `JWT` deve ser assinado usando o algoritmo `SHA256withRSA`.

O `JWT` deve ser assinado com uma chave privada, parte de um par de chave privada RSA - chave pública gerenciada pelo serviço de identidade externo.

A chave pública desse par deve ser entregue à Autenticação Adobe Pass para que seja possível reconhecer `JWT` tokens assinados com a chave privada acima.

## Exemplos {#examples}

```JSON
// JWT
// Header
// {
//  "alg": "RS256",
//  "kid": "qapEaY0hYNvphytwII3Sae_cAKyLS7GZOqtT_a4ajeo"
// }
// Payload data
// {
//  "sub": "Jane",
//  "name": "Jane Smith",
//  "iat": 1516239022,
//  "iss": "adobe",
//  "exp": 1720152820,
//  "aud": "adobe",
//  "jti": "3b2fb040-30a9-43d7-b647-d00ac495bab"
// }
 
// JWS
// eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA

AD-Service-Token: eyJhbGciOiJSUzI1NiIsImtpZCI6InFhcEVhWTBoWU52cGh5dHdJSTNTYWVfY0FLeUxTN0daT3F0VF9hNGFqZW8ifQ.eyJzdWIiOiJKYW5lIiwibmFtZSI6IkphbmUgU21pdGgiLCJpYXQiOjE1MTYyMzkwMjIsImlzcyI6ImFkb2JlIiwiZXhwIjoxNzIwMTUyODIwLCJhdWQiOiJhZG9iZSIsImp0aSI6IjNiMmZiMDQwLTMwYTktNDNkNy1iNjQ3LWQwMGFjNDk1YmFiIn0.stHLZFh-635LDNjv9HRHzq912ICNCVGUS3f4RS_bAxpUiUSB6CShS2VvU4V-THEXj7d_zk1mxtPP0QM_pCrh4Vk2GaPRa856Bt_PhsfQY-_benDcB6MIoFX67qrREGncGiv7JEs3ksa-P1YvBYXolT7t52K093kFaQtICfB-aBa8danRZvUrJHjjFoILEpTbQuzxKRN6y36J3p1FZ-SfDuofHp3SnXDrWFRYyXYQnb9WFlhNBxR400-0vzTONZYd097WWy1shMw5V8TvIDvCDE5ifqk31gMdYga-N3JkcTA5QoW7Zl80UV7BhR5v14Va1IZLcbFra_UJdEzbBwW_nA
```
