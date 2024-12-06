---
title: Notas de versão da Autenticação Adobe Pass 3.0.3
description: Notas de versão da Autenticação Adobe Pass 3.0.3
exl-id: f54b7c4a-78c5-4536-bed7-3c5f15640dea
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---

# Notas de versão da Autenticação Adobe Pass 3.0.3 {#pt-authn-303-rn}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-303}

* [Número da Build](#build-number-303)
* [Visão geral da versão](#release-overview-303)

### Número da Build {#build-number-2651}

Autenticação Adobe Pass: adobe-pass-**3.0.3**
Data de Lançamento: **10/29/2024 - 31/10/2024**

### Novos recursos {#new-features-303}

#### REST API v2 {#rest-apis}

##### Código

* Aprimoramentos na REST API V2 (desde a versão principal do Adobe Pass 3.0 entregue a [REST API V2](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)).
* Adicionados `notBefore` e `notAfter` campos em `/sessions/{code}` para retornar informações sobre a validade do código de autenticação.
* Identificação de plataforma aprimorada.

##### Documentação

* Para começar com a nova API REST v2, consulte o documento [Visão geral da API REST v2](../integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

##### Ferramentas

* Para experimentar a nova API REST v2, consulte a nova página Autenticação do Adobe Pass do site [Adobe Developer](https://developer.adobe.com/adobe-pass).
