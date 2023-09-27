---
title: Operações no Account IQ
description: As operações no Account IQ envolvem a tomada de ações para executar automações e operações em massa em contas de assinantes e rastrear seus efeitos.
exl-id: ba6bceca-221c-42db-b207-804e4b9f6d54
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 0%

---

# Operações {#operations-tab-next-steps}

Depois de entender os padrões de uso dos assinantes e identificar o compartilhamento de senhas para o segmento selecionado (usando [!DNL Analytics] no Account IQ), você pode realizar ações direcionadas a um objetivo para reduzir o compartilhamento de senhas.

A funcionalidade de operações no Account IQ ajuda você a lidar e gerenciar com eficiência o compartilhamento de credenciais por meio de procedimentos focados chamados operações. Ele oferece opções para projetar um objetivo e personalizar ações direcionadas (com base no objetivo) para grupos específicos de contas de assinantes, além de automatizar a execução para uma duração futura. Através da funcionalidade Operações, você pode não só criar e executar operações, mas também medir seus impactos. Então, avaliando os impactos, você pode ajustar sua estratégia para otimizar o efeito, seja convertendo mutuários ou mitigando o compartilhamento de credenciais.

Para exibir **Operações** seleção de página **Operações** opção em **Ações** na navegação à esquerda do aplicativo Account IQ. A página Operações lista todas as operações já existentes no sistema Account IQ, juntamente com seus detalhes.

![](assets/operations-page.png)

*Figura: Lista e detalhes de operações existentes no Account IQ*

Na página Operações, é possível:

* Exibir uma lista de operações já existentes no Account IQ

* Exibir detalhes da operação, como:

   * Status (Agendado, Em Execução, Encerrado, Com Erro ou Interrompido)

   * progresso (em porcentagem de conclusão)

   * público alvo (segmento no qual executar a operação)

   * programação (data inicial e final da operação)

   * data de criação e de término da operação

* [Criar nova operação](/help/accountiq/operation-affecting-user-segment.md)

* [Exibir relatórios de operação](#operation-reports)

<!--* Search from the list of operations using Search field

* Stop an operation.

* Create a duplicate operation.

* [Configure columns of Operations details page](#configure-columns)-->

## Exibir relatórios de operação {#operation-reports}

Você pode analisar os impactos de uma operação exibindo seu relatório. Para exibir um relatório de operação:

1. Selecione o nome da operação na página principal Operações.

   O relatório é exibido no formato de um gráfico de colunas empilhadas.

   ![](assets/operation-impact-report.png)

   *Figura: Relatório de operações para visualizar os impactos das operações*

   O eixo X representa o período de avaliação e o eixo y representa o impacto da operação (em termos de número de contas em um segmento durante o período de avaliação). Cada barra é dividida em três partes.

   * Uma parte representa o número de contas que ainda atendem aos critérios do segmento de operação.

   * Outra parte representa o número de contas ativas para esse período que estavam originalmente no segmento, mas que não atendem mais aos critérios do segmento de operação.

   * A terceira parte representa as contas que não estavam ativas nesse período.

   >[!NOTE]
   >
   >A primeira barra representa o número de contas que atendem às condições do segmento de operação no início do período de avaliação.

   Com o tempo, o gráfico mostra o efeito da sua ação (por meio da operação ), indicando o número de contas que alteraram seu comportamento em relação aos critérios originais (por exemplo, têm probabilidade de compartilhamento superior a 90 e usam mais de 5 dispositivos) ou que se tornaram inativas.

<!--For example, in the above image the variable on the y-axis is number of accounts. Looking at the graph you can compare the number of accounts that are in the operations' segment versus the number of accounts that are outside the operations segment at a particular time (such as week 2nd of the operations evaluation period). Therefore, you can analyze how over the evaluation period do number of accounts vary within the operation segment and outside the segment.

So, if your operation was to send out warning emails to suspecting accounts, and accounts in operations segment were those with sharing probability more than 90 and using more than 5 devices to stream content, then in the beginning of the evaluation period accounts in segment are more than 17 thousand. This number changes over the evaluation period as shown in the graph, thereby indicating the impact of operation. Based on the evaluation, you can take remedial measures on suspecting accounts, or continue with the operation, or adjust your strategy for better outcomes to curb credential sharing.-->

1. Para fechar o relatório e voltar à página principal de Operações, selecione **Operações** opção em **Ações** na navegação à esquerda.

<!--

![](assets/operations-details.png)

*Figure: Operation details*
## Configure columns {#configure-columns}

You can select the icon to **Configure columns** on the top of the operations table.

![](assets/config-columns.png)

*Figure: Configure columns of Operations details page*-->