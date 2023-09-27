---
title: Relatórios de Contas compartilhadas
description: Relatórios de contas compartilhadas
exl-id: 16c5ded1-2a95-4373-8b90-b445131f333a
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 0%

---

# Relatórios de Contas Compartilhadas {#shared-accounts-reports}

Os Relatórios de Contas compartilhadas detalham as métricas como número de dispositivos e tipos de dispositivos pelo intervalo selecionado de probabilidade de compartilhamento, por exemplo **[!UICONTROL Over Moderate Probability]** e **[!UICONTROL Over Low Probability]** para o segmento atual.

Esses intervalos podem servir como limites definidos pelo usuário e os gráficos são atualizados com base nos limites selecionados.

O Account IQ classifica todas as contas de assinantes do segmento definido nas contas com as cinco categorias a seguir com base em suas probabilidades de compartilhamento:

* Muito alto (80%-100%)
* Alta (60%-80%)
* Moderado (40%-60%)
* Baixa (20%-40%)
* Muito baixo (0%-20%)

## Probabilidade de Compartilhamento de Contas {#accounts-sharing-probability}

O gráfico de rosca aqui categoriza e mostra as porcentagens (e números absolutos) das contas dos assinantes de várias categorias de probabilidade.

A linha vermelha marca o intervalo limite selecionado pelos usuários em [Contas acima do limite no segmento atual](#threshold-selector) painel.

![](assets/accounts-sharing-probability-pie.png)

O gráfico de barras representa o número de contas no eixo y para várias categorias de probabilidades de compartilhamento (representadas no eixo x).

![](assets/accounts-sharing-probability-bar.png)

A linha vermelha marca o intervalo de limite e pode ser ajustada no gráfico de barras. O limite ajustado no gráfico de barras reflete no intervalo de limites no gráfico de rosca.

<!--![](assets/shared-accounts-rep.gif)-->

### Contas acima do limite no segmento atual{#threshold-selector}

Esse painel permite selecionar um intervalo entre os seguintes como limite para contas de assinantes (com base em suas probabilidades de compartilhamento):

* Contas **muito baixo** compartilhamento **probabilidade**

* Contas **sobre baixo** compartilhamento **probabilidade**

* Contas **excessivamente moderado** compartilhamento **probabilidade**

* Contas **acima do alto** compartilhamento **probabilidade**

![](assets/threshold-selector-shared-accounts.png)

Depois de selecionar o limite, o painel mostra a porcentagem (e o número) de contas de todas as contas do assinante no segmento selecionado.

## Segmento - Reproduzir solicitações do total {#play-request-out-total}

O gráfico de rosca mostra a porcentagem (e o número) de solicitações de reprodução feitas pelos assinantes no segmento e permite comparar as solicitações de reprodução feitas pelos assinantes que não estão no segmento definido.

![](assets/play-req-outof-total.png)

Ao mover o cursor no gráfico de rosca, ele também mostra porcentagens e números do assinante de vários intervalos de probabilidade.

<!--![](assets/play-request-total.gif)-->

## Número médio de dispositivos do segmento por conta{#avg-devices-account}

O gráfico de barras mostra o número médio de dispositivos de cada tipo de dispositivo em uso pelos assinantes no segmento atual e pelos assinantes que não estão no segmento atual.

![](assets/avg-devices-per-acc.png)

## Segmento - códigos postais por período por conta {#zip-codes-period-account}

Este gráfico informa sobre o número de assinantes que estão consumindo conteúdo de diferentes locais em um intervalo de tempo.

![](assets/zip-period-account.png)

Você pode ampliar para restringir e exibir especificidades de uma barra no gráfico que representa um intervalo de locais.

<!--![](assets/zip-code-period.gif)-->

## Segmento - Extensão geográfica / Período / Conta {#geo-span-period-account}

Este gráfico de barras representa o número de contas do assinante em relação a diferentes intervalos geográficos em milhas. O intervalo se baseia na distância máxima entre os locais a partir dos quais um assinante transmitiu durante o intervalo de tempo.

<!--Total number of users ...

How many accounts are within 99 miles of each other.....and how many are apart. 

Based on points on the map.-->

![](assets/geogr-span-account.png)

Quando você seleciona uma barra representando um intervalo de distância geográfica, ela expande o intervalo para mostrar mais detalhes.

<!--![](assets/geo-span-period-acc.gif)-->
