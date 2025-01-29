---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass Authentication
user-guide-description: O Adobe Pass Authentication é uma solução de concessão de direitos para o TV Everywhere, o qual fornece uma estrutura modular para determinar se a pessoa que solicita o acesso a um recurso possui direito a ele.
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '1256'
ht-degree: 2%

---


# Ajuda de autenticação do Adobe Pass {#authentication}

+ [Adobe Pass Authentication](home.md)
+ [Anúncios de produto](product-announcements.md)
+ Versões do produto {#product-releases}
   + 2024 {#2024}
      + [Notas de versão da Autenticação Adobe Pass 3.0.3](notes-releases/auth-rn-303.md)
      + [Notas de versão da Autenticação Adobe Pass 3.0](notes-releases/auth-rn-300.md)
      + [Notas de versão da Autenticação Adobe Pass 2.70](notes-releases/auth-rn-270.md)
      + [Notas de versão da Autenticação Adobe Pass 2.69](notes-releases/auth-rn-269.md)
      + [Notas de versão do Adobe Pass Authentication JavaScript 4.7.0](notes-releases/authn-rn-javascript-470.md)
      + [Notas de versão do Adobe Pass Authentication iOS / tvOS 3.9.2](notes-releases/authn-rn-ios-tvos-392.md)
      + [Notas de versão do Adobe Pass Authentication iOS / tvOS 3.8.4](notes-releases/authn-rn-ios-tvos-384.md)
   + 2023 {#2023}
      + [Notas de versão da Autenticação Adobe Pass 2.68](notes-releases/auth-rn-268.md)
      + [Notas de versão da Autenticação Adobe Pass 2.67](notes-releases/auth-rn-267.md)
      + [Notas de versão da Autenticação Adobe Pass 2.66](notes-releases/auth-rn-266.md)
      + [Notas de versão do Adobe Pass Authentication 2.65.1](notes-releases/auth-rn-2651.md)
      + [Notas de versão da Autenticação Adobe Pass 2.65](notes-releases/auth-rn-265.md)
      + [Notas de versão do Adobe Pass Authentication 2.64.1](notes-releases/auth-rn-2641.md)
      + [Notas de versão do Adobe Pass Authentication iOS / tvOS 3.8.3](notes-releases/authn-rn-ios-tvos-383.md)
      + [Notas de versão do Adobe Pass Authentication iOS / tvOS 3.8.2](notes-releases/authn-rn-ios-tvos-382.md)
      + [Notas de versão do Adobe Pass Authentication iOS / tvOS 3.8.1](notes-releases/authn-rn-ios-tvos-381.md)
      + [Notas de versão do Adobe Pass Authentication Android 3.7.3](notes-releases/authn-rn-android-373.md)
   + 2022 {#2022}
      + [Notas de versão da Autenticação Adobe Pass 2.64](notes-releases/auth-rn-264.md)
      + [Notas de versão da Autenticação Adobe Pass 2.63](notes-releases/auth-rn-263.md)
      + [Notas de versão do Adobe Pass Authentication 2.62.1](notes-releases/auth-rn-2621.md)
      + [Notas de versão do Adobe Pass Authentication JavaScript 4.6.0](notes-releases/authn-rn-javascript-460.md)
   + 2021 {#2021}
      + [Notas de versão do Adobe Pass Authentication JavaScript 4.4.0](notes-releases/authn-rn-javascript-440.md)
      + [Notas de versão do Adobe Pass Authentication iOS / tvOS 3.7.0](notes-releases/authn-rn-ios-tvos-370.md)
+ Kickstart {#kickstart}
   + [Papel técnico](kickstart/technical-paper.md)
   + [Visão geral do programador](kickstart/programmer-overview.md)
   + [Visão geral do MVPD](kickstart/mvpd-overview.md)
   + [Guia de início rápido do programador](kickstart/programmer-kickstart-guide.md)
   + [Guia de início rápido do MVPD](kickstart/mvpd-kickstart-guide.md)
   + [Procedimentos de escalonamento](kickstart/escalation-procedures.md)
+ Guia De Integração Para Programadores {#integration-guide-programmers}
   + APIs REST {#rest-apis}
      + DCR DA API REST {#rest-api-dcr}
         + [Visão geral do registro dinâmico do cliente](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
         + [Glossário de registro dinâmico do cliente](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)
         + [Perguntas frequentes sobre o registro dinâmico de clientes](integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)
         + APIs {#rest-api-dcr-apis}
            + [Recuperar credenciais do cliente](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
            + [Recuperar token de acesso](integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
         + Fluxos {#rest-api-dcr-flows}
            + [Fluxo de registro dinâmico do cliente](integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)
      + REST API V2 {#rest-api-v2}
         + [Visão geral da REST API V2](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md)
         + [Glossário da REST API V2](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)
         + [Perguntas frequentes sobre REST API V2](integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)
         + APIs {#rest-api-v2-apis}
            + [Visão geral das APIs REST API V2](integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
            + Configuração {#rest-api-v2-configuration-apis}
               + [Recuperar configuração para provedor de serviços específico](integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
            + Sessões {#rest-api-v2-sessions-apis}
               + [Criar sessão de autenticação](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
               + [Retomar sessão de autenticação](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
               + [Recuperar sessão de autenticação](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
               + [Executar autenticação no agente do usuário](integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
            + Perfis {#rest-api-v2-profiles-apis}
               + [Recuperar perfis](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
               + [Recuperar perfil para mvpd específico](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
               + [Recuperar perfil para código específico](integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
            + Decisões {#rest-api-v2-decisions-apis}
               + [Recuperar decisões de autorização usando mvpd específico](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
               + [Recuperar decisões de pré-autorização usando mvpd específico](integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
            + Sair {#rest-api-v2-logout-apis}
               + [Iniciar logout para mvpd específico](integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
            + Logon Único de Parceiro {#rest-api-v2-partner-single-sign-on-apis}
               + [Recuperar solicitação de autenticação do parceiro](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
               + [Recuperar perfil usando resposta de autenticação de parceiro](integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
         + Fluxos {#rest-api-v2-flows}
            + [Visão geral de fluxos da REST API V2](integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
            + Fluxos de Acesso Básico {#rest-api-v2-basic-access-flows}
               + [Fluxo de perfis básicos realizado no aplicativo principal](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
               + [Fluxo de perfis básicos realizado no aplicativo secundário](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
               + [Fluxo de autenticação básica executado no aplicativo principal](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
               + [Fluxo de autenticação básico executado no aplicativo secundário](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
               + [Fluxo de autorização básico executado no aplicativo principal](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
               + [Fluxo básico de pré-autorização executado no aplicativo principal](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
               + [Fluxo de logout básico executado no aplicativo principal](integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
            + Fluxos de Acesso Degradados {#rest-api-v2-degraded-access-flows}
               + [Fluxos de acesso degradados](integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
            + Fluxos de Acesso Temporário {#rest-api-v2-temporary-access-flows}
               + [Fluxos de acesso temporário](integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
            + Fluxos de Acesso de Logon Único {#rest-api-v2-single-sign-on-access-flows}
               + [Logon único usando fluxos de parceiros](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
               + [Logon único usando fluxos de identidade da plataforma](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
               + [Logon único usando fluxos de token de serviço](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
               + [Fluxo de logout único](integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
         + Cookbooks {#rest-api-v2-cookbooks}
            + [Cookbook REST API V2 (cliente para servidor)](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-client-server.md)
            + [Cookbook REST API V2 (servidor para servidor)](integration-guide-programmers/rest-apis/rest-api-v2/cookbooks/rest-api-v2-cookbook-server-server.md)
         + Apêndice {#rest-api-v2-appendix}
            + Cabeçalhos {#rest-api-v2-appendix-headers}
               + [Cabeçalho - autorização](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)
               + [Cabeçalho - AP-Identificador de dispositivo](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
               + [Cabeçalho - X-Device-Info](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
               + [Cabeçalho - AD-Service-Token](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
               + [Cabeçalho - Adobe-Subject-Token](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
               + [Cabeçalho - AP-Parceiro-Estrutura-Status](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
               + [Cabeçalho - AP-TempPass-Identity](integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
   + Recursos Padrão {#standard-features}
      + Direitos {#entitlements}
         + [Autorização de comprovação](integration-guide-programmers/features-standard/entitlements/preflight-authz.md)
         + [Recurso protegido](integration-guide-programmers/features-standard/entitlements/protected-resources.md)
         + [Tokens de mídia](integration-guide-programmers/features-standard/entitlements/media-tokens.md)
         + [Metadados do usuário](integration-guide-programmers/features-standard/entitlements/user-metadata.md)
      + Relatório de Erros {#error-reporting}
         + [Códigos de erro aprimorados](integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md)
      + Acesso de Logon Único {#sso-access}
         + Logon Único de Parceiro {#partner-sso}
            + Logon único no Apple {#apple-sso}
               + [Visão geral do Apple SSO](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-overview.md)
               + [Guia do Apple SSO (REST API V2)](integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md)
         + Logon único da plataforma {#platform-sso}
            + Logon único no Amazon {#amazon-sso}
               + [Guia do Amazon SSO (REST API V2)](integration-guide-programmers/features-standard/sso-access/platform-sso/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v2.md)
            + Logon único do Roku {#roku-sso}
               + [Visão geral do SSO do Roku](integration-guide-programmers/features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-overview.md)
      + Acesso de Autenticação Baseado na Página Inicial {#hba-access}
         + [Autenticação doméstica para TV em qualquer lugar](integration-guide-programmers/features-standard/hba-access/home-based-authn-tve.md)
         + [Status de HBA para MVPDs](integration-guide-programmers/features-standard/hba-access/hba-status-mvpds.md)
      + Suporte à privacidade {#privacy-support}
         + [Visão geral do suporte de privacidade](integration-guide-programmers/features-premium/privacy-support/privacy-supp-overview.md)
         + [Como fazer uma solicitação de privacidade](integration-guide-programmers/features-premium/privacy-support/make-privacy-req.md)
   + Recursos Premium {#features-premium}
      + Acesso temporário {#temporary-access}
         + [Temp pass](integration-guide-programmers/features-premium/temporary-access/temp-pass.md)
         + [Temp pass promocional](integration-guide-programmers/features-premium/temporary-access/promotional-temp-pass.md)
         + [Redefinir aprovação temporária](integration-guide-programmers/features-premium/temporary-access/reset-temp-pass.md)
      + Acesso degradado {#degraded-access}
         + [Visão geral da API de degradação](integration-guide-programmers/features-premium/degraded-access/degradation-api-overview.md)
      + ESM {#esm}
         + [Visão geral do monitoramento do serviço de qualificação](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md)
         + [API de monitoramento do serviço de qualificação](integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)
         + [Métricas do lado do servidor](integration-guide-programmers/features-premium/esm/understanding-serverside-metrics.md)
      + Analytics {#analytics}
         + [Integração dos dados do lado do servidor de autenticação da Adobe Pass ao Adobe Analytics](integration-guide-programmers/features-premium/analytics/integrate-authn-servr-data-analytics.md)
         + [Utilização da ID de Experience Cloud na autenticação da Adobe Pass](integration-guide-programmers/features-premium/analytics/exp-cloud-id-authn.md)
   + Herdados {#legacy}
      + API REST V1 (Herdada) {#rest-api-v1}
         + [Visão geral da REST API V1 (herdada)](integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md)
         + [(Herdado) Referência da API REST V1](integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
         + APIs (Herdadas) {#rest-api-v1-apis}
            + [Solicitação de código de registro (herdado)](integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md)
            + [Registro de registro de devolução (herdado)](integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md)
            + [Excluir Registro (Herdado)](integration-guide-programmers/legacy/rest-api-v1/apis/delete-registration-record.md)
            + [(Herdado) Fornecer lista do MVPD](integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md)
            + [(Herdado) Iniciar autenticação](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md)
            + [Verificar token de autenticação (herdado)](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md)
            + [(Herdado) Recuperar token de autenticação](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md)
            + [(Herdado) Iniciar autorização](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md)
            + [(Herdado) Recuperar token de autorização](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md)
            + [(Herdado) Obtenção de token de mídia curta](integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md)
            + [(Herdado) Verificar fluxo de autenticação por Second Screen Web App](integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md)
            + [(Herdado) Recuperar lista de recursos pré-autorizados](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md)
            + [(Herdado) Recuperar lista de recursos pré-autorizados pelo aplicativo web de segunda tela](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
            + [(Herdado) Iniciar logout](integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md)
            + [Metadados de usuário (herdados)](integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)
            + [(Herdado) Recuperar solicitação de perfil](integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-profilerequest.md)
            + [Token Exchange (herdado)](integration-guide-programmers/legacy/rest-api-v1/apis/token-exchange.md)
            + [(Herdado) Visualização Gratuita para Aprovação Temporária e Aprovação Temporária Promocional](integration-guide-programmers/legacy/rest-api-v1/apis/free-preview-for-temp-pass-and-promotional-temp-pass.md)
         + Cookbooks (herdados) {#rest-api-v1-cookbooks}
            + [Cookbook REST API V1 (herdado) (cliente para servidor)](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-clienttoserver.md)
            + [Cookbook REST API V1 (herdado) (servidor para servidor)](integration-guide-programmers/legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md)
      + SDKs {#sdks} (Herdados)
         + (Herdado) JavaScript SDK {#javascript-sdk}
            + [Visão geral do JavaScript SDK (herdado)](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-overview.md)
            + [Guia do JavaScript SDK (herdado)](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-cookbook.md)
            + [(Herdado) Referência da API do JavaScript SDK](integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
            + [(Herdado) Pré-autorização da API do JavaScript SDK](integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md)
            + Diretrizes (Herdadas) {#javascript-sdk-guidelines}
               + [(Herdado) Logon e logout sem atualização](integration-guide-programmers/legacy/sdks/javascript-sdk/refreshless-login-and-logout.md)
         + (Herdado) iOS/tvOS SDK {#ios-tvos-sdk}
            + [Visão geral do iOS/tvOS SDK (herdado)](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-overview.md)
            + [Cookbook do iOS/tvOS SDK (herdado)](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md)
            + [(Herdado) Referência da API do SDK do iOS/tvOS](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
            + [(Herdado) Pré-autorização da API do SDK do iOS/tvOS](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md)
            + Diretrizes (Herdadas) {#ios-tvos-sdk-guidelines}
               + [Registro de aplicativo iOS/tvOS (herdado)](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)
               + [Guia de migração do iOS/tvOS v3.x (herdado)](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-v3x-migration-guide.md)
               + [Verificações de integridade do armazenamento iOS/tvOS (herdado)](integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-storage-integrity-checks.md)
         + (Herdado) Android SDK {#android-sdk}
            + [Visão geral do Android SDK (herdado)](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-overview.md)
            + [Guia do Android SDK (herdado)](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-cookbook.md)
            + [(Herdado) Referência da API do Android SDK](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md)
            + [(Herdado) Pré-autorização da API do Android SDK](integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md)
            + Diretrizes (Herdadas) {#android-sdk-guidelines}
               + [Registro de aplicativo Android (herdado)](integration-guide-programmers/legacy/sdks/android-sdk/android-application-registration.md)
               + [(Herdado) Android SDK com Registro de cliente dinâmico](integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-with-dynamic-client-registration.md)
         + (Herdado) FireOS SDK {#fireos-sdk}
            + [Visão geral técnica do Amazon FireOS (herdado)](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-technical-overview.md)
            + [Cookbook de integração do Amazon FireOS (herdado)](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-integration-cookbook.md)
            + [(Herdado) Referência da API do Amazon FireOS](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md)
            + [Registro de aplicativo Amazon FireOS (herdado)](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-application-registration.md)
            + [(Herdado) FireOS SDK com registro de cliente dinâmico](integration-guide-programmers/legacy/sdks/fireos-sdk/fireos-sdk-with-dynamic-client-registration.md)
            + [(Herdado) Amazon FireOS SSO - Guia de início do programador](integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-firetv-sso-programmer-kickoff-guide.md)
      + Informações do Cliente (Herdadas) {#client-information}
         + [(Herdado) Transmissão de informações do cliente (dispositivo, conexão e aplicativo)](integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md)
      + Relatório de Erros (Herdado) {#error-reporting}
         + [Relatório de erros (herdado)](integration-guide-programmers/legacy/error-reporting/error-reporting.md)
      + Acesso de Logon Único (Herdado) {#sso-access}
         + [Suporte para logon único (herdado)](integration-guide-programmers/legacy/sso-access/sso-support.md)
         + [SSO (herdado) via autenticação passiva](integration-guide-programmers/legacy/sso-access/sso-passive-authn.md)
         + [(Herdado) Guia de SSO do Amazon (REST API V1)](integration-guide-programmers/legacy/sso-access/amazon-sso-cookbook-rest-api-v1.md)
         + [(Herdado) Guia de SSO do Apple (REST API V1)](integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-rest-api-v1.md)
         + [(Herdado) Guia de SSO do Apple (iOS/tvOS SDK)](integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-iostvos-sdk.md)
      + Painel TVE {#tve-dashboard} (Herdado)
         + [Guia do usuário do Painel TVE (herdado)](integration-guide-programmers/legacy/tve-dashboard/tve-dashboard-user-guide.md)
      + Notas Técnicas (Herdadas) {#tech-notes}
         + API REST V1 (Herdada) {#rest-api-v1}
            + [Implementação da API sem cliente (herdada) - Códigos de erro/mensagens com motivo provável/causa](integration-guide-programmers/legacy/notes-technical/clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
            + [Fluxo da API sem cliente (herdado) na ausência da ID do dispositivo](integration-guide-programmers/legacy/notes-technical/clientless-api-flow-in-the-absence-of-device-id.md)
            + [(Herdado) Sem cliente: Evitar o uso de &#39;&amp;&#39;reg_code in /authenticate Request](integration-guide-programmers/legacy/notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)
            + [(Herdado) Ativação dos Serviços de Qualificações da Adobe Pass para um Programador no Xbox 360 e no Xbox One Clientless](integration-guide-programmers/legacy/notes-technical/enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
            + [Tipo de dispositivo e métricas (herdadas) sem cliente](integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
         + SDKs {#sdks} (Herdados)
            + [Perguntas e respostas sobre certificados (herdados)](integration-guide-programmers/legacy/notes-technical/certificates-qa.md)
            + [(Herdado) Compreensão de IDs de usuário](integration-guide-programmers/legacy/notes-technical/understanding-user-ids.md)
            + (Herdado) JavaScript SDK {#javascript-sdk}
               + [Avaliação de prevenção de rastreamento (herdado) - Apple Safari](integration-guide-programmers/legacy/notes-technical/tracking-prevention-assessment-apple-safari.md)
               + [Avaliação de prevenção de rastreamento (herdado) - Google Chrome](integration-guide-programmers/legacy/notes-technical/tracking-prevention-assessment-google-chrome.md)
               + [Atualizações de cookies (herdados) - Sinalizadores SameSite e Seguro](integration-guide-programmers/legacy/notes-technical/cookies-updates-samesite-and-secure-flags.md)
               + [Dicas de depuração (herdadas)](integration-guide-programmers/legacy/notes-technical/appendix-b-debugging-tips.md)
            + (Herdado) Android SDK {#android-sdk}
               + [(Herdado) Acesso Ativador Android SDK Single Sign-On (SSO) em aplicativos Android 10](integration-guide-programmers/legacy/notes-technical/access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
               + [(Herdado) Autenticação do Adobe Pass e o novo modelo de permissões &quot;Marshmallow&quot; do Android 6](integration-guide-programmers/legacy/notes-technical/adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
            + (Herdado) iOS/tvOS SDK {#ios-tvos-sdk}
               + [Suporte ao WKWebView (herdado) no iOS SDK 3.1+](integration-guide-programmers/legacy/notes-technical/wkwebview-support-on-ios-sdk-31.md)
               + [Suporte a SFSafariViewController (herdado) no iOS SDK 3.2+](integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md)
               + [SSO (herdado) no iOS ao usar o Adobe Pass Authentication Access Enabler](integration-guide-programmers/legacy/notes-technical/sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
               + [Erro de autenticação do iOS (herdado) - adobepass.ios.app Não Foi Encontrado](integration-guide-programmers/legacy/notes-technical/ios-authentication-error-adobepassiosapp-cannot-be-found.md)
               + [(Herdado) Redefinir Temp Pass no iOS](integration-guide-programmers/legacy/notes-technical/reset-temp-pass-on-ios.md)
               + [(Herdado) Depuração do AccessEnabler iOS/tvOS SDK usando logs de aplicativo do console](integration-guide-programmers/legacy/notes-technical/debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
               + [Caminho de atualização do AccessEnabler iOS/tvOS 3.7.0 (herdado)](integration-guide-programmers/legacy/notes-technical/accessenabler-iostvos-370-upgrade-path.md)
         + Experiência do Usuário (Herdada) {#user-experience}
            + [(Herdado) Como migrar a página de logon do MVPD do iFrame para pop-up](integration-guide-programmers/legacy/notes-technical/migr-mvpd-login-iframe-popup.md)
            + [Recurso de comprovação (herdado): como habilitar, solucionar problemas ou determinar o problema](integration-guide-programmers/legacy/notes-technical/preflight-feature.md)
            + [(Herdado) Permitir MVPDs na caixa de diálogo de seleção](integration-guide-programmers/legacy/notes-technical/allow-mvpd-selectn-dialog.md)
            + [(Herdado) Impedir que os MVPDs apareçam na caixa de diálogo de seleção](integration-guide-programmers/legacy/notes-technical/prevent-mvpd-selectn-dialog.md)
         + Solução de problemas (herdada) {#troubleshooting}
            + [(Herdado) usando o Charles Proxy](integration-guide-programmers/legacy/notes-technical/using-charles-proxy.md)
            + [(Herdado) Monitoramento do Adobe Pass Adobe PayTV Pass](integration-guide-programmers/legacy/notes-technical/monitoring-adobe-pay-tv-pass.md)
            + [(Herdado) Como testar fluxos de autenticação e autorização usando o site de teste da API do Adobe](integration-guide-programmers/legacy/notes-technical/test-authn-authz-flows-using-adobes-api-test-site.md)
   + [Visão geral do guia de integração do programador](integration-guide-programmers/programmer-integration-guide-overview.md)
   + [Fluxo de direitos do programador](integration-guide-programmers/entitlement-flow.md)
   + [Casos de uso do programador](integration-guide-programmers/programmer-use-cases.md)
   + [Mecanismo de limitação](integration-guide-programmers/throttling-mechanism.md)
   + [Requisitos mínimos do sistema](integration-guide-programmers/minimum-system-requirements.md)
+ Guia de Integração para MVPDs {#integration-guide-mvpds}
   + [Recursos de integração](integration-guide-mvpds/mvpd-integr-features.md)
   + [Autenticação](integration-guide-mvpds/authn-usecase.md)
   + [Autenticação usando o protocolo OAuth 2.0](integration-guide-mvpds/authn-oauth2-protocol.md)
   + [Autorização](integration-guide-mvpds/authz-usecase.md)
   + [Autorização de comprovação](integration-guide-mvpds/mvpd-preflight-authz.md)
   + [Logout do MVPD](integration-guide-mvpds/usecase-mvpd-logout.md)
   + [Troca de metadados de conteúdo](integration-guide-mvpds/mvpd-content-metadata-exchange.md)
   + [Troca de metadados de usuário](integration-guide-mvpds/mvpd-user-metadata-exchng.md)
   + [Serviço Web Proxy MVPD](integration-guide-mvpds/proxy-mvpd-webserv.md)
   + [Integração Proxy MVPD SAML](integration-guide-mvpds/proxy-mvpd-saml-int.md)
   + [Escopo do provedor de serviços](integration-guide-mvpds/serv-provider-scoping.md)
   + [Permitir endereços IP no MVPD](integration-guide-mvpds/mvpd-listing-ip-addres.md)
+ Guia Do Usuário Para O Painel TVE {#user-guide-tve-dashboard}
   + [Visão geral do painel TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md)
   + [Ambientes](/help/authentication/user-guide-tve-dashboard/tve-dashboard-environments.md)
   + [Revisar e enviar alterações](/help/authentication/user-guide-tve-dashboard/tve-dashboard-review-push-changes.md)
   + [Painel](/help/authentication/user-guide-tve-dashboard/tve-dashboard-home.md)
   + [Canais](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md)
   + [Programadores](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md)
   + [MVPDs](/help/authentication/user-guide-tve-dashboard/tve-dashboard-mvpds.md)
   + [Integrações](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md)
   + [Relatórios](/help/authentication/user-guide-tve-dashboard/tve-dashboard-reports.md)
   + [Log de alterações](/help/authentication/user-guide-tve-dashboard/tve-dashboard-changes-log.md)
+ Notas Técnicas {#tech-notes}
   + Ambientes {#environments}
      + [Compreensão dos ambientes de Adobe](notes-technical/environments/understanding-the-adobe-environments.md)
      + [Configuração do ambiente e teste na pré-qualificação](notes-technical/environments/setting-up-your-environment-and-testing-in-prequal.md)
