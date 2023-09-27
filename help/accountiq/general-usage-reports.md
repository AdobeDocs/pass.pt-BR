---
title: Relatórios de uso geral
description: Relatórios de uso geral
exl-id: 1272073a-61fe-47ec-aced-2e8055b6b11e
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '1275'
ht-degree: 0%

---

# [!UICONTROL General Usage] relatórios {#general-usage-reports}

[!UICONTROL Account IQ] relatórios são ferramentas analíticas básicas e relatórios que permitem que você faça drill-down em seus dados para isolar [coortes](/help/accountiq/product-concepts.md#segmet-def)O, identifica anomalias e cria uma compreensão das características da sua conta.

[!UICONTROL General Usage] A página Relatórios fornece ferramentas para separar métricas de subgrupos com base no número de dispositivos de conta em uso, IPs detectados e respectivos códigos postais.

<!--Divide the content in cohorts.

Content filters
device filters

segment and definition replicate to cohorts. Number of people and number of account that ......
content consumption.....-->

Todos os relatórios se baseiam no segmento atual selecionado usando [Segmentos e intervalo de tempo](/help/accountiq/howto-select-segment-timeframe.md) painel. Você pode ajustar a seleção e restringi-la ainda mais especificando limites (número de dispositivos, número de IPs e número de códigos postais) no [Visão Geral do Instantâneo - Contas acima dos limites](#snapshot-overview) painel.

<!--To view General Usage Reports:

1. Select the desired MVPDs from the **MVPDs in Segment** option.

2. Select the desired programmer channels from the **Channels in Segment** Option.

3. Select an appropriate time frame from the **Granularity and time frame** option.

   Using the above options you have defined segments for your analysis. Based on your segment selection, following graphs and reports are displayed.

4. You can fine tune your selection and further narrow it down by specifying (number of devices, number of IPs, and number of zip codes) thresholds in [Snapshot Overview - Accounts above thresholds](#snapshot-overview) widget/panel.-->

## AuthN OK / AuthZ OK / Solicitações de reprodução / Assinantes únicos {#authn-authz-playreq-uniquesubs}

Os gráficos de linha aqui fornecem uma visualização das alterações ao longo do tempo nos valores de AuthN OK, AuthZ OK, Solicitações de reprodução e Assinantes únicos em um intervalo de tempo selecionado para o segmento definido.

+++Programador- **AuthN OK / AuthZ OK / Solicitações de reprodução / Assinantes únicos**

![](assets/progr-line-graph-gu.png)


*Figura: AuthN OK / AuthZ OK / Solicitações de reprodução / Assinantes únicos para o usuário programador*


+++


+++MVPD- **AuthN OK / AuthZ OK / Assinantes únicos**

![](assets/mvpd-line-graph-gu.png)


*Figura: AuthN OK / AuthZ OK / Assinantes únicos para usuário MVPD*


+++

O eixo x apresenta as unidades no intervalo de tempo atual e o eixo y representa as métricas básicas de atividade do assinante durante esse período. Os gráficos de linha permitem comparar os seguintes valores para os assinantes de MVPDs e canais selecionados no painel de seleção de segmentos:

* **AuthN OK**

  Autenticação OK é o número de autenticações bem-sucedidas. Para obter mais informações e definições, consulte [Conceitos do produto: AuthN OK](/help/accountiq/product-concepts.md#authn-ok-def).

* **AuthZ OK**

  AuthZ OK é o número de autorizações bem-sucedidas. Para obter mais informações e definições, consulte [Conceitos do produto: AuthZ OK](/help/accountiq/product-concepts.md#authz-ok-def).

* **Solicitações Play**

  Solicitações de reprodução são o número de Solicitações de reprodução. Para obter mais informações e definições, consulte [Conceitos do produto: Solicitações de reprodução](/help/accountiq/product-concepts.md#play-requests-def)

  >[!NOTE]
  >
  >O gráfico de linha de solicitações de reprodução não está disponível para usuários do MVPD.


* **Assinantes únicos**

  Assinantes únicos são o número de assinantes únicos bem-sucedidos. Para obter mais informações e definições, consulte [Conceitos do produto: assinantes únicos](/help/accountiq/product-concepts.md#unique-subscriber-def)

  >[!NOTE]
  >
  >O número total de assinantes únicos também inclui o número de dispositivos únicos se o uso de Adobe TempPass por um programador (ou seja, pré-visualização gratuita) fizer parte do segmento.

## Visão Geral do Instantâneo - Contas acima dos limites {#snapshot-overview}

Ajuste a análise e os relatórios usando esse filtro adicional para definir vários limites de uso. Depois de definir seu segmento (ou coorte) para análise selecionando os MVPD(s) e canais desejados, você também pode usar os seguintes filtros para analisar o comportamento dos assinantes:

* Limite de número de dispositivos

* Limite de número de IPs

* Limite do número de códigos postais

Ao atualizar valores de limite no [Segmento de contas - com base nos limites selecionados](#account-segments-basedon-segments) você pode exibir o efeito em:

* [Dispositivos por semana (ou mês) por conta](#devices-week-account)

* [Locais por semana (ou mês) por conta](#locations-week-account)

* [IPs por semana (ou mês) por conta](#ip-week-account)

* [Visualização histórica do segmento de contas](#account-segment-historical-view)

>[!NOTE]
>
>O valor padrão para cada limite é 4. Ou seja, a página Uso geral mostra análises de MVPDs com assinantes que usam quatro (e mais de quatro) dispositivos, consumindo conteúdo de quatro (e mais) localizações geográficas diferentes e quatro (e mais) códigos postais diferentes.

### Segmento de contas - com base nos limites selecionados {#account-segments-basedon-segments}

A variável **Segmento de contas - com base nos limites selecionados** O painel oferece opções para definir limites (entre 1 e 10) para o número de dispositivos, o número de IPs e o número de CEPs.

O gráfico mostra:

* número absoluto de contas de assinantes e

* porcentagem do total de contas do assinante nesse segmento,

  que estão usando X número de dispositivos, Y número de IPs e Z número de códigos Zip para consumir conteúdo do seu canal para o (segmento definido de) MVPDs, para um intervalo de tempo.

![](assets/select-thresholds.png)

## Dispositivos por semana (ou mês) por conta {#devices-week-account}

A variável **gráfico de barras** O fornece insights sobre o comportamento de uso em termos de como os assinantes estão usando seus dispositivos para acessar conteúdo.

O eixo x representa o Número de contas e o eixo y representa o Número de dispositivos. Com base no limite definido para o número de dispositivos por conta, ele marca o número absoluto de contas de assinantes que consomem conteúdo de um número específico de dispositivos durante uma semana.

![](assets/bar-gr-devices-w-acc.png)

Ao passar o cursor do mouse sobre uma barra (específica do número de dispositivos), um rótulo é exibido fornecendo informações sobre o número de contas de assinantes (e a porcentagem do total de contas de assinantes no segmento) que estão transmitindo conteúdo do canal usando esses vários dispositivos em uma semana.

O gráfico também marca o seguinte:

* Uma linha vermelha para marcar o limite definido.

* Uma linha verde para marcar a média do número de dispositivos diferentes usados por uma conta de assinante por semana (ou mês).

É possível comparar o nível de limite com a média semanal do número de diferentes dispositivos usados por uma conta, para julgar o nível de compartilhamento.

O gráfico também fornece uma ideia da porcentagem de contas de assinantes que estão usando mais dispositivos do que o limite definido.

O gráfico de rosca ajuda a avaliar a magnitude das contas dos assinantes que consomem conteúdo de canal usando dispositivos além do limite definido (em um período) rapidamente.

![](assets/donut-devices-w-acc.png)

## Locais por semana (ou mês) por conta {#locations-week-account}

Curtir [Dispositivos por semana (ou mês) por conta](#devices-week-account), a métrica Locais por semana (ou mês) por conta ajuda a analisar o uso da conta do assinante em diferentes locais para identificar mais detalhadamente o compartilhamento de senhas. O eixo x representa o Número de contas e o eixo y representa o Número de locais.

Os resultados desta métrica combinados com o número de [Dispositivos por semana (ou mês) por conta](#devices-week-account) e número de [IPs por semana (ou mês) por conta](#ip-week-account) Ajudar você a julgar com mais precisão as instâncias de compartilhamento de senha; de modo que os usuários autênticos não sejam contados em.

![](assets/graph-loc-week-acc.png)

Depois de definir um segmento e definir o limite para o número de locais, você pode identificar no gráfico:

* Número (e porcentagem) de assinantes que estão consumindo conteúdo de (um específico) x número de locais em uma semana.

* Porcentagem do total de contas de assinantes que estão exibindo conteúdo de mais locais do que o limite.

* Comparar a média semanal (número de locais diferentes para uma conta) com o Limite.

## IPs por semana (ou mês) por conta {#ip-week-account}

Semelhante [Dispositivos por semana (ou mês) por conta](#devices-week-account) e [Locais por semana (ou mês) por conta](#locations-week-account), o **Número de IPs por semana por conta** permite analisar o compartilhamento de senhas com mais precisão e granularidade.

O eixo x representa o Número de contas e o eixo y representa o Número de IPs.

![](assets/graph-ip-week-acc.png)

Depois de definir um segmento (selecionando MVPDs e canais) e definir o limite para o número de IPs, você pode identificar no gráfico:

* Número (e porcentagem) de assinantes que estão consumindo conteúdo de (um específico) x número de IP em uma semana.

* Porcentagem do total de contas de assinantes que estão exibindo conteúdo de mais endereços IP do que o limite.

* Compare a média semanal (número de IPs diferentes para uma conta) com o Limite.

## Segmento de contas - exibição histórica {#account-segment-historical-view}

O gráfico de barras Exibição histórica ajuda a comparar as métricas de uso em diferentes períodos. Além disso, representa coletivamente as várias métricas de uso, como [Dispositivos por semana (ou mês) por conta](#devices-week-account), [Locais por semana (ou mês) por conta](#locations-week-account), e [IPs por semana (ou mês) por conta](#ip-week-account).

* O eixo x representa o período e o eixo y representa o número de contas de assinantes, dispositivos, locais e IPs.

* As barras coloridas laranja significam segmentos em vários intervalos de tempo.

* O gráfico de linhas representa as alterações no [Dispositivos por semana (ou mês) por conta](#devices-week-account), [Locais por semana (ou mês) por conta](#locations-week-account), e [IPs por semana (ou mês) por conta](#ip-week-account) valores no período com base no limite.

![](assets/historical-view.png)

* As barras azuis significam o número total de assinantes ativos em todo o setor para um intervalo de tempo.

* É possível selecionar legendas específicas para ajudá-lo a dimensionar o gráfico.

![](assets/historical-view-total.png)

>[!MORELIKETHIS]
>
>* Saiba como exportar relatórios para os 1000 principais assinantes no segmento selecionado usando filtros no Relatório de uso geral usando [Exportar as 1000 contas principais](/help/accountiq/export-acc-information.md) opção.
