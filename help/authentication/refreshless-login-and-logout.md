---
title: Logon e logout sem atualização
description: Logon e logout sem atualização
exl-id: 3ce8dfec-279a-4d10-93b4-1fbb18276543
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1761'
ht-degree: 0%

---

# Logon e logout sem atualização {#tefresh-less-login-and-logout}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview}

Para aplicativos web, você deve considerar alguns cenários possíveis diferentes para autenticar e fazer logout dos usuários.  Os MVPDs exigem que os usuários façam logon na página da Web do MVPD para se autenticarem, com os seguintes fatores adicionais sendo abordados:

- Alguns MVPDs exigem um redirecionamento completo do site para a página de logon deles
- Alguns MVPDs exigem que você abra um iFrame no seu site para exibir a página de logon do MVPD
- Alguns navegadores não lidam bem com o cenário do iFrame, portanto, para esses navegadores, uma alternativa melhor é usar uma janela pop-up em vez do iFrame

Antes da Autenticação Adobe Pass 2.7, todos esses cenários para autenticação de um usuário envolviam uma atualização completa da página do Programador.Para a versão 2.7 e versões subsequentes, a equipe de Autenticação Adobe Pass melhorou esses fluxos para que o usuário não precise experimentar uma atualização de página no seu aplicativo durante o logon e o logout.


## Descrição detalhada {#detailed_description}

Vamos começar com um resumo dos fluxos de autenticação e logout originais e, em seguida, seguir com a autenticação aprimorada e os fluxos de logout. Observe que as primeiras quatro seções abordam MVPDs normais (não TempPass), enquanto a última seção descreve a implementação especial que precisa ser aplicada para TempPass:

