---
title: Temp pass
description: Temp pass
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '2243'
ht-degree: 0%

---

# Temp pass {#temp-pass}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Resumo dos recursos {#tempass-featur-summary}

O Temp Pass permite que os programadores ofereçam acesso temporário a seu conteúdo protegido, para usuários que não têm credenciais de conta em uma MVPD.  O Temp Pass inclui os seguintes recursos:

* O Temp Pass pode ser configurado para fornecer acesso temporário para cobrir uma variedade de cenários, incluindo os seguintes:
   * Um Programador pode oferecer uma visualização diária e curta (por exemplo, uma visualização de 10 minutos) de um de seus sites.
   * Um programador pode oferecer uma única e longa apresentação (por exemplo, quatro horas) de um grande evento esportivo, como as Olimpíadas, ou a Loucura de março da NCAA.
   * Um Programador pode fornecer uma combinação dos dois cenários anteriores; por exemplo, um período inicial de visualização mais longo em um dia, seguido por uma série de períodos curtos que se repetem diariamente por algum número de dias subsequentes.
* Os programadores especificam a duração (Tempo de vida útil ou TTL) de seu passe temporário.
* Operação Temp Pass por solicitante.  Por exemplo, a NBC poderia configurar um passe Temp de 4 horas para o solicitante &quot;NBCOlympics&quot;.
* Os programadores podem redefinir todos os tokens concedidos a um solicitante específico.  A &quot;MVPD temporária&quot; usada para implementar a passagem temporária deve ser configurada com a opção &quot;Autenticação por solicitante&quot; ativada.
* **O acesso de Temp Pass é concedido a usuários individuais em dispositivos específicos**. Depois que o acesso de Aprovação Temporária expira para um usuário, ele não poderá obter acesso temporário no mesmo dispositivo até que o token de autorização expirado do usuário seja limpo no servidor de Autenticação do Adobe Pass.


>[!NOTE]
>
>O Temp Pass faz parte do pacote de Fluxo de trabalho Premium. Entre em contato com seu representante de vendas da Adobe Pass, se estiver interessado em usar essa funcionalidade.

## Detalhes do recurso {#tempass-featur-details}

* **Como o Tempo de Exibição é Calculado** - O tempo que uma Aprovação Temporária permanece válida não está correlacionado com o tempo que um usuário passa visualizando o conteúdo no aplicativo do Programador.  Após a solicitação inicial do usuário para autorização por meio da Aprovação de Temp, uma hora de expiração é calculada adicionando a hora da solicitação atual inicial ao TTL especificado pelo Programador. Esse tempo de expiração está associado à ID do dispositivo do usuário e à ID do solicitante do programador, e armazenado no banco de dados de autenticação do Adobe Pass. Sempre que o usuário tentar acessar o conteúdo usando a Aprovação Temporária do mesmo dispositivo, a Autenticação do Adobe Pass comparará o tempo de solicitação do servidor com o tempo de expiração associado à ID de dispositivo do usuário e à ID do solicitante do Programador. Se o tempo de solicitação do servidor for menor que o tempo de expiração, a autorização será concedida; caso contrário, a autorização será negada.
* **Parâmetros de Configuração** - Os seguintes parâmetros de Temp Pass podem ser especificados por um Programador para criar uma regra de Temp Pass:
   * **TTL do token** - O tempo que um usuário pode assistir sem entrar em uma MVPD. Esse horário é baseado em relógio e expira independentemente de o usuário estar assistindo ao conteúdo ou não.
  >[!NOTE]
  >Uma ID de solicitante não pode ter mais de uma regra de senha temporária associada a ela.
