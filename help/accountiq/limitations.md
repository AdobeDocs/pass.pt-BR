---
title: Limitação
description: Saiba mais sobre limitações e MVPD de modo de isolamento para programadores no Account IQ.
exl-id: 08d65716-8b6a-4300-acda-fec63e1e6815
source-git-commit: 791d661e1495bdb6fe4eb25efbefeecd813f0f3a
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 0%

---

# Limitação {#limitations}

As versões D2C e TV Everywhere do Account IQ fornecem análise de compartilhamento de uso e assinatura para provedores de transmissão. No entanto, a versão atual 1.3 tem determinadas limitações, que serão abordadas em versões futuras.

* A variável [Pontuação geral de compartilhamento](/help/accountiq/data-panels.md#overall-sharing-score) inclui atualmente [Nível de compartilhamento](/help/accountiq/data-panels.md#sharing-level) e [Uso de contas compartilhadas](/help/accountiq/data-panels.md#usage-from-shared-accounts). As próximas versões incluirão mais métricas.

* Ao definir segmentos no painel ou padrões de uso, a variável [Categorias de vídeo no segmento](/help/accountiq/data-panels.md#video-categories-segment), [Pontuação de compartilhamento por canais e MVPDs](/help/accountiq/data-panels.md#sharin-score-by-channels-and-mvpds), e [Distribuição do padrão de uso para categorias de vídeo](/help/accountiq/usage-patterns.md#usage-pattern-dis-video-categories) os relatórios só podem exibir dados de até 20 [categorias de vídeo](product-concepts.md#video-category-def). Segmentos que contêm mais de 20 categorias de vídeo não mostrarão dados nesses relatórios.

* Atualmente, a opção de exportar estatísticas de conta está restrita à exportação de 1000 contas.

* Ao definir as Operações, a opção para selecionar [tipo de segmento](/help/accountiq/operations.md#segment) está limitado a **Número fixo de contas**. A variável **Número variável de contas** estará disponível em versões futuras.

* A variável **Benchmarking**, **Modelos de detecção**, **Ações**, e **Configurações** as seções na navegação à esquerda estão desativadas no momento e estarão disponíveis em uma versão futura.

* Ao criar operações, você pode aplicar apenas dois tipos de [ações](/help/accountiq/operations.md#action) — Regras de monitoramento de simultaneidade e ações externas.

* Atualmente, você só pode [criar](/help/accountiq/operations.md#create-new-operation) e [programação](/help/accountiq/operations.md#schedule) Operações. As próximas versões permitirão pausar, retomar e gerenciá-las completamente.

* Você só pode analisar dados de uma única semana ou mês por vez ao selecionar Granularidade e Intervalo de tempo.

* Não é possível adicionar MVPDs do Modo de Isolamento à definição de segmento com outros MVPDs. Alguns MVPDs não identificam exclusivamente assinantes em vários canais de programador. Portanto, esses MVPDs são tratados separadamente para programadores da TV Everywhere.



