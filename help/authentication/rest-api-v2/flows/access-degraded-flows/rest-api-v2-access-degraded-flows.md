---
title: Fluxos de acesso degradados
description: REST API V2 - Fluxos de acesso degradados
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '1568'
ht-degree: 0%

---


# Fluxos de acesso degradados {#degraded-access-flows}

A degradação fornece o desvio temporário de endpoints específicos de autenticação e autorização do MVPD. Normalmente, o Programador inicia essa ação, mas independentemente de quem aciona um evento de degradação, a ação depende de acordos anteriores feitos com os MVPDs afetados.

Para obter mais detalhes sobre o recurso de Degradação, consulte a documentação de [Degradação](../../../degradation-api-overview.md).

Os fluxos de acesso degradados permitem consultar os seguintes cenários:

* [Executar autenticação enquanto a degradação é aplicada](#perform-authentication-while-degradation-is-applied)
* [Recuperar decisões de autorização enquanto a degradação é aplicada](#retrieve-authorization-decisions-while-degradation-is-applied)
* [Recuperar decisões de pré-autorização enquanto a degradação é aplicada](#retrieve-preauthorization-decisions-while-degradation-is-applied)
* [Recuperar perfil enquanto a degradação é aplicada](#retrieve-profile-while-degradation-is-applied)

## Executar autenticação enquanto a degradação é aplicada {#perform-authentication-while-degradation-is-applied}

### Pré-requisitos {#prerequisites-perform-authentication-while-degradation-is-applied}

Antes de executar o fluxo de autenticação enquanto a degradação é aplicada, verifique se os seguintes pré-requisitos foram atendidos:

* O aplicativo de streaming deve iniciar uma sessão de autenticação quando precisar entrar com o MVPD.

>[!IMPORTANT]
> 
> Suposições
> 
> <br/>
> 
> * O aplicativo de streaming não tem um perfil válido para esse MVPD específico salvo no back-end do Adobe Pass.
> * Há uma regra de degradação AuthNAll aplicada à integração entre os `serviceProvider` e `mvpd` fornecidos.

### Fluxo de trabalho (WRK) {#workflow-perform-authentication-while-degradation-is-applied}

Siga as etapas fornecidas para implementar o fluxo de autenticação enquanto a degradação é aplicada, conforme mostrado no diagrama a seguir.

![Executar autenticação enquanto a degradação é aplicada](../../../assets/rest-api-v2/flows/access-degraded-flows/rest-api-v2-perform-authentication-while-degradation-is-applied-flow.png)

*Executar autenticação enquanto a degradação é aplicada*

1. **Criar sessão de autenticação:** o aplicativo de streaming reúne todos os dados necessários para iniciar uma sessão de autenticação chamando o ponto de extremidade Sessões.

   Consulte a documentação da API [Criar sessão de autenticação](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) para obter detalhes sobre:
   * Todos os parâmetros _necessários_, como `serviceProvider`, `mvpd`, `domainName` e `redirectUrl`
   * Todos os cabeçalhos _necessários_, como `Authorization` e `AP-Device-Identifier`
   * Todos os _parâmetros e cabeçalhos_ opcionais

1. **Verificar regras de degradação:** o servidor Adobe Pass verifica se há uma regra de degradação AuthNAll aplicada à integração entre o `serviceProvider` e o `mvpd` fornecidos.

1. **Indique a próxima ação:** A resposta do ponto de extremidade Sessions contém os dados necessários para orientar o aplicativo de streaming em relação à próxima ação:
   * O atributo `actionName` está definido como &quot;autorize&quot;.
   * O atributo `actionType` está definido como &quot;direto&quot;.

   Consulte a documentação da API [Criar sessão de autenticação](../../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md) para obter detalhes sobre as informações fornecidas na resposta da sessão.

   >[!IMPORTANT]
   >
   > O endpoint de Sessões valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve estar ativa.
   >
   > <br/>
   > 
   > Se a validação básica falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a [documentação de Códigos de erro aprimorados](../../../enhanced-error-codes.md).
   >
   > <br/>
   > 
   > O endpoint de Sessões usa os dados da solicitação para verificar se as condições de acesso degradadas são atendidas:
   >
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve ter uma regra de degradação AuthNAll aplicada.
   >
   > <br/>
   > 
   > Se a validação de acesso degradado falhar, a resposta assumirá como padrão o fluxo de autenticação básico.

1. **Continuar com fluxos de decisões:** o aplicativo de streaming pode continuar com fluxos de decisões subsequentes.

## Recuperar decisões de autorização enquanto a degradação é aplicada {#retrieve-authorization-decisions-while-degradation-is-applied}

### Pré-requisitos {#prerequisites-retrieve-authorization-decisions-while-degradation-is-applied}

Antes de recuperar decisões de autorização enquanto a degradação é aplicada, verifique se os seguintes pré-requisitos foram atendidos:

* O aplicativo de streaming deve recuperar uma decisão de autorização antes de reproduzir um recurso selecionado pelo usuário.

>[!IMPORTANT]
>
> Suposições
> 
> <br/>
> 
> * O aplicativo de streaming não tem um perfil válido para esse MVPD específico.
> * Há uma regra de degradação AuthZAll ou AuthNAll aplicada à integração entre o `serviceProvider` fornecido e `mvpd`.

### Fluxo de trabalho (WRK) {#workflow-retrieve-authorization-decisions-while-degradation-is-applied}

Siga as etapas fornecidas para implementar o fluxo de autorização enquanto a degradação é aplicada, conforme mostrado no diagrama a seguir.

![Recuperar decisões de autorização enquanto a degradação é aplicada](../../../assets/rest-api-v2/flows/access-degraded-flows/rest-api-v2-retrieve-authorization-decisions-while-degradation-is-applied-flow.png)

*Recuperar decisões de autorização enquanto a degradação é aplicada*

1. **Recuperar decisão de autorização:** o aplicativo de streaming reúne todos os dados necessários para obter uma decisão de autorização para um recurso específico, chamando o ponto de extremidade de Autorização de Decisões.

   Consulte a [Recuperar decisões de autorização usando a documentação da API do mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) específica para obter detalhes sobre:
   * Todos os parâmetros _necessários_, como `serviceProvider`, `mvpd` e `resources`
   * Todos os cabeçalhos _necessários_, como `Authorization` e `AP-Device-Identifier`
   * Todos os _parâmetros e cabeçalhos_ opcionais

1. **Verificar regras de degradação:** o servidor Adobe Pass verifica se há uma regra de degradação AuthZAll ou AuthNAll aplicada à integração entre o `serviceProvider` e o `mvpd` fornecidos.

1. **Retornar a decisão `Permit` com o token de mídia:** A resposta do ponto de extremidade de Autorização de Decisões contém uma decisão `Permit` e um token de mídia.

   Consulte a [Recuperar decisões de autorização usando a documentação específica da API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) para obter detalhes sobre as informações fornecidas em uma resposta de decisão.

   >[!IMPORTANT]
   >
   > O endpoint de autorização de decisões valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve estar ativa.
   >
   > <br/>
   > 
   > Se a validação básica falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a [documentação de Códigos de erro aprimorados](../../../enhanced-error-codes.md).
   >
   > <br/>
   >
   > O endpoint de autorização de decisões usa os dados da solicitação para verificar se as condições de acesso degradadas são atendidas:
   >
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve ter uma regra de degradação AuthZAll ou AuthNAll aplicada.
   >
   > <br/>
   > 
   > Se a validação de acesso degradado falhar, a resposta assumirá como padrão o fluxo de autorização básico.

1. **Iniciar fluxo com token de mídia:** O aplicativo de streaming usa o token de mídia para reproduzir o conteúdo.

## Recuperar decisões de pré-autorização enquanto a degradação é aplicada {#retrieve-preauthorization-decisions-while-degradation-is-applied}

### Pré-requisitos {#prerequisites-retrieve-preauthorization-decisions-while-degradation-is-applied}

Antes de recuperar decisões de pré-autorização enquanto a degradação é aplicada, verifique se os seguintes pré-requisitos foram atendidos:

* O aplicativo de transmissão deseja recuperar as decisões de pré-autorização para exibir uma lista de recursos junto com seus status associados.

>[!IMPORTANT]
>
> Suposições
>
> <br/>
> 
> * O aplicativo de streaming não tem um perfil válido para esse MVPD específico.
> * Há uma regra de degradação AuthZAll ou AuthNAll aplicada à integração entre o `serviceProvider` fornecido e `mvpd`.

### Fluxo de trabalho (WRK) {#workflow-retrieve-preauthorization-decisions-while-degradation-is-applied}

Siga as etapas fornecidas para implementar o fluxo de pré-autorização enquanto a degradação é aplicada, conforme mostrado no diagrama a seguir.

![Recuperar decisões de pré-autorização enquanto a degradação é aplicada](../../../assets/rest-api-v2/flows/access-degraded-flows/rest-api-v2-retrieve-preauthorization-decisions-while-degradation-is-applied-flow.png)

*Recuperar decisões de pré-autorização enquanto a degradação é aplicada*

1. **Recuperar decisões de pré-autorização:** O aplicativo de streaming reúne todos os dados necessários para obter decisões de pré-autorização para uma lista de recursos, chamando o ponto de extremidade de Pré-autorização de Decisões.

   Consulte a [Recuperar decisões de pré-autorização usando a documentação da API do mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) específica para obter detalhes sobre:
   * Todos os parâmetros _necessários_, como `serviceProvider`, `mvpd` e `resources`
   * Todos os cabeçalhos _necessários_, como `Authorization` e `AP-Device-Identifier`
   * Todos os _parâmetros e cabeçalhos_ opcionais

1. **Verificar regras de degradação:** o servidor Adobe Pass verifica se há uma regra de degradação AuthZAll ou AuthNAll aplicada à integração entre o `serviceProvider` e o `mvpd` fornecidos.

1. **Retornar decisões de pré-autorização:** A resposta de Ponto de Extremidade de Pré-autorização de Decisões contém uma decisão `Permit` para cada recurso.

   Consulte a [Recuperar decisões de pré-autorização usando a documentação específica da API mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) para obter detalhes sobre as informações fornecidas em uma resposta de decisão.

   >[!IMPORTANT]
   >
   > O endpoint de pré-autorização de decisões valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve estar ativa.
   >
   > <br/>
   > 
   > Se a validação básica falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a [documentação de Códigos de erro aprimorados](../../../enhanced-error-codes.md).
   >
   > <br/>
   >
   > O endpoint de pré-autorização de decisões usa os dados da solicitação para verificar se as condições de acesso degradadas são atendidas:
   >
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve ter uma regra de degradação AuthZAll ou AuthNAll aplicada.
   >
   > <br/>
   > 
   > Se a validação de acesso degradado falhar, a resposta assumirá como padrão o fluxo básico de pré-autorização.

1. **Manipular decisões de pré-autorização:** o aplicativo de streaming processa a resposta e pode usá-la para exibir, opcionalmente, o status apropriado para cada recurso na interface do usuário.

## Recuperar perfil enquanto a degradação é aplicada {#retrieve-profile-while-degradation-is-applied}

>[!IMPORTANT]
>
> A consulta de endpoint de Perfis é opcional enquanto a degradação é aplicada.
>
> <br/>
> 
> A resposta do endpoint Sessions instrui o aplicativo a continuar com os fluxos de decisão enquanto a degradação é aplicada. Para obter mais detalhes, consulte a seção [Executar autenticação enquanto a degradação é aplicada](#perform-authentication-while-degradation-is-applied).

### Pré-requisitos {#prerequisites-retrieve-profile-while-degradation-is-applied}

Antes de recuperar o perfil de um MVPD específico enquanto a degradação é aplicada, verifique se os seguintes pré-requisitos foram atendidos:

* O aplicativo de streaming, que tem um identificador `mvpd` selecionado ou armazenado em cache, deseja recuperar o perfil de um MVPD específico.

>[!IMPORTANT]
>
> Suposições
>
> <br/>
> 
> * O aplicativo de streaming não tem um perfil válido para esse MVPD específico.
> * Há uma regra de degradação AuthNAll aplicada à integração entre os `serviceProvider` e `mvpd` fornecidos.

### Fluxo de trabalho (WRK) {#workflow-retrieve-profile-while-degradation-is-applied}

Siga as etapas fornecidas para implementar o fluxo de recuperação de perfil para um MVPD específico enquanto a degradação é aplicada, conforme mostrado no diagrama a seguir.

![Recuperar perfil enquanto a degradação é aplicada](../../../assets/rest-api-v2/flows/access-degraded-flows/rest-api-v2-retrieve-profile-while-degradation-is-applied-flow.png)

*Recuperar perfil enquanto a degradação é aplicada*

1. **Recuperar perfil para mvpd específico:** O aplicativo de streaming reúne todos os dados necessários para recuperar informações de perfil para esse MVPD específico, enviando uma solicitação ao ponto de extremidade de Perfis.

   Consulte a [Recuperar perfil para a documentação específica da API mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md) para obter detalhes sobre:
   * Todos os parâmetros _necessários_, como `serviceProvider` e `mvpd`
   * Todos os cabeçalhos _necessários_, como `Authorization` e `AP-Device-Identifier`
   * Todos os _parâmetros e cabeçalhos_ opcionais

1. **Verificar regras de degradação:** o servidor Adobe Pass verifica se há uma regra de degradação AuthNAll aplicada à integração entre o `serviceProvider` e o `mvpd` fornecidos.

1. **Retornar informações sobre o perfil degradado:** A resposta do ponto de extremidade Profiles contém informações sobre o perfil degradado, incluindo o atributo `type` definido como &quot;degradado&quot;.

   Consulte a [Recuperar perfil para a documentação específica da API do mvpd](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-mvpd.md) para obter detalhes sobre as informações fornecidas na resposta do perfil.

   >[!IMPORTANT]
   >
   > O endpoint de Perfis valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve estar ativa.
   >
   > <br/>
   > 
   > Se a validação básica falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a [documentação de Códigos de erro aprimorados](../../../enhanced-error-codes.md).
   >
   > <br/>
   > 
   > O endpoint de Perfis usa os dados da solicitação para verificar se as condições de acesso degradadas são atendidas:
   >
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve ter uma regra de degradação AuthNAll aplicada.
   >
   > <br/>
   > 
   > Se a validação de acesso degradado falhar, a resposta assumirá como padrão o fluxo básico de recuperação do perfil.

1. **Continuar com fluxos de decisões:** Se a resposta do ponto de extremidade Perfis contiver um perfil, o aplicativo de streaming usará as informações de perfil degradadas para continuar com os fluxos de decisões subsequentes.

1. **Indicar novo fluxo de autenticação básico:** Se a resposta do ponto de extremidade Perfis não contiver um perfil, o aplicativo de streaming indicará ao usuário que ele iniciará um novo fluxo de autenticação básico.

>[!NOTE]
>
> As etapas do fluxo de recuperação de perfil para um código de autenticação específico são as mesmas descritas acima, exceto que o ponto de extremidade usado é o descrito na documentação [Recuperar perfil para código específico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles-for-specific-code.md).
