---
title: Modelos de implementação
description: Modelos de implementação
exl-id: 3bcb63ba-9b4a-4df4-8d24-e520b8830a10
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '62'
ht-degree: 0%

---

# Modelos de implementação {#imp-models}

## Políticas do lado do servidor {#ss-policies}

Este modelo utilizará o CM como um ponto de decisão política, delegando assim a decisão de acesso ao serviço.

Como o cliente não deve fazer suposições em relação às políticas aplicadas, a implementação precisa verificar a decisão na inicialização da sessão, bem como regularmente, durante a reprodução da resposta de heartbeat.
