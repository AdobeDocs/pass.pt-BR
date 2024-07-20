---
title: Relatórios de contas compartilhadas
description: Relatórios de contas compartilhadas
exl-id: 16c5ded1-2a95-4373-8b90-b445131f333a
source-git-commit: 85316a40ba5f6564c84a5aecf689c077e936a91a
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 0%

---

# Relatórios de contas compartilhadas {#shared-accounts-reports}

Os Relatórios de contas compartilhadas fornecem outro grupo de gráficos que refletem o comportamento de compartilhamento e o consumo do segmento atual. Por exemplo, **[!UICONTROL Over Moderate Probability]** e **[!UICONTROL Over Low Probability]** para o segmento atual.

## Probabilidade de compartilhamento de contas {#accounts-sharing-probability}

Esses gráficos de rosca e de barra mostram as porcentagens (e números absolutos) das contas de assinantes que se enquadram em intervalos específicos de probabilidade de compartilhamento. Esses intervalos são definidos como:

* Muito alto (80%-100%)
* Alta (60%-80%)
* Moderado (40%-60%)
* Baixa (20%-40%)
* Muito baixo (0%-20%)

A linha vermelha marca o intervalo de limite selecionado no painel [Contas acima do limite no segmento atual](#threshold-selector) e a área vermelha clara contém o total de todas as contas acima desse limite.

![](assets/accounts-sharing-probability-pie.png)

O gráfico de barras representa o número de contas que se enquadram em cada intervalo no eixo y para cada um dos intervalos (representados no eixo x).

![](assets/accounts-sharing-probability-bar.png)

Aqui novamente, a linha vermelha marca o limite atual, e a área vermelha clara contém o total de todas as contas acima desse limite.

>[!NOTE]
>
> O eixo y do gráfico de barras é logarítmico.

### Contas acima do limite no segmento atual{#threshold-selector}

Esse painel permite selecionar o intervalo de limites para os gráficos de rosca e de barras acima. As quatro opções são:

* Contas **acima de muito baixo** compartilhando **probabilidade**

* Contas **muito baixas** compartilhando **probabilidade**

* Contas **acima do moderado** compartilhando **probabilidade**

* Contas **acima do alto** compartilhando **probabilidade**

![](assets/threshold-selector-shared-accounts.png)

Depois de selecionar o limite, o painel mostra a porcentagem (e o número) de contas de todas as contas do assinante no segmento selecionado.

## Solicitações de reprodução de segmento do total {#play-request-out-total}

O gráfico de rosca mostra a porcentagem (e o número) de solicitações de reprodução feitas pelos assinantes no segmento e permite comparar as solicitações de reprodução feitas pelos assinantes que não estão no segmento definido.

![](assets/play-req-outof-total.png)

Ao mover o cursor sobre o gráfico de rosca, ele também mostra porcentagens e números do assinante de vários intervalos de probabilidade.

<!--![](assets/play-request-total.gif)-->

## Número médio de segmentos de dispositivos por conta{#avg-devices-account}

O gráfico de barras mostra o número médio de dispositivos de cada tipo que estão sendo usados atualmente pelos assinantes no segmento atual e daqueles que não estão no segmento atual.

![](assets/avg-devices-per-acc.png)

## Códigos postais de segmento por período por conta {#zip-codes-period-account}

Este gráfico informa sobre o número de assinantes no segmento atual que estão consumindo conteúdo de diferentes locais (conforme medido pelo código postal) para o intervalo de tempo determinado.

![](assets/zip-period-account.png)

>[!NOTE]
>
>É possível ampliar as barras que representam mais de um conjunto de códigos postais, representados por um sinal de **+** (mais) (por exemplo, 10+), clicando duas vezes neles.


## Segmento-intervalo geográfico por período por conta {#geo-span-period-account}

Este gráfico de barras representa o número de contas de assinantes que consomem conteúdo de locais que se enquadram em diferentes intervalos geográficos em milhas. O intervalo se baseia na distância máxima entre os locais a partir dos quais um assinante transmitiu durante o intervalo de tempo.

![](assets/geogr-span-account.png)

>[!NOTE]
>
> É possível ampliar as barras que representam mais de um conjunto de distâncias geográficas, representadas com um sinal de **+** (mais) (por exemplo, 1000+), clicando duas vezes nelas.

>[!MORELIKETHIS]
>
>* Saiba como exportar relatórios para os 1.000 assinantes principais no segmento selecionado usando filtros em Relatórios de Contas compartilhadas usando a opção [Exportar 1.000 contas principais](/help/accountiq/export-acc-information.md).
