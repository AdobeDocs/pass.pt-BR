---
title: Painéis de dados no painel
description: O painel ajuda a identificar as instâncias de compartilhamento de senha analisando uma grande variedade de dados do assinante.
source-git-commit: 88b11527b2a432c2cd27bf9e29fd286969036eb0
workflow-type: tm+mt
source-wordcount: '889'
ht-degree: 0%

---

# Painéis de dados no painel {#data-panels}

Depois de selecionar um segmento e um intervalo de tempo, o painel exibe vários painéis de dados, tabelas e gráficos que refletem uma visualização de alto nível da atividade de compartilhamento no segmento selecionado.

A tabela abaixo descreve a disponibilidade e as diferenças entre os painéis de dados em diferentes [versões](/help/accountiq/versions-aiq.md) do Account IQ:

| Painéis de dados | Serviços D2C | Programadores de TVE | MVPDs TVE |
|---|---|---|---|
| [Pontuação de compartilhamento média agregada para o segmento atual](#aggregated-sharing) | Disponível e consistente | Disponível e consistente | Disponível e consistente |
| [Categorias de vídeo no segmento](#video-categories-segment) | Disponível com pequenas variações | Disponível com pequenas variações | Disponível com pequenas variações |
| [Pontuação de compartilhamento por canais e MVPDs](#sharin-score-by-channels-and-mvpds) | Indisponível | Disponível | Indisponível |
| [Probabilidade de compartilhamento de contas](#accounts-sharing-probability) | Disponível e consistente | Disponível e consistente | Disponível e consistente |
| [Número de contas e uso por nível de probabilidade de compartilhamento](#number-of-accounts-usage-sharing-probability) | Disponível e consistente | Disponível e consistente | Disponível e consistente |


## Pontuação média de compartilhamento agregada para o segmento atual {#aggregated-sharing}

O painel de pontuação média de compartilhamento fornece uma leitura de linha superior resumindo a quantidade e o impacto do compartilhamento em termos de contas e volume de transmissão.

As métricas ajudam você a entender a magnitude (variando de baixa, média, alta a anormal) do compartilhamento de credenciais por seus assinantes, medida em termos de contas e consumo.

![](assets/aggregate-sharing-score.png)


*Painel de pontuação média de compartilhamento agregado para o segmento atual*

>[!NOTE]
>
> O indicador azul na janela **Pontuação média de compartilhamento agregada para o segmento atual** serve diferentes finalidades para os serviços D2C em comparação com a TV Everywhere. Para os serviços D2C, representa o **Índice médio de serviço** conforme mostrado na imagem anterior. Se você fizer logon como Programador ou MVPD, esse rótulo será alterado para **Índice médio da indústria**.

As métricas a seguir são componentes do painel Pontuação média de compartilhamento.

### Nível de compartilhamento {#sharing-level}

O medidor do nível de compartilhamento mostra o percentual de todas as contas de assinantes compartilhadas dentro do segmento definido durante o intervalo selecionado.

A porcentagem é calculada com base em uma média da probabilidade de compartilhamento calculada para cada conta no segmento. Esse cálculo inclui contas que foram transmitidas pelo menos uma vez durante o intervalo selecionado.

O indicador de Tendência mostra a alteração de percentual no valor da métrica em relação ao intervalo anterior.

![](assets/sharing-level.png){width="350" align="left"}


*Nível de compartilhamento*

### Uso de contas compartilhadas {#usage-from-shared-accounts}

O medidor indica a porcentagem de uso pelas contas compartilhadas entre todas as contas do assinante para o segmento e o período de tempo definidos. Esses intervalos, chamados de Baixo, Médio, Alto e Anormal, baseiam-se nas médias do setor.

O indicador de Tendência, que descreve um aumento ou uma queda no uso de contas compartilhadas em comparação ao intervalo de tempo anterior.

![](assets/usage-4mshared-accounts.png){width="350" align="left"}


*Uso de contas compartilhadas*

### Pontuação geral de compartilhamento {#overall-sharing-score}

A pontuação de compartilhamento geral é uma combinação de pontuações de compartilhamento, incluindo &quot;Nível de compartilhamento&quot; e &quot;Uso de contas compartilhadas&quot;.

Ele fornece uma pontuação que reflete o impacto geral do compartilhamento. Seu propósito é semelhante ao de uma pontuação de crédito, resumindo o nível de compartilhamento com um único número. Mas, nesse caso, uma pontuação mais alta indica um nível de compartilhamento maior.

![](assets/overall-sharing-score.png){width="350" align="left"}


*Pontuação geral de compartilhamento*

## Categorias de vídeo no segmento {#video-categories-segment}

É possível selecionar os cabeçalhos da coluna para classificar os dados em todas as versões do Account IQ.

+++Serviços D2C: Regiões no segmento

Ao efetuar login como um serviço D2C, **Regiões no segmento** A tabela fornece uma exibição comparativa das diferentes pontuações de compartilhamento agregadas para o [categorias de vídeo](/help/accountiq/product-concepts.md#video-category-def) no segmento atual.

![](assets/sharing-scores-by-regions-in-segment.png)

*Compartilhamento de pontuação por regiões no segmento*

>[!NOTE]
>
> A variável [categorias de vídeo](product-concepts.md#video-category-def)  mostrada na imagem anterior, como **Regiões** no segmento é apenas um exemplo. Quando você faz logon no Account IQ, esse painel exibe a categoria de vídeo específica da sua empresa.

Selecionar **Exportar** para baixar os dados em um arquivo .csv. Saiba mais [como exportar relatórios do painel de dados](/help/accountiq/export-reports.md).

+++

+++Programadores: MVPDs no segmento

Ao fazer logon como Programador, **MVPDs no segmento** A tabela fornece uma exibição comparativa das diferentes pontuações de compartilhamento agregadas para os MVPDs no segmento atual.

![](assets/sharing-scores-by-mvpds-in-segment.png)

Selecionar **Exportar** para baixar os dados em um arquivo .csv. Saiba mais [como exportar relatórios do painel de dados](/help/accountiq/export-reports.md).

+++

+++MVPDs: Programadores no segmento

Ao fazer logon como um MVPD, **Programadores no segmento** A tabela fornece uma exibição comparativa das diferentes pontuações de compartilhamento agregadas para os Programadores no segmento atual.

Selecione os cabeçalhos de coluna para classificar os dados.

![](assets/sharing-scores-by-programmers-in-segment.png)

*Pontuação de compartilhamento por programadores no segmento*

Selecionar **Exportar** para baixar os dados em um arquivo .csv. Saiba mais [como exportar relatórios do painel de dados](/help/accountiq/export-reports.md).

+++

## Pontuação de compartilhamento por canais e MVPDs  {#sharin-score-by-channels-and-mvpds}

Quando você faz logon como Programador, esta tabela fornece uma exibição comparativa das pontuações de compartilhamento dos canais selecionados para os MVPDs no segmento atual.

Selecione os cabeçalhos de coluna para classificar os dados.

![](assets/sharing-scores-by-channels-mvpds.png)


*Compartilhamento de pontuações por canais e MVPDs*

## Probabilidade de compartilhamento de contas {#accounts-sharing-probability}

As partições deste gráfico são contabilizadas em intervalos de quintis de probabilidade de compartilhamento, variando de muito baixo (0-20%) a muito alto (80-100%). Leia mais sobre os intervalos de [Probabilidade de compartilhamento de conta](#accounts-sharing-probability).

>[!NOTE]
>
>O gráfico de barras usa uma escala logarítmica.


![](assets/dashboard-ac-sharing-prob.png)


*Números e porcentagens de contas de assinantes em diferentes intervalos de probabilidade de compartilhamento*


## Número de contas e uso por nível de probabilidade de compartilhamento {#number-of-accounts-usage-sharing-probability}

Esse painel fornece uma exibição tabular de contas particionadas em intervalos de quintis de probabilidade de compartilhamento, variando de muito baixo (0-20%) a muito alto (80-100%), com cada uso associado de quintil a partir de contas compartilhadas. Leia mais sobre os intervalos de [Probabilidade de compartilhamento de conta](#accounts-sharing-probability).

![](assets/no-acc-usage-prob-level.png)

*Número de contas, tendências e usos que estão em vários intervalos de probabilidade*

Selecionar **Exportar** para baixar os dados em um arquivo .csv. Saiba mais [como exportar relatórios do painel de dados](/help/accountiq/export-reports.md).

