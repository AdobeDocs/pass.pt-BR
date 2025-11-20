---
title: Tratamento de erros de conflito 409
description: Saiba como lidar com erros de conflito 409 quando os limites de uso simultâneo são atingidos
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---


# Tratamento de erros de conflito 409 {#handling-409-errors}

Quando um usuário tenta iniciar um novo fluxo e atinge um limite de uso simultâneo, o Monitoramento de Simultaneidade retorna uma resposta de **409 Conflito**. Entender como lidar com esse erro é fundamental para fornecer uma boa experiência ao usuário.

## O que é um conflito 409? {#what-is-a-409-conflict}

Um conflito 409 ocorre quando:

1. **O usuário tenta iniciar um novo fluxo**
2. **A avaliação da política determina** que a solicitação excederia os limites
3. **O sistema retorna 409** com informações detalhadas sobre conflito
4. **O aplicativo deve decidir** como lidar com o conflito

### Estrutura de resposta 409

```json
{
  "status": "error",
  "error": {
    "code": "POLICY_VIOLATION",
    "message": "Concurrent usage limit exceeded"
  },
  "evaluationResult": {
    "decision": "DENY",
    "associatedAdvice": [
      {
        "id": "advice-1",
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "max_streams",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {
                "deviceId": "device-789",
                "channel": "Channel1",
                "contentType": "live"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## Noções básicas da resposta do {#understanding-the-response}

### Campos-chave

- **`decision`**: Sempre &quot;NEGAR&quot; para conflitos 409
- **`associatedAdvice`**: Matriz de objetos de aviso explicando a violação
- **`conflicts`**: Lista de sessões ativas que estão causando o conflito
- **`terminationCode`**: Código exclusivo para encerrar uma sessão específica

### Tipos de Dicas

#### Dica de violação de regra

```json
{
  "adviceType": "rule-violation",
  "attributes": {
    "rule": "max_streams",
    "threshold": 3,
    "current": 4,
    "conflicts": [...]
  }
}
```

#### Dica de Término Remoto

```json
{
  "adviceType": "remote-termination",
  "attributes": {
    "terminatedBy": "session-456",
    "reason": "New session requested with X-Terminate header"
  }
}
```

## Estratégias de manuseio {#handling-strategies}

- No modo FIFO, o CM permite que a nova sessão seja iniciada encerrando uma existente.
- No modo LIFO, o CM bloqueia a nova sessão e informa ao usuário


## Práticas recomendadas {#best-practices}

### &#x200B;1. Comunicação clara com o utilizador

- **Explicar o limite** - Os usuários devem entender por que estão bloqueados
- **Mostrar opções disponíveis** - O que eles podem fazer para resolver o conflito
- **Fornecer contexto** - Mostrar quais sessões estão ativas

### &#x200B;2. Tratamento de erros adequado

- **Não travar** - Manipular erros 409 normalmente
- **Fornecer alternativas** - Oferecer maneiras de resolver o conflito
- **Salvar estado do usuário** - Não perca a seleção de conteúdo

### &#x200B;3. Considerações sobre a experiência do usuário

- **Resolução rápida** - Facilitar a resolução de conflitos
- **Limpar opções** - Os usuários devem entender suas opções
- **Comportamento consistente** - Sempre tratar conflitos da mesma forma

### &#x200B;4. Considerações técnicas

- **Analisar a resposta com cuidado** - Extrair todas as informações relevantes
- **Tratar casos de borda** - E se nenhum conflito for retornado?
- **Registrar conflitos** - Rastrear violações de política para análise


