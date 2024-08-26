---
title: Visão geral do registro dinâmico do cliente
description: Visão geral do registro dinâmico do cliente
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '840'
ht-degree: 0%

---


# Visão geral do registro dinâmico do cliente {#dynamic-client-registration-overview}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

O registro de cliente dinâmico representa um mecanismo de autorização definido por [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591) e é baseado na estrutura de autorização OAuth 2.0 descrita por [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749).

O Adobe Pass fornece um serviço dinâmico de registro de cliente que permite o acesso às seguintes APIs protegidas:

* APIs de gerenciamento de autenticação da Adobe Pass:
   * [Redefinir API Temp Pass](../reset-temp-pass.md)
   * [API de degradação](../degradation-api-overview.md)
   * [API MVPD do proxy](../proxy-mvpd-webserv.md)
   * [API de monitoramento do serviço de qualificação](../entitlement-service-monitoring-api.md)
* APIs REST de autenticação da Adobe Pass:
   * [REST API V1](../rest-api-reference.md)
   * [REST API V2](../rest-api-v2/apis/rest-api-v2-apis-overview.md)
* SDKs de autenticação da Adobe Pass:
   * [JAVASCRIPT SDK](../javascript-sdk-api-reference.md)
   * [iOS/tvOS SDK](../iostvos-sdk-api-reference.md)
   * [ANDROID SDK](../android-sdk-api-reference.md)
   * [FireOS SDK](../amazon-fireos-native-client-api-reference.md)

>[!IMPORTANT]
>
> O mecanismo dinâmico de autorização de registro de clientes substitui as soluções de Autenticação da Adobe Pass mais antigas que estão sujeitas a serem descontinuadas:
>
> * O mecanismo de ID do solicitante assinado.
> * O mecanismo de listagem de domínios.
> * O mecanismo da chave da API.

Com a adoção do registro dinâmico de clientes, os principais benefícios são:

* Segurança aprimorada.
* Modelo unificado entre plataformas.
* Controle refinado do ciclo de vida do aplicativo.

Para saber mais sobre como gerenciar e usar o registro de cliente dinâmico, consulte as seções a seguir.

## Gerenciamento dinâmico de registro de clientes {#dynamic-client-registration-management}

