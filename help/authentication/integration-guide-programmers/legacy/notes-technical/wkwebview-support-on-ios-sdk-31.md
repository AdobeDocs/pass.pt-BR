---
title: Suporte ao WKWebView no iOS SDK 3.1+
description: Suporte ao WKWebView no iOS SDK 3.1+
exl-id: 90062be0-1a0a-44ae-8d8e-f4d97a92b17a
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 0%

---

# Suporte a WKWebView (herdado) no iOS SDK 3.1+ {#wkwebview-support-on-ios-sdk-3.1}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

</br>

**Devido à descontinuação do UIWebView do Apple no iOS, atualizamos o iOS SDK 3.1 com suporte para WKWebView.**

## Compatibilidade {#compatibility}

A partir do iOS SDK versão 3.1, os implementadores podem usar agora WKWebView ou UIWebView alternadamente. Como a UIWebView foi descontinuada pela Apple, os aplicativos devem migrar para a WKWebView para evitar problemas com versões futuras do iOS.

Observe que a migração implicaria simplesmente alternar a classe UIWebView com WKWebView; não há trabalho específico a ser feito em relação ao Adobe AccessEnabler.

## Problemas conhecidos {#known-issues}

Adobe AccessEnabler usou uma instância UIWebView interna oculta para executar &quot;[autenticação passiva](/help/authentication/integration-guide-programmers/legacy/sso-access/sso-passive-authn.md)&quot; para determinados MVPDs. O fluxo &quot;passivo&quot; foi útil para MVPDs que exigem autenticação para cada ID de solicitante, e desse fluxo beneficiou os programadores que usaram a mesma ID de equipe em vários aplicativos do iOS para simular uma experiência de SSO (Adobe SSO). Esse recurso é usado atualmente por um número limitado de MVPDs.

O recurso usava um comportamento do UIWebView que permitia ao Adobe capturar os cookies de autenticação e repeti-los durante o fluxo &quot;passivo&quot;. A WKWebView apresenta uma segurança mais forte que impede que o Adobe capture os cookies definidos no logon e os reproduza novamente usando uma instância oculta do WKWebView. Devido a essa melhoria de segurança e considerando que o fluxo &quot;passivo&quot; beneficiou apenas um conjunto muito limitado de MVPDs em um cenário de implementação muito específico (vários aplicativos usando a mesma id de equipe), o Adobe removeu o recurso de &quot;autenticação passiva&quot; para MVPDs usando visualizações da Web para autenticação.

O recurso ainda está presente para MVPDs configurados para usar SFSafariViewController, mas observe que, nesse caso, a autenticação &quot;passiva&quot; estará visível para o usuário, pois SFSafariViewController não pode ser usado de uma maneira &quot;oculta&quot;.
