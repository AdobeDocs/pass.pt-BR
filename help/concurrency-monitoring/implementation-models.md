---
title: Modelos de implementação
description: Modelos de implementação
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '62'
ht-degree: 0%

---


# Modelos de implementação {#imp-models}

## Políticas do lado do servidor {#ss-policies}

Este modelo utilizará o CM como um ponto de decisão política, delegando assim a decisão de acesso ao serviço.

Como o cliente não deve fazer suposições em relação às políticas aplicadas, a implementação precisa verificar a decisão na inicialização da sessão, bem como regularmente, durante a reprodução da resposta de heartbeat.
