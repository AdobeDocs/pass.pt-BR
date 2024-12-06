---
title: Notas de versão do JavaScript 4.7.0 de autenticação da Adobe Pass
description: Notas de versão do JavaScript 4.7.0 de autenticação da Adobe Pass
exl-id: 07f90270-e64a-4c6b-a072-183af0f53352
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '121'
ht-degree: 0%

---

# Notas de versão do JavaScript 4.7.0 de autenticação da Adobe Pass {#javascript-sdk-470-release-notes}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Número da Build {#build-no-javascript-sdk-470}

Autenticação do Adobe Pass: JavaScript 4.7.0

Data de Lançamento: **27/02/2024 - 29/02/2024**

## Visão geral da versão {#overview-javascript-sdk-470}

* Remoção da versão obsoleta 2.0.1 do SDK do JavaScript do Access Enabler devido a vulnerabilidades de segurança.
Os seguintes URLs não são mais suportados e retornarão um código de status HTTP 410:
   * https://entitlement.auth.adobe.com/entitlement/AccessEnabler.js
   * http://entitlement.auth.adobe.com/entitlement/AccessEnabler.js
   * https://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.js
   * http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.js

## Lançar pacote {#rel-pkg-javascript-sdk-470}

O URL de produção é: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

O URL de armazenamento temporário é: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
