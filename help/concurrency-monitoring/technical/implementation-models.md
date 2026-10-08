---
title: Modelos de implementação
description: Modelos de implementação
exl-id: 3bcb63ba-9b4a-4df4-8d24-e520b8830a10
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '63'
ht-degree: 0%
---
# Modelos de implementação {#imp-models}

## Políticas do lado do servidor {#ss-policies}

Este modelo utilizará o CM como um ponto de decisão política, delegando assim a decisão de acesso ao serviço.

Como o cliente não deve fazer suposições em relação às políticas aplicadas, a implementação precisa verificar a decisão na inicialização da sessão, bem como regularmente, durante a reprodução da resposta de heartbeat.
