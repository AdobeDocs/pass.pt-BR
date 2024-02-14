---
title: Mecanismo de limitação
description: Mecanismo de limitação
source-git-commit: 1390c09d3de6b4608cdcc97b3f053cce71e84639
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 1%

---


# Mecanismo de limitação {#throttling-mechanism}

## Introdução {#introduction}

O Adobe, na sua função de processador de dados, deve tomar as medidas apropriadas para garantir que os usuários de nossos clientes usem os recursos de forma equitativa e que o serviço não seja inundado por solicitações desnecessárias de API. Para isso, implantamos um mecanismo de controle.\
Um aplicativo de Monitoramento de simultaneidade pode ser usado por vários usuários e um deles pode ter várias sessões. Portanto, o serviço terá limites configurados para o número de chamadas aceitas por usuário/sessão em um intervalo de tempo específico.\
Quando o limite for atingido, as solicitações serão marcadas com um status de resposta específico (HTTP 429 Demasiadas solicitações). Qualquer chamada subsequente feita após o recebimento de uma resposta &quot;429 Muitas solicitações&quot; deve ser feita com um período de resfriamento de pelo menos 1 minuto, para garantir que obtenha uma resposta comercial válida.

## Visão geral do mecanismo {#mechanism-overview}

O mecanismo determina o número máximo de chamadas aceitas para cada endpoint de Monitoramento de simultaneidade em um intervalo de tempo específico.
Quando esse número máximo de chamadas for atingido, nosso serviço responderá com &quot;429 Muitas solicitações&quot;. O serviço precisa de mais 60 segundos para inicializar o limite novamente para seu valor máximo.

Os endpoints configurados com limitação são:
1. Criar uma nova sessão: POST /sessions/{idp}/{subject}
2. Chamada do Heartbeat: POST /sessions/{idp}/{subject}/{sessionId}
3. Encerrar uma sessão: DELETE /sessions/{idp}/{subject}/{sessionId}

A limitação é configurada em dois níveis:
1. session: mesmo exclusivo {sessionId} parâmetro enviado em `Heartbeat` chame e `Terminate a session` chame.
2. user: same unique {subject} parâmetro enviado em `Create a new session` chame.

O limite para limitação de nível de sessão é definido como 200 solicitações em um minuto.\
O limite de limitação no nível do usuário é definido como 200 solicitações em um minuto.\
Ambos os limites (limitação de nível de sessão e limitação de nível de usuário) são configuráveis e nós os atualizaremos caso sejam atingidos por meio de cenários de integração válidos. Para isso, recomendamos que a equipe de suporte seja contatada.

Este é um cenário para limitação de nível de sessão:

| Hora | Solicitação de envio para o CM | Número de solicitações | Resposta recebida do CM | Explicação |
|-----------|-----------------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 10 segundos | POST /sessions/idp1/subject1/session1 | 50 | Todas as chamadas recebem ‘202 aceitas’ | 50 chamadas consumidas do limite |
| Segundo 50 | POST /sessions/idp1/subject1/session1 | 151 | 150 chamadas recebem &quot;202 aceitas&quot; e 1 chamada recebe &quot;429 muitas solicitações&quot; | 200 chamadas consumidas do limite e 1 chamada receberão resposta 429 |
| Segundo 61 | DELETE /sessions/idp1/subject1/session1 | 1 | 1 chamada recebe ‘429 muitas solicitações’ | Nenhuma chamada no limite disponível ainda |
| Segundos 70 | DELETE /sessions/idp1/subject1/session1 | 1 | 1 chamada recebe ‘202 Aceito’ | O limite foi definido como 200 chamadas disponíveis porque 60 segundos se passaram desde os 10 segundos |

e um cenário para limitação no nível do usuário:

| Hora | Solicitação de envio para o CM | Número de solicitações | Resposta recebida do CM | Explicação |
|-----------|------------------------------|--------------------|------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| 10 segundos | POST /sessions/idp1/subject1 | 50 | 50 chamadas recebem ‘202 aceitas’ | 50 chamadas consumidas do limite |
| Segundo 50 | POST /sessions/idp1/subject1 | 151 | 150 chamadas recebem &quot;202 aceitas&quot; e 1 chamada recebe &quot;429 muitas solicitações&quot; | 200 chamadas consumidas do limite e 1 chamada receberão resposta 429 |
| Segundo 61 | POST /sessions/idp1/subject1 | 1 | 1 chamada recebe ‘429 muitas solicitações’ | Nenhuma chamada no limite disponível ainda |
| Segundos 70 | POST /sessions/idp1/subject1 | 1 | 1 chamada recebe ‘202 Aceito’ | O limite foi definido como 200 chamadas disponíveis porque 60 segundos se passaram desde os 10 segundos |


## Recomendações de integração do cliente {#customer-integration-recommendations}

Com uma implementação correta, os clientes não receberão a resposta &quot;429 muitas solicitações&quot;.\
Além disso, a Adobe recomenda que cada cliente manipule a resposta &quot;429 muitas solicitações&quot; apropriadamente, usando os detalhes técnicos apresentados acima.
