---
title: Perguntas frequentes sobre o Registro de cliente dinâmico (DCR)
description: Perguntas frequentes sobre o Registro de cliente dinâmico (DCR)
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---

# Perguntas frequentes sobre o Registro de cliente dinâmico (DCR) {#rest-api-dcr-faqs}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Este documento fornece respostas de alta visão geral para perguntas frequentes sobre a adoção do Registro dinâmico de cliente (DCR) de autenticação da Adobe Pass.

Para obter mais informações sobre o registro dinâmico de clientes (DCR) em geral, consulte a [Documentação de Visão geral do registro dinâmico de clientes](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

>[!MORELIKETHIS]
>
> * [Perguntas frequentes sobre a REST API v2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md)

## Perguntas frequentes gerais {#general-faqs}

Comece com esta seção se estiver trabalhando em um aplicativo que precisa integrar o Registro de cliente dinâmico (DCR), seja um aplicativo novo ou existente que migra de um dos mecanismos anteriores.

### Perguntas frequentes sobre a fase de registro {#registration-phase-faqs-general}

+++Perguntas frequentes sobre a fase de registro

#### 1. Qual é o objetivo da Fase de Registro? {#registration-phase-faq1}

A finalidade da Fase de Registro é registrar o aplicativo cliente na Autenticação Adobe Pass por meio do [processo de Registro Dinâmico de Cliente (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#dcr).

O processo de Registro dinâmico de cliente (DCR) exige que o aplicativo cliente obtenha um par de credenciais de cliente e recupere um token de acesso como meta final da fase de registro.

Para obter mais informações, consulte a documentação [Visão geral do registro dinâmico do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

#### 2. A fase de registro é obrigatória? {#registration-phase-faq2}

A Fase de Registro é obrigatória, mas o aplicativo cliente poderá ignorar essa fase se tiver um par de credenciais de cliente em cache e um token de acesso que ainda seja válido.

#### 3. O que é uma declaração de software e por quanto tempo ela é válida? {#registration-phase-faq3}

A instrução de software é um termo definido na documentação do [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#software-statement).

A instrução de software consiste em um JSON Web Token (JWT) que pode ser gerado e baixado do [Painel TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) do Adobe Pass por um dos administradores da organização ou por um representante de Autenticação Adobe Pass que atua em seu nome.

A declaração do software é válida por um período ilimitado, mas você pode solicitar que um representante da Autenticação da Adobe Pass a revogue a qualquer momento.

O aplicativo cliente deve armazenar a instrução de software e usá-la quando precisar recuperar credenciais de cliente.

Para obter mais detalhes, consulte a documentação [Visão geral do registro dinâmico do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

#### 4. Como gerar e baixar uma instrução de software? {#registration-phase-faq4}

Esta operação pode ser concluída por meio do [Painel do TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da Adobe Pass por um dos administradores da organização ou por um representante de Autenticação da Adobe Pass que atue em seu nome.

Para obter mais detalhes, consulte a documentação do [Guia do Usuário de Canais do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-channels.md#registered-applications) ou o [Guia do Usuário de Programadores do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-programmers.md#registered-applications).

#### 5. O que acontece se uma instrução de software for revogada? {#registration-phase-faq5}

Quando a declaração de software for revogada, há uma consequência importante a ser considerada:

* Os aplicativos cliente que usam a instrução de software revogada não poderão mais passar pelos fluxos [direitos](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#entitlement), o que significa que os usuários serão impedidos de reproduzir conteúdo.

#### 6. Quais são as credenciais do cliente e por quanto tempo elas são válidas? {#registration-phase-faq6}

As credenciais do cliente são um termo definido na documentação do [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#client-credentials).

As credenciais do cliente consistem em um identificador do cliente e um par de segredos do cliente que podem ser recuperados do ponto de extremidade do Registro do cliente.

As credenciais do cliente são válidas por um período ilimitado.

O aplicativo cliente deve armazenar indefinidamente as credenciais do cliente e usá-las quando precisar recuperar um token de acesso.

Para obter mais informações, consulte a documentação [Recuperar credenciais do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).

#### 7. Como gerenciar credenciais do cliente? {#registration-phase-faq7}

Recomendamos que o aplicativo cliente gerencie um par exclusivo de credenciais do cliente para cada instância do aplicativo do usuário no caso de integrações cliente para servidor e servidor para servidor com a Autenticação do Adobe Pass.

O aplicativo cliente deve armazenar indefinidamente as credenciais do cliente e usá-las quando precisar recuperar um token de acesso.

#### 8. O que acontece se as credenciais de cliente em cache forem perdidas? {#registration-phase-faq8}

Quando as credenciais de cliente em cache são perdidas, há três consequências importantes a serem consideradas:

* O aplicativo cliente deve obter um novo par de credenciais de cliente.
* O aplicativo cliente deve obter um novo token de acesso usando o novo par de credenciais do cliente.
* O aplicativo cliente precisará solicitar ao usuário uma nova autenticação, pois o aplicativo cliente perderá o acesso aos perfis autenticados obtidos anteriormente.

#### 9. O que é um token de acesso e por quanto tempo ele é válido? {#registration-phase-faq9}

O token de acesso é um termo definido no [Glossário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#access-token).

O token de acesso consiste em um [token de portador](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md) que pode ser recuperado do ponto de extremidade do Token de Cliente.

O token de acesso é válido por um período limitado e curto especificado no momento da emissão.

O aplicativo cliente deve armazenar o token de acesso e usá-lo até que expire ao direcionar a API REST V2.

O aplicativo cliente deve obter um novo token de acesso antes que o atual expire para evitar solicitações não autorizadas.

Para obter mais informações, consulte a documentação [Recuperar token de acesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).

#### 10. Como o aplicativo cliente pode atualizar um token de acesso? {#registration-phase-faq10}

O aplicativo cliente deve atualizar um token de acesso da mesma forma que recuperar um novo token de acesso, mas usando credenciais de cliente em cache.

O aplicativo cliente não deve se registrar novamente para atualizar um token de acesso. Em vez disso, ele deve usar as credenciais de cliente armazenadas, caso contrário, os usuários precisarão se autenticar novamente.

Para obter mais informações, consulte a documentação [Recuperar token de acesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
