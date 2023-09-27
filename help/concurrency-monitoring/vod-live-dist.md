---
title: Como distinguir entre VOD e conteúdo ao vivo no monitoramento de simultaneidade
description: Como distinguir entre VOD e conteúdo ao vivo no monitoramento de simultaneidade
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 0%

---


# Como: distinguir entre VOD e conteúdo ao vivo no monitoramento de simultaneidade {#dist-vod-live}

**P:** O serviço de Monitoramento de simultaneidade consegue distinguir entre o tipo de conteúdo que está sendo reproduzido (conteúdo ativo vs. vídeo sob demanda)?



**R:** O monitoramento de simultaneidade não consegue distinguir diretamente entre conteúdo dinâmico e VOD (vídeo sob demanda). O reprodutor de vídeo deve saber o tipo de conteúdo que está reproduzindo e enviar essas informações durante o [chamada de inicialização de sessão](/help/concurrency-monitoring/cm-api-overview.md#session-initial) (obrigatório para Monitoramento de simultaneidade). O fluxo de trabalho normal tem esta aparência:

1. Os clientes do Monitoramento de simultaneidade definem um conjunto de metadados no qual desejam que as regras sejam implementadas (por exemplo, content-type=live|vod, device-type=mobile|console|desktop).
1. A equipe de monitoramento de simultaneidade implementa a política desejada. Exemplo:
   1. se content-type=live, max streams=3, wins mais recente
   1. se content-type=vod, max streams=1, wins mais recente

1. Quando o usuário final reproduz o conteúdo, o reprodutor de vídeo deve enviar valores para os campos de metadados que foram estabelecidos como parte de uma política.

1. O serviço de monitoramento de simultaneidade, com base na política definida e nos valores recebidos, emitirá uma decisão (reproduzir/parar).

1. A decisão deve ser obedecida pelo reprodutor de vídeo para que o sistema funcione.



## Informações relacionadas {#related-info-vod-live-dist}

* [Atributos de metadados padrão de monitoramento de simultaneidade](/help/concurrency-monitoring/standard-metadata-attributes.md)
* [Visão geral da API de monitoramento de concorrência](/help/concurrency-monitoring/cm-api-overview.md)
