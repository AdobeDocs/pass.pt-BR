---
title: Autenticação básica - Aplicativo secundário - Fluxo
description: REST API V2 - Autenticação básica - Aplicativo secundário - Fluxo
exl-id: 83bf592e-c679-4cfe-984d-710a9598c620
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '2006'
ht-degree: 0%

---

# Fluxo de autenticação básico executado no aplicativo secundário {#basic-authentication-flow-performed-within-secondary-application}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> A implementação da REST API V2 é limitada pela documentação do [Mecanismo de limitação](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Visite também as [Perguntas frequentes sobre a REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

O **Fluxo de autenticação** dentro do direito de Autenticação Adobe Pass permite que o aplicativo de streaming verifique se um usuário tem uma conta válida do MVPD. Esse processo exige que o usuário tenha uma conta ativa do MVPD e insira credenciais de logon válidas na página de logon do MVPD.

O fluxo de autenticação é necessário nos seguintes casos:

* Quando o usuário abre um aplicativo pela primeira vez.
* Quando a autenticação anterior do usuário expirar.
* Quando o usuário faz logoff da conta do MVPD.
* Quando o usuário deseja autenticar com uma MVPD diferente.

Em todos esses casos, o aplicativo que chama qualquer um dos endpoints de Perfis recebe uma resposta vazia para um ou mais perfis, mas para MVPDs diferentes.

O **Fluxo de autenticação** requer que um agente do usuário (navegador) conclua uma série de chamadas do aplicativo para o back-end do Adobe Pass, depois para a página de logon do MVPD e, por fim, de volta para o aplicativo. Esse fluxo pode incluir vários redirecionamentos para sistemas MVPD e gerenciamento de cookies ou sessões armazenados para cada domínio, o que pode ser desafiador de alcançar e proteger sem um agente do usuário.

Com base nos recursos do aplicativo principal (aplicativo de transmissão) para oferecer suporte à interação do usuário para selecionar uma MVPD e autenticar com o MVPD selecionado em um agente do usuário, os cenários de autenticação são:

* [Executar autenticação no aplicativo principal](rest-api-v2-basic-authentication-primary-application-flow.md)
* [Realizar autenticação no aplicativo secundário com mvpd pré-selecionado](./rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Executar autenticação no aplicativo secundário sem mvpd pré-selecionado](./rest-api-v2-basic-authentication-secondary-application-flow.md)

## Realizar autenticação no aplicativo secundário com mvpd pré-selecionado {#perform-authentication-within-secondary-application-with-preselected-mvpd}

### Pré-requisitos {#prerequisites-perform-authentication-within-secondary-application-with-preselected-mvpd}

Antes de iniciar o fluxo de autenticação em um aplicativo principal e finalizá-lo por meio da interação do usuário em um aplicativo secundário, verifique se os seguintes pré-requisitos foram atendidos:

* O aplicativo de streaming deve selecionar uma MVPD.
* O aplicativo de streaming deve iniciar uma sessão de autenticação para entrar com o MVPD selecionado.
* O aplicativo secundário deve ser autenticado com o MVPD selecionado em um agente do usuário.

>[!IMPORTANT]
>
> Suposições
>
> <br/>
> 
> * O aplicativo de streaming é compatível com a interação do usuário para selecionar uma MVPD.
> * O aplicativo secundário (geralmente em um dispositivo secundário) oferece suporte à interação do usuário para autenticação com o MVPD selecionado em um agente do usuário.

### Fluxo de trabalho (WRK) {#workflow-perform-authentication-within-secondary-application-with-preselected-mvpd}

Siga as etapas fornecidas para implementar o fluxo de autenticação básico executado em um aplicativo secundário com um MVPD pré-selecionado, conforme mostrado no diagrama a seguir.

![Executar autenticação no aplicativo secundário com mvpd pré-selecionado](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-with-preselected-mvpd.png)

*Executar autenticação no aplicativo secundário com mvpd pré-selecionado*

