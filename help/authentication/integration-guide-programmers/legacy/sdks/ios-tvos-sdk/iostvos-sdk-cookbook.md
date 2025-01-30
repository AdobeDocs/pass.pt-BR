---
title: Guia do iOS/tvOS
description: Guia do iOS/tvOS
exl-id: 4743521e-d323-4d1d-ad24-773127cfbe42
source-git-commit: e448427ae4a36c4c6cb9f9c1cb4d0cc5c6d564ed
workflow-type: tm+mt
source-wordcount: '2424'
ht-degree: 0%

---

# Cookbook do iOS/tvOS SDK (herdado) {#iostvos-sdk-cookbook}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

## Introdução {#intro}

Este documento descreve os workflows de direito que um aplicativo de nível superior do programador pode implementar por meio das APIs expostas pela biblioteca AccessEnabler do iOS/tvOS.

A solução de direitos de autenticação da Adobe Pass para iOS/tvOS é dividida em dois domínios:

* O domínio da interface do usuário — essa é a camada de aplicativo de nível superior que implementa a interface do usuário e usa os serviços fornecidos pela biblioteca do AccessEnabler para fornecer acesso ao conteúdo restrito.

* O domínio AccessEnabler - é aqui que os workflows de direito são implementados no formato de:

   * Chamadas de rede feitas para servidores back-end do Adobe
   * Regras de lógica de negócios relacionadas aos workflows de autenticação e autorização
   * Gerenciamento de vários recursos e processamento do estado do fluxo de trabalho (como o cache de token)

O objetivo do domínio AccessEnabler é ocultar todas as complexidades dos workflows de direito e fornecer ao aplicativo de camada superior (por meio da biblioteca AccessEnabler) um conjunto de primitivos de direito simples com os quais você implementa os workflows de direito:

1. Definir a identidade do solicitante
1. Verificar e obter autenticação em relação a um provedor de identidade específico
1. Verificar e obter autorização para um recurso específico
1. Sair
1. Fluxos de SSO do Apple por meio da combinação da estrutura Apple VSA

A atividade de rede do AccessEnabler ocorre em seu próprio thread, portanto, o thread da interface do usuário nunca é bloqueado. Como resultado, o canal de comunicação bidirecional entre os dois domínios de aplicativos deve seguir um padrão totalmente assíncrono:

* A camada de aplicativo da interface envia mensagens para o domínio AccessEnabler por meio das chamadas de API expostas pela biblioteca AccessEnabler.
* O AccessEnabler responde à camada da interface do usuário por meio dos métodos de retorno de chamada incluídos no protocolo AccessEnabler que a camada da interface do usuário registra na biblioteca AccessEnabler.

## Configurar o serviço de ID do Experience Cloud (ID do visitante) {#visitorIDSetup}

