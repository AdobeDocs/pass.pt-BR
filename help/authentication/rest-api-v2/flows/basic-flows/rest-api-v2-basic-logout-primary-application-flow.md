---
title: Logout básico - Aplicativo principal - Fluxo
description: REST API V2 - Logout básico - Aplicativo principal - Fluxo
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '937'
ht-degree: 0%

---


# Fluxo de logout básico executado no aplicativo principal {#basic-logout-flow-performed-within-primary-application}

>[!NOTE]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

O **fluxo de logout** no direito de Autenticação Adobe Pass permite que o aplicativo de streaming execute duas etapas principais:

* Exclua os perfis comuns salvos no back-end do Adobe Pass.
* Use um agente do usuário (navegador) para navegar até o endpoint de logout do MVPD, acionando uma limpeza no back-end do MVPD.

O fluxo de logout básico permite consultar os seguintes cenários:

* [Iniciar logout para mvpd específico com ponto de extremidade de logout](#initiate-logout-for-specific-mvpd-with-logout-endpoint)
* [Iniciar logout para mvpd específico sem ponto de extremidade de logout](#initiate-logout-for-specific-mvpd-without-logout-endpoint)

## Iniciar logout para mvpd específico com ponto de extremidade de logout {#initiate-logout-for-specific-mvpd-with-logout-endpoint}

### Pré-requisitos {#prerequisites-initiate-logout-for-specific-mvpd-with-logout-endpoint}

Antes de iniciar o logout de um MVPD específico com um endpoint de logout, verifique se os seguintes pré-requisitos foram atendidos:

* O aplicativo de streaming deve ter um perfil regular válido, criado com êxito para o MVPD usando um dos fluxos de autenticação básicos:
   * [Executar autenticação no aplicativo principal](../basic-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Realizar autenticação no aplicativo secundário com mvpd pré-selecionado](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Executar autenticação no aplicativo secundário sem mvpd pré-selecionado](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* O aplicativo de streaming deve iniciar o fluxo de logout quando precisar fazer logoff do MVPD.

>[!IMPORTANT]
>
> Suposições
>
> <br/>
> 
> * O MVPD oferece suporte ao fluxo de logout e tem um ponto de extremidade de logout.

### Fluxo de trabalho (WRK) {#workflow-initiate-logout-for-specific-mvpd-with-logout-endpoint}

Siga as etapas fornecidas para implementar o fluxo de logout básico para um MVPD específico com um endpoint de logout executado em um aplicativo primário, conforme mostrado no diagrama a seguir.

![Iniciar logout para mvpd específico com ponto de extremidade de logout](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-initiate-logout-within-primary-application-for-specific-mvpd-with-logout-endpoint.png)

*Iniciar logout para mvpd específico com ponto de extremidade de logout*

1. **Iniciar logout do Adobe Pass:** o aplicativo de streaming reúne todos os dados necessários para iniciar o fluxo de logout chamando o ponto de extremidade de Logout do Adobe Pass.

   Consulte a documentação da API [Iniciar logout para mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) específica para obter detalhes sobre:
   * Todos os parâmetros _necessários_, como `serviceProvider`, `mvpd` e `redirectUrl`
   * Todos os cabeçalhos _necessários_, como `Authorization`, `AP-Device-Identifier`
   * Todos os _parâmetros e cabeçalhos_ opcionais

1. **Localizar perfil regular:** O servidor Adobe Pass identifica um perfil válido com base nos parâmetros e cabeçalhos recebidos.

1. **Excluir perfil regular:** o servidor do Adobe Pass exclui o perfil regular identificado do back-end do Adobe Pass.

1. **Indique a próxima ação:** A resposta do ponto de extremidade de logout do Adobe Pass contém os dados necessários para orientar o aplicativo de streaming em relação à próxima ação:
   * O atributo `url` está presente, pois o MVPD dá suporte ao fluxo de logout.
   * O atributo `actionName` está definido como &quot;logout&quot;.
   * O atributo `actionType` está definido como &quot;interativo&quot;.

   Consulte a documentação da API [Iniciar logout para mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) específica para obter detalhes sobre as informações fornecidas em uma resposta de logout.

   >[!IMPORTANT]
   >
   > O endpoint de logout do Adobe Pass valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve estar ativa.
   >
   > <br/>
   > 
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../enhanced-error-codes.md).

1. **Iniciar logout do MVPD:** O aplicativo de streaming lê o `url` e usa um agente do usuário para iniciar o fluxo de logout com o MVPD. O fluxo pode incluir vários redirecionamentos para sistemas MVPD. Ainda assim, o resultado é que o MVPD executa sua limpeza interna e envia a confirmação de logout final de volta para o back-end do Adobe Pass.

1. **Indicar logout concluído:** O aplicativo de streaming pode esperar que o agente do usuário alcance o `redirectUrl` fornecido e pode usá-lo como um sinal para exibir, opcionalmente, uma mensagem específica na interface do usuário.

## Iniciar logout para mvpd específico sem ponto de extremidade de logout {#initiate-logout-for-specific-mvpd-without-logout-endpoint}

### Pré-requisitos {#prerequisites-initiate-logout-for-specific-mvpd-without-logout-endpoint}

Antes de iniciar o logout de um MVPD específico sem um endpoint de logout, verifique se os seguintes pré-requisitos foram atendidos:

* O aplicativo de streaming deve ter um perfil regular válido, criado com êxito para o MVPD usando um dos fluxos de autenticação básicos:
   * [Executar autenticação no aplicativo principal](../basic-flows/rest-api-v2-basic-authentication-primary-application-flow.md)
   * [Realizar autenticação no aplicativo secundário com mvpd pré-selecionado](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
   * [Executar autenticação no aplicativo secundário sem mvpd pré-selecionado](../basic-flows/rest-api-v2-basic-authentication-secondary-application-flow.md)
* O aplicativo de streaming deve iniciar o fluxo de logout quando precisar fazer logoff do MVPD.

>[!IMPORTANT]
>
> Suposições
>
> <br/>
> 
> * O MVPD não oferece suporte ao fluxo de logout e não tem um ponto de extremidade de logout.

### Fluxo de trabalho (WRK) {#workflow-initiate-logout-for-specific-mvpd-without-logout-endpoint}

Siga as etapas fornecidas para implementar o fluxo de logout básico para um MVPD específico sem um endpoint de logout executado em um aplicativo primário, conforme mostrado no diagrama a seguir.

![Iniciar logout para mvpd específico sem ponto de extremidade de logout](../../../assets/rest-api-v2/flows/basic-flows/rest-api-v2-initiate-logout-within-primary-application-for-specific-mvpd-without-logout-endpoint.png)

*Iniciar logout para mvpd específico sem ponto de extremidade de logout*

1. **Iniciar logout do Adobe Pass:** o aplicativo de streaming reúne todos os dados necessários para iniciar o fluxo de logout chamando o ponto de extremidade de Logout do Adobe Pass.

   Consulte a documentação da API [Iniciar logout para mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) específica para obter detalhes sobre:
   * Todos os parâmetros _necessários_, como `serviceProvider`, `mvpd` e `redirectUrl`
   * Todos os cabeçalhos _necessários_, como `Authorization`, `AP-Device-Identifier`
   * Todos os _parâmetros e cabeçalhos_ opcionais

1. **Localizar perfil regular:** O servidor Adobe Pass identifica um perfil válido com base nos parâmetros e cabeçalhos recebidos.

1. **Excluir perfil regular:** o servidor do Adobe Pass exclui o perfil regular identificado.

1. **Indique a próxima ação:** A resposta do ponto de extremidade de logout do Adobe Pass contém os dados necessários para orientar o aplicativo de streaming em relação à próxima ação:
   * O atributo `url` está ausente, pois o MVPD não oferece suporte ao fluxo de logout.
   * O atributo `actionName` está definido como &quot;concluído&quot;.
   * O atributo `actionType` está definido como &quot;nenhum&quot;.

   Consulte a documentação da API [Iniciar logout para mvpd](../../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) específica para obter detalhes sobre as informações fornecidas em uma resposta de logout.

   >[!IMPORTANT]
   >
   > O endpoint de logout do Adobe Pass valida os dados da solicitação para garantir que as condições básicas sejam atendidas:
   >
   > * Os parâmetros e cabeçalhos _requeridos_ devem ser válidos.
   > * A integração entre o `serviceProvider` e o `mvpd` fornecidos deve estar ativa.
   >
   > <br/>
   > 
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../enhanced-error-codes.md).

1. **Indicar logout concluído:** o aplicativo de streaming processa a resposta e pode usá-la para exibir uma mensagem específica na interface do usuário.
