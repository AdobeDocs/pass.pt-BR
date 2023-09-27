---
title: Acesse o Logon único (SSO) do SDK do Android Enabler em aplicativos Android 10
description: Acesse o Logon único (SSO) do SDK do Android Enabler em aplicativos Android 10
exl-id: dedade15-c451-4757-b684-d3728e11dd87
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# Acesse o Logon único (SSO) do SDK do Android Enabler em aplicativos Android 10 {#access-enabler-android-sdk-single-sign-on-sso-on-android-10-apps}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral

O Logon único (SSO) entre aplicativos alimentados pela Autenticação da Adobe Pass está disponível em dispositivos que usam o Android OS por meio do SDK Android do Access Enabler. Para oferecer o Logon único (SSO) em dispositivos Android, o SDK do Android do Access Enabler versão 3.2.1 (mais recente) e as versões anteriores usam um arquivo de banco de dados compartilhado salvo em uma implementação de armazenamento Android, acessível por todos os aplicativos habilitados pela Autenticação da Adobe Pass.

No entanto, o Google na versão mais recente do Android 10 produziu algumas alterações &quot;para dar aos usuários mais controle sobre seus arquivos e limitar a desordem dos arquivos, os aplicativos direcionados ao Android 10 (nível de API 29) e superior recebem acesso com escopo em um dispositivo de armazenamento externo, ou armazenamento com escopo, por padrão. Esses aplicativos podem ver somente o diretório específico do aplicativo `\[...\]`&quot;. Mais detalhes relacionados a essas alterações no armazenamento do Android 10 são apresentados em [Documentação de armazenamento de dados e arquivos para Android](https://developer.android.com/training/data-storage/files/external-scoped).

Como resultado dessas alterações, o Logon único (SSO) oferecido pela versão do Access Enabler Android **3.2.1 SDK (mais recente)** As versões anteriores podem ser afetadas em dispositivos Android 10, conforme explicado na próxima seção.

Consulte [Visão geral do SSO do Roku](/help/authentication/roku-sso-overview.md).

## Comportamento

Dependendo do **nível de SDK de destino** ou a utilização de **android:requestLegacyExternalStorage** atributo manifest o Single Sign-On (SSO) oferecido pelo Access Enabler Android versão 3.2.1 SDK (mais recente) e as versões anteriores se comportarão atualmente da seguinte maneira:

- Seus destinos de aplicativo **Android 9 (API nível 28)** ou inferior **-\>** Logon único (SSO) **funcionará**
- Seus destinos de aplicativo **Android 10** **(Nível de API 29)** e faz **set** o valor de **requestLegacyExternalStorage para verdadeiro** no arquivo de manifesto do aplicativo **-\>** Logon único (SSO) **funcionará**
- Seus destinos de aplicativo **Android 10** **(Nível de API 29)** e faz **não definido** o valor de **requestLegacyExternalStorage para verdadeiro** no arquivo de manifesto do aplicativo **-\>** Logon único (SSO) **não funcionará**


>[!TIP]
>
> Antes de o SDK do Android do Adobe Pass Authentication Access Enabler ser totalmente compatível com o armazenamento com escopo, você pode recusar temporariamente com base no nível de SDK de destino do seu aplicativo ou no atributo de manifesto requestLegacyExternalStorage, conforme explicado em público [Documentação do Android](https://developer.android.com/training/data-storage/files/external-scoped#opt-out-of-scoped-storage).
