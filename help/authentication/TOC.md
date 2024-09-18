---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass Authentication
user-guide-description: O Adobe Pass Authentication é uma solução de concessão de direitos para o TV Everywhere, o qual fornece uma estrutura modular para determinar se a pessoa que solicita o acesso a um recurso possui direito a ele.
source-git-commit: 94fcb4e8c94330561596cd4006738c4f4d75e371
workflow-type: tm+mt
source-wordcount: '1159'
ht-degree: 2%

---


# Ajuda de autenticação do Adobe Pass {#authentication}

+ [Visão geral da autenticação do Adobe Pass](home.md)
+ Conceitos de Autenticação do Adobe Pass {#authentication-concepts}
   + [Papel técnico](technical-paper.md)
   + [Visão geral para programadores](programmer-overview.md)
   + [Visão geral do MVPD](mvpd-overview.md)
+ Guias de início rápido {#kickstart-guides}
   + [Guia de início rápido do programador](programmer-kickstart-guide.md)
   + [Guia de início rápido do MVPD](mvpd-kickstart-guide.md)
+ Guia de integração do programador {#programmer-integration-guide}
   + [Visão geral do guia de integração do programador](programmer-integration-guide-overview.md)
   + [O fluxo de direitos do programador](entitlement-flow.md)
   + [Casos de uso do programador](programmer-use-cases.md)
   + [Transmissão de informações do cliente (dispositivo, conexão e aplicativo)](passing-client-information-device-connection-and-application.md)
   + [Mecanismo de limitação](throttling-mechanism.md)
   + REST API V1 {#rest-api-v1}
      + [Visão geral da REST API](rest-api-overview.md)
      + [Cookbook da API REST (servidor para servidor)](rest-api-cookbook-servertoserver.md)
      + [Cookbook da API REST (cliente para servidor)](rest-api-cookbook-clienttoserver.md)
      + Referência da API Rest {#rest-api-reference}
         + [Referência da API REST](rest-api-reference.md)
         + [Solicitação de código de registro](registration-code-request.md)
         + [Retornar Registro de Registro](return-registration-record.md)
         + [Excluir Registro de Registro](delete-registration-record.md)
         + [Fornecer Lista MVPD](provide-mvpd-list.md)
         + [Iniciar autenticação](initiate-authentication.md)
         + [Verificar token de autenticação](check-authentication-token.md)
         + [Recuperar token de autenticação](retrieve-authentication-token.md)
         + [Iniciar autorização](initiate-authorization.md)
         + [Recuperar token de autorização](retrieve-authorization-token.md)
         + [Obter o token do Short Media](obtain-short-media-token.md)
         + [Verificar Fluxo de Autenticação por Aplicativo Web de Segunda Tela](check-authentication-flow-by-second-screen-web-app.md)
         + [Recuperar lista de recursos pré-autorizados](retrieve-list-of-preauthorized-resources.md)
         + [Recuperar lista de recursos pré-autorizados pelo aplicativo web de segunda tela](retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md)
         + [Iniciar logout](initiate-logout.md)
         + [Metadados do usuário](user-metadata.md)
         + [Recuperar solicitação de perfil](retrieve-profilerequest.md)
         + [Token Exchange](token-exchange.md)
         + [Visualização gratuita para aprovação temporária e aprovação temporária promocional](free-preview-for-temp-pass-and-promotional-temp-pass.md)
   + REST API V2 {#rest-api-v2}
      + [REST API V2 - Visão geral](./rest-api-v2/rest-api-v2-overview.md)
      + APIs {#rest-api-v2-apis}
         + [REST API V2 - APIs - Visão geral](./rest-api-v2/apis/rest-api-v2-apis-overview.md)
         + Configuração {#rest-api-v2-configuration-apis}
            + [Recuperar configuração para provedor de serviços específico](./rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md)
         + Sessões {#rest-api-v2-sessions-apis}
            + [Criar sessão de autenticação](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
            + [Retomar sessão de autenticação](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
            + [Recuperar sessão de autenticação](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
            + [Executar autenticação no agente do usuário](./rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
         + Perfis {#rest-api-v2-profiles-apis}
            + [Recuperar perfis](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
            + [Recuperar perfil para mvpd específico](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
            + [Recuperar perfil para código específico](./rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
         + Decisões {#rest-api-v2-decisions-apis}
            + [Recuperar decisões de autorização usando mvpd específico](./rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
            + [Recuperar decisões de pré-autorização usando mvpd específico](./rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
         + Sair {#rest-api-v2-logout-apis}
            + [Iniciar logout para mvpd específico](./rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)
         + Logon Único de Parceiro {#rest-api-v2-partner-single-sign-on-apis}
            + [Recuperar solicitação de autenticação do parceiro](rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-partner-authentication-request.md)
            + [Recuperar perfil usando resposta de autenticação de parceiro](rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md)
      + Fluxos {#rest-api-v2-flows}
         + [REST API V2 - Fluxos - Visão geral](./rest-api-v2/flows/rest-api-v2-flows-overview.md)
         + Fluxos de Acesso Básico {#rest-api-v2-basic-access-flows}
            + [Fluxo de perfis básicos realizado no aplicativo principal](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
            + [Fluxo de perfis básicos realizado no aplicativo secundário](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)
            + [Fluxo de autenticação básica executado no aplicativo principal](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
            + [Fluxo de autenticação básico executado no aplicativo secundário](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
            + [Fluxo de autorização básico executado no aplicativo principal](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
            + [Fluxo básico de pré-autorização executado no aplicativo principal](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
            + [Fluxo de logout básico executado no aplicativo principal](rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)
         + Fluxos de Acesso Degradados {#rest-api-v2-degraded-access-flows}
            + [Fluxos de acesso degradados](rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md)
         + Fluxos de Acesso Temporário {#rest-api-v2-temporary-access-flows}
            + [Fluxos de acesso temporário](rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
         + Fluxos de Acesso de Logon Único {#rest-api-v2-single-sign-on-access-flows}
            + [Logon único usando fluxos de parceiros](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md)
            + [Logon único usando fluxos de identidade da plataforma](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md)
            + [Logon único usando fluxos de token de serviço](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md)
            + [Fluxo de logout único](rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-logout-flow.md)
      + Apêndice {#rest-api-v2-appendix}
         + Cabeçalhos {#rest-api-v2-appendix-headers}
            + [Cabeçalho - autorização](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md)
            + [Cabeçalho - AP-Identificador de dispositivo](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)
            + [Cabeçalho - X-Device-Info](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md)
            + [Cabeçalho - AD-Service-Token](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md)
            + [Cabeçalho - Adobe-Subject-Token](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md)
            + [Cabeçalho - AP-Parceiro-Estrutura-Status](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md)
            + [Cabeçalho - AP-TempPass-Identity](./rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-temppass-identity.md)
   + SDK {#accessenabler-sdk} do AccessEnabler
      + JavaScript SDK {#javascriptsdk}
         + [Visão geral do SDK do JavaScript](javascript-sdk-overview.md)
         + [Guia do SDK do JavaScript](javascript-sdk-cookbook.md)
         + [Referência da API do SDK do JavaScript](javascript-sdk-api-reference.md)
         + Diretrizes {#js-sdk-guidelines}
            + [Logon e logout sem atualização](refreshless-login-and-logout.md)
         + API DO JavaScript {#js-api}
            + [Pré-autorizar](js-preauthorize.md)
      + SDK do iOS/tvOS {#ios-sdk}
         + [Visão geral do SDK do iOS/tvOS](iostvos-sdk-overview.md)
         + [Guia do SDK do iOS/tvOS](iostvos-sdk-cookbook.md)
         + [Referência de API do SDK do iOS/tvOS](iostvos-sdk-api-reference.md)
         + Diretrizes {#ios-tvos-sdk-guidelines}
            + [Registro de aplicativo iOS/tvOS](iostvos-application-registration.md)
            + Diretrizes de migração {#migration-guidelines}
               + [Guia de migração do iOS/tvOS v3.x](iostvos-v3x-migration-guide.md)
            + [Verificações de integridade do armazenamento iOS/tvOS](iostvos-sdk-storage-integrity-checks.md)
         + API iOS/tvOS {#ios-tvos-api}
            + [Pré-autorizar](preauthorize.md)
      + Android SDK {#androidsdk}
         + [Visão geral do SDK do Android](android-sdk-overview.md)
         + [Guia do SDK do Android](android-sdk-cookbook.md)
         + [Referência da API do SDK do Android](android-sdk-api-reference.md)
         + Diretrizes {#androidguidelines}
            + [Registro do aplicativo Android](android-application-registration.md)
            + [SDK do Android com registro dinâmico do cliente](android-sdk-with-dynamic-client-registration.md)
         + API DO Android {#androidapi}
            + [Pré-autorizar](preauthorize-android.md)
      + SDK {#fireossdk} do Amazon FireOS
         + [Amazon FireOS SSO - Guia de início do programador](amazon-firetv-sso-programmer-kickoff-guide.md)
         + [Amazon FireOS SSO usando livro de cookies de API sem cliente](amazon-fireos-sso-using-clientless-api-cookbook.md)
         + [Visão geral técnica do Amazon FireOS](amazon-fireos-technical-overview.md)
         + [Guia de integração do Amazon FireOS](amazon-fireos-integration-cookbook.md)
         + [Referência da API do Amazon FireOS](amazon-fireos-native-client-api-reference.md)
         + [Registro do aplicativo Amazon FireOS](amazon-fireos-application-registration.md)
         + [SDK do FireOS com registro de cliente dinâmico](fireos-sdk-with-dynamic-client-registration.md)
   + Plataforma SSO {#platform-sso}
      + Apple SSO {#apple-sso}
         + [Visão geral do Apple SSO](apple-sso-overview.md)
         + [Guia do Apple SSO (REST API)](apple-sso-cookbook-rest-api.md)
         + [Guia do Apple SSO (iOS/tvOS SDK)](apple-sso-cookbook-iostvos-sdk.md)
      + SSO {#roku-sso} do Roku
         + [SSO do Roku](roku-sso-overview.md)
   + Metadados de conteúdo {#content-metadata}
      + [Identificação do recurso protegido](identify-protected-resources.md)
   + Integração do servidor de conteúdo {#content-serv-int}
      + [Como integrar o Verificador de token de mídia](media-token-verifier-int.md)
   + Apêndices {#appendices}
      + [Dicas de depuração](appendix-b-debugging-tips.md)
+ Guia de integração do MVPD {#mvpd-int-guide}
   + [Recursos de integração](mvpd-integr-features.md)
   + [Autenticação](authn-usecase.md)
   + [Autenticação usando o protocolo OAuth 2.0](authn-oauth2-protocol.md)
   + [Autorização](authz-usecase.md)
   + [Autorização de comprovação](mvpd-preflight-authz.md)
   + [Logout do MVPD](usecase-mvpd-logout.md)
   + [Troca de metadados de conteúdo](mvpd-content-metadata-exchange.md)
   + [Troca de metadados de usuário](mvpd-user-metadata-exchng.md)
   + [Serviço Web Proxy MVPD](proxy-mvpd-webserv.md)
   + [Integração Proxy MVPD SAML](proxy-mvpd-saml-int.md)
   + [Escopo do provedor de serviços](serv-provider-scoping.md)
   + [Permitir endereços IP de MVPD](mvpd-listing-ip-addres.md)
+ Recursos de Autenticação Adobe Pass {#auth-features}
   + Integração do Adobe Analytics {#analytics-int}
      + [Integração dos dados do lado do servidor de autenticação da Adobe Pass ao Adobe Analytics](integrate-authn-servr-data-analytics.md)
      + [Utilização da ID de Experience Cloud na autenticação da Adobe Pass](exp-cloud-id-authn.md)
   + Monitoramento do serviço de qualificação {#entitlement-service-monitoring}
      + [Visão geral do monitoramento do serviço de qualificação](entitlement-service-monitoring-overview.md)
      + [API de monitoramento do serviço de qualificação](entitlement-service-monitoring-api.md)
      + [Métricas do lado do servidor](understanding-serverside-metrics.md)
   + Temp pass {#temp-pass}
      + [Temp pass](temp-pass.md)
      + [Temp pass promocional](promotional-temp-pass.md)
      + [Redefinir aprovação temporária](reset-temp-pass.md)
   + Logon único {#sso}
      + [Suporte para logon único](sso-support.md)
      + [SSO via autenticação passiva](sso-passive-authn.md)
   + Autenticação baseada em casa {#home-based-auth}
      + [Autenticação doméstica para TV em qualquer lugar](home-based-authn-tve.md)
      + [Status de HBA para MVPDs](hba-status-mvpds.md)
   + Metadados do usuário {#user-metadat}
      + [Metadados do usuário](user-metadata-feature.md)
   + [Autorização de comprovação](preflight-authz.md)
   + Relatório de erros {#error-reportn}
      + [Relatório de erros](error-reporting.md)
      + [Códigos de erro aprimorados](enhanced-error-codes.md)
   + Registro de Cliente {#dcr-api}
      + [Visão geral do registro dinâmico do cliente](./dcr-api/dynamic-client-registration-overview.md)
      + APIs {#dcr-api-apis}
         + [Recuperar credenciais do cliente](./dcr-api/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
         + [Recuperar token de acesso](./dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md)
      + Fluxos {#dcr-api-flows}
         + [Fluxo dinâmico de registro de cliente](./dcr-api/flows/dynamic-client-registration-flow.md)
   + Serviço de degradação {#degrn-service}
      + [Visão geral da API de degradação](degradation-api-overview.md)
   + Disponibilidade de privacidade {#privacy-readiness}
      + [Visão geral do suporte de privacidade](privacy-supp-overview.md)
      + [Como fazer uma solicitação de privacidade](make-privacy-req.md)
+ Dicas e solução de problemas {#tips-troubleshoot}
   + [Permitir MVPDs na caixa de diálogo de seleção](allow-mvpd-selectn-dialog.md)
   + [Impedir que os MVPDs apareçam na caixa de diálogo de seleção](prevent-mvpd-selectn-dialog.md)
+ Suporte {#support}
   + [Procedimentos de escalonamento](escalation-procedures.md)
   + [Monitoramento do Adobe Pass Adobe PayTV Pass](monitoring-adobe-pay-tv-pass.md)
   + [Requisitos mínimos do sistema](minimum-system-requirements.md)
+ Notas de versão {#release-notes}
   + [Notas de versão da Autenticação Adobe Pass 3.0](auth-rn-300.md)
   + [Notas de versão da Autenticação Adobe Pass 2.70](auth-rn-270.md)
   + [Notas de versão da Autenticação Adobe Pass 2.69](auth-rn-269.md)
   + [Notas de versão da Autenticação Adobe Pass 2.68](auth-rn-268.md)
   + [Notas de versão da Autenticação Adobe Pass 2.67](auth-rn-267.md)
   + [Notas de versão da Autenticação Adobe Pass 2.66](auth-rn-266.md)
   + [Notas de versão do Adobe Pass Authentication 2.65.1](auth-rn-2651.md)
   + [Notas de versão da Autenticação Adobe Pass 2.65](auth-rn-265.md)
   + [Notas de versão do Adobe Pass Authentication 2.64.1](auth-rn-2641.md)
   + [Notas de versão da Autenticação Adobe Pass 2.64](auth-rn-264.md)
   + [Notas de versão da Autenticação Adobe Pass 2.63](auth-rn-263.md)
   + [Notas de versão do Adobe Pass Authentication 2.62.1](auth-rn-2621.md)
   + Notas de Versão do SDK do JavaScript {#release-notes-javascript}
      + [Notas de versão do JavaScript 4.7.0 de autenticação da Adobe Pass](authn-rn-javascript-470.md)
      + [Notas de versão do JavaScript 4.6.0 de autenticação da Adobe Pass](authn-rn-javascript-460.md)
      + [Notas de versão do JavaScript 4.4.0 de autenticação da Adobe Pass](authn-rn-javascript-440.md)
      + [Notas de versão do JavaScript 4.2.0 de autenticação da Adobe Pass](authn-rn-javascript-420.md)
      + [Notas de versão do JavaScript 4.1.1 de autenticação da Adobe Pass](authn-rn-javascript-411.md)
      + [Notas de versão do JavaScript 4.1.0 de autenticação da Adobe Pass](authn-rn-javascript-410.md)
      + [Notas de versão do JavaScript 4.0.0 de autenticação da Adobe Pass](authn-rn-javascript-400.md)
      + [Notas de versão do Adobe Pass Authentication JavaScript 3.5.0](authn-rn-javascript-350.md)
   + Notas de Versão do SDK iOS/tvOS {#release-notes-ios}
      + [Notas de versão do Adobe Pass Authentication iOS / tvOS 3.9.2](authn-rn-ios-tvos-392.md)
      + [Notas de versão do Adobe Pass Authentication iOS / tvOS 3.8.4](authn-rn-ios-tvos-384.md)
      + [Notas de versão do Adobe Pass Authentication iOS / tvOS 3.8.3](authn-rn-ios-tvos-383.md)
      + [Notas de versão do Adobe Pass Authentication iOS / tvOS 3.8.2](authn-rn-ios-tvos-382.md)
      + [Notas de versão do Adobe Pass Authentication iOS / tvOS 3.8.1](authn-rn-ios-tvos-381.md)
      + [Notas de versão do Adobe Pass Authentication iOS / tvOS 3.7.0](authn-rn-ios-tvos-370.md)
   + Notas de Versão do SDK do Android {#release-notes-android}
      + [Notas de versão do Adobe Pass Authentication Android 3.7.3](authn-rn-android-373.md)
+ Notas Técnicas {#tech-notes}
   + SDKs de Autenticação do Adobe Pass {#primetime-authentication-sdks}
      + [Perguntas e respostas sobre certificados](certificates-qa.md)
      + JavaScript SDK {#javascript}
         + [Avaliação da prevenção de rastreamento - Apple Safari](tracking-prevention-assessment-apple-safari.md)
         + [Avaliação da prevenção de rastreamento - Google Chrome](tracking-prevention-assessment-google-chrome.md)
         + [Atualizações de cookies - Sinalizadores SameSite e Seguro](cookies-updates-samesite-and-secure-flags.md)
      + Android SDK {#android}
         + [Acesse o Logon único (SSO) do SDK do Android Enabler nos aplicativos do Android 10](access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Autenticação do Adobe Pass e o novo modelo de permissões &quot;Marshmallow&quot; do Android 6](adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + SDK do iOS/tvOS {#iostvos}
         + [Suporte ao WKWebView no iOS SDK 3.1+](wkwebview-support-on-ios-sdk-31.md)
         + [Suporte a SFSafariViewController no iOS SDK 3.2+](sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [SSO no iOS ao usar o Adobe Pass Authentication Access Enabler](sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [Erro de autenticação do iOS - adobepass.ios.app não pode ser encontrado](ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [Redefinir Temp Pass no iOS](reset-temp-pass-on-ios.md)
         + [Depuração do SDK iOS/tvOS do AccessEnabler usando logs de aplicativo do console](debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [Caminho de atualização do AccessEnabler iOS/tvOS 3.7.0](accessenabler-iostvos-370-upgrade-path.md)
   + Ambientes de autenticação de aprovação {#primetime-authentication-environments}
      + [Compreensão dos ambientes de Adobe](understanding-the-adobe-environments.md)
      + [Configuração do ambiente e teste na pré-qualificação](setting-up-your-environment-and-testing-in-prequal.md)
      + [Como testar os fluxos de autenticação e autorização usando o site de teste da API do Adobe](test-authn-authz-flows-using-adobes-api-test-site.md)
   + API sem cliente {#clientless-api}
      + [Implementação da API sem cliente - códigos de erro/mensagens com motivo provável/causa](clientless-api-implementation-error-codes-messages-with-probable-reason-cause.md)
      + [Fluxo da API sem cliente na ausência de ID do dispositivo](clientless-api-flow-in-the-absence-of-device-id.md)
      + [Sem clientes: Evite usar &#39;&amp;&#39;reg_code&#39; na solicitação /authenticate](clientless-avoid-using-reg-code-in-authenticate-request.md)
      + [Ativação dos Serviços de Qualificações da Adobe Pass para um Programador no Xbox 360 e no Xbox One Clientless](enabling-primetime-entitlement-services-for-a-programmer-on-xbox-360-and-xboxone-clientless-solution.md)
      + [Tipo de dispositivo e métricas sem cliente](benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)
   + Experiência do usuário {#user-exp}
      + [Como migrar a página de logon do MVPD do iFrame para o pop-up](migr-mvpd-login-iframe-popup.md)
      + [Recurso de comprovação: como ativar, solucionar problemas ou determinar o problema](preflight-feature.md)
   + Ferramentas e Utilitários {#tools-and-utilities}
      + [Usando o Charles Proxy](using-charles-proxy.md)
   + Conceitos {#concepts}
      + [Compreensão das IDs de usuário](understanding-user-ids.md)
+ [Guia do usuário do Painel TVE](tve-dashboard/old-tve-dashboard/tve-dashboard-user-guide.md)
+ Novo guia do usuário do Painel TVE {#user-guide}
   + [Visão geral do painel TVE](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-overview.md)
   + [Ambientes](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-environments.md)
   + [Revisar e enviar alterações](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-review-push-changes.md)
   + [Painel](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-home.md)
   + [Canais](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-channels.md)
   + [Programadores](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-programmers.md)
   + [MVPDs](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-mvpds.md)
   + [Integrações](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-integrations.md)
   + [Relatórios](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-reports.md)
   + [Log de alterações](/help/authentication/tve-dashboard/new-tve-dashboard/tve-dashboard-changes-log.md)
+ [Glossário](glossary.md)
