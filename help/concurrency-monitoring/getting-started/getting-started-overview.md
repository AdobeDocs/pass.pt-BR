---
title: Introdução ao monitoramento de simultaneidade
description: Saiba mais sobre as noções básicas de Monitoramento de simultaneidade e como começar a usar a integração
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---


# Introdução ao monitoramento de simultaneidade {#getting-started-overview}

Bem-vindo ao Monitoramento de simultaneidade! Este guia ajudará você a entender os fundamentos e colocar sua integração em funcionamento rapidamente.

## O problema que o monitoramento de simultaneidade resolve {#problem-solved}

No cenário de transmissão atual, os assinantes esperam acessar o conteúdo em vários dispositivos e aplicativos. No entanto, isso cria desafios para os provedores de conteúdo:

- **Os contratos de licenciamento de conteúdo** geralmente limitam o número de fluxos simultâneos
- **Os custos de largura de banda** aumentam com o uso simultâneo excessivo
- **A experiência do usuário** é afetada quando muitos fluxos degradam o desempenho
- **A proteção de receita** requer a prevenção contra abuso de compartilhamento de conta

O monitoramento simultâneo oferece uma solução centralizada para esses desafios.


## Como funciona o monitoramento de simultaneidade

### Funcionalidade principal

O Monitoramento de Simultaneidade opera como um **serviço de gerenciamento de sessão** que:

1. **Rastreia sessões de streaming ativas** em tempo real em todos os seus aplicativos
2. **Avalia as políticas comerciais** em relação aos padrões de uso atuais
3. **Impõe limites** quando as políticas são violadas
4. **Fornece opções de resolução** quando ocorrem conflitos

## Benefícios para as diferentes partes interessadas

### Provedores de conteúdo (programadores)

- **Proteger os direitos de conteúdo** e cumprir os contratos de licenciamento
- **Otimize os custos de largura de banda** evitando o uso excessivo
- **Melhore a experiência do usuário** com comentários claros sobre limites
- **Obter insights de uso** por meio de relatórios detalhados

### Provedores de identidade (MVPDs)

- **Impor contratos de assinante** em todos os parceiros de conteúdo
- **Gerenciamento centralizado de políticas** para vários aplicativos
- **Monitoramento em tempo real** dos padrões de uso
- **Arquitetura escalável** que lida com milhões de sessões

### Usuários finais

- **Compreensão clara** de seus limites de uso
- **Experiência consistente** em todos os aplicativos
- **Opções para resolver conflitos** quando os limites são atingidos
- **Comentários transparentes** sobre por que o acesso é restrito


## Caminho de início rápido {#quick-start-path}

1. **Revise os principais conceitos** - Saiba mais sobre [sessões, políticas e metadados](key-concepts.md)
2. **Escolha sua estratégia** - Revise [estratégias LIFO vs FIFO](../use-cases/lifo-fifo-strategies.md)
3. **Começar a implementar** - Siga a [Referência da API](../api/api-reference-overview.md)

## Pronto para integrar? {#ready-to-integrate}

- [Referência da API](../api/api-reference-overview.md)
- [Casos de uso](../use-cases/cm-use-cases.md)


## Registro de serviço {#service-registration}

Para começar a usar o Monitoramento de Simultaneidade, contate nossa [Equipe de Suporte](mailto:tve-support@adobe.com) com as seguintes informações:

1. **Nome da empresa** e detalhes de contato
2. **Aplicativos** que você deseja integrar ao Monitoramento de Simultaneidade. Para cada aplicativo, forneça:
   1. Nome do aplicativo
   2. Plataforma(s) de aplicativos
3. **Parceiro de integração** (caso esteja assinando o Monitoramento de Simultaneidade na solicitação de outro participante, programador ou MVPD)


## Precisa de ajuda? {#need-help}

- **API Explorer** - APIs de teste interativamente na [Interface do usuário do Swagger](https://streams-stage.adobeprimetime.com/swagger-ui/index.html)
- **Termos e definições principais** - [Glossário](../cm-glossary.md)
- **Como obter ajuda?** - [Procedimentos de suporte](../support/cm-escalation-procedures.md)
- **Suporte** - Contate [tve-support@adobe.com](mailto:tve-support@adobe.com)
