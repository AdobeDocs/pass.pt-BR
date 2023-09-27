---
title: Ponto de decisão da política
description: Ponto de decisão da política
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '725'
ht-degree: 0%

---


# Ponto de decisão da política {#policy-desc-pt}

## Modelo de domínio {#domain-model}

Esta página serve como referência para diferentes casos de uso e implementações de políticas. Recomendamos que você também consulte o [Glossário](/help/concurrency-monitoring/cm-glossary.md) parte da documentação para definições de termos.

A **inquilino** possui **aplicativos** para as quais pretende executar **políticas**. **Aplicativos clientes** deve ser configurado com o **ID do aplicativo** (fornecido pelo Adobe).

Em seguida, o locatário associa cada aplicativo a uma ou mais políticas, criadas por ele ou criadas e compartilhadas por outros. As políticas podem ser vinculadas entre vários locatários.

A variável **atividade do assunto** consiste em todos os fluxos (independentemente do aplicativo) relatados ao Monitoramento de simultaneidade para um determinado assunto.

Quando um fluxo deve ser autorizado para um determinado assunto, o sistema primeiro verificará todas as políticas definidas para o aplicativo que criou o fluxo.

Para cada uma das políticas aplicáveis, precisamos coletar todos os **atividade relevante** que será passado para a regra. A variável **atividade relevante** para uma política P, só incluirá um fluxo S se atender à seguinte condição:

**O fluxo &quot;S&quot; é iniciado por um aplicativo que inclui a política &quot;P&quot; entre suas políticas.**

![O fluxo &quot;S&quot; é iniciado por um aplicativo que inclui a política &quot;P&quot; entre suas políticas.](assets/pdp-domain-model.png)

## Casos de uso de simulação {#dry-run-use-cases}

A apresentação abaixo tem como objetivo validar o modelo em relação a alguns casos de uso. Faremos isso gradualmente, começando com uma configuração básica e adicionando complexidade de várias maneiras.

### 1. Um locatário. Um aplicativo. Uma política. Um fluxo {#onetenant-oneapp-onepolicy-onestream}

Começaremos com um único locatário, com um único aplicativo e uma única política associada. Vamos supor que a política determine que possa haver no máximo um fluxo ativo para qualquer usuário (o fluxo mais recente tem permissão para reprodução).

Depois que um fluxo é iniciado, a atividade só consistirá nesse fluxo e ela poderá ser reproduzida.

![Um inquilino. Um aplicativo. Uma política. Um fluxo](assets/onetenant-app-policy-stream.png)


### 2. Um locatário. Um aplicativo. Uma política. Dois fluxos. {#onetenant-oneapp-onepolicy-twostreams}

Depois que um segundo fluxo for iniciado (pelo mesmo assunto usando o mesmo aplicativo), a atividade usada para validação consistirá em **s1** e **s2**.

O limite foi excedido porque a política declara que somente um fluxo tem permissão para reprodução, portanto, só permitiremos o fluxo mais recente (**s2**) para jogar.

![Um inquilino. Um aplicativo. Uma política. Dois fluxos.](assets/tenant-app-policy-twostream.png)

>[!NOTE]
>
>Os diagramas representam a visualização do sistema na atividade do usuário. Para tentativas de inicialização de fluxo, a decisão de acesso será incluída na resposta. Para fluxos ativos, a decisão será retornada na resposta do heartbeat.

### 3. Dois inquilinos. Duas aplicações. Uma política. Dois fluxos. {#twotenant-twoapp-onepolicy-twostreams}

Vamos supor agora que um novo locatário deseje aplicar a mesma política em seus aplicativos:

![Dois inquilinos. Duas aplicações. Uma política. Dois fluxos.](assets/onepolicy-twotenant-app-stream.png)

Uma vez que os dois locatários estão ligados pela mesma política, a situação descrita no caso de utilização 2 é aplicável aqui e **s3** pode ser reproduzido, pois é o fluxo mais recente.

### 4. Dois inquilinos. Três aplicativos. Duas políticas. Dois fluxos. {#twotenants-threeapps-twopolicies-twostreams}

Agora, vamos supor que o segundo locatário implante um novo aplicativo e deseje definir uma nova política que será compartilhada entre **app2** e **app3**.

![Dois inquilinos. Três aplicativos. Duas políticas. Dois fluxos.](assets/twotenant-policies-streams-threeapps.png)

Nesse momento, os fluxos ativos **s3** e **s4** são permitidos. Para **s3**, quando a política **P1** for avaliado, o sistema somente contará **s3** as **atividade relevante** (**s4** não tem qualquer relação com a política **P1** então não há violação.

Política **P2** aplica-se a ambos os fluxos e incluirá **s3** e **s4** atividade relevante. Como essa atividade está dentro dos limites de dois fluxos, ambos os fluxos são permitidos.

### 5. Dois inquilinos. Três aplicativos. Duas políticas. Três correntes. {#twotenants-threeapps-twopolicies-threestreams}

Agora assumindo que uma nova tentativa de inicialização de fluxo é executada usando **app2**:

![Dois inquilinos. Três aplicativos. Duas políticas. Três correntes.](assets/twotenants-policies-threeapps-streams.png)

**s5** pode começar por **P1** (o que permite que fluxos mais recentes assumam o controle), mas é negado por **P2**, então não vai começar.

O mesmo acontecerá se uma inicialização de fluxo for tentada com o app3: a mesma política P2 negará acesso a ela.

![](assets/stream-init-attempted-app3.png)

Agora, vamos ver o que aconteceria se o usuário tentasse criar um novo fluxo usando o app1:

![](assets/new-stream-with-app1.png)

O aplicativo app1 não está de forma alguma relacionado à política **P2**, portanto, só aplicará a política **P1**: que permite que o novo fluxo seja iniciado e nega o mais antigo (**s3** neste caso).

