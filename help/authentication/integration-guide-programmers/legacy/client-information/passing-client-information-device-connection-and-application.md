---
title: Transmissão de informações do cliente (dispositivo, conexão e aplicativo)
description: Transmissão de informações do cliente (dispositivo, conexão e aplicativo)
exl-id: 0b21ef0e-c169-48ff-ac01-25411cfece1e
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1666'
ht-degree: 3%

---

# (Herdado) Transmissão de informações do cliente (dispositivo, conexão e aplicativo) {#pass-client-info}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

## Escopo {#pass-client-info-scope}

Este documento agrega detalhes e manuais para transmitir informações do cliente (dispositivo, conexão e aplicativo) de um aplicativo do programador para APIs REST de autenticação do Adobe Pass ou SDKs.

Os benefícios de fornecer informações aos clientes são:

* A capacidade de habilitar adequadamente a Autenticação da Base Inicial (HBA) no caso de alguns tipos de dispositivos e MVPDs que possam oferecer suporte a HBA.
* A capacidade de aplicar corretamente TTLs no caso de alguns tipos de dispositivos (por exemplo, configurar TTLs mais longos para sessões de autenticação em dispositivos conectados a TV).
* A capacidade de agregar adequadamente as métricas de negócios em relatórios detalhados entre tipos de dispositivos usando o ESM (Entitlement Service Monitoring, monitoramento do serviço de qualificação).
* Desbloqueia a capacidade de aplicar corretamente várias regras de negócios (por exemplo, degradação) em tipos específicos de dispositivos.

## Visão geral {#pass-client-info-overview}

As informações do cliente consistem em:

* **Dispositivo** informações sobre os atributos de hardware e software do dispositivo a partir do qual o usuário está tentando consumir o conteúdo do Programador.
* **Conexão** informações sobre os atributos de conexão do dispositivo a partir do qual o usuário está se conectando aos serviços de Autenticação da Adobe Pass e/ou aos serviços do Programador (por exemplo, implementações de servidor para servidor).
* Informações de **Aplicativo** sobre o aplicativo registrado de onde o usuário está tentando consumir o conteúdo do Programador.

As informações do cliente são um objeto JSON criado com chaves apresentadas na tabela a seguir.

>[!NOTE]
>
>As **chaves** a seguir são **obrigatórias** para serem enviadas no objeto JSON de informações do cliente: **modelo**, **osName**.
>
>As seguintes chaves têm **valores restritos**: `primaryHardwareType`, `osName`, `osFamily`, `browserName`, `browserVendor`, `connectionSecure`.

