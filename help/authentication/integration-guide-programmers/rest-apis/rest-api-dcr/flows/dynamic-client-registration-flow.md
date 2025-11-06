---
title: Fluxo de registro dinâmico do cliente
description: Fluxo de registro dinâmico do cliente
exl-id: d881cf0a-de09-4b1d-a094-d5490f944796
source-git-commit: b753c6a6bdfd8767e86cbe27327752620158cdbb
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# Fluxo de registro dinâmico do cliente {#dynamic-client-registration-flow}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> A implementação da API de Registro de Cliente Dinâmico é limitada pela documentação do [Mecanismo de limitação](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

## Acessar APIs protegidas pelo Adobe Pass {#access-adobe-pass-protected-apis}

### Pré-requisitos {#prerequisites-access-adobe-pass-protected-apis}

Antes de acessar APIs protegidas pelo Adobe Pass, verifique se os seguintes pré-requisitos foram atendidos:

* Um representante do cliente deve criar um aplicativo registrado conforme descrito na seção [Gerenciar aplicativos registrados](../dynamic-client-registration-overview.md#manage-registered-applications).
* Um representante do cliente deve baixar e incorporar uma instrução de software conforme descrito na seção [Gerenciar instruções de software](../dynamic-client-registration-overview.md#manage-software-statements).

>[!IMPORTANT]
>
> Os SDKs de autenticação da Adobe Pass são responsáveis por obter e atualizar as credenciais do cliente e o token de acesso em nome do aplicativo do cliente.
> 
> Para todas as outras APIs protegidas pelo Adobe Pass, o aplicativo cliente deve seguir o fluxo de trabalho abaixo.

### Fluxo de trabalho (WRK) {#workflow-access-adobe-pass-protected-apis}

Siga as etapas fornecidas para acessar APIs protegidas pelo Adobe Pass, conforme mostrado no diagrama a seguir.

![Acessar APIs protegidas pelo Adobe Pass](../../../../assets/dcr-api/dcr-api-access-adobe-pass-protected-apis.png)

*Acessar APIs protegidas pelo Adobe Pass*

1. **Recuperar credenciais do cliente:** O aplicativo cliente reúne todos os dados necessários para recuperar credenciais do cliente chamando o ponto de extremidade de Registro do Cliente.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Recuperar credenciais do cliente](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#request) para obter detalhes sobre:
   >
   > * Todos os parâmetros _necessários_, como `software_statement`
   > * Todos os cabeçalhos _necessários_, como `Content-Type`, `X-Device-Info`
   > * Todos os _parâmetros e cabeçalhos_ opcionais

1. **Retornar credenciais de cliente:** A resposta do ponto de extremidade do Registro do Cliente contém informações sobre as credenciais de cliente associadas aos parâmetros e cabeçalhos recebidos.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Recuperar credenciais do cliente](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#success) para obter detalhes sobre as informações fornecidas em uma resposta de credenciais do cliente.
   >
   > <br/>
   >
   > O Registro do cliente valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   >
   > <br/>
   >
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a [Recuperar credenciais do cliente](../apis/dynamic-client-registration-apis-retrieve-client-credentials.md#error) documentação da API.

   >[!TIP]
   >
   > As credenciais do cliente devem ser armazenadas em cache e usadas indefinidamente.

1. **Recuperar token de acesso:** O aplicativo cliente reúne todos os dados necessários para recuperar o token de acesso chamando o ponto de extremidade do Token do Cliente.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Recuperar token de acesso](../apis/dynamic-client-registration-apis-retrieve-access-token.md#request) para obter detalhes sobre:
   >
   > * Todos os parâmetros _necessários_, como `client_id`, `client_secret` e `grant_type`
   > * Todos os cabeçalhos _necessários_, como `Content-Type`, `X-Device-Info`
   > * Todos os _parâmetros e cabeçalhos_ opcionais

1. **Retornar token de acesso:** A resposta do ponto de extremidade do Token do Cliente contém informações sobre o token de acesso associado aos parâmetros e cabeçalhos recebidos.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Recuperar token de acesso](../apis/dynamic-client-registration-apis-retrieve-access-token.md#success) para obter detalhes sobre as informações fornecidas em uma resposta de token de acesso.
   >
   > <br/>
   >
   > O token do cliente valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   >
   > <br/>
   >
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a [Recuperar token de acesso](../apis/dynamic-client-registration-apis-retrieve-access-token.md#error) documentação da API.

   >[!TIP]
   >
   > O token de acesso deve ser armazenado em cache e usado somente durante o período especificado (por exemplo, 24 horas de vida útil). Após a expiração, o aplicativo cliente deve solicitar um novo token de acesso.

1. **Continuar com o acesso a APIs protegidas:** o aplicativo cliente usa o token de acesso para acessar outras APIs protegidas do Adobe Pass. O aplicativo cliente deve incluir o token de acesso no cabeçalho da solicitação `Authorization` usando o esquema de autenticação `Bearer` (ou seja, `Authorization: Bearer <access_token>`).

   >[!IMPORTANT]
   >
   > As APIs protegidas pelo Adobe Pass validam o token de acesso para garantir que as condições básicas sejam atendidas:
   >
   > * O _access_token_ deve ser válido.
   > * O _access_token_ deve ser associado a um _client_id_ e _client_secret_ válidos.
   > * O _access_token_ deve ser associado a um _software_statement_ válido.
   >
   > <br/>
   >
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../features-standard/error-reporting/enhanced-error-codes.md).