1. **Criar sessão de autenticação:** o aplicativo de streaming reúne todos os dados necessários para iniciar uma sessão de autenticação chamando o ponto de extremidade Sessões.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Criar sessão de autenticação](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) para obter detalhes sobre:
   > 
   > * Todos os parâmetros _necessários_, como `serviceProvider`, `mvpd`, `domainName` e `redirectUrl`
   > * Todos os cabeçalhos _necessários_, como `Authorization`, `AP-Device-Identifier`
   > * Todos os _parâmetros e cabeçalhos_ opcionais
   >
   > <br/>
   > 
   > O aplicativo de streaming deve fornecer todos os parâmetros necessários em uma única chamada ao criar a sessão de autenticação.

1. **Indique a próxima ação:** A resposta do ponto de extremidade Sessions contém os dados necessários para orientar o aplicativo de streaming em relação à próxima ação.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Criar sessão de autenticação](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) para obter detalhes sobre as informações fornecidas na resposta da sessão.
   > 
   > <br/>
   > 
   > O endpoint de Sessões valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve estar ativa.
   >
   > <br/>
   > 
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Continuar com fluxos de decisões:** A resposta do ponto de extremidade Sessões contém os seguintes dados:
   * O atributo `actionName` está definido como &quot;autorize&quot;.
   * O atributo `actionType` está definido como &quot;direto&quot;.

   Se o back-end do Adobe Pass identificar um perfil válido, o aplicativo de transmissão não precisará reautenticar com o MVPD selecionado, pois já há um perfil que pode ser usado para fluxos de decisões subsequentes.

1. **Exibir código de autenticação:** A resposta do ponto de extremidade Sessions contém os seguintes dados:
   * O `code` que pode ser usado para retomar a sessão de autenticação em um aplicativo secundário.
   * O atributo `actionName` está definido como &quot;autenticar&quot;.
   * O atributo `actionType` está definido como &quot;interativo&quot;.

   Se o back-end do Adobe Pass não identificar um perfil válido, o aplicativo de streaming exibirá o `code` que pode ser usado para retomar a sessão de autenticação em um aplicativo secundário.

1. **Validar código de autenticação:** o aplicativo secundário valida o usuário fornecido `code` para garantir que ele possa continuar com a autenticação MVPD no agente do usuário.

   >[!IMPORTANT]
   >
   > Consulte a [Documentação da API Recuperar informações da sessão de autenticação](../../apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) para obter detalhes sobre:
   >
   > * Todos os parâmetros _necessários_, como `serviceProvider` e `code`
   > * Todos os cabeçalhos _necessários_, como `Authorization`
   > * Todos os _parâmetros e cabeçalhos_ opcionais

1. **Retornar informações sobre a sessão de autenticação:** A resposta do ponto de extremidade Sessions contém os seguintes dados:
   * O atributo `existing` contém os parâmetros existentes que já foram fornecidos.
   * O atributo `missing` contém os parâmetros ausentes que precisam ser fornecidos para concluir o fluxo de autenticação.

   >[!IMPORTANT]
   >
   > Consulte a [documentação da API Recuperar informações da sessão de autenticação](../../apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md) para obter detalhes sobre as informações fornecidas em uma resposta de validação de sessão.
   >
   > <br/>
   >
   > O endpoint de Sessões valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   >
   > <br/>
   >
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

   >[!TIP]
   >
   > O aplicativo secundário pode informar aos usuários que o `code` usado é inválido no caso de uma resposta de erro indicando uma sessão de autenticação ausente, e aconselhá-los a tentar novamente usando um novo.

1. **Abrir URL no agente do usuário:** o aplicativo secundário abre um agente do usuário para carregar o `url` autocomputado, fazendo uma solicitação ao ponto de extremidade de Autenticação. Esse fluxo pode incluir vários redirecionamentos, levando o usuário à página de logon do MVPD e fornecendo credenciais válidas.

   >[!IMPORTANT]
   >
   > Consulte a [Executar autenticação na documentação da API do agente do usuário](../../apis/sessions-apis/rest-api-v2-sessions-apis-perform-authentication-in-user-agent.md) para obter detalhes sobre:
   >
   > * Todos os parâmetros _necessários_, como `serviceProvider` e `code`
   > * Todos os _parâmetros e cabeçalhos_ opcionais

