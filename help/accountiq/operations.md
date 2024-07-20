---
title: Operações no Account IQ
description: As operações no Account IQ envolvem a tomada de ações para executar automações e operações em massa em contas de assinantes e rastrear seus efeitos.
exl-id: ba6bceca-221c-42db-b207-804e4b9f6d54
source-git-commit: 85316a40ba5f6564c84a5aecf689c077e936a91a
workflow-type: tm+mt
source-wordcount: '1417'
ht-degree: 0%

---

# Operações {#operations-tab-next-steps}

Depois de analisar os padrões de uso do assinante e identificar instâncias de compartilhamento de senhas para um segmento selecionado usando o [!DNL Account IQ] Analytics, você poderá realizar ações direcionadas por meio de procedimentos focalizados chamados operações em [!DNL Account IQ].

**As** permitem que você rastreie e gerencie efetivamente o compartilhamento de credenciais em um grupo de contas, para reduzir o compartilhamento de senhas e aprimorar a experiência de assinantes importantes.

Você pode aplicar ações a um [segmento](/help/accountiq/product-concepts.md#segment-def) definido para endereçar o compartilhamento de senhas em um [intervalo](/help/accountiq/product-concepts.md#time-interval-def) específico e agendar a operação para execução em uma data futura. Essas ações incluem restrições para minimizar o compartilhamento de senhas ou atenuar as restrições em contas que não estejam compartilhando.

Usando operações, você não só especifica ações e seu escopo, mas também mede seus resultados.

Ao avaliar os resultados, você pode refinar sua estratégia para otimizar os efeitos, seja convertendo mutuários, mitigando o compartilhamento de credenciais ou reduzindo o churn.

Você pode executar várias funções com operações:

* [Exibir relatórios de operação](#operation-reports)
* [Criar uma nova operação](#create-new-operation)
* [Parar operação](#stop-operation)

## Exibir relatórios de operação {#operation-reports}

Você pode revisar os efeitos de uma operação por meio de relatórios de operação. Para exibir o relatório de operações, selecione a guia **Operações** em **Ações** no painel esquerdo do aplicativo do Account IQ. Uma lista de operações disponíveis no sistema é exibida. Você pode acessar os principais detalhes sobre cada operação em formato tabular. Os detalhes incluem:

* Nome da operação
* Status atual (como Agendado, Em Execução, Encerrado, Com Erro ou Interrompido)
* Porcentagem de conclusão do andamento
* Público ou segmento de destino no qual a operação é aplicada
* Tipo de ação selecionada para a operação
* Data inicial da operação
* Data final da operação
* Data de criação da operação
* Data da última modificação da operação

![](assets/operations-page.png)

*Lista e detalhes de operações existentes no Account IQ*

Selecione o **Nome da Operação** desejado na lista de operações. Os seguintes relatórios são exibidos:

### Desempenho da operação {#operation-performance}

O desempenho da operação fornece uma leitura de linha superior resumindo o número de contas afetadas, o andamento da operação e a pontuação geral de compartilhamento das contas no segmento durante o [período de avaliação](/help/accountiq/product-concepts.md#evaluation-period-def) da operação.

![Relatório de desempenho da operação](assets/operation-performance.png)

*Relatório de desempenho da operação*

**A.** Contas afetadas **B.** Progresso da operação **C.** Pontuação geral de compartilhamento

#### Contas afetadas {#impacted-accounts}

Esse número exibe a contagem de contas de assinantes afetadas pela ação tomada durante o período de avaliação da operação.

#### Progresso da operação {#operation-progress}

Este medidor mostra o número de dias e a porcentagem da operação concluída fora do cronograma planejado.

#### Pontuação geral de compartilhamento {#overall-sharing-score}

Este gráfico de linhas representa a [pontuação geral de compartilhamento](/help/accountiq/data-panels.md#overall-sharing-score), que inclui o nível de compartilhamento e o uso de contas compartilhadas em cada semana durante o período de avaliação da operação.

### Impacto da operação: contas no segmento {#impact-accounts}

Esse relatório é exibido como um gráfico de colunas empilhadas que ilustra o impacto de uma operação ao longo do tempo.

![Impacto da operação em contas no gráfico de segmentos](assets/accounts-in-segment.png)

*Impacto da operação em contas no gráfico de segmentos*

O eixo x representa o [período de avaliação](/help/accountiq/product-concepts.md#evaluation-period-def) da operação, enquanto o eixo y indica o status das contas no segmento da operação. Cada barra no gráfico é dividida em três cores:

* Rosa representa o número de contas que atendem às condições do segmento usadas nessa operação.

* Azul representa o número de contas ativas originalmente no segmento, mas que não atenderam às condições do segmento durante cada semana ou mês no [período de avaliação](/help/accountiq/product-concepts.md#evaluation-period-def) da operação.

* Cinza representa as contas que estavam inativas durante o período de avaliação.

>[!NOTE]
>
>A primeira barra rosa representa o número de contas que atendem às condições do segmento de operação no início do período de avaliação.

Com o tempo, o gráfico ilustra as alterações no comportamento da conta em relação aos critérios originais (por exemplo, ter uma probabilidade de compartilhamento superior a 90 e usar mais de 5 dispositivos ficou inativo).

### Impacto da operação: métricas de contas compartilhadas {#impact-shared-accounts}

As métricas de contas compartilhadas fornecem uma visão geral do nível de compartilhamento e das solicitações de reprodução pelas contas de assinantes no segmento da operação durante o [período de avaliação](/help/accountiq/product-concepts.md#evaluation-period-def) da operação.

#### Nível de compartilhamento {#share-level}

Este gráfico de linhas representa o [nível de compartilhamento](/help/accountiq/data-panels.md#sharing-level) a cada semana durante o período de avaliação da operação.

![Gráfico de linhas de nível de compartilhamento](assets/share-level.png){width="550" align="left"}

*Gráfico de linhas de nível de compartilhamento*

#### Número de solicitações de reprodução {#play-requests}

Este gráfico de linhas representa as [solicitações de reprodução](/help/accountiq/general-usage-reports.md#playreq-uniquesubs) todas as semanas no período de avaliação da operação.

![Gráfico de linhas de número de solicitações de reprodução](assets/number-play-requests.png){width="550" align="left"}

*Gráfico de linhas de número de solicitações de reprodução*

### Impacto da operação: métricas de uso geral {#impact-general-usage}

As métricas de uso geral fornecem uma visão geral do número médio de dispositivos, IPs e locais no segmento da operação durante o [período de avaliação](/help/accountiq/product-concepts.md#evaluation-period-def) da operação.

#### Número de dispositivos {#devices}

Este gráfico de linhas representa o [número médio de dispositivos](/help/accountiq/general-usage-reports.md#devices-week-account) a cada semana no período de avaliação da operação.

![Gráfico de linhas de dispositivos](assets/number-devices.png){width="550" align="left"}

*Gráfico de linhas de dispositivos*

#### Número de IPs e localizações {#IPs-locations}

Este gráfico de linhas representa o [número médio de IPs](/help/accountiq/general-usage-reports.md#ip-week-account) e [locais](/help/accountiq/general-usage-reports.md#locations-week-account) semanalmente no período de avaliação da operação.

![Gráfico de linhas de número de IPs e localizações](assets/number-ips-locations.png){width="550" align="left"}

*Gráfico de linhas de número de IPs e localizações*

Para fechar o relatório e voltar à página principal de **Operações**, selecione a guia **Operações** em **Ações** no painel esquerdo.

## Criar nova operação {#create-new-operation}

Ao ir para a guia **Operações** em **Ações** no painel esquerdo, selecione **Criar nova operação** na parte superior da página **Operações**.

Para criar uma nova operação, siga as instruções nas seguintes seções:

* [Detalhes da operação](#operation-details)
* [Segmento](#segment)
* [Ação](#action)
* [Agendar](#schedule)

### Detalhes da operação {#operation-details}

Nesta seção, digite o nome da operação em **Nome da operação**.

>[!TIP]
>
>Descreva a finalidade da operação ou a natureza da ação em **nome da operação** para identificação rápida. A opção de **Adicionar descrição e marcas** estará disponível em versões futuras.

![Adicionar nome da operação aos detalhes da operação](assets/operation-details.png)

*Adicionar nome da operação*

### Segmento {#segment}

Nesta seção, clique em **Selecionar segmento** e escolha um segmento para o qual deseja usar esta operação. Saiba [como selecionar um segmento](/help/accountiq/segments-timeinterval.md#segment-selection).

Depois de selecionar um segmento, use Ícone <img alt= "expandir resumo de segmentos" src="./assets/expand-segment-summary.svg" width="25"> para exibir o resumo detalhado do segmento. Leia mais sobre [resumo do segmento](segments-timeinterval.md#segment-summary).

![Selecionar segmento e intervalo de tempo](assets/select-segment-timeinterval.png)

*Selecionar segmento e intervalo de tempo*

>[!NOTE]
>
>As [categorias de vídeo](product-concepts.md#video-category-def) mostradas na imagem anterior, como **MVPDs**, **Programadores** e **Canais**, representam os rótulos usados na versão para TV em Qualquer Lugar do Account IQ. Se você estiver conectado como um serviço D2C, esses rótulos exibirão as categorias de vídeo específicas da sua empresa.

Se necessário, use Ícone <img alt= "editar segmento" src="./assets/edit-segment.svg" width="25"> para editar o segmento selecionado ou  Ícone <img alt= "criar novo segmento" src="./assets/create-new-segment.svg" width="25"> para criar um novo segmento. Para obter mais detalhes, consulte as instruções para [criar um novo segmento](work-with-segments.md#create-new-segment) ou [editar um segmento](work-with-segments.md#edit-segment).

>[!IMPORTANT]
>
>O **tipo de segmento** denominado **[!UICONTROL Fixed number of accounts]** está selecionado por padrão no momento. A opção para selecionar **[!UICONTROL Variable number of accounts]** estará disponível em versões futuras.

Selecione **Granularidade e intervalo de tempo** para monitorar a operação durante um período específico. Saiba mais sobre [como selecionar granularidade e intervalo de tempo](/help/accountiq/segments-timeinterval.md#granularity-timeinterval).

### Ação {#action}

Nesta seção, escolha no menu suspenso uma **Ação** que deseja executar no segmento selecionado.

![Selecione o tipo de Ação](assets/apply-actions.png)

*Selecione o tipo de Ação*

Há duas opções disponíveis:

* Selecione a **Política CM** para o sistema de monitoramento simultâneo integrado com o Account IQ.

* Selecione **Ações externas** para criar e processar fluxos de trabalho externos para o Account IQ e não integrados ao sistema Account IQ.

>[!NOTE]
>
>As ações externas nem sempre podem estar diretamente relacionadas ao compartilhamento de senhas, mas ainda podem afetá-lo, como o lançamento de uma nova temporada.

### Agendar {#schedule}

Nesta seção, selecione a **Data inicial** e a **Data final** no seletor de datas para definir a ativação para a operação.

>[!IMPORTANT]
>
>Atualmente, a **Data inicial** e a **Data final** de ativação padrão estão definidas como **Na data**. A opção para selecionar **Quando uma condição for atendida** e **Manualmente** estará disponível em versões futuras.

>[!NOTE]
>
>Verifique se a data de início e a data de término estão alinhadas com a granularidade selecionada para avaliação na **Etapa 4**.

* Se você optou pela granularidade agregada por semanas, selecione as datas de início e término em semanas (por exemplo, Semana 10).
* Se você optou pela granularidade agregada por meses, selecione as datas de início e término em meses.

![Selecione data de início e data de término no seletor de datas](assets/add-schedule.png)

*Selecione Data de início e Data de término no seletor de datas*

**A.** Seletor de data inicial **B.** Seletor de data final

>[!NOTE]
>
>A **Data de início** deve ser posterior ao período de avaliação e à data atual, enquanto a **Data de término** deve ser posterior à data de início e à data atual para agendar e executar operações no período futuro.

Selecione **Salvar operação** na parte superior da página **Operações** para processar uma nova operação.

## Parar operação {#stop-operation}

Você só pode parar as operações que estão atualmente no status **Em execução**. Para interromper uma operação existente, siga estas etapas:

1. Navegue até a guia **Operações** em **Ações**, na navegação à esquerda do aplicativo Account IQ.
1. Selecione o menu **Opções** da operação que deseja parar.

   ![Selecione o menu de opções para interromper a operação](assets/stop-operation.png)

   *Selecione o menu Opções para interromper a operação*

1. Selecione **Parar**.



