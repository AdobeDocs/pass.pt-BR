---
title: Acesse o Logon único (SSO) do SDK do Android Enabler nos aplicativos do Android 10
description: Acesse o Logon único (SSO) do SDK do Android Enabler nos aplicativos do Android 10
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 0%

---

# Acesse o Logon único (SSO) do SDK do Android Enabler nos aplicativos do Android 10 {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral

O Logon único (SSO) entre aplicativos alimentados pela Autenticação da Adobe Pass está disponível em dispositivos que usam o sistema operacional da Android por meio do SDK do Android do Access Enabler. Para oferecer Logon único (SSO) em dispositivos Android, o SDK do Android do Access Enabler versão 3.2.1 (mais recente) e as versões anteriores usam um arquivo de banco de dados compartilhado salvo em uma implementação de armazenamento do Android, acessível por todos os aplicativos habilitados pela Autenticação Adobe Pass.

No entanto, o Google na versão mais recente do Android 10 produziu algumas alterações &quot;para dar aos usuários mais controle sobre seus arquivos e limitar a desorganização de arquivos, os aplicativos que direcionam o Android 10 (nível 29 da API) e superior recebem acesso com escopo em um dispositivo de armazenamento externo, ou armazenamento com escopo, por padrão. Estes aplicativos podem ver somente seu diretório específico do aplicativo `\[...\]`&quot;. Mais detalhes relacionados a essas alterações de armazenamento do Android 10 são apresentados em [Documentação de armazenamento de dados e arquivos do Android](https://developer.android.com/training/data-storage/files/external-scoped).

Como resultado dessas alterações, o Logon Único (SSO) oferecido pelo SDK do Access Enabler Android versão **3.2.1 (mais recente)** e as versões anteriores podem ser afetados em dispositivos Android 10, conforme explicado na próxima seção.

## Comportamento

Dependendo do **[!UICONTROL target SDK level]** do seu aplicativo ou do uso do atributo de manifesto **android:requestLegacyExternalStorage**, o Logon Único (SSO) oferecido pelo SDK 3.2.1 do Access Enabler Android (mais recente) e as versões anteriores se comportarão da seguinte maneira no momento:

- Seu aplicativo é direcionado ao **Android 9 (nível de API 28)** ou inferior **-\>** Logon Único (SSO) **funcionará**
- Seu aplicativo é direcionado ao **Android 10** **(nível de API 29)** e **define** o valor de **requestLegacyExternalStorage como true** no arquivo de manifesto do seu aplicativo **-\>** Logon Único (SSO) **funcionará**
- Seu aplicativo é direcionado ao **Android 10** **(nível de API 29)** e **não define** o valor de **requestLegacyExternalStorage para true** no arquivo de manifesto do seu aplicativo **-\>** Logon Único (SSO) **não funcionará**

>[!TIP]
>
> Antes de o SDK do Android do Adobe Pass Authentication Access Enabler ser totalmente compatível com o armazenamento com escopo, você pode recusar temporariamente com base no nível de SDK de destino do seu aplicativo ou no atributo de manifesto requestLegacyExternalStorage, conforme explicado na [documentação pública do Android](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage).
