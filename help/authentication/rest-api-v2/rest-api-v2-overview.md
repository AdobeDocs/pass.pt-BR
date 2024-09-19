---
title: REST API V2 - Visão geral
description: REST API V2 - Visão geral
source-git-commit: 94fcb4e8c94330561596cd4006738c4f4d75e371
workflow-type: tm+mt
source-wordcount: '494'
ht-degree: 0%

---

# REST API V2 - Visão geral {#rest-api-v2-overview}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Deseja melhorar a relação custo/benefício de seus aplicativos de TVE?

Deseja reduzir o tempo de desenvolvimento e os recursos necessários para suportar aplicativos TVE em várias plataformas?

Deseja garantir uma experiência de usuário consistente em todas as plataformas?

Deseja reduzir o esforço de manutenção e simplificar o fornecimento de atualizações, correções de erros ou melhorias?

## Introdução à REST API V2

A Autenticação Adobe Pass tem o prazer de anunciar o lançamento da REST API V2, projetada para aprimorar a experiência do usuário e simplificar a integração com os serviços Pass.

Estamos entusiasmados com as possibilidades de nossa nova API REST, que representa um grande passo à frente para nossa plataforma e abre as portas para novos recursos e fluxos de aplicativos.

## Novidades?

Implementação exclusiva em todas as plataformas

Os aplicativos dos clientes agora podem usar a mesma implementação em todas as plataformas, facilitando a inicialização de novos recursos ou a manutenção de aplicativos ativos.

### SSO entre dispositivos

A REST API V2 permite que uma sessão de autenticação seja transmitida com segurança entre dispositivos diferentes. Ao simplesmente passar a sessão entre dispositivos, os usuários podem se autenticar em seus dispositivos móveis e transmitir vídeo em um dispositivo conectado à TV sem reautenticação.

### Várias sessões de autenticação ativas

Agora, diferentes sessões de MVPD ativas são possíveis e os clientes podem optar por alternar entre o TempPass e a integração MVPD regular, quando necessário.

### Mecanismo de segurança aprimorado

O Registro dinâmico de cliente é o mecanismo de segurança usado em todos os fluxos e funcionalidades. Ele permite um controle mais seguro e granular dos aplicativos dos clientes e pode registrar aplicativos em todas as plataformas.

### Desempenho aprimorado para tempos de resposta mais rápidos

Mecanismos aprimorados de cache permitem menos tráfego para MVPDs, melhorando os tempos de resposta e reduzindo a latência. Em geral, há uma redução no número de chamadas à API até que o vídeo seja iniciado.

### Códigos de erro aprimorados em todos os fluxos

Os códigos de erro avançados agora estão disponíveis em todos os fluxos Pass no mesmo formato, com detalhes adicionais orientando os aplicativos para melhorar a experiência geral do usuário.

### Controle aprimorado sobre todas as sessões de autenticação

A nova REST API V2 permite ações em várias sessões autenticadas ao mesmo tempo.

### Reduzir os custos de manutenção

Todas as respostas e informações de erro agora estão normalizadas.

## O que vem a seguir?

Todos os clientes que atualmente usam nossas APIs por meio de SDKs ou chamadas REST podem continuar a fazê-lo, pois planejamos continuar fornecendo suporte até o final de 2025.

No entanto, todos os desenvolvimentos futuros serão baseados na REST API V2. Recomendamos iniciar o processo de migração para se beneficiar das funcionalidades mais recentes do Adobe Pass.

## Quer saber mais?

Para começar, visite nossa documentação pública para acessar a [descrição do fluxo](./flows/rest-api-v2-flows-overview.md) e a [referência da API](./apis/rest-api-v2-apis-overview.md).

Nossa equipe de suporte dedicada também está disponível para ajudá-lo com qualquer pergunta ou assistência técnica que você possa precisar.

## Deseja experimentar a REST API V2?

Agora você pode explorar a REST API V2 por meio de nossa página dedicada ao produto do site [Adobe Developer](https://developer.adobe.com/adobe-pass/).