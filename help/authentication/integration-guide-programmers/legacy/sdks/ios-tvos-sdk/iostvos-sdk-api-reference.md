---
title: Referência da API do iOS/tvOS
description: Referência da API do iOS/tvOS
exl-id: 017a55a8-0855-4c52-aad0-d3d597996fcb
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '6942'
ht-degree: 0%

---

# (Herdado) Referência da API do SDK do iOS/tvOS {#iostvos-sdk-api-reference}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

## Introdução {#intro}

Esta página descreve os métodos e as funções de retorno de chamada expostos pelo cliente nativo do iOS/tvOS para autenticação da Adobe Pass. Os métodos e as funções de retorno de chamada descritos aqui estão definidos nos arquivos de cabeçalho `AccessEnabler.h` e `EntitlementDelegate.h`. Você pode encontrá-los no SDK do iOS AccessEnabler aqui: `[SDK directory]/AccessEnabler/headers/api/`


Documentação associada:

* Para obter uma apresentação passo a passo de como implementar o Adobe Pass
fluxo de direitos de autenticação usando esta API, consulte o [Guia de Integração do iOS](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-cookbook.md).
* Para obter a mais recente SDK do iOS AccessEnabler, consulte a [Biblioteca do iOS Native Access Enabler](https://tve.zendesk.com/hc/en-us/articles/204963209-iOS-Native-AccessEnabler-Library).

>[!NOTE]
>
>O Adobe incentiva você a usar somente as APIs *públicas* de Autenticação da Adobe Pass:
>
>* As APIs públicas estão disponíveis e totalmente testadas em todos os tipos de clientes compatíveis. Para qualquer recurso público, garantimos que cada tipo de cliente tenha uma versão correspondente dos métodos associados.
>* As APIs públicas devem ser o mais estáveis possível, para oferecer compatibilidade com versões anteriores e garantir que as integrações de parceiros não sejam interrompidas. No entanto, para APIs não públicas, reservamos o direito de alterar sua assinatura em qualquer ponto futuro. Se você encontrar um fluxo específico que não possa ser suportado por meio de uma combinação das chamadas de API de autenticação do Adobe Pass públicas atuais, a melhor abordagem é informar o. Levando em consideração suas necessidades, podemos modificar as APIs públicas e fornecer uma solução estável a partir de agora.

</br>

## Referência da API {#apis}

* [init](#initWithSoftwareStatement):softwareStatement - Instancia o objeto AccessEnabler.

* **[DEPRECATED]** [init](#init) - Instancia o objeto AccessEnabler.

* [`setOptions:options:`](#setOptions) - Configura as opções globais do SDK, como profile ou visitorID.

* [`setRequestor:`](#setReqV3)[`requestorID`](#setReqV3),[`setRequestor:requestorID:serviceProviders:`](#setReqV3) - Estabelece a identidade do Programador.

* **[OBSOLETO]** [`setRequestor:signedRequestorId:`](#setReq),[`setRequestor:signedRequestorId:serviceProviders:`](#setReq) - Estabelece a identidade do Programador.

* **[OBSOLETO]** [`setRequestor:signedRequestorId:secret:publicKey`](#setReq_tvos), [`setRequestor:signedRequestorId:serviceProviders:secret:publicKey`](#setReq_tvos)-Estabelece a identidade do Programador.

* [`setRequestorComplete:`](#setReqComplete) - Informa ao aplicativo que a fase de configuração foi concluída.

* [`checkAuthentication`](#checkAuthN) - Verifica o status de autenticação do usuário atual.

* [`getAuthentication`](#getAuthN), [`getAuthentication:withData:`](#getAuthN) - Inicia o fluxo de trabalho de autenticação completa.

* [`getAuthentication:filter`](#getAuthN_filter),[`getAuthentication:withData:`](#getAuthN)[andFilter](#getAuthN_filter) - Inicia o fluxo de trabalho de autenticação completa.

* [`displayProviderDialog:`](#dispProvDialog) - Informa seu aplicativo a instanciar os elementos apropriados da interface do usuário para que o usuário selecione uma MVPD.

* [`setSelectedProvider:`](#setSelProv) - Informa o AccessEnabler sobre a seleção de MVPD do usuário.

* [`navigateToUrl:`](#nav2url) - Informa ao aplicativo que o usuário precisa ser apresentado à página de logon do MVPD.

* [`navigateToUrl:useSVC:`](#nav2urlSVC) - Informa ao aplicativo que o usuário precisa ser apresentado à página de logon do MVPD, usando SFSafariViewController

* [`handleExternalURL:url`](#handleExternalURL) - Conclui o fluxo de autenticação/logout.

* **[DEPRECATED]** [`getAuthenticationToken`](#getAuthNToken) - Solicita o token de autenticação do servidor back-end.

* [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) - Informa ao aplicativo o status do fluxo de autenticação.

* [`checkPreauthorizedResources:`](#checkPreauth) - Determina se o usuário já está autorizado a exibir recursos protegidos específicos.

* [`checkPreauthorizedResources:cache:`](#checkPreauthCache) - Determina se o usuário já está autorizado a exibir recursos protegidos específicos.

* [`preauthorizedResources:`](#preauthResources) - Fornece uma lista de recursos que o usuário já está autorizado a visualizar.

* [`checkAuthorization:`](#checkAuthZ), [`checkAuthorization:withData:`](#checkAuthZ) - Verifica o status de autorização do usuário atual.

* [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ) - Inicia o fluxo de autorização.

* [`setToken:forResource:`](#setToken) - Informa ao aplicativo que o fluxo de autorização foi concluído com êxito.

* [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) - Informa ao aplicativo que houve falha no fluxo de autorização.

* [`logout`](#logout) - Inicia o fluxo de logout.

* [`getSelectedProvider`](#getSelProv) - Determina o provedor selecionado no momento.

* [`selectedProvider:`](#selProv) - Fornece informações sobre o MVPD selecionado no momento ao seu aplicativo.

* [`getMetadata:`](#getMeta) - Recupera informações expostas como metadados pela biblioteca AccessEnabler.

* [`presentTvProviderDialog:`](#presentTvDialog) - Informa seu aplicativo para exibir a Caixa de Diálogo Apple SSO.

* [`dismissTvProviderDialog:`](#dismissTvDialog) - Informa seu aplicativo para ocultar a Caixa de Diálogo Apple SSO.

* [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus) - Entrega os metadados solicitados por uma chamada [`getMetadata:`](#getMeta).

* [`sendTrackingData:forEventType:`](#sendTracking) - Fornece informações de dados de rastreamento.

* [`MVPD`](#mvpd) - A classe MVPD. [Contém informações sobre a MVPD]

### init:softwareStatement {#initWithSoftwareStatement}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Instancia o objeto AccessEnabler. Deve haver uma única instância AccessEnabler por instância do aplicativo.

| **Chamada de API: construtor AccessEnabler do iOS** |
| --- |
| `- (id) init:` <br> |
| `(NSString *)softwareStatement;` |


**Disponibilidade:** v3.0+

**Parâmetros:**

* **softwareStatement:** uma cadeia de caracteres que identifica o aplicativo no sistema Adobe. Veja como obter uma Declaração de Software.

[Voltar ao início...](#apis)



### init - [DEPRECATED]{#init}

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

**Observação:** os valores serão passados ao servidor independentemente do fluxo atual (autenticação/autorização). Se quiser alterar os valores, você pode chamar esse método a qualquer momento.

| Chamada de API: setOptions |
| --- |
| ```- (void) setOptions:(NSDictionary *)options;``` |

**Disponibilidade:** v2.3.0+

**Parâmetros:**

* *options*: um NSDictionary contendo opções globais do SDK. Atualmente, as seguintes opções estão disponíveis:
   * **applicationProfile** - Ele pode ser usado para fazer configurações de servidor baseadas nesse valor.
   * **visitorID** - O Serviço de ID de Experience Cloud. Esse valor pode ser usado posteriormente para relatórios de análise avançada.
   * **handleSVC** - Booleano que indica se o programador manipulará o SFSafariViewControllers. Consulte o [Suporte a SFSafariViewController no iOS SDK 3.2+](/help/authentication/integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md) para obter mais detalhes.
      * Se definido como **false**, o SDK apresentará automaticamente ao usuário final um SFSafariViewController. O SDK navegará ainda mais para o URL da página de logon dos MVPDs.
      * Se definido como **true**, o SDK **NOT** apresentará automaticamente ao usuário final um SFSafariViewController. O SDK acionará ainda **navigate(toUrl:{url}, useSVC:YES)**.
* **device\_info** - Informações do cliente conforme descrito em [Passando informações do cliente](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md).

[Voltar ao início...](#apis)


### `setRequestor:requestorID`, `setRequestor:requestorID:serviceProviders:` {#setReqV3}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Define a identidade do Programador. Cada programador recebe uma ID exclusiva ao se registrar no Adobe para o sistema de autenticação da Adobe Pass. Ao lidar com SSO e tokens remotos, o estado de autenticação pode mudar quando o aplicativo estiver em segundo plano, setRequestor pode ser chamado novamente quando o aplicativo for trazido para o primeiro plano para sincronizar com o estado do sistema (busque um token remoto se o SSO estiver ativado ou exclua o token local se um logout tiver ocorrido enquanto isso).

A resposta do servidor contém uma lista de MVPDs juntamente com algumas informações de configuração anexadas à identidade do Programador. A resposta do servidor é usada internamente pelo código AccessEnabler. Somente o status da operação (ou seja, SUCESSO/FALHA) é apresentado ao seu aplicativo por meio do retorno de chamada `setRequestorComplete:`.

Se o parâmetro `urls` não for usado, a chamada de rede resultante será direcionada para a URL do provedor de serviços padrão: o ambiente de Adobe RELEASE/produção.


Se um valor for fornecido para o parâmetro `urls`, a chamada de rede resultante será direcionada a todas as URLs fornecidas no parâmetro `urls`. Todas as solicitações de configuração são acionadas simultaneamente em threads separados. O primeiro respondente tem prioridade ao compilar a lista de MVPDs. Para cada MVPD na lista, o AccessEnabler lembra o URL do provedor de serviços associado. Todas as solicitações de direito subsequentes são direcionadas ao URL associado ao provedor de serviços que foi emparelhado com o MVPD de destino durante a fase de configuração.

| Chamada de API: configuração do solicitante |
| --- |
| ```- (void) setRequestor:(NSString *)requestorID``` |


**Disponibilidade:** v3.0+

| Chamada de API: configuração do solicitante |
| --- |
| `- (void) setRequestor:(NSString *)requestorID serviceProviders:(NSArray *)urls;` |


**Disponibilidade:** v3.0+

**Parâmetros:**

* *requestorID*: a identificação exclusiva associada ao Programador. Transmita o identificador exclusivo atribuído pelo Adobe ao seu site quando você se registra pela primeira vez no serviço de autenticação da Adobe Pass.
* *urls*: parâmetro opcional; por padrão, o provedor de serviços da Adobe é usado (http://sp.auth.adobe.com/). Essa matriz permite especificar endpoints para serviços de autenticação e autorização fornecidos pelo Adobe (instâncias diferentes podem ser usadas para fins de depuração). Você pode usar essa opção para especificar várias instâncias do provedor de serviços de Autenticação da Adobe Pass. Ao fazer isso, a lista do MVPD é composta pelos endpoints de todos os provedores de serviços. Cada MVPD está associado ao provedor de serviços mais rápido; ou seja, o provedor que respondeu primeiro e que oferece suporte a esse MVPD.

>[!NOTE]
>
>Se chamada sem o parâmetro `serviceProviders`, a biblioteca recuperará a configuração do provedor de serviços padrão (ou seja, `https://sp.auth.adobe.com` para o perfil de produção ou `https://sp.auth-staging.adobe.com` para o perfil de preparo). Se o parâmetro `serviceProviders` for fornecido, ele deverá ser uma matriz de URLs. As informações de configuração são recuperadas de todos os endpoints especificados e são mescladas. Se houver informações duplicadas em diferentes respostas do provedor de serviços, o conflito será resolvido em favor do servidor com resposta mais rápida (ou seja, o servidor com o menor tempo de resposta tem prioridade).

**Retornos de chamada disparados:** [`setRequestorComplete:`](#setReqComplete)

[Voltar ao início...](#apis)

</br>

### `setRequestor:setSignedRequestorId:`, `setRequestor:setSignedRequestorId:serviceProviders:` - [OBSOLETO] {#setReq}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Define a identidade do Programador. Cada programador recebe uma ID exclusiva ao se registrar no Adobe para o sistema de autenticação da Adobe Pass. Ao lidar com SSO e tokens remotos, o estado de autenticação pode mudar quando o aplicativo estiver em segundo plano, setRequestor pode ser chamado novamente quando o aplicativo for colocado em primeiro plano para sincronizar com o estado do sistema (busque um token remoto se o SSO estiver habilitado ou exclua o token local se um logout tiver ocorrido enquanto isso).

A resposta do servidor contém uma lista de MVPDs juntamente com algumas informações de configuração anexadas à identidade do Programador. A resposta do servidor é usada internamente pelo código AccessEnabler. Somente o status da operação (ou seja, SUCESSO/FALHA) é apresentado ao seu aplicativo por meio do retorno de chamada `setRequestorComplete:`.

Se o parâmetro `urls` não for usado, a chamada de rede resultante será direcionada para a URL do provedor de serviços padrão: o ambiente de Adobe RELEASE/produção.

Se um valor for fornecido para o parâmetro `urls`, a chamada de rede resultante será direcionada a todas as URLs fornecidas no parâmetro `urls`. Todas as solicitações de configuração são acionadas simultaneamente em threads separados. O primeiro respondente tem prioridade ao compilar a lista de MVPDs. Para cada MVPD na lista, o AccessEnabler lembra o URL do provedor de serviços associado. Todas as solicitações de direito subsequentes são direcionadas ao URL associado ao provedor de serviços que foi emparelhado com o MVPD de destino durante a fase de configuração.

| Chamada de API: configuração do solicitante |
| --- |
| </br>`- (void) setRequestor:(NSString *)requestorID`</br>`signedRequestorID:(NSString *)signedRequestorID;` </br></br> |

**Disponibilidade:** v1.0+ **Até:** v3.0

| Chamada de API: configuração do solicitante |
| --- |
| `- (void) setRequestor:(NSString *)requestorID ` <br> `       signedRequestorID:(NSString *)signedRequestorID` <br> `         serviceProviders:(NSArray *)urls;` |

**Disponibilidade:** v1.0+ **Até:** v3.0

**Parâmetros:**

* *requestorID*: a identificação exclusiva associada ao Programador. Transmita o identificador exclusivo atribuído pelo Adobe ao seu site quando você se registrou pela primeira vez no serviço de autenticação da Adobe Pass.
* *signedRequestorID*: **Este parâmetro existe nas versões 1.2 e posteriores do iOS AccessEnabler.** Uma cópia da ID do solicitante que foi assinada digitalmente com sua chave privada. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls*: parâmetro opcional; por padrão, o provedor de serviços da Adobe é usado (http://sp.auth.adobe.com/). Essa matriz permite especificar endpoints para serviços de autenticação e autorização fornecidos pelo Adobe (instâncias diferentes podem ser usadas para fins de depuração). Você pode usar essa opção para especificar várias instâncias do provedor de serviços de Autenticação da Adobe Pass. Ao fazer isso, a lista do MVPD é composta pelos endpoints de todos os provedores de serviços. Cada MVPD está associado ao provedor de serviços mais rápido; ou seja, o provedor que respondeu primeiro e que oferece suporte a esse MVPD.

**Observações:** Se chamada sem o parâmetro `serviceProviders`, a biblioteca recuperará a configuração do provedor de serviços padrão (ou seja, `https://sp.auth.adobe.com` para o perfil de produção ou `https://sp.auth-staging.adobe.com` para o perfil de preparo). Se o parâmetro `serviceProviders` for fornecido, ele deverá ser uma matriz de URLs. As informações de configuração são recuperadas de todos os endpoints especificados e são mescladas. Se houver informações duplicadas em diferentes respostas do provedor de serviços, o conflito será resolvido em favor do servidor com resposta mais rápida (ou seja, o servidor com o menor tempo de resposta tem prioridade).

**Retornos de chamada disparados:** [`setRequestorComplete:`](#setReqComplete)


[Voltar ao início...](#apis)

### `setRequestor:setSignedRequestorId:secret:publicKey`, `setRequestor:setSignedRequestorId:serviceProviders:secret:publicKey` - [OBSOLETO] {#setReq_tvos}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Define a identidade do Programador. Cada programador recebe uma ID exclusiva ao se registrar no Adobe para o sistema de autenticação da Adobe Pass. Esta configuração deve ser executada somente uma vez durante o ciclo de vida do aplicativo.

A resposta do servidor contém uma lista de MVPDs juntamente com algumas informações de configuração anexadas à identidade do Programador. A resposta do servidor é usada internamente pelo código AccessEnabler. Somente o status da operação (ou seja, SUCESSO/FALHA) é apresentado ao seu aplicativo por meio do retorno de chamada `setRequestorComplete:`.

Se o parâmetro `urls` não for usado, a chamada de rede resultante será direcionada para a URL do provedor de serviços padrão: o ambiente de Adobe RELEASE/produção.

Se um valor for fornecido para o parâmetro `urls`, a chamada de rede resultante será direcionada a todas as URLs fornecidas no parâmetro `urls`. Todas as solicitações de configuração são acionadas simultaneamente em threads separados. O primeiro respondente tem prioridade ao compilar a lista de MVPDs. Para cada MVPD na lista, o AccessEnabler lembra o URL do provedor de serviços associado. Todas as solicitações de direito subsequentes são direcionadas ao URL associado ao provedor de serviços que foi emparelhado com o MVPD de destino durante a fase de configuração.



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

* *requestorID*: a identificação exclusiva associada ao Programador. Transmita o identificador exclusivo atribuído pelo Adobe ao seu site pela primeira vez   registrado com o serviço de Autenticação da Adobe Pass.
* *signedRequestorID*: **Este parâmetro existe no iOS AccessEnabler   versões 1.2 e posteriores.** Uma cópia da ID do solicitante que foi assinada digitalmente com sua chave privada. <!--For more details, see [Registering Native Clients](https://tve.helpdocsonline.com/registering-native-clients)-->.
* *urls*: parâmetro opcional; por padrão, o provedor de serviços da Adobe   é usado (http://sp.auth.adobe.com/). Essa matriz permite especificar endpoints para serviços de autenticação e autorização fornecidos pelo Adobe (instâncias diferentes podem ser usadas para fins de depuração). Você pode usar essa opção para especificar várias instâncias do provedor de serviços de Autenticação da Adobe Pass. Ao fazer isso, a lista do MVPD é composta pelos endpoints de todos os provedores de serviços. Cada MVPD está associado ao provedor de serviços mais rápido; ou seja, o provedor que respondeu primeiro e que oferece suporte a esse MVPD.
* secret e publicKey: as chaves secreta e pública usadas para assinar as chamadas da segunda tela. Para obter mais informações, consulte a [documentação sem cliente](#create_dev).

Se chamada sem o parâmetro `serviceProviders`, a biblioteca recuperará a configuração do provedor de serviços padrão (ou seja, `https://sp.auth.adobe.com` para o perfil de produção ou https://sp.auth-staging.adobe.com para o perfil de preparo). Se o parâmetro `serviceProviders` for fornecido, ele deverá ser uma matriz de URLs. As informações de configuração são recuperadas de todos os endpoints especificados e são mescladas. Se houver informações duplicadas em diferentes respostas do provedor de serviços, o conflito será resolvido em favor do servidor com resposta mais rápida (ou seja, o servidor com o menor tempo de resposta tem prioridade).

**Retornos de chamada disparados:** [`setRequestorComplete:`](#setReqComplete)

[Voltar ao início...](#apis)

</br>

### setRequestorComplete: {#setReqComplete}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição** Retorno de chamada disparado pelo AccessEnabler que informa ao aplicativo que a fase de configuração foi concluída. Esse é um sinal de que o aplicativo pode começar a emitir solicitações de direito. Nenhuma solicitação de direito pode ser emitida pelo aplicativo até que a fase de configuração seja concluída.

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

* *status*: pode assumir um dos seguintes valores:
   * `ACCESS_ENABLER_STATUS_SUCCESS` - a fase de configuração foi concluída com êxito
   * `ACCESS_ENABLER_STATUS_ERROR` - falha na fase de configuração

**Acionado por:**

`setRequestor:setSignedRequestorId:, `[`setRequestor:setSignedRequestorId:serviceProviders:`](#setReq)

[Voltar ao início...](#apis)

</br>

### checkAuthentication {#checkAuthN}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** verifica o status de autenticação do usuário atual.
Ele faz isso procurando um token de autenticação válido no local
espaço de armazenamento de token. Esse método não executa chamadas de rede e recomendamos chamá-lo na thread principal.
Ele é usado pelo aplicativo para consultar o status de autenticação do usuário e
atualizar a interface de acordo (ou seja, atualizar a interface de logon/logout). A variável
o status de autenticação é comunicado ao aplicativo por meio de
o retorno de chamada [`setAuthenticationStatus:errorCode:`](#setAuthNStatus).


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

### `getAuthentication`, `getAuthentication:withData:` {#getAuthN}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** inicia o fluxo de trabalho de autenticação completa. Ele é iniciado verificando o status de autenticação. Se ainda não estiver autenticado, a máquina de estado do fluxo de autenticação é iniciada:

* se a última tentativa de autenticação tiver sido bem-sucedida, o MVPD   a fase de seleção é ignorada e   o retorno de chamada [`navigateToUrl:`](#nav2url) foi disparado. A variável   O aplicativo usa esse retorno de chamada para instanciar o controle WebView que apresenta ao usuário a página de login do MVPD. **[OBSERVAÇÃO: desde o Access Enabler 1.5, esta funcionalidade não está disponível devido a uma limitação no SDK].**
* se a última tentativa de autenticação não tiver sido bem-sucedida ou se o usuário tiver feito logout explicitamente, o retorno de chamada [`displayProviderDialog:`](#dispProvDialog) será   acionado. Seu aplicativo usa essa chamada de retorno para exibir a interface de seleção do MVPD. Além disso, o aplicativo é necessário para retomar o fluxo de autenticação, informando a biblioteca AccessEnabler sobre a seleção de MVPD do usuário por meio do método [`setSelectedProvider:`](#setSelProv).

À medida que as credenciais do usuário são verificadas na página de logon da MVPD, sua aplicação é solicitada a monitorar as várias operações de redirecionamento que ocorrem enquanto o usuário é autenticado na página de logon da MVPD. Quando as credenciais corretas são inseridas, o controle WebView é redirecionado para uma URL personalizada definida pela constante `ADOBEPASS_REDIRECT_URL`. Este URL não deve ser carregado pelo WebView. O aplicativo deve interceptar esse URL e interpretar esse evento como um sinal de que a fase de logon foi concluída. Em seguida, ele deve entregar o controle ao AccessEnabler para concluir o fluxo de autenticação (chamando o método [handleExternalURL](#handleExternalURL)).

Finalmente, o status de autenticação é comunicado ao aplicativo por meio do retorno de chamada [`setAuthenticationStatus:errorCode:`](#setAuthNStatus).

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
* *dados*: um dicionário que consiste em pares de valores chave a serem enviados para o serviço de passe de TV por Assinatura. O Adobe pode usar esses dados para habilitar funcionalidades futuras sem alterar o SDK.

**Retornos de chamada disparados:** `setAuthenticationStatus:errorCode:`, [`displayProviderDialog:`](#dispProvDialog), `sendTrackingData:forEventType:`


[Voltar ao início...](#apis)

</br>

### `getAuthentication:filter`, `getAuthentication:withData:andFilter` {#getAuthN_filter}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** inicia o fluxo de trabalho de autenticação completa. Ele é iniciado verificando o status de autenticação. Se ainda não estiver autenticado, a máquina de estado do fluxo de autenticação é iniciada:

* [presentTvProviderDialog()](#presentTvDialog) será chamado se o solicitante atual tiver pelo menos uma MVPD que ofereça suporte a SSO. Se nenhum MVPD for compatível com SSO, o fluxo de autenticação clássica será iniciado e o parâmetro de filtro será ignorado.
* Depois que o usuário concluir, o fluxo de SSO do Apple [`dismissTvProviderDialog()`](#dismissTvDialog) será acionado e o processo de autenticação será concluído.

Finalmente, o status de autenticação é comunicado ao aplicativo por meio do retorno de chamada [`setAuthenticationStatus:errorCode:`](#setAuthNStatus).

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
* *dados*: um dicionário que consiste em pares de valores chave a serem enviados para o serviço de passe de TV por Assinatura. O Adobe pode usar esses dados para habilitar funcionalidades futuras sem alterar o SDK.
* filtro: um dicionário com duas listas de IDs do MVPD que devem aparecer na caixa de diálogo SSO do Apple. Qualquer MVPD que não seja compatível com SSO será ignorada, mas o pedido será respeitado. O dicionário precisa ter duas chaves:
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
* *dados*: um dicionário que consiste em pares de valores chave a serem enviados para o serviço de passe de TV por Assinatura. O Adobe pode usar esses dados para habilitar funcionalidades futuras sem alterar o SDK.
* filtro: uma lista de IDs do MVPD que devem aparecer na caixa de diálogo SSO do Apple. Qualquer MVPD que não seja compatível com SSO será ignorada, mas o pedido será respeitado.

**Retornos de chamada disparados:** `setAuthenticationStatus:errorCode:, presentTvProviderDialog, dismissTvProviderDialog`


[Voltar ao início...](#apis)

</br>

#### displayProviderDialog: {#dispProvDialog}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição** Retorno de chamada disparado pelo AccessEnabler para informar ao aplicativo que os elementos apropriados da interface do usuário precisam ser instanciados para permitir que o usuário selecione o MVPD desejado. A chamada de retorno fornece uma lista de objetos do MVPD com informações adicionais que podem ajudar a criar corretamente o painel da interface do usuário de seleção (como o URL que aponta para o logotipo do MVPD, o nome de exibição amigável etc.)

Depois que o usuário seleciona o MVPD desejado, o aplicativo de camada superior deve retomar o fluxo de autenticação, chamando `setSelectedProvider:` e transmitindo a ID do MVPD correspondente à seleção do usuário.

**Anulando o fluxo de autenticação** - Este é um ponto em que o usuário pode pressionar o botão &quot;Voltar&quot;, o que é equivalente a anular o fluxo de autenticação. Nesse cenário, o aplicativo deve chamar o método [setSelectedProvider:](#setSelProv), transmitindo nulo como parâmetro, para dar ao AccessEnabler a oportunidade de redefinir seu computador de estado de autenticação.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Retorno de chamada: exibir a interface do usuário de seleção do MVPD</th>
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

* *mvpds*: lista de objetos do MVPD que contêm informações relacionadas ao MVPD que o aplicativo pode usar para criar os elementos da interface de seleção do MVPD.

**Acionado por:** `getAuthentication`, [`getAuthentication:withData:`](#getAuthN),`getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)


[Voltar ao início...](#apis)

</br>

#### setSelectedProvider: {#setSelProv}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Este método é chamado pelo seu aplicativo para informar ao Habilitador de Acesso a seleção de MVPD do usuário. O aplicativo pode usar esse método para selecionar ou alterar o provedor de serviços usado para autenticação.

Se o MVPD selecionado for um TempPass MVPD, ele será autenticado automaticamente com esse MVPD sem precisar chamar getAuthentication() posteriormente.

Observe que isso não é possível para a Passagem Temp Promocional, onde parâmetros extras são fornecidos no método getAuthentication().

Ao passar *null* como parâmetro, o Habilitador de Acesso presume que o usuário cancelou o fluxo de autenticação (ou seja, pressionou o botão &quot;Voltar&quot;) e responde redefinindo a máquina de estado de autenticação e chamando a chamada de retorno [`setAuthenticationStatus:errorCode:`](#setAuthNStatus) com o código de erro `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR`.

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

**Retornos de chamada disparados:** `setAuthenticationStatus:errorCode:`,`sendTrackingData:forEventType:`, [`navigateToUrl:`](#nav2url)

[Voltar ao início...](#apis)

</br>

#### navigateToUrl: {#nav2url}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição:** Retorno de chamada disparado pelo AccessEnabler para solicitar que seu aplicativo instancie um controlador UIWebView/WKWebView e para carregar a URL fornecida no parâmetro **`url`** do retorno de chamada. O retorno de chamada passa o parâmetro **`url`**, que representa a URL do ponto de extremidade de autenticação ou a URL do ponto de extremidade de logout.

Como o controlador UIWebView/WKWebView` `passa por vários redirecionamentos, seu aplicativo deve monitorar a atividade do controlador e detectar o momento em que carrega uma URL personalizada específica definida pela constante `ADOBEPASS_REDIRECT_URL ` (ou seja, `adobepass://ios.app`). Observe que esse URL personalizado específico é realmente inválido e não se destina ao controlador para carregá-lo. Ela deve ser interpretada somente pelo seu aplicativo como um sinal de que o fluxo de autenticação ou logout foi concluído e que é seguro fechar a controladora. Quando o controlador carrega esta URL personalizada específica, seu aplicativo deve fechar UIWebView/WKWebView e chamar o método `handleExternalURL:url `API do AccessEnabler.

**Observação:** observe que, no caso do fluxo de autenticação, esse é um ponto em que o usuário pode pressionar o botão &quot;Voltar&quot;, o que equivale à anulação do fluxo de autenticação. Nesse cenário, o aplicativo deve chamar o método [setSelectedProvider:](#setSelProv) transmitindo **`nil`** como parâmetro e dando ao AccessEnabler a chance de redefinir seu computador de estado de autenticação.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: exibir a página de logon do MVPD</th>
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

* *url*: a URL que aponta para a página de logon do MVPD

**Acionado por:** [setSelectedProvider:](#setSelProv)



[Voltar ao início...](#apis)

</br>

#### `navigateToUrl:useSVC:` {#nav2urlSVC}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição:** Retorno de chamada disparado pelo AccessEnabler em vez do retorno de chamada `navigateToUrl:` caso seu aplicativo tenha habilitado anteriormente o controle manual do Safari View Controller (SVC) por meio da chamada [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](#setOptions), e somente no caso de MVPDs que exigem o Safari View Controller (SVC). Para todos os outros MVPDs, o retorno de chamada `navigateToUrl:` será chamado. Consulte[Suporte a SFSafariViewController no iOS SDK 3.2+](/help/authentication/integration-guide-programmers/legacy/notes-technical/sfsafariviewcontroller-support-on-ios-sdk-32.md) para obter detalhes sobre como o Safari View Controller (SVC) deve ser gerenciado.

Semelhante ao retorno de chamada `navigateToUrl:`, o `navigateToUrl:useSVC:` é acionado pelo AccessEnabler para solicitar que seu aplicativo instancie um controlador `SFSafariViewController` e carregue a URL fornecida no parâmetro **`url`** do retorno de chamada. O retorno de chamada passa o parâmetro **`url`**, que representa a URL do ponto de extremidade de autenticação ou a URL do ponto de extremidade de logout, e o parâmetro **`useSVC`**, que especifica que o aplicativo deve usar um `SFSafariViewController`.

À medida que o controlador `SFSafariViewController` passa por vários redirecionamentos, seu aplicativo deve monitorar a atividade do controlador e detectar o momento em que carrega um URL personalizado específico definido pelo seu `application's custom scheme` (por exemplo,** **`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`). Observe que esse URL personalizado específico é realmente inválido e não se destina ao controlador para carregá-lo. Ela deve ser interpretada somente pelo seu aplicativo como um sinal de que o fluxo de autenticação ou logout foi concluído e que é seguro fechar a controladora. Quando o controlador carrega esta URL personalizada específica, seu aplicativo deve fechar o `SFSafariViewController` e chamar o método de API `handleExternalURL:url ` do AccessEnabler.

**Observação:** observe que, no caso do fluxo de autenticação, esse é um ponto em que o usuário pode pressionar o botão &quot;Voltar&quot;, o que equivale à anulação do fluxo de autenticação. Nesse cenário, o aplicativo deve chamar o método [setSelectedProvider:](#setSelProv) transmitindo **`nil`** como parâmetro e dando ao AccessEnabler a chance de redefinir seu computador de estado de autenticação.

<table class="pass_api_table">
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th>Callback: exibir a página de logon do MVPD em SFSafariViewController</th>
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

* *url:* a URL que aponta para a página de logon do MVPD
* *useSVC:* se a URL deve ser carregada em SFSafariViewController.

**Acionado por:**[ setOptions:](#setOptions) antes de [setSelectedProvider:](#setSelProv)

[Voltar ao início...](#apis)

</br>

#### handleExternalURL:url {#handleExternalURL}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Este método é chamado pelo aplicativo para concluir o fluxo de autenticação ou logout. Este método deve ser chamado logo após o aplicativo detectar o momento em que o controlador `UIWebView/WKWebView or SFSafariViewController` é redirecionado para um URL personalizado específico. Caso seu aplicativo precise usar um controlador `SFSafariViewController `, a URL personalizada específica é definida por seu `application's custom scheme` (por exemplo, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`); caso contrário, essa URL personalizada específica é definida pela constante `ADOBEPASS_REDIRECT_URL ` (por exemplo, `adobepass://ios.app`).

No caso do fluxo de autenticação, o AccessEnabler conclui o fluxo recuperando o token de autenticação do servidor back-end e armazenando-o localmente no armazenamento de token. O AccessEnabler informará ao aplicativo que o fluxo de autenticação foi concluído, chamando o retorno de chamada `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> com um código de status 1, indicando sucesso. Se houver um erro durante a execução dessas etapas, o retorno de chamada `setAuthenticationStatus()`<!--(http://tve.helpdocsonline.com/ios-technical-overview#setAuthNStatus)--> será disparado com um código de status 0, indicando falha de autenticação, bem como um código de erro correspondente.

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

* *url*: a URL interceptada do controle ` UIWebView/WKWebView or SFSafariViewController ` como cadeia de caracteres.


**Retornos de chamada disparados:** `setAuthenticationStatus:errorCode, sendTrackingData:forEventType:`

[Voltar ao início...](#apis)

</br>

#### getAuthenticationToken - [DEPRECATED] {#getAuthNToken}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** conclui o fluxo de autenticação solicitando o token de autenticação do servidor back-end. Esse método deve ser chamado pelo aplicativo somente em resposta ao evento em que o controle WebView que hospeda a página de logon do MVPD é redirecionado para a URL personalizada definida pela constante `ADOBEPASS_REDIRECT_URL`.


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

#### `setAuthenticationStatus:errorCode:` {#setAuthNStatus}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição** Retorno de chamada disparado pelo AccessEnabler que informa ao aplicativo o status do fluxo de autenticação. Há muitos lugares onde esse fluxo pode falhar, seja como resultado da interação do usuário ou devido a outros cenários imprevistos (ou seja, problemas de conectividade de rede etc.). Essa chamada de retorno informa a aplicação do status de sucesso/falha do fluxo de autenticação, além de fornecer informações adicionais sobre o motivo da falha, quando necessário.

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

* *status*: pode assumir um dos seguintes valores:
   * `ACCESS_ENABLER_STATUS_SUCCESS` - fluxo de autenticação concluído com êxito
   * `ACCESS_ENABLER_STATUS_ERROR` - falha no fluxo de autenticação
* *código*: motivo da falha. Se o *status* for `ACCESS_ENABLER_STATUS_SUCCESS`, então o *código* será uma cadeia de caracteres vazia (isto é, definida pela constante `USER_AUTHENTICATED`). Em caso de falha, esse parâmetro pode ter um dos seguintes valores:
   * `USER_NOT_AUTHENTICATED_ERROR` - Usuário não autenticado. Em resposta à chamada do método [checkAuthentication:](#checkAuthN) quando não há um token de autenticação válido no cache de token local.
   * `PROVIDER_NOT_SELECTED_ERROR` - O AccessEnabler redefiniu o       máquina de estado de autenticação após o aplicativo de camada superior       passou *null* para [`setSelectedProvider:`](#setSelProv) para anular o fluxo de autenticação.  Provavelmente, o usuário cancelou o fluxo de autenticação (ou seja, pressionou o botão &quot;Voltar&quot;).
   * `GENERIC_AUTHENTICATION_ERROR` - Falha no fluxo de autenticação devido a motivos como indisponibilidade da rede ou cancelamento explícito do fluxo de autenticação pelo usuário.

**Acionado por:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ)

[Voltar ao início...](#apis)

</br>

### checkPreauthorizedResources: {#checkPreauth}


**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Este método é usado pelo aplicativo para determinar se o usuário já está autorizado a exibir recursos protegidos específicos. A principal finalidade deste método é recuperar informações para uso na decoração da interface do usuário **(por exemplo, indicando o status de acesso com ícones de bloqueio e desbloqueio).**

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

* *recursos:* matriz de recursos cuja autorização deve ser verificada. Cada elemento na lista deve ser uma string que representa a ID do recurso. A variável     A ID do recurso está sujeita às mesmas limitações que a ID do recurso na chamada, ou seja, ela deve ser um valor acordado estabelecido entre o Programador e a MVPD ou um fragmento de RSS de mídia.

**Retorno de chamada disparado:** [`preauthorizedResources:`](#preauthResources)

[Voltar ao início...](#apis)

</br>

### `checkPreauthorizedResources:cache:` {#checkPreauthCache}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Este método é usado pelo aplicativo para determinar se o usuário já está autorizado a exibir recursos protegidos específicos. O objetivo principal desse método é recuperar informações para usar na decoração da interface do usuário (por exemplo, indicando o status de acesso com ícones de bloqueio e desbloqueio). O parâmetro **cache** controla se o cache interno é usado para resolver recursos.

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

* *recursos:* matriz de recursos cuja autorização deve ser verificada. Cada elemento na lista deve ser uma string que representa a ID do recurso. A ID do recurso está sujeita às mesmas limitações que a ID do recurso na chamada `getAuthorization:`, ou seja, ela deve ser um valor acordado estabelecido entre o Programador e a MVPD ou um fragmento de RSS de mídia.
* *cache:* booliano que especifica se o cache interno deve ser usado para resolver recursos. Se false, o cache será ignorado, resultando em chamadas do servidor sempre que essa API for chamada.

**Retorno de chamada disparado:** [`preauthorizedResources:`](#preauthResources)

[Voltar ao início...](#apis)

</br>

### recursos pré-autorizados: {#preauthResources}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição:** Retorno de chamada disparado por `checkPreauthorizedResources:`. Fornece uma lista de recursos que o usuário já está autorizado a visualizar.

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

### `checkAuthorization:`, `checkAuthorization:withData:` {#checkAuthZ}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Este método é usado pelo aplicativo para verificar o status de autorização. Ela é iniciada verificando o status de autenticação primeiro. Se não estiver autenticado, o retorno de chamada [`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed) será disparado e o método será encerrado. Se o usuário estiver autenticado, ele também acionará o fluxo de autorização. Veja detalhes sobre o método [`getAuthorization:`](#getAuthZ).


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

* *recurso*: a identificação do recurso para o qual o usuário solicita autorização.
* *dados*: um dicionário que consiste em pares de valores chave a serem enviados para o serviço de passe de TV por Assinatura. O Adobe pode usar esses dados para habilitar funcionalidades futuras sem alterar o SDK.

**Retornos de chamada disparados:**

[`tokenRequestFailed:errorCode:errorDescription:`](#tokenReqFailed),`setToken:forResource:`, `sendTrackingData:forEventType:`, `setAuthenticationStatus:errorCode:`

[Voltar ao início...](#apis)

</br>

### `getAuthorization:`, `getAuthorization:withData:` {#getAuthZ}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Este método é usado pelo aplicativo para iniciar o fluxo de autorização. Se o usuário ainda não estiver autenticado, ele também iniciará o fluxo de autenticação. Se o usuário for autenticado, o AccessEnabler continuará a emitir solicitações para o token de autorização (se nenhum token de autorização válido estiver presente no cache do token local) e para o token de mídia de vida curta. Depois que o token de mídia curta é obtido, o fluxo de autorização é considerado concluído. O retorno de chamada [`setToken:forResource:`](#setToken) é disparado e o token de mídia curto é entregue como um parâmetro ao aplicativo. Se, por qualquer motivo, a autorização falhar, o retorno de chamada [`tokenRequestFailed:forEventType:`](#tokenReqFailed) será acionado e o código/detalhes do erro serão fornecidos.

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

* *recurso*: a identificação do recurso para o qual o usuário solicita autorização.
* *dados*: um dicionário que consiste em pares de valores chave a serem enviados para o serviço de passe de TV por Assinatura. O Adobe pode usar esses dados para habilitar funcionalidades futuras sem alterar o SDK.

**Retornos de chamada disparados:** `tokenRequestFailed:errorCode:errorDescription:, setToken:forResource:,sendTrackingData:forEventType:`

**Retornos de chamada adicionais disparados:**\
Este método também pode disparar os seguintes retornos de chamada (se o fluxo de autenticação também for iniciado): `setAuthenticationStatus:errorCode:`, `displayProviderDialog:`

>[!NOTE]
>
>Use `checkAuthorization:` / `checkAuthorization:withData:` em vez de `getAuthorization:` / `getAuthorization:withData:` sempre que possível. O método `getAuthorization:` / `getAuthorization:withData:` iniciará um fluxo de autenticação completo (se o usuário não estiver autenticado) e isso pode levar a uma implementação complicada por parte do Programador.

[Voltar ao início...](#apis)

</br>

### `setToken:forResource:` {#setToken}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição** Retorno de chamada disparado pelo AccessEnabler que informa ao aplicativo que o fluxo de autorização foi concluído com êxito. O token de mídia de vida curta também é fornecido como um parâmetro.


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

**Acionado por:** [`checkAuthorization:`](#checkAuthZ) , [`checkAuthorization:withData:`](#checkAuthZ), [`getAuthorization:`](#getAuthZ), [`getAuthorization:withData:`](#getAuthZ)

[Voltar ao início...](#apis)

</br>

### `tokenRequestFailed:errorCode:errorDescription:` {#tokenReqFailed}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição** Retorno de chamada disparado pelo AccessEnabler que informa ao aplicativo de camada superior que o fluxo de autorização falhou.

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
   * `USER_NOT_AUTHORIZED_ERROR` - o usuário não pôde autorizar
para o recurso especificado
* *descrição*: detalhes adicionais sobre o cenário de falha. Se essa cadeia de caracteres descritiva não estiver disponível por algum motivo, a Autenticação Adobe Pass enviará uma cadeia de caracteres vazia **(&quot;)**.\
  Esta cadeia de caracteres pode ser usada por uma MVPD para enviar mensagens de erro personalizadas ou mensagens relacionadas às vendas. Por exemplo, se um assinante tiver a autorização negada para um recurso, o MVPD poderá enviar uma mensagem como: &quot;No momento, você não tem acesso a esse canal em seu pacote. Se quiser atualizar seu pacote, clique **aqui**.&quot; A mensagem é passada pela Autenticação Adobe Pass por meio dessa chamada de retorno ao Programador, que tem a opção de exibi-la ou ignorá-la. A Autenticação do Adobe Pass também pode usar esse parâmetro para fornecer notificação da condição que pode ter levado a um erro. Por exemplo, &quot;Ocorreu um erro de rede ao se comunicar com o serviço de autorização do provedor&quot;.

**Acionado por:** `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ)

[Voltar ao início...](#apis)

</br>

### logout {#logout}

**Arquivo:** AccessEnabler/headers/AccessEnabler.h

**Descrição:** Este método é chamado pelo aplicativo para iniciar o fluxo de logout. O logout é o resultado de uma série de operações de redirecionamento HTTP devido ao fato de que o usuário precisa ser desconectado dos servidores de Autenticação do Adobe Pass e também dos servidores da MVPD. Como esse fluxo não pode ser concluído com uma simples solicitação HTTP emitida pela biblioteca AccessEnabler, um controlador `UIWebView/WKWebView or SFSafariViewController` precisa ser instanciado para poder seguir as operações de redirecionamento HTTP.

O fluxo de logout difere do fluxo de autenticação na medida em que o usuário não é obrigado a interagir com o controlador `UIWebView/WKWebView or SFSafariViewController` de nenhuma maneira. Portanto, o Adobe recomenda que você torne o controle invisível (ou seja, oculto) durante o processo de logout.

Um padrão semelhante ao fluxo de autenticação é empregado. O iOS AccessEnabler aciona o retorno de chamada `navigateToUrl:` ou o `navigateToUrl:useSVC:` para criar um controlador `UIWebView/WKWebView or SFSafariViewController` e carregar a URL fornecida no parâmetro `url` do retorno de chamada. Este é o URL do ponto de extremidade de logout no servidor back-end. Para tvOS AccessEnabler, nem o retorno de chamada `navigateToUrl:` nem o retorno de chamada `navigateToUrl:useSVC:` são chamados.

À medida que passa por vários redirecionamentos, seu aplicativo deve monitorar a atividade do controlador `UIWebView/WKWebView or SFSafariViewController ` e detectar o momento em que carrega um URL personalizado específico. Observe que esse URL personalizado específico é realmente inválido e não se destina ao controlador para carregá-lo. Ela deve ser interpretada somente pelo seu aplicativo como um sinal de que o fluxo de logout foi concluído e que é seguro fechar a controladora. Quando o controlador carrega esta URL personalizada específica, seu aplicativo deve fechar o controlador e chamar o método de API `handleExternalURL:url ` do AccessEnabler. Caso seu aplicativo precise usar um controlador `SFSafariViewController `, a URL personalizada específica é definida por seu `application's custom scheme` (por exemplo, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`); caso contrário, essa URL personalizada específica é definida pela constante `ADOBEPASS_REDIRECT_URL ` (por exemplo, `adobepass://ios.app`).

No final, o AccessEnabler chamará o retorno de chamada [`setAuthenticationStatus()`](#setAuthNStatus) com um código de status 0, indicando sucesso do fluxo de logout.

**Observação:** se o usuário estiver conectado usando o Apple SSO, o status VSA203 será acionado. Se esse for o caso, o usuário deve ser instruído a também fazer logoff das configurações do sistema. Se isso não for feito, haverá uma nova autenticação quando o aplicativo for reiniciado.


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

**Retornos de chamada disparados:** `navigateToUrl:`, [`setAuthenticationStatus:errorCode:`](#setAuthNStatus)



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

**Descrição** Retorno de chamada disparado pelo AccessEnabler que fornece informações sobre o MVPD atualmente selecionado para o aplicativo.

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

**Descrição:** Use este método para recuperar informações expostas como metadados pela biblioteca AccessEnabler. O aplicativo pode acessar esses dados fornecendo um parâmetro de entrada *key* baseado em dicionário.

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

* *keyDictionary*: uma estrutura de dados de dicionário, com o seguinte
formato:
   * Se a chave for `METADATA_OPCODE_KEY` e o valor for `METADATA_AUTHENTICATION`, a consulta será feita para obter o tempo de expiração do token de autenticação.
   * Se a chave for `METADATA_OPCODE_KEY` e o valor for `METADATA_AUTHORIZATION` **and**\
     a chave é `METADATA_RESOURCE_ID_KEY` e o valor é uma ID de recurso específica, então a consulta é feita para obter a hora de expiração do token de autorização associado ao recurso especificado.
   * Se a chave for `METADATA_OPCODE_KEY` e o valor for `METADATA_DEVICE_ID`, será feita a consulta para obter a ID do dispositivo atual. Observe que esse recurso está desativado por padrão e os programadores devem entrar em contato com o Adobe para obter informações sobre ativação e taxas.
   * Se a chave for `METADATA_OPCODE_KEY` e o valor for `METADATA_USER_META` **e a chave** for `METADATA_USER_META_KEY` e o valor for o nome dos metadados, a consulta será feita para os metadados do usuário. A lista de tipos de metadados de usuário disponíveis:
      * `zip` - Lista de códigos postais
      * `householdID` - Identificador da família. Caso uma MVPD não suporte subcontas, será idêntico a `userID`.
      * `maxRating` - Uma coleção de classificações parentais máximas para o usuário
      * `userID` - O identificador do usuário. Se uma MVPD der suporte a subcontas e o usuário não for a conta principal, `userID` será diferente de `householdID.`
      * `channelID` - Uma lista de canais que um usuário está autorizado a visualizar.

  >[!NOTE]
  >
  >Os metadados reais do usuário disponíveis para um programador dependem do que uma MVPD disponibiliza. Essa lista será expandida à medida que novos metadados forem disponibilizados e adicionados ao sistema de autenticação da Adobe Pass.

**Retornos de chamada disparados:** [`setMetadataStatus:encrypted:forKey:andArguments:`](#setMetaStatus)

**Mais Informações:** [Metadados de Usuário](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)

[Voltar ao início...](#apis)

</br>

### presentTVProviderDialog {#presentTvDialog}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição** Retorno de chamada disparado pelo AccessEnabler após chamar[getAuthentication()](#getAuthN) se o solicitante atual oferecer suporte a pelo menos uma MVPD com suporte a SSO.

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

**Mais Informações** [Logon Único do iOS/tvOS](#presentTvDialog)

[Voltar ao início...](#apis)

</br>

### dismissTVProviderDialog {#dismissTvDialog}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição** Retorno de chamada disparado pelo AccessEnabler depois que o usuário fecha a Caixa de Diálogo Apple SSO.

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

**Mais Informações** [Logon Único do iOS/tvOS](#presentTvDialog)

[Voltar ao início...](#apis)

</br>

### `setMetadataStatus:encrypted:forKey:andArguments:` {#setMetaStatus}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição** Retorno de chamada disparado pelo AccessEnabler que fornece os metadados solicitados por meio de uma chamada [`getMetadata:`](#getMeta).

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

* *metadados*: os metadados solicitados. Esse valor é um `NSString` no caso de metadados estáticos (TTL de autenticação, TTL de autorização, ID do dispositivo).  É um objeto complexo ao solicitar metadados específicos do usuário. Esse objeto complexo geralmente é a representação em Objetive-C de uma carga JSON (por exemplo, &#39;{&quot;street&quot;: &quot;Main Avenue&quot;, &quot;building&quot;: [&quot;150&quot;, &quot;320&quot;]}&#39; é traduzido em Objetive-C como NSDictionary(&quot;street&quot; -> &quot;Main Avenue&quot;, &quot;building&quot; -> NSArray(&quot;150&quot;, &quot;320&quot;))).   Exemplo de objeto JSON de metadados:

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

* *criptografado*: valor booliano que especifica se os metadados recuperados estão criptografados ou não. Esse parâmetro é importante apenas para solicitações de metadados do usuário, não tem significado para metadados estáticos (por exemplo, TTL de autenticação) que são sempre recebidos não criptografados. Se esse parâmetro for definido como true, cabe ao Programador obter o valor de Metadados do Usuário não criptografado executando uma descriptografia RSA usando a chave privada da lista de permissões (a mesma chave privada usada para a assinatura da ID do Solicitante nas chamadas [`setRequestor:setSignedRequestorId:`](#setReq) e `setRequestor:setSignedRequestorId:serviceProviders: `).

* *chave*: a chave usada para formular a solicitação de recuperação de metadados.

* *argumentos*: o mesmo dicionário passado para a chamada [`getMetadata:`](#getMeta). Isso é fornecido para permitir que o aplicativo corresponda às solicitações com as respostas.

**Acionado por:** [`getMetadata:`](#getMeta)

**Mais Informações:** [Metadados de Usuário](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/user-metadata.md)


[Voltar ao início...](#apis)

</br>

### MVPD {#mvpd}

**Arquivo:** AccessEnabler/headers/model/MVPD.h

**Descrição** Descreve o objeto do MVPD. Pode ser usado para obter informações sobre as propriedades da MVPD.

**Disponibilidade:** v1.0+ [a propriedade boardingStatus está disponível na v2.2]

**Propriedades**:

* ID (NSString) - A MVPD Id.
* (NSString) displayName - O nome do MVPD. [Isso deve ser usado para exibir no seletor]
* (NSString) logoURL - O endereço do logotipo do MVPD.
* (BOOL) enablePlatformServices - Se true, o MVPD oferecerá suporte a serviços SSO como o [Apple SSO](#presentTvDialog).
* (NSString) boardingStatus - pode ter 3 valores:
   * nil - O MVPD não é compatível com o Apple SSO.
   * SELETOR - O MVPD pode aparecer no seletor de Apple, mas o fluxo de autenticação é feito pelo Adobe.
   * COMPATÍVEL - A MVPD é totalmente compatível com o Apple e usará o token SSO da Apple.

[Voltar ao início...](#apis)

</br>

## Rastreamento de eventos {#tracking}

O AccessEnabler aciona um retorno de chamada adicional que não está necessariamente relacionado aos fluxos de direito. A implementação da função de retorno de chamada [`sendTrackingData()`](#sendTracking) é opcional, mas permite que o aplicativo rastreie eventos específicos e compile estatísticas, como o número de tentativas de autenticação/autorização com êxito/falha.

### sendTrackingData:forEventType: {#sendTracking}

**Arquivo:** AccessEnabler/headers/EntitlementDelegate.h

**Descrição** Retorno de chamada disparado pelo AccessEnabler sinalizando ao aplicativo a ocorrência de vários eventos, como a conclusão/falha de fluxos de autenticação/autorização. Com a Autenticação Adobe Pass 1.6, o tipo de dispositivo, o tipo de cliente AccessEnabler e o sistema operacional são relatados por [`sendTrackingData()`](#sendTracking). O retorno de chamada [`sendTrackingData()`](#sendTracking) permanece compatível com versões anteriores.

**Retorno de chamada: eventos de rastreamento**

```
(void) sendTrackingData:(NSArray *)data 
             forEventType:(int)event;
```

**Disponibilidade:** v1.0+

**Observação:** o tipo de dispositivo e o sistema operacional são derivados do uso de uma biblioteca Java pública (<http://java.net/projects/user-agent-utils>) e da cadeia de caracteres do agente do usuário. Esteja ciente de que essas informações são fornecidas apenas como uma forma grosseira de dividir as métricas operacionais em categorias de dispositivos, mas esse Adobe não pode assumir nenhuma responsabilidade por resultados incorretos. Use a nova funcionalidade adequadamente.

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
   * **authorizationDetection:** sempre que uma solicitação de token de autorização retornar (o evento é `TRACKING_AUTHORIZATION`)
   * **authenticationDetection:** sempre que ocorrer uma verificação de autenticação (o evento é `TRACKING_AUTHENTICATION`)
   * **mvpdSelection:** quando o usuário seleciona um MVPD no formulário de seleção do MVPD (o evento é `TRACKING_GET_SELECTED_PROVIDER`)
* *dados*: dados adicionais associados ao evento relatado. Esses dados são apresentados no formato de uma lista de valores.

**Acionado por:** `checkAuthentication`, `getAuthentication`, [`getAuthentication:withData:`](#getAuthN), `checkAuthorization:`, [`checkAuthorization:withData:`](#checkAuthZ), `getAuthorization:`, [`getAuthorization:withData:`](#getAuthZ), `setSelectedProvider:`

Instruções para interpretar os valores na matriz *data*:

* Para trackingEventType `TRACKING_AUTHENTICATION:`
   * **0** - Se a solicitação de token foi bem-sucedida (true/false) e se bem-sucedida:
   * **1** - Cadeia de caracteres da ID do MVPD
   * **2** - GUID (md5 com hash)
   * **3** - Token já no cache (true/false)
   * **4** - Tipo de dispositivo
   * **5** - Tipo de cliente AccessEnabler
   * **6** - Tipo de sistema operacional

* Para trackingEventType `TRACKING_AUTHORIZATION:`
   * **0** - Se a solicitação de token foi bem-sucedida (true/false) e se bem-sucedida:
   * **1** - MVPD ID
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
