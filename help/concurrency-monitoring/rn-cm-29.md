---
title: Notas de versão do Adobe Concurrency Monitoring 2.9
description: Notas de versão do Adobe Concurrency Monitoring 2.9
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---


# Notas de versão do Monitoramento de simultaneidade 2.9 {#rn-cm29}

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão.

## Data de lançamento {#release-date}

03/14/2019


## Visão geral da versão {#release-overview}

* A partir desta versão, introduzimos um novo relatório para entender o uso simultâneo: um histograma para o nível de simultaneidade, mostrando:

* o número de usuários que atingiram cada nível de simultaneidade (ou seja, quantos usuários já tiveram dois fluxos simultâneos, três fluxos simultâneos e assim por diante) durante cada intervalo de granularidade
* a duração total de cada nível de simultaneidade, em minutos (o valor médio pode ser calculado simplesmente dividindo esse valor pela contagem acima)
* o número total de vezes que os usuários encontraram cada nível de simultaneidade, para estimar o impacto de uma determinada regra em termos de usuários afetados e experiência de usuário agregada. Mais detalhes estão no [Relatórios de uso](/help/concurrency-monitoring/cm-usage-reports.md) página.

Também melhoramos a proteção de injeção de SQL e adicionamos várias correções de erros.

## Problemas conhecidos {#known-issues}

Nenhum.
