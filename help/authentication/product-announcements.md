---
title: Anúncios de produto
description: Anúncios de produto
exl-id: 3c9c66e1-d31d-4af3-8ab2-eb32492f42ca
source-git-commit: 4ad5ea619f64a78a72f69228c9ae3c83a7b66f24
workflow-type: tm+mt
source-wordcount: '883'
ht-degree: 2%

---

# Anúncios de produto {#product-announcements}

<a href="https://experienceleague.adobe.com/en/docs/pass/authentication/product-announcements">![Série de webinários ao vivo](/help/authentication/assets/rest-api-v2/live-webinar-series-banner.png)</a>

Como um importante parceiro de autenticação da Adobe Pass, gostaríamos de convidá-lo para nosso próximo webinário em tempo real sobre a nova API REST. A nova API foi lançada no ano passado para aprimorar a experiência do usuário final e simplificar a integração com os serviços da Adobe Pass. 

Conduziremos uma série de dois webinários para fornecer uma visão geral da nova API, benefícios e casos de uso adicionais que podem ser ativados ao migrar para a nova API.

Inscreva-se no webinário que funciona melhor para você e sua equipe.

* [Webinário 1 - 19 de fevereiro de 2025](https://events.teams.microsoft.com/event/83c6ec1e-2522-4918-910f-c529b4fb0574@fa7b1b5a-7b34-4387-94ae-d2c178decee1)
* [Webinário 2 - 5 de março de 2025](https://events.teams.microsoft.com/event/6f673689-e848-4c00-93d9-4f5d8f108977@fa7b1b5a-7b34-4387-94ae-d2c178decee1)

Durante a sessão, você aprenderá sobre:

* Visão geral e benefícios da REST API v2
* Apresentação de fluxos básicos
* Linhas do tempo e próximas etapas

O webinário será útil se você estiver:

* Um novo cliente que planeja iniciar um novo aplicativo TVE
* Clientes existentes que precisam migrar para as novas APIs
* Parceiros de implementação que implementariam APIs para clientes do

Você pode encontrar a documentação técnica sobre a nova API [aqui](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md). Recomendamos que você e sua equipe preencham todas as perguntas que você gostaria de discutir durante a sessão [aqui](https://forms.office.com/r/sJea78tUy3).

## Fim da vida útil (EOL) {#eol}

Esta seção descreve o fim do suporte e as datas do fim da vida útil de determinados recursos e produtos de Autenticação da Adobe Pass que estão planejados para serem descontinuados.

Mantenha-se informado sobre os cronogramas de desativação e planeje tomar as medidas descritas abaixo.

| Aviso | Data da notificação | Data do fim do suporte | Data do fim da vida útil | Descrição |
|-----------------------------------------------------------------|-------------------|---------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| EOL do Adobe Pass Authentication AccessEnabler JavaScript SDK v3.5 | 11/12/2024 | N/A | 08/01/2025 | Planejamos o fim da vida útil do Adobe Pass Authentication AccessEnabler JavaScript SDK v3.5 em **8 de janeiro de 2025**. Após essa data, os recursos e os serviços fornecidos pelo AccessEnabler JavaScript SDK v3.5 não funcionarão mais, incluindo a autenticação e a autorização do usuário. |
| Mecanismo de segurança não DCR de autenticação do Adobe Pass EOL | 11/12/2024 | N/A | 20/01/2025 | Planejamos o fim da vida útil do Mecanismo de Segurança Não DCR da Autenticação do Adobe Pass em **20 de janeiro de 2025**. Esse mecanismo foi usado para proteger o acesso às seguintes APIs de autenticação da Adobe Pass:<ul><li><a href="/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md">Redefinir API Temp Pass</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md">API de Degradação</a></li><li><a href="/help/authentication/integration-guide-mvpds/proxy-mvpd-webserv.md">API de Proxy MVPD</a></li><li><a href="/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md">API de Monitoramento do Serviço de Qualificação</a></li></ul>Após essa data, os recursos e os serviços fornecidos pelas APIs acima que são acessados usando esse mecanismo de segurança não funcionarão.</br></br>Para garantir o serviço contínuo, será necessário migrar todos os aplicativos para usar o mecanismo [Registro de Cliente Dinâmico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md). |
| EOL da API REST v1 de autenticação do Adobe Pass | 11/12/2024 | 31/12/2025 | Final de 2026 (a ser definido) | Planejamos descontinuar o suporte à REST API v1 da Autenticação do Adobe Pass em **31 de dezembro de 2025**. Após essa data, as atualizações ou correções não serão mais fornecidas.</br></br>Para garantir o suporte contínuo, será necessário migrar todos os aplicativos para a <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>.</br></br>Planejamos o fim da vida útil da API REST v1 de Autenticação do Adobe Pass até o **fim de 2026**. Após essa data, os recursos e os serviços fornecidos pela REST API v1 não funcionarão mais, incluindo a autenticação e a autorização do usuário.</br></br>Para garantir o serviço contínuo, será necessário migrar todos os aplicativos para a <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>. |
| EOL dos SDKs do Adobe Pass Authentication AccessEnabler | 11/12/2024 | 31/05/2026 | Final de 2026 (a ser definido) | Planejamos descontinuar o suporte aos SDKs do Adobe Pass Authentication AccessEnabler em **31 de maio de 2026**. Após essa data, as atualizações ou correções não serão mais fornecidas.</br></br>Para garantir o suporte contínuo, será necessário migrar todos os aplicativos para a <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>.</br></br>Planejamos o fim da vida útil dos SDKs do Adobe Pass Authentication AccessEnabler até **fim de 2026**. Após essa data, os recursos e os serviços fornecidos pelos SDKs do AccessEnabler não funcionarão mais, incluindo a autenticação e a autorização do usuário.</br></br>Para garantir o serviço contínuo, será necessário migrar todos os aplicativos para a <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md">REST API v2</a>. |

## Versões do produto {#product-releases}

Esta seção compila referências ao histórico de versões e às notas de versão correspondentes para a autenticação da Adobe Pass.

### 2025 {#product-releases-2025}

| Notas de versão | Datas |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Notas de versão da Autenticação do Adobe Pass 3.1.0](notes-releases/auth-rn-310.md) | 25/02/2025 - 27/02/2025 |
| Adobe Pass [Notas de versão do JavaScript Authentication SDK 4.7.1](notes-releases/authn-rn-javascript-471.md) | 25/02/2025 - 27/02/2025 |

### 2024 {#product-releases-2024}

| Notas de versão | Datas |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Notas de versão da Autenticação do Adobe Pass 3.0.3](notes-releases/auth-rn-303.md) | 29/10/2024 - 31/10/2024 |
| [Notas de versão da Autenticação do Adobe Pass 3.0](notes-releases/auth-rn-300.md) | 10/09/2024 - 12/09/2024 |
| [Notas de versão da Autenticação do Adobe Pass 2.70](notes-releases/auth-rn-270.md) | 23/04/2024 - 25/04/2024 |
| [Notas de versão da Autenticação do Adobe Pass 2.69](notes-releases/auth-rn-269.md) | 27/02/2024 - 29/02/2024 |
| [Notas de versão do JavaScript SDK 4.7.0 de autenticação da Adobe Pass](notes-releases/authn-rn-javascript-470.md) | 27/02/2024 - 29/02/2024 |
| [Notas de versão do Adobe Pass Authentication iOS / tvOS SDK 3.9.2](notes-releases/authn-rn-ios-tvos-392.md) | 26/03/2024 |
| [Notas de versão do Adobe Pass Authentication iOS / tvOS SDK 3.8.4](notes-releases/authn-rn-ios-tvos-384.md) | 26/01/2024 |

### 2023 {#product-releases-2023}

| Notas de versão | Datas |
|---------------------------------------------------------------------------------------------------------|-------------------------|
| [Notas de versão da Autenticação do Adobe Pass 2.68](notes-releases/auth-rn-268.md) | 05/12/2023 - 07/12/2023 |
| [Notas de versão da Autenticação do Adobe Pass 2.67](notes-releases/auth-rn-267.md) | 12/09/2023 - 14/09/2023 |
| [Notas de versão da Autenticação do Adobe Pass 2.66](notes-releases/auth-rn-266.md) | 11/07/2023 - 13/07/2023 |
| [Notas de versão da Autenticação 2.65.1 do Adobe Pass](notes-releases/auth-rn-2651.md) | 20/06/2023 - 22/06/2023 |
| [Notas de versão da Autenticação do Adobe Pass 2.65](notes-releases/auth-rn-265.md) | 25/04/2023 - 27/04/2023 |
| [Notas de versão da Autenticação 2.64.1 do Adobe Pass](notes-releases/auth-rn-2641.md) | 31/01/2023 - 02/02/2023 |
| [Notas de versão do Adobe Pass Authentication iOS / tvOS SDK 3.8.3](notes-releases/authn-rn-ios-tvos-383.md) | 16/11/2023 |
| [Notas de versão do Adobe Pass Authentication iOS / tvOS SDK 3.8.2](notes-releases/authn-rn-ios-tvos-382.md) | 10/02/2023 |
| [Notas de versão do Adobe Pass Authentication iOS / tvOS SDK 3.8.1](notes-releases/authn-rn-ios-tvos-381.md) | 05/26/2023 |
| [Notas de versão do Android SDK 3.7.3 de autenticação da Adobe Pass](notes-releases/authn-rn-android-373.md) | 19/09/2023 |

### 2022 {#product-releases-2022}

| Notas de versão | Datas |
|-----------------------------------------------------------------------------------------------------------|-------------------------|
| [Notas de versão da Autenticação do Adobe Pass 2.64](notes-releases/auth-rn-264.md) | 08/11/2022 - 10/11/2022 |
| [Notas de versão da Autenticação do Adobe Pass 2.63](notes-releases/auth-rn-263.md) | 20/09/2022 - 22/09/2022 |
| [Notas de versão da Autenticação 2.62.1 do Adobe Pass](notes-releases/auth-rn-2621.md) | 02/08/2022 - 04/08/2022 |
| [Notas de versão do JavaScript SDK 4.6.0 de autenticação da Adobe Pass](notes-releases/authn-rn-javascript-460.md) | 20/09/2022 - 22/09/2022 |

### 2021 {#product-releases-2021}

| Notas de versão | Datas |
|-----------------------------------------------------------------------------------------------------------|------------|
| Adobe Pass [Notas de versão do JavaScript Authentication SDK 4.4.0](notes-releases/authn-rn-javascript-440.md) | 22/06/2021 |
| [Notas de versão do Adobe Pass Authentication iOS / tvOS SDK 3.7.0](notes-releases/authn-rn-ios-tvos-370.md) | 03/09/2021 |