1. **Concluir autenticação do MVPD:** Se o fluxo de autenticação for bem-sucedido, a interação do agente do usuário salvará um perfil regular no back-end do Adobe Pass e alcançará o `redirectUrl` fornecido.

1. **Recuperar perfil para código específico:** O aplicativo de streaming reúne todos os dados necessários para recuperar informações de perfil, enviando uma solicitação ao ponto de extremidade de Perfis.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Recuperar perfil para código específico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) para obter detalhes sobre:
   > 
   > * Todos os parâmetros _necessários_, como `serviceProvider` e `code`
   > * Todos os cabeçalhos _necessários_, como `Authorization`, `AP-Device-Identifier`
   > * Todos os _parâmetros e cabeçalhos_ opcionais

   >[!TIP]
   >
   > O aplicativo de streaming deve implementar um mecanismo de sondagem usando o `code` para verificar se o perfil regular foi gerado e salvo com êxito.

1. **Retornar informações sobre o perfil regular:** A resposta do ponto de extremidade Perfis contém informações sobre o perfil regular associado aos parâmetros e cabeçalhos recebidos.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Recuperar perfil para código específico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) para obter detalhes sobre as informações fornecidas em uma resposta de perfil.
   > 
   > <br/>
   > 
   > O endpoint de Perfis valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   >
   > <br/>
   > 
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

## Executar autenticação no aplicativo secundário sem mvpd pré-selecionado {#perform-authentication-within-secondary-application-without-preselected-mvpd}

### Pré-requisitos {#prerequisites-perform-authentication-within-secondary-application-without-preselected-mvpd}

Antes de iniciar o fluxo de autenticação em um aplicativo principal e finalizá-lo por meio da interação do usuário em um aplicativo secundário, verifique se os seguintes pré-requisitos foram atendidos:

* O aplicativo de streaming deve iniciar uma sessão de autenticação quando precisar entrar.
* O aplicativo secundário deve selecionar uma MVPD.
* O aplicativo secundário deve ser autenticado com o MVPD selecionado em um agente do usuário.

>[!IMPORTANT]
>
> Suposições
>
> <br/>
> 
> * O aplicativo secundário (geralmente em um dispositivo secundário) é compatível com a interação do usuário para selecionar uma MVPD.
> * O aplicativo secundário (geralmente em um dispositivo secundário) oferece suporte à interação do usuário para autenticação com o MVPD selecionado em um agente do usuário.

### Fluxo de trabalho (WRK) {#workflow-perform-authentication-within-secondary-application-without-preselected-mvpd}

Siga as etapas fornecidas para implementar o fluxo de autenticação básico executado em um aplicativo secundário sem um MVPD pré-selecionado, conforme mostrado no diagrama a seguir.

![Executar autenticação no aplicativo secundário sem mvpd pré-selecionado](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-secondary-application-without-preselected-mvpd.png)

*Executar autenticação no aplicativo secundário sem mvpd pré-selecionado*

1. **Criar sessão de autenticação:** o aplicativo de streaming reúne alguns dos dados necessários para iniciar uma sessão de autenticação chamando o ponto de extremidade Sessões.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Criar sessão de autenticação](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) para obter detalhes sobre:
   >
   > * Todos os parâmetros _necessários_, como `serviceProvider`
   > * Todos os cabeçalhos _necessários_, como `Authorization`, `AP-Device-Identifier`
   > * Todos os _parâmetros e cabeçalhos_ opcionais
   >
   > <br/>
   > 
   > O aplicativo de streaming não pode fornecer todos os parâmetros necessários em uma única chamada ao criar a sessão de autenticação.

1. **Indique a próxima ação:** A resposta do ponto de extremidade Sessions contém os dados necessários para orientar o aplicativo de streaming em relação à próxima ação:
   * O `code` que pode ser usado para retomar a sessão de autenticação em um aplicativo secundário.
   * O atributo `actionName` está definido como &quot;retomar&quot;.
   * O atributo `actionType` está definido como &quot;direto&quot;.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Criar sessão de autenticação](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) para obter detalhes sobre as informações fornecidas na resposta da sessão.
   > 
   > <br/>
   > 
   > O endpoint de Sessões valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   >
   > <br/>
   > 
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Exibir código de autenticação:** o aplicativo de streaming exibe o `code` que pode ser usado para retomar a sessão de autenticação em um aplicativo secundário.

