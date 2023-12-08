---
title: Segmentos do assinante e intervalo de tempo
description: Defina coortes ou selecione segmentos de assinante para medir as possibilidades de compartilhamento de conta e os padrões dos visualizadores de canal para usar ferramentas gráficas e relatórios no Account IQ.
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: 6b790728f3d6a8eed5dfc0f8b3d0dad283af6418
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 0%

---


# Segmentos do assinante e intervalo de tempo {#cohorts-segments}


Ao fazer logon no Account IQ, o painel iniciador de segmentos na parte superior permite especificar o assinante [segmento](/help/accountiq/product-concepts.md#segment-segmet-def). Isso ajuda a filtrar os resultados ao exibir relatórios sobre o comportamento e os padrões de compartilhamento do assinante. Um segmento padrão chamado Todas as contas nas propriedades já está selecionado e você vê as seguintes opções no iniciador de segmentos:

![](assets/new-segment-selector-collapsed.png){width="800" align="left"}

*Figura: Iniciador de segmentos com resumo de segmentos recolhido*

**A** Nome do segmento selecionado no momento<br/>
**B** Seletor de intervalo de tempo e granularidade<br/>
**C** Resumo do segmento recolhido<br/>
**D** Opção para expandir o resumo do segmento<br/>
**E** Dados do segmento (em termos de número de contas de assinantes no segmento por um período de tempo)<br/>
**F** Opção Abrir lista de segmentos<br/>
**G** Opção Editar segmento<br/>
**H** Opção Criar novo segmento<br/>

## Seleção de segmento {#segment-selection}

Para programadores ou usuários do MVPD, navegue até o **Abrir segmento** opção. Escolha um segmento na lista e selecione **Abrir segmento** para exibir os relatórios de compartilhamento de conta.

Use o **Olho** ícone para exibir o resumo detalhado do segmento, apresentando as informações sobre o número de contas de assinantes e solicitações de reprodução feitas por eles dentro do intervalo escolhido.

+++Painel de seleção de segmentos para programadores/MVPDs

![](assets/segment-panel-programmers-mvpds.png) {width="800" align="left"}

*Figura: Painel de segmentos para programadores/MVPDs*

+++

O resumo de segmentos é usado para definir os seguintes parâmetros:

**[!UICONTROL Programmers in segment]**

**[!UICONTROL Channels in segment]**

**[!UICONTROL MVPD in segment]**

**[!UICONTROL Metrics in segment]**

<!-- The definitions of these parameters will be defined in the glossary article-->

## [!UICONTROL Granularity and time interval] {#granularity-timeinterval}

A variável **[!UICONTROL Granularity and time interval]** o seletor permite especificar as datas e a duração agregadas semanal/mensalmente para observar o comportamento de compartilhamento da conta do assinante. A seleção padrão do intervalo de tempo é a semana atual, mas você pode modificar a duração usando as opções mostradas na imagem.

![[!UICONTROL Granularity and timeinterval]](assets/granularity-timeinterval-weekwise.png){width="350" align="left"}

*Figura: Caixa de diálogo Granularidade e intervalo de tempo*

**A** Escolha uma data no seletor de datas<br/>
**B** Selecione a seta para a esquerda para voltar<br/>
**C** Selecione a seta para a direita para avançar<br/>
**D** Selecionar a granularidade por semana/mês<br/>
**E** Intervalo de tempo selecionado<br/>

Aplicando esses controles, você pode definir sua declaração de problema como &quot;assinantes do MVPD A que assistiram aos canais X, Y e Z no mês de outubro&quot;.

