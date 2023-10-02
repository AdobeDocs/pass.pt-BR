---
title: Referência da API do iOS/tvOS
description: Referência da API do iOS/tvOS
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: 854698397d9d14c1bfddcc10eecc61c7e3c32b71
workflow-type: tm+mt
source-wordcount: '7018'
ht-degree: 0%

---

# Referência de API do SDK do iOS/tvOS {#iostvos-sdk-api-reference}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#intro}

Esta página descreve os métodos e as funções de retorno de chamada expostos pelo cliente nativo do iOS/tvOS para autenticação da Adobe Pass. Os métodos e as funções de retorno de chamada descritos aqui são definidos na `AccessEnabler.h` e `EntitlementDelegate.h` arquivos de cabeçalho; você pode encontrá-los no SDK do iOS AccessEnabler aqui: `[SDK directory]/AccessEnabler/headers/api/`


Documentação associada:

* Para obter uma descrição do fluxo de direitos básicos de Autenticação do Adobe Pass, consulte [Fluxo de direitos](/help/authentication/entitlement-flow.md).
* Para obter uma apresentação passo a passo de como implementar o fluxo de direitos de autenticação da Adobe Pass usando essa API, consulte [Guia de integração do iOS](/help/authentication/iostvos-sdk-cookbook.md).
* Para obter o SDK do iOS AccessEnabler mais recente, consulte [Biblioteca do ativador de acesso nativo do iOS](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

>[!NOTE]
>
>O Adobe incentiva você a usar somente a autenticação Adobe Pass *público* APIs:
>
>* As APIs públicas estão disponíveis e totalmente testadas em todos os tipos de clientes compatíveis. Para qualquer recurso público, garantimos que cada tipo de cliente tenha uma versão correspondente dos métodos associados.
>* As APIs públicas devem ser o mais estáveis possível, para oferecer compatibilidade com versões anteriores e garantir que as integrações de parceiros não sejam interrompidas. No entanto, para APIs não públicas, reservamos o direito de alterar sua assinatura em qualquer ponto futuro. Se você encontrar um fluxo específico que não possa ser suportado por meio de uma combinação das chamadas de API de autenticação do Adobe Pass públicas atuais, a melhor abordagem é informar o. Levando em consideração suas necessidades, podemos modificar as APIs públicas e fornecer uma solução estável a partir de agora.

</br>

## Referência da API {#apis}

* [init](#initWithSoftwareStatement):softwareStatement - Instancia o objeto AccessEnabler.

* **[OBSOLETO]** [init](#init) - Instancia o objeto AccessEnabler.

* [setOptions:options:](#setOptions) - Configura as opções globais do SDK, como profile ou visitorID.

* [setRequestor:](#setReqV3)[requestorID](#setReqV3),[setRequestor:requestorID:serviceProviders:](#setReqV3) - Estabelece a identidade do Programador.

* **[OBSOLETO]** [setRequestor:signedRequestorId:](#setReq),[setRequestor:signedRequestorId:serviceProviders:](#setReq) - Estabelece a identidade do Programador.

* **[OBSOLETO]** [setRequestor:signedRequestorId:segredo:publicKey](#setReq_tvos), [setRequestor:signedRequestorId:serviceProviders:secret:publicKey](#setReq_tvos)- Estabelece a identidade do Programador.

* [setRequestorComplete:](#setReqComplete) - Informa ao aplicativo que a fase de configuração foi concluída.

* [checkAuthentication](#checkAuthN) - Verifica o status de autenticação do usuário atual.

* [getAuthentication](#getAuthN), [getAuthentication:withData:](#getAuthN) - Inicia o fluxo de trabalho de autenticação completa.

* [getAuthentication:filtro](#getAuthN_filter),[getAuthentication:withData:](#getAuthN)[andFilter](#getAuthN_filter) - Inicia o fluxo de trabalho de autenticação completa.

* [displayProviderDialog:](#dispProvDialog) - Informa o aplicativo a instanciar os elementos apropriados da interface do usuário para que o usuário selecione um MVPD.

* [setSelectedProvider:](#setSelProv) - Informa o AccessEnabler sobre a seleção de MVPD do usuário.

* [navigateToUrl:](#nav2url) - Informa ao aplicativo que o usuário precisa ser apresentado à página de logon do MVPD.

* [navigateToUrl:useSVC:](#nav2urlSVC) - Informa ao aplicativo que o usuário precisa ser apresentado à página de logon do MVPD, usando SFSafariViewController

* [handleExternalURL:url](#handleExternalURL) - Conclui o fluxo de autenticação/logout.

* **[OBSOLETO]** [getAuthenticationToken](#getAuthNToken) - Solicita o token de autenticação do servidor back-end.

* [setAuthenticationStatus:errorCode:](#setAuthNStatus) - Informa ao aplicativo o status do fluxo de autenticação.

* [checkPreauthorizedResources:](#checkPreauth) - Determina se o usuário já está autorizado a exibir recursos protegidos específicos.

* [checkPreauthorizedResources:cache:](#checkPreauthCache) - Determina se o usuário já está autorizado a exibir recursos protegidos específicos.

* [recursos pré-autorizados:](#preauthResources) - Fornece uma lista de recursos que o usuário já está autorizado a visualizar.

* [checkAuthorization:](#checkAuthZ), [checkAuthorization:withData:](#checkAuthZ) - Verifica o status de autorização do usuário atual.

* [getAuthorization:](#getAuthZ), [getAuthorization:withData:](#getAuthZ) - Inicia o fluxo de autorização.

* [setToken:forResource:](#setToken) - Informa ao aplicativo que o fluxo de autorização foi concluído com êxito.

* [tokenRequestFailed:errorCode:errorDescription:](#tokenReqFailed) - Informa ao aplicativo que houve falha no fluxo de autorização.

* [logout](#logout) - Inicia o fluxo de logout.

* [getSelectedProvider](#getSelProv) - Determina o provedor selecionado no momento.

* [seletedProvider:](#selProv) - Fornece informações sobre o MVPD selecionado no momento para o seu aplicativo.

* [getMetadata:](#getMeta) - Recupera informações expostas como metadados pela biblioteca AccessEnabler.

* [presentTvProviderDialog:](#presentTvDialog) - Informa seu aplicativo para exibir a caixa de diálogo do SSO do Apple.

* [dismissTvProviderDialog:](#dismissTvDialog) - Informa seu aplicativo para ocultar a caixa de diálogo do SSO do Apple.

* [setMetadataStatus:encrypted:forKey:andArguments:](#setMetaStatus) - Fornece os metadados solicitados por um [getMetadata:](#getMeta) chame.

* [sendTrackingData:forEventType:](#sendTracking) - Fornece informações de dados de rastreamento.

* [MVPD](#mvpd) - A classe MVPD. [Contém informações sobre o MVPD]

### init:softwareStatement {#initWithSoftwareStatement}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Instancia o objeto AccessEnabler. Deve haver uma única instância AccessEnabler por instância do aplicativo.

| **Chamada de API: construtor AccessEnabler do iOS** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**Disponibilidade:** v3.0+

**Parâmetros:**

* **softwareDeclaração:** Uma string que identifica o aplicativo no sistema Adobe. Veja como obter uma Declaração de Software.

[Voltar ao início...](#apis)



### init - [OBSOLETO]{#init}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Instancia o objeto AccessEnabler. Deve haver uma única instância AccessEnabler por instância do aplicativo.

| Chamada de API: construtor AccessEnabler do iOS |
| --- |
| ```- (id) init;``` |

**Disponibilidade:** v1.0+ **Até:** v3.0

**Parâmetros:** Nenhum

[Voltar ao início...](#apis)

</br>

### setOptions:opções {#setOptions}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Configura as opções globais do SDK. Ele aceita um NSDictionary como argumento. Os valores do dicionário serão passados para o servidor junto com cada chamada de rede feita pelo SDK.

**Nota:** Os valores serão passados ao servidor independentemente do fluxo atual (autenticação/autorização). Se quiser alterar os valores, você pode chamar esse método a qualquer momento.

| Chamada de API: setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**Disponibilidade:** v2.3.0+

**Parâmetros:**

* *opções*: um NSDictionary contendo opções globais do SDK. Atualmente, as seguintes opções estão disponíveis:
   * **applicationProfile** - Ele pode ser usado para fazer configurações de servidor com base nesse valor.
   * **visitorID** - O Serviço de ID de Experience Cloud. Esse valor pode ser usado posteriormente para relatórios de análise avançada.
   * **handleSVC** - Booleano que indica se o programador manipulará SFSafariViewControllers. Consulte [Suporte a SFSafariViewController no iOS SDK 3.2+](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) para obter mais detalhes.
      * Se definida como **false,** o SDK apresentará automaticamente ao usuário final um SFSafariViewController. O SDK navegará ainda mais para o URL da página de logon dos MVPDs.
      * Se definida como **true,** o SDK **NOT** apresentar automaticamente ao usuário final um SFSafariViewController. O SDK acionará **navigate(toUrl:{url}, useSVC:YES)**.
* **device\_info** - Informações do cliente, conforme descrito em [Transmissão de informações do cliente](/help/authentication/passing-client-information-device-connection-and-application.md).

[Voltar ao início...](#apis)


### setRequestor:requestorID, setRequestor:requestorID:serviceProviders: {#setReqV3}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Estabelece a identidade do Programador. Cada programador recebe uma ID exclusiva ao se registrar no Adobe para o sistema de autenticação da Adobe Pass. Ao lidar com SSO e tokens remotos, o estado de autenticação pode mudar quando o aplicativo estiver em segundo plano, setRequestor pode ser chamado novamente quando o aplicativo for trazido para o primeiro plano para sincronizar com o estado do sistema (busque um token remoto se o SSO estiver ativado ou exclua o token local se um logout tiver ocorrido enquanto isso).

A resposta do servidor contém uma lista de MVPDs juntamente com algumas informações de configuração anexadas à identidade do Programador. A resposta do servidor é usada internamente pelo código AccessEnabler. Somente o status da operação (ou seja, SUCESSO/FALHA) é apresentado ao seu aplicativo por meio do `setRequestorComplete:` retorno de chamada.

Se a variável `urls` não for usado, a chamada de rede resultante será direcionada ao URL do provedor de serviços padrão: o ambiente RELEASE/produção do Adobe.


Se um valor for fornecido para a variável `urls` parâmetro, a chamada de rede resultante será direcionada a todos os URLs fornecidos na variável `urls` parâmetro. Todas as solicitações de configuração são acionadas simultaneamente em threads separados. O primeiro respondente tem prioridade ao compilar a lista de MVPDs. Para cada MVPD na lista, o AccessEnabler lembra o URL do provedor de serviços associado. Todas as solicitações de direito subsequentes são direcionadas ao URL associado ao provedor de serviços que foi emparelhado com o MVPD de destino durante a fase de configuração.

| Chamada de API: configuração do solicitante |
| --- |
| ```- (void) setRequestor:(NSString *)requestorID``` |


**Disponibilidade:** v3.0+

| Chamada de API: configuração do solicitante |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**Disponibilidade:** v3.0+

**Parâmetros:**

* *requestorID*: o identificador exclusivo associado ao programador. Transmita o identificador exclusivo atribuído pelo Adobe ao seu site quando você se registra pela primeira vez no serviço de autenticação da Adobe Pass.
* *urls*: parâmetro opcional; por padrão, o provedor de serviços da Adobe é usado (http://sp.auth.adobe.com/). Essa matriz permite especificar endpoints para serviços de autenticação e autorização fornecidos pelo Adobe (instâncias diferentes podem ser usadas para fins de depuração). Você pode usar essa opção para especificar várias instâncias do provedor de serviços de Autenticação da Adobe Pass. Ao fazer isso, a lista MVPD é composta pelos endpoints de todos os provedores de serviços. Cada MVPD está associado ao provedor de serviços mais rápido; ou seja, o provedor que respondeu primeiro e que oferece suporte a esse MVPD.

>[!NOTE]
>
>Se chamado sem o parâmetro `serviceProviders` , a biblioteca recuperará a configuração do provedor de serviços padrão (ou seja, `https://sp.auth.adobe.com` para o perfil de produção ou `https://sp.auth-staging.adobe.com` para o perfil de preparo). Se a variável `serviceProviders` for fornecido, ele deverá ser uma matriz de URLs. As informações de configuração são recuperadas de todos os endpoints especificados e são mescladas. Se houver informações duplicadas em diferentes respostas do provedor de serviços, o conflito será resolvido em favor do servidor com resposta mais rápida (ou seja, o servidor com o menor tempo de resposta tem prioridade).

**Retornos de chamada disparados:** [`setRequestorComplete:`](#setReqComplete)

[Voltar ao início...](#apis)

</br>

### setRequestor:setSignedRequestorId:, setRequestor:setSignedRequestorId:serviceProviders: - [OBSOLETO] {#setReq}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Estabelece a identidade do Programador. Cada programador recebe uma ID exclusiva ao se registrar no Adobe para o sistema de autenticação da Adobe Pass. Ao lidar com SSO e tokens remotos, o estado de autenticação pode mudar quando o aplicativo estiver em segundo plano, setRequestor pode ser chamado novamente quando o aplicativo for colocado em primeiro plano para sincronizar com o estado do sistema (busque um token remoto se o SSO estiver habilitado ou exclua o token local se um logout tiver ocorrido enquanto isso).

A resposta do servidor contém uma lista de MVPDs juntamente com algumas informações de configuração anexadas à identidade do Programador. A resposta do servidor é usada internamente pelo código AccessEnabler. Somente o status da operação (ou seja, SUCESSO/FALHA) é apresentado ao seu aplicativo por meio do `setRequestorComplete:` retorno de chamada.

Se a variável `urls` não for usado, a chamada de rede resultante será direcionada ao URL do provedor de serviços padrão: o ambiente RELEASE/produção do Adobe.

Se um valor for fornecido para a variável `urls` parâmetro, a chamada de rede resultante será direcionada a todos os URLs fornecidos na variável `urls` parâmetro. Todas as solicitações de configuração são acionadas simultaneamente em threads separados. O primeiro respondente tem prioridade ao compilar a lista de MVPDs. Para cada MVPD na lista, o AccessEnabler lembra o URL do provedor de serviços associado. Todas as solicitações de direito subsequentes são direcionadas ao URL associado ao provedor de serviços que foi emparelhado com o MVPD de destino durante a fase de configuração.

| Chamada de API: configuração do solicitante |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**Disponibilidade:** v1.0+ **Até:** v3.0

| Chamada de API: configuração do solicitante |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**Disponibilidade:** v1.0+ **Até:** v3.0

**Parâmetros:**

* *requestorID*: o identificador exclusivo associado ao programador. Transmita o identificador exclusivo atribuído pelo Adobe ao seu site quando você se registrou pela primeira vez no serviço de autenticação da Adobe Pass.
* *signedRequestorID*: **Esse parâmetro existe no iOS AccessEnabler versões 1.2 e posteriores.** Uma cópia da ID do solicitante que é assinada digitalmente com sua chave privada. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls*: parâmetro opcional; por padrão, o provedor de serviços da Adobe é usado (http://sp.auth.adobe.com/). Essa matriz permite especificar endpoints para serviços de autenticação e autorização fornecidos pelo Adobe (instâncias diferentes podem ser usadas para fins de depuração). Você pode usar essa opção para especificar várias instâncias do provedor de serviços de Autenticação da Adobe Pass. Ao fazer isso, a lista MVPD é composta pelos endpoints de todos os provedores de serviços. Cada MVPD está associado ao provedor de serviços mais rápido; ou seja, o provedor que respondeu primeiro e que oferece suporte a esse MVPD.

**Notas:** Se chamado sem o parâmetro `serviceProviders` , a biblioteca recuperará a configuração do provedor de serviços padrão (ou seja,`https://sp.auth.adobe.com` para o perfil de produção ou `https://sp.auth-staging.adobe.com` para o perfil de preparo). Se a variável `serviceProviders` for fornecido, ele deverá ser uma matriz de URLs. As informações de configuração são recuperadas de todos os endpoints especificados e são mescladas. Se houver informações duplicadas em diferentes respostas do provedor de serviços, o conflito será resolvido em favor do servidor com resposta mais rápida (ou seja, o servidor com o menor tempo de resposta tem prioridade).

**Retornos de chamada disparados:** [`setRequestorComplete:`](#setReqComplete)


[Voltar ao início...](#apis)

### setRequestor:setSignedRequestorId:secret:publicKey, setRequestor:setSignedRequestorId:serviceProviders:secret:publicKey - [OBSOLETO] {#setReq_tvos}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Estabelece a identidade do Programador. Cada programador recebe uma ID exclusiva ao se registrar no Adobe para o sistema de autenticação da Adobe Pass. Esta configuração deve ser executada somente uma vez durante o ciclo de vida do aplicativo.

A resposta do servidor contém uma lista de MVPDs juntamente com algumas informações de configuração anexadas à identidade do Programador. A resposta do servidor é usada internamente pelo código AccessEnabler. Somente o status da operação (ou seja, SUCESSO/FALHA) é apresentado ao seu aplicativo por meio do `setRequestorComplete:` retorno de chamada.

Se a variável `urls` não for usado, a chamada de rede resultante será direcionada ao URL do provedor de serviços padrão: o ambiente RELEASE/produção do Adobe.

Se um valor for fornecido para a variável `urls` parâmetro, a chamada de rede resultante será direcionada a todos os URLs fornecidos na variável `urls` parâmetro. Todas as solicitações de configuração são acionadas simultaneamente em threads separados. O primeiro respondente tem prioridade ao compilar a lista de MVPDs. Para cada MVPD na lista, o AccessEnabler lembra o URL do provedor de serviços associado. Todas as solicitações de direito subsequentes são direcionadas ao URL associado ao provedor de serviços que foi emparelhado com o MVPD de destino durante a fase de configuração.



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: configuração do solicitante</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID
               secret:(NSString *)secret
            publicKey:(NSString *)publicKey;</code></pre></td>
</tr>
</tbody>
</table>


**Disponibilidade:** v2.0+ **Até:** v3.0

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: configuração do solicitante</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestor:(NSString *)requestorID 
    signedRequestorID:(NSString *)signedRequestorID 
     serviceProviders:(NSArray *)urls</code></pre>
<p><code class="sourceCode objectivec">               secret:(NSString *)secret</code></p>
<p><code class="sourceCode objectivec">            publicKey:(NSString *)publicKey;</code></p></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v2.0+ **Até:** v3.0

**Parâmetros:**

* *requestorID*: o identificador exclusivo associado ao programador. Transmita o identificador exclusivo atribuído pelo Adobe ao seu site quando você se registrou pela primeira vez no serviço de autenticação da Adobe Pass.
* *signedRequestorID*: **Esse parâmetro existe no iOS AccessEnabler versões 1.2 e posteriores.** Uma cópia da ID do solicitante que é assinada digitalmente com sua chave privada. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls*: parâmetro opcional; por padrão, o provedor de serviços da Adobe é usado (http://sp.auth.adobe.com/). Essa matriz permite especificar endpoints para serviços de autenticação e autorização fornecidos pelo Adobe (instâncias diferentes podem ser usadas para fins de depuração). Você pode usar essa opção para especificar várias instâncias do provedor de serviços de Autenticação da Adobe Pass. Ao fazer isso, a lista MVPD é composta pelos endpoints de todos os provedores de serviços. Cada MVPD está associado ao provedor de serviços mais rápido; ou seja, o provedor que respondeu primeiro e que oferece suporte a esse MVPD.
* secret e publicKey: as chaves secreta e pública usadas para assinar as chamadas da segunda tela. Para obter mais informações, consulte a [Documentação sem cliente](#create_dev).

Se chamado sem o parâmetro `serviceProviders` , a biblioteca recuperará a configuração do provedor de serviços padrão (ou seja, `https://sp.auth.adobe.com` para o perfil de produção ou https://sp.auth-staging.adobe.com para o perfil de preparo). Se a variável `serviceProviders` for fornecido, ele deverá ser uma matriz de URLs. As informações de configuração são recuperadas de todos os endpoints especificados e são mescladas. Se houver informações duplicadas em diferentes respostas do provedor de serviços, o conflito será resolvido em favor do servidor com resposta mais rápida (ou seja, o servidor com o menor tempo de resposta tem prioridade).

**Retornos de chamada disparados:** [`setRequestorComplete:`](#setReqComplete)

[Voltar ao início...](#apis)

</br>

### setRequestorComplete: {#setReqComplete}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição** Retorno de chamada acionado pelo AccessEnabler que informa ao aplicativo que a fase de configuração foi concluída. Esse é um sinal de que o aplicativo pode começar a emitir solicitações de direito. Nenhuma solicitação de direito pode ser emitida pelo aplicativo até que a fase de configuração seja concluída.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Retorno de chamada: configuração do solicitante concluída</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setRequestorComplete:(int)status;</code></pre></td>
</tr>
</tbody>
</table>


**Disponibilidade:** v1.0+

**Parâmetros**:

* *status*: pode ter um dos seguintes valores:
   * `ACCESS_ENABLER_STATUS_SUCCESS` - a fase de configuração foi concluída com êxito
   * `ACCESS_ENABLER_STATUS_ERROR` - falha na fase de configuração

**Acionado por:**
`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[Voltar ao início...](#apis)

</br>

### checkAuthentication {#checkAuthN}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Verifica o status de autenticação do usuário atual.
Ele faz isso procurando um token de autenticação válido no espaço de armazenamento de token local. Esse método não executa chamadas de rede e recomendamos chamá-lo na thread principal.
Ele é usado pelo aplicativo para consultar o status de autenticação do usuário e atualizar a interface de acordo (ou seja, atualizar a interface de logon/logout). O status de autenticação é comunicado ao aplicativo por meio da [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) retorno de chamada.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: verificar status de autenticação</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v1.0+

**Parâmetros:** Nenhum

**Retornos de chamada disparados:**
[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)

[Voltar ao início...](#apis)

</br>

### getAuthentication, getAuthentication:withData: {#getAuthN}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Inicia o fluxo de trabalho de autenticação completa. Ele é iniciado verificando o status de autenticação. Se ainda não estiver autenticado, a máquina de estado do fluxo de autenticação é iniciada:

* se a última tentativa de autenticação tiver sido bem-sucedida, a fase de seleção do MVPD será ignorada e a variável [`navigateToUrl:`](#nav2url) o retorno de chamada é disparado. O aplicativo usa essa chamada de retorno para instanciar o controle WebView que apresenta ao usuário a página de login do MVPD. **[OBSERVAÇÃO: desde o Access Enabler 1.5, esta funcionalidade não está disponível devido a uma limitação no SDK].**
* se a última tentativa de autenticação não tiver sido bem-sucedida ou se o usuário tiver feito logout explicitamente, a variável [`displayProviderDialog:`](#dispProvDialog) o retorno de chamada é disparado. Seu aplicativo usa esse retorno de chamada para exibir a interface de seleção de MVPD. Além disso, o aplicativo é necessário para retomar o fluxo de autenticação, informando a biblioteca do AccessEnabler sobre a seleção de MVPD do usuário por meio da [`setSelectedProvider:`](#setSelProv) método.

À medida que as credenciais do usuário são verificadas na página de logon do MVPD, sua aplicação é solicitada a monitorar as várias operações de redirecionamento que ocorrem enquanto o usuário é autenticado na página de logon do MVPD. Quando as credenciais corretas são inseridas, o controle WebView é redirecionado para um URL personalizado definido pelo `ADOBEPASS_REDIRECT_URL` constante. Este URL não deve ser carregado pelo WebView. O aplicativo deve interceptar esse URL e interpretar esse evento como um sinal de que a fase de logon foi concluída. Ele deve então entregar o controle ao AccessEnabler para concluir o fluxo de autenticação (chamando o [handleExternalURL](#handleExternalURL) método).

Por fim, o status de autenticação é comunicado ao aplicativo por meio da [setAuthenticationStatus:errorCode:](#setAuthNStatus) retorno de chamada.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: inicia o fluxo de autenticação</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication;</code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: inicia o fluxo de autenticação</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**Disponibilidade:** v1.9+

**Parâmetros:**

* *forceAuthn*: um sinalizador que especifica se o fluxo de autenticação deve ser iniciado, independentemente de o usuário já estar autenticado ou não.
* *dados*: um dicionário que consiste em pares de valores chave a serem enviados para o serviço de passe de TV por assinatura. O Adobe pode usar esses dados para habilitar funcionalidades futuras sem alterar o SDK.

**Retornos de chamada disparados:** ` setAuthenticationStatus:errorCode:, `[`displayProviderDialog:`](#dispProvDialog)`,`` sendTrackingData:forEventType:`


[Voltar ao início...](#apis)

</br>

### getAuthentication:filtro, getAuthentication:withData:andFilter {#getAuthN_filter}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Inicia o fluxo de trabalho de autenticação completa. Ele é iniciado verificando o status de autenticação. Se ainda não estiver autenticado, a máquina de estado do fluxo de autenticação é iniciada:

* [presentTvProviderDialog()](#presentTvDialog) será chamado se o solicitante atual tiver pelo menos um MVPD compatível com SSO. Se nenhum MVPD suportar SSO, o fluxo de autenticação clássico será iniciado e o parâmetro de filtro será ignorado.
* Depois que o usuário conclui o fluxo de SSO do Apple [dismissTvProviderDialog()](#dismissTvDialog) será acionado e o processo de autenticação será concluído.

Por fim, o status de autenticação é comunicado ao aplicativo por meio da [setAuthenticationStatus:errorCode:](#setAuthNStatus) retorno de chamada.

**Disponibilidade:** v2.4+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: inicia o fluxo de autenticação</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSDictionary *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: inicia o fluxo de autenticação</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSDictionary *)filter;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**Parâmetros:**

* *forceAuthn*: um sinalizador que especifica se o fluxo de autenticação deve ser iniciado, independentemente de o usuário já estar autenticado ou não.
* *dados*: um dicionário que consiste em pares de valores chave a serem enviados para o serviço de passe de TV por assinatura. O Adobe pode usar esses dados para habilitar funcionalidades futuras sem alterar o SDK.
* filtro: um dicionário com duas listas de IDs MVPD que devem aparecer na caixa de diálogo SSO do Apple. Qualquer MVPD que não suporte SSO será ignorado, mas a ordem será respeitada. O dicionário precisa ter duas chaves:
   * TV\_PROVIDERS: uma lista com todos os MVPDs que devem aparecer no seletor
   * FEATURED\_TV\_PROVIDERS: uma lista com todos os MVPDs que devem ser marcados como em destaque no seletor. Os MVPDs nessa lista também devem ser especificados na lista TV\_PROVIDERS.

**Disponibilidade:** v2.0 - v2.3.1


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: inicia o fluxo de autenticação</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(NSArray *)filter;</code></pre></td>
</tr>
</tbody>
</table>



<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: inicia o fluxo de autenticação</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthentication:(BOOL)forceAuthn:
                  withData:(NSDictionary* )data
                 andFilter:(NSArray *)filter;</code></pre>
<div>

</div></td>
</tr>
</tbody>
</table>



**Parâmetros:**

* *forceAuthn*: um sinalizador que especifica se o fluxo de autenticação deve ser iniciado, independentemente de o usuário já estar autenticado ou não.
* *dados*: um dicionário que consiste em pares de valores chave a serem enviados para o serviço de passe de TV por assinatura. O Adobe pode usar esses dados para habilitar funcionalidades futuras sem alterar o SDK.
* filtro: uma lista de IDs MVPD que devem aparecer na caixa de diálogo SSO do Apple. Qualquer MVPD que não suporte SSO será ignorado, mas a ordem será respeitada.

**Retornos de chamada disparados:** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[Voltar ao início...](#apis)

</br>

#### displayProviderDialog: {#dispProvDialog}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição** O retorno de chamada acionado pelo AccessEnabler para informar ao aplicativo que os elementos apropriados da interface do usuário precisam ser instanciados para permitir que o usuário selecione o MVPD desejado. O retorno de chamada fornece uma lista de objetos MVPD com informações adicionais que podem ajudar a criar corretamente o painel da interface de seleção (como o URL que aponta para o logotipo do MVPD, nome de exibição amigável etc.)

Depois que o usuário selecionar o MVPD desejado, o aplicativo de camada superior deverá retomar o fluxo de autenticação chamando `setSelectedProvider:` e transmitindo a ID do MVPD correspondente à seleção do usuário.

**Anulando o fluxo de autenticação** - Esse é um ponto em que o usuário tem a capacidade de pressionar o botão &quot;Voltar&quot;, o que é equivalente a suspender o fluxo de autenticação. Nesse cenário, seu aplicativo é solicitado a chamar o [setSelectedProvider:](#setSelProv) passará null como parâmetro para dar ao AccessEnabler a oportunidade de redefinir sua máquina de estado de autenticação.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: exibir a interface de seleção de MVPD</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) displayProviderDialog:(NSArray *)mvpds;</code></pre></td>
</tr>
</tbody>
</table>


**Disponibilidade:** v1.0+

**Parâmetros**:

* *mvpds*: lista de objetos MVPD que contêm informações relacionadas ao MVPD que o aplicativo pode usar para criar os elementos da interface de seleção MVPD.

**Acionado por:** ` getAuthentication, `[getAuthentication:withData:](#getAuthN),` getAuthorization:, `[getAuthorization:withData:](#getAuthZ)


[Voltar ao início...](#apis)

</br>

#### setSelectedProvider: {#setSelProv}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Esse método é chamado pelo aplicativo para informar o Ativador de acesso sobre a seleção de MVPD do usuário. O aplicativo pode usar esse método para selecionar ou alterar o provedor de serviços usado para autenticação.

Se o MVPD selecionado for um MVPD TempPass, ele será autenticado automaticamente com esse MVPD sem precisar chamar getAuthentication() posteriormente.

Observe que isso não é possível para a Passagem Temp Promocional, onde parâmetros extras são fornecidos no método getAuthentication().

Ao passar *null* como parâmetro, o Access Enabler presume que o usuário cancelou o fluxo de autenticação (ou seja, pressionou o botão &quot;Voltar&quot;) e responde redefinindo a máquina de estado de autenticação e chamando a variável [setAuthenticationStatus:errorCode:](#setAuthNStatus) retorno de chamada com o `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` código de erro.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: definir o provedor selecionado no momento</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setSelectedProvider:(NSString *)mvpdId;</code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v1.0+

**Parâmetros:** Nenhum

**Retornos de chamada disparados:** ` setAuthenticationStatus:errorCode:,sendTrackingData:forEventType:,  `[`navigateToUrl:`](#nav2url)

[Voltar ao início...](#apis)

</br>

#### navigateToUrl: {#nav2url}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição:** Retorno de chamada acionado pelo AccessEnabler para solicitar que seu aplicativo instancie um controlador UIWebView/WKWebView e carregue o URL fornecido no registro de retorno de chamada **`url`** parâmetro. O retorno de chamada passa a variável **`url`** parâmetro que representa o URL do endpoint de autenticação ou o URL do endpoint de logout.

Como UIWebView/WKWebView` `controladora passa por vários redirecionamentos, seu aplicativo deve monitorar a atividade da controladora e detectar o momento em que carrega um URL personalizado específico definido pelo `ADOBEPASS_REDIRECT_URL `constante (ou seja, `adobepass://ios.app`). Observe que esse URL personalizado específico é realmente inválido e não se destina ao controlador para carregá-lo. Ela deve ser interpretada somente pelo seu aplicativo como um sinal de que o fluxo de autenticação ou logout foi concluído e que é seguro fechar a controladora. Quando o controlador carrega esse URL personalizado específico, seu aplicativo deve fechar UIWebView/WKWebView e chamar AccessEnabler&#39;s `handleExternalURL:url `método da API.

**Nota:** Observe que, no caso do fluxo de autenticação, esse é um ponto em que o usuário pode pressionar o botão &quot;Voltar&quot;, o que equivale à anulação do fluxo de autenticação. Nesse cenário, seu aplicativo deve chamar o [setSelectedProvider:](#setSelProv) passagem de método **`nil`** como o parâmetro e dando uma chance ao AccessEnabler para redefinir sua máquina de estado de autenticação.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: exibir página de logon do MVPD</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) navigateToUrl:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v1.0+

**Parâmetros**:

* *url*: o URL que aponta para a página de logon do MVPD

**Acionado por:** [setSelectedProvider:](#setSelProv)



[Voltar ao início...](#apis)

</br>

#### navigateToUrl:useSVC: {#nav2urlSVC}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição:** Retorno de chamada acionado pelo AccessEnabler em vez do `navigateToUrl:` retorno de chamada caso seu aplicativo tenha ativado anteriormente a manipulação manual do Safari View Controller (SVC) por meio do [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](#setOptions) chamada e somente no caso de MVPDs que exigem o Safari View Controller (SVC). Para todos os outros MVPDs, a variável `navigateToUrl:` O retorno de chamada será chamado. Consulte[Suporte a SFSafariViewController no iOS SDK 3.2+](/help/authentication/sfsafariviewcontroller-support-on-ios-sdk-32.md) para obter detalhes sobre como o Safari View Controller (SVC) deve ser gerenciado.

Semelhante ao `navigateToUrl:` retorno de chamada o `navigateToUrl:useSVC:` é acionado pelo AccessEnabler para solicitar que o aplicativo instancie uma `SFSafariViewController` e para carregar o URL fornecido no repositório de retorno de chamada **`url`** parâmetro. O retorno de chamada passa a variável **`url`** parâmetro que representa o URL do endpoint de autenticação ou o URL do endpoint de logout e o **`useSVC`** parâmetro que especifica que o aplicativo deve usar um `SFSafariViewController`.

Como a variável `SFSafariViewController` O controlador passa por vários redirecionamentos, seu aplicativo deve monitorar a atividade do controlador e detectar o momento em que carrega um URL personalizado específico definido pelo `application's custom scheme` (por exemplo,** **`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Observe que esse URL personalizado específico é realmente inválido e não se destina ao controlador para carregá-lo. Ela deve ser interpretada somente pelo seu aplicativo como um sinal de que o fluxo de autenticação ou logout foi concluído e que é seguro fechar a controladora. Quando o controlador carrega este URL personalizado específico, seu aplicativo deve fechar o `SFSafariViewController` e chamar o AccessEnabler&#39;s `handleExternalURL:url `método da API.

**Nota:** Observe que, no caso do fluxo de autenticação, esse é um ponto em que o usuário pode pressionar o botão &quot;Voltar&quot;, o que equivale à anulação do fluxo de autenticação. Nesse cenário, seu aplicativo deve chamar o [setSelectedProvider:](#setSelProv) passagem de método **`nil`** como o parâmetro e dando uma chance ao AccessEnabler para redefinir sua máquina de estado de autenticação.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: exibir página de logon MVPD em SFSafariViewController</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>@optional
- (void) navigateToUrl:(NSString *)url useSVC:(BOOL)useSVC; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:**v 3.2+

**Parâmetros**:

* *url:* o URL que aponta para a página de logon do MVPD
* *useSVC:* se a url deve ser carregada em SFSafariViewController.

**Acionado por:**[ setOptions:](#setOptions) antes [setSelectedProvider:](#setSelProv)

[Voltar ao início...](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Esse método é chamado pelo aplicativo para concluir o fluxo de autenticação ou logout. Esse método deve ser chamado logo após o aplicativo detectar o momento em que o `UIWebView/WKWebView or SFSafariViewController` controladora é redirecionado a um URL personalizado específico. Caso seu aplicativo precise usar um `SFSafariViewController `controlador, o URL personalizado específico é definido pelo seu `application's custom scheme` (por exemplo,`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), caso contrário, esse URL personalizado específico será definido pela variável `ADOBEPASS_REDIRECT_URL `constante (ou seja, `adobepass://ios.app`).

No caso do fluxo de autenticação, o AccessEnabler conclui o fluxo recuperando o token de autenticação do servidor back-end e armazenando-o localmente no armazenamento de token. O AccessEnabler informará ao aplicativo que o fluxo de autenticação foi concluído chamando o `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> retorno de chamada com código de status 1, indicando sucesso. Se houver um erro durante a execução dessas etapas, a variável `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> O retorno de chamada é disparado com um código de status 0, indicando falha de autenticação, bem como um código de erro correspondente.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: conclua o fluxo de autenticação ou logout</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code> (void) handleExternalURL:(NSString *)url; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v3.0+

**Parâmetros:**

* *url*: o URL interceptado do ` UIWebView/WKWebView or SFSafariViewController ` control como string.


**Retornos de chamada disparados:** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[Voltar ao início...](#apis)

</br>

#### getAuthenticationToken - [OBSOLETO] {#getAuthNToken}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Conclui o fluxo de autenticação solicitando o token de autenticação do servidor back-end. Esse método deve ser chamado pelo aplicativo somente em resposta ao evento em que o controle WebView que hospeda a página de logon MVPD é redirecionado para o URL personalizado definido pelo `ADOBEPASS_REDIRECT_URL` constante.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: recuperar o token de autenticação</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthenticationToken; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v1.0+ **Até:** v3.0

**Parâmetros:** Nenhum

**Retornos de chamada disparados:** `setAuthenticationStatus:errorCode,sendTrackingData:forEventType:`

[Voltar ao início...](#apis)

&lt;/br

#### setAuthenticationStatus:errorCode: {#setAuthNStatus}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição** Retorno de chamada acionado pelo AccessEnabler que informa a aplicação do status do fluxo de autenticação. Há muitos lugares onde esse fluxo pode falhar, seja como resultado da interação do usuário ou devido a outros cenários imprevistos (ou seja, problemas de conectividade de rede etc.). Essa chamada de retorno informa a aplicação do status de sucesso/falha do fluxo de autenticação, além de fornecer informações adicionais sobre o motivo da falha, quando necessário.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: relatar o status do fluxo de autenticação</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setAuthenticationStatus:(int)status 
                       errorCode:(NSString *)code; </code></pre></td>
</tr>
</tbody>
</table>


**Disponibilidade:** v1.0+

**Parâmetros**:

* *status*: pode ter um dos seguintes valores:
   * `ACCESS_ENABLER_STATUS_SUCCESS` - fluxo de autenticação concluído com sucesso
   * `ACCESS_ENABLER_STATUS_ERROR` - falha no fluxo de autenticação
* *código*: motivo da falha. Se *status* é `ACCESS_ENABLER_STATUS_SUCCESS`, depois *código* é uma cadeia de caracteres vazia (ou seja, definida pela variável `USER_AUTHENTICATED` constante). Em caso de falha, esse parâmetro pode ter um dos seguintes valores:
   * `USER_NOT_AUTHENTICATED_ERROR` - O usuário não está autenticado. Em resposta à [checkAuthentication:](#checkAuthN) chamada de método quando não há um token de autenticação válido no cache de token local.
   * `PROVIDER_NOT_SELECTED_ERROR` - O AccessEnabler redefiniu a máquina de estado de autenticação depois que o aplicativo de camada superior passou *null* para [`setSelectedProvider:`](#setSelProv) para suspender o fluxo de autenticação.  Provavelmente, o usuário cancelou o fluxo de autenticação (ou seja, pressionou o botão &quot;Voltar&quot;).
   * `GENERIC_AUTHENTICATION_ERROR` - O fluxo de autenticação falhou devido a motivos como indisponibilidade de rede ou cancelamento explícito do fluxo de autenticação pelo usuário.

**Acionado por:** ` checkAuthentication, getAuthentication, `[getAuthentication:withData:](#getAuthN),` checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ)

[Voltar ao início...](#apis)

</br>

### checkPreauthorizedResources: {#checkPreauth}


**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Esse método é usado pelo aplicativo para determinar se o usuário já está autorizado a exibir recursos protegidos específicos. O objetivo principal desse método é recuperar informações para usar na decoração da interface do usuário **(por exemplo, indicando o status de acesso com ícones de bloquear e desbloquear).**

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: definir o provedor selecionado no momento</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v1.3+

**Parâmetros:**

* *recursos:* matriz de recursos cuja autorização deve ser verificada. Cada elemento na lista deve ser uma string que representa a ID do recurso. A ID do recurso está sujeita às mesmas limitações que a ID do recurso na chamada, ou seja, ela deve ser um valor acordado estabelecido entre o Programador e o MVPD ou um fragmento de RSS de mídia.

**Retorno de chamada disparado:** [`preauthorizedResources:`](#preauthResources)

[Voltar ao início...](#apis)

</br>

### checkPreauthorizedResources:cache: {#checkPreauthCache}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Esse método é usado pelo aplicativo para determinar se o usuário já está autorizado a exibir recursos protegidos específicos. O objetivo principal desse método é recuperar informações para usar na decoração da interface do usuário (por exemplo, indicando o status de acesso com ícones de bloqueio e desbloqueio). A variável **cache** O parâmetro controla se o cache interno é usado para resolver recursos.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: definir o provedor selecionado no momento</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources cache:(BOOL)cache; </code></pre></td>
</tr>
</tbody>
</table>



**Disponibilidade:** v3.1+



**Parâmetros:**

* *recursos:* matriz de recursos cuja autorização deve ser verificada. Cada elemento na lista deve ser uma string que representa a ID do recurso. A ID do recurso está sujeita às mesmas limitações que a ID do recurso na `getAuthorization:` ou seja, deve ser um valor acordado estabelecido entre o Programador e o MVPD ou um fragmento de RSS de mídia.
* *cache:* Booleano que especifica se o cache interno deve ser usado para resolver recursos. Se false, o cache será ignorado, resultando em chamadas do servidor sempre que essa API for chamada.

**Retorno de chamada disparado:** [`preauthorizedResources:`](#preauthResources)

[Voltar ao início...](#apis)

</br>

### recursos pré-autorizados: {#preauthResources}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição:** Retorno de chamada acionado por `checkPreauthorizedResources:`. Fornece uma lista de recursos que o usuário já está autorizado a visualizar.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: definir o provedor selecionado no momento</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkPreauthorizedResources:(NSArray *)resources; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v1.3+

**Parâmetros:**

* `resources`: matriz de recursos para a qual o usuário já está autorizado a visualizar.

**Acionado por:** [`checkPreauthorizedResources:`](#checkPreauth)



[Voltar ao início...](#apis)

</br>

### checkAuthorization:, checkAuthorization:withData: {#checkAuthZ}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Esse método é usado pelo aplicativo para verificar o status da autorização. Ela é iniciada verificando o status de autenticação primeiro. Se não estiver autenticado, a variável [tokenRequestFailed:errorCode:errorDescription:](#tokenReqFailed) O retorno de chamada é acionado e o método é encerrado. Se o usuário estiver autenticado, ele também acionará o fluxo de autorização. Veja os detalhes no [`getAuthorization:`](#getAuthZ) método.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: verificar status de autorização</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: verificar status de autorização</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) checkAuthorization:(NSString *)resource:
                   withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v1.9+

**Parâmetros:**

* *recurso*: a ID do recurso para o qual o usuário solicita autorização.
* *dados*: um dicionário que consiste em pares de valores chave a serem enviados para o serviço de passe de TV por assinatura. O Adobe pode usar esses dados para habilitar funcionalidades futuras sem alterar o SDK.

**Retornos de chamada disparados:**
[tokenRequestFailed:errorCode:errorDescription:](#tokenReqFailed)`,setToken:forResource:, sendTrackingData:forEventType:, setAuthenticationStatus:errorCode:`

[Voltar ao início...](#apis)

</br>

### getAuthorization:, getAuthorization:withData: {#getAuthZ}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Este método é usado pelo aplicativo para iniciar o fluxo de autorização. Se o usuário ainda não estiver autenticado, ele também iniciará o fluxo de autenticação. Se o usuário for autenticado, o AccessEnabler continuará a emitir solicitações para o token de autorização (se nenhum token de autorização válido estiver presente no cache do token local) e para o token de mídia de vida curta. Depois que o token de mídia curta é obtido, o fluxo de autorização é considerado concluído. A variável [setToken:forResource:](#setToken) o retorno de chamada é acionado e o token de mídia curto é fornecido como um parâmetro para o aplicativo. Se, por qualquer motivo, a autorização falhar, a [tokenRequestFailed:forEventType:](#tokenReqFailed) O retorno de chamada é acionado e o código/detalhes do erro são fornecidos.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: iniciar o fluxo de autorização</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v1.0+

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: iniciar o fluxo de autorização</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getAuthorization:(NSString *)resource:
                 withData:(NSDictionary *)data; </code></pre></td>
</tr>
</tbody>
</table>



**Disponibilidade:** v1.9+

**Parâmetros:**

* *recurso*: a ID do recurso para o qual o usuário solicita autorização.
* *dados*: um dicionário que consiste em pares de valores chave a serem enviados para o serviço de passe de TV por assinatura. O Adobe pode usar esses dados para habilitar funcionalidades futuras sem alterar o SDK.

**Retornos de chamada disparados:** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**Retornos de chamada adicionais acionados:**\
Esse método também pode acionar os seguintes retornos de chamada (se o fluxo de autenticação também for iniciado): `setAuthenticationStatus:errorCode:`, `displayProviderDialog:`

**NOTA: Use o checkAuthorization: / checkAuthorization:withData: em vez de getAuthorization: / getAuthorization:withData: sempre que possível. O getAuthorization: / getAuthorization:withData: O método iniciará um fluxo de autenticação completo (se o usuário não estiver autenticado) e isso pode levar a uma implementação complicada por parte do programador.**

[Voltar ao início...](#apis)

</br>

### setToken:forResource: {#setToken}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição** Retorno de chamada acionado pelo AccessEnabler que informa ao aplicativo que o fluxo de autorização foi concluído com êxito. O token de mídia de vida curta também é fornecido como um parâmetro.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Retorno de chamada: fluxo de autorização concluído com êxito</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setToken:(NSString *)token 
      forResource:(NSString *)resource; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v1.0+

**Parâmetros**:

* *token*: o token de mídia de vida curta
* *recurso*: o recurso para o qual a autorização foi obtida

**Acionado por:** [checkAuthorization:](#checkAuthZ)` , `[checkAuthorization:withData:](#checkAuthZ),` `[getAuthorization:](#getAuthZ), [getAuthorization:withData:](#getAuthZ)

[Voltar ao início...](#apis)

</br>

### tokenRequestFailed:errorCode:errorDescription: {#tokenReqFailed}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição** Retorno de chamada acionado pelo AccessEnabler que informa ao aplicativo de camada superior que o fluxo de autorização falhou.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Retorno de chamada: falha no fluxo de autorização</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) tokenRequestFailed:(NSString *)resource 
                  errorCode:(NSString *)code 
           errorDescription:(NSString *)description; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v1.0+

**Parâmetros**:

* *recurso*: o recurso para o qual a autorização foi obtida.
* *código*: o código de erro associado ao cenário de falha. Valores possíveis:
   * `USER_NOT_AUTHORIZED_ERROR` - o usuário não pôde autorizar para o recurso fornecido
* *descrição*: Detalhes adicionais sobre o cenário de falha. Se essa cadeia descritiva não estiver disponível por algum motivo, a Autenticação Adobe Pass enviará uma cadeia de caracteres vazia **(&quot;&quot;)**.\
  Essa cadeia de caracteres pode ser usada por um MVPD para passar mensagens de erro personalizadas ou mensagens relacionadas a vendas. Por exemplo, se um assinante tiver a autorização negada para um recurso, o MVPD poderá enviar uma mensagem como: &quot;No momento, você não tem acesso a esse canal em seu pacote. Se quiser atualizar seu pacote, clique em **aqui**.&quot; A mensagem é passada pela Autenticação Adobe Pass por meio dessa chamada de retorno ao Programador, que tem a opção de exibi-la ou ignorá-la. A Autenticação do Adobe Pass também pode usar esse parâmetro para fornecer notificação da condição que pode ter levado a um erro. Por exemplo, &quot;Ocorreu um erro de rede ao se comunicar com o serviço de autorização do provedor&quot;.

**Acionado por:** ` checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ), `getAuthorization:, `[getAuthorization:withData:](#getAuthZ)

[Voltar ao início...](#apis)

</br>

### logout {#logout}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Esse método é chamado pelo aplicativo para iniciar o fluxo de logout. O logout é o resultado de uma série de operações de redirecionamento HTTP devido ao fato de que o usuário precisa ser desconectado dos servidores de Autenticação do Adobe Pass e também dos servidores do MVPD. Como esse fluxo não pode ser concluído com uma simples solicitação HTTP emitida pela biblioteca do AccessEnabler, um `UIWebView/WKWebView or SFSafariViewController` é necessário instanciar o controlador para poder seguir as operações de redirecionamento HTTP.

O fluxo de logout é diferente do fluxo de autenticação porque o usuário não precisa interagir com o `UIWebView/WKWebView or SFSafariViewController`  de qualquer forma. Portanto, o Adobe recomenda que você torne o controle invisível (ou seja, oculto) durante o processo de logout.

Um padrão semelhante ao fluxo de autenticação é empregado. O iOS AccessEnabler aciona o `navigateToUrl:` retorno de chamada ou o `navigateToUrl:useSVC:` para criar um `UIWebView/WKWebView or SFSafariViewController` e para carregar o URL fornecido no repositório de retorno de chamada `url` parâmetro. Este é o URL do ponto de extremidade de logout no servidor back-end. Para o tvOS AccessEnabler, nem o `navigateToUrl:` retorno de chamada ou o `navigateToUrl:useSVC:` o retorno de chamada é chamado.

À medida que passa por vários redirecionamentos, seu aplicativo deve monitorar a atividade do `UIWebView/WKWebView or SFSafariViewController `e detectar o momento em que carrega um URL personalizado específico. Observe que esse URL personalizado específico é realmente inválido e não se destina ao controlador para carregá-lo. Ela deve ser interpretada somente pelo seu aplicativo como um sinal de que o fluxo de logout foi concluído e que é seguro fechar a controladora. Quando o controlador carrega esse URL personalizado específico, seu aplicativo deve fechar o controlador e chamar o AccessEnabler&#39;s `handleExternalURL:url `método da API. Caso seu aplicativo precise usar um `SFSafariViewController `controlador, o URL personalizado específico é definido pelo seu `application's custom scheme` (por exemplo,`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), caso contrário, esse URL personalizado específico será definido pela variável `ADOBEPASS_REDIRECT_URL `constante (ou seja, `adobepass://ios.app`).

No final, o AccessEnabler chamará o [`setAuthenticationStatus()`](#setAuthNStatus) retorno de chamada com código de status 0, indicando sucesso do fluxo de logout.

**Nota:** Se o usuário estiver conectado usando o Apple SSO, o status VSA203 será acionado. Se esse for o caso, o usuário deve ser instruído a também fazer logoff das configurações do sistema. Se isso não for feito, haverá uma nova autenticação quando o aplicativo for reiniciado.


<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: iniciar o fluxo de logout</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) logout; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v1.0+

**Parâmetros:** Nenhum

**Retornos de chamada disparados:** `navigateToUrl:, `[`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



[Voltar ao início...](#apis)

</br>

### getSelectedProvider {#getSelProv}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Use este método para determinar o provedor selecionado no momento.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: determine o MVPD selecionado no momento</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getSelectedProvider; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v1.0+

**Parâmetros:** Nenhum

**Retornos de chamada disparados:** [`selectedProvider:`](#selProv)

[Voltar ao início...](#apis)

</br>

### seletedProvider {#selProv}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição** Retorno de chamada acionado pelo AccessEnabler que fornece informações sobre o MVPD selecionado no momento para o aplicativo.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: informações sobre o MVPD selecionado no momento</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) selectedProvider:(MVPD *)mvpd;</code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v1.0+

**Parâmetros**:

* *mvpd*: objeto que contém informações sobre o MVPD selecionado no momento

**Acionado por:** [`getSelectedProvider`](#getSelProv)

[Voltar ao início...](#apis)

</br>

### getMetadata: {#getMeta}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Use esse método para recuperar informações expostas como metadados pela biblioteca AccessEnabler. O aplicativo pode acessar esses dados fornecendo um formulário com base em dicionário *key* parâmetro de entrada.

Há dois tipos de metadados disponíveis para programadores:

* Metadados estáticos (TTL do token de autenticação, TTL do token de autorização e ID do dispositivo)
* Metadados do usuário (informações específicas do usuário, como ID do usuário, código postal; podem ser transmitidas de um MVPD para o dispositivo de um usuário durante os fluxos de Autenticação e Autorização)

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Chamada de API: consulte o AccessEnabler para obter metadados</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) getMetadata:(NSDictionary *)keyDictionary; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v1.0+

**Parâmetros:**

* *keyDictionary*: uma estrutura de dados de dicionário, com o seguinte formato:
   * Se a chave for `METADATA_OPCODE_KEY` e o valor é `METADATA_AUTHENTICATION`, a consulta é feita para obter o tempo de expiração do token de autenticação.
   * Se a chave for `METADATA_OPCODE_KEY` e o valor é `METADATA_AUTHORIZATION` **e**\
     a chave é `METADATA_RESOURCE_ID_KEY` e o valor for uma ID de recurso específica, a consulta será feita para obter a hora de expiração do token de autorização associado ao recurso especificado.
   * Se a chave for `METADATA_OPCODE_KEY` e o valor é `METADATA_DEVICE_ID`, a consulta é feita para obter a id do dispositivo atual. Observe que esse recurso está desativado por padrão e os programadores devem entrar em contato com o Adobe para obter informações sobre ativação e taxas.
   * Se a chave for `METADATA_OPCODE_KEY` e o valor é `METADATA_USER_META` **e** a chave é `METADATA_USER_META_KEY` e valor é o nome dos metadados, então a consulta é feita para metadados do usuário. A lista de tipos de metadados de usuário disponíveis:
      * `zip` - Lista de códigos postais
      * `householdID` - Identificador do agregado. No caso de um MVPD não suportar subcontas, isso será idêntico a `userID`.
      * `maxRating` - Uma coleção de classificações máximas dos pais para o usuário
      * `userID` - O identificador do usuário. Se um MVPD suportar subcontas e o usuário não for a conta principal, `userID` será diferente de `householdID.`
      * `channelID` - Uma lista de canais que um usuário está autorizado a visualizar.

  >[!NOTE]
  >
  >Os metadados reais do usuário disponíveis para um Programador dependem do que um MVPD disponibiliza. Essa lista será expandida à medida que novos metadados forem disponibilizados e adicionados ao sistema de autenticação da Adobe Pass.

**Retornos de chamada disparados:** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**Mais informações:** [Metadados do usuário](/help/authentication/user-metadata.md)

[Voltar ao início...](#apis)

</br>

### presentTVProviderDialog {#presentTvDialog}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição** Retorno de chamada acionado pelo AccessEnabler após a chamada[getAuthentication()](#getAuthN) se o solicitante atual suportar pelo menos um MVPD com suporte a SSO.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: resultado de fluxos de SSO</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) presentTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v2.0+

**Parâmetros**:

* viewController: representa a caixa de diálogo SSO do Apple. Este viewController precisa ser exibido na tela.

**Acionado por:** [`getAuthentication`](#getAuthN)

**Mais informações:** [Logon único do iOS/tvOS](#presentTvDialog)

[Voltar ao início...](#apis)

</br>

### dismissTVProviderDialog {#dismissTvDialog}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição** Retorno de chamada acionado pelo AccessEnabler após o usuário fechar a caixa de diálogo SSO do Apple.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: resultado de fluxos de SSO</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) dismissTvProviderDialog: (UIViewController *) viewController; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v2.0+

**Parâmetros**:

* viewController: representa a caixa de diálogo SSO do Apple. Este viewController precisa ser removido da tela.

**Acionado por:** Ação do usuário

**Mais informações:** [Logon único do iOS/tvOS](#presentTvDialog)

[Voltar ao início...](#apis)

</br>

### setMetadataStatus:encrypted:forKey:andArguments: {#setMetaStatus}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição** Retorno de chamada acionado pelo AccessEnabler que fornece os metadados solicitados por meio de um [`getMetadata:`](#getMeta) chame.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: resultado de solicitação de recuperação de metadados</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><pre><code>- (void) setMetadataStatus:(id)metadata
                 encrypted:(bool)encrypted 
                    forKey:(int)key 
              andArguments:(NSDictionary *)arguments; </code></pre></td>
</tr>
</tbody>
</table>

**Disponibilidade:** v1.0+

**Parâmetros**:

* *metadados*: os metadados solicitados. Esse valor é um `NSString` no caso de metadados estáticos (TTL de autenticação, TTL de autorização, ID do dispositivo).  É um objeto complexo ao solicitar metadados específicos do usuário. Esse objeto complexo geralmente é a representação em Objetive-C de uma carga JSON (por exemplo, &#39;{&quot;street&quot;: &quot;Main Avenue&quot;, &quot;building&quot;): [&quot;150&quot;, &quot;320&quot;]}&#39; é traduzido em Objetive-C como NSDictionary(&quot;street&quot; -> &quot;Main Avenue&quot;, &quot;building&quot; -> NSArray(&quot;150&quot;, &quot;320&quot;))).   Exemplo de objeto JSON de metadados:

```JSON
        {
            updated: 1334243471,
            encrypted: ["encryptedProp"],
            data: {
                zip: ["12345", "34567"],
                maxRating: { 
                    "MPAA": "PG-13",
                    "VCHIP": "TV-Y", 
                    "URL": "http://exam.pl/e/manage/ratings"
                },
                householdID: "3456",
                userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
                channelID: ["channel-1", "channel-2"]
            }
```

* *criptografado*: valor booliano que especifica se os metadados recuperados estão criptografados ou não. Esse parâmetro é importante apenas para solicitações de metadados do usuário, não tem significado para metadados estáticos (por exemplo, TTL de autenticação) que são sempre recebidos não criptografados. Se esse parâmetro for definido como true, cabe ao Programador obter o valor de Metadados do usuário não criptografado executando uma descriptografia RSA usando a chave privada da lista de permissões (a mesma chave privada usada para a assinatura da ID do solicitante no [`setRequestor:setSignedRequestorId:`](#setReq) e `setRequestor:setSignedRequestorId:serviceProviders: `chamadas).

* *key*: a chave usada para formular a solicitação de recuperação de metadados.

* *argumentos*: O mesmo dicionário passado para o [`getMetadata:`](#getMeta) chame. Isso é fornecido para permitir que o aplicativo corresponda às solicitações com as respostas.

**Acionado por:** [`getMetadata:`](#getMeta)

**Mais informações:** [Metadados do usuário](/help/authentication/user-metadata.md)


[Voltar ao início...](#apis)

</br>

### MVPD {#mvpd}

**Arquivo:** AccessEnabler/headers/model/MVPD.h

**Descrição** Descreve o objeto MVPD. Pode ser usado para obter informações sobre as propriedades do MVPD.

**Disponibilidade:** v1.0+ [A propriedade boardingStatus está disponível na v2.2]

**Propriedades**:

* ID (NSString) - A ID do MVPD.
* (NSString) displayName - O nome MVPD. [Isso deve ser usado para exibir no seletor]
* (NSString) logoURL - O endereço do logotipo do MVPD.
* (BOOL) enablePlatformServices - Se true, o MVPD oferecerá suporte a serviços SSO como [APPLE SSO](#presentTvDialog).
* (NSString) boardingStatus - pode ter 3 valores:
   * nil - O MVPD não oferece suporte ao Apple SSO.
   * SELETOR - O MVPD pode aparecer no seletor de Apple, mas o fluxo de autenticação é feito pelo Adobe.
   * COMPATÍVEL - O MVPD é totalmente compatível com o Apple e usará o token SSO do Apple.

[Voltar ao início...](#apis)

</br>

## Rastreamento de eventos {#tracking}

O AccessEnabler aciona um retorno de chamada adicional que não está necessariamente relacionado aos fluxos de direito. A implementação da [`sendTrackingData()`](#sendTracking) a função de retorno de chamada é opcional, mas permite que o aplicativo rastreie eventos específicos e compile estatísticas, como o número de tentativas de autenticação/autorização bem-sucedidas/com falha.

### sendTrackingData:forEventType: {#sendTracking}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição** Retorno de chamada acionado pelo AccessEnabler que sinaliza para o aplicativo a ocorrência de vários eventos, como a conclusão/falha de fluxos de autenticação/autorização. Com o Adobe Pass Authentication 1.6, o tipo de dispositivo, o tipo de cliente AccessEnabler e o sistema operacional são relatados por [`sendTrackingData()`](#sendTracking). A variável [`sendTrackingData()`](#sendTracking) O retorno de chamada permanece compatível com versões anteriores.

**Retorno de chamada: rastreamento de eventos**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**Disponibilidade:** v1.0+

**Nota:** O tipo de dispositivo e o sistema operacional são derivados do uso de uma biblioteca Java pública (<http://java.net/projects/user-agent-utils>) e a sequência de agente do usuário. Esteja ciente de que essas informações são fornecidas apenas como uma forma grosseira de dividir as métricas operacionais em categorias de dispositivos, mas esse Adobe não pode assumir nenhuma responsabilidade por resultados incorretos. Use a nova funcionalidade adequadamente.

* Valores possíveis para o tipo de dispositivo:
   * `computer`
   * `tablet`
   * `mobile`
   * `gameconsole`
   * `unknown`

* Valores possíveis para o tipo de cliente AccessEnabler:
   * `flash`
   * `html5`
   * `ios`
   * `android`


**Parâmetros**:

* *evento*: o código do evento que está sendo rastreado. Há três tipos possíveis de eventos de rastreamento:
   * **authorizationDetection:** sempre que uma solicitação de token de autorização for retornada (o evento é `TRACKING_AUTHORIZATION`)
   * **authenticationDetection:** sempre que ocorrer uma verificação de autenticação (o evento é `TRACKING_AUTHENTICATION`)
   * **mvpdSelection:** quando o usuário seleciona um MVPD no formulário de seleção de MVPD (o evento é `TRACKING_GET_SELECTED_PROVIDER`)
* *dados*: dados adicionais associados ao evento relatado. Esses dados são apresentados no formato de uma lista de valores.

**Acionado por:** `checkAuthentication, getAuthentication, `[getAuthentication:withData:](#getAuthN), `checkAuthorization:, `[checkAuthorization:withData:](#checkAuthZ), `getAuthorization:, `[getAuthorization:withData:](#getAuthZ), `setSelectedProvider:`

Instruções para interpretar os valores na variável *dados* matriz:

* Para trackingEventType `TRACKING_AUTHENTICATION:`
   * **0** - Se a solicitação de token foi bem-sucedida (true/false) e, se bem-sucedida:
   * **1** - Sequência de caracteres de ID MVPD
   * **2** - GUID (md5 com hash)
   * **3** - Token já no cache (true/false)
   * **4** - Tipo de dispositivo
   * **5** - Tipo de cliente AccessEnabler
   * **6** - Tipo de sistema operacional

* Para trackingEventType `TRACKING_AUTHORIZATION:`
   * **0** - Se a solicitação de token foi bem-sucedida (true/false) e, se bem-sucedida:
   * **1** - ID MVPD
   * **2** - GUID (md5 com hash)
   * **3** - Token já no cache (true/false)
   * **4** - Erro
   * **5** - Detalhes
   * **6** - Tipo de dispositivo
   * **7** - Tipo de cliente AccessEnabler
   * **8** - Tipo de sistema operacional
* Para trackingEventType `TRACKING_GET_SELECTED_PROVIDER:`
   * **0** - ID do MVPD selecionado no momento
   * **1** - Tipo de dispositivo
   * **2** - Tipo de cliente AccessEnabler
   * **3** - Tipo de sistema operacional

</br>
