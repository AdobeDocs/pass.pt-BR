---
title: LIFO versus estratégias FIFO
description: Entenda a diferença entre as estratégias LIFO e FIFO e quando usar cada abordagem
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 0%

---


# LIFO versus estratégias FIFO {#lifo-fifo-strategies}

Ao implementar o Monitoramento de simultaneidade, você precisa escolher entre duas estratégias fundamentais para tratar de conflitos quando os limites de uso são atingidos: **LIFO (Última Entrada, Primeira Saída)** ou **FIFO (Primeira Entrada, Primeira Saída)**. Entender essas estratégias é fundamental para projetar a experiência do usuário correta e implementar a manipulação de erros apropriada.

## Estratégias de sessão de monitoramento de simultaneidade {#concurrency-monitoring-session-strategies}

LIFO e FIFO são baseados na **teoria da pilha** da ciência da computação:

### LIFO (Última entrada, primeira saída) - Comportamento da pilha

Em Monitoramento de simultaneidade:
- **As sessões mais antigas estão protegidas das mais recentes**
- **Novas sessões serão bloqueadas quando os limites forem atingidos**
- **Os usuários devem encerrar manualmente as sessões existentes para iniciar novas**


### FIFO (Primeira entrada, primeira saída) - Comportamento da fila

Em Monitoramento de simultaneidade:
- **Novas sessões podem encerrar sessões mais antigas** quando os limites são atingidos
- **O fluxo mais recente pode &quot;expulsar&quot; um fluxo mais antigo**
- **Os usuários podem iniciar novos conteúdos substituindo o que estavam assistindo**

## Estratégia LIFO {#lifo-strategy}

### Como o LIFO funciona

No modo LIFO, quando um usuário tenta iniciar um novo fluxo e atinge o limite simultâneo:

1. **Nova sessão bloqueada** com uma resposta de Conflito 409
2. **As sessões existentes permanecem inalteradas**
3. **O usuário deve encerrar manualmente** uma sessão existente para continuar

### Diagrama de Fluxo LIFO

![Diagrama de Fluxo LIFO](../assets/lifo-flow-diagram.png)

*Figura: Fluxo de estratégia LIFO (Última Entrada, Primeira Saída) - Novas sessões são bloqueadas quando os limites são atingidos, exigindo o encerramento manual de sessões existentes.*

### Quando usar LIFO

**Usar LIFO quando:**
- **Os usuários esperam que seu conteúdo atual seja protegido** de interrupções
- **Você deseja incentivar decisões conscientes** sobre a alternância de conteúdo
- **O aplicativo tem complexidade de interface limitada** para resolução de conflitos
- **Os usuários normalmente assistem ao conteúdo por períodos prolongados**

**Exemplos:**
- Serviços de streaming de filmes nos quais os usuários assistem a conteúdo completo
- Plataformas de conteúdo educacional em que as interrupções causam interrupções
- Aplicativos com interface simples que não podem lidar com seleção complexa de sessão

## Estratégia FIFO {#fifo-strategy}

### Como a FIFO funciona

No modo FIFO, quando um usuário tenta iniciar um novo fluxo e atinge o limite simultâneo:

1. **Nova sessão permitida** para iniciar
2. **A sessão mais antiga é encerrada automaticamente** (ou o usuário escolhe qual encerrar)
3. **O usuário continua com o novo conteúdo**

### Diagrama de Fluxo FIFO

![Diagrama de Fluxo FIFO](../assets/fifo-flow-diagram.png)

*Figura: Fluxo de estratégia FIFO (primeiro a entrar, primeiro a sair) - Novas sessões podem começar encerrando sessões existentes com a seleção do usuário.*

### Quando usar FIFO

**Usar FIFO quando:**
- **Os usuários alternam frequentemente entre conteúdos** (navegação, navegação)
- **Você deseja priorizar a intenção atual do usuário** em relação à atividade anterior
- **A interface do usuário pode manipular a seleção de sessão** quando ocorrem conflitos
- **Os usuários esperam poder iniciar novo conteúdo** mesmo quando os limites são atingidos

**Exemplos:**
- Aplicativos de TV ao vivo, onde os usuários trocam de canal com frequência
- Aplicativos de descoberta de conteúdo nos quais os usuários navegam e visualizam conteúdo
- Aplicativos móveis em que os usuários esperam resposta imediata

### Experiência do usuário FIFO

Quando ocorrer um conflito no modo FIFO:

1. **Mostrar uma caixa de diálogo** com todas as sessões ativas
2. **Permitir que o usuário selecione** qual sessão encerrar
3. **Fornecer detalhes da sessão** (dispositivo, conteúdo, duração)
4. **Confirme a ação** antes de continuar
5. **Iniciar a nova sessão** após o término

## Resumo das principais diferenças {#key-differences-summary}

| Aspecto | FIFO | LIFO |
|-------------------------------|-----------------------------------------|-------------------------------|
| **Resolução de Conflitos** | Encerramento automático da sessão mais antiga | Encerramento manual necessário |
| **Tratamento de erros** | Não é necessário lidar com respostas 409 | Deve lidar com respostas 409 |
| **Experiência do usuário** | Interface de usuário mais complexa, mas com fluxo mais suave | Interface mais simples, mas com mais atrito |
| **Complexidade de implementação** | Mais alto (interface de seleção de sessão) | Inferior (mensagens de erro simples) |
| **Caso de uso** | Troca de conteúdo, descoberta | Visualização e proteção ampliadas |


## Práticas recomendadas {#best-practices}

### Para implementações LIFO

1. **Mostrar mensagens de erro claras** explicando o limite
2. **Fornecer acesso fácil** ao gerenciamento de sessão
3. **Exibir sessões ativas** para referência do usuário
4. **Implementar o término da sessão** nas configurações do seu aplicativo
5. **Considere mostrar os indicadores de uso** antes que ocorram conflitos

### Para implementações FIFO

1. **Sempre fornecer interface de seleção de sessão** quando ocorrerem conflitos
2. **Mostrar detalhes significativos da sessão** (dispositivo, conteúdo, duração)
3. **Implementar caixas de diálogo de confirmação** para evitar terminações acidentais
4. **Tratar casos de borda** em que o encerramento falha
5. **Forneça comentários claros** sobre o que está acontecendo


## Escolhendo sua estratégia {#choosing-your-strategy}

Considere estes fatores ao escolher entre LIFO e FIFO:

1. **Padrões de comportamento do usuário** - Como os usuários normalmente interagem com o seu conteúdo?
2. **Tipo de conteúdo** - TV ao vivo vs. filmes vs. conteúdo educacional
3. **Complexidade da interface do usuário** - O aplicativo pode lidar com a seleção sofisticada de sessões?
4. **Expectativas do usuário** - Os usuários esperam poder alternar o conteúdo facilmente?
5. **Requisitos comerciais** - Você precisa proteger certos tipos de exibição?