|   | Chave | Restrito | Descrição | Valores possíveis |
|---|---|---|---|---|
|            | primaryHardwareType | # Sim | O tipo de hardware principal do dispositivo. | # Os valores são restritos:                                                                     Câmera                                                      DataCollectionTerminal                                                      Desktop                                                      EmbeddedNetworkModule                                                      eReader                                                      GamesConsole                                                      GeolocationTracker                                                      Óculos                                                      MediaPlayer                                                      Celular                                                      TerminalDePagamento                                                      PluginModem                                                      DefinirCaixaSuperior                                                      TV                                                      Tablet                                                      Ponto de acesso sem fio                                                      Relógio de pulso                                                      Desconhecido |
| #mandatory | modelo | Não | O nome do modelo do dispositivo. | Por exemplo, iPhone, SM-G930V, Apple TV etc. |
|            | version | Não | A versão do dispositivo. | Por exemplo, 2.0.1, etc. |
|            | fabricante | Não | A empresa/organização de fabricação do dispositivo. | Por exemplo, Samsung, LG, ZTE, Huawei, Motorola, Apple, etc. |
|            | fornecedor | Não | A empresa/organização de venda do dispositivo. | Por exemplo, Apple, Samsung, LG, Google, etc. |
| #mandatory | osName | # Sim | O nome do sistema operacional do dispositivo. | # Os valores são restritos:                                                   Android                   SO CHROME                   Linux                   SO MAC                   OS X                   OpenBSD                   Roku OS                   Windows                   iOS                   tvOS                   webOS |
|            | osFamily | Sim | O nome do grupo do Sistema Operacional (SO) do dispositivo. | # Os valores são restritos:                                                   Android                   BSD                   Linux                   PlayStation OS                   Roku OS                   Symbian                   Tizen                   Windows                   iOS                   macOS                   tvOS                   webOS |
|            | osVendor | Não | O fornecedor do sistema operacional do dispositivo. | Amazon                   Apple                   Google                   LG                   Microsoft                   Mozilla                   Nintendo                   Nokia                   Roku                   Samsung                   Sony                   Projeto Tizen |
|            | osVersion | Não | A versão do sistema operacional do dispositivo. | Por exemplo, 10.2, 9.0.1, etc. |
|            | browserName | # Sim | O nome do navegador. | # Os valores são restritos:                                                   Navegador Android                   Chrome                   Edge                   Firefox                   Internet Explorer                   Opera                   Safari                   SeaMonkey                   Navegador Symbian |
|            | browserVendor | # Sim | A empresa/organização de construção do navegador. | # Os valores são restritos:                                                   Amazon                   Apple                   Google                   Microsoft                   Motorola                   Mozilla                   Netscape                   Nintendo                   Nokia                   Samsung                   Sony Ericsson |
|            | browserVersion | Não | A versão do navegador do dispositivo. | ex: 60.0.3112 |
|            | userAgent | Não | O agente do usuário do dispositivo. | por exemplo, Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, como Gecko) Versão/10.0.3 Safari/602.4.8 |
|            | displayWidth | Não | A largura da tela física do dispositivo. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayHeight | Não | A altura da tela física do dispositivo. |                                                                                                                                                                                                                                                                                                                                                           |
|            | displayPpi | Não | A densidade de pixels da tela física do dispositivo. | por exemplo, 294 |
|            | diagonalScreenSize | Não | A dimensão diagonal da tela física do dispositivo em polegadas. | Por exemplo, 5.5, 10.1 |
|            | connectionIp | Não | O IP do dispositivo usado para enviar solicitações HTTP. | Por exemplo, 8.8.4.4 |
|            | connectionPort | Não | A porta do dispositivo usada para enviar solicitações HTTP. | por exemplo, 53124 |
|            | connectionType | Não | O tipo de conexão de rede. | por exemplo, WiFi, LAN, 3G, 4G, 5G |
|            | connectionSecure | # Sim | O status de segurança da conexão de rede. | # Os valores são restritos:                                                   true - no caso de uma rede segura                   false - no caso de um ponto de acesso público |
|            | applicationId | Não | O identificador exclusivo do aplicativo. | por exemplo, CNN |

## Referências de API {#api-ref}

Esta seção apresenta a API responsável por manipular informações do cliente ao usar as APIs REST de autenticação da Adobe Pass ou SDKs.

### REST API {#rest-api}

Os serviços de Autenticação da Adobe Pass oferecem suporte para o recebimento de informações do cliente das seguintes maneiras:

* Como um cabeçalho **: &quot;X-Device-Info&quot;**
* Como um **parâmetro de consulta: &quot;device_info&quot;**
* Como um **parâmetro de postagem: &quot;device_info&quot;**

>[!IMPORTANT]
>
>Nos três cenários, a carga do cabeçalho ou parâmetro deve ser codificada em **Base64 e codificada em URL**.

**SDK**

#### JavaScript SDK {#js-sdk}

O AccessEnabler JavaScript SDK cria por padrão um objeto JSON de informações do cliente, que será passado para os serviços de autenticação da Adobe Pass, a menos que seja substituído.

