---
title: Cabeçalho - AP-Identificador de dispositivo
description: REST API V2 - Cabeçalho - AP-Identificador de dispositivo
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '103'
ht-degree: 3%

---


# Cabeçalho - AP-Identificador de dispositivo {#header-ap-device-identifier}

>[!NOTE]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview}

O cabeçalho de solicitação <b>AP-Device-Identifier</b> contém o identificador do dispositivo de streaming conforme foi criado pelo aplicativo cliente.

## Sintaxe {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Identificador-dispositivo-AP</b>: &lt;tipo&gt; &lt;identificador&gt;</td>
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

<b>&lt;tipo></b>

O tipo de identificador do dispositivo.

Há apenas um tipo compatível, conforme apresentado abaixo.

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Tipo</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>impressão digital</td>
      <td>O identificador de dispositivo consiste em um identificador opaco e é criado pelo aplicativo cliente.</td>
   </tr>
</table>


<b>&lt;identificador></b>

O valor `Base64-encoded` do identificador do dispositivo.

## Exemplo {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```
