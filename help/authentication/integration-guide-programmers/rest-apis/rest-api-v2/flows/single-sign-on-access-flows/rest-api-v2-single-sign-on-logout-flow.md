---
title: Logout único - Fluxo
description: REST API V2 - Logout único - Fluxo
exl-id: d7092ca7-ea7b-4e92-b45f-e373a6d673d6
source-git-commit: 92417dd4161be8ba97535404e262fd26d67383e4
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 0%

---

# Fluxo de logout único {#single-logout-flow}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> A implementação da REST API V2 é limitada pela documentação do [Mecanismo de limitação](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

>[!MORELIKETHIS]
>
> Visite também as [Perguntas frequentes sobre a REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authentication-phase-faqs-general).

## Iniciar logout único para mvpd específico {#initiate-single-logout-for-specific-mvpd}

### Pré-requisitos {#prerequisites-initiate-single-logout-for-specific-mvpd}

Antes de iniciar o logout único para uma MVPD específica, verifique se os seguintes pré-requisitos foram atendidos:

* O segundo aplicativo de streaming deve ter um perfil de logon único válido que tenha sido criado com êxito para o MVPD usando um dos fluxos de autenticação de logon único:
   * [Executar autenticação por meio de logon único usando a identidade da plataforma](rest-api-v2-single-sign-on-platform-identity-flows.md)
   * [Executar autenticação por meio de logon único usando token de serviço](rest-api-v2-single-sign-on-service-token-flows.md)
* O segundo aplicativo de streaming deve iniciar o fluxo de logout único quando precisar sair do MVPD.

>[!IMPORTANT]
> 
> Suposições
>
> <br/>
> 
> * O primeiro e o segundo aplicativos de streaming obtêm a mesma carga de identificador de plataforma exclusiva que `JWS` ou `JWE` ou a mesma carga de identificador de usuário exclusiva que `JWS`.

### Fluxo de trabalho (WRK) {#workflow-initiate-single-logout-for-specific-mvpd}

Execute as etapas fornecidas para implementar o fluxo de logout único para uma MVPD específica, conforme mostrado no diagrama a seguir.

![Iniciar logout único para mvpd específico](/help/authentication/assets/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-initiate-single-logout-for-specific-mvpd-flow.png)

*Iniciar logout único para mvpd específico*

1. **Iniciar logout do Adobe Pass:** o aplicativo de streaming reúne todos os dados necessários para iniciar o fluxo de logout chamando o ponto de extremidade de Logout do Adobe Pass.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Iniciar logout para mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) específica para obter detalhes sobre:
   >
   > * Todos os parâmetros _necessários_, como `serviceProvider`, `mvpd` e `redirectUrl`
   > * Todos os cabeçalhos _necessários_, como `Authorization`, `AP-Device-Identifier`
   > * Todos os _parâmetros e cabeçalhos_ opcionais
   >
   > <br/>
   >
   > A aplicação de transmissão deve garantir que ela inclua um valor válido para o identificador exclusivo da plataforma ou o identificador exclusivo do usuário antes de fazer uma solicitação.
   >
   > <br/>
   > 
   > Para obter mais detalhes sobre o cabeçalho `Adobe-Subject-Token`, consulte a documentação [Adobe-Subject-Token](../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md).
   > 
   > <br/>
   > 
   > Para obter mais detalhes sobre o cabeçalho `AD-Service-Token`, consulte a documentação [AD-Service-Token](../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md).

1. **Localizar perfis de logon regular e único:** O servidor Adobe Pass identifica perfis válidos de logon regular e único com base nos parâmetros e cabeçalhos recebidos.

1. **Excluir perfis de logon único e regular:** o servidor Adobe Pass exclui os perfis de logon único e regular identificados do back-end do Adobe Pass.

1. **Indique a próxima ação:** A resposta do ponto de extremidade de logout do Adobe Pass contém os dados necessários para orientar o aplicativo de streaming em relação à próxima ação.

   >[!IMPORTANT]
   >
   > Consulte a documentação da API [Iniciar logout para mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) específica para obter detalhes sobre as informações fornecidas em uma resposta de logout.
   > 
   > <br/>
   > 
   > O endpoint de logout do Adobe Pass valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve estar ativa.
   >
   > <br/>
   > 
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../../features-standard/error-reporting/enhanced-error-codes.md).

1. **Indicar logout concluído:** Se a MVPD não oferecer suporte ao fluxo de logout, o aplicativo de streaming processará a resposta e poderá usá-la para exibir uma mensagem específica na interface do usuário.

1. **Iniciar logout do MVPD:** Se o MVPD não oferecer suporte ao fluxo de logout, o aplicativo de streaming processará a resposta e usará um agente do usuário para iniciar o fluxo de logout com o MVPD. O fluxo pode incluir vários redirecionamentos para sistemas MVPD. Ainda assim, o resultado é que o MVPD executa sua limpeza interna e envia a confirmação de logout final de volta para o back-end do Adobe Pass.

1. **Indicar logout concluído:** O aplicativo de streaming pode esperar que o agente do usuário alcance o `redirectUrl` fornecido e pode usá-lo como um sinal para exibir, opcionalmente, uma mensagem específica na interface do usuário.

>[!NOTE]
>
> As etapas para o fluxo de logout único são as mesmas descritas acima, se iniciadas a partir do primeiro aplicativo de streaming.
