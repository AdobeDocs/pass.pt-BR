---
title: Definir um segmento e um intervalo de tempo
description: Definir um segmento e um intervalo de tempo
exl-id: 86fe010d-3202-4ce2-b803-ff44f5538d7e
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 0%

---

# Definir um segmento e um intervalo de tempo {#define-segment}

Todas as análises ou exibições de relatórios no [!UICONTROL Account IQ] comece com a definição do segmento e a seleção do intervalo de tempo para avaliação. [Segmento](/help/accountiq/product-concepts.md#segmet-def) refere-se a todos os assinantes ou visualizadores que atendem aos seus critérios (assinatura de um MVPD e exibição de canais específicos) de avaliação.

![](assets/segment-panel.png)

*Figura: Seleção do segmento e do intervalo de tempo*

Na parte superior de todas as páginas de relatórios no [!UICONTROL Account IQ], há um painel para definir o segmento selecionando MVPDs, programadores de canal, granularidade e intervalo de tempo.

## Seleção de segmento {#select-segment}

### Selecionar MVPDs no segmento {#select-segment-mvpds}

Para selecionar MVPDs de **[!UICONTROL MVPDs in segment]** opção:

1. Clique ou toque no **[!UICONTROL MVPDs in segment]** opção suspensa.

   >[!NOTE]
   >
   >**Todos** Os MVPDs do setor são selecionados por padrão. Aqui, é possível selecionar uma das opções **Os 10 principais MVPDs por pontuação de compartilhamento**, **Os 10 principais MVPDs por uso**, **Os 10 principais MVPDs por contas** ou MVPDs individuais. No entanto, para selecionar MVPDs individuais, é necessário desmarcar **Todos**.

1. Clique ou toque nos MVPDs desejados.

   Você pode remover um MVPD da seleção desmarcando-o.

1. Clique ou toque **[!UICONTROL Apply selection]** para que sua seleção tenha efeito. Caso contrário, você perderá a seleção feita.

   >[!NOTE]
   >
   >Se você selecionar o modo Isolamento, nenhum dos outros MVPDs poderá ser selecionado.

### Selecionar canais no segmento {#select-segment-channels}

Para selecionar os canais do programador desejados na **[!UICONTROL Channels in segment]** opção:

1. Clique ou toque no **[!UICONTROL Channels in segment]** opção suspensa.

   >[!NOTE]
   >
   >**Todos** os canais do programador da sua empresa são selecionados por padrão. Para selecionar canais ou programadores individuais, é necessário primeiro desmarcar **Todos**.

1. Clique ou toque nos canais ou programadores desejados.

   Os itens da lista de nível superior no **[!UICONTROL Channels in segment]** são [programador](/help/accountiq/product-concepts.md#programmer-def) empresas e os itens da lista sob os nomes dos programadores são seus [canais](/help/accountiq/product-concepts.md#channel-def). Você pode selecionar canais individuais em programadores ou selecionar programadores e todas as atividades dos canais nesse programador são incluídas nos resultados do relatório e do gráfico.

   ![](assets/programmer-channels.png)


   *Figura: Programadores e canais listados no seletor de canais*

   >[!IMPORTANT]
   >
   >Os resultados da seleção de canais individuais sob um programador não são os mesmos que aqueles da seleção do programador.
   >
   >
   >Quando você seleciona canais individuais, as atividades desses canais são detalhadas individualmente em alguns relatórios. No entanto, quando você seleciona o programador principal de todos esses canais, todas as atividades desses canais são incluídas, mas não são detalhadas individualmente nos relatórios.

1. Clique ou toque **[!UICONTROL Apply selection]** para que sua seleção tenha efeito.

>[!NOTE]
>
>Não é possível selecionar mais de 10 itens nos menus suspensos MVPD ou programador.

### Desmarcar MVPDs e canais {#deselect-segment-mvpds-channels}

Além de alterar a seleção no campo **[!UICONTROL MVPDs in segment]** e **[!UICONTROL Channels in segment]** seletores de segmento, é possível desmarcar os MVPDs e canais selecionados anteriormente da seguinte maneira:

* Selecionar o **[!UICONTROL Remove]** ícone (![ícone remover](assets/remove-icon.png)) nos nomes desses MVPDs e canais selecionados exibidos abaixo do seletor de segmentos.

* Também é possível usar **[!UICONTROL Clear Selection]** para remover todos os MVPDs ou canais selecionados anteriormente.

![](assets/segment-panel-selection.png)

*Figura: MVPDs e canais selecionados no segmento e no painel de intervalo de tempo*

## Granularidade e seleção do intervalo de tempo {#granularity-timeframe}

Para selecionar um período de avaliação:

1. Selecione o **[!UICONTROL Granularity and time frame]** seletor de datas.

1. Selecione **[!UICONTROL Week]** ou **[!UICONTROL Month]** de **[!UICONTROL Aggregate By]** opção para definir a granularidade da sua avaliação.

   ![](assets/granularity-timeframe-weekwise.png)


   *Figura: Seletor de datas para selecionar a granularidade e o intervalo de tempo*

1. Depois de selecionar a granularidade, você pode usar as setas para frente ou para trás para avançar ou retroceder no tempo.

1. Especifique um período no passado (em mês ou semana com base na granularidade selecionada) para avaliação.

1. Selecionar **[!UICONTROL Apply Selection]** para garantir que sua seleção seja efetivada.
