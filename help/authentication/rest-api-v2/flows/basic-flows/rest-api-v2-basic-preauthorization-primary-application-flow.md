---
title: Pré-autorização Básica - Aplicativo Principal - Fluxo
description: REST API V2 - Pré-autorização básica - Aplicativo principal - Fluxo
source-git-commit: c3aa2a24b242669ce0818b95ec34de2adec8001b
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 0%

---


# Fluxo básico de pré-autorização executado no aplicativo principal {#basic-preauthorization-flow-performed-within-primary-application}

O **Fluxo de pré-autorização** no direito de Autenticação Adobe Pass permite que o aplicativo de streaming determine se um MVPD pode permitir ou negar ao usuário acesso a uma lista de recursos. Essa verificação garante que o aplicativo possa apresentar informações precisas ao usuário sobre o conteúdo que ele pode estar qualificado a visualizar.

## Recuperar decisões de pré-autorização usando mvpd específico {#retrieve-preauthorization-decisions-using-specific-mvpd}

### Pré-requisitos {#prerequisites-retrieve-preauthorization-decisions-using-specific-mvpd}

Antes de recuperar decisões de pré-autorização usando um MVPD específico, verifique se os seguintes pré-requisitos foram atendidos:

* O aplicativo de streaming deve ter um perfil regular válido, criado com êxito para o MVPD usando um dos fluxos de autenticação básicos:
   * [Executar autenticação no aplicativo principal](../basic-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Realizar autenticação no aplicativo secundário com mvpd pré-selecionado](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Executar autenticação no aplicativo secundário sem mvpd pré-selecionado](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* O aplicativo de transmissão deseja recuperar as decisões de pré-autorização para exibir uma lista de recursos junto com seus status associados.

### Fluxo de trabalho (WRK) {#workflow-retrieve-preauthorization-decisions-using-specific-mvpd}

Siga as etapas fornecidas para implementar o fluxo básico de pré-autorização usando um MVPD específico executado em um aplicativo principal, conforme mostrado no diagrama a seguir.

![Recuperar decisões de pré-autorização usando mvpd](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-retrieve-preauthorization-decisions-within-primary-application-using-specific-mvpd.png) específico

*Recuperar decisões de pré-autorização usando mvpd* específico

1. **Recuperar decisões de pré-autorização:** O aplicativo de streaming reúne todos os dados necessários para obter decisões de pré-autorização para uma lista de recursos, chamando o ponto de extremidade de Pré-autorização de Decisões.

   Consulte a [Recuperar decisões de pré-autorização usando a documentação da API do mvpd](../../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) específica para obter detalhes sobre:
   * Todos os parâmetros _necessários_, como `serviceProvider`, `mvpd` e `resources`
   * Todos os cabeçalhos _necessários_, como `Authorization` e `AP-Device-Identifier`
   * Todos os _parâmetros e cabeçalhos_ opcionais

1. **Localizar perfil regular:** O servidor Adobe Pass identifica um perfil válido com base nos parâmetros e cabeçalhos recebidos.

1. **Recuperar decisões de MVPD para recursos solicitados:** O servidor do Adobe Pass chama o ponto de extremidade de pré-autorização de MVPD para obter uma decisão `Permit` ou `Deny` para cada recurso recebido do aplicativo de streaming.

1. **Retornar decisões de pré-autorização:** A resposta de Ponto de Extremidade de Pré-autorização de Decisões contém uma decisão `Permit` ou `Deny` para cada recurso:
   * Uma decisão `Permit` significa que o recurso é reproduzível. A resposta não inclui um token de mídia, pois o fluxo de pré-autorização não deve ser usado para reproduzir recursos.
   * Uma decisão `Deny` significa que o recurso não pode ser reproduzido. A resposta inclui uma carga de erro que segue a documentação de [Códigos de erro aprimorados](../../../enhanced-error-codes.md).

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
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../enhanced-error-codes.md).

1. **Manipular decisões de pré-autorização:** o aplicativo de streaming processa a resposta e pode usá-la para exibir, opcionalmente, o status apropriado para cada recurso na interface do usuário.
