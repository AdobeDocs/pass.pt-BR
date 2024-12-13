---
title: Guia de migração do iOS/tvOS v3.x
description: Guia de migração do iOS/tvOS v3.x
exl-id: 4c43013c-40af-48b7-af26-0bd7f8df2bdb
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# Guia de migração do iOS/tvOS v3.x (herdado) {#iostvos-v3x-migration-guide}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!TIP]
> 
> **Notas:**
>
> - A partir do iOS sdk versão 3.1, os implementadores agora podem usar WKWebView ou UIWebView alternadamente. Como a UIWebView está obsoleta, os aplicativos devem migrar para a WKWebView para evitar problemas com versões futuras do iOS.
> - Observe que a migração implicaria simplesmente alternar a classe UIWebView com WKWebView; não há trabalho específico a ser feito em relação ao Adobe AccessEnabler.

</br>

## Atualizar configurações de compilação {#update}

Esta versão contém funcionalidades escritas em linguagem SWIFT. Se seu aplicativo for totalmente Objetive-C, é necessário definir a caixa de seleção &quot;Sempre incorporar bibliotecas padrão Swift&quot; nas configurações de criação do destino como &quot;Sim&quot;. Quando essa opção é definida, o Xcode verifica as estruturas agrupadas no aplicativo e, se qualquer uma delas contiver código Swift, ele copia as bibliotecas pertinentes ao pacote do aplicativo. Se você não atualizar as configurações de compilação, seu aplicativo poderá falhar com erros informando que não pode carregar o AccessEnabler.framework ou várias bibliotecas `ibswift*`.

</br>

## Adicionando sua instrução de software {#add}

> Para obter informações sobre como obter a instrução de software, vá para
> página:
> [Registro de Aplicativo](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)

Depois que você tiver sua instrução de software, recomendamos hospedá-la em um servidor remoto para que você possa revogá-la ou alterá-la facilmente sem implantar uma nova versão do aplicativo no App Store. Quando o aplicativo for iniciado, obtenha a instrução de software do local remoto e transmita-a no construtor AccessEnabler:

```swift
    accessEnabler = AccessEnabler("YOUR_SOFTWARE_STATEMENT_HERE");
```

> Informações da API aqui: [Referência da API iOS/tvOS](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## Adicionar o esquema de URL personalizado {#add-custom}

> Para obter informações sobre como obter um esquema de URL personalizado, vá para esta página: [Obter um esquema de URL do cliente](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-application-registration.md)

Após obter o esquema de URL personalizado, é necessário adicioná-lo ao arquivo info.plist do aplicativo. O esquema personalizado tem este formato: `adbe.u-XFXJeTSDuJiIQs0HVRAg://`. É necessário omitir os dois pontos e as barras ao adicioná-los ao arquivo. O exemplo acima será `adbe.u-XFXJeTSDuJiIQs0HVRAg`.

```plist
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>CUSTOM_URL_SCHEME_HERE</string>
            </array>
        </dict>
    </array>
```

</br>

## Interceptar chamadas no esquema de URL personalizado {#intercept}

Isso se aplica somente caso seu aplicativo tenha habilitado anteriormente a manipulação manual do Safari View Controller (SVC) por meio da chamada [setOptions(\[&quot;handleSVC&quot;:true&quot;\])](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md) e para MVPDs específicos que exigem o Safari View Controller (SVC), exigindo, portanto, o carregamento dos URLs dos pontos de extremidade de autenticação e logout por um controlador SFSafariViewController em vez de um controlador UIWebView/WKWebView.

Durante os fluxos de autenticação e logout, o aplicativo deve monitorar a atividade do controlador `SFSafariViewController ` à medida que passa por vários redirecionamentos. Seu aplicativo deve detectar o momento em que carrega uma URL personalizada específica definida por seu `application's custom URL scheme` (por exemplo, `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com)`. Quando o controlador carrega esta URL personalizada específica, seu aplicativo deve fechar o `SFSafariViewController` e chamar o método de API `handleExternalURL:url ` do AccessEnabler.

Em seu `AppDelegate`, adicione o seguinte método:

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey: Any]) -> Bool {
            if (url.absoluteString.hasPrefix("adbe.")) {
                accessEnabler.handleExternalURL(url.description)
                return true;
            } 
        }
```

> Informações da API aqui: [Tratar URL Externa](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## Atualizar a assinatura do método setRequestor {#update-setreq}

Como a nova SDK está usando um novo mecanismo de autenticação, não há necessidade do parâmetro signedRequestId nem da chave pública e do segredo (para tvOS). O método `setRequestor` é simplificado e precisa apenas da requestorID.

### iOS

Este código:

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId)
```

torna-se:

```swift
    accessEnabler.setRequestor(requestorId)
```

</br>

### tvOS

Este código:

```swift
    accessEnabler.setRequestor(requestorId, setSignedRequestorId: signedRequestorId,
                    secret: "secret", publicKey: "public_key")
```

torna-se:

```swift
    accessEnabler.setRequestor(requestorId)
```

> Informações da API aqui: [Definir Solicitante](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

</br>

## Substituir o método getAuthenticationToken pelo método handleExternalURL {#replace}

O método `getAuthentication` foi usado no passado para concluir o fluxo de autenticação. Como seu nome induzia em erro, ele foi renomeado para `handleExternalURL` e toma a url como um parâmetro.

Altere todas as ocorrências:

```swift
    accessEnabler.getAuthenticationToken()
```

nesta:

```swift
    accessEnabler.handleExternalURL(request.url?.description);
```

> Informações da API aqui: [Tratar URL Externa](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)
