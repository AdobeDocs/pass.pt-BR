---
title: Glossário do Account IQ
description: Um glossário de terminologias de produtos.
exl-id: 2ee54442-9538-4c30-b999-265310b3935f
source-git-commit: cfcaa00ab05c99a64bcb0edfe5af60845a6769a9
workflow-type: tm+mt
source-wordcount: '1330'
ht-degree: 0%

---

# Conceitos e glossário do produto {#glossary}

## Terminologias comuns em D2C e TV em todos os lugares

As seguintes terminologias de produtos e suas definições são comuns a todos [versões do Account IQ](versions-aiq.md).

### [!UICONTROL Accounts Sharing Probability] {#account-sharing-probability-def}

Um painel de painel com gráficos que dividem as pontuações de compartilhamento do segmento atual em categorias de intervalo de compartilhamento Muito baixo, Baixo, Moderado, Alto e Muito alto.

### [!UICONTROL Action] {#action-def}

Um evento direto ou indireto associado a um [Operação](#operation-def) que afeta as características (por exemplo, pontuação de compartilhamento ou número de dispositivos em uso) de um segmento de operação relacionado.

### [!UICONTROL Aggregated sharing score] {#sharing-probability-level-def}

Um painel de painel com gráficos que dividem as pontuações de compartilhamento do segmento atual em categorias de intervalo de compartilhamento Muito baixo, Baixo, Moderado, Alto e Muito alto, juntamente com cada porcentagem de categorias da quantidade total de streaming do segmento.

### [!UICONTROL AuthN] {#authn-def}

O número de tentativas de autenticação. Uma tentativa de autenticação é o processo pelo qual um usuário tenta fazer logon com o serviço D2C ou MVPD. Para usuários do TV Everywhere, o usuário é redirecionado para o MVPD escolhido, onde se identificam para o MVPD - normalmente com um nome de usuário e senha.

### [!UICONTROL AuthN OK] {#authn-ok-def}

O número de autenticações bem-sucedidas. Uma autenticação bem-sucedida ocorre quando a identificação de um usuário é confirmada por um serviço D2C ou MVPD. Para usuários do TV Everywhere, isso resulta no redirecionamento do usuário de volta para o aplicativo ou site do programador.

### [!UICONTROL Cluster] {#cluster-def}

Um cluster é uma coleção de locais e dispositivos. Os clusters são criados encontrando locais comuns entre dispositivos. Os dispositivos que foram vistos em um local comum serão considerados como pertencentes ao mesmo cluster. Dois dispositivos podem estar no mesmo cluster, mesmo que não tenham locais comuns, mas podem ser conectados por meio dos locais de outros dispositivos.

#### [!UICONTROL Mobile cluster] {#mobile-cluster-def}

Um cluster que não tem dispositivos estáticos.

#### [!UICONTROL Static cluster] {#static-cluster-def}

Um cluster que tenha pelo menos um dispositivo estático.

### [!UICONTROL Concurrency] {#consurrency-def}

O simultâneo é definido por dois (ou mais) fluxos reproduzidos ao mesmo tempo ou muito próximo no tempo, de modo que o intervalo entre eles não pode ser justificado por viajar a uma velocidade normal.
O uso simultâneo é calculado usando a velocidade máxima (milhas/hora) entre 2 clusters diferentes. Considera-se que um utilizador tem utilização simultânea se tiver uma velocidade superior a 124 m/h a uma distância inferior a 124 milhas ou se tiver uma velocidade superior a 400 m/h a uma distância superior a 124 milhas. A distância é calculada entre locais de clusters diferentes. O uso simultâneo é permitido no mesmo cluster.

### [!UICONTROL Device] {#device-def}

Um produto de hardware de vídeo digital capaz de reproduzir conteúdo de upstream. Por exemplo, smartphones, notebooks e desktops, consoles de jogos e televisões inteligentes.

### [!UICONTROL Evaluation period] {#evaluation-period-def}

O período de avaliação é o tempo desde o início da ação associada à Operação até o fim da ação ou sua medição.

### [!UICONTROL Geographical Span] {#geographical-span-def}

A distância entre os pontos mais distantes em um conjunto de locais.

### [!UICONTROL Granularity] {#granularity-def}

Em referência ao intervalo de tempo, o tamanho do período; como uma semana ou um mês.

### [!UICONTROL IP] {#ip-def}

O endereço de protocolo IP atribuído a um dispositivo por um provedor de serviços de Internet. Por exemplo, provedor de serviços de cabo e provedor de serviços de célula.

### [!UICONTROL Location] {#location-def}

Um ponto único na Terra. Também é conhecida como geolocalização para um pedido de reprodução específico com uma precisão de 1000 m x 1000 m (um km quadrado).

### [!UICONTROL Media Company] {#media-company-def}

Media Company é uma empresa proprietária de um grupo de redes de mídia.

### [!UICONTROL Metric] {#metric}

Métrica é um atributo da conta do assinante (por exemplo, seu MVPD, os programadores e canais do conteúdo que transmitem e o número de dispositivos que usam).

### [!UICONTROL Mobile device] {#mobile-device-def}

Um dispositivo com alta mobilidade. Por exemplo, celular e tablet.

### Operação {#operation-def}

A operação é um registro criado para rastrear o efeito de um determinado [ação](#action-def) em um segmento associado. Um exemplo de ação pode ser um limite colocado no número de fluxos simultâneos permitidos para contas identificadas pelo segmento.

### [!UICONTROL Overall sharing score] {#overall-sharing-score}

Um valor que ajuda os usuários a entender a magnitude do compartilhamento de senhas e fornece a eles um senso de urgência para agir em relação a ele.

### [!UICONTROL Play Request] {#play-requests-def}

Equivalente a um início de fluxo. Este evento marca o início de um fluxo de conteúdo de usuário.

### [!UICONTROL Risk Index-Usage] {#risk-index-usage}

Também conhecido como Uso de contas compartilhadas, é um valor calculado com base no número de solicitações de reprodução feitas por cada conta ponderadas pela probabilidade de compartilhamento de cada conta. Também é conhecido como Uso pelo Índice de Risco de Contas Compartilhadas.

### [!UICONTROL Segment] {#segmet-def}

Segmento é um conjunto de contas que atendem às condições definidas pelo usuário especificadas pelas métricas selecionadas. Por exemplo, &quot;usuários nas regiões A, B, C, D ou E que têm mais de três dispositivos&quot;.

### [!UICONTROL Sharing level] {#sharing-level-def}

Também conhecido como Índice de risco - Contas ou Índice de risco de contas compartilhadas, é um valor calculado com base em uma média da probabilidade de compartilhamento calculada para cada conta no segmento atual que foi transmitida pelo menos uma vez durante o intervalo selecionado.

### [!UICONTROL Static device] {#static-device-def}

Um dispositivo com baixa mobilidade. Por exemplo, console de jogos, decodificador de sinais e televisor.

### [!UICONTROL Time interval] {#time-interval-def}

Também conhecida como ponto, é a janela de tempo que contém a atividade de solicitação de reprodução representada na interface do usuário e nas tabelas do início ao fim.

### [!UICONTROL Trend] {#trend-def}

A diferença de porcentagem na métrica associada (por exemplo, porcentagem do total de solicitações de reprodução) entre o período atual e o anterior.

### [!UICONTROL Unique Subscribers] {#unique-subscriber-def}

O número de contas exclusivas que foram transmitidas pelo menos uma vez durante um determinado período.

### [!UICONTROL Usage] {#usage-defs}

#### [!UICONTROL Avid user] {#avid-user-def}

Mais de 37 solicitações de reprodução por mês.

#### [!UICONTROL Infrequent user] {#infrequent-users-def}

Menos de 9 solicitações de reprodução por mês.

#### [!UICONTROL Regular user] {#regular-user-def}

De 9 a 37 solicitações de reprodução por mês.

### [!UICONTROL Usage Pattern] {#usage-patern-def}

Um de um conjunto finito de rótulos de categoria aplicados a uma conta que melhor caracteriza os usuários da conta em termos de grupos sociais ou comportamentos (por exemplo, uma pequena família, um viajante ou viajante, compartilhamento social etc.).

### [!UICONTROL Video category] {#video-category-def}

#### [!UICONTROL For D2C service] {#d2c-service}

Uma categoria de vídeo é um rótulo específico que qualifica a natureza do vídeo. Por exemplo, região da origem, tipos de conteúdo, como VOD ou Em tempo real, evento ou rótulos específicos, como equipe.

#### [!UICONTROL For TV Everywhere] {#tv-everywhere}

Uma categoria de vídeo é definida pela combinação de Programador, canal e MVPD associada ao fluxo.

### [!UICONTROL Zip Code] {#zip-code-def}

O CEP dos EUA associado a locais dentro dos EUA
<!--calculated metrics-->


## Terminologias específicas da TV Everywhere

### [!UICONTROL AuthZ] {#authz-def}

Autorização ou o número da solicitação de autorização. Uma solicitação de autorização é o processo pelo qual um programador solicita permissão de um MVPD por meio do Adobe para começar a transmitir o conteúdo solicitado por um usuário. O MVPD normalmente concede a solicitação com base nos direitos de conteúdo associados à assinatura do MVPD do usuário (por exemplo, se o canal associado ao conteúdo está na assinatura do usuário). Algumas respostas de solicitação de autorização são armazenadas em cache pelo Adobe, o que permite que o Adobe responda imediatamente sem transmitir a solicitação para o MVPD.

### [!UICONTROL AuthZ OK] {#authz-ok-def}

O número de autorizações bem-sucedidas.

### [!UICONTROL Channel] {#channel-def}

O canal, também conhecido como Propriedade, é uma fonte de conteúdo de vídeo relacionada a temas. Tradicionalmente, representa um feed de vídeo contínuo distinto, endereçável numericamente, de um MVPD. O canal mapeia diretamente para um canal acessível de conteúdo disponível para os assinantes por meio do Set Top Box (STB).

### [!UICONTROL Industry Average Index] {#industry-avg-index-def}

Um valor calculado para cada um dos Índices de Risco (Contas, Uso, Geral) entre todos os Programadores e MVPDs durante o intervalo selecionado.

### [!UICONTROL Isolation Mode] {#isolation-mode-def}

Um tipo de análise de compartilhamento em que a avaliação de uma conta é limitada a eventos que ocorreram diretamente nos programadores no segmento selecionado.  Normalmente, todos os eventos de conta são avaliados, o que fornece uma estimativa muito mais precisa de compartilhamento.  Alguns dados do MVPD são estruturados de uma maneira que permite apenas a análise do Modo de isolamento.

### [!UICONTROL MVPD] {#mvpd-def}

MVPD, também conhecido como Distribuidor, é um agregador, revendedor e distribuidor de conteúdo de vídeo da Media Company.

### [!UICONTROL Programmer] {#programmer-def}

Programmer, também conhecida como Network, é uma empresa que é subsidiária de uma empresa maior (corporação) que possui e gerencia um ou mais canais.

### [!UICONTROL requestorID] {#requestorid-def}

A ID que uma Empresa de mídia usa para identificar a si mesma ou a uma subsidiária de um MVPD.  Dependendo da implementação do Programador, isso pode mapear para uma Empresa de mídia, Programador ou Canal. Tradicionalmente, isso é mapeado para um Canal.  Com a criação de pseudo-canais como MML (March Madness Live) e movimentações tecnicamente orientadas para lidar com limitações de dados orientadas por MVPD, requestorID está começando a se tornar mais associado com a Media Company.

### [!UICONTROL resourceID] {#resource-id-def}

O conteúdo solicitado pelo usuário final. Tradicionalmente, isso identificou o Canal associado ao conteúdo que o usuário solicitou.  As melhorias no sistema permitem que a ID represente programas específicos (por exemplo, com classificações específicas), a ID continua identificando o Canal associado.


