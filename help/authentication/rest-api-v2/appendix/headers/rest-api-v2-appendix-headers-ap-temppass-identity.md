---
title: Cabeçalho - AP-TempPass-Identity
description: REST API V2 - Cabeçalho - AP-TempPass-Identity
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 6%

---


# Cabeçalho - AP-TempPass-Identity {#header-ap-temppass-identity}

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
