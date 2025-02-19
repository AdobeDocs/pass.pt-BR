---
title: Notas de versão da Autenticação Adobe Pass 3.1.0
description: Notas de versão da Autenticação Adobe Pass 3.1.0
source-git-commit: 4ad5ea619f64a78a72f69228c9ae3c83a7b66f24
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 0%

---

# Notas de versão da Autenticação Adobe Pass 3.1.0 {#authn-310-rn}

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-310}

* [Número da Build](#build-number-310)
* [Visão geral da versão](#release-overview-310)

### Número da Build {#build-number-310}

Autenticação Adobe Pass: adobe-pass-**3.1.0**

Data de Lançamento: **02/25/2025 - 27/02/2025**

### Visão geral da versão {#release-overview-310}

#### REST API v2

* Novo nome de ação `partner_logout` e tipo de ação `partner_interactive` para diferenciar entre o logout normal e o logout de logon único de parceiro na resposta da API REST v2 [API de logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md).
* Novo campo `reason` para fornecer mais insights sobre o nome da ação nas respostas da [API de sessões](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) e da [API de SSO de sessões](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md) da API REST v2.

#### Correções de erros

* Correção de um problema que impedia que os assinantes do Spectrum se autenticassem por meio da API REST v2 [API de Autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).
* Correção de um problema que impedia que os eventos gerados pela REST API V2 [API de Autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) fossem agregados corretamente no ESM.
* Correção de um problema que causava o cálculo incorreto do carimbo de data/hora `notBefore` para os perfis de usuário na resposta da API REST v2 [API de perfis](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md).

#### JavaScript SDK

* Remoção da versão 3.5.0 do [AccessEnabler JavaScript SDK](authn-rn-javascript-471.md).