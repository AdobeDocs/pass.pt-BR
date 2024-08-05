---
title: Logon único - Identidade da plataforma - Fluxos
description: REST API V2 - Logon único - Identidade da plataforma - Fluxos
source-git-commit: 150e064d0287eaac446c694fb5a2633f7ea4b797
workflow-type: tm+mt
source-wordcount: '1830'
ht-degree: 0%

---


# Logon único usando fluxos de identidade da plataforma {#single-sign-on-platform-identity-full-flows}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> A implementação da REST API V2 é limitada pela documentação do [Mecanismo de limitação](/help/authentication/throttling-mechanism.md).

O método de identificação da plataforma permite que vários aplicativos usem um identificador de plataforma exclusivo para obter logon único (SSO) no nível do dispositivo ou da plataforma ao usar os serviços da Adobe Pass.

Os aplicativos são responsáveis por recuperar a carga do identificador de plataforma exclusivo usando serviços de identidade específicos do dispositivo ou bibliotecas fora dos sistemas Adobe Pass.

Os aplicativos são responsáveis por incluir essa carga de identificador de plataforma exclusiva como parte do cabeçalho `Adobe-Subject-Token` para todas as solicitações que a especificam.

Para obter mais detalhes sobre o cabeçalho `Adobe-Subject-Token`, consulte a documentação [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

## Executar autenticação por meio de logon único usando a identidade da plataforma {#perform-authentication-through-single-sign-on-using-platform-identity}

### Pré-requisitos {#prerequisites-perform-authentication-through-single-sign-on-using-platform-identity}

Antes de executar o fluxo de autenticação por meio do logon único usando uma identidade de plataforma, verifique se os seguintes pré-requisitos estão sendo atendidos:

* A plataforma deve fornecer um serviço de identidade ou biblioteca que retorna informações consistentes como carga `JWS` ou `JWE` em todos os aplicativos no mesmo dispositivo ou plataforma.
* O primeiro aplicativo de streaming deve recuperar o identificador de plataforma exclusivo e incluir a carga `JWS` ou `JWE` como parte do cabeçalho [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) para todas as solicitações que o especificarem.
* O primeiro aplicativo de streaming deve selecionar um MVPD.
* O primeiro aplicativo de streaming deve iniciar uma sessão de autenticação para entrar com o MVPD selecionado.
* O primeiro aplicativo de streaming deve ser autenticado com o MVPD selecionado em um agente do usuário.
* O segundo aplicativo de streaming deve recuperar o identificador de plataforma exclusivo e incluir a carga `JWS` ou `JWE` como parte do cabeçalho [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) para todas as solicitações que o especificarem.

>[!IMPORTANT]
>
> Suposições
>
> <br/>
> 
> * O primeiro aplicativo de streaming é compatível com a interação do usuário para selecionar um MVPD.
> * O primeiro aplicativo de streaming suporta a interação do usuário para autenticar com o MVPD selecionado em um agente do usuário.

### Fluxo de trabalho (WRK) {#workflow-perform-authentication-through-single-sign-on-using-platform-identity}

Execute as etapas fornecidas para implementar o fluxo de autenticação por meio do logon único usando uma identidade de plataforma conforme mostrado no diagrama a seguir.

![Executar autenticação por meio de logon único usando a identidade da plataforma](../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-perform-authentication-through-single-sign-on-using-platform-identity-flow.png)

*Executar autenticação por meio de logon único usando a identidade da plataforma*

1. **Recuperar identificador de plataforma:** O primeiro aplicativo de streaming chama o serviço de identidade ou a biblioteca, fora dos sistemas Adobe Pass, para obter a carga `JWS` ou `JWE` associada ao identificador de plataforma exclusivo.

1. **Retornar um identificador de plataforma exclusivo como JWS ou JWE:** O primeiro aplicativo de streaming valida os dados de resposta para garantir que as condições básicas de segurança sejam atendidas:
   * A carga não expirou.
   * A carga está assinada ou criptografada.

1. **Criar sessão de autenticação:** o primeiro aplicativo de streaming reúne todos os dados necessários para iniciar uma sessão de autenticação chamando o ponto de extremidade Sessões.

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
   > O aplicativo de streaming deve garantir que inclua um valor válido para o identificador exclusivo da plataforma antes de fazer uma solicitação.
   >
   > <br/>
   > 
   > Para obter mais detalhes sobre o cabeçalho `Adobe-Subject-Token`, consulte a documentação [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

1. **Indique a próxima ação:** A resposta do ponto de extremidade Sessions contém os dados necessários para orientar o primeiro aplicativo de streaming em relação à próxima ação.

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
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../enhanced-error-codes.md).

1. **Abrir URL no agente do usuário:** A resposta do ponto de extremidade Sessões contém os seguintes dados:
   * O `url` que pode ser usado para iniciar a autenticação interativa na página de logon MVPD.
   * O atributo `actionName` está definido como &quot;autenticar&quot;.
   * O atributo `actionType` está definido como &quot;interativo&quot;.

   Se o back-end do Adobe Pass não identificar um perfil válido, o primeiro aplicativo de streaming abrirá um agente do usuário para carregar o `url` fornecido, fazendo uma solicitação ao ponto de extremidade de Autenticação. Esse fluxo pode incluir vários redirecionamentos, levando o usuário à página de logon do MVPD e fornecendo credenciais válidas.

1. **Autenticação MVPD concluída:** Se o fluxo de autenticação for bem-sucedido, a interação do agente do usuário salvará um perfil regular no back-end do Adobe Pass e atingirá o `redirectUrl` fornecido.

1. **Recuperar perfil para código específico:** O primeiro aplicativo de streaming reúne todos os dados necessários para recuperar informações de perfil, enviando uma solicitação ao ponto de extremidade de Perfis.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Recuperar perfil para código específico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) para obter detalhes sobre:
   > 
   > * Todos os parâmetros _necessários_, como `serviceProvider`, `code`
   > * Todos os cabeçalhos _necessários_, como `Authorization`, `AP-Device-Identifier`
   > * Todos os _parâmetros e cabeçalhos_ opcionais

   >[!NOTE]
   >
   > Sugestão: O aplicativo de streaming pode esperar que o agente do usuário acesse o `redirectUrl` fornecido para verificar se o perfil regular foi gerado e salvo com êxito.

