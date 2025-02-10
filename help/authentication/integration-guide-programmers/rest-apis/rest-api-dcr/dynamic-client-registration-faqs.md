---
title: Perguntas frequentes sobre o Registro de cliente dinâmico (DCR)
description: Perguntas frequentes sobre o Registro de cliente dinâmico (DCR)
exl-id: 12268163-632e-4884-b35d-a29cc8ef45bf
source-git-commit: 747c3d9b6de537be5e7e0a0244b2b301603d9b18
workflow-type: tm+mt
source-wordcount: '1135'
ht-degree: 0%

---

# Perguntas frequentes sobre o Registro de cliente dinâmico (DCR) {#rest-api-dcr-faqs}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Este documento fornece respostas de alta visão geral para perguntas frequentes sobre a adoção do Registro dinâmico de cliente (DCR) de autenticação da Adobe Pass.

Para obter mais informações sobre o registro dinâmico de clientes (DCR) em geral, consulte a [Documentação de Visão geral do registro dinâmico de clientes](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Perguntas frequentes gerais {#general-faqs}

Comece com esta seção se estiver trabalhando em um aplicativo que precisa integrar o Registro de cliente dinâmico (DCR), seja um aplicativo novo ou existente que migra de um dos mecanismos anteriores.

>[!MORELIKETHIS]
>
> * [Perguntas frequentes sobre a REST API v2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#general-faqs)

### Perguntas frequentes sobre acesso à REST API V2 {#rest-api-v2-access-faqs}

+++Perguntas frequentes sobre acesso à API REST V2

#### 1. Qual é o objetivo da Fase de Registro? {#rest-api-v2-access-faq1}

A finalidade da Fase de Registro é registrar o aplicativo cliente na Autenticação Adobe Pass por meio do [processo de Registro Dinâmico de Cliente (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr).

O processo de Registro dinâmico de cliente (DCR) exige que o aplicativo cliente obtenha um par de credenciais de cliente e recupere um token de acesso como meta final da fase de registro.

Para obter mais informações, consulte a documentação [Visão geral do registro dinâmico do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

#### 2. A fase de registro é obrigatória? {#rest-api-v2-access-faq2}

A Fase de Registro é obrigatória, mas o aplicativo cliente poderá ignorar essa fase se tiver um par de credenciais de cliente em cache e um token de acesso que ainda seja válido.

#### 3. O que é uma declaração de software e por quanto tempo ela é válida? {#rest-api-v2-access-faq3}

A instrução de software é um termo definido na documentação do [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#software-statement).

A instrução de software consiste em um JSON Web Token (JWT) que pode ser gerado e baixado do [Painel TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) do Adobe Pass por um dos administradores da organização ou por um representante de Autenticação Adobe Pass que atua em seu nome.

A declaração do software é válida por um período ilimitado, mas você pode solicitar que um representante da Autenticação da Adobe Pass a revogue a qualquer momento.

O aplicativo cliente deve armazenar a instrução de software e usá-la quando precisar recuperar credenciais de cliente.

Para obter mais detalhes, consulte a documentação [Visão geral do registro dinâmico do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

#### 4. Como gerar e baixar uma instrução de software? {#rest-api-v2-access-faq4}

Esta operação pode ser concluída por meio do [Painel do TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da Adobe Pass por um dos administradores da organização ou por um representante de Autenticação da Adobe Pass que atue em seu nome.

Para obter mais detalhes, consulte a documentação do [Guia do Usuário de Canais do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications) ou o [Guia do Usuário de Programadores do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications).

#### 5. O que acontece se uma instrução de software for revogada? {#rest-api-v2-access-faq5}

Quando a declaração de software for revogada, há uma consequência importante a ser considerada:

* Os aplicativos cliente que usam a instrução de software revogada não poderão mais passar pelos fluxos [direitos](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#entitlement), o que significa que os usuários serão impedidos de reproduzir conteúdo.

#### 6. Quais são as credenciais do cliente e por quanto tempo elas são válidas? {#rest-api-v2-access-faq6}

As credenciais do cliente são um termo definido na documentação do [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#client-credentials).

As credenciais do cliente consistem em um identificador do cliente e um par de segredos do cliente que podem ser recuperados do ponto de extremidade do Registro do cliente.

As credenciais do cliente são válidas por um período ilimitado.

O aplicativo cliente deve armazenar as credenciais do cliente e usá-las indefinidamente quando precisar recuperar um token de acesso.

Para obter mais informações, consulte a documentação [Recuperar credenciais do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).

#### 7. Como gerenciar credenciais do cliente? {#rest-api-v2-access-faq7}

Recomendamos que o aplicativo cliente gerencie um par exclusivo de credenciais do cliente para cada instância do aplicativo do usuário no caso de integrações cliente para servidor e servidor para servidor com a Autenticação do Adobe Pass.

#### 8. O aplicativo cliente deve armazenar em cache as credenciais do cliente em um armazenamento persistente? {#rest-api-v2-access-faq8}

O aplicativo cliente deve armazenar as credenciais do cliente e usá-las indefinidamente quando precisar recuperar um token de acesso.

#### 9. O que acontece se as credenciais de cliente em cache forem perdidas? {#rest-api-v2-access-faq9}

Quando as credenciais de cliente em cache são perdidas, há três consequências importantes a serem consideradas:

* O aplicativo cliente deve obter um novo par de credenciais de cliente.
* O aplicativo cliente deve obter um novo token de acesso usando o novo par de credenciais do cliente.
* O aplicativo cliente precisará solicitar ao usuário uma nova autenticação, pois o aplicativo cliente perderá o acesso aos perfis autenticados obtidos anteriormente.

#### 10. O que é um token de acesso e por quanto tempo ele é válido? {#rest-api-v2-access-faq10}

O token de acesso é um termo definido no [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#access-token).

O token de acesso consiste em um [token de portador](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) que pode ser recuperado do ponto de extremidade do Token de Cliente.

O token de acesso é válido por um período limitado e curto especificado no momento da emissão.

O aplicativo cliente deve armazenar o token de acesso e usá-lo até que expire ao direcionar a API REST V2.

O aplicativo cliente deve obter um novo token de acesso antes que o atual expire para evitar solicitações não autorizadas.

Para obter mais informações, consulte a documentação [Recuperar token de acesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

#### 11. O aplicativo cliente deve armazenar o token de acesso em cache em um armazenamento persistente? {#rest-api-v2-access-faq11}

O aplicativo cliente deve armazenar e usar o token de acesso até que ele expire, descartá-lo e obter um novo.

#### 12. Como o aplicativo cliente pode atualizar um token de acesso? {#rest-api-v2-access-faq12}

O aplicativo cliente deve atualizar um token de acesso da mesma forma que recuperar um novo token de acesso, mas usando credenciais de cliente em cache.

O aplicativo cliente não deve se registrar novamente para atualizar um token de acesso. Em vez disso, ele deve usar as credenciais de cliente armazenadas, caso contrário, os usuários precisarão se autenticar novamente.

Para obter mais informações, consulte a documentação [Recuperar token de acesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

+++

## Perguntas frequentes sobre migração {#migration-faqs}

Prossiga com esta seção se estiver trabalhando em um aplicativo que precisa migrar um aplicativo existente para usar o Registro de Cliente Dinâmico (DCR).

>[!MORELIKETHIS]
>
> * [Perguntas frequentes sobre a REST API v2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#migration-faqs)

### Perguntas frequentes sobre a migração para a REST API V2 {#rest-api-v2-migration-faqs}

+++Perguntas frequentes sobre a migração para a REST API V2

#### 1. O aplicativo cliente pode reutilizar os aplicativos registrados existentes (declarações de software)? {#rest-api-v2-migration-faq1}

O aplicativo cliente não pode reutilizar os aplicativos registrados existentes (instruções de software), portanto, deve gerar e baixar novos aplicativos registrados (instruções de software) dedicados ao consumo da REST API V2.

Esta operação pode ser concluída por meio do [Painel do TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da Adobe Pass por um dos administradores da organização ou por um representante de Autenticação da Adobe Pass que atue em seu nome.

Para obter mais detalhes, consulte a documentação do [Guia do Usuário de Canais do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications) ou o [Guia do Usuário de Programadores do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications).

No momento, você deverá solicitar que um representante da Autenticação da Adobe Pass habilite o uso da REST API V2 para seus novos aplicativos registrados (instruções de software), até que o [Painel do TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da Adobe Pass seja atualizado para permitir o autogerenciamento desta operação.

Para distinguir os aplicativos registrados (instruções de software) usados em aplicativos clientes que consomem a REST API V2, exigimos que você adicione um sufixo específico ao nome do aplicativo registrado, como &quot;RESTV2&quot;.

#### 2. O aplicativo cliente pode reutilizar os esquemas personalizados existentes? {#rest-api-v2-migration-faq2}

O aplicativo cliente pode reutilizar os esquemas personalizados existentes gerados pelo [Painel TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) do Adobe Pass.

Para obter mais detalhes, consulte a documentação do [Guia do Usuário de Canais do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#custom-schemes) ou o [Guia do Usuário de Programadores do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#custom-schemes).

+++
