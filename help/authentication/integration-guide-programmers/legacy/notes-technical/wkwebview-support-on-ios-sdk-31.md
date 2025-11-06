---
title: Suporte ao WKWebView no iOS SDK 3.1+
description: Suporte ao WKWebView no iOS SDK 3.1+
exl-id: 90062be0-1a0a-44ae-8d8e-f4d97a92b17a
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# Suporte ao WKWebView (herdado) no iOS SDK 3.1+ {#wkwebview-support-on-ios-sdk-3.1}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

</br>

**Devido à descontinuação do UIWebView do Apple no iOS, atualizamos o iOS SDK 3.1 com suporte para WKWebView.**

## Compatibilidade {#compatibility}

A partir do iOS SDK versão 3.1, os implementadores podem usar agora WKWebView ou UIWebView alternadamente. Como a UIWebView foi descontinuada pela Apple, os aplicativos devem migrar para a WKWebView para evitar problemas com versões futuras do iOS.

Observe que a migração implicaria simplesmente alternar a classe UIWebView com WKWebView; não há trabalho específico a ser feito em relação ao AccessEnabler da Adobe.

## Problemas conhecidos {#known-issues}

O AccessEnabler da Adobe usou uma instância UIWebView interna oculta para executar &quot;[autenticação passiva](/help/authentication/integration-guide-programmers/legacy/sso-access/sso-passive-authn.md)&quot; para determinados MVPDs. O fluxo &quot;passivo&quot; foi útil para MVPDs que exigem autenticação para cada ID de solicitante, e desse fluxo beneficiaram os programadores que usaram a mesma ID de equipe em vários aplicativos do iOS para simular uma experiência de SSO (Adobe SSO). Esse recurso é usado atualmente por um número limitado de MVPDs.

O recurso usava um comportamento do UIWebView que permitia que o Adobe capturasse os cookies de autenticação e os repetisse durante o fluxo &quot;passivo&quot;. A WKWebView apresenta uma segurança mais forte que impede o Adobe de capturar os cookies definidos no logon e repeti-los usando uma instância oculta da WKWebView. Devido a essa melhoria de segurança e considerando que o fluxo &quot;passivo&quot; beneficiou apenas um conjunto muito limitado de MVPDs em um cenário de implementação muito específico (vários aplicativos usando a mesma id de equipe), a Adobe removeu o recurso de &quot;autenticação passiva&quot; para MVPDs usando visualizações da Web para autenticação.

O recurso ainda está presente para MVPDs configurados para usar SFSafariViewController, mas observe que, nesse caso, a autenticação &quot;passiva&quot; estará visível para o usuário, pois SFSafariViewController não pode ser usado de uma maneira &quot;oculta&quot;.
