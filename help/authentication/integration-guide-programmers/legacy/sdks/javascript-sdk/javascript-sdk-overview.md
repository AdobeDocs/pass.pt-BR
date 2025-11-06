---
title: Visão geral do JavaScript SDK
description: Visão geral do JavaScript SDK
exl-id: 8756c804-a4c1-4ee3-b2b9-be45f38bdf94
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 0%

---

# Visão geral do JavaScript SDK (herdado) {#javascript-sdk-overview}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

## Introdução

A Adobe recomenda que você migre para a versão mais recente do JS v4.x da biblioteca do AccessEnabler.

A integração do JavaScript de autenticação da Adobe Pass oferece aos programadores uma solução TV em todos os lugares no ambiente familiar de desenvolvimento de aplicativos Web JS. Os principais componentes da integração são seu aplicativo de &quot;alto nível&quot; (interação do usuário, apresentação de vídeo) e a biblioteca de &quot;baixo nível&quot; AccessEnabler fornecida pela Adobe, que fornece sua entrada para os fluxos de direitos e lida com a comunicação com os servidores de autenticação da Adobe Pass.

As seções a seguir fornecem descrições e exemplos específicos para a integração do JavaScript AccessEnabler.

>[!IMPORTANT]
>
>Este documento descreve a implementação de uma solução Web para desktop. A biblioteca do JavaScript não é compatível com plataformas móveis (por exemplo, Safari no iOS, Chrome no Android). Use nossos SDKs nativos se quiser direcionar a uma plataforma móvel (iOS, Android, Windows).

## Criando a caixa de diálogo de seleção do MVPD {#creating-the-mvpd-selection-dialog}

Para que um usuário faça logon em sua MVPD e se torne autenticado, sua página ou reprodutor deve fornecer uma maneira de o usuário identificar sua MVPD. Uma versão padrão de uma caixa de diálogo de seleção do MVPD é fornecida para desenvolvimento. Para uso em produção, você deve implementar seu próprio seletor de MVPD.

Se você já souber quem é o provedor do cliente, poderá [definir o MVPD de forma programática](/help/authentication/home.md), sem interação com o usuário. A técnica é a mesma, mas ignora a etapa de chamar a caixa de diálogo Seletor de provedor e pedir ao cliente para selecionar sua MVPD.

## Exibindo o Provedor de Serviços {#displaying-the-service-provider}

A amostra de código a seguir demonstra como descobrir e exibir o provedor de serviços do cliente atual:

**HTML** - Esta página adiciona uma seção à página que exibe o provedor escolhido pelo cliente, se ele já estiver conectado:

```HTML
    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
            "http://www.w3.org/TR/html4/loose.dtd"> 
    <html>
    <head>
        <title>Get the distributor that is used for the authentication</title>
        <script type="text/javascript" src="libs/jquery-1.4.4.min.js"></script>
        <script type="text/javascript" src="libs/swfobject.js"></script>
        <script type="text/javascript" src="scripts/sample6.js"></script>
    </head>
    <body>
        <div id="alternative">
        <a href="http://www.adobe.com/go/getflashplayer_br"> 
            <img src="http://www.adobe.com/images/shared/download_buttons/get_flash_player.gif" 
                 alt="Get Adobe Flash player"/> </a>
        </div> 
        <div id="user_authenticated">Please wait while we verify your authentication status</div> 
        <div id="control">
        <button id="sign_in" style="display:none;">Sign in</button>
        <button id="sign_out" style="display:none;">Sign out</button>
        </div> 
        <div id="distributor"></div> 
        <div id="provider_selector" style="display:none;">
        <select id="provider_list"></select>
        <button id="login">Login</button>
        <button id="cancel">Cancel</button>
        </div> 
        <div id="video_player">This is a video player showing an image as a preview.  You are not yet authorized to see this content.  Make sure that you first sign in.
        </div> 
        <div id="get_authorization">
        <button id="request_access" style="display:none;">Request access</button>
        </div> 
    </body>
    </html>
```


**JavaScript** Este arquivo do JavaScript consulta o Habilitador de Acesso para o provedor atual se o usuário já estiver conectado e exibe o resultado na seção da página reservada para ele. Também implementa uma caixa de diálogo do seletor do MVPD:

