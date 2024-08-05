---
title: Cabeçalho - AP-TempPass-Identity
description: REST API V2 - Cabeçalho - AP-TempPass-Identity
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '84'
ht-degree: 4%

---


# Cabeçalho - AP-TempPass-Identity {#header-ap-temppass-identity}

>[!NOTE]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview}

O cabeçalho de solicitação <b>AP-TempPass-Identity</b> contém as informações de identidade do usuário usadas para obter TempPass promocional.

## Sintaxe {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-TempPass-Identity</b>: &lt;informações_da_identidade_do_usuário&gt;</td>
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

<b>&lt;user_identity_information></b>

O valor `Base64-encoded` sobre as informações de identidade do usuário associadas ao usuário final ao qual um acesso temporário promocional precisa ser concedido.

## Exemplos {#examples}

```JSON
// Identity
// {"email": "example@domain.com"}

// Base64-encoded
// eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==

AP-TempPass-Identity: eyJlbWFpbCI6ICJleGFtcGxlQGRvbWFpbi5jb20ifQ==
```
