---
title: Recurso de comprovação, como ativar, solucionar ou determinar o problema
description: Recurso de comprovação, como ativar, solucionar ou determinar o problema
exl-id: 9e4ec343-371f-4116-915f-191e5f42cb47
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '521'
ht-degree: 0%

---

# Recurso de comprovação (herdado): como ativar, solucionar problemas ou determinar o problema {#preflight-feature}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

Houve uma mudança na maneira como a Autenticação do Adobe Pass calcula preAuthorizeResources. A API de pré-autorização tem uma nova implementação. Essa implementação substitui a solução antiga, que consiste em fazer várias chamadas de autorização somente.
A interface externa da API de pré-autorização não foi alterada. Nenhuma atualização é necessária no aplicativo do programador.

Há três maneiras de calcular os recursos de Comprovação:

* **Fork and join method to MVPD**: envolve a Adobe fazendo várias chamadas de autorização para a MVPD (embora o cliente ainda tenha que fazer uma chamada de comprovação).
* **Linha de canal**: a MVPD expõe a linha de canal para o usuário conectado na resposta de autenticação SAML e a Adobe retorna os recursos autorizados com base nisso. A resposta authN do SAML no rastreador SAML deve expor essa lista.
* **Autorização de vários canais**: a autenticação de cliente e Adobe faz uma única chamada à MVPD para um conjunto de recursos.

Independentemente do MVPD, o aplicativo cliente fará uma única chamada para o endpoint de Comprovação (checkPreauthorizedResources API), transmitindo um conjunto de resourceIDs. Com base em uma das formas acima compatíveis com o MVPD, a Adobe retornará as resourceIDs pré-autorizadas.

Se a Comprovação for baseada no método fork &amp; join, o back-end de Autenticação do Adobe Pass verificará um valor definido para o &quot;máximo de chamadas de pré-autorização&quot; em sua configuração. Isso é configurado pela Adobe.

O valor padrão para a configuração &quot;máximo de chamadas de pré-autorização&quot; é &quot;5&quot;, o que significa que, no máximo, apenas 5 recursos podem ser enviados na Comprovação para os MVPDs fork &amp; join. Transmitir mais de 5 recursos resultará em uma exceção e uma lista nula será retornada. Esse é o comportamento esperado. Podemos configurá-lo com qualquer valor se o MVPD não for compatível com a programação de canais ou autorização de vários canais, mas somente depois de consultá-los, pois várias chamadas de autorização de bifurcação e ingresso aumentarão o tempo de carregamento.

Consequentemente, estes são os itens que devem ser observados ao ativar/solucionar problemas de simulação de uma MVPD:

* O método compatível com o MVPD (bifurcação e junção, linha de canal ou vários canais).
* Se somente houver suporte para bifurcação e junção, o Programador precisará ser perguntado quantas resourceIDs ele enviará na chamada de Comprovação.
* O MVPD precisa ser consultado e precisa saber o impacto de fazer um número &quot;n&quot; de chamadas de autorização de bifurcação e junção. Posteriormente, o valor deve ser configurado na configuração se for maior que 5.

**Limitação**

Observe que não obtemos nenhuma resourceID de volta da chamada de Comprovação para alguns MVPDs, como AT&amp;T e TWC, se qualquer uma das resourceIDs for uma ID falsa ou uma ID não reconhecida na lista de resourceIDs enviadas na chamada de comprovação, mesmo que também tenhamos recursos válidos e autorizados nessa lista.
