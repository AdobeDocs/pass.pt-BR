---
title: Guia do iOS/tvOS
description: Guia do iOS/tvOS
exl-id: 4743521e-d323-4d1d-ad24-773127cfbe42
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '2404'
ht-degree: 0%

---

# Guia do SDK do iOS/tvOS {#iostvos-sdk-cookbook}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

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

Configuração do [ID do Experience Cloud](https://experienceleague.adobe.com/docs/id-service/using/home.html) o valor é importante do [!DNL Analytics] ponto de vista. Uma vez a `visitorID` for definido, o SDK enviará essas informações juntamente com cada chamada de rede e a variável [!DNL Adobe Pass] O servidor de autenticação coleta essas informações. É possível correlacionar a análise do serviço de Autenticação da Adobe Pass com quaisquer outros relatórios de análise que você tenha de outros aplicativos ou sites. É possível encontrar informações sobre como configurar a visitorID [aqui](#setOptions).

## Fluxos de Direitos {#entitlement}

A.  [Pré-requisitos](#prereqs) </br>
B.  [Fluxo de inicialização](#startup_flow) </br>
C  [Fluxo de autenticação sem o Apple SSO](#authn_flow_wo_applesso)  </br>
D.  [Fluxo de autenticação com o Apple SSO no iOS](#authn_flow_with_applesso) </br>
E  [Fluxo de autenticação com Apple SSO no tvOS](#authn_flow_with_applesso_tvOS) </br>
F  [Fluxo de autorização](#authz_flow) </br>
G.  [Exibir fluxo de mídia](#media_flow) </br>
H.  [Fluxo de logout sem Apple SSO](#logout_flow_wo_AppleSSO) </br>
I.  [Fluxo de logout com Apple SSO](#logout_flow_with_AppleSSO) </br>


### A. Pré-requisitos {#prereqs}

1. Crie suas funções de retorno de chamada:
   * `setRequestorComplete()` </br>
   * Acionado por [setRequestor()](#$setReq), retorna sucesso ou falha. </br>
   * Success indica que você pode continuar com chamadas de direito.

   * [`displayProviderDialog(mvpds)`](#$dispProvDialog) </br>
      * Acionado por [`getAuthentication()`](#$getAuthN) somente se o usuário não tiver selecionado um provedor (MVPD) e ainda não estiver autenticado. </br>
      * A variável `mvpds` é uma matriz de provedores disponíveis para o usuário.

   * `setAuthenticationStatus(status, errorcode)` </br>
      * Acionado por `checkAuthentication()` sempre. </br>
      * Acionado por [`getAuthentication()`](#$getAuthN) somente se o usuário já estiver autenticado e tiver selecionado um provedor. </br>
      * O status retornado é sucesso ou falha, o código de erro descreve o tipo da falha.

   * [`navigateToUrl(url)`](#$nav2url) </br>
      * Acionado por [`getAuthentication()`](#$getAuthN) após o usuário selecionar um MVPD. A variável `url` fornece a localização da página de logon do MVPD.

   * `sendTrackingData(event, data)` </br>
      * Acionado por `checkAuthentication()`, [`getAuthentication()`](#$getAuthN), `checkAuthorization()`, [`getAuthorization()`](#$getAuthZ), `setSelectedProvider()`.
      * A variável `event` indica qual evento de direito ocorreu; a variável `data` parameter é uma lista de valores relacionados ao evento.

   * `setToken(token, resource)`

      * Acionado por [checkAuthorization()](#checkAuthZ) e [getAuthorization()](#$getAuthZ) após uma autorização bem-sucedida para visualizar um recurso.
      * A variável `token` é o token de mídia de vida curta; a variável `resource` parâmetro é o conteúdo que o usuário está autorizado a visualizar.

   * `tokenRequestFailed(resource, code, description)` </br>
      * Acionado por [checkAuthorization()](#checkAuthZ) e [getAuthorization()](#$getAuthZ) após uma autorização malsucedida.
      * A variável `resource` é o conteúdo que o usuário estava tentando visualizar; a variável `code` é o código de erro que indica que tipo de falha ocorreu; a variável `description` descreve o erro associado ao código de erro.

   * `selectedProvider(mvpd)` </br>
      * Acionado por [`getSelectedProvider()`](#getSelProv).
      * A variável `mvpd` O parâmetro fornece informações sobre o provedor selecionado pelo usuário.

   * `setMetadataStatus(metadata, key, arguments)`
      * Acionado por `getMetadata().`
      * A variável `metadata` fornece os dados específicos solicitados; a variável `key` é a chave usada na variável [getMetadata()](#getMeta) pedido; e a `arguments` é o mesmo dicionário que foi passado para [getMetadata()](#getMeta).

   * [&quot;preauthorizedResources(authorizedResources)&quot;](#preauthResources)

      * Acionado por [`checkPreauthorizedResources()`](#checkPreauth).

      * A variável `authorizedResources` Este parâmetro apresenta os recursos que o usuário está autorizado a visualizar.

   * [&quot;presentTvProviderDialog(viewController)&quot;](#presentTvDialog)

      * Acionado por [getAuthentication()](#getAuthN) quando o solicitante atual suporta pelo menos um MVPD que tenha suporte para SSO.
      * O parâmetro viewController é a Caixa de Diálogo de SSO do Apple e precisa ser apresentado no controlador de exibição principal.

   * [&quot;dismissTvProviderDialog(viewController)&quot;](#dismissTvDialog)

      * Acionado por uma ação do usuário (selecionando &quot;Cancelar&quot; ou &quot;Outros provedores de TV&quot; na caixa de diálogo SSO do Apple).
      * O parâmetro viewController é a Caixa de Diálogo de SSO do Apple e precisa ser descartado do controlador de exibição principal.

![](assets/iOS-flows.png)

### B. Fluxo de inicialização {#startup_flow}

1. Inicie o aplicativo de nível superior.</br>
1. Iniciar autenticação do Adobe Pass </br>

   a. Chame [`init`](#$init) para criar uma única instância do Adobe Pass Authentication AccessEnabler.
   * **Dependência:** Biblioteca iOS/tvOS nativa de autenticação da Adobe Pass (AccessEnabler)

   b. Chame `setRequestor()` para estabelecer a identidade do Programador; transmita no site do Programador `requestorID` e (opcionalmente) uma matriz de endpoints de Autenticação do Adobe Pass. Para tvOS você também precisará fornecer a chave pública e o segredo. Consulte [Documentação sem cliente](#create_dev) para obter detalhes.

   * **Dependência:** RequestorID de autenticação válida do Adobe Pass (trabalhe com seu gerente de conta de autenticação da Adobe Pass para organizar isso).

   * **Acionadores:**
     [setRequestorComplete()](#$setReqComplete) retorno de chamada.

   >[!NOTE]
   >
   >Nenhuma solicitação de direito pode ser concluída até que a identidade do solicitante seja totalmente estabelecida. Isto significa efetivamente que, embora [`setRequestor()`](#$setReq)  ainda estiver em execução, todas as solicitações de direito subsequentes. Por exemplo, [`checkAuthentication()`](#checkAuthN) estão bloqueados.

   Você tem duas opções de implementação: depois que as informações de identificação do solicitante são enviadas ao servidor de backend, a camada de aplicativo da interface do usuário pode escolher uma das duas abordagens a seguir: </br>

   1. Aguarde o acionamento do [`setRequestorComplete()`](#setReqComplete) retorno de chamada (parte do delegado AccessEnabler). Essa opção oferece a maior certeza de que [`setRequestor()`](#$setReq) concluído, portanto, é recomendado para a maioria das implementações.

   1. Continuar sem esperar o acionamento do [`setRequestorComplete()`](#setReqComplete) retorno de chamada e comece a emitir solicitações de direito. Essas chamadas (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) são enfileiradas pela biblioteca AccessEnabler, que fará as chamadas de rede reais após o [`setRequestor()`](#$setReq). Ocasionalmente, essa opção pode ser interrompida se, por exemplo, a conexão de rede estiver instável.

1. Chame `checkAuthentication()` para verificar uma autenticação existente sem iniciar o fluxo de Autenticação completa.  Se essa chamada for bem-sucedida, você poderá prosseguir diretamente para o Fluxo de autorização. Caso contrário, prossiga para o Fluxo de autenticação.

   * **Dependência:** Uma chamada bem-sucedida para [setRequestor()](#$setReq) (essa dependência também se aplica a todas as chamadas subsequentes).

   * **Acionadores:** [setAuthenticationStatus()](#$setAuthNStatus) retorno de chamada.


### C. Fluxo de autenticação sem o Apple SSO {#authn_flow_wo_applesso}

1. Chame [`getAuthentication()`](#$getAuthN) para iniciar o fluxo de autenticação ou obter a confirmação de que o usuário já está autenticado.

   **Acionadores:**

   * A variável [setAuthenticationStatus()](#$setAuthNStatus) retorno de chamada, se o usuário já estiver autenticado. Nesse caso, prossiga diretamente para o [Fluxo de autorização](#authz_flow).

   * A variável [displayProviderDialog()](#$dispProvDialog) retorno de chamada, se o usuário ainda não estiver autenticado.

1. Apresentar ao usuário a lista de provedores enviada para
   [`displayProviderDialog()`](#dispProvDialog).

1. Depois que o usuário selecionar um provedor, obtenha o URL do MVPD do usuário no `navigateToUrl:` ou `navigateToUrl:useSVC:` retorno de chamada e abrir um `UIWebView/WKWebView` ou `SFSafariViewController` e direcione essa controladora para o URL.

1. Por meio da `UIWebView/WKWebView` ou `SFSafariViewController` instanciado na etapa anterior, o usuário acessa a página de logon do MVPD e insere credenciais de logon. Várias operações de redirecionamento ocorrem dentro do controlador.</br>

>[!NOTE]
>
>Nesse momento, o usuário tem a oportunidade de cancelar o fluxo de autenticação. Se isso ocorrer, a camada da interface do usuário será responsável por informar o AccessEnabler sobre esse evento, chamando [setSelectedProvider()](#setSelProv) com `null` como parâmetro. Isso permite que o AccessEnabler limpe seu estado interno e redefina o Fluxo de autenticação.

1. Após um logon bem-sucedido do usuário, a camada do aplicativo detecta o carregamento de um URL personalizado específico. Observe que esse URL personalizado específico é realmente inválido e não se destina ao controlador para carregá-lo. Ele só deve ser interpretado pelo aplicativo como um sinal de que o fluxo de autenticação foi concluído e que é seguro fechar o `UIWebView/WKWebView` ou `SFSafariViewController` controlador. No caso de uma `SFSafariViewController`deve ser usado o URL personalizado específico é definido pelo parâmetro **`application's custom scheme`** (por exemplo,`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), caso contrário, esse URL personalizado específico será definido pela variável **`ADOBEPASS_REDIRECT_URL`** constante (ou seja, `adobepass://ios.app`).

1. Feche o controlador UIWebView/WKWebView ou SFSafariViewController e chame AccessEnabler&#39;s `handleExternalURL:url` Método de API, que instrui o AccessEnabler a recuperar o token de autenticação do servidor back-end.

1. (Opcional) Chame [`checkPreauthorizedResources(resources)`](#$checkPreauth) para verificar quais recursos o usuário está autorizado a visualizar. A variável `resources` é uma matriz de recursos protegidos associada ao token de autenticação do usuário. Um uso das informações de autorização obtidas do MVPD do usuário é decorar sua interface do usuário (por exemplo, símbolos bloqueados/desbloqueados ao lado de conteúdo protegido).

   * **Acionadores:** [`preauthorizedResources()`](#preauthResources) retorno de chamada
   * **Ponto de execução:** Após a conclusão do fluxo de autenticação

1. Se a autenticação tiver sido bem-sucedida, prossiga para o Fluxo de autorização.

### D. Fluxo de autenticação com Apple SSO no iOS {#authn_flow_with_applesso}

1. Chame [`getAuthentication()`](#$getAuthN) para iniciar o fluxo de autenticação ou obter a confirmação de que o usuário já está autenticado.
   **Acionadores:**

   * A variável [presentTvProviderDialog()](#presentTvDialog) retorno de chamada, se o usuário não estiver autenticado e o solicitante atual tiver pelo menos um MVPD compatível com SSO. Se nenhum MVPD suportar SSO, o fluxo de autenticação clássico será usado.

1. Depois que o usuário seleciona um provedor, a biblioteca AccessEnabler obtém um token de autenticação com as informações fornecidas pela estrutura VSA da Apple.

1. A variável [setAuthenticationStatus()](#setAuthNStatus) O retorno de chamada será disparado. Nesse ponto, o usuário deve ser autenticado com o Apple SSO.

1. [Opcional] Chame [`checkPreauthorizedResources(resources)`](#$checkPreauth) para verificar quais recursos o usuário está autorizado a visualizar. A variável `resources` é uma matriz de recursos protegidos associada ao token de autenticação do usuário. Um uso das informações de autorização obtidas do MVPD do usuário é decorar sua interface do usuário (por exemplo, símbolos bloqueados/desbloqueados ao lado de conteúdo protegido).

   * **Acionadores:** [`preauthorizedResources()`](#preauthResources) retorno de chamada
   * **Ponto de execução:** Após a conclusão do fluxo de autenticação

1. Se a autenticação tiver sido bem-sucedida, prossiga para o Fluxo de autorização.

### E. Fluxo de autenticação com Apple SSO no tvOS {#authn_flow_with_applesso_tvOS}

1. Chame [`getAuthentication()`](#$getAuthN) para iniciar o fluxo de autenticação ou obter a confirmação de que o usuário já está autenticado.
   **Acionadores:**
   * A variável [`presentTvProviderDialog()`](#presentTvDialog) retorno de chamada, se o usuário não estiver autenticado e o solicitante atual tiver pelo menos um MVPD compatível com SSO. Se nenhum MVPD suportar SSO, o fluxo de autenticação clássico será usado.

1. Depois que o usuário seleciona um provedor, a variável [`status()`](#status_callback_implementation) O retorno de chamada será chamado. Um código de registro será fornecido e a biblioteca AccessEnabler iniciará a sondagem do servidor para uma segunda autenticação de tela bem-sucedida.

1. Se o código de registro fornecido foi usado para a autenticação bem-sucedida na segunda tela, a variável [`setAuthenticatiosStatus()`](#setAuthNStatus) O retorno de chamada será disparado. Nesse ponto, o usuário deve ser autenticado com o Apple SSO.
1. [Opcional] Chame [`checkPreauthorizedResources(resources)`](#$checkPreauth) para verificar quais recursos o usuário está autorizado a visualizar. A variável `resources` é uma matriz de recursos protegidos associada ao token de autenticação do usuário. Um uso das informações de autorização obtidas do MVPD do usuário é decorar sua interface do usuário (por exemplo, símbolos bloqueados/desbloqueados ao lado de conteúdo protegido).

   * **Acionadores:** [`preauthorizedResources()`](#preauthResources) retorno de chamada

   * **Ponto de execução:** Após a conclusão do fluxo de autenticação
1. Se a autenticação tiver sido bem-sucedida, prossiga para o Fluxo de autorização.

### F. Fluxo de autorização {#authz_flow}

1. Chame [getAuthorization()](#$getAuthZ) para iniciar o fluxo de autorização.

   * **Dependência:** ResourceID(s) válido(s) acordado(s) com o MVPD(s).
   * As IDs de recursos devem ser as mesmas usadas em quaisquer outros dispositivos ou plataformas e serão as mesmas em todos os MVPDs. Para obter informações sobre IDs de recursos, consulte [Identificação de recursos protegidos](/help/authentication/identify-protected-resources.md)

1. Validar autenticação e autorização.

   * Se a variável [getAuthorization()](#$getAuthZ) Chamada bem-sucedida: o usuário tem tokens AuthN e AuthZ válidos (o usuário é autenticado e autorizado a assistir à mídia solicitada).

   * Se [getAuthorization()](#$getAuthZ) failed: examine a exceção lançada para determinar seu tipo (AuthN, AuthZ ou algo mais):
      * Se foi um erro de autenticação (AuthN), reinicie o fluxo de autenticação.
      * Se foi um erro de autorização (AuthZ), o usuário não está autorizado a assistir à mídia solicitada, e algum tipo de mensagem de erro deve ser exibido para o usuário.
      * Se houver algum outro tipo de erro (erro de conexão, erro de rede etc.) em seguida, exiba uma mensagem de erro apropriada para o usuário.

1. Valide o token de mídia curta.\
   Use a biblioteca Verificador de token de mídia de autenticação da Adobe Pass para verificar o token de mídia de vida curta retornado do [getAuthorization()](#$getAuthZ) chame acima:

   * Se a validação for bem-sucedida: Reproduza a mídia solicitada para o usuário.
   * Se a validação falhar: O token AuthZ era inválido, a solicitação de mídia deve ser recusada e uma mensagem de erro deve ser exibida ao usuário.


1. Retorne ao fluxo normal do aplicativo.

### G. Fluxo de mídia de exibição {#media_flow}

1. O usuário seleciona a mídia a ser exibida.
1. A mídia está protegida? Seu aplicativo verifica se a mídia selecionada está protegida:

   * Se a mídia selecionada estiver protegida, o aplicativo iniciará o [Fluxo de autorização](#authz_flow) acima.

   * Se a mídia selecionada não estiver protegida, reproduza a mídia para o usuário.

### H. Fluxo de logout sem Apple SSO {#logout_flow_wo_AppleSSO}

1. Chame [`logout()`](#$logout) para desconectar o usuário. O AccessEnabler apaga todos os valores e tokens em cache. Depois de limpar o cache, o AccessEnabler faz uma chamada de servidor para limpar as sessões do lado do servidor. Como a chamada do servidor pode resultar em um redirecionamento SAML para o IdP (isso permite a limpeza da sessão no lado do IdP), essa chamada deve seguir todos os redirecionamentos. Por esse motivo, essa chamada deve ser tratada em um controlador UIWebView/WKWebView ou SFSafariViewController.

   a. Seguindo o mesmo padrão do workflow de autenticação, o domínio AccessEnabler faz uma solicitação à camada de aplicativo da interface do usuário, por meio da `navigateToUrl:` ou `navigateToUrl:useSVC:` callback, para criar um controlador UIWebView/WKWebView ou SFSafariViewController e instruir que carregue o URL fornecido no comando `url` parâmetro. Este é o URL do ponto de extremidade de logout no servidor back-end.

   b. Seu aplicativo deve monitorar a atividade do `UIWebView/WKWebView or SFSafariViewController` e detecte o momento em que carrega um URL personalizado específico, conforme passa por vários redirecionamentos. Observe que esse URL personalizado específico é realmente inválido e não se destina ao controlador para carregá-lo. Ele só deve ser interpretado pelo aplicativo como um sinal de que o fluxo de logout foi concluído e que é seguro fechar o `UIWebView/WKWebView` ou `SFSafariViewController` controlador. Quando o controlador carrega este URL personalizado específico, seu aplicativo deve fechar o `UIWebView/WKWebView or SFSafariViewController` controladora e chamar o AccessEnabler&#39;s `handleExternalURL:url`método da API. No caso de uma `SFSafariViewController`deve ser usado o URL personalizado específico é definido pelo parâmetro **`application's custom scheme`** (por exemplo, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), caso contrário, esse URL personalizado específico será definido pela variável **`ADOBEPASS_REDIRECT_URL`**  constante (ou seja, `adobepass://ios.app`).

   >[!NOTE]
   >
   >O fluxo de logout difere do fluxo de autenticação porque o usuário não precisa interagir com UIWebView/WKWebView ou SFSafariViewController de nenhuma maneira. A camada de aplicativo da interface do usuário usa UIWebView/WKWebView ou SFSafariViewController para verificar se todos os redirecionamentos estão sendo seguidos. Portanto, é possível (e recomendado) tornar o controlador invisível (ou seja, oculto) durante o processo de logout.


### I. Fluxo de logout com o Apple SSO {#logout_flow_with_AppleSSO}

1. Chame [`logout()`](#$logout) para desconectar o usuário.
1. A variável [status()](#status_callback_implementation) o retorno de chamada será chamado com a id VSA203.
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
