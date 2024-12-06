---
title: Perfis básicos - Aplicativo secundário - Fluxo
description: REST API V2 - Perfis básicos - Aplicativo secundário - Fluxo
exl-id: 1fcefcfa-7534-4b85-b3b5-df513685d66b
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 0%

---

# Fluxo de perfis básicos realizado no aplicativo secundário {#basic-profiles-flow-secondary-application}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> A implementação da REST API V2 é limitada pela documentação do [Mecanismo de limitação](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

O **Fluxo de perfis** dentro do direito de Autenticação Adobe Pass permite que o aplicativo secundário acesse informações sobre logons de usuário ativos.

O fluxo de perfis básicos permite consultar os seguintes cenários:

* [Recuperar perfil para código específico](#retrieve-profile-for-specific-code)

## Recuperar perfil para código específico {#retrieve-profile-for-specific-code}

### Pré-requisitos {#prerequisites-retrieve-profile-for-specific-code}

Antes de recuperar o perfil para um código de autenticação específico, verifique se os seguintes pré-requisitos foram atendidos:

* O aplicativo secundário, que tem um `code` usado para executar a autenticação interativa com o MVPD, deseja recuperar o perfil para um código de autenticação específico.

### Fluxo de trabalho (WRK) {#workflow-retrieve-profile-for-specific-code}

Siga as etapas fornecidas para implementar o fluxo básico de recuperação de perfil para um código de autenticação específico executado em um aplicativo secundário, conforme mostrado no diagrama a seguir.

![Recuperar perfil para código específico](../../../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-retrieve-profile-within-secondary-application-for-specific-code.png)

*Recuperar perfil para código específico*

1. **Recuperar perfil para código específico:** O aplicativo secundário reúne todos os dados necessários para recuperar informações de perfil para esse código de autenticação específico enviando uma solicitação para o ponto de extremidade de Perfis.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Recuperar perfil para código específico](../../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md) para obter detalhes sobre:
   >
   > * Todos os parâmetros _necessários_, como `serviceProvider` e `code`
   > * Todos os cabeçalhos _necessários_, como `Authorization`
   > * Todos os _parâmetros e cabeçalhos_ opcionais

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
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Indicar fluxo de autenticação concluído com êxito:** Se a resposta do ponto de extremidade Profiles contiver um perfil, o aplicativo secundário processará a resposta e poderá usá-la para exibir opcionalmente uma mensagem específica na interface do usuário.

1. **O fluxo de autenticação de indicação encontrou um problema:** Se a resposta do ponto de extremidade de Perfis não contiver um perfil, o aplicativo secundário processará a resposta e poderá usá-la para exibir opcionalmente uma mensagem específica na interface do usuário.