Configurar o valor [ID de Experience Cloud](https://experienceleague.adobe.com/docs/id-service/using/home.html) é importante do ponto de vista [!DNL Analytics]. Depois que um valor `visitorID` é definido, o SDK envia essas informações junto com cada chamada de rede e o servidor de Autenticação [!DNL Adobe Pass] coleta essas informações. É possível correlacionar a análise do serviço de Autenticação da Adobe Pass com quaisquer outros relatórios de análise que você tenha de outros aplicativos ou sites. Informações sobre como configurar visitorID podem ser encontradas [aqui](#setOptions).

## Fluxos de Direitos {#entitlement}

A. [Pré-requisitos](#prereqs) </br>
B. [Fluxo de Inicialização](#startup_flow) </br>
C. [Fluxo de Autenticação sem Apple SSO](#authn_flow_wo_applesso) </br>
D. [Fluxo de Autenticação com Apple SSO no iOS](#authn_flow_with_applesso) </br>
E. [Fluxo de Autenticação com Apple SSO em tvOS](#authn_flow_with_applesso_tvOS) </br>
F. [Fluxo de autorização](#authz_flow) </br>
G. [Exibir Fluxo De Mídia](#media_flow) </br>
H. [Fluxo de logout sem Apple SSO](#logout_flow_wo_AppleSSO) </br>
I. [Fluxo de logout com Apple SSO](#logout_flow_with_AppleSSO) </br>


### A. Pré-requisitos {#prereqs}

1. Crie suas funções de retorno de chamada:
   * `setRequestorComplete()` </br>
   * Acionado por [setRequestor()](#$setReq), retorna êxito ou falha. </br>
   * Success indica que você pode continuar com chamadas de direito.

   * [`displayProviderDialog(mvpds)`](#$dispProvDialog) </br>
      * Disparado por [`getAuthentication()`](#$getAuthN) somente se o usuário não tiver selecionado um provedor (MVPD) e ainda não estiver autenticado. </br>
      * O parâmetro `mvpds` é uma matriz de provedores disponíveis para o usuário.

   * `setAuthenticationStatus(status, errorcode)` </br>
      * Disparado por `checkAuthentication()` toda vez. </br>
      * Disparado por [`getAuthentication()`](#$getAuthN) somente se o usuário já estiver autenticado e tiver selecionado um provedor. </br>
      * O status retornado é sucesso ou falha, o código de erro descreve o tipo da falha.

   * [`navigateToUrl(url)`](#$nav2url) </br>
      * Disparado por [`getAuthentication()`](#$getAuthN) depois que o usuário seleciona uma MVPD. O parâmetro `url` fornece o local da página de logon do MVPD.

   * `sendTrackingData(event, data)` </br>
      * Acionado por `checkAuthentication()`, [`getAuthentication()`](#$getAuthN), `checkAuthorization()`, [`getAuthorization()`](#$getAuthZ), `setSelectedProvider()`.
      * O parâmetro `event` indica qual evento de direito ocorreu; o parâmetro `data` é uma lista de valores relacionados ao evento.

   * `setToken(token, resource)`

      * Acionado por [checkAuthorization()](#checkAuthZ) e [getAuthorization()](#$getAuthZ) após uma autorização bem-sucedida para exibir um recurso.
      * O parâmetro `token` é o token de mídia de vida curta; o parâmetro `resource` é o conteúdo que o usuário está autorizado a exibir.

   * `tokenRequestFailed(resource, code, description)` </br>
      * Acionado por [checkAuthorization()](#checkAuthZ) e [getAuthorization()](#$getAuthZ) após uma autorização malsucedida.
      * O parâmetro `resource` é o conteúdo que o usuário estava tentando exibir; o parâmetro `code` é o código de erro que indica que tipo de falha ocorreu; o parâmetro `description` descreve o erro associado ao código de erro.

   * `selectedProvider(mvpd)` </br>
      * Disparado por [`getSelectedProvider()`](#getSelProv).
      * O parâmetro `mvpd` fornece informações sobre o provedor selecionado pelo usuário.

   * `setMetadataStatus(metadata, key, arguments)`
      * Acionado por `getMetadata().`
      * O parâmetro `metadata` fornece os dados específicos solicitados; o parâmetro `key` é a chave usada na solicitação [getMetadata()](#getMeta); e o parâmetro `arguments` é o mesmo dicionário passado para [getMetadata()](#getMeta).

   * [&quot;preauthorizedResources(authorizedResources)&quot;](#preauthResources)

      * Disparado por [`checkPreauthorizedResources()`](#checkPreauth).

      * O parâmetro `authorizedResources` apresenta os recursos que o usuário
está autorizado a visualizar.

   * [&quot;presentTvProviderDialog(viewController)&quot;](#presentTvDialog)

      * Disparado por [getAuthentication()](#getAuthN) quando o solicitante atual oferece suporte pelo menos a um MVPD que tem suporte para SSO.
      * O parâmetro viewController é a Caixa de Diálogo de SSO do Apple e precisa ser apresentado no controlador de exibição principal.

   * [&quot;dismissTvProviderDialog(viewController)&quot;](#dismissTvDialog)

      * Acionado por uma ação do usuário (selecionando &quot;Cancelar&quot; ou &quot;Outros provedores de TV&quot; na caixa de diálogo SSO do Apple).
      * O parâmetro viewController é a Caixa de Diálogo de SSO do Apple e precisa ser descartado do controlador de exibição principal.

![](../../../../assets/iOS-flows.png)

### B. Fluxo de inicialização {#startup_flow}

1. Iniciar o aplicativo de nível superior.</br>
1. Iniciar Autenticação Adobe Pass </br>

   a. Chame [`init`](#$init) para criar uma única instância do Adobe Pass Authentication AccessEnabler.
   * **Dependência:** Biblioteca iOS/tvOS Nativa de Autenticação do Adobe Pass (AccessEnabler)

   b. Chame `setRequestor()` para estabelecer a identidade do Programador; transmita no `requestorID` do Programador e (opcionalmente) uma matriz de pontos de acesso de Autenticação do Adobe Pass. Para tvOS você também precisará fornecer a chave pública e o segredo. Consulte a [Documentação sem clientes](#create_dev) para obter mais detalhes.

   * **Dependência:** RequestorID de Autenticação Adobe Pass Válida (Trabalhe com sua Conta de Autenticação Adobe Pass
gerente para organizar isso).

   * **Acionadores:**
     Retorno de chamada [setRequestorComplete()](#$setReqComplete).

   >[!NOTE]
   >
   >Nenhuma solicitação de direito pode ser concluída até que a identidade do solicitante seja totalmente estabelecida. Isso significa que, enquanto o [`setRequestor()`](#$setReq) ainda estiver em execução, todas as solicitações de direito subsequentes. Por exemplo, [`checkAuthentication()`](#checkAuthN) estão bloqueados.

   Você tem duas opções de implementação: uma vez que as informações de identificação do solicitante são enviadas ao servidor back-end, a camada de aplicativo da interface do usuário pode escolher uma das duas seguintes abordagens: </br>

   1. Aguarde o acionamento do retorno de chamada [`setRequestorComplete()`](#setReqComplete) (parte do delegado AccessEnabler). Essa opção fornece a maior certeza de que [`setRequestor()`](#$setReq) foi concluído, portanto, ela é recomendada para a maioria das implementações.

   1. Continue sem aguardar o acionamento do retorno de chamada [`setRequestorComplete()`](#setReqComplete) e comece a emitir solicitações de direito. Essas chamadas (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) são enfileiradas pela biblioteca AccessEnabler, que fará as chamadas de rede reais após [`setRequestor()`](#$setReq). Ocasionalmente, essa opção pode ser interrompida se, por exemplo, a conexão de rede estiver instável.

1. Chame `checkAuthentication()` para verificar uma autenticação existente sem iniciar o fluxo de Autenticação completo.  Se essa chamada for bem-sucedida, você poderá prosseguir diretamente para o Fluxo de autorização. Caso contrário, prossiga para o Fluxo de autenticação.

   * **Dependência:** uma chamada bem-sucedida para [setRequestor()](#$setReq) (essa dependência também se aplica a todas as chamadas subsequentes).

   * **Triggers:** retorno de chamada de [setAuthenticationStatus()](#$setAuthNStatus).


### C. Fluxo de autenticação sem o Apple SSO {#authn_flow_wo_applesso}

1. Chame [`getAuthentication()`](#$getAuthN) para iniciar o fluxo de autenticação ou obter a confirmação de que o usuário já está
autenticado.

   **Acionadores:**

   * O retorno de chamada [setAuthenticationStatus()](#$setAuthNStatus), se o usuário já estiver autenticado. Nesse caso, prossiga diretamente para o [Fluxo de Autorização](#authz_flow).

   * O retorno de chamada [displayProviderDialog()](#$dispProvDialog), se o usuário ainda não estiver autenticado.

1. Apresentar ao usuário a lista de provedores enviada para
   [`displayProviderDialog()`](#dispProvDialog).

1. Depois que o usuário selecionar um provedor, obtenha a URL do MVPD do usuário do retorno de chamada `navigateToUrl:` ou `navigateToUrl:useSVC:` e abra um controlador `UIWebView/WKWebView` ou `SFSafariViewController` e direcione esse controlador para a URL.

1. Através do `UIWebView/WKWebView` ou `SFSafariViewController` instanciado na etapa anterior, o usuário acessa a página de logon do MVPD e insere credenciais de logon. Várias operações de redirecionamento ocorrem no controlador.</br>

>[!NOTE]
>
>Nesse momento, o usuário tem a oportunidade de cancelar o fluxo de autenticação. Se isso ocorrer, a camada da interface do usuário será responsável por informar o AccessEnabler sobre esse evento, chamando [setSelectedProvider()](#setSelProv) com `null` como parâmetro. Isso permite que o AccessEnabler limpe seu estado interno e redefina o Fluxo de autenticação.

1. Após um logon bem-sucedido do usuário, a camada do aplicativo detecta o carregamento de um URL personalizado específico. Observe que esse URL personalizado específico é realmente inválido e não se destina ao controlador para carregá-lo. Ela deve ser interpretada somente pelo seu aplicativo como um sinal de que o fluxo de autenticação foi concluído e que é seguro fechar o controlador `UIWebView/WKWebView` ou `SFSafariViewController`. Caso seja necessário usar um controlador `SFSafariViewController`, a URL personalizada específica é definida pelo **`application's custom scheme`** (por exemplo, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`); caso contrário, essa URL personalizada específica é definida pela constante **`ADOBEPASS_REDIRECT_URL`** (ou seja, `adobepass://ios.app`).

1. Feche o controlador UIWebView/WKWebView ou SFSafariViewController e chame o método de API `handleExternalURL:url` do AccessEnabler, que instrui o AccessEnabler a recuperar o token de autenticação do servidor back-end.

1. (Opcional) Chame [`checkPreauthorizedResources(resources)`](#$checkPreauth) para verificar quais recursos o usuário está autorizado a visualizar. O parâmetro `resources` é uma matriz de recursos protegidos associada ao token de autenticação do usuário. Um uso das informações de autorização obtidas no MVPD do usuário é decorar a interface do usuário (por exemplo, símbolos bloqueados/desbloqueados ao lado de conteúdo protegido).

   * **Acionadores:** Retorno de chamada de [`preauthorizedResources()`](#preauthResources)
   * **Ponto de execução:** após a conclusão do fluxo de autenticação

1. Se a autenticação tiver sido bem-sucedida, prossiga para o Fluxo de autorização.

### D. Fluxo de autenticação com Apple SSO no iOS {#authn_flow_with_applesso}

1. Chame [`getAuthentication()`](#$getAuthN) para iniciar o fluxo de autenticação ou obter a confirmação de que o usuário já está autenticado.
   **Acionadores:**

   * O retorno de chamada [presentTvProviderDialog()](#presentTvDialog), se o usuário não estiver autenticado e o solicitante atual tiver pelo menos no MVPD com suporte para SSO. Se nenhum MVPD suportar SSO, o fluxo de autenticação clássico será usado.

1. Depois que o usuário seleciona um provedor, a biblioteca AccessEnabler obtém um token de autenticação com as informações fornecidas pela estrutura VSA da Apple.

1. O retorno de chamada [setAuthenticationStatus()](#setAuthNStatus) será disparado. Nesse ponto, o usuário deve ser autenticado com o Apple SSO.

1. [Opcional] Chame [`checkPreauthorizedResources(resources)`](#$checkPreauth) para verificar quais recursos o usuário está autorizado a visualizar. O parâmetro `resources` é uma matriz de recursos protegidos associada ao token de autenticação do usuário. Uma utilização das informações de autorização obtidas no MVPD do usuário é decorar a interface do usuário (por exemplo, símbolos bloqueados/desbloqueados ao lado de conteúdo protegido).

   * **Acionadores:** Retorno de chamada de [`preauthorizedResources()`](#preauthResources)
   * **Ponto de execução:** após a conclusão do fluxo de autenticação

1. Se a autenticação tiver sido bem-sucedida, prossiga para o Fluxo de autorização.

### E. Fluxo de autenticação com Apple SSO no tvOS {#authn_flow_with_applesso_tvOS}

1. Ligue para [`getAuthentication()`](#$getAuthN) para iniciar o
fluxo de autenticação ou para obter a confirmação de que o usuário já está
autenticado.
   **Acionadores:**
   * O retorno de chamada [`presentTvProviderDialog()`](#presentTvDialog), se o usuário não estiver autenticado e o solicitante atual tiver pelo menos no MVPD compatível com SSO. Se nenhum MVPD suportar SSO, o fluxo de autenticação clássico será usado.

1. Depois que o usuário selecionar um provedor, o retorno de chamada [`status()`](#status_callback_implementation) será chamado. Um código de registro será fornecido e a biblioteca AccessEnabler iniciará a sondagem do servidor para uma segunda autenticação de tela bem-sucedida.

1. Se o código de registro fornecido tiver sido usado para autenticação com êxito na segunda tela, o retorno de chamada [`setAuthenticatiosStatus()`](#setAuthNStatus) será acionado. Nesse ponto, o usuário deve ser autenticado com o Apple SSO.
1. [Opcional] Chame [`checkPreauthorizedResources(resources)`](#$checkPreauth) para verificar quais recursos o usuário está autorizado a visualizar. O parâmetro `resources` é uma matriz de recursos protegidos associada ao token de autenticação do usuário. Uma utilização das informações de autorização obtidas no MVPD do usuário é decorar a interface do usuário (por exemplo, símbolos bloqueados/desbloqueados ao lado de conteúdo protegido).

   * **Acionadores:** Retorno de chamada de [`preauthorizedResources()`](#preauthResources)

   * **Ponto de execução:** após a conclusão do fluxo de autenticação
1. Se a autenticação tiver sido bem-sucedida, prossiga para o Fluxo de autorização.

### F. Fluxo de autorização {#authz_flow}

1. Chame [getAuthorization()](#$getAuthZ) para iniciar o fluxo de autorização.

   * **Dependência:** ResourceID(s) válido(s) acordado(s) com a(s) MVPD(s).
   * As IDs de recursos devem ser as mesmas usadas em quaisquer outros dispositivos ou plataformas e serão as mesmas em todos os MVPDs. Para obter informações sobre IDs de Recursos, consulte [Identificador de Recursos](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier)

1. Validar autenticação e autorização.

   * Se a chamada [getAuthorization()](#$getAuthZ) tiver êxito: o usuário tem tokens AuthN e AuthZ válidos (o usuário é autenticado e autorizado a assistir à mídia solicitada).

   * Se [getAuthorization()](#$getAuthZ) falhar: examine a exceção lançada para determinar seu tipo (AuthN, AuthZ ou algo mais):
      * Se foi um erro de autenticação (AuthN), reinicie o fluxo de autenticação.
      * Se foi um erro de autorização (AuthZ), o usuário não está autorizado a assistir à mídia solicitada, e algum tipo de mensagem de erro deve ser exibido para o usuário.
      * Se houver algum outro tipo de erro (erro de conexão, erro de rede etc.), exiba uma mensagem de erro apropriada para o usuário.

1. Valide o token de mídia curta.\
   Use a biblioteca do Verificador de Token de Mídia de Autenticação do Adobe Pass para verificar o token de mídia de vida curta retornado da chamada [getAuthorization()](#$getAuthZ) acima:

   * Se a validação for bem-sucedida: Reproduza a mídia solicitada para o usuário.
   * Se a validação falhar: O token AuthZ era inválido, a solicitação de mídia deve ser recusada e uma mensagem de erro deve ser exibida ao usuário.


1. Retorne ao fluxo normal do aplicativo.

### G. Fluxo de mídia de exibição {#media_flow}

1. O usuário seleciona a mídia a ser exibida.
1. A mídia está protegida? Seu aplicativo verifica se a mídia selecionada está protegida:

   * Se a mídia selecionada estiver protegida, o aplicativo iniciará o [Fluxo de Autorização](#authz_flow) acima.

   * Se a mídia selecionada não estiver protegida, reproduzir a mídia por
o usuário.

### H. Fluxo de logout sem Apple SSO {#logout_flow_wo_AppleSSO}

1. Ligue para [`logout()`](#$logout) para desconectar o usuário. O AccessEnabler apaga todos os valores e tokens em cache. Depois de limpar o cache, o AccessEnabler faz uma chamada de servidor para limpar as sessões do lado do servidor. Como a chamada do servidor pode resultar em um redirecionamento SAML para o IdP (isso permite a limpeza da sessão no lado do IdP), essa chamada deve seguir todos os redirecionamentos. Por esse motivo, essa chamada deve ser tratada em um controlador UIWebView/WKWebView ou SFSafariViewController.

   a. Seguindo o mesmo padrão do fluxo de trabalho de autenticação, o domínio AccessEnabler faz uma solicitação à camada de aplicativo da interface do usuário, por meio do retorno de chamada `navigateToUrl:` ou `navigateToUrl:useSVC:`, para criar um controlador UIWebView/WKWebView ou SFSafariViewController e instruir esse controlador a carregar a URL fornecida no parâmetro `url` do retorno de chamada. Este é o URL do ponto de extremidade de logout no servidor back-end.

   b. Seu aplicativo deve monitorar a atividade do controlador `UIWebView/WKWebView or SFSafariViewController` e detectar o momento em que carrega um URL personalizado específico, conforme passa por vários redirecionamentos. Observe que esse URL personalizado específico é realmente inválido e não se destina ao controlador para carregá-lo. Ela deve ser interpretada somente pelo seu aplicativo como um sinal de que o fluxo de logout foi concluído e que é seguro fechar o controlador `UIWebView/WKWebView` ou `SFSafariViewController`. Quando o controlador carrega esta URL personalizada específica, seu aplicativo deve fechar o controlador `UIWebView/WKWebView or SFSafariViewController` e chamar o método de API `handleExternalURL:url` do AccessEnabler. Caso seja necessário usar um controlador `SFSafariViewController`, a URL personalizada específica é definida pelo **`application's custom scheme`** (por exemplo, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), caso contrário, essa URL personalizada específica é definida pela constante **`ADOBEPASS_REDIRECT_URL`** (ou seja, `adobepass://ios.app`).

   >[!NOTE]
   >
   >O fluxo de logout difere do fluxo de autenticação porque o usuário não precisa interagir com UIWebView/WKWebView ou SFSafariViewController de nenhuma maneira. A camada de aplicativo da interface do usuário usa UIWebView/WKWebView ou SFSafariViewController para verificar se todos os redirecionamentos estão sendo seguidos. Portanto, é possível (e recomendado) tornar o controlador invisível (ou seja, oculto) durante o processo de logout.


### I. Fluxo de logout com o Apple SSO {#logout_flow_with_AppleSSO}

1. Ligue para [`logout()`](#$logout) para desconectar o usuário.
1. O retorno de chamada [status()](#status_callback_implementation) será chamado com a id VSA203.
1. O usuário também deve ser instruído a fazer logon nas configurações do sistema. Se isso não for feito, haverá uma nova autenticação quando o aplicativo for reiniciado.



<!--
### Related Information {#related}


- [iOS API Reference](#)

- [iOS Technical Overview](#)

- [Generating Digital Certificates](#)

- [Identifying Protected Resources](#)

- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)

- [iOS Authentication error - adobepass.ios.app cannot be found (Tech
  Note)](#)
-->
