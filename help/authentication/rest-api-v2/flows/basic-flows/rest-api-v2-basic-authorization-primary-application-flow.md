---
title: Autorização Básica - Aplicativo Principal - Fluxo
description: REST API V2 - Autorização básica - Aplicativo principal - Fluxo
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '592'
ht-degree: 0%

---


# Fluxo de autorização básico executado no aplicativo principal {#basic-authorization-flow-performed-within-primary-application}

>[!NOTE]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

O **Fluxo de autorização** dentro do direito de Autenticação Adobe Pass permite que o aplicativo de streaming determine se um MVPD permite ou nega a solicitação do usuário para transmitir conteúdo. Se a decisão for `Permit`, a resposta incluirá um token de mídia. O servidor do Adobe Pass assina o token de mídia e permite que o aplicativo de transmissão use a biblioteca de verificação de token de mídia para verificar sua autenticidade antes que o fluxo seja lançado.

A verificação com a biblioteca do verificador de token de mídia deve ocorrer no serviço de back-end do aplicativo de streaming vinculado à cadeia de permissões para liberar um fluxo do CDN.

## Recuperar decisões de autorização usando mvpd específico {#retrieve-authorization-decisions-using-specific-mvpd}

### Pré-requisitos {#prerequisites-retrieve-authorization-decisions-using-specific-mvpd}

Antes de recuperar decisões de autorização usando um MVPD específico, verifique se os seguintes pré-requisitos foram atendidos:

* O aplicativo de streaming deve ter um perfil regular válido, criado com êxito para o MVPD usando um dos fluxos de autenticação básicos:
   * [Executar autenticação no aplicativo principal](../basic-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Realizar autenticação no aplicativo secundário com mvpd pré-selecionado](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Executar autenticação no aplicativo secundário sem mvpd pré-selecionado](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* O aplicativo de streaming deve recuperar uma decisão de autorização antes de reproduzir um recurso selecionado pelo usuário.

### Fluxo de trabalho (WRK) {#workflow-retrieve-authorization-decisions-using-specific-mvpd}

Siga as etapas fornecidas para implementar o fluxo de autorização básico usando um MVPD específico executado em um aplicativo principal, conforme mostrado no diagrama a seguir.

![Recuperar decisões de autorização usando mvpd](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-retrieve-authorization-decisions-within-primary-application-using-specific-mvpd.png) específico

*Recuperar decisões de autorização usando mvpd* específico

1. **Recuperar decisão de autorização:** o aplicativo de streaming reúne todos os dados necessários para obter uma decisão de autorização para um recurso específico, chamando o ponto de extremidade de Autorização de Decisões.

   Consulte a [Recuperar decisões de autorização usando a documentação da API do mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) específica para obter detalhes sobre:
   * Todos os parâmetros _necessários_, como `serviceProvider`, `mvpd` e `resources`
   * Todos os cabeçalhos _necessários_, como `Authorization` e `AP-Device-Identifier`
   * Todos os _parâmetros e cabeçalhos_ opcionais

1. **Localizar perfil regular:** O servidor Adobe Pass identifica um perfil válido com base nos parâmetros e cabeçalhos recebidos.

1. **Recuperar decisão MVPD para o recurso solicitado:** O servidor do Adobe Pass chama o ponto de extremidade de autorização MVPD para obter uma decisão `Permit` ou `Deny` para o recurso específico recebido do aplicativo de streaming.

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
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../enhanced-error-codes.md).

1. **Iniciar fluxo com token de mídia:** O aplicativo de streaming usa o token de mídia para reproduzir o conteúdo.

1. **Retornar a decisão `Deny` com detalhes:** A resposta do ponto de extremidade de Autorização de Decisões contém uma decisão `Deny` e uma carga de erro que segue a documentação de [Códigos de Erro Aprimorados](../../../enhanced-error-codes.md).

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
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../enhanced-error-codes.md).

1. **Identificador de detalhes de decisão `Deny`:** o aplicativo de streaming processa as informações de erro da resposta e pode usá-las para exibir opcionalmente uma mensagem específica na interface do usuário.
