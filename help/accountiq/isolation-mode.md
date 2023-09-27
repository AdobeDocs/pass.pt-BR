---
title: Exibir relatórios no modo de isolamento
description: Visualizar relatórios no modo de isolamento do Xfinity.
exl-id: e7cf24c5-9bfa-48f6-b5c8-20443a976891
source-git-commit: d543bbe972944ad83f4cb28c8a17ea6e10f66975
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 0%

---

# Exibir relatórios de compartilhamento no modo de isolamento {#report-isolation-mode}

No Modo de isolamento, os MVPDs (como Xfinity) identificam os assinantes de forma consistente em todos os dispositivos, mas identificam os assinantes de forma diferente, com base nos programadores com os quais eles interagem. Enquanto no modo padrão, os MVPDs identificam consistentemente os assinantes em todos os dispositivos, independentemente dos programadores.

Por exemplo, na imagem a seguir, se um Assinante B de um MVPD de Modo de Isolamento (como Xfinity) acessar o conteúdo oferecido por dois programadores diferentes usando o mesmo dispositivo, o MVPD associará identificadores diferentes às duas tentativas de acesso diferentes. Assim, para esses programadores (L e M na figura) e para o Account IQ, parece que há dois assinantes diferentes acessando o conteúdo. No entanto, para o MVPD Padrão, se o Assinante B acessar o conteúdo oferecido por dois programadores diferentes, o MVPD associará um único identificador de acesso para ambas as tentativas de acesso. Os MVPDs (como Xfinity) no Modo de isolamento não identificam um assinante de maneira consistente, mesmo que o assinante esteja usando o mesmo dispositivo em programadores diferentes.

![](assets/isolation-diff-new.png)

*Figura: O MVPD do modo de isolamento identifica quatro assinantes diferentes em vez de dois*

Para gerenciar a distorção de dados (devido à identificação do mesmo assinante como diferente com base no acesso de diferentes programadores), o Modo de isolamento limita a atividade relatada sobre um programador à atividade somente nos aplicativos desse programador. Por exemplo, para o Modo de isolamento na imagem acima, o Programador L vê os dados com base apenas na atividade das Identidades W e Y, ignorando as Identidades X e Z.

>[!IMPORTANT]
>
> A desvantagem é que o Programador L é privado de compartilhar informações coletadas sobre os Assinantes A e B devido à atividade com qualquer Programador diferente de L.

No Modo de Isolamento, todos os cálculos feitos para obter as Pontuações de Compartilhamento e todas as métricas associadas são feitos usando apenas a atividade dos dispositivos que transmitem a partir de aplicativos pertencentes ao Programador e aos canais selecionados.
As pontuações e probabilidades de compartilhamento são calculadas somente usando o fluxo que começa nos canais selecionados atualmente.

Para exibir métricas no modo de isolamento:

1. Selecionar **[!UICONTROL isolation mode]** do **[!UICONTROL MVPDs in segment]** e selecione **[!UICONTROL Apply Selection]**.

   ![](assets/xfinity-in-segment.gif)

   *Figura: Seleção de MVPD no modo de Isolamento*

1. Selecione os canais desejados na **[!UICONTROL Channels in segment]** e selecione **[!UICONTROL Apply Selection]**.

   Além disso, selecione um [intervalo de tempo](/help/accountiq/product-concepts.md#granularity-def).

   >[!IMPORTANT]
   >
   >Como o compartilhamento de conta é mais relevante quando medido para transmissão em todos os aplicativos dos programadores, você verá pontuações de compartilhamento mais baixas e alguma variação nas métricas quando estiver no Modo de isolamento.

   ![](assets/aggregate-sharing-isolation.png)

   *Figura: Medidores de probabilidade de compartilhamento no modo de Isolamento*

   Observe que os medidores acima mostram que apenas 6% de todas as contas estão sendo compartilhadas e apenas 8% do conteúdo está sendo consumido por esses 8%. Portanto, os canais podem comparar suas pontuações no Modo de isolamento com as de outros MVPDs. Portanto, as informações obtidas usando o Modo de isolamento devem ser interpretadas de forma diferente dos outros dados.
