---
title: Usando o Charles Proxy
description: Usando o Charles Proxy
exl-id: bb38543f-f6bc-4b5a-91b8-41bc51ee4c56
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# Usando o Charles Proxy {#using-charles-proxy}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.


**Charles:** <http://charlesproxy.com>


## Download, instalação e introdução ao Charles Proxy {#download-install-and-get-stared-with-charles-proxy}

- **Baixar** - <http://www.charlesproxy.com/download/>
- **Instalar** - <http://www.charlesproxy.com/documentation/installation/>
- **Introdução** - <http://www.charlesproxy.com/documentation/getting-started/>


## Guias Estrutura vs Sequência {#structure-vs-sequence-tabs}

Há duas maneiras diferentes de exibir o tráfego:

1. **Estrutura** - As solicitações são agrupadas por host
1. **Sequência** - As solicitações são listadas na ordem em que são chamadas


## SSL e certificados {#ssl-and-certificates}

Habilitar proxy SSL `\[ *Proxy -\> Proxy Settings... -\> SSL* \]`

Marque a caixa de seleção &quot;Ativar proxy SSL&quot; e adicione todos os locais HTTPS.


![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/ProxySettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/SSLSettings.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/AddHttpsLocations.PNG)



- Proxying SSL - <http://www.charlesproxy.com/documentation/proxying/ssl-proxying/>
- Certificados SSL - <http://www.charlesproxy.com/documentation/using-charles/ssl-certificates/>
- Proxying SSL de dispositivos móveis - Consulte abaixo.


## Ignorar / Excluir Hosts {#ignore-/-exclude-hosts}

Se a saída ficar muito desorganizada, você poderá optar por ignorar ou excluir locais. Você pode ignorar ou excluir locais seguindo um destes procedimentos:

- Clique com o botão direito do mouse nas solicitações que deseja ignorar e selecione &quot;Ignorar&quot;
- Adicionar manualmente os locais a serem excluídos de `\[ *Proxy -\> Recording Settings... -\> Exclude* \]`


## Falsificação de DNS {#dns-spoffing}

`\[ *Tools -\> DNS Spoofing...* \]`



A falsificação de DNS é muito útil ao tentar redirecionar uma solicitação para um IP diferente, especialmente ao trabalhar com dispositivos móveis:

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/DNSSpoofing.PNG)

<http://www.charlesproxy.com/documentation/tools/dns-spoofing/>


## Remota do mapa {#map-remote}

`\[ *Tools -\> Map Remote...* \]`



Com o Map Remote, é possível redirecionar uma solicitação &quot;recebida&quot; para um endpoint diferente. O caso de uso mais comum para este recurso é &quot;Mapear&quot; `AccessEnabler.swf` para `AccessEnablerDebug.swf:`

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemote.PNG) ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/MapRemoteAdd.PNG)

<http://www.charlesproxy.com/documentation/tools/map-remote/>



## Proxy reverso {#reverse-proxy}

<http://www.charlesproxy.com/documentation/proxying/reverse-proxy/>

## Celular {#mobile}

### Usar o Charles em um dispositivo iOS (iPhone/iPad) {#use-charles-on-an-ios-device-(iphone-/-ipad)}

#### Conexão SSL do iPhone {#ssl-connection-from-iphone}

Navegue até <http://charlesproxy.com/charles.crt> no seu dispositivo iOS.  Isso iniciará a caixa de diálogo de instalação do certificado:

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate1\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate2\(1\).PNG)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceSSLCertificate3.PNG)

</br>

Clique em `\[ *Install*... *Install*... *Done* \]` para concluir a instalação do certificado.

<http://www.charlesproxy.com/documentation/faqs/ssl-connections-from-within-iphone-applications/>



#### Uso do Charles em um dispositivo iOS {#using-charles-from-an-ios-device}

Em seu dispositivo iOS, selecione `\[ *Settings* -\> *Wi-FI* -\> (*YOUR\_WIFI\_NETWORK)* \]`. Clique na pequena seta azul ao lado da sua rede, vá para HTTP Proxy e selecione &quot;Manual&quot;:


</br>

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy1.png)![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy2.PNG)


</br>
Aqui você precisa especificar o IP e a porta da máquina em que o Charles está sendo executado. <span style="line-height: 1.6em;">Se você abrir o Safari agora em seu dispositivo iOS e tentar abrir uma página da Web, deverá obter o seguinte pop-up na máquina que está executando o Charles:

</br>

![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/iOSDeviceManualProxy3.PNG)

</br>
Clique em "Permitir" para permitir que o dispositivo use Charles para fazer proxy de todos os seus
solicitações.

<http://www.charlesproxy.com/documentation/faqs/using-charles-from-an-iphone/>


#### iOS - Confiar em quaisquer certificados {#ios-trust-any-certificates}

<http://stackoverflow.com/questions/933331/how-to-use-nsurlconnection-to-connect-with-ssl-for-an-untrusted-cert>

#### Erro de autenticação do iOS - adobepass.ios.app não pode ser encontrado

<https://tve.zendesk.com/entries/22135907-ios-authentication-error-adobepass-ios-app-cannot-be-found>


## Usar o Charles para Android

<http://www.charlesproxy.com/documentation/configuration/browser-and-system-configuration>


Navegue até [Charles proxy](http://charlesproxy.com/charles.crt) do seu dispositivo Android.
