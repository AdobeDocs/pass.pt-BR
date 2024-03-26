---
product: adobe primetime
audience: end-user
feature: Authentication
user-guide-title: Adobe Pass Authentication
user-guide-description: O Adobe Pass Authentication é uma solução de concessão de direitos para o TV Everywhere, o qual fornece uma estrutura modular para determinar se a pessoa que solicita o acesso a um recurso possui direito a ele.
source-git-commit: 73d87de7db950f23973ad31b61409a1b711fc916
workflow-type: tm+mt
source-wordcount: '930'
ht-degree: 3%

---


# Ajuda de autenticação do Adobe Pass {#authentication}

+ [Visão geral da autenticação do Adobe Pass](home.md)
+ Conceitos de autenticação da Adobe Pass {#authentication-concepts}
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
   + REST API {#restapi}
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
   + SDK do AccessEnabler {#accessenabler-sdk}
      + SDK do JavaScript {#javascriptsdk}
         + [Visão geral do SDK do JavaScript](javascript-sdk-overview.md)
         + [Guia do SDK do JavaScript](javascript-sdk-cookbook.md)
         + [Referência da API do SDK do JavaScript](javascript-sdk-api-reference.md)
         + Diretrizes {#js-sdk-guidelines}
            + [Logon e logout sem atualização](refreshless-login-and-logout.md)
         + API JavaScript {#js-api}
            + [Pré-autorizar](js-preauthorize.md)
      + iOS/tvOS SDK {#ios-sdk}
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
         + [Visão geral do Android SDK](android-sdk-overview.md)
         + [Guia do Android SDK](android-sdk-cookbook.md)
         + [Referência da API do SDK do Android](android-sdk-api-reference.md)
         + Diretrizes {#androidguidelines}
            + [Registro do aplicativo Android](android-application-registration.md)
            + [Android SDK com registro de cliente dinâmico](android-sdk-with-dynamic-client-registration.md)
         + API do Android{#androidapi}
            + [Pré-autorizar](preauthorize-android.md)
      + SDK do Amazon FireOS {#fireossdk}
         + [Amazon FireOS SSO - Guia de início do programador](amazon-firetv-sso-programmer-kickoff-guide.md)
         + [Amazon FireOS SSO usando livro de cookies de API sem cliente](amazon-fireos-sso-using-clientless-api-cookbook.md)
         + [Visão geral técnica do Amazon FireOS](amazon-fireos-technical-overview.md)
         + [Guia de integração do Amazon FireOS](amazon-fireos-integration-cookbook.md)
         + [Referência da API do Amazon FireOS](amazon-fireos-native-client-api-reference.md)
         + [Registro do aplicativo Amazon FireOS](amazon-fireos-application-registration.md)
         + [SDK do FireOS com registro de cliente dinâmico](fireos-sdk-with-dynamic-client-registration.md)
   + Plataforma SSO {#platform-sso}
      + APPLE SSO {#apple-sso}
         + [Visão geral do Apple SSO](apple-sso-overview.md)
         + [Guia do Apple SSO (REST API)](apple-sso-cookbook-rest-api.md)
         + [Guia do Apple SSO (iOS/tvOS SDK)](apple-sso-cookbook-iostvos-sdk.md)
      + SSO do Roku {#roku-sso}
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
+ Recursos de autenticação do Adobe Pass {#auth-features}
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
   + Registro do cliente {#client-regn}
      + [Registro de cliente dinâmico](dynamic-client-registration.md)
      + [API de registro dinâmico do cliente](dynamic-client-registration-api.md)
      + [Gerenciamento dinâmico de registro de clientes](dynamic-client-registration-management.md)
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
   + Notas de versão do SDK JavaScript  {#release-notes-javascript}
      + [Notas de versão do JavaScript 4.7.0 de autenticação da Adobe Pass](authn-rn-javascript-470.md)
      + [Notas de versão do JavaScript 4.6.0 de autenticação da Adobe Pass](authn-rn-javascript-460.md)
      + [Notas de versão do JavaScript 4.4.0 de autenticação da Adobe Pass](authn-rn-javascript-440.md)
      + [Notas de versão do JavaScript 4.2.0 de autenticação da Adobe Pass](authn-rn-javascript-420.md)
      + [Notas de versão do JavaScript 4.1.1 de autenticação da Adobe Pass](authn-rn-javascript-411.md)
      + [Notas de versão do JavaScript 4.1.0 de autenticação da Adobe Pass](authn-rn-javascript-410.md)
      + [Notas de versão do JavaScript 4.0.0 de autenticação da Adobe Pass](authn-rn-javascript-400.md)
      + [Notas de versão do JavaScript 3.5.0 de autenticação da Adobe Pass](authn-rn-javascript-350.md)
   + Notas de versão do iOS/tvOS SDK  {#release-notes-ios}
      + [Notas de versão do Adobe Pass Authentication iOS / tvOS 3.9.2](authn-rn-ios-tvos-392.md)
      + [Notas de versão do Adobe Pass Authentication iOS / tvOS 3.8.4](authn-rn-ios-tvos-384.md)
      + [Notas de versão do Adobe Pass Authentication iOS / tvOS 3.8.3](authn-rn-ios-tvos-383.md)
      + [Notas de versão do Adobe Pass Authentication iOS / tvOS 3.8.2](authn-rn-ios-tvos-382.md)
      + [Notas de versão do Adobe Pass Authentication iOS / tvOS 3.8.1](authn-rn-ios-tvos-381.md)
      + [Notas de versão do Adobe Pass Authentication iOS / tvOS 3.7.0](authn-rn-ios-tvos-370.md)
   + Notas de versão do Android SDK {#release-notes-android}
      + [Notas de versão do Adobe Pass Authentication Android 3.7.3](authn-rn-android-373.md)
+ Notas técnicas {#tech-notes}
   + SDKs de autenticação da Adobe Pass {#primetime-authentication-sdks}
      + [Perguntas e respostas sobre certificados](certificates-qa.md)
      + SDK do JavaScript {#javascript}
         + [Avaliação da prevenção de rastreamento - Apple Safari](tracking-prevention-assessment-apple-safari.md)
         + [Avaliação da prevenção de rastreamento - Google Chrome](tracking-prevention-assessment-google-chrome.md)
         + [Atualizações de cookies - Sinalizadores SameSite e Seguro](cookies-updates-samesite-and-secure-flags.md)
      + Android SDK {#android}
         + [Acesse o Logon único (SSO) do SDK do Android Enabler em aplicativos Android 10](access-enabler-android-sdk-single-signon-sso-on-android-10-devices.md)
         + [Autenticação do Adobe Pass e o novo modelo de permissões &quot;Marshmallow&quot; do Android 6](adobe-primetime-authentication-and-the-android-6-marshmallow-new-permissions-model.md)
      + iOS/tvOS SDK {#iostvos}
         + [Suporte ao WKWebView no iOS SDK 3.1+](wkwebview-support-on-ios-sdk-31.md)
         + [Suporte a SFSafariViewController no iOS SDK 3.2+](sfsafariviewcontroller-support-on-ios-sdk-32.md)
         + [SSO no iOS ao usar o Adobe Pass Authentication Access Enabler](sso-on-ios-when-using-the-primetime-authentication-access-enabler.md)
         + [Erro de autenticação do iOS - adobepass.ios.app não pode ser encontrado](ios-authentication-error-adobepassiosapp-cannot-be-found.md)
         + [Redefinir Temp Pass no iOS](reset-temp-pass-on-ios.md)
         + [Depuração do SDK iOS/tvOS do AccessEnabler usando logs de aplicativo do console](debugging-the-accessenabler-iostvos-sdk-using-console-app-logs.md)
         + [Caminho de atualização do AccessEnabler iOS/tvOS 3.7.0](accessenabler-iostvos-370-upgrade-path.md)
   + Passar nos ambientes de autenticação {#primetime-authentication-environments}
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
   + Ferramentas e utilitários {#tools-and-utilities}
      + [Usando o Charles Proxy](using-charles-proxy.md)
   + Conceitos {#concepts}
      + [Compreensão das IDs de usuário](understanding-user-ids.md)
+ [Guia do usuário do Painel TVE](tve-dashboard-user-guide.md)
+ [Glossário](glossary.md)