---
title: Ponto de decisão da política
description: Ponto de decisão da política
exl-id: 94bc638c-bef8-45ea-b20a-9b7038adecdd
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '731'
ht-degree: 0%

---

# Ponto de decisão da política {#policy-desc-pt}

## Modelo de domínio {#domain-model}

Esta página serve como referência para diferentes casos de uso e implementações de políticas. Recomendamos que você também consulte a parte do [Glossário](/help/concurrency-monitoring/cm-glossary.md) da documentação para obter as definições de termos.

Um **locatário** possui **aplicativos** para os quais deseja impor **políticas**. Os **aplicativos cliente** devem ser configurados com a **ID do aplicativo** (fornecida pela Adobe).

Em seguida, o locatário associa cada aplicativo a uma ou mais políticas, criadas por ele ou criadas e compartilhadas por outros. As políticas podem ser vinculadas entre vários locatários.

A **atividade de assunto** consiste em todos os fluxos (independentemente do aplicativo) relatados ao Monitoramento de Simultaneidade para um determinado assunto.

Quando um fluxo deve ser autorizado para um determinado assunto, o sistema primeiro verificará todas as políticas definidas para o aplicativo que criou o fluxo.

Para cada uma das políticas aplicáveis, precisamos coletar todas as **atividades relevantes** que serão passadas para a regra. A **atividade relevante** para um P de política só incluirá um S de fluxo se ele atender à seguinte condição:

**O fluxo &quot;S&quot; é iniciado por um aplicativo que inclui a política &quot;P&quot; entre suas políticas.**

![O fluxo &quot;S&quot; é iniciado por um aplicativo que inclui a política &quot;P&quot; entre suas políticas.](assets/pdp-domain-model.png)

## Casos de uso de simulação {#dry-run-use-cases}

A apresentação abaixo tem como objetivo validar o modelo em relação a alguns casos de uso. Faremos isso gradualmente, começando com uma configuração básica e adicionando complexidade de várias maneiras.

### &#x200B;1. Um locatário. Um aplicativo. Uma política. Um fluxo {#onetenant-oneapp-onepolicy-onestream}

Começaremos com um único locatário, com um único aplicativo e uma única política associada. Vamos supor que a política determine que possa haver no máximo um fluxo ativo para qualquer usuário (o fluxo mais recente tem permissão para reprodução).

Depois que um fluxo é iniciado, a atividade só consistirá nesse fluxo e ela poderá ser reproduzida.

![Um locatário. Um aplicativo. Uma política. Um fluxo](assets/onetenant-app-policy-stream.png)


### &#x200B;2. Um locatário. Um aplicativo. Uma política. Dois fluxos. {#onetenant-oneapp-onepolicy-twostreams}

Depois que um segundo fluxo for iniciado (pelo mesmo assunto usando o mesmo aplicativo), a atividade usada para validação consistirá em **s1** e **s2**.

O limite foi excedido porque a política declara que apenas um fluxo tem permissão para reprodução, portanto, permitiremos apenas a reprodução do fluxo mais recente (**s2**).

![Um locatário. Um aplicativo. Uma política. Dois fluxos.](assets/tenant-app-policy-twostream.png)

>[!NOTE]
>
>Os diagramas representam a visualização do sistema na atividade do usuário. Para tentativas de inicialização de fluxo, a decisão de acesso será incluída na resposta. Para fluxos ativos, a decisão será retornada na resposta do heartbeat.

### &#x200B;3. Dois inquilinos. Duas aplicações. Uma política. Dois fluxos. {#twotenant-twoapp-onepolicy-twostreams}

Vamos supor agora que um novo locatário deseje aplicar a mesma política em seus aplicativos:

![Dois locatários. Duas aplicações. Uma política. Dois fluxos.](assets/onepolicy-twotenant-app-stream.png)

Devido ao fato de os dois locatários estarem vinculados pela mesma política, a situação descrita no caso de uso 2 é aplicável aqui e **s3** tem permissão para reprodução, pois é o fluxo mais recente.

### &#x200B;4. Dois inquilinos. Três aplicativos. Duas políticas. Dois fluxos. {#twotenants-threeapps-twopolicies-twostreams}

Agora, vamos supor que o segundo locatário implante um novo aplicativo e queira definir uma nova política que será compartilhada entre o **aplicativo2** e o **aplicativo3**.

![Dois locatários. Três aplicativos. Duas políticas. Dois fluxos.](assets/twotenant-policies-streams-threeapps.png)

No momento, os fluxos ativos **s3** e **s4** são permitidos. Para **s3**, quando a política **P1** for avaliada, o sistema contará somente **s3** como **atividade relevante** (**s4** não está de forma alguma relacionado à política **P1**); portanto, não há violação.

A política **P2** é aplicada a ambos os fluxos e incluirá **s3** e **s4** como atividade relevante. Como essa atividade está dentro dos limites de dois fluxos, ambos os fluxos são permitidos.

### &#x200B;5. Dois inquilinos. Três aplicativos. Duas políticas. Três correntes. {#twotenants-threeapps-twopolicies-threestreams}

Agora assumindo que uma nova tentativa de inicialização de fluxo é executada usando o **app2**:

![Dois locatários. Três aplicativos. Duas políticas. Três fluxos.](assets/twotenants-policies-threeapps-streams.png)

**s5** tem permissão para começar por **P1** (que permite que fluxos mais recentes assumam o controle), mas foi negado por **P2** e, portanto, não será iniciado.

O mesmo acontecerá se uma inicialização de fluxo for tentada com o app3: a mesma política P2 negará acesso a ela.

![](assets/stream-init-attempted-app3.png)

Agora, vamos ver o que aconteceria se o usuário tentasse criar um novo fluxo usando o app1:

![](assets/new-stream-with-app1.png)

O aplicativo app1 não está de forma alguma relacionado à política **P2**, portanto, ele só aplicará a política **P1**: que permite que o novo fluxo seja iniciado e nega o mais antigo (**s3** neste caso).
