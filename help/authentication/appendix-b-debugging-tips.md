---
title: Apêndice B "Dicas de depuração"
description: Apêndice B "Dicas de depuração"
exl-id: ea024797-315e-47c0-99ea-1ac49c8c9697
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# Apêndice B: Dicas de depuração {#appendix-b-debugging-tips}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.


## Limpando Dados Temporários {#clearing-temporary-data}

A Autenticação do Adobe Pass armazena dados temporários, como cache do navegador, cache de LSOs e cookies. Limpar dados temporários é importante para garantir que você fique limpo ao testar.

- [Limpeza do cache do navegador e cookies](#clearing-the-browser-cache-and-cookies)
- [Limpando cache de LSOs](#clearing-lsos-cache)


## Limpeza do cache do navegador e cookies {#clearing-the-browser-cache-and-cookies}

É dependente do navegador, mas no Firefox: &quot;Ferramentas&quot; -\> &quot;Limpar histórico recente...&quot; -\> Em &quot;Intervalo de tempo a limpar:&quot; selecione &quot;Tudo&quot;; e em &quot;Detalhes&quot;: marque a opção &quot;Cookies&quot; e &quot;Cache&quot; -\> Clique em &quot;Limpar agora&quot;.


## Limpando cache de LSOs {#clearing-lsos-cache}

Acesse o [Ajuda do Flash Player](http://www.macromedia.com/support/documentation/en/flashplayer/help/settings_manager07.html).

Selecione o ```entitlement.\*``` (dependendo do que for testado) e clique em &quot;Excluir site&quot;.


## Ferramentas de depuração {#tools}

Os engenheiros de autenticação da Adobe Pass usam as seguintes ferramentas de depuração:

- Firebug - <http://www.getfirebug.com/>
- Flashbug (funciona com a versão de depuração do flash player)
- Violador - <http://www.fiddler2.com/fiddler2/>
- Charles - <http://www.charlesproxy.com/>
- Wireshark - <http://www.wireshark.org/>


<!--
## Related Information

- [Programmer Integration Guide](/help/authentication/programmer-integration-guide-overview.md)

- [Using Charles Proxy (Tech Note)](https://tve.zendesk.com/hc/en-us/articles/204962849-Using-Charles-Proxy)
-->
