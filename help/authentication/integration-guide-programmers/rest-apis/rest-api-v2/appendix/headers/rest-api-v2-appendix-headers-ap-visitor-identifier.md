---
title: Cabeçalho - AP-Visitor-Identifier
description: REST API V2 - Cabeçalho - AP-Visitor-Identifier
exl-id: b4f8e2a1-9c7d-4e3a-8f5b-2d1c6e9a4b7f
source-git-commit: 26245e019afac2c0844ed64b222208cc821f9c6c
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 4%

---


# Cabeçalho - AP-Visitor-Identifier {#header-ap-visitor-identifier}

>[!NOTE]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview}

O cabeçalho de solicitação <b>AP-Visitor-Identifier</b> contém o `ECID` exigido pelo aplicativo cliente para identificar exclusivamente um visitante nas soluções da Adobe Experience Cloud.

Para obter mais detalhes sobre o uso da ECID na Autenticação do Adobe Pass, consulte a documentação [Uso da Experience Cloud ID na Autenticação do Adobe Pass](../../../../features-premium/analytics/exp-cloud-id-authn.md).

## Sintaxe {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Visitor-Identifier</b>: &lt;visitor_identifier&gt;</td>
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

## Exemplos {#examples}

```JSON
AP-Visitor-Identifier: "THE_ECID_VALUE"
```
