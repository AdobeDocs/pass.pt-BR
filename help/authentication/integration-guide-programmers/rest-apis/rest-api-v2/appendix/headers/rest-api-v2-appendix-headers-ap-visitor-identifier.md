---
title: Cabeçalho - AP-Visitor-Identifier
description: REST API V2 - Cabeçalho - AP-Visitor-Identifier
exl-id: 216f398b-1cfa-4453-a81d-963675b33ec2
source-git-commit: af867cb5e41843ffa297a31c2185d6e4b4ad1914
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

Para obter mais detalhes sobre o uso da ECID na Autenticação do Adobe Pass, consulte a documentação [Uso da Experience Cloud ID na Autenticação do Adobe Pass](/help/authentication/integration-guide-programmers/features-standard/analytics/exp-cloud-id-authn.md).

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
