---
title: Perguntas frequentes sobre REST API V2
description: Perguntas frequentes sobre REST API V2
exl-id: 2dd74b47-126e-487b-b467-c16fa8cc14c1
source-git-commit: 0b8ef6c6b326d1a9de52b24823886c708c2aad33
workflow-type: tm+mt
source-wordcount: '9682'
ht-degree: 0%

---

# Perguntas frequentes sobre REST API V2 {#rest-api-v2-faqs}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

Este documento fornece respostas de alta visão geral para perguntas frequentes sobre a adoção da API REST V2 de autenticação da Adobe Pass.

Para obter mais informações sobre a REST API V2 como um todo, consulte a [Documentação de Visão geral da REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md).

## Perguntas frequentes gerais {#general-faqs}

Comece com esta seção se estiver trabalhando em um aplicativo que precise integrar a REST API V2, seja um aplicativo novo ou existente que migre da [REST API V1](#migration-rest-api-v1-to-rest-api-v2) ou do [SDK](#migration-sdk-to-rest-api-v2).

Para obter mais informações sobre detalhes e etapas da migração, consulte também as próximas seções.

### Perguntas frequentes sobre a fase de registro {#registration-phase-faqs-general}

+++Perguntas frequentes sobre a fase de registro

Consulte a documentação de [Perguntas frequentes sobre o Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#rest-api-v2-access-faqs).

+++

### Perguntas frequentes sobre a fase de configuração {#configuration-phase-faqs-general}

+++Perguntas frequentes sobre a fase de configuração

#### &#x200B;1. Qual é a finalidade da fase de configuração? {#configuration-phase-faq1}

A finalidade da Fase de Configuração é fornecer ao aplicativo cliente a lista de MVPDs com os quais ele está ativamente integrado, juntamente com os detalhes de configuração (por exemplo, `id`, `displayName`, `logoUrl`, etc.) salvos pela Autenticação Adobe Pass para cada MVPD.

A Fase de configuração atua como uma etapa de pré-requisito para a Fase de autenticação quando o aplicativo cliente precisa solicitar que o usuário selecione o Provedor de TV.

#### &#x200B;2. A Fase de configuração é obrigatória? {#configuration-phase-faq2}

A Fase de configuração não é obrigatória. O aplicativo cliente deve recuperar a configuração somente quando o usuário precisar selecionar o MVPD para autenticar ou autenticar novamente.

O aplicativo cliente pode ignorar esta fase nos seguintes cenários:

* O usuário já está autenticado.
* O usuário recebe acesso temporário por meio do recurso [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) básico ou promocional.
* A autenticação do usuário expirou, mas o aplicativo cliente armazenou em cache o MVPD selecionado anteriormente como uma escolha motivada por experiência do usuário e solicita que o usuário confirme que ainda é assinante desse MVPD.

#### &#x200B;3. O que é uma configuração e por quanto tempo ela é válida? {#configuration-phase-faq3}

A configuração é um termo definido no [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#configuration).

A configuração contém uma lista de MVPDs definidos pelos seguintes atributos `id`, `displayName`, `logoUrl`, etc., que podem ser recuperados do ponto de extremidade [Configuração](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

O aplicativo cliente deve recuperar a configuração somente quando o usuário precisar selecionar o MVPD para autenticar ou autenticar novamente.

O aplicativo cliente pode usar a resposta de configuração para apresentar um seletor de interface do usuário com opções de MVPD disponíveis sempre que o usuário precisar selecionar seu provedor de TV.

O aplicativo cliente deve armazenar o identificador MVPD selecionado pelo usuário, conforme especificado pelo atributo de nível de configuração `id` da MVPD, para continuar com as fases de Autenticação, Pré-autorização, Autorização ou Logout.

Para obter mais informações, consulte a documentação [Recuperar configuração](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

#### &#x200B;4. A configuração é específica de um provedor de serviços, plataforma ou usuário? {#configuration-phase-faq4}

A configuração é específica para um [provedor de serviços](rest-api-v2-glossary.md#service-provider).

A configuração é específica para um tipo de plataforma.

A configuração não é específica de um usuário.

Para aplicativos cliente que usam uma arquitetura de servidor para servidor, é recomendável armazenar em cache a resposta da configuração (por exemplo, com um TTL de 2 minutos) para cada tipo de plataforma em armazenamento de memória do lado do servidor. Isso reduz solicitações desnecessárias para cada usuário e melhora a experiência geral do usuário.

#### &#x200B;5. O aplicativo cliente deve armazenar em cache as informações de resposta da configuração em um armazenamento persistente? {#configuration-phase-faq5}

>[!IMPORTANT]
> 
> Para aplicativos cliente que usam uma arquitetura de servidor para servidor, é recomendável armazenar em cache a resposta da configuração (por exemplo, com um TTL de 2 minutos) para cada tipo de plataforma em armazenamento de memória do lado do servidor. Isso reduz solicitações desnecessárias para cada usuário e melhora a experiência geral do usuário.

O aplicativo cliente deve recuperar a configuração somente quando o usuário precisar selecionar o MVPD para autenticar ou autenticar novamente.

O aplicativo cliente deve armazenar as informações de resposta da configuração em cache em um armazenamento de memória para evitar solicitações desnecessárias e melhorar a experiência do usuário quando:

* O usuário já está autenticado.
* O usuário recebe acesso temporário por meio do recurso [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) básico ou promocional.
* A autenticação do usuário expirou, mas o aplicativo cliente armazenou em cache o MVPD selecionado anteriormente como uma escolha motivada por experiência do usuário e solicita que o usuário confirme que ainda é assinante desse MVPD.

#### &#x200B;6. O aplicativo cliente pode gerenciar sua própria lista de MVPDs? {#configuration-phase-faq6}

O aplicativo cliente pode gerenciar sua própria lista de MVPDs, mas seria necessário manter os identificadores do MVPD sincronizados com a Autenticação do Adobe Pass. Portanto, é recomendável usar a configuração fornecida pela Autenticação Adobe Pass para garantir que a lista esteja atualizada e precisa.

O aplicativo cliente receberia um [erro](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) da API REST V2 de Autenticação do Adobe Pass se o identificador do MVPD fornecido fosse inválido ou caso não tivesse uma integração ativa com o [provedor de serviços](rest-api-v2-glossary.md#service-provider) especificado.

#### &#x200B;7. O aplicativo cliente pode filtrar a lista de MVPDs? {#configuration-phase-faq7}

O aplicativo cliente pode filtrar a lista de MVPDs fornecida na resposta de configuração implementando um mecanismo personalizado com base em sua própria lógica de negócios e requisitos, como localização do usuário ou histórico de usuário de seleção anterior.

O aplicativo cliente pode filtrar a lista de [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) MVPDs ou os MVPDs que ainda têm sua integração em desenvolvimento ou teste.

#### &#x200B;8. O que acontece se a integração com uma MVPD for desativada e marcada como inativa? {#configuration-phase-faq8}

Quando a integração com uma MVPD é desativada e marcada como inativa, o MVPD é removido da lista de MVPDs fornecidos em outras respostas de configuração e há duas consequências importantes a serem consideradas:

* Os usuários não autenticados desse MVPD não poderão mais concluir a Fase de autenticação usando esse MVPD.
* Os usuários autenticados desse MVPD não poderão mais concluir as Fases de pré-autorização, autorização ou logout usando esse MVPD.

O aplicativo cliente receberia um [erro](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) da API REST V2 de Autenticação do Adobe Pass se o usuário selecionado no MVPD não tivesse mais uma integração ativa com o [provedor de serviços](rest-api-v2-glossary.md#service-provider) especificado.

#### &#x200B;9. O que acontece se a integração com uma MVPD for habilitada novamente e marcada como ativa? {#configuration-phase-faq9}

Quando a integração com uma MVPD é habilitada e marcada como ativa, o MVPD é incluído de volta na lista de MVPDs fornecidos em outras respostas de configuração e há duas consequências importantes a serem consideradas:

* Os usuários não autenticados desse MVPD poderão concluir novamente a Fase de autenticação usando esse MVPD.
* Os usuários autenticados desse MVPD poderão concluir novamente as Fases de pré-autorização, autorização ou logout usando esse MVPD.

#### &#x200B;10. Como ativar ou desativar a integração com uma MVPD? {#configuration-phase-faq10}

Esta operação pode ser concluída por meio do [Painel do TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da Adobe Pass por um dos administradores da organização ou por um representante de Autenticação da Adobe Pass que atue em seu nome.

Para obter mais detalhes, consulte a documentação do [Guia do Usuário de Integrações do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#disable-integration).

+++

### Perguntas frequentes sobre a fase de autenticação {#authentication-phase-faqs-general}

+++Perguntas frequentes sobre a fase de autenticação

#### &#x200B;1. Qual é a finalidade da Fase de autenticação? {#authentication-phase-faq1}

A finalidade da Fase de autenticação é fornecer ao aplicativo cliente a capacidade de verificar a identidade do usuário e obter informações de metadados do usuário.

A Fase de autenticação atua como uma etapa de pré-requisito para a Fase de pré-autorização ou Fase de autorização quando o aplicativo cliente precisa reproduzir o conteúdo.

#### &#x200B;2. A Fase de autenticação é obrigatória? {#authentication-phase-faq2}

A Fase de autenticação é obrigatória. O aplicativo cliente deve autenticar o usuário quando ele não tiver um perfil válido na Autenticação do Adobe Pass.

O aplicativo cliente pode ignorar esta fase nos seguintes cenários:

* O usuário já está autenticado e o perfil ainda é válido.
* O usuário recebe acesso temporário por meio do recurso [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) básico ou promocional.

O tratamento de erros de aplicativo cliente requer o tratamento de códigos de [erro](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) (por exemplo, `authenticated_profile_missing`, `authenticated_profile_expired`, `authenticated_profile_invalidated`, etc.), que indicam que o aplicativo cliente requer a autenticação do usuário.

#### &#x200B;3. O que é uma sessão de autenticação e por quanto tempo ela é válida? {#authentication-phase-faq3}

A sessão de autenticação é um termo definido na documentação do [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#session).

A sessão de autenticação armazena informações sobre o processo de autenticação iniciado que podem ser recuperadas do endpoint de Sessões.

A sessão de autenticação é válida por um período curto e limitado especificado no momento da emissão pelo carimbo de data/hora `notAfter`, indicando o tempo que o usuário tem para concluir o processo de autenticação antes de exigir a reinicialização do fluxo.

O aplicativo cliente pode usar a resposta da sessão de autenticação para saber como proceder com o processo de autenticação. Observe que há casos em que o usuário não precisa se autenticar, como fornecer acesso temporário, acesso degradado ou quando o usuário já está autenticado.

Para obter mais informações, consulte os seguintes documentos:

* [Criar API de sessão de autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Retomar API de sessão de autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Fluxo de autenticação básica executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### &#x200B;4. Qual é um código de autenticação e por quanto tempo ele é válido? {#authentication-phase-faq4}

O código de autenticação é um termo definido na documentação do [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#code).

O código de autenticação armazena um valor exclusivo gerado quando um usuário inicia o processo de autenticação e identifica exclusivamente a sessão de autenticação de um usuário até que o processo seja concluído ou até que a sessão de autenticação expire.

O código de autenticação é válido por um período curto e limitado especificado no momento do início da sessão de autenticação pelo carimbo de data/hora `notAfter`, indicando o tempo que o usuário tem para concluir o processo de autenticação antes de solicitar a reinicialização do fluxo.

O aplicativo cliente pode usar o código de autenticação para verificar se o usuário concluiu com êxito a autenticação e recuperar as informações de perfil do usuário, normalmente por meio de um mecanismo de pesquisa.

O aplicativo cliente também pode usar o código de autenticação para permitir que o usuário conclua ou retome o processo de autenticação no mesmo dispositivo ou em um segundo (tela), considerando que a sessão de autenticação não expirou.

Para obter mais informações, consulte os seguintes documentos:

* [Criar API de sessão de autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Retomar API de sessão de autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Fluxo de autenticação básica executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)

#### &#x200B;5. Como o aplicativo cliente pode saber se o usuário digitou um código de autenticação válido e se a sessão de autenticação ainda não expirou? {#authentication-phase-faq5}

O aplicativo cliente pode validar o código de autenticação digitado pelo usuário em um aplicativo secundário (tela) enviando uma solicitação a um dos pontos de extremidade das sessões responsáveis por retomar a sessão de autenticação ou recuperar as informações da sessão de autenticação associadas ao código de autenticação.

O aplicativo cliente receberia um [erro](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) se o código de autenticação fornecido fosse digitado incorretamente ou caso a sessão de autenticação expirasse.

Para obter mais informações, consulte os documentos [Retomar sessão de autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) e [Recuperar sessão de autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md).

#### &#x200B;6. Como o aplicativo cliente pode saber se o usuário já está autenticado? {#authentication-phase-faq6}

O aplicativo cliente pode consultar um dos seguintes endpoints, capazes de verificar se um usuário já está autenticado e retornar informações do perfil:

* [API do endpoint de perfis](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Endpoint de perfis para API MVPD específica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Endpoint de perfis para API de código específica (autenticação)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Para obter mais detalhes, consulte os seguintes documentos:

* [Fluxo de perfis básicos realizado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Fluxo de perfis básicos realizado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

#### &#x200B;7. Qual é um perfil e por quanto tempo ele é válido? {#authentication-phase-faq7}

O perfil é um termo definido no [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#profile).

O perfil armazena informações sobre a validade de autenticação do usuário, informações de metadados e muito mais que podem ser recuperadas do endpoint de Perfis.

O aplicativo cliente pode usar o perfil para saber o status de autenticação do usuário, acessar informações de metadados do usuário, encontrar o método usado para autenticar ou a entidade usada para fornecer a identidade.

Para obter mais detalhes, consulte os seguintes documentos:

* [API do endpoint de perfis](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Endpoint de perfis para API MVPD específica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Endpoint de perfis para API de código específica (autenticação)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)
* [Fluxo de perfis básicos realizado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Fluxo de perfis básicos realizado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

O perfil é válido por um período limitado especificado quando consultado pelo carimbo de data/hora `notAfter`, indicando o tempo que o usuário tem uma autenticação válida antes de passar pela Fase de Autenticação novamente.

Esse período de tempo limitado, conhecido como autenticação (authN) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl), pode ser visualizado e alterado no [Painel TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da Adobe Pass por um dos administradores da organização ou por um representante de Autenticação da Adobe Pass que atue em seu nome.

Para obter mais detalhes, consulte a documentação do [Guia do Usuário de Integrações do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

#### &#x200B;8. O aplicativo cliente deve armazenar em cache as informações de perfil do usuário em um armazenamento persistente? {#authentication-phase-faq8}

O aplicativo cliente deve armazenar em cache partes das informações de perfil do usuário em um armazenamento persistente para evitar solicitações desnecessárias e melhorar a experiência do usuário considerando os seguintes aspectos:

| Atributo | Experiência do usuário |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `mvpd` | O aplicativo cliente pode usá-lo para rastrear o provedor de TV selecionado pelo usuário e continuar a usá-lo durante as Fases de Pré-autorização ou Autorização.<br/><br/>Quando o perfil de usuário atual expira, o aplicativo cliente pode usar a seleção MVPD lembrada e solicitar apenas que o usuário confirme. |
| `attributes` | O aplicativo cliente pode usar isso para personalizar a experiência do usuário com base em diferentes chaves [de metadados de usuário](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) (por exemplo, `zip`, `maxRating` etc.).<br/><br/>Os metadados do usuário ficam disponíveis após a conclusão do fluxo de autenticação, portanto, o aplicativo cliente não precisa consultar um ponto de extremidade separado para recuperar as informações de [metadados do usuário](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md), pois elas já estão incluídas nas informações do perfil.<br/><br/>Determinados atributos de metadados podem ser atualizados durante o fluxo de autorização, dependendo do MVPD e do atributo de metadados específico. Como resultado, o aplicativo cliente pode precisar consultar as APIs de perfis novamente para recuperar os metadados do usuário mais recentes. |
| `notAfter` | O aplicativo cliente pode usar isso para rastrear a data de expiração do perfil do usuário.<br/><br/>A manipulação de erros do aplicativo cliente requer a manipulação de códigos de [erro](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2) (por exemplo, `authenticated_profile_missing`, `authenticated_profile_expired`, `authenticated_profile_invalidated`, etc.), o que indica que o aplicativo cliente requer a autenticação do usuário. |

#### &#x200B;9. O aplicativo cliente pode estender o perfil do usuário sem exigir reautenticação? {#authentication-phase-faq9}

Não.

O perfil do usuário não pode ser estendido além de sua validade sem interação do usuário, pois sua expiração é determinada pelo TTL de autenticação estabelecido com MVPDs.

Portanto, o aplicativo cliente deve solicitar que o usuário se autentique novamente e interaja com a página de logon do MVPD para atualizar seu perfil no sistema.

No entanto, para os MVPDs que oferecem suporte à [autenticação baseada em página inicial](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md) (HBA), o usuário não precisará inserir credenciais.

#### &#x200B;10. Quais são os casos de uso para cada endpoint de Perfis disponível? {#authentication-phase-faq10}

Os endpoints básicos de Perfis são projetados para fornecer ao aplicativo cliente a capacidade de saber o status de autenticação do usuário, acessar informações de metadados do usuário, encontrar o método usado para autenticar ou a entidade usada para fornecer identidade.

Cada endpoint se adapta a um caso de uso específico, da seguinte maneira:

| API | Descrição | Caso de uso |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [API de ponto de extremidade de perfis](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) | Recuperar todos os perfis de usuário. | **O usuário abre o aplicativo cliente pela primeira vez**<br/><br/> Neste cenário, o aplicativo cliente não tem o identificador MVPD selecionado pelo usuário em cache no armazenamento persistente.<br/><br/>Como resultado, ele enviará uma única solicitação para recuperar todos os perfis de usuário disponíveis. |
| [Ponto de extremidade de perfis para a API do MVPD específica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) | Recupere o perfil de usuário associado a uma MVPD específica. | **O usuário retorna ao aplicativo cliente após a autenticação em uma visita anterior**<br/><br/> Nesse caso, o aplicativo cliente deve ter o identificador MVPD selecionado anteriormente pelo usuário em cache no armazenamento persistente.<br/><br/>Como resultado, ele enviará uma única solicitação para recuperar o perfil do usuário para essa MVPD específica. |
| [Ponto de extremidade de perfis para API de código (autenticação) específica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | Recupere o perfil de usuário associado a um código de autenticação específico. | **O usuário inicia o processo de autenticação**<br/><br/> Neste cenário, o aplicativo cliente deve determinar se o usuário concluiu com êxito a autenticação e recuperar suas informações de perfil.<br/><br/>Como resultado, ele iniciará um mecanismo de sondagem para recuperar o perfil do usuário associado ao código de autenticação. |

Para obter mais detalhes, consulte o [Fluxo de perfis básicos executado no aplicativo primário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md) e o [Fluxo de perfis básicos executado em documentos do aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md).

O endpoint de SSO de Perfis atende a uma finalidade diferente; ele fornece ao aplicativo cliente a capacidade de criar um perfil de usuário usando a resposta de autenticação do parceiro e recuperá-lo em uma única operação única.

| API | Descrição | Caso de uso |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [API de ponto de extremidade de SSO de perfis](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/partner-single-sign-on-apis/rest-api-v2-partner-single-sign-on-apis-retrieve-profile-using-partner-authentication-response.md) | Criar e recuperar perfil de usuário usando resposta de autenticação de parceiro. | **O usuário permite que o aplicativo use logon único de parceiro para autenticação**<br/><br/> Neste cenário, o aplicativo cliente deve criar um perfil de usuário depois de receber a resposta de autenticação de parceiro e recuperá-lo em uma única operação única. |

Para qualquer consulta subsequente, os endpoints básicos de Perfis devem ser usados para determinar o status de autenticação do usuário, acessar as informações de metadados do usuário, encontrar o método usado para autenticar ou a entidade usada para fornecer a identidade.

Para obter mais detalhes, consulte os documentos [Logon único usando fluxos de parceiros](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md) e [Guia de Cookies do Apple SSO (REST API V2)](/help/authentication/integration-guide-programmers/features-standard/sso-access/partner-sso/apple-sso/apple-sso-cookbook-rest-api-v2.md).

#### &#x200B;11. O que o aplicativo cliente deve fazer se o usuário tiver vários perfis do MVPD? {#authentication-phase-faq11}

A decisão de oferecer suporte a vários perfis depende das necessidades de negócios do aplicativo cliente.

A maioria dos usuários terá apenas um perfil. No entanto, nos casos em que existem vários perfis (conforme detalhado abaixo), o aplicativo cliente é responsável por determinar a melhor experiência do usuário para a seleção de perfis.

O aplicativo cliente pode optar por solicitar que o usuário selecione o perfil do MVPD desejado ou fazer a seleção automaticamente, como escolher o primeiro perfil de usuário da resposta ou aquele com o período de validade mais longo.

A REST API v2 é compatível com vários perfis para acomodar:

* Usuários que podem precisar escolher entre um perfil MVPD comum e um obtido por meio do logon único (SSO).
* Os usuários recebem acesso temporário sem precisar fazer logoff do MVPD normal.
* Usuários com assinatura do MVPD combinada com serviços de Direto para o consumidor (DTC).
* Usuários com várias assinaturas do MVPD.

#### &#x200B;12. O que acontece quando os perfis de usuários expiram? {#authentication-phase-faq12}

Quando os perfis de usuário expiram, eles não são mais incluídos na resposta do endpoint Perfis.

Se o endpoint de Perfis retornar uma resposta vazia do mapa de perfis, o aplicativo cliente deverá criar uma nova sessão de autenticação e solicitar que o usuário se autentique novamente.

Para obter mais informações, consulte a documentação da [Criar API de sessão de autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

#### &#x200B;13. Quando os perfis de usuário se tornam inválidos? {#authentication-phase-faq13}

Os perfis de usuário se tornam inválidos nos seguintes cenários:

* Quando o TTL de autenticação expira, conforme indicado pelo carimbo de data/hora `notAfter` na resposta do ponto de extremidade Profiles.
* Quando o aplicativo cliente altera o valor do cabeçalho [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md).
* Quando o aplicativo cliente atualiza, as credenciais de cliente usadas para recuperar o valor do cabeçalho [Autorização](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md).
* Quando o aplicativo cliente revoga ou atualiza a instrução de software usada para obter credenciais do cliente.

#### &#x200B;14. Quando o aplicativo cliente deve iniciar o mecanismo de polling? {#authentication-phase-faq14}

Para garantir a eficiência e evitar solicitações desnecessárias, o aplicativo cliente deve iniciar o mecanismo de pesquisa nas seguintes condições:

**Autenticação executada no aplicativo (tela) primário**

O aplicativo principal (streaming) deve iniciar a pesquisa quando o usuário atingir a página de destino final, depois que o componente do navegador carregar a URL especificada para o parâmetro `redirectUrl` na solicitação de ponto de extremidade [Sessões](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

**Autenticação executada em um aplicativo secundário (tela)**

O aplicativo principal (transmissão) deve iniciar a sondagem assim que o usuário iniciar o processo de autenticação, logo após receber a resposta do ponto de extremidade [Sessões](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) e exibir o código de autenticação ao usuário.

#### &#x200B;15. Quando o aplicativo cliente deve interromper o mecanismo de polling? {#authentication-phase-faq15}

Para garantir a eficiência e evitar solicitações desnecessárias, o aplicativo cliente deve interromper o mecanismo de pesquisa nas seguintes condições:

**Autenticação bem-sucedida**

As informações de perfil do usuário foram recuperadas com êxito, confirmando o status de autenticação. Neste ponto, a pesquisa não é mais necessária.

**Sessão de autenticação e expiração do código**

A sessão de autenticação e o código expiram, conforme indicado pelo carimbo de data/hora `notAfter` (por exemplo, 30 minutos) na resposta do ponto de extremidade [Sessões](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md). Se isso acontecer, o usuário deverá reiniciar o processo de autenticação e a pesquisa usando o código de autenticação anterior deverá ser interrompida imediatamente.

**Novo código de autenticação gerado**

Se o usuário solicitar um novo código de autenticação no dispositivo principal (tela), a sessão existente não será mais válida e o polling usando o código de autenticação anterior deverá ser interrompido imediatamente.

#### &#x200B;16. Que intervalo entre chamadas o aplicativo cliente deve usar para o mecanismo de sondagem? {#authentication-phase-faq16}

Para garantir a eficiência e evitar solicitações desnecessárias, o aplicativo cliente deve configurar a frequência do mecanismo de polling nas seguintes condições:

| **Autenticação executada no aplicativo (tela) primário** | **Autenticação executada em um aplicativo secundário (tela)** |
|----------------------------------------------------------------------------|----------------------------------------------------------------------------|
| O aplicativo principal (transmissão) deve pesquisar a cada 3-5 segundos ou mais. | O aplicativo principal (transmissão) deve pesquisar a cada 3-5 segundos ou mais. |

#### &#x200B;17. Qual é o número máximo de solicitações de sondagem que o aplicativo cliente pode enviar? {#authentication-phase-faq17}

O aplicativo cliente deve seguir os limites atuais definidos pelo [Mecanismo de limitação](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-limits) da Autenticação Adobe Pass.

A manipulação de erros do aplicativo cliente deve poder manipular o código de erro [429 Muitas Solicitações](/help/authentication/integration-guide-programmers/throttling-mechanism.md#throttling-mechanism-response), que indica que o aplicativo cliente excedeu o número máximo de solicitações permitidas.

Para obter mais detalhes, consulte a documentação do [Mecanismo de limitação](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

#### &#x200B;18. Como o aplicativo cliente pode obter as informações de metadados do usuário? {#authentication-phase-faq18}

O aplicativo cliente pode consultar um dos seguintes pontos de extremidade capazes de retornar informações de [metadados do usuário](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) como parte das informações do perfil:

* [API do endpoint de perfis](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Endpoint de perfis para API MVPD específica](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Endpoint de perfis para API de código específica (autenticação)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

Os metadados do usuário ficam disponíveis após a conclusão do fluxo de autenticação, portanto, o aplicativo cliente não precisa consultar um ponto de extremidade separado para recuperar as informações de [metadados do usuário](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md), pois elas já estão incluídas nas informações do perfil.

Para obter mais detalhes, consulte os seguintes documentos:

* [Fluxo de perfis básicos realizado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Fluxo de perfis básicos realizado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

Determinados atributos de metadados podem ser atualizados durante o fluxo de autorização, dependendo do MVPD e do atributo de metadados específico. Como resultado, o aplicativo cliente pode precisar consultar as APIs acima novamente para recuperar os metadados do usuário mais recentes.

#### &#x200B;19. Como o aplicativo cliente deve gerenciar o acesso degradado? {#authentication-phase-faq19}

O [Recurso de Degradação](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md) permite que o aplicativo cliente mantenha uma experiência de streaming contínua para os usuários, mesmo quando os serviços de autenticação ou autorização do MVPD tiverem problemas.

Em resumo, isso pode garantir acesso ininterrupto ao conteúdo, apesar das interrupções temporárias de serviço do MVPD.

Considerando que sua organização pretende usar o recurso de degradação premium, o aplicativo cliente deve lidar com fluxos de acesso degradados, que descrevem como os endpoints da API REST v2 se comportam nesses cenários.

Para obter mais detalhes, consulte a documentação dos [Fluxos de acesso degradados](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md).

#### &#x200B;20. Como o aplicativo cliente deve gerenciar o acesso temporário? {#authentication-phase-faq20}

O [Recurso TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md) permite que o aplicativo cliente forneça acesso temporário ao usuário.

Em resumo, isso pode envolver os usuários fornecendo acesso limitado ao conteúdo ou a um número predefinido de títulos do VOD por um período especificado.

Considerando que sua organização pretende usar o recurso premium TempPass, o aplicativo cliente deve lidar com fluxos de acesso temporários, que descrevem como os endpoints da REST API v2 se comportam em tais cenários.

Em versões anteriores da API, o aplicativo cliente tinha que fazer logoff de um usuário autenticado com seu MVPD normal para oferecer acesso temporário.

Com a REST API v2, o aplicativo cliente pode alternar facilmente entre um MVPD comum e um TempPass MVPD ao autorizar um fluxo, eliminando a necessidade de o usuário ser desconectado.

Para obter mais detalhes, consulte a documentação dos [Fluxos de acesso temporário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md).

#### &#x200B;21. Como o aplicativo cliente deve gerenciar o acesso de logon único entre dispositivos? {#authentication-phase-faq21}

A REST API v2 pode habilitar logon único entre dispositivos se o aplicativo cliente fornecer um identificador de usuário exclusivo consistente entre dispositivos.

Esse identificador, conhecido como token de serviço, deve ser gerado pelo aplicativo cliente por meio da implementação ou integração de um serviço de identidade externo de escolha.

Para obter mais detalhes, consulte a documentação do [Logon único usando fluxos de token de serviço](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md).

+++

### Perguntas frequentes da fase de pré-autorização {#preauthorization-phase-faqs-general}

+++Perguntas frequentes da fase de pré-autorização

#### &#x200B;1. Qual é o objetivo da Fase de pré-autorização? {#preauthorization-phase-faq1}

O objetivo da Fase de pré-autorização é fornecer ao aplicativo cliente a capacidade de apresentar um subconjunto de recursos de seu catálogo que o usuário teria direito de acessar.

A Fase de pré-autorização pode aprimorar a experiência do usuário quando ele abre o aplicativo do cliente pela primeira vez ou navega para uma nova seção.

#### &#x200B;2. A fase de pré-autorização é obrigatória? {#preauthorization-phase-faq2}

A Fase de pré-autorização não é obrigatória. O aplicativo cliente pode ignorar essa fase se quiser apresentar um catálogo de recursos sem filtrá-los primeiro com base nos direitos do usuário.

#### &#x200B;3. O que é uma decisão de pré-autorização? {#preauthorization-phase-faq3}

A pré-autorização é um termo definido na documentação do [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#preauthorization), enquanto o termo de decisão também pode ser encontrado no [Glossário](rest-api-v2-glossary.md#decision).

A decisão de pré-autorização armazena informações sobre o resultado da consulta do processo de pré-autorização do MVPD que pode ser recuperado do endpoint de Pré-autorização de decisões.

O aplicativo cliente pode usar as decisões de pré-autorização para apresentar um subconjunto de recursos que as decisões do Provedor de TV (informativas) permitiriam ao usuário acessar.

Para obter mais detalhes, consulte os seguintes documentos:

* [Recuperar API de decisões de pré-autorização](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Fluxo básico de pré-autorização executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

#### &#x200B;4. O aplicativo cliente deve armazenar em cache as decisões de pré-autorização em um armazenamento persistente? {#preauthorization-phase-faq4}

O aplicativo cliente não é necessário para armazenar decisões de pré-autorização no armazenamento persistente. No entanto, é recomendável armazenar em cache as decisões de permissão na memória para melhorar a experiência do usuário. Isso ajuda a evitar chamadas desnecessárias para o endpoint de Pré-autorização de Decisões para recursos que já foram pré-autorizados, reduzindo a latência e melhorando o desempenho.

#### &#x200B;5. Como o aplicativo cliente pode determinar por que uma decisão de pré-autorização foi negada? {#preauthorization-phase-faq5}

O aplicativo cliente pode determinar o motivo de uma decisão de pré-autorização negada ao inspecionar o [código de erro e a mensagem](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) incluídos na resposta do ponto de extremidade de Pré-autorização de Decisões. Esses detalhes fornecem ao insight o motivo específico pelo qual a solicitação de pré-autorização foi negada, ajudando a informar a experiência do usuário ou acionar qualquer manipulação necessária no aplicativo.

Certifique-se de que qualquer mecanismo de repetição implementado para recuperar decisões de pré-autorização não resulte em um loop infinito se a decisão de pré-autorização for negada.

Considere limitar as tentativas a um número razoável e lidar com as negações normalmente ao exibir comentários claros para o usuário.

#### &#x200B;6. Por que a decisão de pré-autorização não tem um token de mídia? {#preauthorization-phase-faq6}

A decisão de pré-autorização não tem um token de mídia porque a Fase de pré-autorização não deve ser usada para reproduzir recursos, pois essa é a finalidade da Fase de autorização.

#### &#x200B;7. A fase de autorização pode ser ignorada se já existir uma decisão de pré-autorização? {#preauthorization-phase-faq7}

Não.

A Fase de autorização não pode ser ignorada mesmo se uma decisão de pré-autorização estiver disponível. As decisões de pré-autorização são apenas informativas e não concedem direitos de reprodução reais. A Fase de pré-autorização tem como objetivo fornecer orientação antecipada, mas a Fase de autorização ainda é necessária antes de reproduzir qualquer conteúdo.

#### &#x200B;8. Qual é um recurso e quais formatos são compatíveis? {#preauthorization-phase-faq8}

O recurso é um termo definido no [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource).

O recurso é um identificador exclusivo que foi acordado com os MVPDs e está associado a um conteúdo que o aplicativo cliente pode transmitir.

O identificador exclusivo do recurso pode ter dois formatos:

* Um formato de string simples, como um identificador exclusivo de um canal (marca).
* Um formato RSS de mídia (MRSS) com informações adicionais, como título, classificações e metadados de controle dos pais.

Para obter mais detalhes, consulte a documentação de [Recursos Protegidos](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### &#x200B;9. Para quantos recursos o aplicativo cliente pode obter uma decisão de pré-autorização de cada vez? {#preauthorization-phase-faq9}

O aplicativo cliente pode obter uma decisão de pré-autorização para um número limitado de recursos em uma única solicitação de API, geralmente até 5, devido a condições impostas pelos MVPDs.

Este número máximo de recursos pode ser exibido e alterado após a aceitação dos MVPDs por meio do [Painel TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da Adobe Pass por um dos administradores da organização ou por um representante da Autenticação Adobe Pass que atue em seu nome.

Para obter mais detalhes, consulte a documentação do [Guia do Usuário de Integrações do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties).

+++

### Perguntas frequentes sobre a fase de autorização {#authorization-phase-faqs-general}

+++Perguntas frequentes sobre a fase de autorização

#### &#x200B;1. Qual é o objetivo da fase de autorização? {#authorization-phase-faq1}

A finalidade da Fase de autorização é fornecer ao aplicativo cliente a capacidade de reproduzir recursos que o usuário solicita após validar seus direitos com o MVPD.

#### &#x200B;2. A fase de autorização é obrigatória? {#authorization-phase-faq2}

A Fase de autorização é obrigatória, o aplicativo cliente não poderá ignorar essa fase se quiser reproduzir recursos que o usuário solicita, pois requer a verificação com o MVPD de que o usuário tem direito antes de liberar o fluxo.

#### &#x200B;3. Qual é a decisão de autorização e por quanto tempo ela é válida? {#authorization-phase-faq3}

A autorização é um termo definido na documentação do [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#authorization), enquanto o termo de decisão também pode ser encontrado no [Glossário](rest-api-v2-glossary.md#decision).

A decisão de autorização armazena informações sobre o resultado da consulta do processo de autorização do MVPD que pode ser recuperado do endpoint de autorização de decisões.

O aplicativo cliente pode usar a decisão de autorização contendo um token de mídia para reproduzir o fluxo de recursos, caso a decisão do Provedor de TV (autoritativo) permita que o usuário acesse-o.

Para obter mais detalhes, consulte os seguintes documentos:

* [Recuperar API de decisões de autorização](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Fluxo de autorização básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

A decisão de autorização é válida por um período limitado e curto especificado no momento da emissão, indicando a quantidade de tempo que será armazenada em cache pela Autenticação Adobe Pass antes de solicitar a consulta do MVPD novamente.

Este período de tempo limitado, conhecido como autorização (authZ) [TTL](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#ttl), pode ser visualizado e alterado no [Painel TVE](rest-api-v2-glossary.md#tve-dashboard) da Adobe Pass por um dos administradores da sua organização ou por um representante da Autenticação da Adobe Pass que atue em seu nome.

Para obter mais detalhes, consulte a documentação do [Guia do Usuário de Integrações do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

#### &#x200B;4. O aplicativo cliente deve armazenar em cache as decisões de autorização em um armazenamento persistente? {#authorization-phase-faq4}

O aplicativo cliente não é necessário para armazenar decisões de autorização em armazenamento persistente.

#### &#x200B;5. Como o aplicativo cliente pode determinar por que uma decisão de autorização foi negada? {#authorization-phase-faq5}

O aplicativo cliente pode determinar o motivo de uma decisão de autorização negada ao inspecionar o [código de erro e a mensagem](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) incluídos na resposta do ponto de extremidade de Autorização de Decisões. Esses detalhes fornecem ao insight o motivo específico pelo qual a solicitação de autorização foi negada, ajudando a informar a experiência do usuário ou acionar qualquer manipulação necessária no aplicativo.

Certifique-se de que qualquer mecanismo de repetição implementado para recuperar decisões de autorização não resulte em um loop infinito se a decisão de autorização for negada.

Considere limitar as tentativas a um número razoável e lidar com as negações normalmente ao exibir comentários claros para o usuário.

#### &#x200B;6. O que é um token de mídia e por quanto tempo ele é válido? {#authorization-phase-faq6}

O token de mídia é um termo definido no [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#media-token).

O token de mídia consiste em uma sequência de caracteres assinada enviada em texto não criptografado que pode ser recuperada do endpoint da Autorização de decisões.

Para obter mais informações, consulte a documentação do [Verificador de Token de Mídia](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).

O token de mídia é válido por um período limitado e curto especificado no momento da emissão, indicando o limite de tempo antes que ele seja verificado e usado pelo aplicativo cliente.

O aplicativo cliente pode usar o token de mídia para reproduzir um fluxo de recursos caso a decisão do Provedor de TV (autoritativo) permita que o usuário acesse-o.

Para obter mais detalhes, consulte os seguintes documentos:

* [Recuperar API de decisões de autorização](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)
* [Fluxo de autorização básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

#### &#x200B;7. O aplicativo cliente deve validar o token de mídia antes de reproduzir o fluxo de recursos? {#authorization-phase-faq7}

Sim.

O aplicativo cliente deve validar o token de mídia antes de iniciar a reprodução do fluxo de recursos. Esta validação deve ser executada usando o [Verificador de Token de Mídia](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier). Ao verificar o `serializedToken` do `token` retornado, o aplicativo cliente ajuda a impedir o acesso não autorizado, como a cópia de fluxo, e garante que somente os usuários devidamente autorizados possam reproduzir o conteúdo.

#### &#x200B;8. O aplicativo cliente deve atualizar um token de mídia expirado durante a reprodução? {#authorization-phase-faq8}

Não.

O aplicativo cliente não é necessário para atualizar um token de mídia expirado enquanto o fluxo estiver sendo reproduzido ativamente. Se o token de mídia expirar durante a reprodução, o fluxo deverá continuar sem interrupções. No entanto, o cliente deve solicitar uma nova decisão de autorização — e obter um novo token de mídia — na próxima vez que o usuário tentar reproduzir um recurso.

#### &#x200B;9. Qual é o objetivo de cada atributo de carimbo de data e hora na decisão de autorização? {#authorization-phase-faq9}

A decisão de autorização inclui vários atributos de carimbo de data e hora que fornecem contexto essencial sobre o período de validade da própria autorização e o token de mídia associado. Esses carimbos de data e hora atendem a diferentes finalidades, dependendo se estão relacionados à decisão de autorização ou ao token de mídia.

**Carimbos de data e hora de nível de decisão**

Estes carimbos de data e hora descrevem o período de validade da decisão de autorização geral:

| Atributo | Descrição | Notas |
|-------------|----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | O tempo em milissegundos durante o qual a decisão de autorização foi emitida. | Isso marca o início da janela de validade da autorização. |
| `notAfter` | O tempo em milissegundos quando a decisão de autorização expira. | O [TTL (Time-to-Live) de autorização](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#authorization-ttl-management) determina por quanto tempo a autorização permanece válida antes de ser necessária uma nova autorização. Esse TTL é negociado com representantes da MVPD. |

**Carimbos de Data/Hora no Nível do Token**

Esses carimbos de data e hora descrevem o período de validade do token de mídia vinculado à decisão de autorização:

| Atributo | Descrição | Notas |
|-------------|-----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `notBefore` | O tempo em milissegundos durante o qual o token de mídia foi emitido. | Isso marca quando o token se torna válido para reprodução. |
| `notAfter` | O tempo em milissegundos quando o token de mídia expira. | Os tokens de mídia têm uma vida útil deliberadamente curta (normalmente 7 minutos) para minimizar riscos de uso incorreto e levar em conta possíveis diferenças de relógio entre o servidor gerador de tokens e o servidor de verificação de token. |

#### &#x200B;10. O que é um recurso e quais formatos são compatíveis? {#authorization-phase-faq10}

O recurso é um termo definido no [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#resource).

O recurso é um identificador exclusivo que foi acordado com os MVPDs e está associado a um conteúdo que o aplicativo cliente pode transmitir.

O identificador exclusivo do recurso pode ter dois formatos:

* Um formato de string simples, como um identificador exclusivo de um canal (marca).
* Um formato RSS de mídia (MRSS) com informações adicionais, como título, classificações e metadados de controle dos pais.

Para obter mais detalhes, consulte a documentação de [Recursos Protegidos](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

#### &#x200B;11. Para quantos recursos o aplicativo cliente pode obter uma decisão de autorização de cada vez? {#authorization-phase-faq11}

O aplicativo cliente pode obter uma decisão de autorização para um número limitado de recursos em uma única solicitação de API, geralmente até 1, devido a condições impostas pelos MVPDs.

+++

### Perguntas frequentes sobre a fase de logout {#logout-phase-faqs-general}

+++Perguntas frequentes sobre a fase de logout

#### &#x200B;1. Qual é a finalidade da Fase de logout? {#logout-phase-faq1}

A finalidade da Fase de logout é fornecer ao aplicativo cliente a capacidade de encerrar o perfil autenticado do usuário na Autenticação do Adobe Pass mediante solicitação do usuário.

#### &#x200B;2. A Fase de logout é obrigatória? {#logout-phase-faq2}

A Fase de logout é obrigatória, o aplicativo cliente deve fornecer ao usuário a capacidade de fazer logout.

+++

### Perguntas frequentes sobre cabeçalhos {#headers-faqs-general}

+++Perguntas frequentes sobre cabeçalhos

#### &#x200B;1. Como calcular o valor do cabeçalho de autorização? {#headers-faq1}

>[!IMPORTANT]
>
> Caso o aplicativo cliente esteja migrando da API REST V1 para a API REST V2, o aplicativo cliente pode continuar a usar o mesmo método para obter o valor do token de acesso `Bearer` como antes.

O cabeçalho de solicitação [Authorization](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) contém o token de acesso `Bearer` exigido pelo aplicativo cliente para acessar APIs protegidas pelo Adobe Pass.

O valor do cabeçalho de Autorização deve ser obtido da Autenticação Adobe Pass durante a Fase de Registro.

Para obter mais detalhes, consulte os seguintes documentos:

* [Visão geral do registro dinâmico do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
* [Recuperar API de credenciais do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Recuperar API do token de acesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)
* [Fluxo de registro dinâmico do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

#### &#x200B;2. Como calcular o valor do cabeçalho AP-Device-Identifier? {#headers-faq2}

>[!IMPORTANT]
>
> Caso o aplicativo cliente esteja migrando da REST API V1 para a REST API V2, o aplicativo cliente pode continuar a usar o mesmo método para calcular o valor do identificador do dispositivo como antes.

O cabeçalho de solicitação [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) contém o identificador do dispositivo de streaming conforme foi criado pelo aplicativo cliente.

A documentação do cabeçalho [AP-Device-Identifier](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md) fornece exemplos para as principais plataformas de como calcular o valor, mas o aplicativo cliente pode optar por usar um método diferente com base em sua própria lógica e requisitos de negócios.

#### &#x200B;3. Como calcular o valor do cabeçalho X-Device-Info? {#headers-faq3}

>[!IMPORTANT]
>
> Caso o aplicativo cliente esteja migrando da REST API V1 para a REST API V2, o aplicativo cliente pode continuar a usar o mesmo método para calcular o valor das informações do dispositivo como antes.

O cabeçalho de solicitação [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) contém as informações do cliente (dispositivo, conexão e aplicativo) relacionadas ao dispositivo de streaming real e é usado para determinar regras específicas da plataforma que os MVPDs podem impor.

A documentação do cabeçalho [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) fornece exemplos para as principais plataformas de como calcular o valor, mas o aplicativo cliente pode optar por usar um método diferente com base em sua própria lógica e requisitos de negócios.

Se o cabeçalho [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) estiver ausente ou contiver valores incorretos, a solicitação poderá ser classificada como originária de uma plataforma `unknown`.

Isso pode fazer com que a solicitação seja tratada como insegura e sujeita a regras mais restritivas, como TTLs de autenticação mais curtas. Além disso, alguns campos, como o dispositivo de streaming `connectionIp` e `connectionPort`, são obrigatórios para recursos como a [Autenticação da Base Inicial](/help/authentication/integration-guide-programmers/features-standard/hba-access/home-based-authentication.md) do Spectrum.

Mesmo quando a solicitação é originada de um servidor em nome de um dispositivo, o valor do cabeçalho [X-Device-Info](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md) deve refletir as informações reais do dispositivo de streaming.

+++

### Perguntas frequentes diversas {#misc-faqs-general}

+++Perguntas frequentes diversas

#### &#x200B;1. Posso explorar solicitações e respostas da API REST V2 e testar a API? {#misc-faq1}

Sim.

Você pode explorar a REST API V2 através do nosso site dedicado [Adobe Developer](https://developer.adobe.com/adobe-pass/). O site do Adobe Developer fornece acesso irrestrito a:

* [API DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

Para interagir com a [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/), você deve incluir o cabeçalho [Autorização](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) com um token de acesso `Bearer` obtido por meio da [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/).

Para usar a [API DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/), é necessária uma instrução de software com o escopo REST API V2. Para obter mais detalhes, consulte o documento [Perguntas frequentes sobre o Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md).

#### &#x200B;2. Posso explorar solicitações e respostas da REST API V2 usando uma ferramenta de desenvolvimento de API com suporte a OpenAPI? {#misc-faq2}

Sim.

Você pode baixar arquivos de especificação OpenAPI para a [API DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/) e a [API REST V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/) do site [Adobe Developer](https://developer.adobe.com/adobe-pass/).

Para baixar os arquivos de especificação OpenAPI, clique nos botões de download para salvar os seguintes arquivos no computador local:

* [API JSON DE DCR](https://developer.adobe.com/adobe-pass/dcrApi.json)
* [REST API V2 JSON](https://developer.adobe.com/adobe-pass/restApiV2.json)

Em seguida, você pode importar esses arquivos para sua ferramenta de desenvolvimento de API preferida para explorar solicitações e respostas da API REST V2 e testar a API.

#### &#x200B;3. Ainda posso usar a ferramenta de teste de API existente hospedada em https://sp.auth-staging.adobe.com/apitest/api.html? {#misc-faq3}

Não.

Os aplicativos clientes que estão migrando para a REST API V2 devem usar a nova ferramenta de teste hospedada em https://developer.adobe.com/adobe-pass/. O site do Adobe Developer fornece acesso irrestrito a:

* [API DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/)
* [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/)

Para interagir com a [REST API V2](https://developer.adobe.com/adobe-pass/api/rest_api_v2/interactive/), você deve incluir o cabeçalho [Autorização](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) com um token de acesso `Bearer` obtido por meio da [DCR API](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/).

Para usar a [API DCR](https://developer.adobe.com/adobe-pass/api/dcr_api/interactive/), é necessária uma instrução de software com o escopo REST API V2. Para obter mais detalhes, consulte o documento [Perguntas frequentes sobre o Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md).

+++

## Perguntas frequentes sobre migração {#migration-faqs}

Prossiga com esta seção se estiver trabalhando em um aplicativo que precise migrar um aplicativo existente para a REST API V2.

>[!MORELIKETHIS]
>
> * [Perguntas frequentes sobre o Registro Dinâmico de Clientes (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md#migration-faqs)

### Perguntas frequentes gerais sobre migração {#general-migration-faqs}

+++Perguntas frequentes gerais sobre migração

#### &#x200B;1. Sou obrigado a implantar um novo aplicativo cliente migrado para a REST API V2 para todos os usuários de uma só vez? {#migration-faq1}

Não.

O aplicativo cliente não é necessário para implantar uma nova versão que integre a REST API V2 a todos os usuários simultaneamente.

A Autenticação do Adobe Pass continuará a ser compatível com versões mais antigas do aplicativo cliente integrando a REST API V1 ou o SDK até o final de 2025.

#### &#x200B;2. Sou obrigado a implantar um novo aplicativo cliente migrado para a REST API V2 em todas as APIs e fluxos de uma só vez? {#migration-faq2}

Sim.

O aplicativo cliente é necessário para implantar uma nova versão que integre a REST API V2 em todas as APIs de autenticação da Adobe Pass e flui simultaneamente.

No caso do fluxo &#39;segunda autenticação de tela&#39;, o aplicativo cliente é necessário para implantar uma nova versão integrando a API REST V2 para os aplicativos [primário](rest-api-v2-glossary.md#primary-application) e [secundário](rest-api-v2-glossary.md#secondary-application) simultaneamente.

A autenticação do Adobe Pass não oferecerá suporte a implementações &quot;híbridas&quot; que integram a REST API V2 e a REST API V1/SDK entre APIs e fluxos.

#### &#x200B;3. A autenticação do usuário será preservada ao atualizar para um novo aplicativo cliente migrado para a REST API V2? {#migration-faq3}

Não.

A autenticação de usuário obtida em versões anteriores do aplicativo cliente que integram a REST API V1 ou o SDK não será preservada.

Portanto, o usuário precisará autenticar novamente no novo aplicativo cliente migrado para a REST API V2.

#### &#x200B;4. Os códigos de erro aprimorados estão habilitados por padrão na REST API V2? {#migration-faq4}

Sim.

Por padrão, os aplicativos clientes que migram para a REST API V2 se beneficiam automaticamente desse recurso, fornecendo informações de erro mais detalhadas e precisas.

Para obter mais detalhes, consulte a documentação dos [Códigos de erro aprimorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2).

#### &#x200B;5. As integrações existentes exigem alterações de configuração ao migrar para a REST API V2? {#migration-faq5}

Não.

Os aplicativos clientes que estão migrando para a REST API V2 não exigem alterações de configuração nas integrações existentes do MVPD. Além disso, eles continuarão a usar os mesmos identificadores para os [provedores de serviços](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#service-provider) e [MVPDs](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#mvpd) existentes.

Além disso, os protocolos usados pela Autenticação Adobe Pass para se comunicar com os endpoints do MVPD permanecem inalterados.

+++

### Migração da REST API V1 para REST API V2 {#migration-rest-api-v1-to-rest-api-v2-faqs}

Prossiga com esta subseção se estiver trabalhando em um aplicativo que precise migrar da API REST V1 para a API REST V2.

#### Perguntas frequentes sobre a fase de registro {#registration-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Perguntas frequentes sobre a fase de registro

##### &#x200B;1. Quais são as migrações de API de alto nível necessárias para a fase de registro? {#registration-phase-v1-to-v2-faq1}

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

##### &#x200B;1. Quais são as migrações de API de alto nível necessárias para a fase de configuração? {#configuration-phase-v1-to-v2-faq1}

Na migração da REST API V1 para REST API V2, há alterações de alto nível a serem consideradas que são apresentadas na tabela a seguir:

| Escopo | REST API V1 | REST API V2 | Observações |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| Recuperar lista de MVPDs com integração ativa | [GET <br/> /api/v1/config/{serviceProvider}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/provide-mvpd-list.md) | [GET <br/> /api/v2/{serviceProvider}/configuration](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) |              |

+++

#### Perguntas frequentes sobre a fase de autenticação {#authentication-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Perguntas frequentes sobre a fase de autenticação

##### &#x200B;1. Quais são as migrações de API de alto nível necessárias para a Fase de autenticação? {#authentication-phase-v1-to-v2-faq1}

Na migração da REST API V1 para REST API V2, há alterações de alto nível a serem consideradas que são apresentadas na tabela a seguir:

| Escopo | REST API V1 | REST API V2 | Observações |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar código de registro (código de autenticação) | [POST <br/> /reggie/v1/{serviceProvider}/regcode](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md) | [POSTAR <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificar código de registro (código de autenticação) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Retomar a autenticação do (MVPD) na segunda tela (aplicativo) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POSTAR <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Iniciar autenticação do (MVPD) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificar status de autenticação do usuário | [GET <br/> /api/v1/checkauthn (primeira tela)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-token.md) <br/> [GET <br/> /api/v1/checkauthn (segunda tela)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/check-authentication-flow-by-second-screen-web-app.md) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar token de autenticação de usuário (perfil) | [GET <br/> /api/v1/tokens/authn](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authentication-token.md) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar informações de metadados do usuário | [GET <br/> /api/v1/tokens/usermetadata](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Perguntas frequentes da fase de pré-autorização {#preauthorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Perguntas frequentes da fase de pré-autorização

##### &#x200B;1. Quais são as migrações de API de alto nível necessárias para a fase de pré-autorização? {#preauthorization-phase-v1-to-v2-faq1}

Na migração da REST API V1 para REST API V2, há alterações de alto nível a serem consideradas que são apresentadas na tabela a seguir:

| Escopo | REST API V1 | REST API V2 | Observações |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar recursos pré-autorizados (decisões de pré-autorização) | [GET <br/> /api/v1/preauthorize (primeira tela)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources.md) <br/> [GET <br/> /api/v1/preauthorize (segunda tela)](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-list-of-preauthorized-resources-by-second-screen-web-app.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo básico de pré-autorização executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Perguntas frequentes sobre a fase de autorização {#authorization-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Perguntas frequentes sobre a fase de autorização

##### &#x200B;1. Quais são as migrações de API de alto nível necessárias para a fase de autorização? {#authorization-phase-v1-to-v2-faq1}

Na migração da REST API V1 para REST API V2, há alterações de alto nível a serem consideradas que são apresentadas na tabela a seguir:

| Escopo | REST API V1 | REST API V2 | Observações |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar autorização do (MVPD) | [GET <br/> /api/v1/authorize](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authorization.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | O aplicativo cliente pode usar a resposta desta API para várias finalidades de uma só vez: <br/> <ul><li>Iniciar autorização do (MVPD)</li><li>Recuperar decisão de autorização</li><li>Recuperar token de mídia curto</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autorização básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Recuperar token de autorização (decisão de autorização) | [GET <br/> /api/v1/tokens/authz](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/retrieve-authorization-token.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | O aplicativo cliente pode usar a resposta desta API para várias finalidades de uma só vez: <br/> <ul><li>Iniciar autorização do (MVPD)</li><li>Recuperar decisão de autorização</li><li>Recuperar token de mídia curto</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autorização básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |
| Recuperar token de autorização curto (token de mídia) | [GET <br/> /api/v1/tokens/media](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/obtain-short-media-token.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/authorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) | O aplicativo cliente pode usar a resposta desta API para várias finalidades de uma só vez: <br/> <ul><li>Iniciar autorização do (MVPD)</li><li>Recuperar decisão de autorização</li><li>Recuperar token de mídia curto</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autorização básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)</li></ul> |

+++

#### Perguntas frequentes sobre a fase de logout {#logout-phase-faqs-migration-rest-api-v1-to-rest-api-v2}

+++Perguntas frequentes sobre a fase de logout

##### &#x200B;1. Quais são as migrações de API de alto nível necessárias para a Fase de logout? {#logout-phase-v1-to-v2-faq1}

Na migração da REST API V1 para REST API V2, há alterações de alto nível a serem consideradas que são apresentadas na tabela a seguir:

| Escopo | REST API V1 | REST API V2 | Observações |
|-----------------|---------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar logout | [GET <br/> /api/v1/logout](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-logout.md) | [GET <br/> /api/v2/{serviceProvider}/logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de logout básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)</li></ul> |

+++

### Migração do SDK para a REST API V2 {#migration-sdk-to-rest-api-v2}

Prossiga com esta subseção se estiver trabalhando em um aplicativo que precise migrar do SDK para a REST API V2.

#### Perguntas frequentes sobre a fase de registro {#registration-phase-faqs-migration-sdk-to-rest-api-v2}

+++Perguntas frequentes sobre a fase de registro

##### &#x200B;1. Quais são as migrações de API de alto nível necessárias para a fase de registro? {#registration-phase-sdk-to-v2-faq1}

Na migração dos SDKs para a REST API V2, há alterações de alto nível a serem consideradas que são apresentadas nas seguintes tabelas:

###### AccessEnabler JavaScript SDK

| Escopo | SDK | REST API V2 | Observações |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Concluir o Registro de Cliente Dinâmico (DCR) | Fornecendo instrução de software ao construtor | [POSTAR <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Visão Geral do Registro Dinâmico do Cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Fluxo de Registro Dinâmico do Cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Escopo | SDK | REST API V2 | Observações |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Concluir o Registro de Cliente Dinâmico (DCR) | Fornecendo instrução de software ao construtor | [POSTAR <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Visão Geral do Registro Dinâmico do Cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Fluxo de Registro Dinâmico do Cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Escopo | SDK | REST API V2 | Observações |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Concluir o Registro de Cliente Dinâmico (DCR) | Fornecendo instrução de software ao construtor | [POSTAR <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Visão Geral do Registro Dinâmico do Cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Fluxo de Registro Dinâmico do Cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Escopo | SDK | REST API V2 | Observações |
|--------------------------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Concluir o Registro de Cliente Dinâmico (DCR) | Fornecendo instrução de software ao construtor | [POSTAR <br/> /o/client/register](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md) <br/> [GET <br/> /o/client/token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Visão Geral do Registro Dinâmico do Cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)</li><li>[Fluxo de Registro Dinâmico do Cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)</li></ul> |

+++

#### Perguntas frequentes sobre a fase de configuração {#configuration-phase-faqs-migration-sdk-to-rest-api-v2}

+++Perguntas frequentes sobre a fase de configuração

##### &#x200B;1. Quais são as migrações de API de alto nível necessárias para a fase de configuração? {#configuration-phase-sdk-to-v2-faq1}

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

##### &#x200B;1. Quais são as migrações de API de alto nível necessárias para a Fase de autenticação? {#authentication-phase-sdk-to-v2-faq1}

Na migração dos SDKs para a REST API V2, há alterações de alto nível a serem consideradas que são apresentadas nas seguintes tabelas:

###### AccessEnabler JavaScript SDK

| Escopo | SDK | REST API V2 | Observações |
|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar autenticação do (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setSelProv) | [POSTAR <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificar status de autenticação do usuário | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkAuthN) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar informações de metadados do usuário | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#getMeta) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler iOS SDK

| Escopo | SDK | REST API V2 | Observações |
|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar autenticação do (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POSTAR <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificar status de autenticação do usuário | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar informações de metadados do usuário | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler tvOS SDK

| Escopo | SDK | REST API V2 | Observações |
|-------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar código de registro (código de autenticação) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POSTAR <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificar código de registro (código de autenticação) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Retomar a autenticação do (MVPD) na segunda tela (aplicativo) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POSTAR <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Iniciar autenticação do (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setSelProv) | [POSTAR <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificar status de autenticação do usuário | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkAuthN) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar informações de metadados do usuário | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#getMeta) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Escopo | SDK | REST API V2 | Observações |
|------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Iniciar autenticação do (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setSelectedProvider) | [POSTAR <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificar status de autenticação do usuário | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkAuthN) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar informações de metadados do usuário | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#getMetadata) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

###### AccessEnabler FireOS SDK

| Escopo | SDK | REST API V2 | Observações |
|-------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar código de registro (código de autenticação) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POSTAR <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificar código de registro (código de autenticação) | [GET <br/> /reggie/v1/{serviceProvider}/regcode/{code}](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/return-registration-record.md) | [GET <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Retomar a autenticação do (MVPD) na segunda tela (aplicativo) | [GET <br/> /api/v1/authenticate](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/initiate-authentication.md) | [POSTAR <br/> /api/v2/{serviceProvider}/sessions/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Iniciar autenticação do (MVPD) | [AccessEnabler.getAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getAuthN) <br/> [AccessEnabler.setSelectedProvider](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#setSelectedProvider) | [POSTAR <br/> /api/v2/{serviceProvider}/sessions](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) <br/> [GET <br/> /api/v2/authenticate/{serviceProvider}/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de autenticação básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)</li><li>[Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)</li></ul> |
| Verificar status de autenticação do usuário | [AccessEnabler.checkAuthentication](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkAuthN) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |
| Recuperar informações de metadados do usuário | [AccessEnabler.getMetadata](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#getMetadata) | Use um dos seguintes: <br/><br/> [GET <br/> /api/v2/{serviceProvider}/profiles](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md) <br/> [GET <br/> /api/v2/{serviceProvider}/profiles/code/{code}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) | O aplicativo cliente pode usar a resposta dessas APIs para vários propósitos de uma só vez: <br/> <ul><li>Verificar status de autenticação do usuário</li><li>Recuperar perfil de usuário</li><li>Recuperar informações de metadados do usuário</li></ul> <br/> Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo de perfis básicos executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)</li><li>[Fluxo de perfis básicos executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)</li></ul> |

+++

#### Perguntas frequentes da fase de pré-autorização {#preauthorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++Perguntas frequentes da fase de pré-autorização

##### &#x200B;1. Quais são as migrações de API de alto nível necessárias para a fase de pré-autorização? {#preauthorization-phase-sdk-to-v2-faq1}

Na migração dos SDKs para a REST API V2, há alterações de alto nível a serem consideradas que são apresentadas nas seguintes tabelas:

###### AccessEnabler JavaScript SDK

| Escopo | SDK | REST API V2 | Observações |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar recursos pré-autorizados (decisões de pré-autorização) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#checkPreauthRes) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/preauthorize-api-javascript-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo básico de pré-autorização executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler iOS/tvOS SDK

| Escopo | SDK | REST API V2 | Observações |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar recursos pré-autorizados (decisões de pré-autorização) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/preauthorize-api-ios-tvos-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo básico de pré-autorização executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

###### AccessEnabler Android SDK

| Escopo | SDK | REST API V2 | Observações |
|---------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar recursos pré-autorizados (decisões de pré-autorização) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#checkPreauth) <br/> [AccessEnabler.preauthorize](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/preauthorize-api-android-sdk.md) | [POST <br/> /api/v2/{serviceProvider}/Decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo básico de pré-autorização executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

| Escopo | SDK | REST API V2 | Observações |
|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recuperar recursos pré-autorizados (decisões de pré-autorização) | [AccessEnabler.checkPreauthorizedResources](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#checkPreauth) | [POST <br/> /api/v2/{serviceProvider}/Decisions/preauthorize/{mvpd}](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) | Para obter mais detalhes, consulte os seguintes documentos: <br/> <ul><li>[Fluxo básico de pré-autorização executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)</li></ul> |

+++

#### Perguntas frequentes sobre a fase de autorização {#authorization-phase-faqs-migration-sdk-to-rest-api-v2}

+++Perguntas frequentes sobre a fase de autorização

##### &#x200B;1. Quais são as migrações de API de alto nível necessárias para a fase de autorização? {#authorization-phase-sdk-to-v2-faq1}

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

##### &#x200B;1. Quais são as migrações de API de alto nível necessárias para a Fase de logout? {#logout-phase-sdk-to-v2-faq1}

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