O AccessEnabler JavaScript SDK oferece suporte **à substituição somente** da chave &quot;applicationId&quot; do objeto JSON de informações do cliente por meio do parâmetro de opções *applicationId* de [setRequestor](/help/authentication/integration-guide-programmers/legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md#setrequestor(inRequestorID,endpoints,options)).

>[!CAUTION]
>
>O valor do parâmetro `applicationId` deve ser um valor de cadeia de caracteres de texto sem formatação.
>Caso o aplicativo Programmer decida passar o applicationId, o restante das chaves de informações do cliente ainda será calculado pelo AccessEnabler JavaScript SDK.

#### iOS/tvOS SDK {#ios-tvos-sdk}

O AccessEnabler iOS/tvOS SDK cria por padrão um objeto JSON de informações do cliente, que será passado para os serviços de autenticação da Adobe Pass, a menos que seja substituído.

O AccessEnabler iOS/tvOS SDK oferece suporte à **substituição do objeto JSON de informações do cliente inteiro** por meio do parâmetro device_info de [setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setoptions).

>[!CAUTION]
>
>O valor do parâmetro *device_info* deve ser um valor **Base64 encoded** *NSString*.
>
>Caso o aplicativo Programador decida passar o *device_info*, todas as chaves de informações do cliente computadas pelo AccessEnabler iOS/tvOS SDK serão substituídas. Portanto, é muito importante calcular e transmitir os valores para o maior número possível de chaves. Para obter mais detalhes sobre a implementação, consulte a tabela [Visão geral](#pass-client-info-overview) e o [guia do iOS/tvOS](#ios-tvos).

#### Android/FireOS SDK {#and-fire-os-sdk}

O SDK do Android/FireOS `AccessEnabler` compila por padrão um objeto JSON de informações do cliente, que será passado para os serviços de Autenticação da Adobe Pass, a menos que seja substituído.

O SDK do `AccessEnabler` Android/FireOS oferece suporte à **substituição do objeto JSON de todas as** informações do cliente por meio do parâmetro `device_info` de [setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/android-sdk/android-sdk-api-reference.md#setOptions)&#39;s/[setOptions](/help/authentication/integration-guide-programmers/legacy/sdks/fireos-sdk/amazon-fireos-native-client-api-reference.md#fire_setOption).

>[!NOTE]
>
>O valor do parâmetro `device_info` deve ser um valor de Cadeia de Caracteres **Base64 codificado**.

>[!IMPORTANT]
>
>Caso o aplicativo Programador decida passar o `device_info`, todas as chaves de informações do cliente computadas pelo SDK do Android/FireOS `AccessEnabler` serão substituídas. Portanto, é muito importante calcular e transmitir os valores para o maior número possível de chaves. Para obter mais detalhes sobre a implementação, consulte a tabela [Visão geral](#pass-client-info-overview) e o guia do [Android](#android) e [FireOS](#fire-tv).

## Cookbooks {#cookbooks}

Esta seção apresenta um guia para criar o objeto JSON de informações do cliente no caso de diferentes tipos de dispositivos.

>[!IMPORTANT]
>
>As chaves que estão marcadas com **!** são obrigatórios para serem enviados.

### Android {#android}

As informações do dispositivo podem ser construídas da seguinte maneira:

|   | Chave | Source | Valor (exemplo) |
|---|---------------|-----------------------------|---------------|
| ! | modelo | Build.MODEL | GT-I9505 |
|   | fornecedor | Build.BRAND | samsung |
|   | fabricante | Build.MANUFACTURER | samsung |
| ! | version | Build.DEVICE | jflet |
|   | displayWidth | DisplayMetrics.widthPixels | 600 |
|   | displayHeight | DisplayMetrics.heightPixels | 800 |
| ! | osName | codificado | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.0.1 |

As informações de conexão podem ser construídas da seguinte maneira:

|   | Chave | Source | Valor (exemplo) |
|---|---|---|---|
| ! | connectionType | `<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>` `getSystemService(Context.CONNECTIVITY_SERVICE).getActiveNetworkInfo().getType()` | `"WIFI","BLUETOOTH","MOBILE","ETHERNET","VPN","DUMMY","MOBILE_DUN","WIMAX","notAccessible"` |
|   | connectionSecure |                                                                                                                                                           |                                                                                           |

As informações do aplicativo podem ser construídas da seguinte maneira:

|   | Chave | Source | Valor (exemplo) |
|---|---------------|-----------|--------------|
|   | applicationId | codificado | CNN |

>[!IMPORTANT]
>
>As informações do dispositivo, da conexão e do aplicativo devem ser adicionadas ao mesmo objeto JSON. Depois, o objeto resultante deve ser **Base64 codificado**. Além disso, no caso das REST APIs de autenticação da Adobe Pass, o valor deve ser **codificado por URL**.

**Código de exemplo**

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
          clientInformation.put("model",Build.MODEL);
          clientInformation.put("vendor", Build.BRAND);
          clientInformation.put("manufacturer",Build.MANUFACTURER);
          clientInformation.put("version",Build.DEVICE);
          clientInformation.put("osName","Android");
          clientInformation.put("osVersion",Build.VERSION.RELEASE);
          clientInformation.put("connectionType",connectionType);
          clientInformation.put("applicationId","CNN");
     } catch (JSONException e) {
          Log.e(LOGGING_TAG, e.getMessage());
     }

     return Base64.encodeToString(clientInformation.toString().getBytes(), Base64.NO_WRAP);
}
```

>[!NOTE]
>
>**Recursos:**
>* classe pública [build](https://developer.android.com/reference/android/os/Build.html){target=_blank} na documentação dos desenvolvedores Java.

### FireTV {#fire-tv}

As informações do dispositivo podem ser construídas da seguinte maneira:

|   | Chave | Source | Valor (por exemplo) |
|---|---------------|-----------------------------|--------------|
| ! | modelo | Build.MODEL | AFTM |
|   | fornecedor | Build.BRAND | Amazon |
|   | fabricante | Build.MANUFACTURER | Amazon |
| ! | version | Build.DEVICE | montoya |
|   | displayWidth | DisplayMetrics.widthPixels |              |
|   | displayHeight | DisplayMetrics.heightPixels |              |
| ! | osName | codificado | Android |
| ! | osVersion | Build.VERSION.RELEASE | 5.1.1 |

As informações de conexão podem ser construídas da seguinte maneira:

|   | Chave | Source | Valor (exemplo) |
|---|------------------|--------|---------------|
| ! | connectionType |        |               |
|   | connectionSecure |        |               |

As informações do aplicativo podem ser construídas da seguinte maneira:

|   | Chave | Source | Valor (exemplo) |
|---|---------------|-----------|--------------|
|   | applicationId | codificado | CNN |

>[!IMPORTANT]
>
>As informações do dispositivo, da conexão e do aplicativo devem ser adicionadas ao mesmo objeto JSON. Depois, o objeto resultante deve ser **Base64 codificado**. Além disso, no caso das REST APIs de autenticação da Adobe Pass, o valor deve ser **codificado por URL**.

>[!NOTE]
>
>**Recursos:**
>* classe pública [Build](https://developer.android.com/reference/android/os/Build.html){target=_blank} na documentação dos desenvolvedores do Android.
>* [Identificando dispositivos FireTV](https://developer.amazon.com/docs/fire-tv/identify-amazon-fire-tv-devices.html){target=_blank}

### iOS/tvOS {#ios-tvos}

As informações do dispositivo podem ser construídas da seguinte maneira:

|   | Chave | Source | Valor (exemplo) |
|---|---------------|------------------------|--------------|
| ! | modelo | uname.machine | iPhone |
|   | fornecedor | codificado | Apple |
|   | fabricante | codificado | Apple |
| ! | version | uname.machine | 8,1 |
|   | displayWidth | UIScreen.mainScreen | 320 |
|   | displayHeight | UIScreen.mainScreen | 568 |
| ! | osName | UIDevice.systemName | iOS |
| ! | osVersion | UIDevice.systemVersion | 10,2 |

As informações de conexão podem ser construídas da seguinte maneira:

|   | Chave | Source | Valor (exemplo) |
|---|------------------|-------------------------------------------|--------------|
| ! | connectionType | [CurrentReachabilityStatus de acessibilidade] |              |
|   | connectionSecure |                                           |              |


As informações do aplicativo podem ser construídas da seguinte maneira:

|   | Chave | Source | Valor (exemplo) |
|---|---------------|-----------|--------------|
|   | applicationId | codificado | CNN |

>[!IMPORTANT]
>
>As informações do dispositivo, da conexão e do aplicativo devem ser adicionadas ao mesmo objeto JSON. Posteriormente, o objeto resultante deve ser codificado na Base64. Além disso, no caso das REST APIs de autenticação da Adobe Pass, o valor deve ser codificado no URL.

**Código de exemplo**

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
                @"applicationId": @"CNN" 
        } mutableCopy];

        NSError *error;
        NSData *jsonData = [NSJSONSerialization dataWithJSONObject:clientInformation options:NSJSONWritingPrettyPrinted error:&error];
        NSString *base64Encoded = [jsonData base64EncodedStringWithOptions:0];

        return base64Encoded;
}
```

>[!NOTE]
>
>**Recursos:**
>* [UIDevice](https://developer.apple.com/documentation/uikit/uidevice#//apple_ref/occ/cl/UIDevice){target=_blank}
>* [uname](https://man7.org/linux/man-pages/man2/uname.2.html){target=_blank}
>* [Sobre Acessibilidade](https://developer.apple.com/library/archive/samplecode/Reachability/Introduction/Intro.html){target=_blank}

### Roku {#roku}

As informações do dispositivo podem ser construídas da seguinte maneira:

| Chave | Source | Valor (exemplo) |                 |
|-----|---------------|--------------------------------------------|-----------------|
| ! | modelo | codificado | &quot;Roku&quot; |
|     | fornecedor | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
|     | fabricante | ifDeviceInfo.GetModelDetails().VendorName | &quot;Sharp&quot;, &quot;Roku&quot; |
| ! | version | ifDeviceInfo.GetModelDetails().ModelNumber | &quot;5303X&quot; |
|     | displayWidth | ifDeviceInfo.GetDisplaySize().w | 1920 |
|     | displayHeight | ifDeviceInfo.GetDisplaySize().h | 1080 |
| ! | osName | codificado | &quot;Roku&quot; |
| ! | osVersion | ifDeviceInfo.getVersion() |                 |

As informações de conexão podem ser construídas da seguinte maneira:

|   | Chave | Source | Valor (exemplo) |
|---|---|---|---|
| ! | connectionType | ifDeviceInfo.GetConnectionType() | &quot;WifiConnection&quot;, &quot;WiredConnection&quot; |
|   | connectionSecure | codificado | true se a conexão for com fio |

As informações do aplicativo podem ser construídas da seguinte maneira:

|   | Chave | Source | Valor (exemplo) |
|---|---------------|-----------|--------------|
|   | applicationId | codificado | CNN |

>[!IMPORTANT]
>
>As informações do dispositivo, da conexão e do aplicativo devem ser adicionadas ao mesmo objeto JSON. Depois, o objeto resultante deve ser **Base64 codificado**. Além disso, no caso das REST APIs de autenticação da Adobe Pass, o valor deve ser codificado no URL.

>[!NOTE]
>
>Para obter mais informações, consulte [ifDeviceInfo](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md)

### XBOX 1/360 {#xbox}

As informações do dispositivo podem ser construídas da seguinte maneira:

|   | Chave | Source | Valor (exemplo) |
|---|---|---|---|
| ! | modelo | EasClientDeviceInformation.SystemProductName |                 |
|   | fornecedor | codificado | Microsoft |
|   | fabricante | codificado | Microsoft |
| ! | version | EasClientDeviceInformation.SystemHardwareVersion |                 |
|   | displayWidth | DisplayInformation.ScreenWidthInRawPixels | 1920 |
|   | displayHeight | DisplayInformation.ScreenHeightInRawPixels | 1080 |
| ! | osName | EasClientDeviceInformation.OperatingSystem |                 |
| ! | osVersion | EasClientDeviceInformation.SystemFirmwareVersion |                 |

As informações de conexão podem ser construídas da seguinte maneira:

|   | Chave | Source | Exemplo |
|---|---|---|---|
| ! | connectionType |                                                   |                   |
|   | connectionSecure | NetworkAuthenticationType | &quot;Nenhum&quot;, &quot;Wpa&quot; etc |

As informações do aplicativo podem ser construídas da seguinte maneira:

| Chave | Source | Valor (exemplo) |
|---|---|---|
| applicationId | codificado | CNN |

>[!IMPORTANT]
>
>As informações do dispositivo, da conexão e do aplicativo devem ser adicionadas ao mesmo objeto JSON. Depois, o objeto resultante deve ser **Base64 codificado**. Além disso, no caso das REST APIs de autenticação da Adobe Pass, o valor deve ser **codificado por URL**.

**Recursos**

* [Classe EasClientDeviceInformation](https://docs.microsoft.com/en-us/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation?view=winrt-22000)
* [Classe DisplayInformation](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.display.displayinformation?view=winrt-22000)
