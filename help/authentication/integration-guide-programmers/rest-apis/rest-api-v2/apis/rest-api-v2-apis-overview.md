---
title: Visão geral das APIs REST API V2
description: Visão geral das APIs REST API V2
exl-id: 62b48bf0-d200-4949-b268-8f8ea2daabfa
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 35c6ed6491cd47e6f85ee8974d72236c19fb8a6d
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 0%
---
# Visão geral das APIs REST API V2 {#rest-api-v2-apis-overview}

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

Para começar, visite a documentação pública das APIs nas quais você está interessado, na lista abaixo.

## URLs base {#rest-api-v2-apis-base-urls}

Use o URL base específico do ambiente abaixo para solicitações da REST API V2. Anexar
o `path` relativo documentado para cada API para esta URL base.

Algumas respostas de API retornam URLs interativos que o dispositivo de streaming deve abrir
em um navegador ou agente do usuário. Os exemplos incluem o atributo `url` retornado por
[Iniciar logout para mvpd específico](logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
e o redirecionamento `Location` retornado por
[Executar autenticação no agente do usuário](sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).

Esses URLs interativos usam o mesmo host do Provedor de serviços (SP) da Adobe Pass.
Abra um URL interativo absoluto exatamente como retornado. Se o Adobe Pass retornar um
URL interativa relativa começando com `/adobe-services/`, resolva-a com
o host da controladora de armazenamento para o ambiente de destino:

| Ambiente | REST API V2 e URL base interativo do URL |
| --- | --- |
| Produção | `https://sp.auth.adobe.com` |
| Estágios | `https://sp.auth-staging.adobe.com` |

## Configuração {#rest-api-v2-apis-configuration}

* [Recuperar configuração para provedor de serviços específico](configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)

## Sessões {#rest-api-v2-apis-sessions}

* [Criar sessão de autenticação](sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Retomar sessão de autenticação](sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Recuperar sessão de autenticação](sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [Executar autenticação no agente do usuário](sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)

## Perfis {#rest-api-v2-apis-profiles}

* [Recuperar perfis](profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Recuperar perfil para mvpd específico](profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Recuperar perfil para código específico](profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

## Decisões {#rest-api-v2-apis-decisions}

* [Recuperar decisões de autorização usando mvpd específico](decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Recuperar decisões de pré-autorização usando mvpd específico](decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

## Sair {#rest-api-v2-apis-logout}

* [Iniciar logout para mvpd específico](logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

## Logon único de parceiro {#rest-api-v2-apis-partner-single-sign-on}

* [Recuperar solicitação de autenticação do parceiro](partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
* [Criar e recuperar perfil usando a resposta de autenticação do parceiro](partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)

## Deseja experimentar a REST API V2?

Agora você pode explorar a REST API V2 por meio de nossa página dedicada ao produto do site [Adobe Developer](https://developer.adobe.com/adobe-pass/).
