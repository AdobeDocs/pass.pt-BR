---
title: Visão geral da REST API V2
description: Visão geral da REST API V2
exl-id: a5595193-82c4-4033-bd98-596b4908b401
source-git-commit: a02ba4ca1b6579781e40ecd0d12dbfdd23ea7398
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---

# Visão geral da REST API V2 {#rest-api-v2-overview}

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

Deseja melhorar a relação custo/benefício de seus aplicativos de TVE?

Deseja reduzir o tempo de desenvolvimento e os recursos necessários para suportar aplicativos TVE em várias plataformas?

Deseja garantir uma experiência de usuário consistente em todas as plataformas?

Deseja reduzir o esforço de manutenção e simplificar o fornecimento de atualizações, correções de erros ou melhorias?

## Introdução à REST API V2 {#rest-api-v2-introduction}

A Autenticação Adobe Pass tem o prazer de anunciar o lançamento da REST API V2, projetada para aprimorar a experiência do usuário e simplificar a integração com os serviços Pass.

Estamos entusiasmados com as possibilidades de nossa nova API REST, que representa um grande passo à frente para nossa plataforma e abre as portas para novos recursos e fluxos de aplicativos.

## Novidades? {#rest-api-v2-whats-new}

Implementação exclusiva em todas as plataformas

Os aplicativos dos clientes agora podem usar a mesma implementação em todas as plataformas, facilitando a inicialização de novos recursos ou a manutenção de aplicativos ativos.

### SSO entre dispositivos {#rest-api-v2-cross-device-sso}

A REST API V2 permite que uma sessão de autenticação seja transmitida com segurança entre dispositivos diferentes. Ao simplesmente passar a sessão entre dispositivos, os usuários podem se autenticar em seus dispositivos móveis e transmitir vídeo em um dispositivo conectado à TV sem reautenticação.

### Várias sessões de autenticação ativas {#rest-api-v2-multiple-active-authentication-sessions}

Agora, diferentes sessões ativas do MVPD são possíveis, e os clientes podem optar por alternar entre o TempPass e a integração regular do MVPD, quando necessário.

### Mecanismo de segurança aprimorado {#rest-api-v2-enhanced-security-mechanism}

O Registro dinâmico de cliente é o mecanismo de segurança usado em todos os fluxos e funcionalidades. Ele permite um controle mais seguro e granular dos aplicativos dos clientes e pode registrar aplicativos em todas as plataformas.

### Desempenho aprimorado para tempos de resposta mais rápidos {#rest-api-v2-improved-performance}

Mecanismos aprimorados de cache permitem menos tráfego para MVPDs, melhorando os tempos de resposta e reduzindo a latência. Em geral, há uma redução no número de chamadas à API até que o vídeo seja iniciado.

### Códigos de erro aprimorados em todos os fluxos {#rest-api-v2-enhanced-error-codes}

Os códigos de erro avançados agora estão disponíveis em todos os fluxos Pass no mesmo formato, com detalhes adicionais orientando os aplicativos para melhorar a experiência geral do usuário.

### Controle aprimorado sobre todas as sessões de autenticação {#rest-api-v2-improved-control}

A nova REST API V2 permite ações em várias sessões autenticadas ao mesmo tempo.

### Reduzir os custos de manutenção {#rest-api-v2-reduce-maintenance-costs}

Todas as respostas e informações de erro agora estão normalizadas.

## O que vem a seguir? {#rest-api-v2-whats-next}

Todos os clientes que atualmente usam nossas APIs por meio de SDKs ou chamadas REST podem continuar a fazê-lo, pois planejamos continuar fornecendo suporte até o final de 2025.

No entanto, todos os desenvolvimentos futuros serão baseados na REST API V2. Recomendamos iniciar o processo de migração para se beneficiar das funcionalidades mais recentes do Adobe Pass.

## Quer saber mais? {#rest-api-v2-want-to-learn-more}

Para começar, visite nossa documentação pública:

- [Webinário](#rest-api-v2-webinar)
- [Glossário](rest-api-v2-glossary.md)
- [Lista de verificação](rest-api-v2-checklist.md)
- [Perguntas frequentes](rest-api-v2-faqs.md)
- [APIs](apis/rest-api-v2-apis-overview.md)
- [Fluxos](flows/rest-api-v2-flows-overview.md)
- Cookbooks
- Apêndice
- [Requisitos mínimos do sistema](/help/authentication/integration-guide-programmers/minimum-system-requirements.md)

Nossa equipe de suporte dedicada também está disponível para ajudá-lo com qualquer pergunta ou assistência técnica que você possa precisar.

### Webinário {#rest-api-v2-webinar}

Assista ao webinário sobre a nova REST API V2, onde fornecemos uma visão geral dos novos recursos e funcionalidades, bem como uma demonstração em tempo real da REST API V2 em ação.

>[!VIDEO](https://video.tv.adobe.com/v/3457461/?quality=12&learn=on)

## Deseja experimentar a REST API V2? {#rest-api-v2-want-to-try}

Agora você pode explorar a REST API V2 por meio de nossa página dedicada ao produto do site [Adobe Developer](https://developer.adobe.com/adobe-pass/).
