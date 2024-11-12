---
title: Cookbook REST API V2 (cliente para servidor)
description: Cookbook REST API V2 (cliente para servidor)
exl-id: 6a5a89d2-ea54-4f9c-9505-e575ced4301c
source-git-commit: 563e0b17de3be9290a661242355b4835b8c386e1
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 0%

---

# Cookbook REST API V2 (cliente para servidor) {#rest-api-v2-cookbook-clientserver}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> A implementação da REST API V2 é limitada pela documentação do [Mecanismo de limitação](/help/authentication/throttling-mechanism.md).

## Etapas para implementar a REST API V2 em aplicativos do lado do cliente {#steps-to-implement-the-rest-api-v2-in-client-side-applications}

Para implementar a API REST V2 do Adobe Pass, é necessário seguir as etapas abaixo, agrupadas em fases.

## A. Fase de registro {#registration-phase}

### Etapa 1: Registrar seu aplicativo {#step-1-register-your-application}

Para que o aplicativo possa chamar a API REST V2 do Adobe Pass, ele precisa de um token de acesso exigido pela camada de segurança da API.

Para obter o token de acesso, o aplicativo precisa seguir as etapas descritas na documentação do [Registro de Cliente Dinâmico](../../dcr-api/apis/dynamic-client-registration-apis-retrieve-access-token.md).

## B. Fase de autenticação {#authentication-phase}

### Etapa 2: verificar perfis autenticados existentes {#step-2-check-for-existing-authenticated-profiles}

O aplicativo de streaming verifica os perfis autenticados existentes: <b>/api/v2/{serviceProvider}/profiles</b><br>
([Recuperar perfis autenticados](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profiles.md))

* Se nenhum perfil for encontrado e o aplicativo de Streaming implementar um fluxo TempPass
   * Siga a documentação sobre como implementar [Fluxos de acesso temporário](../flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md)
* Se nenhum perfil for encontrado e o aplicativo de Streaming implementar um Fluxo de autenticação
   * O aplicativo de streaming recupera a lista de MVPDs disponíveis para serviceProvider: <b>/api/v2/{serviceProvider}/configuration</b><br>
([Recuperar lista de MVPDs](../apis/configuration-apis/rest-api-v2-configuration-apis-retrieve-configuration-for-specific-service-provider.md) disponíveis)
   * O aplicativo de transmissão pode implementar a filtragem na lista de MVPDs e exibir apenas MVPDs destinados ao ocultar outros (TempPass, MVPDs de teste, MVPDs em desenvolvimento etc.)
   * O aplicativo de transmissão exibe o seletor, o usuário seleciona o MVPD
   * O aplicativo de streaming cria uma sessão: <b>/api/v2/{serviceProvider}/sessions</b><br>
([Criar sessão de autenticação](../apis/sessions-apis/rest-api-v2-sessions-apis-create-authentication-session.md))<br>
      * um CÓDIGO e um URL a serem usados para autenticação são retornados
      * se um perfil for encontrado, o aplicativo de Streaming poderá prosseguir para <a href="#preauthorization-phase">C. Fase de pré-autorização </a> ou <a href="#authorization-phase">D. Fase de autorização</a>

### Etapa 3: Autenticar o usuário {#step-3-authenticate-the-user}

Usando um navegador ou um aplicativo baseado na Web de segunda tela:

* Opção 1. O aplicativo de streaming pode abrir um navegador ou visualização da Web, carregar o URL para autenticar e o usuário acessar a página de logon do MVPD, onde as credenciais precisam ser enviadas
   * O usuário insere login/senha, o redirecionamento final mostra uma página de sucesso
* Opção 2. O Aplicativo de Streaming não pode abrir um navegador e apenas exibir o CÓDIGO. <b>É necessário desenvolver um aplicativo Web separado</b> para solicitar que o usuário insira CÓDIGO, compile e abra a URL: <b>/api/v2/authenticate/{serviceProvider}/{CODE}</b>
   * O usuário insere login/senha, o redirecionamento final mostra uma página de sucesso

### Etapa 4: verificar perfis autenticados {#step-4-check-for-authenticated-profiles}

O aplicativo de streaming verifica a autenticação com MVPD para concluir no Navegador ou na Segunda Tela

* A sondagem a cada 15 segundos é recomendada em <b>/api/v2/{serviceProvider}/profiles/{mvpd}</b><br>
([Recuperar perfis autenticados para MVPD específico](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-mvpd.md))
   * Se a seleção de MVPD não for feita no aplicativo de Streaming como o seletor de MVPD é apresentado no aplicativo Segunda Tela, a sondagem deve ocorrer com CODE <b>/api/v2/{serviceProvider}/profiles/code/{CODE}</b><br>
([Recuperar perfis autenticados para CÓDIGO específico](../apis/profiles-apis/rest-api-v2-profiles-apis-retrieve-profile-for-specific-code.md))
* A sondagem não deve exceder 30 minutos, caso 30 minutos sejam atingidos e o aplicativo de transmissão ainda esteja ativo, uma nova sessão precisa ser iniciada e um novo CÓDIGO e URL serão retornados
* Quando a autenticação estiver concluída, o retorno será de 200 com perfil autenticado
* O aplicativo de Streaming pode prosseguir para <a href="#preauthorization-phase">C. Fase de pré-autorização </a> ou <a href="#authorization-phase">D. Fase de autorização</a>

## C. Fase de pré-autorização {#preauthorization-phase}

### Etapa 5: verificar recursos pré-autorizados {#step-5-check-for-preauthorized-resources}

O aplicativo de streaming se prepara para exibir os vídeos disponíveis para o usuário autenticado e tem a possibilidade de verificar o
acesso a esses recursos.

* A etapa é opcional e executada se o aplicativo quiser filtrar os recursos não disponíveis no pacote de usuário autenticado
* Chamada para <b>/api/v2/{serviceProvider}/decision/preauthorize/{mvpd}</b><br>
([Recuperar decisão de pré-autorização usando MVPD](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md) específico)

## D. Fase de autorização {#authorization-phase}

### Etapa 6: verificar recursos autorizados {#step-6-check-for-authorized-resources}

O aplicativo de streaming se prepara para reproduzir um vídeo/ativo/recurso selecionado pelo usuário.

* Etapa necessária para cada início de reprodução
* Chamar <b>/api/v2/{serviceProvider}/decision/authorize/{mvpd}</b><br>
([Recuperar decisão de autorização usando MVPD](../apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md) específico)
   * decision = &#39;Permit&#39;, Dispositivo de transmissão inicia a transmissão
   * decisão = &#39;Negar&#39;, o dispositivo de transmissão informa ao usuário que ele não tem acesso a esse vídeo

## E. Fase de saída {#logout-phase}

### Etapa 7: Logout {#step-7-logout}

Dispositivo de transmissão: o usuário deseja sair do MVPD

* Chamar <b>/api/v2/{serviceProvider}/logout/{mvpd}</b><br>
([Iniciar logout para MVPD](../apis/logout-apis/rest-api-v2-logout-apis-initiate-logout-for-specific-mvpd.md) específico)
* Se a resposta actionType=&#39;interative&#39; e o url estiver presente, abra o url em um Navegador/Segunda Tela para concluir o logout com o MVPD
