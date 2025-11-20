---
title: Introdução ao monitoramento de simultaneidade
description: Introdução ao monitoramento de simultaneidade
exl-id: 725cc64b-6b03-46e3-a038-41e9b1341c6b
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Introdução ao monitoramento de simultaneidade {#intro}

O Monitoramento de simultaneidade é um serviço que permite que provedores de conteúdo e de identidade (MVPDs e Programadores) definam e apliquem limites no streaming de vídeo simultâneo em vários aplicativos, dispositivos e plataformas. Quer você seja um programador que deseja controlar quantos fluxos um assinante pode assistir simultaneamente ou um MVPD que deseja aplicar políticas de uso em seus parceiros de conteúdo, o Monitoramento de simultaneidade fornece as ferramentas necessárias.

## O que é Monitoramento de simultaneidade? {#what-is-cm}

O Monitoramento de simultaneidade é um serviço centralizado que rastreia e gerencia sessões ativas de transmissão de vídeo em tempo real. Ele permite:

- **Limitar fluxos simultâneos por assinante** - Controla quantos fluxos de vídeo simultâneos um usuário pode acessar
- **Impor restrições de dispositivo** - Limitar streaming a tipos ou quantidades de dispositivo específicos
- **Implementar políticas baseadas em localização** - Restringir streaming com base na localização geográfica
- **Criar regras específicas de conteúdo** - Aplicar limites diferentes para conteúdo dinâmico vs. conteúdo do VOD
- **Monitorar padrões de uso** - Obtenha insights sobre como seu conteúdo está sendo consumido

## Como funciona {#how-it-works}

O monitoramento de simultaneidade opera por meio de uma API simples, mas eficiente:

1. **Inicialização da sessão** - Quando um usuário começa a assistir ao conteúdo, seu aplicativo cria uma sessão
2. **Avaliação de Política** - O serviço avalia as políticas definidas em relação ao uso atual
3. **Monitoramento em Tempo Real** - As chamadas de pulsação mantêm as sessões ativas e monitoram a conformidade

## Novo no monitoramento de simultaneidade? {#new-to-cm}

Comece com nosso [Guia de Introdução](getting-started/getting-started-overview.md) para entender as noções básicas e configurar sua primeira integração.

