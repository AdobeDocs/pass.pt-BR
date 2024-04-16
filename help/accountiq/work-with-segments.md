---
title: Trabalhar com segmentos
description: Entender e usar segmentos. Saiba como criar e gerenciar um segmento.
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '1970'
ht-degree: 0%

---

# Trabalhar com segmentos {#work-with-segments}

[Segmentos](product-concepts.md#segmet-def) são uma coleção de contas de assinantes que permitem analisar o compartilhamento de credenciais em condições definidas pelo usuário. Você pode usar segmentos para examinar diferentes conjuntos de contas de assinantes e gerar relatórios de dados correspondentes em tabelas e gráficos. Há dois tipos de segmentos no Account IQ:

1. **Segmento padrão**: **Todas as contas em suas propriedades** é um segmento pronto para uso no sistema que inclui todas as contas de assinantes ativas sem condições específicas aplicadas.

   >[!NOTE]
   >
   >O uso do segmento padrão pode impedir a exibição de determinadas tabelas, como [Categorias de vídeo no segmento](data-panels.md#video-categories-segment), [Pontuação de compartilhamento por canais e MVPDs](data-panels.md#sharin-score-by-channels-and-mvpds), e [Distribuição do padrão de uso para categorias de vídeo](usage-patterns.md#usage-pattern-dis-video-categories). Essas tabelas só podem acomodar e exibir dados de até 20 linhas por vez. As tabelas, os gráficos e os relatórios restantes são os mesmos para segmentos padrão e personalizados.

1. **Segmentos personalizados**: são segmentos personalizados que permitem agrupar contas de assinantes de categorias específicas, como tipos de conteúdo D2C, programadores, canais e MVPDs, para analisar o compartilhamento de credenciais em condições definidas pelo usuário. Saiba como [criar um segmento personalizado](#create-new-segment).

   >[!IMPORTANT]
   >
   >Todos os procedimentos descritos neste guia são baseados em segmentos personalizados. No entanto, os conceitos permanecem os mesmos para segmentos padrão e personalizados.

Quando você for ao **Ações** e selecione o **[!UICONTROL Segments]** no painel esquerdo, uma lista de segmentos disponíveis no sistema é exibida. A página de segmentos permite avaliar rapidamente os principais detalhes sobre cada segmento em formato tabular. Os detalhes incluem o nome do segmento, o número de [categorias de vídeo](product-concepts.md#video-category-def), métricas, [operações](product-concepts.md#operation-def) usando o segmento atual, a data e hora da última modificação, bem como o nome do criador do segmento.

Você pode executar as seguintes funções com segmentos:

* [Criar um novo segmento](#create-new-segment)
* [Gerenciar segmentos](#manage-segments)


## Criar um novo segmento {#create-new-segment}

O processo de criação de um novo segmento é semelhante para serviços D2C e TV Everywhere. As categorias de vídeo serão diferentes para cada versão do Account IQ.

+++Serviços D2C

Para criar um segmento e analisar o comportamento de compartilhamento do assinante, selecione **[!UICONTROL Create new segment]** no canto superior direito.

![Selecione Criar novo segmento](assets/d2c-create-new-segment.png)

*Selecione Criar novo segmento*

>[!NOTE]
>
>As categorias de vídeo mostradas na imagem anterior, como **Regiões** e **Tipos de conteúdo** são apenas exemplos. Ao fazer logon no Account IQ, esses rótulos exibem as categorias de vídeo específicas da sua empresa.

Ele abre um **Novo segmento** , que inclui os seguintes elementos:

![Nova página de segmento](assets/d2c-new-segment-dialog.png)

*Nova página de segmento*

**A.** Componentes do segmento **B.** Definição de segmento **C** Resumo do segmento

* **Componentes do segmento**: um inventário de [categorias de vídeo](product-concepts.md##video-category-def) e métricas calculadas usadas para definir um segmento.

  >[!NOTE]
  >
  >Uso **[!UICONTROL Show all]** para expandir a lista de componentes do segmento. Para localizar um componente rapidamente, pesquise seu nome em **componentes do segmento de pesquisa** em vez de percorrer toda a lista.

* **Definição de segmento**: uma tela onde você pode arrastar e soltar vários componentes de segmento para criar um segmento.

* **Resumo do segmento**: um resumo que estima as contas qualificadas com base nos componentes na definição do segmento e fornece uma breve visão geral dele durante o período de avaliação.

Execute as seguintes etapas para criar um segmento:

1. Digite o nome do segmento em **Nome do segmento** que estarão visíveis na lista de segmentos e durante a seleção de segmentos.
1. Digite uma descrição detalhada do seu segmento em **Descrição do segmento**.
1. Por exemplo, arrastar **Regiões e tipos de conteúdo** componentes de segmento no painel esquerdo e solte-os na **Regiões/Tipos de conteúdo** seção no **Definição de segmento**.

   >[!NOTE]
   >
   >Você pode criar um segmento com base em regiões ou tipos de conteúdo. Visualize os tipos de conteúdo associados de uma região em um menu suspenso.

   Se você começar adicionando um **tipo de conteúdo** no **Regiões/Tipos de conteúdo** , só é possível adicionar tipos de conteúdo como componentes subsequentes.

   Se você começar adicionando um **Região** no **Regiões/Tipos de conteúdo** é exibida uma caixa de diálogo de decisão.

   ![Adicionar componente de segmento como uma região ou seus tipos de conteúdo ](assets/d2c-segment-basis-selector.png){width="550" align="left"}

   *Adicionar componente de segmento como uma região ou sua caixa de diálogo de tipos de conteúdo*

   Decida se você deseja comparar regiões específicas ou um segmento com base nos tipos de conteúdo associados a uma região.

   Selecionar **[!UICONTROL As a region]** para adicionar regiões à **Regiões/Tipos de conteúdo** seção.

   Selecionar **[!UICONTROL As its content types]** para adicionar tipos de conteúdo de uma região.

1. Arrastar **Métricas** componentes de segmento no painel esquerdo e solte-os na **Métricas** seção no **Definição de segmento**.

   ![Selecionar um operador e definir um valor para a métrica adicionada](assets/component-metrics.png)

   *Selecionar um operador e atribuir um valor para a métrica adicionada*

   Depois de adicionar métricas na definição do segmento, escolha um operador em **[!UICONTROL Select an operator]** menu suspenso e atribua um valor usando **[!UICONTROL Select an option]**.

   Ajuste valores para determinadas métricas usando a seta para cima para aumentar e a seta para baixo para diminuir.

1. Arrastar **Métricas calculadas** componentes de segmento no painel esquerdo e solte-os na **Métricas calculadas** seção no **Definição de segmento**.

   ![Selecione um operador e defina um valor para a métrica calculada adicionada](assets/component-calculated-metrics.png)

   *Selecione um operador e atribua um valor para a métrica calculada adicionada*

   Depois de adicionar métricas calculadas na definição do segmento, **[!UICONTROL Select an operator]** no menu suspenso e atribua um valor usando **[!UICONTROL Select an option]**.

   >[!NOTE]
   >
   >Todas as métricas e métricas calculadas que você solta na definição do segmento são acompanhadas por operadores apropriados para atribuir valores às respectivas métricas e métricas calculadas.

1. Revise os detalhes do segmento no **Resumo do segmento** para decidir as alterações que deseja implementar no segmento.
1. Selecionar **[!UICONTROL Last week]** ou **[!UICONTROL Last month]** do **Período de avaliação** menu suspenso para estimar os valores do resumo da semana ou do mês passado.
1. Selecionar **[!UICONTROL Update estimation]** para calcular o número de contas qualificadas estimadas no segmento atual com base no período de avaliação selecionado.
1. Selecionar **[!UICONTROL Save segment]**.

O segmento que você criou agora está disponível na lista de segmentos.

+++

+++TV em todos os lugares

Para criar um segmento e analisar o comportamento de compartilhamento do assinante, selecione **[!UICONTROL Create new segment]** no canto superior direito.

![Selecione Criar novo segmento](assets/create-new-segment.png)

*Selecione Criar novo segmento*

Ele abre um **Novo segmento** , que inclui os seguintes elementos:

![Nova página de segmento](assets/new-segment-dialog.png)

*Nova página de segmento*

**A.** Componentes do segmento **B.** Definição de segmento **C** Resumo do segmento

* **Componentes do segmento**: um inventário de programadores e canais, MVPDs, métricas e métricas calculadas usadas para definir um segmento.

  >[!NOTE]
  >
  >Uso **[!UICONTROL Show all]** para expandir a lista de componentes do segmento. Para localizar um componente rapidamente, pesquise seu nome em **componentes do segmento de pesquisa** em vez de percorrer toda a lista.

* **Definição de segmento**: uma tela onde você pode arrastar e soltar vários componentes de segmento para criar um segmento.

* **Resumo do segmento**: um resumo que estima as contas qualificadas com base nos componentes na definição do segmento e fornece uma breve visão geral dele durante o período de avaliação.

Execute as seguintes etapas para criar um segmento:

1. Digite o nome do segmento em **Nome do segmento** que estarão visíveis na lista de segmentos e durante a seleção de segmentos.
1. Digite uma descrição detalhada do seu segmento em **Descrição do segmento**.
1. Arrastar **Programadores e canais** componentes de segmento no painel esquerdo e solte-os na **Programadores/Canais** seção no **Definição de segmento**.

   >[!NOTE]
   >
   >Você pode criar um segmento com base em programadores ou canais. Visualize os canais associados a um programador em um menu suspenso.

   Se você começar adicionando um **Canal** no **Programadores/Canais** só é possível adicionar canais como componentes subsequentes.

   Se você começar adicionando um **Programador** no **Programadores/Canais** é exibida uma caixa de diálogo de decisão.

   ![Adicionar componente de segmento como programador ou seus canais ](assets/segment-basis-selector.png){width="550" align="left"}


   *Adicionar componente de segmento como programador ou caixa de diálogo de canais*

   Decida se deseja comparar programadores específicos ou um segmento com base nos canais associados a um programador.

   Selecionar **[!UICONTROL As a programmer]** para adicionar programadores à **Programadores/Canais** seção.

   Selecionar **[!UICONTROL As its channels]** para adicionar todos os canais de um programador.

1. Arrastar **MVPDs** componentes de segmento no painel esquerdo e solte-os na **MVPDs** seção no **Definição de segmento**.

   >[!NOTE]
   >
   >Ao fazer logon como programador, um MVPD chamado **xfinity** é exibida como uma opção independente no **MVPDs** seção. Não é possível combiná-lo com qualquer outro MVPD.

1. Arrastar **Métricas** componentes de segmento no painel esquerdo e solte-os na **Métricas** seção no **Definição de segmento**.

   ![Selecionar um operador e definir um valor para a métrica adicionada](assets/component-metrics.png)

   *Selecionar um operador e atribuir um valor para a métrica adicionada*

   Depois de adicionar métricas na definição do segmento, escolha um operador em **[!UICONTROL Select an operator]** menu suspenso e atribua um valor usando **[!UICONTROL Select an option]**.

   Ajuste valores para determinadas métricas usando a seta para cima para aumentar e a seta para baixo para diminuir.

1. Arrastar **Métricas calculadas** componentes de segmento no painel esquerdo e solte-os na **Métricas calculadas** seção no **Definição de segmento**.

   ![Selecione um operador e defina um valor para a métrica calculada adicionada](assets/component-calculated-metrics.png)

   *Selecione um operador e atribua um valor para a métrica calculada adicionada*

   Depois de adicionar métricas calculadas na definição do segmento, **[!UICONTROL Select an operator]** no menu suspenso e atribua um valor usando **[!UICONTROL Select an option]**.

   >[!NOTE]
   >
   >Todas as métricas e métricas calculadas que você solta na definição do segmento são acompanhadas por operadores apropriados para atribuir valores às respectivas métricas e métricas calculadas.

1. Revise os detalhes do segmento no **Resumo do segmento** para decidir as alterações que deseja implementar no segmento.
1. Selecionar **[!UICONTROL Last week]** ou **[!UICONTROL Last month]** do **Período de avaliação** menu suspenso para estimar os valores do resumo da semana ou do mês passado.
1. Selecionar **[!UICONTROL Update estimation]** para calcular o número de contas qualificadas estimadas no segmento atual com base no período de avaliação selecionado.
1. Selecionar **[!UICONTROL Save segment]**.

O segmento que você criou agora está disponível na lista de segmentos.
+++

## Gerenciar segmentos {#manage-segments}

Você pode selecionar um segmento na lista de segmentos e executar as seguintes ações:

* [Editar um segmento](#edit-segment)
* [Duplicar um segmento](#duplicate-segment)
* [Excluir um segmento](#delete-segment)

![Editar, duplicar ou excluir um segmento](assets/manage-segments-list.png)

*Selecione um segmento para editar, duplicar ou excluir*

**A.** [Categorias de vídeo](product-concepts.md#video-category-def) **B.** [Segmento padrão](#work-with-segments)

>[!NOTE]
>
>As categorias de vídeo mostradas nesta seção, como **MVPDs**, **Programadores**, e **Canais** representam os rótulos usados na versão para TV em todos os locais do Account IQ. Se você estiver conectado como um serviço D2C, esses rótulos exibirão as categorias de vídeo específicas da sua empresa.

Não é possível editar, duplicar ou excluir o segmento padrão chamado **Todas as contas em suas propriedades**.

### Editar um segmento {#edit-segment}

1. Navegue até a **[!UICONTROL Segments]** em **Ações** no painel esquerdo para exibir uma lista de segmentos.
1. Selecione um segmento que deseja editar.
1. Selecionar **[!UICONTROL Edit]**.
1. Modifique detalhes do segmento, como nome do segmento, descrição ou componentes na **Definição de segmento**.

   >[!TIP]
   >
   >Uso **[!UICONTROL Clear all]** para remover todos os componentes de segmento em cada seção na definição de segmentos de uma só vez. Como alternativa, selecione o botão cruzar para remover itens individuais.

   ![Limpar todos os componentes de segmento em cada seção na definição do segmento ](assets/clear-all-components.png)

   *Selecione Limpar tudo para remover todos os componentes do segmento de uma só vez*

1. Selecione **[!UICONTROL Update segment]** para atualizar o segmento existente ou **[!UICONTROL Save as new segment]** para criar um novo segmento com as alterações.

   >[!NOTE]
   >
   >Não é permitido atualizar segmentos que estão atualmente em operações. Salvar alterações como um novo segmento é a única opção para segmentos com operações em andamento.

### Duplicar um segmento {#duplicate-segment}

1. Navegue até a **[!UICONTROL Segments]** em **Ações** no painel esquerdo para exibir uma lista de segmentos.
1. Selecione um segmento que deseja duplicar.
1. Selecionar **[!UICONTROL Duplicate]**.

Uma cópia do segmento selecionado é gerada e colocada no final da lista de segmentos. Você pode editar os detalhes necessários no segmento duplicado e, em seguida, atualizar o segmento duplicado ou salvá-lo como um novo segmento.

### Excluir um segmento {#delete-segment}

1. Navegue até a **[!UICONTROL Segments]** em **Ações** no painel esquerdo para exibir uma lista de segmentos.
1. Selecione um segmento que deseja remover.

   Selecione vários segmentos para excluí-los em uma única operação. Você também pode marcar uma caixa de seleção à esquerda da caixa de seleção **Nome do segmento** para excluir todos os segmentos de uma só vez.

   >[!NOTE]
   >
   > Você só poderá excluir mais de um segmento ou todos os segmentos se nenhum deles for usado por operações. Além disso, a exclusão do segmento padrão chamado **Todas as contas em suas propriedades** não é permitido. Ela permanecerá desmarcada quando você tentar excluir todos os segmentos de uma só vez.

   ![Excluir mais de um segmento](assets/delete-more-than-one-segment.png)

   *Selecionar vários segmentos para excluir mais de um segmento*

1. Selecionar **[!UICONTROL Delete]**.
1. Confirmar para **[!UICONTROL Delete]** na caixa de diálogo para remover o segmento permanentemente.

   >[!NOTE]
   >
   >O segmento é excluído permanentemente do sistema e você não pode desfazer essa ação.


