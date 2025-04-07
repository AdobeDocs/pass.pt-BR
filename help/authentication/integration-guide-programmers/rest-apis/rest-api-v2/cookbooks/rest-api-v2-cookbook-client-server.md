---
title: Cookbook REST API V2 (cliente para servidor)
description: Cookbook REST API V2 (cliente para servidor)
exl-id: 6a5a89d2-ea54-4f9c-9505-e575ced4301c
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '1846'
ht-degree: 0%

---

# Cookbook REST API V2 (cliente para servidor) {#rest-api-v2-cookbook-client-to-server}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> A implementação da REST API V2 é limitada pela documentação do [Mecanismo de limitação](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

O documento é destinado a desenvolvedores que estão integrando a [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) da Autenticação do Adobe Pass em seus aplicativos de streaming que possuem uma arquitetura C2S (Cliente para Servidor).

## Pré-requisitos {#prerequisites}

Para obter termos e definições, consulte o [Glossário da API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md).

Para obter os requisitos obrigatórios e as práticas recomendadas, consulte a documentação da [Lista de Verificação da API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-checklist.md).

Para perguntas frequentes, consulte a documentação [Perguntas frequentes sobre REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md).

## A. Fase de registro {#registration-phase}

A finalidade da Fase de registro é registrar o aplicativo de transmissão na Autenticação do Adobe Pass por meio do processo de Registro dinâmico de cliente (DCR).

O processo de Registro dinâmico de cliente (DCR) exige que o aplicativo de streaming obtenha um par de credenciais de cliente e recupere um token de acesso como meta final da fase de registro.

A Fase de registro é obrigatória, mas o aplicativo de streaming pode ignorar essa fase se tiver um par de credenciais de cliente em cache e um token de acesso que ainda seja válido.

+++Artigos relacionados

**APIs:**

* [Recuperar credenciais do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md)
* [Recuperar token de acesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md)

**Fluxos:**

* [Fluxo de registro dinâmico do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/flows/dynamic-client-registration-flow.md)

**Perguntas frequentes:**

* [Perguntas frequentes sobre a fase de registro](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-faqs.md)

+++

### Etapa 1: Registrar seu aplicativo {#step-1-register-your-application}

* **Recuperar credenciais do cliente:** O aplicativo de streaming recupera credenciais do cliente chamando o ponto de extremidade [**/o/client/register**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).

   * O aplicativo de streaming deve armazenar as credenciais do cliente e usá-las indefinidamente quando precisar recuperar um token de acesso.


