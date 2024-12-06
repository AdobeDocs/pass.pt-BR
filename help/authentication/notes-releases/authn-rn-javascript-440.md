---
title: Notas de versão do JavaScript 4.4.0 de autenticação da Adobe Pass
description: Notas de versão do JavaScript 4.4.0 de autenticação da Adobe Pass
exl-id: 28cc0ccc-7a1d-45bd-8455-26cfde25c5c5
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 0%

---

# Notas de versão do JavaScript 4.4.0 de autenticação da Adobe Pass {#javascript-sdk-440-release-notes}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Número da Build {#build-no-javascript-sdk-440}

Autenticação do Adobe Pass: JavaScript 4.4.0

Data de lançamento: **06/22/2021**


## Visão geral da versão {#overview-javascript-sdk-440}

### Novos recursos {#new-features-javascript-sdk-440}

Autorização de comprovação

* Nova API pré-autorizada - esta é a nova chamada de API a ser usada para o recurso Autorização de comprovação, que também introduz relatórios de erros aprimorados para o fluxo de comprovação.
* Esse recurso está disponível mediante solicitação, pois deve ser ativado na configuração da Autenticação do Primetime. Entre em contato com o TAM para obter detalhes adicionais sobre como ativar esse recurso.
* Substitui a API checkPreauthorizedResources.

Identificação da plataforma

* Adiciona o cabeçalho AP-SDK-Identifier em todas as chamadas do SDK para melhor identificar o tipo e a versão do SDK.

Outros

* Aprimoramentos arquitetônicos internos.


### Correções de erros {#bug-fixes-javascript-sdk-440}

* Corrigir a condição de corrida produzida quando setRequestor e getAuthentication são chamados simultaneamente.
* Correção de um problema que impedia que os direitos fossem carregados corretamente em ambientes de preparo.
* Solucionado o problema que impedia que o fluxo de logout em segundo plano fosse concluído nos navegadores Safari, portanto, o usuário ainda parecia autenticado até que uma atualização da página ocorresse. Foi introduzido um tempo limite, definido atualmente em 30 segundos, portanto, se não houver resposta do servidor de Autenticação Primetime durante esse período, o SDK chamará o retorno de chamada setAuthenticationStatus.

## Lançar pacote {#rel-pkg-javascript-sdk-440}

O URL de produção é: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

O URL de preparo é: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
