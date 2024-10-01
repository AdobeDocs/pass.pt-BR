---
title: Cabeçalho - AP-Identificador de dispositivo
description: REST API V2 - Cabeçalho - AP-Identificador de dispositivo
exl-id: 90a5882b-2e6d-4e67-994a-050465cac6c6
source-git-commit: 8f4fb5d6cc8b45b300010438c56d4af2e8fc0a76
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 1%

---

# Cabeçalho - AP-Identificador de dispositivo {#header-ap-device-identifier}

>[!NOTE]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview}

O cabeçalho de solicitação <b>AP-Device-Identifier</b> contém o identificador do dispositivo de streaming conforme foi criado pelo aplicativo cliente.

## Sintaxe {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Identificador-dispositivo-AP</b>: &lt;tipo&gt; &lt;identificador&gt;</td>
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

<b>&lt;tipo></b>

O tipo de identificador do dispositivo.

Há apenas um tipo compatível, conforme apresentado abaixo.

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Tipo</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>impressão digital</td>
      <td>
            O identificador do dispositivo consiste em um identificador estável e exclusivo criado e gerenciado pelo aplicativo cliente.
            <br/>
            O aplicativo cliente deve impedir alterações de valor causadas pelas ações do usuário, como desinstalação, reinstalação ou atualizações de aplicativos.
      </td>
   </tr>
</table>


<b>&lt;identificador></b>

O valor `Base64-encoded` do identificador do dispositivo.

## Exemplo {#example}

```JSON
// device identifier
// ba23d141-d715-561c-94f4-e9e4c966b1eb

// Base64-encoded
// YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi

AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
```

## Cookbooks {#cookbooks}

>[!IMPORTANT]
>
> Os recursos de documentação são fornecidos para fins de referência.
>
> Os recursos de documentação não são exaustivos e podem exigir modificações adicionais para funcionar em seu projeto.
> 
> Independentemente da implementação real, o cabeçalho `AP-Device-Identifier` deve conter um valor formatado conforme descrito na seção [Diretivas](#directives).

### Navegadores {#browsers}

Para criar o cabeçalho `AP-Device-Identifier` para dispositivos em execução em um navegador, o aplicativo cliente requer o cálculo de um identificador estável e exclusivo com base nos dados disponíveis, como navegador, dispositivo ou dados específicos do usuário.

_(*) Recomendamos integrar uma biblioteca ou serviço que forneça um mecanismo de impressão digital de navegador ou dispositivo._

### Dispositivos móveis {#mobile-devices}

#### iOS e iPadOS {#ios-ipados}

Para criar o cabeçalho `AP-Device-Identifier` para dispositivos que executam o [iOS ou o iPadOS](https://developer.apple.com/documentation/ios-ipados-release-notes), consulte os seguintes documentos:

* Documentação do desenvolvedor do Apple para [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor).

_(*) Recomendamos aplicar uma função de hash SHA-256 sobre o valor fornecido pelo sistema operacional._

#### Android {#android}

Para criar o cabeçalho `AP-Device-Identifier` para dispositivos que executam o [Android](https://developer.android.com/about/versions), consulte os seguintes documentos:

* Documentação do desenvolvedor do Android para [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID).

_(*) Recomendamos aplicar uma função de hash SHA-256 sobre o valor fornecido pelo sistema operacional._

### Dispositivos conectados à TV {#tv-connected-devices}

#### tvOS {#tvos}

Para compilar o cabeçalho `AP-Device-Identifier` para dispositivos que executam o [tvOS](https://developer.apple.com/documentation/tvos-release-notes), consulte os seguintes documentos:

* Documentação do desenvolvedor do Apple para [identifierForVendor](https://developer.apple.com/documentation/uikit/uidevice/1620059-identifierforvendor).

_(*) Recomendamos aplicar uma função de hash SHA-256 sobre o valor fornecido pelo sistema operacional._

#### Acionar SO {#fireos}

Para criar o cabeçalho `AP-Device-Identifier` para dispositivos que executam o [Fire OS](https://developer.amazon.com/docs/fire-tv/fire-os-overview.html), consulte os seguintes documentos:

* Documentação do desenvolvedor do Android para [ANDROID_ID](https://developer.android.com/reference/android/provider/Settings.Secure#ANDROID_ID).

_(*) Recomendamos aplicar uma função de hash SHA-256 sobre o valor fornecido pelo sistema operacional._

#### Roku OS {#rokuos}

Para criar o cabeçalho `AP-Device-Identifier` para dispositivos que executam o [Roku OS](https://developer.roku.com/docs/developer-program/release-notes/roku-os-release-notes.md), consulte os seguintes documentos:

* Documentação do desenvolvedor do Roku para [GetChannelClientId](https://developer.roku.com/docs/references/brightscript/interfaces/ifdeviceinfo.md#getchannelclientid-as-string).

_(*) Recomendamos aplicar uma função de hash SHA-256 sobre o valor fornecido pelo sistema operacional._