1. **Fornecer parâmetros ausentes da sessão de autenticação:** O aplicativo secundário reúne todos os dados ausentes necessários para retomar a sessão de autenticação e chama o ponto de extremidade Sessões.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Retomar sessão de autenticação](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) para obter detalhes sobre:
   >
   > * Todos os parâmetros _necessários_, como `serviceProvider`, `mvpd`, `domainName` e `redirectUrl`
   > * Todos os cabeçalhos _necessários_, como `Authorization`, `AP-Device-Identifier`
   > * Todos os _parâmetros e cabeçalhos_ opcionais

1. **Indique a próxima ação:** A resposta do ponto de extremidade Sessions contém os dados necessários para orientar o aplicativo de streaming em relação à próxima ação.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Retomar sessão de autenticação](../../apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md) para obter detalhes sobre as informações fornecidas na resposta da sessão.
   > 
   > <br/>
   > 
   > O endpoint de Sessões valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve estar ativa.
   >
   > <br/>
   > 
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

   >[!TIP]
   >
   > O aplicativo secundário pode informar aos usuários que o `code` usado é inválido no caso de uma resposta de erro indicando uma sessão de autenticação ausente, e aconselhá-los a tentar novamente usando um novo.

1. **Indicar perfil existente:** A resposta do ponto de extremidade Sessions contém os seguintes dados:
   * O atributo `actionName` está definido como &quot;autorize&quot;.
   * O atributo `actionType` está definido como &quot;direto&quot;.

   Se o back-end do Adobe Pass identificar um perfil válido, o aplicativo de transmissão não precisará reautenticar com o MVPD selecionado, pois já há um perfil que pode ser usado para fluxos de decisões subsequentes.

1. **Abrir URL no agente do usuário:** A resposta do ponto de extremidade Sessões contém os seguintes dados:
   * O `url` que pode ser usado para iniciar a autenticação interativa na página de logon do MVPD.
   * O atributo `actionName` está definido como &quot;autenticar&quot;.
   * O atributo `actionType` está definido como &quot;interativo&quot;.

   Se o back-end do Adobe Pass não identificar um perfil válido, o aplicativo secundário abrirá um agente do usuário para carregar o `url` fornecido, fazendo uma solicitação ao endpoint de Autenticação. Esse fluxo pode incluir vários redirecionamentos, levando o usuário à página de logon do MVPD e fornecendo credenciais válidas.

1. **Concluir autenticação do MVPD:** Se o fluxo de autenticação for bem-sucedido, a interação do agente do usuário salvará um perfil regular no back-end do Adobe Pass e atingirá o `redirectUrl` fornecido.

1. **Recuperar perfil para código específico:** O aplicativo de streaming reúne todos os dados necessários para recuperar informações de perfil, enviando uma solicitação ao ponto de extremidade de Perfis.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Recuperar perfil para código específico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) para obter detalhes sobre:
   >
   > * Todos os parâmetros _necessários_, como `serviceProvider` e `code`
   > * Todos os cabeçalhos _necessários_, como `Authorization`, `AP-Device-Identifier`
   > * Todos os _parâmetros e cabeçalhos_ opcionais

   >[!TIP]
   >
   > O aplicativo de streaming deve implementar um mecanismo de sondagem usando o `code` para verificar se o perfil regular foi gerado e salvo com êxito.

1. **Retornar informações sobre o perfil regular:** A resposta do ponto de extremidade Perfis contém informações sobre o perfil regular associado aos parâmetros e cabeçalhos recebidos.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Recuperar perfil para código específico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) para obter detalhes sobre as informações fornecidas em uma resposta de perfil.
   > 
   > <br/>
   > 
   > O endpoint de Perfis valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   >
   > <br/>
   > 
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).
