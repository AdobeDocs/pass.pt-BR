---
title: Autenticação Básica - Aplicativo Principal - Fluxo
description: REST API V2 - Autenticação básica - Aplicativo principal - Fluxo
source-git-commit: dc9fab27c7eced2be5dd9f364ab8f2d64f8e4177
workflow-type: tm+mt
source-wordcount: '894'
ht-degree: 0%

---


# Fluxo de autenticação básica executado no aplicativo principal {#basic-authentication-flow-performed-within-primary-application}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> A implementação da REST API V2 é limitada pela documentação do [Mecanismo de limitação](/help/authentication/throttling-mechanism.md).

O **Fluxo de autenticação** no direito de Autenticação Adobe Pass permite que o aplicativo de streaming verifique se um usuário tem uma conta MVPD válida. Esse processo exige que o usuário tenha uma conta MVPD ativa e insira credenciais de logon válidas na página de logon do MVPD.

O fluxo de autenticação é necessário nos seguintes casos:

* Quando o usuário abre um aplicativo pela primeira vez.
* Quando a autenticação anterior do usuário expirar.
* Quando o usuário faz logoff da conta MVPD.
* Quando o usuário deseja autenticar com um MVPD diferente.

Em todos esses casos, o aplicativo que chama qualquer um dos endpoints de Perfis recebe uma resposta vazia para um ou mais perfis, mas para MVPDs diferentes.

O **Fluxo de autenticação** requer que um agente do usuário (navegador) conclua uma série de chamadas do aplicativo para o back-end do Adobe Pass, depois para a página de logon MVPD e, por fim, de volta para o aplicativo. Esse fluxo pode incluir vários redirecionamentos para sistemas MVPD e o gerenciamento de cookies ou sessões armazenados para cada domínio, o que pode ser desafiador de alcançar e proteger sem um agente do usuário.

Com base nos recursos do aplicativo principal (aplicativo de transmissão) para oferecer suporte à interação do usuário para selecionar um MVPD e autenticar com o MVPD selecionado em um agente do usuário, os cenários de autenticação são:

* [Executar autenticação no aplicativo principal](./rest-api-v2-basic-authentication-primary-application-flow.md)
* [Realizar autenticação no aplicativo secundário com mvpd pré-selecionado](./rest-api-v2-basic-authentication-secondary-application-flow.md)
* [Executar autenticação no aplicativo secundário sem mvpd pré-selecionado](./rest-api-v2-basic-authentication-secondary-application-flow.md)

## Executar autenticação no aplicativo principal {#perform-authentication-within-primary-application}

### Pré-requisitos {#prerequisites-perform-authentication-within-primary-application}

Antes de executar a autenticação por meio da interação do usuário em um aplicativo principal, verifique se os seguintes pré-requisitos estão sendo atendidos:

* O aplicativo de streaming deve selecionar um MVPD.
* O aplicativo de streaming deve iniciar uma sessão de autenticação para entrar com o MVPD selecionado.
* O aplicativo de streaming deve ser autenticado com o MVPD selecionado em um agente do usuário.

>[!IMPORTANT]
>
> Suposições
>
> <br/>
> 
> * O aplicativo de streaming é compatível com a interação do usuário para selecionar um MVPD.
> * O aplicativo de streaming suporta a interação do usuário para autenticar com o MVPD selecionado em um agente do usuário.

### Fluxo de trabalho (WRK) {#workflow-perform-authentication-completed-on-primary-application}

Siga as etapas fornecidas para implementar o fluxo de autenticação básico executado em um aplicativo primário, conforme mostrado no diagrama a seguir.

![Executar autenticação no aplicativo principal](../../../assets/rest-api-v2/flows/basic-access-flows/rest-api-v2-perform-authentication-within-primary-application.png)

*Executar autenticação no aplicativo principal*

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
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../enhanced-error-codes.md).

1. **Continuar com fluxos de decisões:** A resposta do ponto de extremidade Sessões contém os seguintes dados:
   * O atributo `actionName` está definido como &quot;autorize&quot;.
   * O atributo `actionType` está definido como &quot;direto&quot;.

   Se o back-end do Adobe Pass identificar um perfil válido, o aplicativo de transmissão não precisará reautenticar com o MVPD selecionado, pois já existe um perfil que pode ser usado para fluxos de decisões subsequentes.

1. **Abrir URL no agente do usuário:** A resposta do ponto de extremidade Sessões contém os seguintes dados:
   * O `url` que pode ser usado para iniciar a autenticação interativa na página de logon MVPD.
   * O atributo `actionName` está definido como &quot;autenticar&quot;.
   * O atributo `actionType` está definido como &quot;interativo&quot;.

   Se o back-end do Adobe Pass não identificar um perfil válido, o aplicativo de streaming abrirá um agente do usuário para carregar o `url` fornecido, fazendo uma solicitação ao endpoint de Autenticação. Esse fluxo pode incluir vários redirecionamentos, levando o usuário à página de logon do MVPD e fornecendo credenciais válidas.

1. **Autenticação MVPD concluída:** Se o fluxo de autenticação for bem-sucedido, a interação do agente do usuário salvará um perfil regular no back-end do Adobe Pass e atingirá o `redirectUrl` fornecido.

1. **Recuperar perfil para código específico:** O aplicativo de streaming reúne todos os dados necessários para recuperar informações de perfil, enviando uma solicitação ao ponto de extremidade de Perfis.

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
   > Se a validação falhar, uma resposta de erro será gerada, fornecendo informações adicionais que seguem a documentação de [Códigos de erro aprimorados](../../../enhanced-error-codes.md).
