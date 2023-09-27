---
title: Limitações e problemas conhecidos
description: Problemas conhecidos no produto.
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 2ced89dc1f77d2c090b599c40e778f3054f1a8dd
workflow-type: tm+mt
source-wordcount: '415'
ht-degree: 0%

---

# Problemas conhecidos e limitações {#known-issues}

O Adobe se esforça para oferecer uma funcionalidade robusta e experiências do usuário perfeitas por meio de suas ofertas. A versão atual (versão 1.0) do Account IQ fornece análise de compartilhamento de uso e assinatura para provedores de streaming com alto grau de confiança. No entanto, as limitações a seguir serão abordadas em versões futuras.

* Atualmente, ao definir coortes nas páginas de painel ou relatórios, não há opção de adicionar métricas como **número de dispositivos** para refinar o segmento. Esse recurso estará disponível em uma versão futura.

* Ao estimar pontuações de compartilhamento para contas individuais, o Account IQ adota uma abordagem conservadora que permite que as empresas atuem no compartilhamento com grande grau de confiança. No entanto, essa abordagem tende a subestimar a quantidade total de compartilhamento quando agregada em muitas contas.

* A variável [Pontuação geral de compartilhamento](/help/accountiq/dashboard.md#overall-sharing-score) atualmente, apenas fatores em [Nível de compartilhamento](/help/accountiq/dashboard.md#sharing-level) e [Uso de contas compartilhadas](/help/accountiq/dashboard.md#usage-from-shared-accounts). As versões futuras levarão em conta as métricas adicionais.

* Ao definir coortes nas páginas de painel ou relatórios, os seletores para MVPDs e canais não têm o mecanismo de pesquisa, por enquanto.

* Ao definir coortes nas páginas de painel ou relatórios, há uma limitação para selecionar até 10 MVPDs e Programadores (ou canais individuais).

* A opção de exportar estatísticas de contas está limitada a exportar 1000 contas, a partir de agora.

* A opção para selecionar [Tipo de segmento](#segment-type) quando a definição de Operações é limitada a **Número fixo de contas**. A variável **Número variável de contas** estará disponível em uma versão futura.

* As seções Avaliação de desempenho, Modelos de detecção, Segmentos, Instantâneos e Regras na navegação à esquerda estão desativadas no momento e estarão disponíveis em uma versão futura.

* Ao criar [Operações](/help/accountiq/operation-affecting-user-segment.md), você pode identificar apenas dois tipos de [Ações](/help/accountiq/operation-affecting-user-segment.md) a partir de agora — Regras de monitoramento de simultaneidade e Ações externas.

* Atualmente, as Operações só podem ser criadas e [programado](/help/accountiq/operation-affecting-user-segment.md#action). As versões futuras permitirão pausá-las, retomá-las e gerenciá-las totalmente.

* Devido ao conjunto de dados mais limitado usado, o modo de Isolamento não reflete realmente a quantidade de compartilhamento. Portanto, o MVPD no modo Isolation não pode ser comparado a qualquer outro MVPD. <!--do we need to separate out this limitation, which is from a different persona i.e. only for Programmer persona?-->

* Quando estiver definindo um novo [segmento](/help/accountiq/segments-timeframe.md) para uma operação, é possível adicionar métricas. Porém, se você selecionar um segmento salvo, não poderá adicionar mais métricas para refinar o segmento.

* A granularidade e o seletor de período são limitados a uma semana ou um mês, o que significa que os dados podem ser avaliados somente em uma semana ou um mês.

* Os intervalos predefinidos estão atualmente desabilitados na granularidade e no seletor de período e estarão disponíveis em uma versão futura.
