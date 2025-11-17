---
title: Painel ESM
description: Saiba como usar o painel do ESM para monitorar os dados de direitos e eventos entre parceiros da MVPD.
source-git-commit: 53ebbd82fc160f68fccdddb18cf98e249ad6ecce
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 1%

---


# Painel ESM {#esm-dashboard}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso dessa API exige uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

O painel ESM fornece uma visão unificada dos dados de direitos e eventos para ajudá-lo a monitorar o desempenho, identificar anomalias e entender os padrões de acesso do usuário em todos os parceiros da MVPD. Este guia explica como usar os filtros do painel, interpretar relatórios e entender as métricas principais em intervalos de tempo configuráveis.

![Exibição do painel do ESM](../assets/tve-dashboard/new-tve-dashboard/esm/esm-full-page.png)

## Casos de uso {#use-cases}

- Visualizar tendências por plataforma ou MVPD
- Comparar desempenhos do MVPD
- Compreender o uso do cliente por aplicativo

Mais detalhes sobre dados e eventos ESM podem ser encontrados em [Visão geral do monitoramento do serviço de qualificação](https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview).

## Relatórios {#reports}

Os seguintes relatórios estão disponíveis:

### Reproduzir solicitações {#play-requests}

Mostra o número de solicitações de reprodução para o intervalo selecionado.

**Estilo do gráfico** - linha.

### Conversão de autenticação {#authentication-conversion}

Mostra a proporção entre eventos de autenticação bem-sucedidos e o número total de tentativas de autenticação.

**Estilo do gráfico** - barra horizontal

### Conversão de autorização {#authorization-conversion}

Mostra a proporção entre eventos de autenticação bem-sucedidos e o número total de tentativas de autenticação.

**Estilo do gráfico** - barra horizontal

### Latência de autorização {#authorization-latency}

Mostra a latência média (em milissegundos) para respostas do MVPD.

**Estilo do gráfico** - linha.

### Uso do MVPD {#mvpd-usage}

Mostra uma comparação do número de usuários únicos por MVPD.

**Estilo do gráfico** - área empilhada.

### Autenticações com êxito {#successful-authentications}

Mostra o número total de autenticações bem-sucedidas para o intervalo selecionado.

**Estilo do gráfico** - linha.

### Autorizações bem-sucedidas {#successful-authorizations}

Mostra o número total de autorizações bem-sucedidas (respostas &quot;Permissão&quot; da MVPD) para o intervalo selecionado.

**Estilo do gráfico** - linha.

## Ações {#actions}

### Exibir por {#view-by}

Para cada gráfico, há uma lista suspensa &quot;Exibir por&quot; que permite selecionar exatamente quais dados serão exibidos.

- **Agregado** - exibe os dados gerais
- **MVPDs / Canais / Plataformas** - descreve os filtros específicos selecionados

### Baixar {#download}

Você pode baixar os dados brutos:

- **Baixar dados do gráfico como CSV** - baixa dados para um gráfico específico
- **Baixar todos os dados como CSV** - baixa todos os dados ESM em todos os gráficos

## Filtros {#filters}

Use filtros para restringir o conjunto de dados e focalizar a análise. Os seguintes filtros estão disponíveis:

- **Canal**: inclui todos os canais (marcas) disponíveis
- **MVPD**: focalize em um ou mais provedores
- **Plataforma**: Web, celular, TV conectada ou família de dispositivos

Para adicionar um novo filtro, clique no botão &quot;Adicionar filtros&quot;.

Na página &quot;Filtros de conjunto de dados&quot;, você pode arrastar e soltar os filtros necessários.

![Filtros do painel do ESM](../assets/tve-dashboard/new-tve-dashboard/esm/filters-modal.png)

Para cada seção, você pode remover filtros individualmente ou limpar toda a seleção.

### Dicas de filtro {#filter-tips}

- Combine vários filtros para isolar uma coorte (por exemplo, um MVPD em uma plataforma móvel para um canal).
- Não adicione filtros para diminuir o zoom e estabelecer uma linha de base antes de fazer drill-in.

## Intervalos de tempo {#time-intervals}

Controle a janela e a granularidade da análise.

![Intervalos de tempo do Painel ESM](../assets/tve-dashboard/new-tve-dashboard/esm/date-picker.png)

### Intervalo de datas {#date-range}

**Predefinições**: Hoje, Semana atual, Últimos 7 dias, Mês atual, Últimos 30 dias, Últimos 3 meses, Últimos 6 meses, Últimos 12 meses

**Personalizado**: selecione o intervalo de tempo desejado

### Granularidade {#granularity}

Diário / Mensal