1. **Localizar perfil regular:** O servidor Adobe Pass identifica um perfil válido com base nos parâmetros e cabeçalhos recebidos.

1. **Retornar informações sobre o perfil regular:** A resposta do ponto de extremidade Perfis contém informações sobre o perfil encontrado associado aos parâmetros e cabeçalhos recebidos.

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
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../enhanced-error-codes.md).

1. **Continuar com fluxos de decisões:** o primeiro aplicativo de streaming pode continuar com fluxos de decisões subsequentes.

   >[!IMPORTANT]
   >
   > O aplicativo de streaming deve garantir que inclua um valor válido para o identificador exclusivo da plataforma antes de fazer uma solicitação.
   >
   > <br/>
   > 
   > Para obter mais detalhes sobre o cabeçalho `Adobe-Subject-Token`, consulte a documentação [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

1. **Recuperar identificador de plataforma:** O segundo aplicativo de streaming chama o serviço de identidade ou a biblioteca, fora dos sistemas Adobe Pass, para obter a carga `JWS` ou `JWE` associada ao identificador de plataforma exclusivo.

1. **Retornar um identificador de plataforma exclusivo como JWS ou JWE:** O segundo aplicativo de streaming valida os dados de resposta para garantir que as condições básicas de segurança sejam atendidas:
   * A carga não expirou.
   * A carga está assinada ou criptografada.

1. **Recuperar perfis:** o segundo aplicativo de streaming reúne todos os dados necessários para recuperar todas as informações de perfil, enviando uma solicitação ao ponto de extremidade de Perfis.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Recuperar perfis](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) para obter detalhes sobre:
   > 
   > * Todos os parâmetros _necessários_, como `serviceProvider`
   > * Todos os cabeçalhos _necessários_, como `Authorization`, `AP-Device-Identifier`
   > * Todos os _parâmetros e cabeçalhos_ opcionais
   >
   > <br/>
   > 
   > O aplicativo de streaming deve garantir que inclua um valor válido para o identificador exclusivo da plataforma antes de fazer uma solicitação.
   >
   > <br/>
   > 
   > Para obter mais detalhes sobre o cabeçalho `Adobe-Subject-Token`, consulte a documentação [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

1. **Localizar perfil de logon único:** O servidor do Adobe Pass identifica um perfil de logon único válido com base nos parâmetros e cabeçalhos recebidos.

1. **Retorne informações sobre o perfil de logon único:** A resposta do ponto de extremidade de Perfis contém informações sobre o perfil encontrado associado aos parâmetros e cabeçalhos recebidos.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Recuperar perfis](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md) para obter detalhes sobre as informações fornecidas na resposta do perfil.
   >
   > <br/>
   > 
   > O endpoint de Perfis valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   >
   > <br/>
   > 
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../enhanced-error-codes.md).

