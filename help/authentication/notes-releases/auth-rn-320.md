---
title: Notas de versão da Autenticação Adobe Pass 3.2.0
description: Notas de versão da Autenticação Adobe Pass 3.2.0
exl-id: 43aee317-dbac-4000-893e-839ee3e9f6ba
source-git-commit: fcdf50b2caad20deef15fceeb3e23f4195c0078d
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---

# Notas de versão da Autenticação Adobe Pass 3.2.0 {#authn-320-rn}

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-320}

* [Número da Build](#build-number-320)
* [Visão geral da versão](#release-overview-320)

### Número da Build {#build-number-320}

Autenticação Adobe Pass: adobe-pass-**3.2.0**

Data de lançamento: **06/10/2025 - 06/12/2025**

### Visão geral da versão {#release-overview-320}

#### REST API v2

* Um novo motivo `missing_parameters_fallback` foi adicionado para casos em que os parâmetros estão ausentes na resposta da [API de sessões](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).
* Um novo campo &quot;dispositivo&quot; foi adicionado à resposta [API de sessões](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md).

#### Novos recursos

* Expanda API de Degradação para adicionar a capacidade de aplicar regras de degradação para vários MVPDs em uma única chamada

#### Correções de erros

* Correção de um problema que impedia que o fluxo de autenticação fosse concluído com êxito nos casos em que o processamento de metadados do usuário falhava.
* Correção de um problema em que o motivo de autorização não era calculado corretamente.
* Correção de um problema em que upstreamUserID não estava presente na resposta do perfil.
* Correção de um problema que fazia com que a solicitação de autenticação não incluísse informações de escopo para solicitações de autenticação de inicialização da API REST V2.
