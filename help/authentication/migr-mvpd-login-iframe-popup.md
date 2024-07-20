---
title: Como migrar a página de logon do MVPD do iFrame para o pop-up
description: Como migrar a página de logon do MVPD do iFrame para o pop-up
exl-id: 389ea0ea-4e18-4c2e-a527-c84bffd808b4
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '686'
ht-degree: 0%

---

# Como migrar a página de logon do MVPD do iFrame para o Pop-up {#migr-mvpd-login-iframe-popup}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Pop-up versus iFrame {#popup-vs-iframe}

Alguns usuários encontraram problemas de cookies de terceiros com a implementação do iFrame de uma página de logon MVPD.
<!--These issues are described in the tech notes linked below:

* [Adobe Pass Authentication and Safari login issues](https://tve.helpdocsonline.com/adobe-pass)
* [MVPD iFrame login and 3rd party cookies](https://tve.helpdocsonline.com/mvpd)-->

A equipe de Autenticação do Adobe Pass **recomenda implementar a pop-up/nova página de logon de janela** em vez da versão iFrame no Firefox e no Safari.  No entanto, se estiver implementando uma página de logon para o Internet Explorer, você pode encontrar problemas com a implementação pop-up. Os problemas do IE são causados pelo fato de que, depois que o usuário é autenticado com seu MVPD na janela pop-up, a Autenticação do Adobe Pass força um redirecionamento de página pai, que é visto pelo Internet Explorer como um bloqueador de pop-up. A equipe de Autenticação do Adobe Pass **recomenda implementar o logon de iFrame para o Internet Explorer**.

O código de amostra apresentado nesta nota técnica usa uma implementação híbrida de iFrame e pop-up - abrindo um iFrame no Internet Explorer e um pop-up nos outros navegadores.

Considerando que uma implementação de iFrame já existe, a primeira parte da nota técnica apresenta o código para a implementação de iFrame e a segunda parte apresenta as alterações para acomodar a implementação de pop-up como padrão.


## Seletor de MVPD com página de logon em um iFrame {#mvpd-pickr-iframe}

Exemplos de código anteriores mostraram uma página de HTML que contém a tag &lt;div>, onde o iFrame deve ser criado junto com o botão Fechar iFrame:

```HTML
<body> 
    <div id="mvpddiv">
        <div style="background: red">
            <input type="button" id="btnCloseIframe" value="X" onclick="closeIframeAction();">
        </div>
        <br/>
        <!-- We use the "about:blank" value so that when the iFrame loads
             we do not see the the parent page. -->
        <iframe id="mvpdframe" name="mvpdframe" src="about:blank"></iframe>
    </div> 
</body>
```

Este é o código **JavaScript** associado:

```JavaScript
/*
 * Callback indicating that the AccessEnabler swf has initialized
 */
function swfLoaded() {
    // AccessEnabler is loaded so we can use the API function it provides
    accessEnablerObject.setRequestor(requestorID); 
    accessEnablerObject.checkAuthentication();
} 
/*
* The code the correctly closes the opened IFrame and does not leave the
* AccessEnabler in an undefined state.
*/
function closeIframeAction () {
    accessEnablerObject.setSelectedProvider(null);
    /* We use the "about:blank" value so that when the iFrame loads
     * we do not see the the previous MVPD.
     */
    document.getElementById('mvpdframe').src="about:blank";
    document.getElementById('mvpddiv').style.visibility="hidden";
    document.getElementById('mvpddiv').style.display="none";
}
/*
* Some of the supported MVPDs require their login pages to be displayed within an iFrame.
* In your HTML page, you must include the following <div> tag: "<div id="mvpddiv"></div>".
* Called when the selected MVPD requires an iFrame in which to display its authentication UI.
*/
function createIFrame(inWidth, inHeight) {
     // Create the iFrame to the specified width and height for the auth page
    ifrm = document.createElement("IFRAME");
    ifrm.name = "mvpdframe";
    ...
    document.getElementById('mvpddiv').appendChild(ifrm);
    ...
    // Force the name into the DOM since it is still not refreshed, for IE
    window.frames["mvpdframe"].name = "mvpdframe";
}
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
    /* This is an example how previously the MVPD picker was build and will be replaced. */
}
/* 
* Sending the user selected MVPD of the custom dialog is achieved
* by calling the API function setSelectedProvider(providerID:String).
*/
function setSelectedProvider(providerID) {
    accessEnablerObject.setSelectedProvider(providerID);
}
```


## Seletor de MVPD com página de logon em uma janela pop-up {#mvpd-pickr-popup}

Como não utilizaremos mais um **iFrame**, o código de HTML não conterá o iFrame nem o botão para fechar o iFrame. A div que continha anteriormente o iFrame - **mvpddiv** - será mantida e usada para o seguinte:

* para notificar o usuário que a página de logon do MVPD já está aberta se o foco do pop-up for perdido
* para fornecer um link para recuperar o foco no pop-up

```HTML
<body onload="javascript:loadAccessEnabler();"> 
   <div id="aeHolder">
       <div name="contentAccessEnabler" id="contentAccessEnabler"></div>
   </div>
   <div id="mvpddiv">
      <p>
      <strong>No login window?</strong>
      <br/>
      <a href="javascript:mvpdWindow.focus();">Click here to open it.</a>
      <br/>
      Still not working? Check popup blockers!
      </p>
   </div> 
 
   <div id="picker" style="display:none">
      Choose MVPD: <br/>
      <select id="mvpdList" multiple></select>
      <br/>
         <a id="authenticateButton" onclick="javascript:authenticate();">Authenticate</a>
   </div>
</body>
```

A lista de MVPDs será exibida no div chamado **seletor** como um **-mvpdList** selecionado.

Um novo retorno de chamada de API será usado - **setConfig(configXML)**. O retorno de chamada é disparado depois de chamar a função setRequestor(requestorID). Esse retorno de chamada retorna a lista de MVPDs que estão integrados com a requestorID definida anteriormente. No método de retorno de chamada, o XML de entrada será analisado e a lista de MVPDs armazenada em cache. O seletor de MVPD também é criado, mas não exibido.

```JavaScript
var mvpdList;  // The list of cached MVPDs
var mvpdWindow;  // The reference to the popup with the MVPD login page
var cancelTimer = 0;
/* Callback indicating that the AccessEnabler swf has initialized */
function swfLoaded() {
   accessEnablerObject = $('#AccessEnabler').get(0);
   // Using a custom implementation of the MVPD picker
   accessEnablerObject.setProviderDialogURL('none');
   accessEnablerObject.setRequestor(requestorID); 
}

function setConfig(configXML) {
   mvpdList = [];
   $.each($($.parseXML(configXML)).find('mvpd'), function (idx, item) {
      var mvpdId = $(item).find('id').text();
      mvpdList[mvpdId] = {
         displayName: $(item).find('displayName').text(),
         logo: $(item).find('logoUrl').text(),
         popup: $(item).find('iFrameRequired').text() == "true",
         width: $(item).find('iFrameWidth').text(),
         height: $(item).find('iFrameHeight').text()
      };

      $('#mvpdList').append($('<option value="' + mvpdId + '" title="' + mvpdId + '">' + mvpdList[mvpdId].displayName + '</option>'));
   });
   accessEnablerObject.getAuthentication();
}
```

Depois que a função getAuthentication() ou getAuthorization() é chamada, o retorno de chamada displayProviderDialog() é acionado. Normalmente, dentro desse retorno de chamada, a lista MVPD teria sido criada e exibida. Como o seletor de MVPD já foi criado, a única coisa a fazer é exibi-lo para o usuário.

```JavaScript
/*
 * The custom non-Flash MVPD selector dialog will be implemented in this function.
 */
function displayProviderDialog(providers) {
   $('#picker').show();
}
```

Depois que o usuário seleciona um MVPD no seletor, o pop-up precisa ser criado. Alguns navegadores podem bloquear o pop-up se ele for criado com about:blank ou com uma página que esteja em outro domínio; portanto, é recomendável abri-lo com o nome do host de onde o AccessEnabler é carregado.

Na implementação do iFrame, a redefinição do fluxo de autenticação estava sendo feita pelo botão btnCloseIframe e pela função de JavaScript closeIframeAction(), mas agora a decoração do iFrame não é mais possível. Assim, o mesmo comportamento é obtido observando o momento em que o pop-up é fechado (pelo usuário ou finalizando o fluxo de autenticação). Um trecho de código foi adicionado que também ajuda caso o usuário perca o foco do pop-up:

```HTML
"<a href="javascript:mvpdWindow.focus();">Click here to open it.</a>".
```

No retorno de chamada createIFrame() a div **mvpddiv** será exibida.

```JavaScript
function createIFrame(width, height) {
   $('#mvpddiv').show();
   if (useIframeLoginStyle) {
      ...
   }
}

/* Function called when user has selected the MVPD from the picker */
function authenticate() {
   var selectedMvpd = $('#mvpdList').val()[0];
   if (mvpdList[selectedMvpd].popup) {
      mvpdWindow = window.open("http://entitlement.auth-staging.adobe.com", "mvpdframe", "width=" + mvpdList[selectedMvpd].width + ",height=" + mvpdList[selectedMvpd].height, true);
      if (!mvpdWindow) {
         // Message to user that popup blocker is not allowing the authentication process to start
      }
      //watch the mvpd popup for close
      clearInterval(cancelTimer);
      cancelTimer = setInterval(checkClosed, 200);
   }
   accessEnablerObject.setSelectedProvider(selectedMvpd);
}

function checkClosed() {
   try {
      if (mvpdWindow && mvpdWindow["closed"]) {
         clearInterval(cancelTimer);
         accessEnablerObject.setSelectedProvider(null);
         accessEnablerObject.getAuthentication();
         $('#mvpddiv').hide();
      }
   } catch (error) {
      console.log(error);
   }
}
```

>[!IMPORTANT]
>
>* O código de amostra contém uma variável codificada permanentemente para o requestorID - &#39;REF&#39; usado, que deve ser substituído por uma ID de solicitante real do programador.
>* O código de amostra só será executado corretamente de um domínio da lista de permissões associado à ID do solicitante usada.
>* Como o código inteiro está disponível para download, o código apresentado nesta nota técnica foi truncado. Para obter uma amostra completa, consulte **iFrame JS vs. Amostra Popup**.
>* As bibliotecas JavaScript externas foram vinculadas do [Google Hosted Services](https://developers.google.com/speed/libraries/).
