---
title: Cabeçalho - X-Device-Info
description: REST API V2 - Cabeçalho - X-Device-Info
exl-id: 0ef25e06-86de-427a-a938-7ba3817f0d5e
source-git-commit: 1c80af438f40fcdba79eca5ac91e9b994941f008
workflow-type: tm+mt
source-wordcount: '1085'
ht-degree: 4%

---

# Cabeçalho - X-Device-Info {#header-x-device-info}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview}

O cabeçalho de solicitação <b>X-Device-Info</b> contém as informações do cliente (dispositivo, conexão e aplicativo) relacionadas ao dispositivo de streaming real.

## Sintaxe {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Device-Info</b>: &lt;informações_do_dispositivo&gt;</td>
   </tr>
   <tr>
      <td>Tipo de cabeçalho</td>
      <td>Cabeçalho da solicitação</td>
   </tr>
   <tr>
      <td>Padrão</td>
      <td>Não</td>
   </tr>
</table>

## Diretivas {#directives}

<b>&lt;informações_do_dispositivo></b>

O valor `Base64-encoded` do elemento JSON que contém pelo menos os atributos marcados como exigidos pela tabela a seguir.

<table>
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">Presença</th>
        <th style="background-color: #EFF2F7; width: 15%;">Chave</th>
        <th style="background-color: #EFF2F7;">Descrição</th>    
        <th style="background-color: #EFF2F7; width: 15%;">Restrito</th>
        <th style="background-color: #EFF2F7;">Valores possíveis</th>
    </tr>
    <tr>
        <td></td>
        <td>primaryHardwareType</td>
        <td>O tipo de hardware principal do dispositivo.</td>
        <td>&amp;verificar;</td>
        <td>
            Os valores são restritos:
            <ul>
                <li>Câmera</li>
                <li>DataCollectionTerminal</li>
                <li>Computador desktop</li>
                <li>EmbeddedNetworkModule</li>
                <li>eReader</li>
                <li>GamesConsole</li>
                <li>GeolocationTracker</li>
                <li>Óculos</li>
                <li>MediaPlayer</li>
                <li>Celular</li>
                <li>TerminalDePagamento</li>
                <li>PluginModem</li>
                <li>DefinirCaixaSuperior</li>
                <li>TV</li>
                <li>Tablet</li>
                <li>Ponto de acesso sem fio</li>
                <li>Relógio de pulso</li>
                <li>Desconhecido</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>obrigatório</i></td>
        <td>modelo</td>
        <td>O nome do modelo do dispositivo.</td>
        <td></td>
        <td>Por exemplo, iPhone, SM-G930V, Apple TV etc.</td>
    </tr>
    <tr>
        <td><i>obrigatório</i></td>
        <td>version</td>
        <td>A versão do dispositivo.</td>
        <td></td>
        <td>Por exemplo, 2.0.1, etc.</td>
    </tr>
    <tr>
        <td></td>
        <td>fabricante</td>
        <td>A empresa/organização de fabricação do dispositivo.</td>
        <td></td>
        <td>Por exemplo, Samsung, LG, ZTE, Huawei, Motorola, Apple, etc.</td>
    </tr>
    <tr>
        <td></td>
        <td>fornecedor</td>
        <td>A empresa/organização de venda do dispositivo.</td>
        <td></td>
        <td>Por exemplo, Apple, Samsung, LG, Google, etc.</td>
    </tr>
    <tr>
        <td><i>obrigatório</i></td>
        <td>osName</td>
        <td>O nome do sistema operacional do dispositivo.</td>
        <td>&amp;verificar;</td>
        <td>
            Os valores são restritos:
            <ul>
                <li>Android</li>
                <li>SO CHROME</li>
                <li>Linux</li>
                <li>Mac OS</li>
                <li>OS X</li>
                <li>OpenBSD</li>
                <li>Roku OS</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>osFamily</td>
        <td>O nome do grupo do Sistema Operacional (SO) do dispositivo.</td>
        <td>&amp;verificar;</td>
        <td>
            Os valores são restritos:
            <ul>
                <li>Android</li>
                <li>BSD</li>
                <li>Linux</li>
                <li>PlayStation OS</li>
                <li>Roku OS</li>
                <li>Symbian</li>
                <li>Tizen</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>macOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>osVendor</td>
        <td>O fornecedor do sistema operacional do dispositivo.</td>
        <td>&amp;verificar;</td>
        <td>
            Os valores são restritos:
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>LG</li>
                <li>Microsoft</li>
                <li>Mozilla</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Roku</li>
                <li>Samsung</li>
                <li>Sony</li>
                <li>Projeto Tizen</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><i>obrigatório</i></td>
        <td>osVersion</td>
        <td>A versão do sistema operacional do dispositivo.</td>
        <td></td>
        <td>Por exemplo, 10.2, 9.0.1, etc.</td>
    </tr>
    <tr>
        <td></td>
        <td>browserName</td>
        <td>O nome do navegador.</td>
        <td>&amp;verificar;</td>
        <td>
            Os valores são restritos:
            <ul>
                <li>Navegador Android</li>
                <li>Chrome</li>
                <li>Edge</li>
                <li>Firefox</li>
                <li>Internet Explorer</li>
                <li>Opera</li>
                <li>Safari</li>
                <li>SeaMonkey</li>
                <li>Navegador Symbian</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVendor</td>
        <td>A empresa/organização de construção do navegador.</td>
        <td>&amp;verificar;</td>
        <td>
            Os valores são restritos:
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>Microsoft</li>
                <li>Motorola</li>
                <li>Mozilla</li>
                <li>Netscape</li>
                <li>Nintendo</li>
                <li>Nokia</li>
                <li>Samsung</li>
                <li>Sony Ericsson</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>browserVersion</td>
        <td>A versão do navegador do dispositivo.</td>
        <td></td>
        <td>ex: 60.0.3112</td>
    </tr>
    <tr>
        <td></td>
        <td>userAgent</td>
        <td>O agente do usuário do dispositivo.</td>
        <td></td>
        <td>por exemplo, Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, como Gecko) Versão/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td></td>
        <td>displayWidth</td>
        <td>A largura da tela física do dispositivo.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayHeight</td>
        <td>A altura da tela física do dispositivo.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>displayPpi</td>
        <td>A densidade de pixels da tela física do dispositivo.</td>
        <td></td>
        <td>por exemplo, 294</td>
    </tr>
    <tr>
        <td></td>
        <td>diagonalScreenSize</td>
        <td>A dimensão diagonal da tela física do dispositivo em polegadas.</td>
        <td></td>
        <td>Por exemplo, 5.5, 10.1</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionIp</td>
        <td>O IP do dispositivo usado para enviar solicitações HTTP.</td>
        <td></td>
        <td>Por exemplo, 8.8.4.4</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionPort</td>
        <td>A porta do dispositivo usada para enviar solicitações HTTP.</td>
        <td></td>
        <td>por exemplo, 53124</td>
    </tr>
    <tr>
        <td><i>obrigatório</i></td>
        <td>connectionType</td>
        <td>O tipo de conexão de rede.</td>
        <td></td>
        <td>por exemplo, WiFi, LAN, 3G, 4G, 5G</td>
    </tr>
    <tr>
        <td></td>
        <td>connectionSecure</td>
        <td>O status de segurança da conexão de rede.</td>
        <td>&amp;verificar;</td>
        <td>
            Os valores são restritos:
            <ul>
                <li>true - no caso de uma rede segura</li>
                <li>false - no caso de um ponto de acesso público</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td></td>
        <td>applicationId</td>
        <td>O identificador exclusivo do aplicativo.</td>
        <td></td>
        <td>ex: REF30</td>
    </tr>
