---
title: Glossário da REST API V2
description: Glossário da REST API V2
exl-id: 8b3bd2de-1ff8-4c57-b18d-27ecdf2b0de2
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1742'
ht-degree: 0%

---

# Glossário da REST API V2 {#rest-api-v2-glossary}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

Este documento fornece definições para termos usados ao integrar a API REST V2 de autenticação da Adobe Pass.

>[!MORELIKETHIS]
>
> * [Glossário do Dynamic Client Registration (DCR)](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md)

## Termos do Glossário {#glossary-terms}

### A {#a}

#### Autenticação {#authentication}

A autenticação é um processo que permite que um usuário prove sua identidade a um [Programador](#programmer), para obter acesso a conteúdo protegido ([recurso](#resource)), após validar a assinatura do usuário com o [MVPD](#mvpd).

#### Código de autenticação {#code}

O código de autenticação é um conceito de Autenticação do Adobe Pass que armazena um valor exclusivo gerado quando um usuário inicia o processo de [autenticação](#authentication) e identifica exclusivamente a [sessão de autenticação](#session) de um usuário até que o processo de autenticação seja concluído.

O código de autenticação pode ser usado por um [Aplicativo Primário (Programador)](#primary-application) ou um [Aplicativo Secundário (Programador)](#secondary-application) para concluir o processo de [autenticação](#authentication), recuperar informações sobre a [sessão de autenticação](#session) ou acessar o [perfil](#profile) do usuário.

Sinônimo do termo usado anteriormente era o código de registro.

#### Sessão de autenticação {#session}

A sessão de autenticação é um conceito de Autenticação do Adobe Pass que armazena informações sobre o processo de autenticação do usuário iniciado (ou continuado) a partir de um aplicativo [Programador](#programmer), e é identificado exclusivamente por um [código de autenticação](#code).

A sessão de autenticação também pode indicar o aplicativo [Programador](#programmer) para continuar com o processo [autorização](#authorization) como a próxima fase do fluxo [direito](#entitlement), caso o usuário já esteja autenticado.

#### Autorização {#authorization}

A autorização é um processo que permite que um usuário acesse conteúdo protegido ([recurso](#resource)) de um catálogo do [Programador](#programmer) com base na assinatura do [MVPD](#mvpd) de propriedade, após validar os direitos do usuário com o [MVPD](#mvpd).

### C {#c}

#### Configuração {#configuration}

A configuração é um conceito de Autenticação do Adobe Pass que armazena informações sobre as configurações de integração do [Programador](#programmer) e do [MVPD](#mvpd) e pode ser usado durante o processo de [autenticação](#authentication) quando o usuário solicita que o seu [Provedor de TV](#tv-provider) seja selecionado de uma lista de integrações ativas.

### D {#d}

#### Decisão {#decision}

A decisão é um conceito de Autenticação da Adobe Pass que armazena informações sobre a consulta de processo [MVPD](#mvpd) [autorização](#authorization) ou [pré-autorização](#preauthorization) para permitir ou negar acesso de usuário a um conteúdo protegido por [Programador](#programmer).

#### Degradação {#degradation}

A degradação é um recurso de Autenticação Adobe Pass que permite que um usuário acesse conteúdo protegido mesmo quando seu [MVPD](#mvpd) sofrer uma interrupção de serviço.

Para obter mais informações, consulte a documentação do [Recurso de Degradação](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md).

#### ID do dispositivo {#device-id}

A ID do dispositivo é um identificador exclusivo associado ao dispositivo do usuário e deve ser fornecida pelo aplicativo [Programador](#programmer) em todas as fases do fluxo [direito](#entitlement).

### E {#e}

#### Direito {#entitlement}

O direito é um conceito de Autenticação da Adobe Pass que incorpora os fluxos e recursos disponíveis que ajudam um usuário a passar por diferentes fases para acessar conteúdo protegido, que variam de [autenticação](#authentication), [pré-autorização](#preauthorization), [autorização](#authorization) e finalmente [logout](#logout).

#### Código de erro aprimorado {#enhanced-error-code}

O código de erro aprimorado é um conceito de autenticação da Adobe Pass que fornece informações adicionais sobre o erro que ocorreu durante o processamento de uma solicitação.

Para obter mais informações, consulte a documentação de [Códigos de erro aprimorados](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

### H {#h}

#### HBA {#hba}

A Autenticação Baseada em Casa (HBA) é o processo pelo qual o consumidor recebe automaticamente acesso ao conteúdo do [TV Everywhere (TVE)](#tve) em dispositivos selecionados que estão conectados à sua rede doméstica, que faz parte do local no contrato de assinatura.

### I {#i}

#### Provedor de identidade {#identity-provider}

O provedor de identidade é uma empresa que fornece serviços de identidade para consumidores via cabo, satélite ou serviços baseados na Internet no contexto da [TV em todos os lugares (TVE)](#tve).

Sinônimo de [MVPD](#mvpd) e [Provedor de TV](#tv-provider).

### L {#l}

#### Sair {#logout}

O logout é um processo que permite ao usuário encerrar o [perfil](#profile) autenticado na Autenticação Adobe Pass e atualizar o aplicativo [Programador](#programmer) para refletir o status do usuário.

### M {#m}

#### Token de mídia {#media-token}

O token de mídia é um token gerado pela Autenticação Adobe Pass como resultado de uma [decisão](#decision) de autorização destinada a fornecer acesso a conteúdo protegido.

O token de mídia é passado para o [Programador](#programmer), que o valida para garantir a segurança de acesso desse [recurso](#resource).

Sinônimo do termo anterior usado token de autorização curto.

#### Verificador de token de mídia {#media-token-verifier}

O Verificador de Token de Mídia é uma biblioteca distribuída pela Autenticação Adobe Pass que é responsável por verificar a autenticidade de um [token de mídia](#media-token).

Para obter mais informações, consulte a documentação do [Verificador de Token de Mídia](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier).

#### MVPD {#mvpd}

O distribuidor de programação de vídeo multicanal (MVPD) é uma empresa que fornece serviços de televisão aos consumidores por cabo, satélite ou serviços baseados na Internet.

A MVPD é identificada por um valor único definido durante o processo de integração entre a MVPD e a Adobe.

Sinônimo de [Provedor de TV](#tv-provider) e [Provedor de Identidade](#identity-provider).

### P {#p}

#### Parceiro {#partner}

O parceiro é uma empresa que fornece um serviço ou uma estrutura a um [Programador](#programmer) para habilitar uma experiência de usuário de logon único.

O parceiro é identificado por um valor único (por exemplo, &quot;apple&quot;) definido durante o processo de integração entre o parceiro e a Adobe.

#### Pré-autorização {#preauthorization}

A pré-autorização é um processo que permite que um usuário visualize um subconjunto de [recursos](#resource) de um catálogo [Programador](#programmer) ao qual ele teria direito de acesso, após validar os direitos de usuário com o [MVPD](#mvpd).

Sinônimo de [Comprovação](#preflight).

#### Comprovação {#preflight}

A comprovação é um processo que permite que um usuário visualize um subconjunto de [recursos](#resource) de um catálogo [Programador](#programmer) ao qual ele teria direito de acesso, após validar os direitos de usuário com o [MVPD](#mvpd).

Sinônimo de [Pré-autorização](#preauthorization).

#### Aplicativo primário (programador) {#primary-application}

O aplicativo primário se refere a um aplicativo [Programador](#programmer) que inicia a [autenticação](#authentication), mas que pode não ser capaz de concluir o processo usando um [agente de usuário](#user-agent) para navegar até a página de logon do [MVPD](#mvpd).

#### Perfil {#profile}

O perfil é um conceito de Autenticação do Adobe Pass que armazena informações sobre a data de início e de término da autenticação do usuário, os [metadados do usuário](#user-metadata) junto com outros campos que indicam o método de obtenção da autenticação (por exemplo, &quot;regular&quot;, &quot;degradado&quot;, &quot;temporário&quot;, &quot;logon único&quot; etc.).

Sinônimo do termo anterior usado para token de autenticação.

#### Programador {#programmer}

O Programador é uma empresa que fornece conteúdo aos consumidores por meio de canais (marcas) próprios em várias plataformas.

O Programador agrupa vários canais (marcas) de propriedade como [provedores de serviço](#service-provider) em sua integração com a Autenticação Adobe Pass.

#### MVPD de proxy {#proxy-mvpd}

O proxy MVPD é uma empresa que fornece serviços de identidade para outros MVPDs e se integra diretamente à autenticação da Adobe Pass.

#### MVPD com proxy {#proxied-mvpd}

A MVPD com proxy é uma empresa que não tem integração direta com a Autenticação Adobe Pass, mas é integrada por meio de uma [MVPD proxy](#proxy-mvpd).

#### Identidade da plataforma {#platform-identity}

A identidade da plataforma é uma carga de identificador de plataforma exclusiva gerada por um serviço ou estrutura (biblioteca) associada ao dispositivo do usuário e é fornecida ao [Programador](#programmer) para habilitar uma experiência de usuário de logon único.

Para obter mais informações, consulte a documentação do [Logon único usando fluxos de identidade da plataforma](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

### R {#r}

#### Recurso {#resource}

O recurso é um conteúdo protegido que um usuário está tentando acessar de um catálogo do [Programador](#programmer).

O recurso é identificado por um valor único acordado entre o Programador e os MVPDs.

Para obter mais informações, consulte a documentação de [Recursos Protegidos](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#protected-resources).

### S {#s}

#### SAML {#saml}

A SAML (Security Assertion Markup Language) é um padrão aberto para a troca de dados de autenticação e autorização entre as partes, em particular entre um [provedor de identidade](#identity-provider) e um [provedor de serviços](#sp).

#### Aplicativo secundário (programador) {#secondary-application}

O aplicativo secundário refere-se a um aplicativo [Programador](#programmer) capaz de concluir o processo [autenticação](#authentication) usando um [agente de usuário](#user-agent) para navegar até a página de logon do [MVPD](#mvpd).

O aplicativo secundário pode ser executado no mesmo dispositivo que o aplicativo primário ou em um dispositivo diferente (secundário). Nesse caso, a experiência de logon é chamada de experiência de usuário de &quot;segunda autenticação de tela&quot;.

#### Token de serviço {#service-token}

O token de serviço é um identificador de usuário exclusivo gerado por um serviço ou uma estrutura (biblioteca) associada ao usuário e é fornecido ao [Programador](#programmer) para habilitar uma experiência de usuário de logon único.

Para obter mais informações, consulte a documentação do [Logon único usando fluxos de token de serviço](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md).

#### Provedor de serviços {#service-provider}

O provedor de serviços é uma marca (canal) pertencente a um [Programador](#programmer).

O provedor de serviços é identificado por um valor único definido durante o processo de integração entre o Programador e a Adobe.

Sinônimo do termo usado anteriormente pela ID do solicitante.

#### SLO {#slo}

O SLO (logon único) é um processo que permite que um usuário saia de todos os aplicativos que faziam parte do [SSO (logon único)](#sso).

#### SP {#sp}

O SP (provedor de serviços) se refere à função desempenhada pela Autenticação Adobe Pass em nome de um [Programador](#programmer) em uma integração com uma [MVPD](#mvpd).

#### SSO {#sso}

O SSO (logon único) é um processo que permite ao usuário autenticar uma vez e obter acesso a vários aplicativos do [Programador](#programmer) sem a necessidade de autenticação para cada um deles.

### T {#t}

#### TempPass Básico {#temp-pass-basic}

O TempPass básico é um recurso de Autenticação da Adobe Pass que permite que um usuário acesse conteúdo protegido por um tempo limitado sem a necessidade de autenticação com um [MVPD](#mvpd).

Para obter mais informações, consulte a documentação do [Basic TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#basic-temp-pass).

#### TempPass Promocional {#temp-pass-promotional}

O TempPass promocional é um recurso de Autenticação da Adobe Pass que permite que um usuário acesse conteúdo protegido por um número máximo de recursos e um tempo limitado sem a necessidade de autenticação com uma [MVPD](#mvpd).

Para obter mais informações, consulte a documentação do [TempPass Promocional](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass).

#### TTL {#ttl}

O TTL (time to live) é um valor que indica a quantidade de tempo pelo qual uma entidade subjacente é válida.

O TTL pode ser mencionado para um [token de acesso](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-glossary.md#access-token), um [perfil](#profile), uma autorização [decisão](#decision) ou um [token de mídia](#media-token).

#### TVE {#tve}

A TV Everywhere (TVE) é um nicho do setor que permite que os consumidores acessem seus programas de TV, filmes e outros conteúdos favoritos em vários dispositivos, como smartphones, tablets, notebooks e muito mais.

#### Painel TVE {#tve-dashboard}

O Painel da TV em Qualquer Lugar (TVE) é uma ferramenta de Autenticação do Adobe Pass fornecida aos [Programadores](#programmer) para gerenciar suas configurações e dados.

Para obter mais informações, consulte a documentação do [Guia do Usuário do Painel TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md).

#### Provedor de TV {#tv-provider}

O provedor de TV é uma empresa que fornece serviços de televisão aos consumidores por cabo, satélite ou serviços baseados na Internet.

O provedor de TV é identificado por um valor único definido durante o processo de integração entre o provedor de TV e a Adobe.

Sinônimo de [MVPD](#mvpd) e [Provedor de Identidade](#identity-provider).

### U {#u}

#### Agente do usuário {#user-agent}

O agente do usuário se refere a um navegador ou componente semelhante (específico da plataforma) capaz de navegar na Web e renderizar a página de logon do [MVPD](#mvpd).

#### ID de usuário {#user-id}

A ID do usuário é um identificador exclusivo associado ao usuário e é originária do processo de autenticação do [MVPD](#mvpd).

#### Metadados do usuário {#user-metadata}

Os metadados do usuário se referem a atributos específicos do usuário (por exemplo, códigos postais, classificações dos pais, IDs de usuário etc.) que são mantidos pela [MVPD](#mvpd) e fornecidos pela Autenticação Adobe Pass como parte de um [perfil](#profile).

Para obter mais informações, consulte a documentação dos [Metadados do usuário](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).

### V {#v}

#### VSA {#vsa}

A VSA (Conta de Assinante de Vídeo) é uma estrutura desenvolvida pela Apple fornecida ao [Programador](#programmer) para habilitar uma experiência de usuário de logon único.

Para obter mais informações, consulte a documentação da [Estrutura da Conta de Assinante de Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) e do [Logon único usando fluxos do parceiro](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md).
