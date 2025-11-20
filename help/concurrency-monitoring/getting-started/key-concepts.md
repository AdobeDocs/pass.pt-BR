---
title: Principais conceitos
description: Saiba mais sobre os conceitos fundamentais do Monitoramento de simultaneidade, incluindo sessões, políticas, metadados e muito mais
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---


# Principais conceitos {#key-concepts}

Entender os conceitos principais de Monitoramento de simultaneidade é essencial para uma implementação bem-sucedida. Este guia explica os elementos fundamentais e como eles trabalham juntos.

## Conceitos principais {#core-concepts}

### Sessões

Uma **sessão** representa uma instância de streaming de vídeo ativa. Pense nisso como um &quot;tíquete&quot; que permite ao usuário assistir ao conteúdo.

#### Ciclo de vida da sessão

1. **Criação** - Quando o usuário começa a assistir ao conteúdo
2. **Ativo** - Enquanto o usuário está assistindo (mantido pelas pulsações)
3. **Encerramento** - Quando o usuário para de assistir ou a sessão expira

#### Propriedades da sessão

```json
{
  "sessionId": "unique-session-identifier",
  "idp": "identity-provider",
  "subject": "user-identifier",
  "startTime": "2024-01-15T10:30:00Z",
  "expiresAt": "2024-01-15T10:40:00Z",
  "metadata": {
    "deviceId": "device-123",
    "channel": "Channel1",
    "contentType": "live"
  }
}
```

### Políticas

**Políticas** são regras de negócios que definem limites e restrições para streaming simultâneo. Eles determinam quando os usuários podem iniciar novos fluxos.

#### Componentes da política

- **Regras** - Limites e condições específicos
- **Requisitos de metadados** - Quais dados são necessários
- **Lógica de Avaliação** - Como a política é aplicada

#### Exemplo de política

```json
{
  "name": "basic_stream_limit",
  "description": "Maximum 3 concurrent streams per user",
  "rules": [
    {
      "type": "maxstreams",
      "limit": 3,
      "message": "You have reached your maximum of 3 concurrent streams"
    }
  ]
}
```

### Metadados

**Os metadados** fornecem contexto sobre cada sessão. Inclui informações sobre o dispositivo, conteúdo, usuário e aplicativo.

#### Categorias de metadados

| Categoria | Exemplos | Finalidade |
|-----------------|------------------------------------------|---------------------------------|
| **Dispositivo** | `deviceId`, `deviceType`, `osName` | Identificar e categorizar dispositivos |
| **Conteúdo** | `channel`, `contentType`, `assetId` | Descreva o que está sendo observado |
| **Usuário** | `subject`, `subscriptionTier` | Informações específicas do usuário |
| **Aplicativo** | `applicationName`, `applicationPlatform` | Detalhes específicos do aplicativo |
| **Local** | `country`, `hba` | Informações geográficas e de rede |

#### Metadados obrigatórios vs. opcionais

- **Metadados necessários** - Devem ser fornecidos para a criação da sessão
- **Metadados opcionais** - Podem ser fornecidos para políticas aprimoradas
- **Metadados padrão** - Campos predefinidos disponíveis para todos os aplicativos
- **Metadados personalizados** - Campos específicos do aplicativo que você define

## Identidade e autenticação {#identity-and-authentication}

### Provedor de identidade (IdP)

Um **Provedor de Identidade** é o serviço que autentica usuários e fornece suas informações de identidade. Na integração do Adobe Pass, geralmente é uma MVPD.

#### Exemplos de IdP

- Empresas de cabo (Comcast, Charter etc.)
- Fornecedores de satélites (DirecTV, Prato)
- Serviços de streaming (Netflix, Hulu)
- Operadoras de celular (Verizon, AT&amp;T)

### Assunto

Um **assunto** é o identificador exclusivo de um usuário em um Provedor de Identidade. Normalmente, é a ID da conta do usuário ou a ID do assinante.

## Gerenciamento de sessão {#session-management}

### Heartbeats

**Heartbeats** são chamadas de API periódicas que mantêm as sessões ativas. Eles informam ao sistema &quot;este usuário ainda está assistindo&quot;.

#### Requisitos do Heartbeat

- **Frequência** - A cada 60 segundos (recomendado)
- **Propósito** - Manter sessão ativa, atualizar metadados
- **Encerramento** - A sessão expira se as pulsações param


### Término da sessão

As sessões podem ser encerradas de várias maneiras:

1. **Usuário para de assistir** - O aplicativo chama o ponto de extremidade DELETE
2. **Sessão expira** - Nenhuma pulsação recebida dentro do tempo limite
3. **Violação de política** - A nova sessão encerra a antiga (FIFO)
4. **Encerramento remoto** - Outro dispositivo encerra a sessão

## Avaliação de política {#policy-evaluation}

### Como as políticas funcionam

1. **O usuário solicita um novo fluxo**
2. **O sistema avalia as políticas** em relação ao uso atual
3. **Decisão tomada** - Permitir ou negar
4. **Resposta enviada** - Informações sobre êxito ou conflito

## Resolução de Conflitos {#conflict-resolution}

### O que é um conflito?

Um **conflito** ocorre quando um usuário tenta iniciar um novo fluxo, mas excede seus limites.

### Resposta de Conflito

Quando ocorre um conflito, o sistema retorna uma resposta de Conflito 409 com informações detalhadas:

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
        "adviceType": "rule-violation",
        "attributes": {
          "rule": "max_streams",
          "threshold": 3,
          "current": 4,
          "conflicts": [
            {
              "sessionId": "session-123",
              "terminationCode": "term-456",
              "metadata": {...}
            }
          ]
        }
      }
    ]
  }
}
```

### Estratégias de resolução

#### LIFO (Última Entrada, Primeira Saída)

- **As sessões antigas estão protegidas**
- **Nova sessão bloqueada**
- **Interface do usuário mais simples**
- **Melhor para visualização estendida**


#### FIFO (Primeiro a entrar, primeiro a sair)

- **Nova sessão encerra uma existente**
- **O usuário escolhe a sessão que deve ser interrompida**
- **Interface de usuário mais complexa necessária**
- **Melhor para alternância de conteúdo**

## Aplicativos e inquilinos {#applications-and-tenants}

### Aplicativo

Um **aplicativo** é um programa de software que se integra ao Monitoramento de Simultaneidade. Cada aplicativo tem uma ID exclusiva e pode ter suas próprias políticas.

#### Exemplos de aplicação

- Aplicativo móvel (iOS/Android)
- aplicação web
- Aplicativo Smart TV
- Aplicativo decodificador de sinais

### Inquilino

Um **locatário** é uma organização que possui um ou mais aplicativos. Os locatários podem compartilhar políticas entre aplicativos.

#### Estrutura do inquilino

```
Tenant: "Streaming Company"
├── Application: "Mobile App"
├── Application: "Web App"
└── Application: "TV App"
```

## Ambientes {#environments}

### Ambiente de preparo

- **Propósito** - Desenvolvimento e teste
- **URL** - `https://streams-stage.adobeprimetime.com/v2/`
- **Credenciais** - Testar IDs de aplicativo
- **Dados** - Testar somente dados

### Ambiente de produção

- **Propósito** - Tráfego de usuário ao vivo
- **URL** - `https://streams.adobeprimetime.com/v2/`
- **Credenciais** - IDs do aplicativo de produção
- **Dados** - Dados do usuário real

## Fluxo de integração {#integration-flow}

### Fluxo básico

1. **Autenticação de Usuário** - Autenticar com o Adobe Pass
2. **Criação de sessão** - Criar sessão CM com metadados do usuário
3. **Gerenciamento de Pulsação** - Enviar pulsações periódicas
4. **Encerramento da sessão** - Encerra quando o usuário para de assistir

### Tratamento de erros

1. **404 Não encontrado** - Manipula solicitações com ID de sessão não geradas pelo serviço CM
2. **409 Conflitos** - Manipular violações de política
3. **410 Sumiu** - Manipular o término da sessão
4. **Erros de Rede** - Lidar com problemas de conectividade
5. **Erros de Autenticação** - Lidar com problemas de credencial


## Terminologia comum {#common-terminology}

| Termo | Definição |
|-----------------|--------------------------------------------|
| **Sessão** | Instância de transmissão de vídeo ativa |
| **Política** | Regra de negócios definindo limites de uso |
| **Metadados** | Informações contextuais sobre sessões |
| **IDP** | Provedor de identidade (serviço de autenticação) |
| **Assunto** | Identificador de usuário em um IDP |
| **Heartbeat** | Chamada periódica para manter a sessão ativa |
| **Conflito** | Violação de política ao iniciar novo fluxo |
| **LIFO** | Resolução de conflitos de Última Entrada, Primeira Saída |
| **FIFO** | Resolução de conflitos de primeiro a entrar, primeiro a sair |
| **Locatário** | Organização que possui aplicativos |
| **Aplicativo** | Programa de software usando CM |