O processo de gerenciamento dinâmico de registros de clientes permite que aplicativos clientes sejam executados em plataformas específicas e que precisem de acesso a APIs de Autenticação Adobe Pass específicas para se registrarem por meio do [Painel do Adobe Pass TVE](https://console.auth.adobe.com/).

O Adobe Pass TVE Dashboard é uma ferramenta para clientes de autenticação da Adobe Pass (programadores) gerenciarem suas configurações e dados. Este painel de autoatendimento habilita uma série de funcionalidades descritas na documentação do [Guia do Usuário do Painel do Adobe Pass TVE](../tve-dashboard-user-guide.md).

Caso você tenha acesso ao [Painel do Adobe Pass TVE](https://console.auth.adobe.com/), siga as etapas das seções abaixo para criar um aplicativo registrado e baixar a declaração de software.

### Gerenciar aplicativos registrados {#manage-registered-applications}

>[!IMPORTANT]
>
> Caso você não tenha acesso ao Painel do Adobe Pass TVE, crie um tíquete por meio do nosso [Zendesk](https://adobeprimetime.zendesk.com) e peça ao seu Gerente técnico de conta (TAM) para criar um aplicativo registrado e compartilhar a declaração de software com você.

Há duas maneiras disponíveis de criar um aplicativo registrado:

* **Nível do programador**

  O processo de registro no nível do programador permite criar um aplicativo registrado vinculado a todos os canais disponíveis ou a um subconjunto selecionado de canais. Para obter mais detalhes, consulte a seção [Criar um aplicativo registrado no nível do programador](../tve-dashboard-user-guide.md#create-registered-application-programmer-level) da documentação do [Guia do Usuário do Painel do TVE](../tve-dashboard-user-guide.md).


* **Nível do canal**

  O processo de registro em nível de canal permite criar um aplicativo registrado vinculado somente ao canal selecionado no momento. Para obter mais detalhes, consulte a seção [Criar um aplicativo registrado no nível de canal](../tve-dashboard-user-guide.md#create-registered-application-channel-level) da documentação do [Guia de Usuário do Painel do TVE](../tve-dashboard-user-guide.md).

>[!IMPORTANT]
>
> É recomendável criar aplicativos registrados com permissões mais específicas e limitadas para melhorar a segurança e impedir o acesso não autorizado. Portanto, ao criar aplicativos registrados, considere usar opções mais restritas para os `channels`, `platforms` e `scopes` atribuídos.
>
> É recomendável criar um novo aplicativo registrado para cada atualização importante do aplicativo cliente para gerenciar o ciclo de vida e o uso. Quando necessário, crie um tíquete por meio de nossa [Zendesk](https://adobeprimetime.zendesk.com) e peça ao seu Gerente técnico de conta (TAM) para revogar um aplicativo registrado para bloquear a funcionalidade de uma versão específica do aplicativo cliente.

### Gerenciar demonstrativos de software {#manage-software-statements}

>[!IMPORTANT]
>
> Caso você não tenha acesso ao Painel do Adobe Pass TVE, crie um tíquete por meio do nosso [Zendesk](https://adobeprimetime.zendesk.com) e peça ao seu Gerente técnico de conta (TAM) para criar um aplicativo registrado e compartilhar a declaração de software com você.

Antes de baixar uma instrução de software, verifique se você tem um aplicativo registrado criado conforme descrito na seção [Gerenciar aplicativos registrados](#manage-registered-applications) que atenda aos requisitos do aplicativo cliente.

Há duas maneiras disponíveis de baixar uma instrução de software com base no nível em que o aplicativo registrado foi criado:

* **Nível do programador**

  Para obter mais detalhes, consulte a seção [Baixar uma instrução de software no nível do programador](../tve-dashboard-user-guide.md#download-software-statement-programmer-level) da documentação do [Guia do Usuário do Painel do TVE](../tve-dashboard-user-guide.md).

* **Nível do canal**

  Para obter mais detalhes, consulte a seção [Baixar uma instrução de software no nível de canal](../tve-dashboard-user-guide.md#download-software-statement-channel-level) da documentação do [Guia de Usuário do Painel do TVE](../tve-dashboard-user-guide.md).

A instrução de software é um JSON Web Token (`JWT`) que contém informações sobre o software do aplicativo cliente como um pacote. Quando apresentada à API [Recuperar credenciais do cliente](./apis/dynamic-client-registration-apis-retrieve-client-credentials.md), a instrução de software é assinada digitalmente usando a Assinatura da Web JSON (`JWS`).

Para obter explicações mais detalhadas sobre o que são as instruções de software e como elas funcionam, consulte a documentação do [RFC 7591](https://tools.ietf.org/html/rfc7591).

## Fluxo de registro dinâmico do cliente  {#dynamic-client-registration-flow}

Em resumo, o mecanismo dinâmico de autorização de registro de cliente envolve várias etapas:

**Gerenciamento**

* Um representante do cliente deve criar um aplicativo registrado conforme descrito na seção [Gerenciar aplicativos registrados](#manage-registered-applications).
* Um representante do cliente deve baixar e incorporar uma instrução de software conforme descrito na seção [Gerenciar instruções de software](#manage-software-statements).

**Fluxo**

* O aplicativo cliente deve obter as credenciais do cliente conforme descrito na documentação da API [Recuperar credenciais do cliente](./apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
* O aplicativo cliente deve obter o token de acesso conforme descrito na documentação da API [Recuperar token de acesso](./apis/dynamic-client-registration-apis-retrieve-access-token.md).

Consulte a documentação do [Fluxo de Registro Dinâmico do Cliente](./flows/dynamic-client-registration-flow.md) para entender melhor como acessar APIs protegidas pelo Adobe Pass. Além disso, você também pode assistir a esta gravação de [webinário](https://my.adobeconnect.com/pzkp8ujrigg1/), que fornece mais contexto e inclui uma demonstração.
