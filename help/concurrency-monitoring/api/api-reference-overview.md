---
title: Visão geral da referência da API
description: Referência completa da API de monitoramento de simultaneidade, incluindo endpoints, autenticação e formatos de resposta
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 2%

---


# Visão geral da referência da API {#api-reference-overview}

A API de monitoramento de simultaneidade fornece uma interface RESTful para gerenciar sessões de transmissão e aplicar políticas de uso simultâneas. Esta referência fornece documentação completa para todos os endpoints, métodos de autenticação, formatos de solicitação/resposta e manipulação de erros.

## URLs de base da API

### Ambiente de produção

```
https://streams.adobeprimetime.com/v2/
```

### Ambiente de preparo

```
https://streams-stage.adobeprimetime.com/v2/
```

**Observação:** sempre use o ambiente de preparo para desenvolvimento e teste. As credenciais de produção são fornecidas somente após a integração de preparo bem-sucedida.

## Autenticação

Todas as chamadas de API exigem Autenticação Básica HTTP usando suas credenciais de aplicativo:

- **Nome de usuário:** a ID do seu aplicativo (fornecida pela Adobe)
- **Senha:** Cadeia de caracteres vazia

### Exemplo de cabeçalho de autenticação

```bash
curl -u "<your-app-id>:" https://streams-stage.adobeprimetime.com/v2/sessions

For an application with id "demo-app" the authentication header would be exactly as shown below, including the quotes and colon:
curl -u "demo-app:" https://streams-stage.adobeprimetime.com/v2/sessions
```

## Padrões de Formato de Resposta

### Respostas de sucesso

Todas as respostas bem-sucedidas seguem esta estrutura:

```json
{
  "status": "success",
  "data": {
    // Response-specific data
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### Respostas de erro

Todas as respostas de erro seguem esta estrutura:

```json
{
  "associatedAdvice": [
    {
      "policyName": "string",
      "ruleName": "string",
      "scope": {},
      "attribute": "string",
      "threshold": 0,
      "conflicts": [
        {}
      ]
    }
  ],
  "obligations": [
    {
      "namespace": "string",
      "action": "string",
      "arguments": [
        "string"
      ]
    }
  ]
}
```

### Formato do resultado da avaliação

Quando as políticas são avaliadas (especialmente para conflitos 409), as respostas incluem um resultado de avaliação:

```json
{
  "evaluationResult": {
    "decision": "DENY",
    "obligations": [
      {
        "id": "obligation-id",
        "fulfillOn": "DENY",
        "attributes": {
          "attribute1": "value1"
        }
      }
    ],
    "associatedAdvice": [
      {
        "id": "advice-id",
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "rule-name",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {
                "deviceId": "device-789",
                "channel": "Channel1"
              }
            }
          ]
        }
      }
    ]
  }
}
```

## Códigos de status HTTP comuns

| Código | Descrição | Quando Retornado |
|------|----------------------|------------------------------------------------|
| 200 | OK | Solicitações do GET com sucesso |
| 202 | Criado/aceito | Criação de sessão/heartbeat registrados com sucesso |
| 400 | Solicitação inválida | Parâmetros inválidos ou campos obrigatórios ausentes |
| 401 | Não autorizado | Autenticação inválida ou ausente |
| 403 | Proibido | Permissões insuficientes |
| 404 | ID de sessão não encontrada | ID da sessão não gerada pelo serviço CM |
| 409 | Conflito | Violação de política (limite simultâneo atingido) |
| 410 | Sumiu | Sessão expirada ou encerrada |
| 429 | Muitas solicitações | Limite de taxa excedido |

## Métodos de envio de parâmetros

### Parâmetros de caminho

Parâmetros obrigatórios que fazem parte do caminho do URL:

1. `{idp}` - Identificador do provedor de identidade
2. `{subject}` - Identificador de usuário (normalmente da Adobe Pass)
3. `{sessionId}` - Identificador de sessão (retornado no cabeçalho Localização)

### Parâmetros adicionais

Parâmetros opcionais são passados no URL:

```bash
GET /sessions/{idp}/{subject}?platform=test
```

### Dados de formulário (POST/PUT)

Metadados e dados de sessão no corpo da solicitação:

```bash
POST /sessions/{idp}/{subject}
Content-Type: application/x-www-form-urlencoded

channel=Channel1&deviceId=device-123&contentType=live
```

### Cabeçalhos

Parâmetros especiais passados em cabeçalhos HTTP:

```bash
X-Terminate: termination-code-123
X-Client-Version: 1.0.0
```

## Práticas recomendadas de tratamento de erros

### 409 Tratamento de conflitos

Quando você receber uma resposta de Conflito 409:

1. **Analise o resultado da avaliação** para entender a violação da política
2. **Extrair informações de conflito** de `associatedAdvice`
3. **Apresentar opções ao usuário** com base em sua estratégia LIFO/FIFO
4. **Usar códigos de encerramento** ao implementar o comportamento LIFO

### 410 Manuseio perdido

Ao receber uma resposta 410 Gone:

1. **Verifique se a resposta tem um corpo** - indica encerramento remoto
2. **Analise o conselho** para entender por que a sessão foi encerrada
3. **Atualizar interface** para refletir o término da sessão
4. **Identificador adequado** - a sessão pode ter expirado naturalmente
5. **Iniciar uma nova sessão** - se for considerado apropriado, iniciar uma nova sessão

### Limite de taxa

Ao receber 429 muitas solicitações:

1. **Limitar a frequência de chamada** para até 200 solicitações por minuto, que é o nível máximo aceito pelo CM
2. **Enviar pulsações no intervalo necessário**, que é um por minuto.

## Ferramentas de teste

### API Explorer interativo

Use nossa [Interface do usuário do Swagger](https://streams-stage.adobeprimetime.com/swagger-ui/index.html) para testes interativos:

1. Insira a ID do aplicativo no canto superior direito
2. Clique em &quot;Explorar&quot; para definir a autenticação
3. Testar endpoints com parâmetros reais
4. Exibir exemplos de solicitação/resposta
