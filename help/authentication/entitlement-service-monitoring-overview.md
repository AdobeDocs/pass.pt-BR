---
title: Visão Geral do Monitoramento do Serviço de Direito
description: Visão Geral do Monitoramento do Serviço de Direito
exl-id: ebd5d650-0a32-4583-9045-5156356494e2
source-git-commit: 1ad2a4e75cd64755ccbde8f3b208148b7d990d82
workflow-type: tm+mt
source-wordcount: '1303'
ht-degree: 0%

---

# Visão Geral do Monitoramento do Serviço de Direito {#entitlement-service-monitoring-overview}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#introduction}

Os sites e aplicativos da TVE precisam estar disponíveis 24 horas por dia, 7 dias por semana, para que os clientes precisem de insights em tempo real sobre eventos de direito para detectar e corrigir problemas o mais rápido possível. Eles também precisam analisar dados mensais para determinar quais plataformas estão fornecendo a maior parte do tráfego e quais plataformas podem ter uma implementação incorreta e taxas de conversão inadequadas.

O Entitlement Service Monitoring (ESM) fornece aos programadores e MVPDs um feed de dados que oferece visibilidade em tempo real sobre seus eventos de Autenticação e Autorização. Os dados são coletados dos sistemas de autenticação da Adobe Pass e fornecidos por meio de uma API RESTful.  Os clientes podem consumir os dados diretamente ou por meio de seus próprios painéis operacionais personalizados.

Os elementos centrais do sistema MEE são as suas métricas e dimensões. O ESM gera relatórios que contêm métricas agregadas de acordo com a seleção da dimensão. Como os eventos do Adobe Pass são registrados no fuso horário PST, os relatórios ESM também estão disponíveis no fuso horário PST.

A API ESM geralmente não está disponível.  Entre em contato com o representante da Adobe para tirar dúvidas sobre disponibilidade.

## ESM para programadores {#esm-for-programmers}

### Os programadores podem monitorar as seguintes métricas: {#programmers-monitor-metrics}


| *Nome das métricas* | *Descrição* |
|-------------------------|--------------------------|
| tentativas de autenticação | Número de fluxos de autenticação iniciados |
| autn-successful | Número de tokens de autenticação obtidos com êxito pelos clientes |
| autoria pendente | Número de tokens de autenticação gerados com êxito (ignorando se o cliente realmente o obteve) |
| autn-failed | Número de falhas de autenticação executadas por meio de um sistema externo. |
| tokens sem cliente | Número de tokens sem cliente emitidos com êxito |
| clientless-failures | Número de tentativas mal sucedidas de receber tokens da API sem cliente |
| authz-tries | Número de tentativas de autorização |
| authz-successful | Número de autorizações bem-sucedidas |
| authz-failed | Número de autorizações negadas por MVPDs no nível do aplicativo |
| authz-rejected | Número de tentativas de autorização consideradas mal-intencionadas pelo Provedor de Serviços da Adobe e rejeitadas como resultado de uma prevenção de ataque de DoS |
| authz-latency | Número total de milissegundos gastos no ponto de extremidade do MVPD |
| media-tokens | Número de tokens de mídia curtos gerados (que são assimilados pelo número de solicitações de reprodução) |
| contas únicas | Número de usuários únicos que executaram ações de autorização (AuthN / AuthZ) no intervalo selecionado. (Essa métrica só será exibida se os valores diários forem solicitados.) </br> Isso é computado para cada Data Center individual. Quando a dimensão &quot;dc&quot; não é solicitada, essa métrica não é exibida. |
| unique-sessions | Número de sessões exclusivas que executaram chamadas de fluxo de autenticação para o serviço de Autenticação do Adobe Pass dentro do intervalo selecionado. (Essa métrica só será exibida se os valores diários forem solicitados.) </br> Isso é computado para cada Data Center individual. Quando a dimensão &quot;dc&quot; não é solicitada, essa métrica não é exibida. |
| count | Um contador simples usado nos relatórios orientados a eventos |

</br>

### Os programadores podem filtrar as métricas listadas acima pelas seguintes dimensões: {#progr-filter-metrics}