* **Autenticação/Autorização** - No fluxo de Aprovação Temporária, especifique a MVPD como &quot;Aprovação Temporária&quot;.  A autenticação do Adobe Pass não se comunica com um MVPD real no fluxo de aprovação temporária, portanto, o MVPD de &quot;aprovação temporária&quot; autoriza qualquer recurso. Os programadores podem especificar um recurso que seja acessível usando o Temp Pass da mesma forma que fazem para o restante dos recursos em seu site. A Biblioteca de Verificador de Mídia pode ser usada como de costume para verificar o token de mídia curto de Aprovação Temporária e impor a verificação de recursos antes da reprodução.
* **Dados de Rastreamento no Fluxo de Aprovação Temp** - Dois pontos relacionados aos dados de rastreamento durante um fluxo de qualificação de Aprovação Temp:
   * A ID de Rastreamento transmitida da Autenticação Adobe Pass para o retorno de chamada **sendTrackingData()** é um hash da ID do Dispositivo.
   * Como a MVPD ID usada no fluxo de Temp Pass é &quot;Temp Pass&quot;, essa mesma MVPD ID é passada de volta para **sendTrackingData()**. Provavelmente, a maioria dos programadores desejará tratar as métricas de Aprovação temporária de forma diferente das métricas reais do MVPD. Isso requer algum trabalho adicional na implementação do Analytics.

A ilustração a seguir mostra o fluxo Aprovação Temporária:

![O Fluxo de Passagem Temporária](../../../assets/temp-pass-flow.png)

*Figura: O fluxo de passagem temporária*

## Implementar aprovação temporária {#implement-tempass}

No lado da Autenticação do Adobe Pass, o Temp Pass é implementado com a adição de um pseudo-MVPD chamado &quot;TempPass&quot; à configuração de servidor do Programador participante.  Esse pseudo MVPD atua como um MVPD real que concede acesso temporário ao conteúdo protegido do Programador.

No lado do Programador, o Temp Pass é implementado da seguinte maneira para os dois cenários que os MVPDs usam para autenticação:

* **iFrame na página do Programador**. O Temp Pass funciona independentemente do tipo de autenticação de um MVPD, mas para o cenário de iFrame, são necessárias etapas adicionais para cancelar o fluxo de autenticação atual e autenticar com o Temp Pass. Estas etapas são mostradas no [Logon do iFrame](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md) abaixo.
* **Redirecionar para a página de logon do MVPD**. No caso mais tradicional em que a interface do usuário para acionar o Temp Pass é apresentada antes de iniciar a autenticação com uma MVPD, não há etapas especiais a serem executadas. O Temp Pass deve ser tratado como um MVPD comum.

Os seguintes pontos se aplicam a ambos os cenários de implementação:

* O &quot;Aprovação temporária&quot; deve ser exibido no seletor de MVPD somente para usuários que ainda não solicitaram uma autorização de Aprovação temporária. Bloquear a exibição para solicitações subsequentes pode ser feito mantendo um sinalizador nos cookies. Isso funcionará desde que o usuário não limpe o cache do navegador. Se os usuários limparem os caches do navegador, &quot;Aprovação temporária&quot; aparecerá novamente no seletor e o usuário poderá solicitá-lo novamente. O acesso será concedido somente se a hora &quot;Aprovação temporária&quot; ainda não tiver expirado.
* Quando um usuário solicita acesso por meio da Aprovação temporária, o Servidor de autenticação da Adobe Pass não executará sua solicitação usual de SAML (Security Assertion Markup Language) durante o processo de autenticação. Em vez disso, o endpoint de autenticação retornará sucesso sempre que for chamado enquanto os tokens forem válidos para o dispositivo.
* Quando um Temp Pass expirar, o usuário não será mais autenticado, pois no fluxo do Temp Pass, o token de autenticação e o token de autorização têm a mesma data de expiração. Para explicar aos usuários que o Temp Pass expirou, os Programadores devem recuperar o MVPD selecionado logo após chamar `setRequestor()` e, em seguida, chamar `checkAuthentication()` como de costume. No retorno de chamada `setAuthenticationStatus()`, é possível verificar se o status de autenticação é 0, para que, se o MVPD selecionado fosse &quot;TempPass&quot;, uma mensagem possa ser apresentada aos usuários informando que a sessão de Temp Pass expirou.
* Se um usuário excluir o token de passagem temporária antes da expiração, as verificações de direito subsequentes gerarão um token que terá um TTL igual ao tempo restante.
* Se o usuário excluir o token de passagem temporária após a expiração, as verificações de direito subsequentes retornarão &quot;usuário não autorizado&quot;.

