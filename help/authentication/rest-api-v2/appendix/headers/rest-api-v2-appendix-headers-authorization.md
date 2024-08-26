---
title: Cabeçalho - autorização
description: REST API V2 - Cabeçalho - Autorização
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 3%

---


# Cabeçalho - autorização {#header-authorization}

>[!NOTE]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview}

O cabeçalho de solicitação <b>Authorization</b> contém o token de acesso `Bearer` exigido pelo aplicativo cliente para acessar APIs protegidas pelo Adobe Pass.

Para obter mais detalhes sobre o mecanismo de acesso a APIs protegidas pelo Adobe Pass, consulte a [documentação de Visão geral do registro dinâmico de clientes](../../../dcr-api/dynamic-client-registration-overview.md).

## Sintaxe {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Autorização</b>: portador &lt;access_token&gt;</td>
   </tr>
   <tr>
      <td>Tipo de cabeçalho</td>
      <td>Cabeçalho da solicitação</td>
   </tr>
   <tr>
      <td>Padrão</td>
      <td>Sim</td>
   </tr>
</table>

## Diretivas {#directives}

<b>&lt;access_token></b>

O valor do token de acesso é um valor opaco com tempo de vida limitado (por exemplo, 24 horas) que deve ser obtido do Adobe Pass conforme descrito na documentação da API [Recuperar token de acesso](../../../dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md).

## Exemplos {#examples}

```JSON
Authorization: bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiI0NmY0MGZiMy01NmJkLTQyYTktOTExYS02YmZmNmEyZmY0
                      MDciLCJuYmYiOjE3MjM1NjE4ODUsImlzcyI6ImF1dGguYWRvYmUuY29tIiwic2NvcGVzIjoiYXBpO
                      mNsaWVudDp2MiIsImV4cCI6MTcyMzU4MzQ4NSwiaWF0IjoxNzIzNTYxODg1fQ.aZUZqwN2fCqNXgX
                      SdKFG9_HcqHjha66G6HmsfTJYcZc12iuLxMu7TT7MbhWVz3kW1jRqgJv8PHhrFSBL5_dgJ1PRSuDg
                      97ZK1secgMKwk46vKZVdtx7LF5t3jGVzQTwN4RqChqyvkW2o67KxVk5xarwJtwB2fwhX_732CYDcv
                      1gWOTLx4xyH5IVvg-P_aImyveG0D-x65I2nOKXaROVvv-kYE6B9OQv_-JBGj72R_yS2AyJQC0R_im
                      0h5S4YvL-c2UZrYK7pvdZq-xAj0uW1wad7PLZjl8yL5CWUz9vzQk2Cmj8adsydjb0u0P3aFrJ0HE9
                      ISqtRvjf4plR1TGWgw6
```