</table>


## Exemplos {#examples}

```JSON
// Device information
// {
//  "primaryHardwareType" : "MobilePhone",
//  "model":"SM-S901U",
//  "vendor":"samsung",
//  "version":"r0q",
//  "manufacturer":"samsung",
//  "osName":"Android",
//  "osVersion":"14"
// }
 
// BASE64-encoded
// ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3I
// iOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb
// 2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
 
X-Device-Info: ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3IiOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
```

## Cookbooks {#cookbooks}

>[!IMPORTANT]
> 
> Os snippets de código e os recursos de documentação são fornecidos para fins de referência.
> 
> Os trechos de código não são exaustivos e podem exigir modificações adicionais para funcionar em seu projeto.
>
> Independentemente da implementação real, o cabeçalho `X-Device-Info` deve conter um valor formatado conforme descrito na seção [Diretivas](#directives).

### Navegadores {#browsers}

Para aplicativos cliente em execução em um navegador, o cabeçalho `X-Device-Info` pode ser omitido, pois o navegador enviará automaticamente um conjunto mínimo de informações necessárias no cabeçalho `User-Agent`.

Você ainda poderá usar o cabeçalho `X-Device-Info` para fornecer informações adicionais sobre o dispositivo, a conexão e o aplicativo, caso o aplicativo cliente integre uma biblioteca ou um serviço que forneça um mecanismo de identificação de dispositivo.

### Dispositivos móveis {#mobile-devices}

#### iOS e iPadOS {#ios-ipados}

Para criar o cabeçalho `X-Device-Info` para dispositivos que executam o [iOS ou o iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes), consulte os seguintes documentos e o trecho de código abaixo:

* Documentação do desenvolvedor do Apple para [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice).
* Documentação do desenvolvedor do Apple para [Acessibilidade](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html).
* Documentação de manual do Linux para [uname](https://man7.org/linux/man-pages/man2/uname.2.html).

```C
+ (NSString *)computeClientInformation {        
        struct utsname u;
        uname(&u);

        NSString *hardware = [NSString stringWithCString:u.machine encoding:NSUTF8StringEncoding];

        UIDevice *device = [UIDevice currentDevice];

        NSString *deviceType;

        switch (UI_USER_INTERFACE_IDIOM()) {
            case UIUserInterfaceIdiomPhone:
                deviceType = @"MobilePhone";
                break;
            case UIUserInterfaceIdiomPad:
                deviceType = @"Tablet";
                break;
            case UIUserInterfaceIdiomTV:
                deviceType = @"TV";
                break;
            default:
                deviceType = @"Unknown";
        }

        CGRect screenRect = [[UIScreen mainScreen] bounds];
        NSNumber *screenWidth = @((float) screenRect.size.width);
        NSNumber *screenHeight = @((float) screenRect.size.height);

        Reachability *reachability = [Reachability reachabilityForInternetConnection];
        [reachability startNotifier];

        NetworkStatus status = [reachability currentReachabilityStatus];

        NSString *connectionType;

        if (status == NotReachable) {
            connectionType = @"notConnected";
        } else if (status == ReachableViaWiFi) {
            connectionType = @"WiFi";
        } else if (status == ReachableViaWWAN) {
            connectionType = @"cellular";
        }

        NSMutableDictionary *clientInformation = [@{
                @"type": deviceType,
                @"model": device.model,
                @"vendor": @"Apple",
                @"manufacturer": @"Apple",
                @"version": [hardware stringByReplacingOccurrencesOfString:device.model withString:@""],
                @"osName": device.systemName,
                @"osVersion": device.systemVersion,
                @"displayWidth": screenWidth,
                @"displayHeight": screenHeight,
                @"connectionType": connectionType,
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

As informações do dispositivo podem ser construídas da seguinte maneira:

| Chave | Source | Valor (exemplo) |
|---------------|------------------------|-----------------|
| modelo | uname.machine | iPhone |
| fornecedor | codificado | Apple |
| fabricante | codificado | Apple |
| version | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 320 |
| displayHeight | UIScreen.mainScreen | 568 |
| osName | UIDevice.systemName | iOS |
| osVersion | UIDevice.systemVersion | 10,2 |

As informações de conexão podem ser construídas da seguinte maneira:

| Chave | Source | Valor (exemplo) |
|------------------|------------------------------------------|-----------------|
| connectionType | [CurrentReachabilityStatus de acessibilidade] |                 |
| connectionSecure |                                          |                 |


As informações do aplicativo podem ser construídas da seguinte maneira:

| Chave | Source | Valor (exemplo) |
|---------------|-----------|-----------------|
| applicationId | codificado | REF30 |

#### Android {#android}

Para criar o cabeçalho `X-Device-Info` para dispositivos que executam o [Android](https://developer.android.com/about/versions), consulte os seguintes documentos e o trecho de código abaixo:

* Documentação do desenvolvedor do Android para a classe [Build](https://developer.android.com/reference/android/os/Build.html).

```JAVA
private JSONObject computeClientInformation() {
     String LOGGING_TAG = "DefineClass.class";
  
     JSONObject clientInformation = new JSONObject();

     String connectionType;

     try {
          ConnectivityManager cm = (ConnectivityManager) getContext().getSystemService(CONNECTIVITY_SERVICE);
          NetworkInfo activeNetwork = cm.getActiveNetworkInfo();

          if (activeNetwork != null && activeNetwork.isConnectedOrConnecting()) {
              switch (activeNetwork.getType()) {
                    case ConnectivityManager.TYPE_WIFI: {
                        connectionType = "WIFI";
                        break;
                    }
                    case ConnectivityManager.TYPE_BLUETOOTH: {
                        connectionType = "BLUETOOTH";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE: {
                        connectionType = "MOBILE";
                        break;
                    }
                    case ConnectivityManager.TYPE_ETHERNET: {
                        connectionType = "ETHERNET";
                        break;
                    }
                    case ConnectivityManager.TYPE_VPN: {
                        connectionType = "VPN";
                        break;
                    }
                    case ConnectivityManager.TYPE_DUMMY: {
                        connectionType = "DUMMY";
                        break;
                    }
                    case ConnectivityManager.TYPE_MOBILE_DUN: {
                        connectionType = "MOBILE_DUN";
                        break;
                    }
                    case ConnectivityManager.TYPE_WIMAX: {
                        connectionType = "WIMAX";
                        break;
                    }
                    default:
                       connectionType = ConnectivityManager.EXTRA_OTHER_NETWORK_INFO;
              }
          } else {
                connectionType = ConnectivityManager.EXTRA_NO_CONNECTIVITY;
          }
     } catch (Exception e) {
          connectionType = "notAccessible";
     }

     try {
          clientInformation.put("model", Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer", Build.MANUFACTURER);
          clientInformation.put("version", Build.DEVICE);
          clientInformation.put("osName", "Android");
          clientInformation.put("osVersion", Build.VERSION.RELEASE);
          clientInformation.put("connectionType", connectionType);
          clientInformation.put("applicationId", "REF30");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

As informações do dispositivo podem ser construídas da seguinte maneira:

| Chave | Source | Valor (exemplo) |
|---------------|-----------------------------|-----------------|
| modelo | Build.MODEL | GT-I9505 |
| fornecedor | Build.BRAND | samsung |
| fabricante | Build.MANUFACTURER | samsung |
| version | Build.DEVICE | jflet |
| displayWidth | DisplayMetrics.widthPixels | 600 |
| displayHeight | DisplayMetrics.heightPixels | 800 |
| osName | codificado | Android |
| osVersion | Build.VERSION.RELEASE | 5.0.1 |

As informações de conexão podem ser construídas da seguinte maneira:

| Chave | Source | Valor (exemplo) |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
| connectionSecure |                                                                                                                                                               |                                                                                             |

As informações do aplicativo podem ser construídas da seguinte maneira:

| Chave | Source | Valor (exemplo) |
|---------------|-----------|-----------------|
| applicationId | codificado | REF30 |

### Dispositivos conectados à TV {#tv-connected-devices}

#### tvOS {#tvos}

Para compilar o cabeçalho `X-Device-Info` para dispositivos que executam o [tvOS](https://developer.apple.com/documentation/tvos-release-notes), consulte os seguintes documentos e o trecho de código abaixo:

* Documentação do desenvolvedor do Apple para [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice).
* Documentação do desenvolvedor do Apple para [Acessibilidade](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html).
* Documentação de manual do Linux para [uname](https://man7.org/linux/man-pages/man2/uname.2.html).

```C
+ (NSString *)computeClientInformation {        
        struct utsname u;
        uname(&u);

        NSString *hardware = [NSString stringWithCString:u.machine encoding:NSUTF8StringEncoding];

        UIDevice *device = [UIDevice currentDevice];

        NSString *deviceType;

        switch (UI_USER_INTERFACE_IDIOM()) {
            case UIUserInterfaceIdiomPhone:
                deviceType = @"MobilePhone";
                break;
            case UIUserInterfaceIdiomPad:
                deviceType = @"Tablet";
                break;
            case UIUserInterfaceIdiomTV:
                deviceType = @"TV";
                break;
            default:
                deviceType = @"Unknown";
        }

        CGRect screenRect = [[UIScreen mainScreen] bounds];
        NSNumber *screenWidth = @((float) screenRect.size.width);
        NSNumber *screenHeight = @((float) screenRect.size.height);

        Reachability *reachability = [Reachability reachabilityForInternetConnection];
        [reachability startNotifier];

        NetworkStatus status = [reachability currentReachabilityStatus];

        NSString *connectionType;

        if (status == NotReachable) {
            connectionType = @"notConnected";
        } else if (status == ReachableViaWiFi) {
            connectionType = @"WiFi";
        } else if (status == ReachableViaWWAN) {
            connectionType = @"cellular";
        }

        NSMutableDictionary *clientInformation = [@{
                @"type": deviceType,
                @"model": device.model,
                @"vendor": @"Apple",
                @"manufacturer": @"Apple",
                @"version": [hardware stringByReplacingOccurrencesOfString:device.model withString:@""],
                @"osName": device.systemName,
                @"osVersion": device.systemVersion,
                @"displayWidth": screenWidth,
                @"displayHeight": screenHeight,
                @"connectionType": connectionType,
                @"applicationId": @"REF30" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

As informações do dispositivo podem ser construídas da seguinte maneira:

| Chave | Source | Valor (exemplo) |
|---------------|------------------------|-----------------|
| modelo | uname.machine | AppleTV |
| fornecedor | codificado | Apple |
| fabricante | codificado | Apple |
| version | uname.machine | 8,1 |
| displayWidth | UIScreen.mainScreen | 1920 |
| displayHeight | UIScreen.mainScreen | 1080 |
| osName | UIDevice.systemName | tvOS |
| osVersion | UIDevice.systemVersion | 10,2 |

As informações de conexão podem ser construídas da seguinte maneira:

| Chave | Source | Valor (exemplo) |
|------------------|------------------------------------------|-----------------|
| connectionType | [CurrentReachabilityStatus de acessibilidade] |                 |
| connectionSecure |                                          |                 |

As informações do aplicativo podem ser construídas da seguinte maneira:

| Chave | Source | Valor (exemplo) |
|---------------|-----------|-----------------|
| applicationId | codificado | REF30 |

#### Acionar SO {#fireos}

Para criar o cabeçalho `X-Device-Info` para dispositivos que executam o [Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html), consulte os seguintes documentos:

* Documentação do desenvolvedor do Android para a classe [Build](https://developer.android.com/reference/android/os/Build.html).
* Documentação do desenvolvedor do Amazon para [Identificação de Dispositivos Fire TV](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html).

As informações do dispositivo podem ser construídas da seguinte maneira:

| Chave | Source | Valor (exemplo) |
|---------------|-----------------------------|-----------------|
| modelo | Build.MODEL | AFTM |
| fornecedor | Build.BRAND | Amazon |
| fabricante | Build.MANUFACTURER | Amazon |
| version | Build.DEVICE | montoya |
| displayWidth | DisplayMetrics.widthPixels |                 |
| displayHeight | DisplayMetrics.heightPixels |                 |
| osName | codificado | Android |
| osVersion | Build.VERSION.RELEASE | 5.1.1 |

As informações de conexão podem ser construídas da seguinte maneira:

| Chave | Source | Valor (exemplo) |
|------------------|--------|-----------------|
| connectionType |        |                 |
| connectionSecure |        |                 |

As informações do aplicativo podem ser construídas da seguinte maneira:

| Chave | Source | Valor (exemplo) |
|---------------|-----------|-----------------|
| applicationId | codificado | REF30 |

#### Roku OS {#rokuos}

Para criar o cabeçalho `X-Device-Info` para dispositivos que executam o [Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md), consulte os seguintes documentos:

* Documentação do desenvolvedor do Roku para [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md).

As informações do dispositivo podem ser construídas da seguinte maneira:

| Chave | Source | Valor (exemplo) |
|---------------|--------------------------------------------|-----------------|
| modelo | codificado | &quot;Roku&quot; |
| fornecedor | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| fabricante | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| version | ifDeviceInfo.GetModelDetails().ModelNumber | &quot;5303X&quot; |
| displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
| displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| osName | codificado | &quot;Roku&quot; |
| osVersion | ifDeviceInfo.getVersion() |                 |

As informações de conexão podem ser construídas da seguinte maneira:

| Chave | Source | Valor (exemplo) |
|-------------------|------------------------------------|---------------------------------------|
| connectionType | ifDeviceInfo.GetConnectionType() | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
| connectionSecure | codificado | true se a conexão for com fio |

As informações do aplicativo podem ser construídas da seguinte maneira:

| Chave | Source | Valor (exemplo) |
|---------------|-----------|-----------------|
| applicationId | codificado | REF30 |
