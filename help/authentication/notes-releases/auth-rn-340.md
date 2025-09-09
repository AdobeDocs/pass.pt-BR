---
title: Notas de versão da Autenticação Adobe Pass 3.4.0
description: Notas de versão da Autenticação Adobe Pass 3.4.0
exl-id: 7a9b8c6d-4e5f-4a3b-8c7d-9e0f1a2b3c4d
source-git-commit: 3a275f64f7f8cffa3bdc0d546c8e2db517840069
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 0%

---

# Notas de versão da Autenticação Adobe Pass 3.4.0 {#authn-340-rn}

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-340}

* [Número da Build](#build-number-340)
* [Visão geral da versão](#release-overview-340)

### Número da Build {#build-number-340}

Autenticação Adobe Pass: adobe-pass-**3.4.0**
Data de lançamento: **09/09/2025 - 11/09/2025**

### Visão geral da versão {#release-overview-340}

#### REST API v2

* Adicionado suporte para a [Experience Cloud ID (ECID)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-visitor-identifier.md) para melhorar a identificação do usuário e os recursos de rastreamento.
* Os cabeçalhos AP-Device-Identifier e X-Device-Info foram alterados para opcional para API REST V2 [API de configuração](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

#### Correções de erros

* Correção de um problema em que a ID do MVPD e a ID do Proxy MVPD eram revertidas no token da resposta de [Decisões](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) da API REST V2.
* Correção de um problema em que a ID do solicitante não estava presente no token da resposta [Decisões](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) da API REST V2.
* Correção de um problema em que transmitir muitos recursos para a API REST V2 [Pré-autorizar Decisões](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) API resultava em uma lista de decisões vazia na resposta em vez do [Código de Erro Aprimorado](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) **muitos_recursos**.

#### Diversos

* Vulnerabilidades de segurança corrigidas.