* **Recuperar token de acesso:** o aplicativo de streaming recupera o token de acesso chamando o ponto de extremidade [**/o/client/token**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

   * O aplicativo de transmissão deve armazenar e usar o token de acesso até que ele expire, descartá-lo e obter um novo.

## B. Fase de autenticação {#authentication-phase}

A finalidade da Fase de autenticação é fornecer ao aplicativo de streaming a capacidade de verificar a identidade do usuário e obter informações de metadados do usuário.

A Fase de autenticação atua como uma etapa de pré-requisito para a Fase de pré-autorização ou Fase de autorização quando o aplicativo de transmissão precisa reproduzir conteúdo.

+++Artigos relacionados

**APIs**

* [Criar sessão de autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md)
* [Retomar sessão de autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md)
* [Recuperar sessão de autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md)
* [Executar autenticação no agente do usuário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md)
* [Recuperar perfis](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md)
* [Recuperar perfil para mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md)
* [Recuperar perfil para código específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md)

**Fluxos**

* [Fluxo de autenticação básica executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
* [Fluxo de autenticação básico executado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Fluxo de perfis básicos realizado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-primary-application-flow.md)
* [Fluxo de perfis básicos realizado no aplicativo secundário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-profiles-secondary-application-flow.md)

**Perguntas frequentes**

* [Perguntas frequentes sobre a fase de autenticação](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general)

+++

### Etapa 2: verificar perfis autenticados existentes {#step-2-check-for-existing-authenticated-profiles}

* **Recuperar perfis:** o aplicativo de streaming verifica os perfis existentes chamando o ponto de extremidade [**/api/v2/{serviceProvider}/profiles**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md).


* **Cenário 1:** Há perfis existentes, o aplicativo de streaming pode prosseguir para a [Fase de Pré-autorização](#preauthorization-phase) ou [Fase de Autorização](#authorization-phase).


* **Cenário 2:** Não há perfis existentes, o aplicativo de streaming pode prosseguir para a próxima etapa para [Autenticar o usuário](#step-3-authenticate-the-user).


* **Cenário 3:** Não há perfis existentes, o aplicativo de streaming pode continuar a fornecer ao usuário acesso temporário por meio do recurso [TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md).

   * Este cenário está fora do escopo deste documento. Consulte a documentação [Fluxos de Acesso Temporário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md) para obter mais informações.

### Etapa 3: Autenticar o usuário {#step-3-authenticate-the-user}

* **Recuperar configuração:** o aplicativo de streaming recupera a lista de MVPDs disponíveis chamando o ponto de extremidade [**/api/v2/{serviceProvider}/configuration**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md).

   * O aplicativo de transmissão pode implementar um mecanismo de filtragem personalizado para refinar a lista de MVPDs da resposta de configuração, exibindo apenas os provedores pretendidos enquanto oculta outros (por exemplo, MVPDs em desenvolvimento, MVPDs de teste, TempPass). Isso garante que os usuários sejam apresentados a uma seleção com curadoria ao escolher seu provedor de TV.


* **Criar sessão de autenticação:** o aplicativo de streaming inicia uma sessão de autenticação chamando o ponto de extremidade [**/api/v2/{serviceProvider}/sessions**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).


* **Cenário 1:** o aplicativo de streaming pode abrir um navegador ou uma exibição da Web, portanto, deve carregar a autenticação `url`.

   * O usuário envia seu nome de usuário e senha na página de logon do MVPD. Após a autenticação bem-sucedida, o redirecionamento final exibe uma página de sucesso.


* **Cenário 2:** o aplicativo de streaming não pode abrir um navegador, portanto, deve exibir a autenticação `code`. É necessário um aplicativo Web separado para solicitar que o usuário insira o `code`, crie a autenticação `url` e abra: [**/api/v2/authenticate/{serviceProvider}/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md).

   * O usuário envia seu nome de usuário e senha na página de logon do MVPD. Após a autenticação bem-sucedida, o redirecionamento final exibe uma página de sucesso.

### Etapa 4: verificar perfis autenticados {#step-4-check-for-authenticated-profiles}

* **Recuperar perfil para código específico:** O aplicativo de streaming deve implementar um mecanismo de sondagem usando o `code` para verificar se o perfil foi gerado e salvo com êxito chamando o ponto de extremidade [**/api/v2/{serviceProvider}/profiles/code/{code}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md).

   * O aplicativo de streaming deve **iniciar o mecanismo de sondagem** sob as seguintes condições:

      * **Autenticação executada no aplicativo (tela) primário:** O aplicativo (streaming) primário deve iniciar a sondagem quando o usuário atingir a página de destino final, depois que o componente de navegador carregar a URL especificada para o parâmetro `redirectUrl` na solicitação de ponto de extremidade [Sessões](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md).

      * **Autenticação executada em um aplicativo secundário (tela):** o aplicativo primário (streaming) deve iniciar a sondagem assim que o usuário iniciar o processo de autenticação, logo após receber a resposta do ponto de extremidade [Sessões](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) e exibir o código de autenticação ao usuário.

   * O aplicativo de streaming deve **interromper o mecanismo de sondagem** sob as seguintes condições:

      * **Autenticação bem-sucedida:** As informações de perfil do usuário foram recuperadas com êxito, confirmando seu status de autenticação. Neste ponto, a pesquisa não é mais necessária.

      * **Sessão de autenticação e expiração do código:** a sessão de autenticação e o código expiram, conforme indicado pelo carimbo de data/hora `notAfter` (por exemplo, 30 minutos) na resposta do ponto de extremidade [Sessões](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md). Se isso acontecer, o usuário deverá reiniciar o processo de autenticação e a pesquisa usando o código de autenticação anterior deverá ser interrompida imediatamente.

      * **Novo código de autenticação gerado:** Se o usuário solicitar um novo código de autenticação no dispositivo primário (tela), a sessão existente não será mais válida e a sondagem usando o código de autenticação anterior deverá ser interrompida imediatamente.

   * O aplicativo de streaming deve **configurar a frequência do mecanismo de sondagem** sob as seguintes condições:

      * **Autenticação executada no aplicativo (tela) primário:** O aplicativo (streaming) primário deve sondar a cada 3-5 segundos ou mais.

      * **Autenticação executada em um aplicativo secundário (tela):** O aplicativo primário (streaming) deve sondar a cada 3-5 segundos ou mais.

   * O aplicativo de transmissão deve armazenar em cache partes das informações de perfil do usuário em um armazenamento persistente para evitar solicitações desnecessárias e melhorar a experiência do usuário.

## C. Fase de pré-autorização (facultativa) {#preauthorization-phase}

O objetivo da Fase de pré-autorização é fornecer ao aplicativo de streaming a capacidade de apresentar um subconjunto de recursos de seu catálogo que o usuário teria direito de acessar.

A Fase de pré-autorização pode aprimorar a experiência do usuário quando ele abre o aplicativo de streaming pela primeira vez ou navega para uma nova seção.

A Fase de pré-autorização não é obrigatória. O aplicativo de streaming pode ignorar essa fase se quiser apresentar um catálogo de recursos sem filtrá-los primeiro com base nos direitos do usuário.

+++Artigos relacionados

**APIs**

* [Recuperar decisões de pré-autorização](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

**Fluxos**

* [Fluxo básico de pré-autorização executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)

**Perguntas frequentes**

* [Perguntas frequentes da fase de pré-autorização](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)

+++

### Etapa 5: verificar recursos pré-autorizados {#step-5-check-for-preauthorized-resources}

* **Recuperar decisões de pré-autorização:** o aplicativo de streaming recupera decisões de pré-autorização para uma lista de recursos chamando o ponto de extremidade [**/api/v2/{serviceProvider}/Decisions/preauthorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md).

   * O aplicativo de streaming não é necessário para armazenar decisões de pré-autorização no armazenamento persistente. No entanto, é recomendável armazenar em cache as decisões de permissão na memória para melhorar a experiência do usuário. Isso ajuda a evitar chamadas desnecessárias para recursos que já foram pré-autorizados, reduzindo a latência e melhorando o desempenho.

   * O aplicativo de streaming pode determinar o motivo de uma decisão de pré-autorização negada ao inspecionar o [código de erro e a mensagem](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) incluídos na resposta do ponto de extremidade de Pré-autorização de Decisões. Esses detalhes fornecem informações sobre o motivo específico pelo qual a solicitação de pré-autorização foi negada, ajudando a informar a experiência do usuário ou acionar qualquer manipulação necessária no aplicativo. Certifique-se de que qualquer mecanismo de repetição implementado para recuperar decisões de pré-autorização não resulte em um loop infinito se a decisão de pré-autorização for negada. Considere limitar as tentativas a um número razoável e lidar com as negações normalmente ao exibir comentários claros para o usuário.

   * O aplicativo de transmissão pode obter uma decisão de pré-autorização para um número limitado de recursos em uma única solicitação de API, geralmente até 5, devido a condições impostas pelos MVPDs. Este número máximo de recursos pode ser exibido e alterado após a aceitação dos MVPDs por meio do [Painel TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da Adobe Pass por um dos administradores da organização ou por um representante da Autenticação Adobe Pass que atue em seu nome.


## D. Fase de autorização {#authorization-phase}

A finalidade da Fase de autorização é fornecer ao aplicativo de streaming a capacidade de reproduzir recursos que o usuário solicita após validar seus direitos com a MVPD.

A Fase de autorização é obrigatória, o aplicativo de streaming não poderá ignorar essa fase se quiser reproduzir recursos que o usuário solicitar, pois requer a verificação com o MVPD de que o usuário tem direito antes de liberar o fluxo.

+++Artigos relacionados

**APIs**

* [Recuperar decisões de autorização](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

**Fluxos**

* [Fluxo de autorização básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

**Perguntas frequentes**

* [Perguntas frequentes sobre a fase de autorização](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)

+++

### Etapa 6: verificar recursos autorizados {#step-6-check-for-authorized-resources}

* **Recuperar decisão de autorização:** o aplicativo de streaming recupera a decisão de autorização para um recurso específico chamando o ponto de extremidade [**/api/v2/{serviceProvider}/decision/authorize/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md).

   * O aplicativo de streaming não é necessário para armazenar decisões de autorização no armazenamento persistente.

   * O aplicativo de streaming pode determinar o motivo de uma decisão de autorização negada ao inspecionar o [código de erro e a mensagem](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md) incluídos na resposta do ponto de extremidade de Autorização de Decisões. Esses detalhes fornecem informações sobre o motivo específico pelo qual a solicitação de autorização foi negada, ajudando a informar a experiência do usuário ou acionar qualquer manipulação necessária no aplicativo. Certifique-se de que qualquer mecanismo de repetição implementado para recuperar decisões de autorização não resulte em um loop infinito se a decisão de autorização for negada. Considere limitar as tentativas a um número razoável e lidar com as negações normalmente ao exibir comentários claros para o usuário.

   * O aplicativo de streaming não é necessário para atualizar um token de mídia expirado enquanto o fluxo estiver sendo reproduzido ativamente. Se o token de mídia expirar durante a reprodução, o fluxo deverá continuar sem interrupções. No entanto, o cliente deve solicitar uma nova decisão de autorização — e obter um novo token de mídia — na próxima vez que o usuário tentar reproduzir um recurso.

   * O aplicativo de streaming pode obter uma decisão de autorização para um número limitado de recursos em uma única solicitação de API, geralmente até 1, devido a condições impostas por MVPDs.

## E. Fase de saída {#logout-phase}

A finalidade da Fase de logout é fornecer ao aplicativo de streaming a capacidade de encerrar o perfil autenticado do usuário na Autenticação do Adobe Pass mediante solicitação do usuário.

A Fase de logout é obrigatória, o aplicativo de streaming deve fornecer ao usuário a capacidade de fazer logout.

+++Artigos relacionados

**APIs**

* [Iniciar logout para mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md)

**Fluxos**

* [Fluxo de logout básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-logout-primary-application-flow.md)

**Perguntas frequentes**

* [Perguntas frequentes sobre a fase de logout](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#logout-phase-faqs-general)

+++

### Etapa 7: Logout {#step-7-logout}

* **Iniciar logout do Adobe Pass:** o aplicativo de streaming inicia o fluxo de logout chamando o ponto de extremidade [**/api/v2/{serviceProvider}/logout/{mvpd}**](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md).

   * O aplicativo de streaming deve seguir as instruções fornecidas nos atributos `actionName` e `actionType` da resposta do ponto de extremidade de logout para garantir que o processo de logout seja concluído corretamente.

      * Se o atributo `actionType` na resposta estiver definido como &quot;interativo&quot;:

         * **Cenário 1:** o aplicativo de streaming pode abrir um navegador ou uma exibição da Web, portanto, deve carregar o logout `url`.

         * **Cenário 2:** o aplicativo de streaming não pode abrir um navegador, portanto, o processo de logout pode ser interrompido porque a sessão do MVPD não foi mantida em um cache de navegador do dispositivo de streaming.