Consulte as amostras em [Código de amostra](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#tempass-sample-code) abaixo para obter exemplos de como codificar os detalhes de implementação descritos nesta seção.

## Código de exemplo {#tempass-sample-code}

As seções abaixo mostram como chamar a API de autenticação da Adobe Pass para implementar o fluxo de passagem temporária:

* [Exemplo de logon no iFrame](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#iframe-login-sample)
* [Amostra de logon automático](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#auto-login-sample)

### Exemplo de logon no iFrame {#iframe-login-sample}

Este exemplo mostra como implementar o Temp Pass para o caso em que os MVPDs oferecem suporte à integração com iFrame:

```HTML
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
        "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <title>Temp Pass Sample</title>
    <script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
    <script type="text/javascript" src="https://raw.github.com/carhartl/jquery-cookie/master/jquery.cookie.js"></script>
    <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js"></script>
 
    <script type="text/javascript">
        var ae, ifrm, providersMenu, previousSelectedProvider;
        var tempassSelected = false;
 
        $(document).ready(function() {
            ifrm = $('#ifrm');
            swfobject.embedSWF("http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.swf"
                    , "ae", "1", "1", "11.0.0", "expressinstall.swf", {}
                    , {wmode: "transparent", allowScriptAccess: "always"}
                    , {id: "accessEnabler", name: "accessEnabler"});
        });
 
        function swfLoaded() {
            ae = $('#accessEnabler')[0];
            ae.setProviderDialogURL("none");
            ae.setRequestor("sample_requestor_Id");
            previousSelectedProvider = ae.getSelectedProvider(); 
            ae.checkAuthentication();
        }
 
        function createIFrame() {
            providersMenu.hide();
 
            // If the user already used TempPass once, hide the button
            if ($.cookie("TPSelected") == "1"){
                $('#tempassBtn').hide();
            }
            ifrm.show();
        }
 
        function displayProviderDialog(providers) {
            if (tempassSelected) {
                // Remember in a cookie that the user selected temp pass
                $.cookie("TPSelected", "1", {expires: 366, path: '/'});
 
                // Authenticate with temp pass
                ae.setSelectedProvider("TempPass");
            } else {
                $('#loginBtn').hide();
                providersMenu = $('<select></select>');
 
                providersMenu.change(function(event){
                    ae.setSelectedProvider(event.target.value);
                });
 
                $.each(providers, function(k, v) {
                    // Add the MVPDs to the menu while making
                    //   sure that the Temp Pass entry is skipped
                    if(v.ID != "TempPass") {
                        providersMenu.append($('<option></option>', {value:v.ID}).text(v.displayName));                       
                    }
                });
                $('body').append(providersMenu);
            }
        }
 
        function setAuthenticationStatus(status, code) {
            loginBtn = $('#loginBtn');
            logoutBtn = $('#logoutBtn');
            console.log(previousSelectedProvider);
 
            if (status == 1) {
                $('#selectedProvider').text("Authenticated with " + ae.getSelectedProvider().MVPD + "   ");
                loginBtn.hide();
                logoutBtn.show();
 
                // Get authorization
                ae.getAuthorization("sample_requestor_Id");
            } else {
                // If selected provider is TempPass but the user is not authenticated,
                //   infer that the TempPass period has expired, so reset the MVPD selection
                if (previousSelectedProvider && previousSelectedProvider.MVPD == "TempPass") {
                    previousSelectedProvider = null;
                    ae.setSelectedProvider(null);
                    alert("Your Temp Pass has expired, please login with your regular cable provider!");
                }
                loginBtn.show();
                logoutBtn.hide();
            }
        }
 
        function selectTempPass() {
            ifrm.hide();
 
            // Signal the fact that the user selected temp pass
            tempassSelected = true;
 
            // Cancel the current authentication flow
            ae.setSelectedProvider(null);
 
            // Retry authentication
            ae.getAuthentication();
 
        }
    </script>
</head>
<body>
    <button id="loginBtn" style="display: none" onclick="ae.getAuthentication();">Login</button>
    <label id="selectedProvider" for="logoutBtn"></label><button id="logoutBtn"
           style="display: none" onclick="ae.logout()">Logout</button>
    <div id="ifrm"
         style="display: none; position: absolute; top: 50px; left:50px; width: 400px; height: 400px; border: 2px solid red;">
        <button id="tempassBtn"
           onclick="selectTempPass();"
             style="float:left">Don't know your credentials? Click here to get a Temp Pass.
        </button>
        <button onclick="window.location.reload()" style="float:right">X</button>
        <br />
        <hr />
        <iframe src="about:blank" id="mvpdframe" name="mvpdframe" width="90%" height="90%" frameborder="0"></iframe>
    </div>
    <br/>
    <div id="ae" style="display: none">
        <p>Loading Access Enabler...</p>
    </div>
</body>
</html>
```

#### Casos de uso de logon de iFrame {#iframe-login-use-cases}

**Para solicitar um Temp Pass pela primeira vez:**

1. Um usuário acessa a página do Programador e clica no link de logon.
1. O seletor de MVPD é aberto e o usuário escolhe uma MVPD na lista.
1. O iFrame de autenticação é exibido. Esse iFrame contém um link &quot;Temp Pass&quot;.
1. O usuário clica em &quot;Temp Pass&quot;, portanto, o Programador adiciona um sinalizador a um cookie para impedir que o usuário veja o link &quot;Temp Pass&quot; em visitas subsequentes à página.
1. A solicitação de autenticação de Aprovação Temporária alcança os servidores de Autenticação da Adobe Pass e eles geram um token de autenticação. O TTL é igual ao período definido pelo Programador para a Aprovação de Temp.
1. A solicitação de autorização de Aprovação Temporária alcança os servidores de Autenticação da Adobe Pass.
1. Os servidores de Autenticação do Adobe Pass extraem as IDs do dispositivo e do solicitante da solicitação e as armazenam no banco de dados junto com a hora de expiração. A hora de expiração é calculada como: tempo de solicitação de aprovação de temperatura inicial mais o TTL (especificado pelo programador).
1. Os servidores de Autenticação do Adobe Pass geram um token de autorização.
1. O usuário acessa o conteúdo protegido.

**Para solicitar novamente um Temp Pass após um usuário de Temp Pass retornar ter excluído os cookies do navegador:**

1. O usuário acessa a página do Programador e clica no link de logon.
1. O seletor de MVPD é aberto e o usuário escolhe uma MVPD na lista.
1. O iFrame de autenticação é exibido. Este iFrame contém um link &quot;Temp Pass&quot; (o usuário excluiu o cookie original, portanto, o Programador não sabe se o usuário clicou no link &quot;Temp Pass&quot; antes).
1. O usuário clica em &quot;Temp Pass&quot; novamente, portanto, o Programador adiciona um sinalizador a um cookie novamente, para impedir que o usuário veja o link &quot;Temp Pass&quot; em visitas subsequentes à página.
1. A solicitação de autenticação de Aprovação Temporária alcança os servidores de Autenticação da Adobe Pass, que geram um token de autenticação. O TTL agora é o tempo restante para a passagem de temperatura (a diferença entre o tempo atual e o tempo de expiração associado à ID de dispositivo).
1. A solicitação de autorização de Aprovação Temporária alcança os servidores de Autenticação da Adobe Pass.
1. Os servidores de Autenticação do Adobe Pass extraem as IDs do dispositivo e do solicitante da solicitação e as usam para recuperar o tempo de expiração do banco de dados de Autenticação do Adobe Pass. A hora atual é comparada à hora de expiração.
1. Se a Senha temporária do usuário não tiver expirado, os servidores de Autenticação do Adobe Pass gerarão um token de autorização.
1. Se o Temp Pass do usuário não tiver expirado, o usuário poderá acessar o conteúdo protegido.

### Amostra de logon automático {#auto-login-sample}

O exemplo a seguir ilustra um caso em que um usuário é automaticamente conectado com TempPass ao visitar um site. O usuário pode optar por fazer logon com uma MVPD comum a qualquer momento e será avisado se o TempPass tiver expirado:

```HTML
<html>
<head>
    <title>Temp Pass Sample</title>
    <script type="text/javascript"
             src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.2/jquery.min.js"></script>
    <script type="text/javascript"
             src="https://raw.github.com/carhartl/jquery-cookie/master/jquery.cookie.js"></script>
    <script type="text/javascript"
             src="http://ajax.googleapis.com/ajax/libs/swfobject/2.2/swfobject.js"></script>
 
    <script type="text/javascript">
        var REQUESTOR = "REF";
        var RESOURCE = "sample_requestor_Id";
        var selectedProvider = null;
        var mvpds;
        var hasTempPassMVPD = false;
 
        // Used to cache the mvpd picker
        var picker;
 
        $(document).ready(function() {
            swfobject.embedSWF("http://entitlement.auth.adobe.com/entitlement/AccessEnablerDebug.swf"
                    , "ae", "1", "1", "11.0.0", "expressinstall.swf", {}
                    , {allowScriptAccess: "always"}
                    , {id: "accessEnabler", name: "accessEnabler"});
        });
 
        function swfLoaded(){
            console.log("AccessEnabler loaded");
            ae = $('#accessEnabler')[0];
 
            // Make sure the default picker is disabled
            ae.setProviderDialogURL("none");
 
            ae.setRequestor(REQUESTOR);
            ae.checkAuthentication();
        }
 
        /**
         * Callback received as a result of setRequestor()
         *
         * @param xml object holding the configuration for the current REQUESTOR
         * including the MVPD list
         */
        function setConfig(config) {
            // Save the mvpd list
            var mvpdList = $.parseXML(config);
            mvpds = $(mvpdList).find('mvpd');
 
            // Create the picker only once and cache it
            if(!picker) {
                picker = $('<div id="mvpdPicker"/>');
 
                var providersMenu = $('<select id="mvpdList" multiple></select>');
 
                $.each(mvpds, function(k, v) {
                    var mvpdID = $(v).find("id").text();
                    var mvpdName = $(v).find("displayName").text();
 
                    // Add the mvpd's to the menu while making
                    //   sure that the Temp Pass entry is skipped
                    if (mvpdID != "TempPass") {
                        providersMenu.append($('<option></option>', {value:mvpdID}).text(mvpdName));
                    } else {
                        hasTempPassMVPD = true;
                    }
                });
                picker.append(providersMenu);
                picker.append($('<br/>'));
                picker.append($('<input type="button" onclick="login()" value="login" />'));
                picker.append($('<input type="button" onclick="cancelPicker()" value="cancel" />'));                  
            }
 
            if (!hasTempPassMVPD) {
                $('#selectedProvider').html("FATAL ERROR: TempPass is not integrated with '" +
                  REQUESTOR + "'<br />This sample is valid only for sites integrated with TempPass !!!");             
            }
        }
 
        /**
         * Callback triggered for iFramed MVPD's
         */
        function createIFrame() {
            $('#mvpdPicker').remove();
            $('#ifrm').show();
        }
 
        /**
         * Hides the MVPD picker
         * when the user clicks "Cancel"
         */
        function cancelPicker() {
            $('#video').show();
            $('#mvpdPicker').remove();
            $('#loginBtn').show();
        }
 
        /**
         * Pops up the MVPD picker
         */
        function showPicker() {
            $('#video').hide();
            $('#loginBtn').hide();
            $('body').append(picker);
        }
 
        function logout() {
            $.removeCookie('tempPassUsed');
            ae.logout();
        }
 
        /**
         * Performs login with the selected MVPD
         */
        function login() {
            selectedProvider = $('#mvpdList').val()[0];
 
            // Make sure we clear out previously
            // selected. This is a must if we want to force
            // login with a real MVPD while still logged in with
            // TempPass, without doing an ae.logout()
            ae.setSelectedProvider(null);
            ae.getAuthentication();
        }

        /**
         * Callback triggered by AccessEnabler. This is usually
         * triggered in order to display the MVPD picker, but
         * since we already constructed, cached, and displayed the
         * picker, and the user already picked the MVPD, we don't need
         * to do anything here but state management
         */
        function displayProviderDialog() {
            // If the selected MVPD is TempPass
            // store this fact in a cookie,
            // otherwise clear it
            if (selectedProvider != 'TempPass') {
                $.removeCookie('tempPassUsed');
            } else {
                $.cookie("tempPassUsed", 1);
            }
 
            // Since the picker was already shown
            // and the user picked an MVPD,
            // just proceed to login
            ae.setSelectedProvider(selectedProvider);
        }
 
        function setAuthenticationStatus(status, code) {
            if (!hasTempPassMVPD) {
                $('#selectedProvider').html("FATAL ERROR: TempPass is not integrated with '" +
                  REQUESTOR + "'<br />This sample is valid only for sites integrated with TempPass !!!");
            } else if(status == 1) {
                selectedProvider = ae.getSelectedProvider().MVPD;
                $('#selectedProvider').text("Authenticated with " + selectedProvider + "   ");
 
                // If authenticated with TempPass
                // allow the user to login with
                // a real MVPD
                if (selectedProvider == "TempPass") {
                    $('#loginBtn').show();
                    $('#logoutBtn').hide();
                } else {
                    $('#loginBtn').hide();
                    $('#logoutBtn').show();
                }
 
                // Get authorization
                // Note: This is mandatory in order to "start" the temp pass countdown
                ae.checkAuthorization(RESOURCE);
            } else if(code != "Provider not Selected Error") {
                // Auto-authenticate with TempPass only if we infer
                // that TempPass has not expired, otherwise we
                // inform the user that TempPass has expired
                if ($.cookie('tempPassUsed') == 1) {
                   $('#selectedProvider').text("Your Temp Pass has expired, please log in with your cable provider!");
                   $('#logoutBtn').show();
                   showPicker();
                } else {
                    selectedProvider = 'TempPass';
                    ae.getAuthentication();
                }
            }
        }
 
        /**
         * Displays the picker as a result
         * of user action
         */
        function loginClicked() {
            $('#loginBtn').hide();
            showPicker();
        }
 
        /**
         * Callback triggered in case of authorization success
         */
        function setToken(token) {
            console.log(token);
            $('#video').html('<img src=">');
        }
 
        /**
         * Callback triggered in case of authz failure
         */
        function tokenRequestFailed(resource, status, message) {
            console.log(resource);
            $('#video').html('<p style="color: red">' + status + ': ' + message + '</p>');
        }
 
    </script>
</head>
<body>
    <button id="loginBtn" style="display: none" onclick="loginClicked()">Login</button>
    <label id="selectedProvider" for="logoutBtn"></label><button id="logoutBtn"
        style="display: none" onclick="logout()">Logout</button>
    <div id="ifrm"
         style="display: none; position: absolute; top: 50px; left:50px;
         width: 400px; height: 400px; border: 2px solid red;">
        <button onclick="window.location.reload()" style="float:right">Close this window</button>
        <br /><hr />
        <iframe src="about:blank" id="mvpdframe" name="mvpdframe" width="80%" height="80%" frameborder="0"></iframe>  
    </div>
    <br/>
 
    <div id="video"></div>
    <div id="ae" style="display: none"><p>Loading Access Enabler...</p></div>
</body>
</html>
```

## Usar várias passagens temporárias {#use-mult-tempass}

Certos eventos exigem acesso livre em fases ao conteúdo, como um intervalo inicial de acesso livre (por exemplo, 4 horas), seguido de acesso gratuito diário (por exemplo, 10 minutos em cada dia subsequente).  Para que um Programador implemente esse cenário, ele deve organizá-lo com seu contato de Adobe para configurar dois MVPDs temporários para o Programador.

Para esse exemplo de cenário (uma sessão inicial de 4 horas livre, seguida por sessões diárias de 10 minutos livres), o Adobe configura uma MVPD chamada TempPass1 com um TTL (Time-To-Live) de 4 horas e uma TempPass2 com um TTL de 10 minutos para o período subsequente.  Ambos estão associados à ID do solicitante do programador.

### Implementação do programador {#mult-tempass-prog-impl}

Depois que o Adobe configurar as duas instâncias do TempPass, os dois MVPDs adicionais (TempPass1 e TempPass2) aparecerão na lista MVPD do Programador.  O Programador precisa executar as seguintes etapas para implementar as várias passagens temporárias:

1. Na visita inicial do usuário ao site, faça logon nele automaticamente com o TempPass1. Você pode usar a Amostra de Login Automático acima como ponto de partida para essa tarefa.
1. Quando você detectar que o TempPass1 expirou, armazene o fato (em um cookie/armazenamento local) e apresente ao usuário seu seletor de MVPD padrão. **Filtre TempPass1 e TempPass2 dessa lista**.
1. Em cada dia subsequente, se TempPass1 expirar, efetue login automático nesse usuário com TempPass2.
1. Quando o TempPass2 expirar, armazene o fato (em um cookie/armazenamento local) para o dia e apresente ao usuário seu seletor de MVPD padrão. Novamente, filtre TempPass1 e TempPass2 dessa lista.
1. A cada novo dia, às 00:00 horas, redefina todos os passos de temp para TempPass2 usando a [Redefinir API da Web TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass.md#reset-all-tempass).

>[!NOTE]
>**Nota de programação:** a Autenticação Adobe Pass não tem um mecanismo interno para interromper a transmissão gratuita após 10 minutos.  Cabe aos programadores restringir o acesso depois que o TempPass2 expirar. Para conseguir isso, os programadores podem implementar em seus sites/aplicativos uma chamada de &quot;checkAuthorization&quot; a cada X minutos, onde X é o período de tempo que o programador determina que faz sentido para seus aplicativos.

## Redefinir todas as passagens temporárias {#reset-all-tempass}

Certas regras de negócios exigem a limpeza regular da Aprovação Temporária ou a redefinição ad hoc de todas as Aprovações Temporárias emitidas para uma ID do Solicitante e uma MVPD ID específicas. Esse recurso oferece suporte a casos de uso como os seguintes:

* Um passe de Temp diário de 10 minutos (o passe de Temp deve ser redefinido no início do dia)
* Um passe temporário disponível para todos os usuários durante as últimas notícias. (O Temp Pass precisa ser redefinido para todos os dispositivos assim que as últimas notícias começarem.)
* O cenário de vários passes temporários que fornece uma combinação de um período de exibição inicial de alguma duração, seguido por períodos diários subsequentes de outra duração.

Para redefinir todas as Passagens Temporárias, a Autenticação do Adobe Pass fornece aos Programadores uma API da Web *pública*:

```url
DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset
```

>[!NOTE]
>O URL acima substitui a API de redefinição anterior. A antiga API de redefinição (v1) não é mais suportada.

* **Protocolo:** HTTPS
* **Host:**
   * Versão - mgmt.auth.adobe.com
   * Pré- Igual - mgmt-prequal.auth.adobe.com
* **Caminho:** /reset-tempass/v2/reset
* **Parâmetros de consulta:** `device_id=all&requestor_id=REQUESTOR_ID&mvpd_id=TEMPPASS_MVPD_ID`
* **Cabeçalhos:** ApiKey - 1232293681726481
* **Resposta:**
   * Sucesso - HTTP 204
   * Falha:
      * HTTP 400 para uma solicitação incorreta
      * HTTP 401 se a ApiKey não foi especificada
      * HTTP 403 se a ApiKey for inválida

Por exemplo:

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset?device_id=all&requestor_id=AdobeBEAST&mvpd_id=TempPass"
```

## Clientes compatíveis {#supp-clients}

Suporte da Ferramenta Temp Pass e Reset por plataforma:

| Clientes de autenticação da Adobe Pass | Temp Pass (Aprovação temporária) | Redefinir ferramenta |
|:--------------------------------------:|:---------:|:----------:|
| JS AccessEnabler | SIM | SIM |
| IOS do cliente nativo | SIM | SIM |
| tvOS do cliente nativo | SIM | SIM |
| Android do cliente nativo | SIM | SIM |
| FireTV do cliente nativo | SIM | SIM |
| API sem cliente | SIM | SIM |

## Limitações e problemas conhecidos {#limitations}

Esta seção descreve as limitações que se aplicam à implementação atual do Temp Pass.

**JavaScript SDK**: oferece suporte à funcionalidade de redefinição de passagem temporária da versão **3.X e superior**.

<!--For Customers migrating from the 2.X JavaScript AccessEnabler to the 3.X JavaScript AccessEnabler, see [AccessEnabler JS 2.x to JS 3.x migration guide](https://tve.helpdocsonline.com/accessenabler-js-to-js-migration-guide).-->
