---
title: Perfis
description: Perfis
source-git-commit: edfde4b463dd8b93dd770bc47353ee8ceb6f39d2
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 0%

---

# Perfis {#profiles}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

Os perfis são criados pela [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) de Autenticação da Adobe Pass quando um usuário é autenticado com êxito com seu provedor de TV por Assinatura (MVPD).

Os tipos de perfis variam de acordo com o método de autenticação usado:

* **Normal**

  Criado por meio de autenticação básica.

* **SSO DO Apple**

  Criado por meio de logon único (SSO) usando a estrutura de conta de assinante de vídeo da Apple.

* **Plataforma SSO**

  Criado por meio de logon único (SSO) usando uma identidade de plataforma.

* **SSO do Token de Serviço**

  Criado via logon único (SSO) usando um token de serviço.

Os perfis armazenam os principais dados que permitem aos aplicativos clientes:

* Determine o status de autenticação do usuário.
* Identifique o método de autenticação usado.
* Identifique o provedor de identidade.
* Acessar [metadados do usuário](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).

Os perfis são armazenados com segurança no back-end da autenticação da Adobe Pass e vinculados ao aplicativo solicitante, ao dispositivo e ao identificador do provedor de serviços. Elas permanecem válidas por um período limitado, conforme definido pelo [Tempo de vida (TTL) de autenticação](#authentication-ttl-management).

## Gerenciamento de TTL (Time-to-Live) de autenticação {#authentication-ttl-management}

O TTL (Time-to-Live) de autenticação define por quanto tempo um usuário permanece autenticado antes de precisar autenticar novamente. Esse período é limitado e deve ser acordado com os representantes da MVPD. Os valores de TTL podem variar com base em:

* Categoria da plataforma (por exemplo, desktop, dispositivo móvel, dispositivos conectados à TV)
* Plataforma específica (por exemplo, iOS, Android, tvOS, Roku, FireTV)

O TTL de autenticação (authN) pode ser exibido e alterado no [Painel do TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da Adobe Pass por um dos administradores da organização ou por um representante da Autenticação da Adobe Pass que atue em seu nome.

Para obter mais detalhes, consulte a documentação do [Guia do Usuário de Integrações do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

## REST API V2 {#rest-api-v2}

Os perfis podem ser recuperados usando as seguintes APIs:

* [Recuperar perfis](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Recuperar perfil para mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Recuperar perfil para código específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Consulte as seções **Resposta** e **Amostras** das APIs acima para entender a estrutura dos perfis.

Para obter mais detalhes sobre como e quando integrar as APIs acima, consulte os seguintes documentos:

* [Fluxo de perfis básicos realizado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Fluxo de perfis básicos realizado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

>[!MORELIKETHIS]
>
> [Perguntas frequentes sobre a Fase de Autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)