1. **Continuar com fluxos de decisões:** o segundo aplicativo de streaming pode continuar com fluxos de decisões subsequentes.

   >[!IMPORTANT]
   >
   > O aplicativo de streaming deve garantir que inclua um valor válido para o identificador exclusivo da plataforma antes de fazer uma solicitação.
   >
   > <br/>
   > 
   > Para obter mais detalhes sobre o cabeçalho `Adobe-Subject-Token`, consulte a documentação [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

## Recuperar decisões de autorização por meio do logon único usando a identidade da plataforma{#performing-authorization-flow-using-platform-identity-single-sign-on-method}

### Pré-requisitos {#prerequisites-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

Antes de executar o fluxo de autorização por meio do logon único usando uma identidade de plataforma, verifique se os seguintes pré-requisitos foram atendidos:

* A plataforma deve fornecer um serviço de identidade ou biblioteca que retorna informações consistentes como carga `JWS` ou `JWE` em todos os aplicativos no mesmo dispositivo ou plataforma.
* O segundo aplicativo de streaming deve recuperar o identificador de plataforma exclusivo e incluir a carga `JWS` ou `JWE` como parte do cabeçalho [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md) para todas as solicitações que o especificarem.
* O segundo aplicativo de streaming deve recuperar uma decisão de autorização antes de reproduzir um recurso selecionado pelo usuário.

>[!IMPORTANT]
>
> Suposições
> 
> <br/>
> 
> * O primeiro aplicativo de streaming executou autenticação e incluiu um valor válido para o cabeçalho de solicitação [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

### Fluxo de trabalho (WRK) {#workflow-scenario-performing-authorization-flow-using-platform-identity-single-sign-on-method}

Execute as etapas fornecidas para implementar o fluxo de autorização por meio do logon único usando uma identidade de plataforma conforme mostrado no diagrama a seguir.

![Recuperar decisões de autorização por meio do logon único usando a identidade da plataforma](../../../assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-retrieve-authorization-decisions-through-single-sign-on-using-platform-identity-flow.png)

*Recuperar decisões de autorização por meio do logon único usando a identidade da plataforma*

1. **Recuperar identificador de plataforma:** O segundo aplicativo de streaming chama o serviço de identidade ou a biblioteca, fora dos sistemas Adobe Pass, para obter a carga `JWS` ou `JWE` associada ao identificador de plataforma exclusivo.

1. **Retornar um identificador de plataforma exclusivo como JWS ou JWE:** O segundo aplicativo de streaming valida os dados de resposta para garantir que as condições básicas de segurança sejam atendidas:
   * A carga não expirou.
   * A carga está assinada ou criptografada.

1. **Recuperar decisão de autorização:** O segundo aplicativo de streaming reúne todos os dados necessários para obter uma decisão de autorização para um recurso específico, chamando o ponto de extremidade de Autorização de Decisões.

   >[!IMPORTANT]
   >
   > Consulte a [Recuperar decisões de autorização usando a documentação da API do mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) específica para obter detalhes sobre:
   >
   > * Todos os parâmetros _necessários_, como `serviceProvider`, `mvpd` e `resources`
   > * Todos os cabeçalhos _necessários_, como `Authorization` e `AP-Device-Identifier`
   > * Todos os _parâmetros e cabeçalhos_ opcionais
   >
   > <br/>
   > 
   > O aplicativo de streaming deve garantir que inclua um valor válido para o identificador exclusivo da plataforma antes de fazer uma solicitação.
   >
   > <br/>
   > 
   > Para obter mais detalhes sobre o cabeçalho `Adobe-Subject-Token`, consulte a documentação [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).

1. **Localizar perfil de logon único:** O servidor do Adobe Pass identifica um perfil de logon único válido com base nos parâmetros e cabeçalhos recebidos.

1. **Recuperar decisão MVPD para o recurso solicitado:** O servidor do Adobe Pass chama o ponto de extremidade de autorização MVPD para obter uma decisão `Permit` ou `Deny` para o recurso específico recebido do aplicativo de streaming.

1. **Retornar a decisão `Permit` com o token de mídia:** A resposta do ponto de extremidade de Autorização de Decisões contém uma decisão `Permit` e um token de mídia.

   >[!IMPORTANT]
   >
   > Consulte a [Recuperar decisões de autorização usando a documentação específica da API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) para obter detalhes sobre as informações fornecidas em uma resposta de decisão.
   > 
   > <br/>
   > 
   > O endpoint de autorização de decisões valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve estar ativa.
   >
   > <br/>
   > 
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../enhanced-error-codes.md).

1. **Iniciar fluxo com token de mídia:** O segundo aplicativo de streaming usa o token de mídia para reproduzir o conteúdo.

1. **Retornar a decisão `Deny` com detalhes:** A resposta do ponto de extremidade de Autorização de Decisões contém uma decisão `Deny` e uma carga de erro que segue a documentação de [Códigos de Erro Aprimorados](../../../enhanced-error-codes.md).

   >[!IMPORTANT]
   >
   > Consulte a [Recuperar decisões de autorização usando a documentação específica da API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) para obter detalhes sobre as informações fornecidas em uma resposta de decisão.
   > 
   > <br/>
   > 
   > O endpoint de autorização de decisões valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve estar ativa.
   >
   > <br/>
   > 
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../enhanced-error-codes.md).

1. **Identificar detalhes de decisão de `Deny`:** o segundo aplicativo de streaming processa as informações de erro da resposta e pode usá-las para exibir uma mensagem específica na interface do usuário.

>[!NOTE]
>
> As etapas do fluxo de pré-autorização são as mesmas do fluxo de autorização, exceto que o ponto de extremidade usado é o descrito na [Recuperar decisões de pré-autorização usando a documentação mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) específica.
