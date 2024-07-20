---
title: Segmentos e intervalo de tempo
description: Defina coortes ou selecione segmentos de assinante para medir as possibilidades de compartilhamento de conta e os padrões dos visualizadores do canal para usar ferramentas gráficas e relatórios no Account IQ.
exl-id: c38cde37-70d9-486d-b8d0-7c1cbd2baf2e
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# Segmentos e intervalo de tempo {#segment-timeinterval}

Ao fazer logon no Account IQ, o painel de segmento e intervalo de tempo, localizado acima do painel, permite definir o [segmento](product-concepts.md#segmet-def) do assinante. Esse painel ajuda a filtrar resultados e exibir relatórios sobre o comportamento e os padrões de compartilhamento do assinante. Um segmento chamado **TODAS AS CONTAS EM SUAS PROPRIEDADES** está selecionado por padrão no momento, onde você pode exibir as seguintes opções:

![](assets/new-segment-selector-collapsed.png){align="left"}

*Painel de segmento e intervalo de tempo com resumo de segmento recolhido*

**A.** Nome do segmento atualmente selecionado **B.** Abrir lista de segmentos **C.** Editar segmento **D.** Criar novo segmento **E.** Granularidade e seletor de intervalo de tempo **F.** Ícone para expandir o resumo do segmento **G.** Resumo do segmento recolhido **H.** Número de contas no segmento para o intervalo selecionado

>[!NOTE]
>
> O resumo de segmento recolhido mostra as [Categorias de vídeo](product-concepts.md#video-category-def) usadas na versão para TV em todos os lugares do Account IQ. Se você estiver conectado como um serviço D2C, esses rótulos exibirão as categorias de vídeo específicas da sua empresa.

Leia mais sobre [como criar](work-with-segments.md#create-new-segment) e [gerenciar segmentos](work-with-segments.md#manage-segment) na guia **Segmentos** do painel esquerdo.

## Seleção de segmento {#segment-selection}

Para selecionar um segmento específico, siga estas etapas:

1. Navegue até a opção **[!UICONTROL Open segment]** dentro do segmento e do painel de intervalo de tempo.
1. Selecione o **Nome do segmento** para o qual deseja exibir os relatórios de compartilhamento de conta.

   ![](assets/open-segment.png){align="left"}

   *Selecionar nome do segmento*

   >[!NOTE]
   >
   > As categorias de vídeo mostradas na imagem anterior, como **MVPDs**, **Programadores** e **Canais**, representam os rótulos usados na versão para TV em todos os locais do Account IQ. Se você estiver conectado como um serviço D2C, esses rótulos exibirão as categorias de vídeo específicas da sua empresa.

1. Selecione **[!UICONTROL Open segment]**.


## Seleção de granularidade e intervalo de tempo {#granularity-timeinterval}

O seletor **Granularidade e Intervalo de Tempo** permite especificar as datas e a duração agregadas semanal/mensalmente para observar o comportamento de compartilhamento do assinante. A seleção padrão é a semana atual.

![Granularidade e intervalo de tempo](assets/granularity-timeinterval-weekwise.png){align="left"}

*Caixa de diálogo Granularidade e intervalo*

**A.** Granularidade e seletor de intervalo de tempo **B.** Seta para a direita para ir para o próximo mês/semana **C.** Opção para escolher granularidade por semana/mês **D.** Intervalo de tempo selecionado no momento **E.** Seta para a esquerda para ir para o mês/semana anterior

Você pode modificar a duração usando as seguintes etapas:

1. Selecione o **[!UICONTROL Granularity and Time Interval]** no seletor de datas.

1. Selecione **[!UICONTROL Week]** ou **[!UICONTROL Month]** em **[!UICONTROL Aggregate By]** para definir a granularidade da sua avaliação.

1. Depois de selecionar a granularidade, você pode usar as setas para frente ou para trás para navegar pelo intervalo de tempo.

1. Selecione um período específico para a avaliação.

1. Selecione **[!UICONTROL Apply]** para garantir que sua seleção seja efetivada.

Isso permite definir sua declaração de problema como &quot;Assinantes do MVPD A que assistiram aos canais X, Y e Z durante a semana escolhida de dezembro&quot;.

## Resumo do segmento {#segment-summary}

O Resumo de segmentos é semelhante para serviços D2C e TV Everywhere. As categorias de vídeo serão diferentes para cada versão do Account IQ.

Selecionar Ícone <img alt= "expandir resumo de segmentos" src="./assets/expand-segment-summary.svg" width="25"> para exibir o resumo detalhado do segmento. Ele também apresenta informações sobre o número de contas do assinante e suas solicitações de reprodução no período escolhido.

+++ Serviços D2C

![](assets/segment-panel-d2c.png){align="left"}

*Resumo de segmentos para serviços D2C*

>[!NOTE]
>
>As [categorias de vídeo](product-concepts.md#video-category-def) mostradas na imagem anterior, como **região** e **tipos de conteúdo** no segmento, são apenas exemplos. Ao fazer logon no Account IQ, esses rótulos exibem as categorias de vídeo específicas da sua empresa.

O **Resumo de Segmentos** inclui as seguintes condições que definem um segmento:

**[Regiões e tipos de conteúdo](product-concepts.md#video-category-def) no segmento** referem-se aos rótulos de metadados associados aos fluxos de vídeo assistidos por contas compartilhadas representadas nos relatórios de compartilhamento de conta.

**[As métricas](product-concepts.md#metric) no segmento** referem-se a atributos ou critérios que os assinantes devem ter atendido para serem identificados nos relatórios de compartilhamento de conta.

+++

+++ TV em todos os lugares

![](assets/segment-panel-programmers-mvpd.png){align="left"}

*Resumo de segmentos para programadores/MVPDs*

O **Resumo de Segmentos** inclui as seguintes condições que definem um segmento:

**[Programadores](product-concepts.md#programmer-def) no segmento** referem-se a provedores de conteúdo cujos fluxos de vídeo foram assistidos por contas compartilhadas representadas nos relatórios de compartilhamento de conta.

**[Canais](product-concepts.md#channel-def) no segmento** referem-se a canais cujos fluxos de vídeo foram assistidos por contas compartilhadas representadas nos relatórios de compartilhamento de conta.

**[MVPDs](product-concepts.md#mvpd-def) no segmento** referem-se a distribuidores de programação de vários vídeos aos quais os assinantes estão associados para serem identificados em relatórios de compartilhamento de conta.

**[As métricas](product-concepts.md#metric) no segmento** referem-se a atributos ou critérios que os assinantes devem ter atendido para serem identificados nos relatórios de compartilhamento de conta.

+++
