---
title: Perguntas frequentes sobre REST API V2
description: Perguntas frequentes sobre REST API V2
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '6664'
ht-degree: 0%

---

# Perguntas frequentes sobre REST API V2 {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Este documento fornece respostas de alta visão geral para perguntas frequentes sobre a adoção da API REST V2 de autenticação da Adobe Pass.

Para obter mais informações sobre a REST API V2 como um todo, consulte a [Documentação de Visão geral da REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

>[!MORELIKETHIS]
>
> * [Perguntas frequentes sobre o Registro Dinâmico de Clientes (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)

## Perguntas frequentes gerais {#general-faqs}

Comece com esta seção se estiver trabalhando em um aplicativo que precise integrar a REST API V2, seja um aplicativo novo ou existente que migre da [REST API V1](#migration-rest-api-v1-to-rest-api-v2) ou do [SDK](#migration-sdk-to-rest-api-v2).

Para obter mais informações sobre detalhes e etapas da migração, consulte também as próximas seções.

### Perguntas frequentes sobre a fase de registro {#registration-phase-faqs-general}

+++Perguntas frequentes sobre a fase de registro

Consulte a documentação de [Perguntas frequentes sobre o Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md).

+++

### Perguntas frequentes sobre a fase de configuração {#configuration-phase-faqs-general}

+++Perguntas frequentes sobre a fase de configuração

#### 1. Qual é a finalidade da fase de configuração? {#configuration-phase-faq1}

A finalidade da Fase de configuração é fornecer ao aplicativo cliente a lista de MVPDs com os quais ele está ativamente integrado, juntamente com os detalhes de configuração salvos pela Autenticação Adobe Pass para cada MVPD.

A Fase de configuração atua como uma etapa de pré-requisito para a Fase de autenticação quando o aplicativo cliente precisa solicitar que o usuário selecione o Provedor de TV.

#### 2. A Fase de configuração é obrigatória? {#configuration-phase-faq2}

A Fase de configuração não é obrigatória, o aplicativo cliente pode ignorar essa fase nos seguintes cenários:

* O usuário já está autenticado.
* O usuário recebe acesso temporário por meio do recurso TempPass básico ou promocional.
* A autenticação do usuário expirou, mas o aplicativo cliente armazenou em cache o MVPD selecionado anteriormente como uma escolha motivada por experiência do usuário e solicita que o usuário confirme que ainda é assinante desse MVPD.

#### 3. O que é uma configuração e por quanto tempo ela é válida? {#configuration-phase-faq3}

A configuração é um termo definido no [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration).

A configuração consiste em uma lista de MVPDs que podem ser recuperados do endpoint de Configuração.

O aplicativo cliente pode usar a configuração para apresentar um componente da interface chamado &quot;Seletor&quot; ao exigir que o usuário selecione sua MVPD.

O aplicativo cliente deve atualizar a configuração antes que o usuário passe pela Fase de autenticação.

Para obter mais informações, consulte a documentação [Recuperar configuração](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

#### 4. O aplicativo cliente pode gerenciar sua própria lista de MVPDs? {#configuration-phase-faq4}

O aplicativo cliente pode gerenciar sua própria lista de MVPDs, mas é recomendável usar a configuração fornecida pela Autenticação Adobe Pass para garantir que a lista esteja atualizada e precisa.

O aplicativo cliente receberia um [erro](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) da API REST V2 de Autenticação do Adobe Pass se o usuário selecionado no MVPD não tivesse uma integração ativa com o [provedor de serviços](rest-api-v2-glossary.md#service-provider) especificado.

#### 5. O aplicativo cliente pode filtrar a lista de MVPDs? {#configuration-phase-faq5}

O aplicativo cliente pode filtrar a lista de MVPDs fornecida na resposta de configuração implementando um mecanismo personalizado com base na própria lógica de negócios e requisitos, como localização do usuário ou histórico de usuário de seleção anterior.

#### 6. O que acontece se a integração com uma MVPD for desativada e marcada como inativa? {#configuration-phase-faq6}

Quando a integração com uma MVPD é desativada e marcada como inativa, o MVPD é removido da lista de MVPDs fornecidos em outras respostas de configuração e há duas consequências importantes a serem consideradas:

* Os usuários não autenticados desse MVPD não poderão mais concluir a Fase de autenticação usando esse MVPD.
* Os usuários autenticados desse MVPD não poderão mais concluir as Fases de pré-autorização, autorização ou logout usando esse MVPD.

O aplicativo cliente receberia um [erro](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) da API REST V2 de Autenticação do Adobe Pass se o usuário selecionado no MVPD não tivesse mais uma integração ativa com o [provedor de serviços](rest-api-v2-glossary.md#service-provider) especificado.

#### 7. O que acontece se a integração com uma MVPD for habilitada novamente e marcada como ativa? {#configuration-phase-faq7}

Quando a integração com uma MVPD é habilitada e marcada como ativa, o MVPD é incluído de volta na lista de MVPDs fornecidos em outras respostas de configuração e há duas consequências importantes a serem consideradas:

* Os usuários não autenticados desse MVPD poderão concluir novamente a Fase de autenticação usando esse MVPD.
* Os usuários autenticados desse MVPD poderão concluir novamente as Fases de pré-autorização, autorização ou logout usando esse MVPD.

#### 8. Como ativar ou desativar a integração com uma MVPD? {#configuration-phase-faq8}

Esta operação pode ser concluída por meio do [Painel do TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da Adobe Pass por um dos administradores da organização ou por um representante de Autenticação da Adobe Pass que atue em seu nome.

Para obter mais detalhes, consulte a documentação do [Guia do Usuário de Integrações do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration).

+++

### Perguntas frequentes sobre a fase de autenticação {#authentication-phase-faqs-general}

+++Perguntas frequentes sobre a fase de autenticação

#### 1. Qual é a finalidade da Fase de autenticação? {#authentication-phase-faq1}

A finalidade da Fase de autenticação é fornecer ao aplicativo cliente a capacidade de verificar a identidade do usuário e obter informações de metadados do usuário.

A Fase de autenticação atua como uma etapa de pré-requisito para a Fase de pré-autorização ou Fase de autorização quando o aplicativo cliente precisa reproduzir o conteúdo.

#### 2. Como o aplicativo cliente pode saber se o usuário já está autenticado? {#authentication-phase-faq2}

O aplicativo cliente pode consultar um dos seguintes endpoints, capazes de verificar se um usuário já está autenticado e retornar informações do perfil:

* [API do endpoint de perfis](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Endpoint de perfis para API MVPD específica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Endpoint de perfis para API de código específica (autenticação)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Para obter mais detalhes, consulte os seguintes documentos:

* [Fluxo de perfis básicos realizado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Fluxo de perfis básicos realizado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 3. Como o aplicativo cliente pode obter as informações de metadados do usuário? {#authentication-phase-faq3}

O aplicativo cliente pode consultar um dos seguintes pontos de extremidade capazes de retornar informações de [metadados do usuário](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) como parte das informações do perfil:

* [API do endpoint de perfis](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Endpoint de perfis para API MVPD específica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Endpoint de perfis para API de código específica (autenticação)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Para obter mais detalhes, consulte os seguintes documentos:

* [Fluxo de perfis básicos realizado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Fluxo de perfis básicos realizado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### 4. O que é uma sessão de autenticação e por quanto tempo ela é válida? {#authentication-phase-faq4}

A sessão de autenticação é um termo definido na documentação do [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session).

A sessão de autenticação armazena informações sobre o processo de autenticação iniciado que podem ser recuperadas do endpoint de Sessões.

A sessão de autenticação é válida por um período limitado e curto especificado no momento do problema, indicando o tempo que o usuário tem para concluir o processo de autenticação antes de exigir a reinicialização do fluxo.

O aplicativo cliente pode usar a resposta da sessão de autenticação para saber como proceder com o processo de autenticação. Observe que há casos em que o usuário não precisa se autenticar, como fornecer acesso temporário, acesso degradado ou quando o usuário já está autenticado.

Para obter mais informações, consulte os seguintes documentos:

* [Criar API de sessão de autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Retomar API de sessão de autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Fluxo de autenticação básica executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 5. Qual é um código de autenticação e por quanto tempo ele é válido? {#authentication-phase-faq5}

O código de autenticação é um termo definido na documentação do [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#code).

O código de autenticação armazena um valor exclusivo gerado quando um usuário inicia o processo de autenticação e identifica exclusivamente a sessão de autenticação de um usuário até que o processo seja concluído ou até que a sessão de autenticação expire.

O código de autenticação é válido por um período limitado e curto especificado no momento do início da sessão de autenticação, indicando o tempo que o usuário tem para concluir o processo de autenticação antes de solicitar a reinicialização do fluxo.

O aplicativo cliente pode usar o código de autenticação para permitir que o usuário conclua ou retome o processo de autenticação no mesmo dispositivo ou em um segundo, considerando que a sessão de autenticação não expirou.

Para obter mais informações, consulte os seguintes documentos:

* [Criar API de sessão de autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Retomar API de sessão de autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Fluxo de autenticação básica executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### 6. Como o aplicativo cliente pode saber se o usuário digitou um código de autenticação válido e se a sessão de autenticação ainda não expirou? {#authentication-phase-faq6}

O aplicativo cliente pode validar o código de autenticação digitado pelo usuário em um aplicativo secundário (tela) enviando uma solicitação ao endpoint de Sessões responsável por recuperar as informações da sessão de autenticação associadas ao código de autenticação.

O aplicativo cliente receberia um [erro](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) se o código de autenticação fornecido fosse digitado incorretamente ou caso a sessão de autenticação expirasse.

Para obter mais informações, consulte a documentação [Recuperar sessão de autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md).

#### 7. Qual é um perfil e por quanto tempo ele é válido? {#authentication-phase-faq7}

O perfil é um termo definido no [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#profile).

O perfil armazena informações sobre a validade de autenticação do usuário, informações de metadados e muito mais que podem ser recuperadas do endpoint de Perfis.

O aplicativo cliente pode usar o perfil para saber o status de autenticação do usuário, acessar informações de metadados do usuário ou encontrar o método usado para autenticação.

Para obter mais detalhes, consulte os seguintes documentos:

* [API do endpoint de perfis](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Endpoint de perfis para API MVPD específica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Endpoint de perfis para API de código específica (autenticação)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [Fluxo de perfis básicos realizado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Fluxo de perfis básicos realizado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

O perfil é válido por um período limitado especificado quando consultado, indicando o tempo que o usuário tem uma autenticação válida antes de passar pela Fase de Autenticação novamente.

Esse período de tempo limitado, conhecido como autenticação (authN) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl), pode ser visualizado e alterado no [Painel TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da Adobe Pass por um dos administradores da organização ou por um representante de Autenticação da Adobe Pass que atue em seu nome.

Para obter mais detalhes, consulte a documentação do [Guia do Usuário de Integrações do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

+++

### Perguntas frequentes da fase de pré-autorização {#preauthorization-phase-faqs-general}

+++Perguntas frequentes sobre a fase de pré-autorização

#### 1. Qual é o objetivo da Fase de pré-autorização? {#preauthorization-phase-faq1}

O objetivo da Fase de pré-autorização é fornecer ao aplicativo cliente a capacidade de apresentar um subconjunto de recursos de seu catálogo que o usuário teria direito de acessar.

A Fase de pré-autorização pode aprimorar a experiência do usuário quando ele abre o aplicativo do cliente pela primeira vez ou navega para uma nova seção.

#### 2. A fase de pré-autorização é obrigatória? {#preauthorization-phase-faq2}

A Fase de pré-autorização não é obrigatória. O aplicativo cliente pode ignorar essa fase se quiser apresentar um catálogo de recursos sem filtrá-los primeiro com base nos direitos do usuário.

#### 3. O que é uma decisão de pré-autorização? {#preauthorization-phase-faq3}

A pré-autorização é um termo definido na documentação do [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#preauthorization), enquanto o termo de decisão também pode ser encontrado no [Glossário](rest-api-v2-glossary.md#decision).

A decisão de pré-autorização armazena informações sobre o resultado da consulta do processo de pré-autorização do MVPD que pode ser recuperado do endpoint de Pré-autorização de decisões.

O aplicativo cliente pode usar as decisões de pré-autorização para apresentar um subconjunto de recursos que as decisões do Provedor de TV (informativas) permitiriam ao usuário acessar.

Para obter mais detalhes, consulte os seguintes documentos:

* [Recuperar API de decisões de pré-autorização](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Fluxo básico de pré-autorização executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

#### 4. Por que a decisão de pré-autorização não tem um token de mídia? {#preauthorization-phase-faq4}

A decisão de pré-autorização não tem um token de mídia porque a Fase de pré-autorização não deve ser usada para reproduzir recursos, pois essa é a finalidade da Fase de autorização.

#### 5. Qual é um recurso e quais formatos são compatíveis? {#preauthorization-phase-faq5}

O recurso é um termo definido no [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource).

O recurso é um identificador exclusivo que foi acordado com os MVPDs e está associado a um conteúdo que o aplicativo cliente pode transmitir.

O identificador exclusivo do recurso pode ter dois formatos:

* Um formato de string simples, como um identificador exclusivo de um canal (marca).
* Um formato RSS de mídia (MRSS) com informações adicionais, como título, classificações e metadados de controle dos pais.

Para obter mais detalhes, consulte a documentação de [Recursos Protegidos](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### 6. Para quantos recursos o aplicativo cliente pode obter uma decisão de pré-autorização de cada vez? {#preauthorization-phase-faq6}

O aplicativo cliente pode obter uma decisão de pré-autorização para um número limitado de recursos em uma única solicitação de API, geralmente até 5, devido a condições impostas pelos MVPDs.

Este número máximo de recursos pode ser exibido e alterado após a aceitação dos MVPDs por meio do [Painel TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da Adobe Pass por um dos administradores da organização ou por um representante da Autenticação Adobe Pass que atue em seu nome.

Para obter mais detalhes, consulte a documentação do [Guia do Usuário de Integrações do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties).

+++

### Perguntas frequentes sobre a fase de autorização {#authorization-phase-faqs-general}

+++Perguntas frequentes sobre a fase de autorização

#### 1. Qual é o objetivo da fase de autorização? {#authorization-phase-faq1}

A finalidade da Fase de autorização é fornecer ao aplicativo cliente a capacidade de reproduzir recursos que o usuário solicita após validar seus direitos com o MVPD.

#### 2. A fase de autorização é obrigatória? {#authorization-phase-faq2}

A Fase de autorização é obrigatória, o aplicativo cliente não poderá ignorar essa fase se quiser reproduzir recursos que o usuário solicita, pois requer a verificação com o MVPD de que o usuário tem direito antes de liberar o fluxo.

#### 3. Qual é a decisão de autorização e por quanto tempo ela é válida? {#authorization-phase-faq3}

A autorização é um termo definido na documentação do [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#authorization), enquanto o termo de decisão também pode ser encontrado no [Glossário](rest-api-v2-glossary.md#decision).

A decisão de autorização armazena informações sobre o resultado da consulta do processo de autorização do MVPD que pode ser recuperado do endpoint de autorização de decisões.

O aplicativo cliente pode usar a decisão de autorização contendo um token de mídia para reproduzir o fluxo de recursos, caso a decisão do Provedor de TV (autoritativo) permita que o usuário acesse-o.

Para obter mais detalhes, consulte os seguintes documentos:

* [Recuperar API de decisões de autorização](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Fluxo de autorização básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

A decisão de autorização é válida por um período limitado e curto especificado no momento da emissão, indicando a quantidade de tempo que será armazenada em cache pela Autenticação Adobe Pass antes de solicitar a consulta do MVPD novamente.

Este período de tempo limitado, conhecido como autorização (authZ) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl), pode ser visualizado e alterado no [Painel TVE](rest-api-v2-glossary.md#tve-dashboard) da Adobe Pass por um dos administradores da sua organização ou por um representante da Autenticação da Adobe Pass que atue em seu nome.

Para obter mais detalhes, consulte a documentação do [Guia do Usuário de Integrações do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

#### 4. O que é um token de mídia e por quanto tempo ele é válido? {#authorization-phase-faq4}

O token de mídia é um termo definido no [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token).

O token de mídia consiste em uma sequência de caracteres assinada enviada em texto não criptografado que pode ser recuperada do endpoint da Autorização de decisões.

Para obter mais informações, consulte a documentação do [Verificador de Token de Mídia](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).

O token de mídia é válido por um período limitado e curto especificado no momento da emissão, indicando a quantidade de tempo que ele deve ser usado pelo aplicativo cliente antes de solicitar a consulta do endpoint da Autorização de Decisões novamente.

O aplicativo cliente pode usar o token de mídia para reproduzir um fluxo de recursos caso a decisão do Provedor de TV (autoritativo) permita que o usuário acesse-o.

Para obter mais detalhes, consulte os seguintes documentos:

* [Recuperar API de decisões de autorização](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Fluxo de autorização básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### 5. Qual é um recurso e quais formatos são compatíveis? {#authorization-phase-faq5}

O recurso é um termo definido no [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource).

O recurso é um identificador exclusivo que foi acordado com os MVPDs e está associado a um conteúdo que o aplicativo cliente pode transmitir.

O identificador exclusivo do recurso pode ter dois formatos:

* Um formato de string simples, como um identificador exclusivo de um canal (marca).
* Um formato RSS de mídia (MRSS) com informações adicionais, como título, classificações e metadados de controle dos pais.

Para obter mais detalhes, consulte a documentação de [Recursos Protegidos](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### 6. Para quantos recursos o aplicativo cliente pode obter uma decisão de autorização de cada vez? {#authorization-phase-faq6}

O aplicativo cliente pode obter uma decisão de autorização para um número limitado de recursos em uma única solicitação de API, geralmente até 1, devido a condições impostas pelos MVPDs.

+++

### Perguntas frequentes sobre a fase de logout {#logout-phase-faqs-general}

+++Perguntas frequentes sobre a fase de logout

#### 1. Qual é a finalidade da Fase de logout? {#logout-phase-faq1}

A finalidade da Fase de logout é fornecer ao aplicativo cliente a capacidade de encerrar o perfil autenticado do usuário na Autenticação do Adobe Pass mediante solicitação do usuário.

+++

### Perguntas frequentes sobre cabeçalhos {#headers-faqs-general}

+++Perguntas frequentes sobre cabeçalhos

#### 1. Como calcular o valor do cabeçalho de autorização? {#headers-faq1}

O cabeçalho de solicitação [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) contém o token de acesso `Bearer` exigido pelo aplicativo cliente para acessar APIs protegidas pelo Adobe Pass.

O valor do cabeçalho de Autorização deve ser obtido da Autenticação Adobe Pass durante a Fase de Registro.

Para obter mais detalhes, consulte os seguintes documentos:

* [Visão geral do registro dinâmico do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [Recuperar API de credenciais do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Recuperar API do token de acesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Fluxo de registro dinâmico do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

Caso o aplicativo cliente esteja migrando da API REST V1 para a API REST V2, o aplicativo cliente pode continuar a usar o mesmo método para obter o token de acesso `Bearer` como antes.

#### 2. Como calcular o valor do cabeçalho AP-Device-Identifier? {#headers-faq2}

O cabeçalho de solicitação [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) contém o identificador do dispositivo de streaming conforme foi criado pelo aplicativo cliente.

A documentação do cabeçalho [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) fornece alguns exemplos de como calcular o valor para plataformas diferentes, mas o aplicativo cliente pode optar por usar um método diferente com base em sua própria lógica e requisitos de negócios.

Caso o aplicativo cliente esteja migrando da REST API V1 para a REST API V2, o aplicativo cliente pode continuar a usar o mesmo método para calcular o identificador do dispositivo como antes.

#### 3. Como calcular o valor do cabeçalho X-Device-Info? {#headers-faq3}

O cabeçalho de solicitação [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) contém as informações do cliente (dispositivo, conexão e aplicativo) relacionadas ao dispositivo de streaming real.

A documentação do cabeçalho [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) fornece alguns exemplos de como calcular o valor para plataformas diferentes, mas o aplicativo cliente pode optar por usar um método diferente com base em sua própria lógica e requisitos de negócios.

Caso o aplicativo cliente esteja migrando da REST API V1 para a REST API V2, o aplicativo cliente pode continuar a usar o mesmo método para calcular as informações do dispositivo como antes.

+++

## Perguntas frequentes sobre migração {#migration-faqs}

Prossiga com esta seção se estiver trabalhando em um aplicativo que precise migrar um aplicativo existente para a REST API V2.

### Perguntas frequentes gerais sobre migração {#general-migration-faqs}

+++Perguntas frequentes sobre a migração geral

#### 1. Sou obrigado a implantar um novo aplicativo cliente migrado para a REST API V2 para todos os usuários de uma só vez? {#migration-faq1}

Não.

O aplicativo cliente não é necessário para implantar uma nova versão que integre a REST API V2 a todos os usuários simultaneamente.

A Autenticação do Adobe Pass continuará a ser compatível com versões mais antigas do aplicativo cliente integrando a REST API V1 ou o SDK até o final de 2025.

#### 2. Sou obrigado a implantar um novo aplicativo cliente migrado para a REST API V2 em todas as APIs e fluxos de uma só vez? {#migration-faq2}

Sim.

O aplicativo cliente é necessário para implantar uma nova versão que integre a REST API V2 em todas as APIs de autenticação da Adobe Pass e flui simultaneamente.

No caso do fluxo &#39;segunda autenticação de tela&#39;, o aplicativo cliente é necessário para implantar uma nova versão integrando a API REST V2 para os aplicativos [primário](rest-api-v2-glossary.md#primary-application) e [secundário](rest-api-v2-glossary.md#secondary-application) simultaneamente.

A autenticação do Adobe Pass não oferecerá suporte a implementações &quot;híbridas&quot; que integram a REST API V2 e a REST API V1/SDK entre APIs e fluxos.

#### 3. A autenticação do usuário será preservada ao atualizar para um novo aplicativo cliente migrado para a REST API V2? {#migration-faq3}

Não.

A autenticação de usuário obtida em versões anteriores do aplicativo cliente que integram a REST API V1 ou o SDK não será preservada.

Portanto, o usuário precisará autenticar novamente no novo aplicativo cliente migrado para a REST API V2.

#### 4. O aplicativo cliente pode usar os aplicativos registrados existentes (instruções de software)? {#migration-faq4}

O aplicativo cliente não pode reutilizar os aplicativos registrados existentes (instruções de software), portanto, deve gerar e baixar novos aplicativos registrados (instruções de software) dedicados ao consumo da REST API V2.

Esta operação pode ser concluída por meio do [Painel do TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da Adobe Pass por um dos administradores da organização ou por um representante de Autenticação da Adobe Pass que atue em seu nome.

Para obter mais detalhes, consulte a documentação do [Guia do Usuário de Canais do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications) ou o [Guia do Usuário de Programadores do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications).

No momento, você deverá solicitar que um representante da Autenticação da Adobe Pass habilite o uso da REST API V2 para seus novos aplicativos registrados (instruções de software), até que o [Painel do TVE](rest-api-v2-glossary.md#tve-dashboard) da Adobe Pass seja atualizado para permitir o autogerenciamento desta operação.

Para distinguir os aplicativos registrados (instruções de software) usados em aplicativos clientes que consomem a REST API V2, exigimos que você adicione um sufixo específico ao nome do aplicativo registrado, como &quot;RESTV2&quot;.

#### 5. O aplicativo cliente pode usar os esquemas personalizados existentes? {#migration-faq5}

O aplicativo cliente pode reutilizar os esquemas personalizados existentes gerados pelo [Painel TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) do Adobe Pass.

Para obter mais detalhes, consulte a documentação do [Guia do Usuário de Canais do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#custom-schemes) ou o [Guia do Usuário de Programadores do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#custom-schemes).

#### 6. Os códigos de erro aprimorados estão ativados por padrão na REST API V2? {#migration-faq6}

Sim.

Os aplicativos clientes que integram a REST API V2 se beneficiam do recurso de códigos de erro aprimorado habilitado por padrão.

Para obter mais detalhes, consulte a documentação dos [Códigos de erro aprimorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2).

+++

### Migração da REST API V1 para REST API V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

Prossiga com esta subseção se estiver trabalhando em um aplicativo que precise migrar da API REST V1 para a API REST V2.

#### Perguntas frequentes sobre a fase de registro {#registration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Perguntas frequentes sobre a fase de registro

##### 1. Quais são as migrações de API de alto nível necessárias para a fase de registro? {#registration-phase-v1-to-v2-faq1}

Na migração da REST API V1 para a REST API V2, não há alterações de alto nível em relação à Fase de registro.

O aplicativo cliente pode continuar a usar o mesmo fluxo para se registrar na Autenticação Adobe Pass por meio do processo [Registro Dinâmico de Cliente (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr) e obter um token de acesso.

Para obter mais informações, consulte os seguintes documentos:

* [Visão geral do registro dinâmico do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [Recuperar API de credenciais do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Recuperar API do token de acesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Fluxo de registro dinâmico do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

+++

#### Perguntas frequentes sobre a fase de configuração {#configuration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Perguntas frequentes sobre a fase de configuração

##### 1. Quais são as migrações de API de alto nível necessárias para a fase de configuração? {#configuration-phase-v1-to-v2-faq1}

Na migração da REST API V1 para REST API V2, há alterações de alto nível a serem consideradas que são apresentadas na tabela a seguir:

| Escopo | REST API V1 | REST API V2 | Observações |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recuperar lista de MVPDs com integração ativa | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Perguntas frequentes sobre a fase de autenticação {#authentication-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Perguntas frequentes sobre a fase de autenticação

##### 1. Quais são as migrações de API de alto nível necessárias para a Fase de autenticação? {#authentication-phase-v1-to-v2-faq1}

Na migração da REST API V1 para REST API V2, há alterações de alto nível a serem consideradas que são apresentadas na tabela a seguir:

| Escopo | REST API V1 | REST API V2 | Observações |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar código de registro (código de autenticação) | [POST <br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificar código de registro (código de autenticação) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Retomar a autenticação do (MVPD) na segunda tela (aplicativo) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Iniciar autenticação do (MVPD) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificar status de autenticação do usuário | [GET <br/> /api/v1/checkauthn (primeira tela)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn (segunda tela)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar token de autenticação de usuário (perfil) | [GET <br/> /api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar informações de metadados do usuário | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Perguntas frequentes da fase de pré-autorização {#preauthorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Perguntas frequentes sobre a fase de pré-autorização

##### 1. Quais são as migrações de API de alto nível necessárias para a fase de pré-autorização? {#preauthorization-phase-v1-to-v2-faq1}

Na migração da REST API V1 para REST API V2, há alterações de alto nível a serem consideradas que são apresentadas na tabela a seguir:

| Escopo | REST API V1 | REST API V2 | Observações |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar recursos pré-autorizados (decisões de pré-autorização) | [GET <br/> /api/v1/preauthorize (primeira tela)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/preauthorize (segunda tela)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/decision/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo básico de pré-autorização executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Perguntas frequentes sobre a fase de autorização {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Perguntas frequentes sobre a fase de autorização

##### 1. Quais são as migrações de API de alto nível necessárias para a fase de autorização? {#authorization-phase-v1-to-v2-faq1}

Na migração da REST API V1 para REST API V2, há alterações de alto nível a serem consideradas que são apresentadas na tabela a seguir:

| Escopo | REST API V1 | REST API V2 | Observações |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar autorização do (MVPD) | [GET <br/> /api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | O aplicativo cliente pode usar a resposta desta API para várias finalidades de uma só vez: <br/> <ul><li>Iniciar autorização do (MVPD)</li><li>Recuperar decisão de autorização</li><li>Recuperar token de mídia curto</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autorização básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Recuperar token de autorização (decisão de autorização) | [GET <br/> /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | O aplicativo cliente pode usar a resposta desta API para várias finalidades de uma só vez: <br/> <ul><li>Iniciar autorização do (MVPD)</li><li>Recuperar decisão de autorização</li><li>Recuperar token de mídia curto</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autorização básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Recuperar token de autorização curto (token de mídia) | [GET <br/> /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | O aplicativo cliente pode usar a resposta desta API para várias finalidades de uma só vez: <br/> <ul><li>Iniciar autorização do (MVPD)</li><li>Recuperar decisão de autorização</li><li>Recuperar token de mídia curto</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autorização básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### Perguntas frequentes sobre a fase de logout {#logout-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Perguntas frequentes sobre a fase de logout

##### 1. Quais são as migrações de API de alto nível necessárias para a Fase de logout? {#logout-phase-v1-to-v2-faq1}

Na migração da REST API V1 para REST API V2, há alterações de alto nível a serem consideradas que são apresentadas na tabela a seguir:

| Escopo | REST API V1 | REST API V2 | Observações |
|-----------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar logout | [GET <br/> /api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de logout básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### Migração do SDK para a REST API V2 {#migration-sdk-to-rest-api-v2}

Prossiga com esta subseção se estiver trabalhando em um aplicativo que precise migrar do SDK para a REST API V2.

#### Perguntas frequentes sobre a fase de registro {#registration-phase-faqs-migration-sdk-to-rest-api-v2}

+++Perguntas frequentes sobre a fase de registro

##### 1. Quais são as migrações de API de alto nível necessárias para a fase de registro? {#registration-phase-sdk-to-v2-faq1}

Na migração dos SDKs para a REST API V2, há alterações de alto nível a serem consideradas que são apresentadas nas seguintes tabelas:

###### AccessEnabler JavaScript SDK

| Escopo | SDK | REST API V2 | Observações |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Concluir o Registro de Cliente Dinâmico (DCR) | Fornecendo instrução de software ao construtor | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Visão Geral do Registro Dinâmico do Cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Fluxo de Registro Dinâmico do Cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Escopo | SDK | REST API V2 | Observações |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Concluir o Registro de Cliente Dinâmico (DCR) | Fornecendo instrução de software ao construtor | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Visão Geral do Registro Dinâmico do Cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Fluxo de Registro Dinâmico do Cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Escopo | SDK | REST API V2 | Observações |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Concluir o Registro de Cliente Dinâmico (DCR) | Fornecendo instrução de software ao construtor | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Visão Geral do Registro Dinâmico do Cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Fluxo de Registro Dinâmico do Cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Escopo | SDK | REST API V2 | Observações |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Concluir o Registro de Cliente Dinâmico (DCR) | Fornecendo instrução de software ao construtor | [POST <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Visão Geral do Registro Dinâmico do Cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Fluxo de Registro Dinâmico do Cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

#### Perguntas frequentes sobre a fase de configuração {#configuration-phase-faqs-migration-sdk-to-rest-api-v2}

+++Perguntas frequentes sobre a fase de configuração

##### 1. Quais são as migrações de API de alto nível necessárias para a fase de configuração? {#configuration-phase-sdk-to-v2-faq1}

Na migração dos SDKs para a REST API V2, há alterações de alto nível a serem consideradas que são apresentadas nas seguintes tabelas:

###### AccessEnabler JavaScript SDK

| Escopo | SDK | REST API V2 | Observações |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recuperar lista de MVPDs com integração ativa | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler iOS/tvOS SDK

| Escopo | SDK | REST API V2 | Observações |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recuperar lista de MVPDs com integração ativa | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler Android SDK

| Escopo | SDK | REST API V2 | Observações |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recuperar lista de MVPDs com integração ativa | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

###### AccessEnabler FireOS SDK

| Escopo | SDK | REST API V2 | Observações |
|------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recuperar lista de MVPDs com integração ativa | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Perguntas frequentes sobre a fase de autenticação {#authentication-phase-faqs-migration-sdk-to-rest-api-v2}

+++Perguntas frequentes sobre a fase de autenticação

##### 1. Quais são as migrações de API de alto nível necessárias para a Fase de autenticação? {#authentication-phase-sdk-to-v2-faq1}

Na migração dos SDKs para a REST API V2, há alterações de alto nível a serem consideradas que são apresentadas nas seguintes tabelas:

###### AccessEnabler JavaScript SDK

| Escopo | SDK | REST API V2 | Observações |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar autenticação do (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificar status de autenticação do usuário | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthN) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar informações de metadados do usuário | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getMeta) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler iOS SDK

| Escopo | SDK | REST API V2 | Observações |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar autenticação do (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificar status de autenticação do usuário | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar informações de metadados do usuário | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler tvOS SDK

| Escopo | SDK | REST API V2 | Observações |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar código de registro (código de autenticação) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificar código de registro (código de autenticação) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Retomar a autenticação do (MVPD) na segunda tela (aplicativo) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Iniciar autenticação do (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificar status de autenticação do usuário | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar informações de metadados do usuário | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Escopo | SDK | REST API V2 | Observações |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar autenticação do (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificar status de autenticação do usuário | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthN) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar informações de metadados do usuário | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getMetadata) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Escopo | SDK | REST API V2 | Observações |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar código de registro (código de autenticação) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificar código de registro (código de autenticação) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Retomar a autenticação do (MVPD) na segunda tela (aplicativo) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POST <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Iniciar autenticação do (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POST <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificar status de autenticação do usuário | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthN) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar informações de metadados do usuário | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getMetadata) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Perguntas frequentes da fase de pré-autorização {#preauthorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++Perguntas frequentes sobre a fase de pré-autorização

##### 1. Quais são as migrações de API de alto nível necessárias para a fase de pré-autorização? {#preauthorization-phase-sdk-to-v2-faq1}

Na migração dos SDKs para a REST API V2, há alterações de alto nível a serem consideradas que são apresentadas nas seguintes tabelas:

###### AccessEnabler JavaScript SDK

| Escopo | SDK | REST API V2 | Observações |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar recursos pré-autorizados (decisões de pré-autorização) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decision/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo básico de pré-autorização executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Escopo | SDK | REST API V2 | Observações |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar recursos pré-autorizados (decisões de pré-autorização) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decision/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo básico de pré-autorização executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Escopo | SDK | REST API V2 | Observações |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar recursos pré-autorizados (decisões de pré-autorização) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/decision/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo básico de pré-autorização executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| Escopo | SDK | REST API V2 | Observações |
|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar recursos pré-autorizados (decisões de pré-autorização) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST <br/> /api/v2/{serviceProvider}/decision/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo básico de pré-autorização executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Perguntas frequentes sobre a fase de autorização {#authorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++Perguntas frequentes sobre a fase de autorização

##### 1. Quais são as migrações de API de alto nível necessárias para a fase de autorização? {#authorization-phase-sdk-to-v2-faq1}

Na migração dos SDKs para a REST API V2, há alterações de alto nível a serem consideradas que são apresentadas nas seguintes tabelas:

###### AccessEnabler JavaScript SDK

| Escopo | SDK | REST API V2 | Observações |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar token de autorização curto (token de mídia) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | O aplicativo cliente pode usar a resposta desta API para várias finalidades de uma só vez: <br/> <ul><li>Iniciar autorização do (MVPD)</li><li>Recuperar decisão de autorização</li><li>Recuperar token de mídia curto</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autorização básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Escopo | SDK | REST API V2 | Observações |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar token de autorização curto (token de mídia) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | O aplicativo cliente pode usar a resposta desta API para várias finalidades de uma só vez: <br/> <ul><li>Iniciar autorização do (MVPD)</li><li>Recuperar decisão de autorização</li><li>Recuperar token de mídia curto</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autorização básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Escopo | SDK | REST API V2 | Observações |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar token de autorização curto (token de mídia) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | O aplicativo cliente pode usar a resposta desta API para várias finalidades de uma só vez: <br/> <ul><li>Iniciar autorização do (MVPD)</li><li>Recuperar decisão de autorização</li><li>Recuperar token de mídia curto</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autorização básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Escopo | SDK | REST API V2 | Observações |
|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar token de autorização curto (token de mídia) | [AccessEnabler.checkAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthZ) <br/> [AccessEnabler.getAuthorization](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthZ) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | O aplicativo cliente pode usar a resposta desta API para várias finalidades de uma só vez: <br/> <ul><li>Iniciar autorização do (MVPD)</li><li>Recuperar decisão de autorização</li><li>Recuperar token de mídia curto</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autorização básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### Perguntas frequentes sobre a fase de logout {#logout-phase-faqs-migration-sdk-to-rest-api-v2}

+++Perguntas frequentes sobre a fase de logout

##### 1. Quais são as migrações de API de alto nível necessárias para a Fase de logout? {#logout-phase-sdk-to-v2-faq1}

Na migração dos SDKs para a REST API V2, há alterações de alto nível a serem consideradas que são apresentadas nas seguintes tabelas:

###### AccessEnabler JavaScript SDK

| Escopo | SDK | REST API V2 | Observações |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar logout | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de logout básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Escopo | SDK | REST API V2 | Observações |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar logout | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de logout básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Escopo | SDK | REST API V2 | Observações |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar logout | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de logout básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Escopo | SDK | REST API V2 | Observações |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar logout | [AccessEnabler.logout](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#logout) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de logout básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++