| *Nome do Dimension* | *Descrição* |
|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ano | O ano de 4 dígitos |
| mês | O mês do ano (1-12) |
| dia | O dia do mês (1-31) |
| hora | A hora do dia |
| minuto | O minuto da hora |
| media-company | A empresa de mídia proprietária do site que iniciou o processo de qualificação para o usuário |
| dc | (Centro de dados) A região de origem onde a solicitação foi recebida. |
| proxy | O proxy MVPD (que será &quot;Direto&quot; para integrações diretas) |
| mvpd | O MVPD responsável por conceder o direito ao usuário |
| requestor-id | A ID do solicitante usada para executar a solicitação de direito |
| channel | O site do canal, extraído do campo de recurso (extraído do conteúdo MRSS como o canal/título, se fornecido, ou mapeado para o valor do recurso, se ele não estiver no formato RSS). |
| resource-id | O título do recurso real envolvido na solicitação de autorização (extraído da carga MRSS como o item/título, se fornecido) |
| dispositivo | A plataforma do dispositivo (PC, dispositivo móvel, console etc.) |
| eap | O provedor de autenticação externa quando o fluxo de autenticação é executado por meio de um sistema externo. </br> Os valores podem ser: </br> - N/A - a autenticação foi fornecida pela Autenticação Adobe Pass </br> - Apple - o sistema externo que forneceu a autenticação é o Apple |
| os-family | Sistema operacional em execução no dispositivo |
| browser-family | Agente do usuário usado para acessar a autenticação da Adobe Pass |
| cdt | A plataforma do dispositivo (alternativa), usada atualmente para Clientless. </br> Os valores podem ser: </br> - N/A - o evento não foi originado de um SDK sem Cliente </br> - Desconhecido - Como o parâmetro deviceType de uma API sem Cliente é opcional, há chamadas que não contêm nenhum valor. </br> - qualquer outro valor que tenha sido enviado por meio da API sem cliente, por exemplo xbox, appletv, roku etc. </br> |
| platform-version | A versão do SDK sem cliente |
| os-type | Sistema operacional em execução no dispositivo, alternativo (não usado no momento) |
| browser-version | Versão do agente do usuário |
| nsdk | O SDK do cliente usado (android, fireTV, js, iOS, tvOS, não sdk) |
| nsdk-version | A versão do SDK do cliente de autenticação da Adobe Pass |
| evento | O nome do evento de autenticação do Adobe Pass |
| motivo | O motivo das falhas, conforme relatado pela Autenticação Adobe Pass |
| sso-type | O mecanismo SSO subjacente: platform/passive/adobe. Indica que o token de autorização foi emitido ao reutilizar o AuthN em um aplicativo diferente |
| platform | A plataforma identificada pelo dispositivo. Valores possíveis: </br> - Android </br> - FireTV </br> - Roku </br> - iOS </br> - tvOS </br> - etc |
| application-name | O nome do aplicativo configurado, no Painel TVE, para o aplicativo registrado DCR configurado para ser usado. |
| application-version | A versão do aplicativo configurada, no Painel TVE, para que o aplicativo DCR registrado seja usado. |
| customer-app | A ID do aplicativo personalizado foi passada via [Informações do Dispositivo](/help/authentication/passing-client-information-device-connection-and-application.md). |
| content-category | A categoria do conteúdo solicitado pelo aplicativo. |

## ESM para MVPDs {#esm-for-mvpds}

### Os MVPDs podem monitorar as seguintes métricas:

| *Nome da métrica* | *Descrição* |
|---|---|
| tentativas de autenticação | Número de fluxos de autenticação iniciados |
| autn-successful | Número de tokens de autenticação obtidos com êxito pelos clientes |
| autoria pendente | Número de tokens de autenticação gerados com êxito (ignorando se o cliente realmente o obteve) |
| autn-failed | Número de falhas de autenticação executadas por meio de um sistema externo. |
| authz-tries | Número de tentativas de autorização |
| authz-successful | Número de autorizações bem-sucedidas |
| authz-failed | Número de autorizações negadas por MVPDs no nível do aplicativo |
| authz-rejected | Número de tentativas de autorização consideradas mal-intencionadas pelo Provedor de Serviços da Adobe e rejeitadas como resultado de uma prevenção de ataque de DoS |
| authz-latency | Número total de milissegundos gastos no ponto de extremidade do MVPD |

### Os MVPDs podem filtrar as métricas listadas acima pelas seguintes dimensões:

| *Nome do Dimension* | *Descrição* |
|------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ano | O ano de 4 dígitos |
| mês | O mês do ano (1-12) |
| dia | O dia do mês (1-31) |
| hora | A hora do dia |
| minuto | O minuto da hora |
| mvpd | A ID do mvpd usada para executar a solicitação de direito |
| requestor-id | A ID do solicitante usada para executar a solicitação de direito |
| eap | O provedor de autenticação externa quando o fluxo de autenticação é executado por meio de um sistema externo. </br> Os valores podem ser: </br> - N/A - a autenticação foi fornecida pela Autenticação Adobe Pass </br> - Apple - o sistema externo que forneceu a autenticação é o Apple |
| cdt | A plataforma do dispositivo (alternativa), usada atualmente para Clientless. </br> Os valores podem ser: </br> - N/A - o evento não foi originado de um SDK sem Cliente </br> - Desconhecido - Como o parâmetro deviceType de uma API sem Cliente é opcional, há chamadas que não contêm nenhum valor. </br> - qualquer outro valor que tenha sido enviado por meio da API sem cliente, por exemplo xbox, appletv, roku etc. </br> |
| sdk-type | O SDK do cliente usado (Flash, HTML5, nativo do Android, iOS, sem clientes etc.) |
| platform | A plataforma identificada pelo dispositivo. Valores possíveis: </br> - Android </br> - FireTV </br> - Roku </br> - iOS </br> - tvOS </br> - etc |
| nsdk | O SDK do cliente usado (android, fireTV, js, iOS, tvOS, não sdk) |
| nsdk-version | A versão do SDK do cliente de autenticação da Adobe Pass |

## Casos de uso {#use-cases}

Você pode usar os dados ESM para os seguintes casos de uso:

- **Monitoramento** - As equipes de operações ou monitoramento podem criar um painel ou gráfico que chame a API a cada minuto. Usando as informações exibidas, é possível detectar um problema (com a Autenticação do Adobe Pass ou com um MVPD) no minuto em que ele é exibido.

- **Depuração/Teste de Qualidade** - Como os dados também são detalhados por plataforma, dispositivo, navegador e sistema operacional, a análise de padrões de uso pode apontar problemas em combinações específicas (por exemplo, Safari no OSX).

- **Analytics** - Os dados fornecidos podem ser usados para complementar/auditar os dados do cliente coletados pelo Adobe Analytics ou outra ferramenta de análise.

<!--
## Related Information {#related-information}

- [ESM API](/help/authentication/entitlement-service-monitoring-api.md)
- [Degradation API Overview](/help/authentication/degradation-api-overview.md)
- [Server-side Metrics](/help/authentication/understanding-serverside-metrics.md)
-->
