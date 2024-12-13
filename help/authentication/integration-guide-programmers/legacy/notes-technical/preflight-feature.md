---
title: Recurso de comprovação, como ativar, solucionar ou determinar o problema
description: Recurso de comprovação, como ativar, solucionar ou determinar o problema
exl-id: 9e4ec343-371f-4116-915f-191e5f42cb47
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '499'
ht-degree: 0%

---

# Recurso de comprovação (herdado): como ativar, solucionar problemas ou determinar o problema {#preflight-feature}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Houve uma mudança na maneira como a Autenticação do Adobe Pass calcula preAuthorizeResources. A API de pré-autorização tem uma nova implementação. Essa implementação substitui a solução antiga, que consiste em fazer várias chamadas de autorização somente.
A interface externa da API de pré-autorização não foi alterada. Nenhuma atualização é necessária no aplicativo do programador.

Há três maneiras de calcular os recursos de Comprovação:

* **Fork and join method to MVPD**: envolve o Adobe fazer várias chamadas de autorização para o MVPD (embora o cliente ainda tenha que fazer uma chamada de comprovação).
* **Linha de canal**: a MVPD expõe a linha de canal para o usuário conectado na resposta de autenticação SAML e o Adobe retorna os recursos autorizados com base nisso. A resposta authN do SAML no rastreador SAML deve expor essa lista.
* **Autorização multicanal**: o cliente e a autenticação Adobe fazem uma única chamada para a MVPD para um conjunto de recursos.

Independentemente do MVPD, o aplicativo cliente fará uma única chamada para o endpoint de Comprovação (checkPreauthorizedResources API), transmitindo um conjunto de resourceIDs. Com base em uma das formas descritas acima com suporte do MVPD, o Adobe retornará as resourceIDs pré-autorizadas.

Se a Comprovação for baseada no método fork &amp; join, o back-end de Autenticação do Adobe Pass verificará um valor definido para o &quot;máximo de chamadas de pré-autorização&quot; em sua configuração. Isso é configurado pelo Adobe.

O valor padrão para a configuração &quot;máximo de chamadas de pré-autorização&quot; é &quot;5&quot;, o que significa que, no máximo, apenas 5 recursos podem ser enviados na Comprovação para os MVPDs fork &amp; join. Transmitir mais de 5 recursos resultará em uma exceção e uma lista nula será retornada. Esse é o comportamento esperado. Podemos configurá-lo com qualquer valor se o MVPD não for compatível com a programação de canais ou autorização de vários canais, mas somente depois de consultá-los, pois várias chamadas de autorização de bifurcação e ingresso aumentarão o tempo de carregamento.

Consequentemente, estes são os itens que devem ser observados ao ativar/solucionar problemas de simulação de uma MVPD:

* O método compatível com o MVPD (bifurcação e junção, linha de canal ou vários canais).
* Se somente houver suporte para bifurcação e junção, o Programador precisará ser perguntado quantas resourceIDs ele enviará na chamada de Comprovação.
* O MVPD precisa ser consultado e precisa saber o impacto de fazer um número &quot;n&quot; de chamadas de autorização de bifurcação e junção. Posteriormente, o valor deve ser configurado na configuração se for maior que 5.

**Limitação**

Observe que não obtemos nenhuma resourceID de volta da chamada de Comprovação para alguns MVPDs, como AT&amp;T e TWC, se qualquer uma das resourceIDs for uma ID falsa ou uma ID não reconhecida na lista de resourceIDs enviadas na chamada de comprovação, mesmo que também tenhamos recursos válidos e autorizados nessa lista.
