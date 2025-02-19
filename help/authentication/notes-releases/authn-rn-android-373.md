---
title: Notas de versão do Adobe Pass Authentication Android 3.7.3
description: Notas de versão do Adobe Pass Authentication Android 3.7.3
exl-id: f335357e-c209-428d-af2a-2181551447d4
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# Notas de versão do Adobe Pass Authentication Android 3.7.3 {#android-sdk-373-rn}

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Número da Build {#build-number-373}

Autenticação do Adobe Pass: Android 3.7.3

Data de lançamento: **09/19/2023**

## Visão geral da versão {#release-overview-373}

* Alterações no suporte ao Android 14 e aplicativos que direcionam o nível 34 da API
   * Adicione o sinalizador necessário para [receptores de difusões registradas em tempo de execução do Android 14](https://developer.android.com/about/versions/14/behavior-changes-14#runtime-receivers-exported).
* Corrigir ChromeCustomTabs que não abrem para logon do MVPD na API do emulador 32+
   * Observação: uma solução alternativa para esse problema no SDK &lt;3.7.3 é abrir o aplicativo Chrome no emulador e concluir a configuração antes de tentar fazer logon no MVPD

## Lançar pacote {#release-package-373}

Você pode baixar o Android SDK v3.7.3 de [aqui](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library).