- [Fluxo de autenticação original](#orig_authn)
- [Fluxo de saída original](#orig_logout)
- [Fluxo de autenticação aprimorado](#improved_authn)
- [Fluxo de logout aprimorado](#improved_logout)
- [Fluxo TempPass](#improved_temppass)

</br>

## Fluxos de autenticação/logout originais {#orig_authn}

**Autenticação**

Os clientes da Web de Autenticação do Adobe Pass têm duas maneiras de autenticação, dependendo dos requisitos dos MVPDs:

1. **Redirecionamento de página inteira -** Depois que o usuário seleciona um provedor    (configurado com redirecionamento de página inteira) do seletor de MVPD no    O site do programador `setSelectedProvider(<mvpd>)` é chamado no AccessEnabler e o usuário é redirecionado para a página de logon do MVPD. Depois que o usuário fornece credenciais válidas, ele é redirecionado de volta ao site do Programador. O AccessEnabler foi inicializado e o token de autenticação foi recuperado da Autenticação Adobe Pass durante `setRequestor`.
1. **iFrame / Janela Pop-up -** Depois que o usuário seleciona um provedor (configurado com iFrame), `setSelectedProvider(<mvpd>)` é chamado no AccessEnabler. Esta ação acionará o retorno de chamada `createIFrame(width, height)`, notificando o Programador para criar um iFrame (ou pop-up - dependendo do navegador/preferências) com o nome `"mvpdframe"` e as dimensões fornecidas. Depois que o iFrame/pop-up é criado, o AccessEnabler carrega a página de logon do MVPD no iFrame/pop-up. O usuário fornece credenciais válidas e o iFrame/pop-up é redirecionado para a Autenticação do Adobe Pass, que retorna um trecho JS que fecha o iFrame/pop-up e recarrega a página principal (site do programador). De modo semelhante ao fluxo 1, o token de autenticação é recuperado durante `setRequestor`.

O retorno de chamada `displayProviderDialog` (disparado por `getAuthentication`/`getAuthorization`) retorna uma lista de MVPDs e suas configurações apropriadas. A propriedade `iFrameRequired` de um MVPD permite que o Programador saiba se ele deve ativar o fluxo 1 ou o fluxo 2. Observe que o Programador é necessário para executar uma ação extra (criar um iFrame/pop-up) somente para o fluxo 2.

**Cancelar autenticação**

Há também a situação em que o usuário cancela explicitamente o fluxo de autenticação fechando a página de logon. Estes são os cenários e a solução proposta para os programadores:

1. **Redirecionamento de página inteira -** Quando a página de logon for fechada, o usuário precisará navegar novamente para o site do Programador e iniciar todo o fluxo desde o início. Nenhuma ação explícita é necessária por parte do programador neste cenário.
1. **iFrame -** Recomenda-se que o Programador hospede o iFrame dentro de um `div` (ou componente de interface semelhante) que tenha um botão Fechar anexado a ele. Quando o usuário pressiona o botão Fechar, o Programador destrói o iFrame com a interface do usuário associada e executa `setSelectedProvider(null)`. Essa chamada permite que o AccessEnabler limpe seu estado interno e possibilita que o usuário inicie um fluxo de autenticação subsequente. `setAuthenticationStatus` e `sendTrackingData(AUTHENTICATION_DETECTION...)` serão acionados para sinalizar um fluxo de autenticação com falha (ambos em `getAuthentication` e `getAuthorization`).
1. **Pop-up -** Alguns navegadores não podem detectar com precisão o evento de fechamento de janela, portanto, é necessário fazer uma abordagem diferente aqui (ao contrário do fluxo do iFrame acima). A Adobe recomenda que o Programador inicialize um temporizador que verifique periodicamente a existência do pop-up de logon. Se a janela não existir, o Programador pode ter certeza de que o usuário cancelou manualmente o fluxo de logon, e o Programador pode continuar chamando `setSelectedProvider(null)`. Os retornos de chamada acionados são os mesmos do fluxo 2 acima.

</br>

## Fluxo de saída original {#orig_logout}

A API de logout do AccessEnabler limpa o estado local da biblioteca e carrega o URL de logout do MVPD na guia/janela atual. O navegador navega até o endpoint de logout do MVPD e, após a conclusão do processo, o usuário é redirecionado de volta ao site do Programador. A única ação necessária em nome do usuário é pressionar o botão/link Logout e iniciar o fluxo; nenhuma interação do usuário é necessária no ponto de extremidade de logout do MVPD.

**Fluxo de Autenticação Original/Saída com Atualização de Página**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_refresh_web.png)

</br>

## Autenticação aprimorada (sem atualização) {#improved_authn}

>[!NOTE]
>
>Os fluxos aprimorados de logon e logout sem atualização exigem que o navegador seja compatível com as modernas tecnologias HTML5, incluindo mensagens da Web.

Os fluxos de autenticação (logon) e logout discutidos acima fornecem uma experiência de usuário semelhante ao recarregar a página principal após a conclusão de cada fluxo.  O recurso atual tem como objetivo melhorar a experiência do usuário, fornecendo logon e logout sem atualização (em segundo plano). O Programador pode habilitar/desabilitar o logon e o logout em segundo plano passando dois sinalizadores booleanos (`backgroundLogin` e `backgroundLogout`) para o parâmetro `configInfo` da API `setRequestor`. Por padrão, o logon/logout em segundo plano está desativado (isso fornece compatibilidade com a implementação anterior).

**Exemplo:**

```JSON
    var configInfo = {
        callSetConfig: true,
        backgroundLogin: true,
        backgroundLogout: true
    };
    accessEnabler.setRequestor(REQUESTOR_ID, null, configInfo);
```

**Autenticação**

Os pontos a seguir descrevem a transição entre os fluxos de autenticação originais e os fluxos aprimorados:

1. O redirecionamento de página inteira é substituído por uma nova guia do navegador na qual o logon do MVPD é executado. O Programador precisa criar uma nova guia (via `window.open`) chamada `mvpdwindow` quando o usuário seleciona um MVPD (com `iFrameRequired = false`). O Programador então executa `setSelectedProvider(<mvpd>)`, permitindo que o AccessEnabler carregue o URL de logon MVPD na nova guia. Depois que o usuário fornecer credenciais válidas, a autenticação da Adobe Pass fechará a guia e enviará uma window.postMessage para o site do programador que sinaliza ao AccessEnabler que o fluxo de autenticação foi concluído. Os seguintes retornos de chamada são acionados:

   - Se o fluxo foi iniciado por `getAuthentication`: `setAuthenticationStatus` e `sendTrackingData(AUTHENTICATION_DETECTION...)` serão acionados para sinalizar autenticação bem-sucedida/malsucedida.

   - Se o fluxo foi iniciado por `getAuthorization`: `setToken/tokenRequestFailed` e `sendTrackingData(AUTHORIZATION_DETECTION...)` serão acionados para sinalizar autorização bem-sucedida/malsucedida.

1. O fluxo da janela de pop-up/iFrame permanece basicamente inalterado, com a diferença de que, depois que o usuário fornece credenciais válidas, a página pai não será recarregada. O iFrame/pop-up será fechado automaticamente após o logon e um `window.postMessage` será enviado à página pai, notificando o AccessEnabler que o fluxo foi concluído. Os mesmos retornos de chamada são acionados como no fluxo anterior, **mais o seguinte novo retorno de chamada:`destroyIFrame`**. O retorno de chamada `destroyIFrame` permite que o Programador remova quaisquer componentes associados/auxiliares do iFrame, como decorações da interface do usuário. A existência desse retorno de chamada não era necessária no fluxo de autenticação antigo porque, após a conclusão do logon, a autenticação da Adobe Pass recarregaria a página do programador, destruindo todos os componentes da interface do usuário nela.

</br>

>[!IMPORTANT]
> 
>Você deve carregar o iFrame de logon MVPD ou a janela pop-up como um filho direto da página que contém a instância AccessEnabler. Se o iFrame de logon MVPD ou a janela pop-up estiver aninhada dois ou mais níveis abaixo da página que contém a instância AccessEnabler, o fluxo poderá travar. Por exemplo, se você tiver um iFrame situado entre a página principal e o iFrame MVPD (Page =\> iFrame =\> MVPD iFrame), o fluxo de logon poderá falhar.

</br>

**Cancelar autenticação**

Estes são os fluxos para cancelar a autenticação:

1. **Guia Navegador -** Como a guia é basicamente uma nova janela, capturar seu evento de fechamento tem as mesmas limitações discutidas no cenário 3 dos fluxos de autenticação antigos. Além disso, a abordagem de cronômetro não é possível aqui, pois não há como distinguir entre uma guia que foi fechada manualmente pelo usuário e uma guia que foi fechada automaticamente no final do fluxo de logon. A solução aqui é que o AccessEnabler permaneça &quot;silencioso&quot; (nenhum retorno de chamada é acionado) quando o usuário cancela o fluxo. Além disso, o programador não é obrigado a tomar qualquer ação específica. O usuário poderá iniciar outro fluxo de autenticação sem receber o erro &quot;Erro de várias solicitações de autenticação&quot; (esse erro foi desativado no AccessEnabler para logon em segundo plano).

1. **iFrame -** O Programador pode usar a abordagem discutida no cenário 2 a partir dos fluxos de autenticação antigos (crie a interface do usuário do wrapper a partir do iFrame e do botão Fechar associado que aciona `setSelectedProvider(null)`. Embora essa abordagem não seja mais um requisito forte (vários fluxos de autenticação são permitidos para logon em segundo plano, conforme discutido no cenário 1 acima), ela ainda é recomendada pelo Adobe.

1. **Pop-up -** É idêntico ao fluxo da guia Navegador acima.

</br>

## Fluxo de logout aprimorado {#improved_logout}

O novo fluxo de logout será executado em um iFrame oculto, eliminando assim o redirecionamento de página inteira.  Isso é possível porque o usuário não precisa realizar uma ação específica na página de logout do MVPD.

Após a conclusão do fluxo de logout, ele redirecionará o iFrame para um endpoint de autenticação Adobe Pass personalizado. Isso fornecerá um trecho JS que executa um `window.postMessage` para o pai, notificando o AccessEnabler que o logout foi concluído. Os seguintes retornos de chamada foram disparados: `setAuthenticationStatus()` e `sendTrackingData(AUTHENTICATION_DETECTION ...)`, sinalizando que o usuário não está mais autenticado.

A ilustração abaixo mostra o fluxo sem atualização que permite que um usuário faça logon em seu MVPD sem atualizar a página principal do aplicativo:

**Fluxo de Autenticação/Logoff Aprimorado (sem atualização)**

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AE_with_no_refresh_web.png)

</br>

## Fluxo TempPass {#improved_temppas}

O logon sem atualização segue uma abordagem diferente para MVPDs do tipo TempPass.

Como o fluxo TempPass requer que uma janela seja criada automaticamente e fechada sem interação explícita do usuário, isso pode causar um problema para alguns navegadores (bloqueadores de pop-up). Portanto, o AccessEnabler implementa a fase de logon em segundo plano, sem exigir um contêiner da Web criado pelo Programador.

Estes são os aspectos que o Programador precisa estar ciente ao implementar o TempPass para logon e logout sem atualização:

- Antes de iniciar a autenticação, o iFrame ou a janela pop-up precisa ser criada somente para MVPDs não TempPass. O Programador pode detectar se um MVPD é TempPass ou não lendo a propriedade `tempPass` do objeto MVPD (retornado por `setConfig()` / `displayProviderDialog()`).

- O retorno de chamada `createIFrame()` deve conter uma verificação para TempPass e executar sua lógica apenas quando o MVPD NÃO for TempPass.

- O retorno de chamada `destroyIFrame()` deve conter uma verificação para TempPass e executar sua lógica apenas quando o MVPD NÃO for TempPass.

- Os retornos de chamada `setAuthenticationStatus()` e `sendTrackingData()` são invocados após a conclusão da autenticação (exatamente como no fluxo sem atualização para MVPDs normais).

>[!NOTE]
>
>Esse fluxo está disponível somente para TempPass sem atualização. Para o fluxo de atualização, o TempPass precisa ser manipulado explicitamente (quando o TempPass exigir iFrame/pop-up)

</br>

A amostra de código a seguir demonstra como lidar com uma janela MVPD em um site do Programador (tanto para MVPDs normais quanto para TempPass):

```javascript
    var aeHostname = "https://entitlement.auth.adobe.com";
    var mvpdWindow = null;
    var mvpd = <mvpd_object_from_displayProviderDialog>;
    var useIframeLogin = <boolean_depending_on_browser_or_Programmer_preferences>;
    var backgroundLogin = <boolean_depending_on_Programmer_preferences>;
     
    // Do not create any windows for refreshless and temp pass
    if (!(backgroundLogin && mvpd.tempPass)) {
        if (backgroundLogin && !mvpd.popup) {
            mvpdWindow = window.open(aeHostname, "mvpdwindow");
        } else if (mvpd.popup && !useIframeLogin) {
            var width = mvpd.width;
            var height = mvpd.height;
            // Center on screen
            var top = (document.all) ? window.screenTop : window.screenY + 100;
            var left = (document.all) ? window.screenLeft : window.screenX + window.innerWidth / 2 - width / 2;
        
            mvpdWindow = window.open(aeHostname, "mvpdframe",
                           "width=" + width + ",height=" + height + ",top=" + top + ",left=" + left);
            // Monitor the mvpd popup for close
            if (!backgroundLogin) {
                clearInterval(cancelTimer);
                cancelTimer = setInterval(function () {
                    if (mvpdWindow && mvpdWindow.closed) {
                        clearInterval(cancelTimer);
                        $('#mvpddiv').hide();
                        accessEnablerAPI.setSelectedProvider(null);
                    }
                }, 200);
            }
        }
    }
```
