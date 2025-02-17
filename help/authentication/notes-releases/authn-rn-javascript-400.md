---
title: Notas de versão do JavaScript 4.0.0 de autenticação da Adobe Pass
description: Notas de versão do JavaScript 4.0.0 de autenticação da Adobe Pass
exl-id: 2ded9ad8-56f7-44b5-87a2-12a195cd0829
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 0%

---

# Notas de versão do JavaScript 4.0.0 de autenticação da Adobe Pass {#javascript-sdk-400-rn}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Número da Build {#build-number-400}

Autenticação do Adobe Pass: JavaScript 4.0.0

Data de lançamento: **07/05/2018**

## Visão geral da versão {#release-overview-400}

* Implementação de suporte para o mecanismo dinâmico de registro de clientes, um mecanismo de segurança unificado em todas as plataformas. O gerenciamento do acesso de segurança em plataformas pode ser feito por meio do painel TVE, pelo próprio programador.
* Elimina o uso de todos os cookies de terceiros e quase todos os cookies próprios para todas as funcionalidades (exceto a individualização, em que um cookie próprio ainda é usado - será removido no futuro), tornando-o mais compatível e inovador com as políticas emergentes de navegador em torno de cookies de terceiros, ou cookies em geral (por exemplo, Safari&#39;s ITP). Observe que a versão 3.x anterior funciona conforme esperado para os fluxos felizes usando apenas cookies próprios, mas isso pode mudar com o lançamento de novas versões do navegador.
* Suporte para desabilitar o cache para comprovar verificações de autorização.
* Esta versão NÃO é compatível com versões anteriores e é hospedada em um novo caminho: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js . Para se beneficiar desse novo JS SDK, é necessário um processo de migração dos programadores para registrar seus aplicativos da Web usando o novo mecanismo dinâmico de registro de cliente e apontar para o novo URL do JS SDK.

## Lançar pacote {#release-package-400}

O URL de produção é: https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js

O URL de preparo é: https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js