```JS
    $(function() {
        embedAE();
    }); 
    function embedAE() {
        var flashvars = {};
        var params = {
            bgcolor: "#869ca7",
            quality: "high",
            allowscriptaccess: "always"
        };
        var attributes = {
            id: "ae_swf",
            align: "middle",
            name: "ae_swf"
        };
        swfobject.embedSWF(
                "https://entitlement.auth-staging.adobe.com/entitlement/AccessEnabler.swf",      
                "alternative", "1", "1", "10.1.53", "", flashvars, params, attributes);
    } 
    function getAE() {
        return document.getElementById("ae_swf");
    } 
    function swfLoaded() {
        var swf = getAE();
        initBindings();
        // don't load the default provider-selection dialog 
        swf.setProviderDialogURL("none");
        // identify yourself 
        swf.setRequestor("sample_requestor_Id");
        swf.checkAuthentication();
    } 
    // Define the button actions for the provider dialog
    function initBindings() {
        var provider_selector = $('#provider_selector');
        var sign_in = $('#sign_in');
        var sign_out = $('#sign_out');
        var login = $('#login');
        var cancel = $('#cancel');
        var request_access = $('#request_access'); 
        sign_out.click(function() {
            getAE().logout();
        });
        sign_in.click(function() {
            getAE().getAuthentication();
        }); 
        login.click(function() {
            var selected_mvpd_id = $("#provider_list option:selected").val();
            getAE().setSelectedProvider(selected_mvpd_id);
        }); 
        cancel.click(function() {
            getAE().setSelectedProvider(null);
            provider_selector.hide();
        }); 
        request_access.click(function(){
            getAE().getAuthorization("sample_requestor_Id");
        });
    }
    // Called if user is already authenticated; queries AE to find the current user's MVPD, 
    // Adds the result to the UI 
    function setDistributor() {
        var distributor = $("#distributor");
        var selectedProvider = getAE().getSelectedProvider();
        distributor.replaceWith('<div id="distributor">' +
                                'Courtesy of ' +
                                selectedProvider.MVPD +
                                '</div>');
    } 
    // Adjust the contents of the provider dialog, based on authentication status 
    // Adds the call to check and display the current distributor, if the user is already
    // logged in. 
    function setAuthenticationStatus(isAuthenticated, errorCode) {
        var user_authenticated = $("#user_authenticated");
        var video_player = $("#video_player");
        var sign_in = $('#sign_in');
        var sign_out = $('#sign_out');
        var request_access = $('#request_access'); 
        if (isAuthenticated == 1) {
            setDistributor();
            user_authenticated.replaceWith('<div id="user_authenticated">
                                           You are authenticated - if you wish you can sign out
                                           </div>');
            sign_out.show();
            sign_in.hide();
            video_player.replaceWith('<div id="video_player">Now you can ask for access.</div>');
            request_access.show();
        } else {
            user_authenticated.replaceWith('<div id="user_authenticated">
                                           You are not authenticated please sign in
                                           </div>');
            sign_in.show();
            sign_out.hide();
        }
    } 
    // Allow user to choose a provider 
    function displayProviderDialog(providers) {
        var provider_list = $("#provider_list");
        var options = provider_list.attr('options');
        for (var index in providers) {
            options[index] = new Option(providers[index].displayName,
                                        providers[index].ID);
        }
        //select the first one by default 
        if (!$("#provider_list option:selected").length)
            $("#provider_list option[0]").attr('selected', 'selected'); 
        $('#provider_selector').show();
    } 
    // Handle the authorization response by sending token to video player 
    function setToken(requested_resource_id, token) {
        var video_player = $("#video_player");
        var request_access = $("#request_access");
        video_player.replaceWith('<div id="video_player">Now you are authorized to ' +
                                 requested_resource_id +
                                 ' content with ' + token + ' . </div>');
    }
```

## Efetuando logout {#logout}

Chame `logout()` para iniciar o processo de logout. Esse método não aceita argumentos. Ele faz logoff do usuário atual, limpando todas as informações de autenticação e autorização para esse usuário e excluindo todos os tokens AuthN e AuthZ do sistema local.

Há alguns casos em que o reprodutor não é responsável por gerenciar logouts de usuário:



- **Quando o logout é iniciado de um site que não está integrado à Autenticação Adobe Pass.** Nesse caso, o MVPD pode invocar o serviço de Logout único da autenticação da Adobe Pass por meio de um redirecionamento do navegador. (No momento, não há suporte para invocar o SLO por meio de uma chamada backchannel.)

>[!NOTE]
>
>Se o usuário deixar a máquina ociosa por tempo suficiente para que os tokens expirem, ainda poderá retornar à sessão e iniciar o logout com êxito. A autenticação da Adobe Pass garante que todos os tokens sejam excluídos e notifica a MVPD para excluir sua sessão.

O código JavaScript a seguir demonstra o logout (desautenticação) de um usuário autenticado no momento:

```JS
    [...]
    /*
     * @param isAuthenticated authentication status: 1 - Authenticated, 0 - not authenticated 
     * @param errorCode Any error that occurred when determining the authentication status.
     * An empty string if no error occurred.
     */
    function setAuthenticationStatus(isAuthenticated, errorCode) {
        if (isAuthenticated == 1 ) {
            /* User is authenticated – we can logout / deauthenticate */
            accessEnablerObject.logout();
            /* Logs out the current user, clearing all authentication
             * and authorization information for that user. Deletes
             * all authN and authZ tokens from the user's system.
             */
        } else {
            /*
             * User is NOT authenticated – we do not need to logout;
             * Show the log-in image/button/message
             */
        }
    }
```

<!--
**Related Information**

- [JavaScript SDK Cookbook](/help/authentication/javascript-sdk-cookbook.md)
- [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
- [JavaScript Code Samples](#javascript-code-samples)
- [Understanding Tokens](#understanding_tokens)
-->
