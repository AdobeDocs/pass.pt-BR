---
title: Sobre a autenticação do Adobe Pass
description: Sobre a autenticação do Adobe Pass
exl-id: 5edeaccb-f9fa-4395-83b4-706c518d5a03
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '1832'
ht-degree: 0%

---

# Sobre o Adobe® Pass Authentication {#about-adobe-pass-authentication}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

## Sobre a TV em todos os lugares {#about-tv-everywhere}

Os telespectadores de hoje esperam acesso contínuo ao conteúdo de TV por assinatura a qualquer hora, em qualquer lugar. Com o aumento do número de dispositivos conectados à Internet, os públicos-alvo estão consumindo conteúdo em uma variedade cada vez maior de plataformas, incluindo:

* Notebooks
* Comprimidos
* Smartphones
* Sites
* Aplicativos Federados
* Consoles de jogos
* Decodificadores
* TVs inteligentes

TV Everywhere é uma iniciativa do setor que garante que os assinantes de TV por assinatura possam acessar o conteúdo pelo qual já pagam em vários dispositivos, dentro e fora de suas casas.

Embora a TV tradicional (linear) permaneça forte, os segmentos de crescimento mais rápido do consumo de vídeo são o conteúdo deslocado no tempo, o streaming on-line e as telas alternativas. Essa mudança interrompeu o mercado de distribuição de vídeos, tornando a TV Everywhere uma solução crucial para os interesses de **programadores, provedores de TV por assinatura e assinantes.**

### Metas da TV em todos os lugares {#goals-tv-everywhere}

**Meta técnica**

* Permita que os clientes de TV por assinatura acessem seu conteúdo assinado perfeitamente em todos os dispositivos e plataformas.

**Metas comerciais**

* Preservar e fortalecer os relacionamentos existentes com os clientes, além de permitir novas oportunidades.
* Capacite os programadores e proprietários de conteúdo a alcançar públicos mais amplos e maximizar o valor do conteúdo premium.
* Estender marcas por meio do envolvimento online direto com os visualizadores.

### Desafios da TV em todos os lugares {#challenges-tv-everywhere}

Com as oportunidades da TV Everywhere surgem desafios significativos, sendo o direito o mais crítico. Para que um visualizador possa acessar o conteúdo de assinatura, um sistema deve verificar seus direitos.

As principais perguntas incluem:

* O usuário tem uma assinatura ativa com um provedor de TV por assinatura?
* A assinatura inclui o conteúdo solicitado?

Determinar direitos é particularmente desafiador para programadores e proprietários de conteúdo, pois os operadores de TV por assinatura controlam os dados do cliente e os privilégios de acesso.

Além dos direitos, surgem vários desafios técnicos e de integração, incluindo:

* Desenvolver uma estratégia de vários dispositivos que garanta acesso ininterrupto.
* Gerenciamento de relações complexas entre programadores e provedores de TV por assinatura.
* Prevenção de acesso fraudulento e aplicação de termos de serviço.
* Fornecendo uma experiência de autenticação consistente e fácil de usar em sites e aplicativos.
* Manutenção de um time-to-market rápido para alinhar-se aos contratos de afiliados.
* Controlar custos associados a várias integrações.

Esses desafios tornam as integrações diretas entre os programadores e vários sistemas de autenticação de TV por assinatura altamente exigentes em recursos, exigindo tempo e conhecimento técnico.

Para superar esses obstáculos, a **Adobe® Pass Authentication** simplifica e simplifica a verificação de direitos, permitindo o acesso seguro e ininterrupto ao conteúdo da TV Everywhere.

## Introdução à autenticação da Adobe Pass {#introduction-adobe-pass-authentication}

A autenticação do Adobe Pass realiza de maneira segura as transações de direitos entre os programadores e os provedores de TV por assinatura, garantindo que os clientes certos possam acessar o conteúdo certo sem esforço.

![](../assets/programmers-connect-authn.png)

*Alguns dos programadores e provedores de TV por Assinatura que se conectam por meio da Autenticação Adobe Pass*

**Quem se beneficia da Autenticação Adobe Pass**

* **Programadores**

  Que desejam integrar facilmente com provedores de TV por assinatura, também conhecidos como MVPDs (Multichannel Video Programming Distributors), para alcançar o maior público e otimizar a receita.

* **Provedores de TV por Assinatura (MVPDs)**

  Que buscam se conectar com vários proprietários de conteúdo (programadores) por meio de uma única integração, melhorando a satisfação do cliente ao facilitar o acesso ao conteúdo de assinatura online.

* **Clientes de TV por assinatura**

  Quem quiser ver o conteúdo pelo qual já paga a qualquer hora, em qualquer lugar, em qualquer dispositivo.

**Programadores**

Os programadores que buscam maximizar o alcance do público-alvo e a receita podem:

* Integre-se facilmente com os principais provedores de TV por assinatura sem gerenciar várias conexões diretas.
* Expanda o público garantindo a autenticação em todos os principais provedores e plataformas.
* Proteja conteúdo premium com autenticação segura, restringindo o acesso a usuários e dispositivos autorizados.
* Habilite o Logon Único (SSO) para melhorar a experiência do usuário em aplicativos e sites.

**Provedores de TV por Assinatura (MVPDs)**

Os provedores de TV por assinatura podem aprimorar a experiência do cliente e simplificar as operações ao:

* Conectar-se com vários proprietários de conteúdo por meio de uma única integração.
* Oferece uma experiência de visualização perfeita e de marca em todos os dispositivos e plataformas.
* Garantia de autenticação segura para impedir o acesso não autorizado e gerenciar fluxos simultâneos por residência.

**Clientes de TV por assinatura**

Os assinantes se beneficiam da TV Everywhere, permitindo que assistam ao conteúdo pelo qual já pagam a qualquer hora, em qualquer lugar, em qualquer dispositivo.

### Integração com a autenticação da Adobe Pass {#integrating-adobe-pass-authentication}

Independentemente de você ser um provedor de TV por assinatura ou um programador, a integração com a Autenticação do Adobe Pass requer participação ativa. Abaixo está uma alta visão geral do processo para ambas as funções.

#### Processo de integração do programador {#programmer-integration-process}

Orientações adicionais estão disponíveis assim que a integração é iniciada formalmente, mas o processo normalmente envolve as seguintes etapas:

**Pré-requisitos**

* Um sistema de gerenciamento de conteúdo (CMS).
* Um mecanismo de entrega de conteúdo, que pode incluir uma rede de entrega de conteúdo de terceiros (CDN).
* Uma plataforma de vídeo online existente com um reprodutor de mídia incorporado em um site ou aplicativo independente.

**Tarefas de integração**

* Integre o DCR [ da API REST ](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)de Autenticação da Adobe Pass.
* Integre a Autenticação Adobe Pass [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md).
* Integre o [Verificador de token de mídia](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) da Autenticação do Adobe Pass.
* Desenvolva uma interface de usuário para o fluxo de trabalho de autenticação, autorização e logout.

Para obter mais detalhes sobre o processo de integração do Programador, consulte os documentos [Guia de início rápido do Programador](/help/authentication/kickstart/programmer-kickstart-guide.md) e [Guia de integração do Programador](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md).

#### Processo de integração do provedor de TV por assinatura {#pay-tv-provider-integration-process}

Orientações adicionais estão disponíveis assim que a integração é iniciada formalmente, mas o processo normalmente envolve as seguintes etapas:

**Pré-requisitos**

* Assine o Contrato de não divulgação de autenticação (NDA) da Adobe Pass.
* Fornecer à equipe de engenharia de autenticação da Adobe Pass especificações para o sistema de autenticação e autorização. Um provedor de identidade baseado em SAML (IdP) para autenticação e um sistema de autorização baseado em SOAP são recomendados para simplificar a integração.

**Tarefas de integração**

* Estabeleça conectividade com os servidores de autenticação da Adobe Pass.
* Conclua a versão de preparo e garanta a garantia da qualidade.
* Conclua a versão de produção e garanta a garantia da qualidade.

A integração padrão é gratuita para provedores de TV por assinatura, incluindo documentação e suporte básico por email. No entanto, os provedores que exigem assistência abrangente ou cronogramas acelerados podem incorrer em uma taxa de suporte ou optar por trabalhar com um parceiro de terceiros experiente, como a Synacor.

A Autenticação Adobe Pass pode oferecer suporte eficiente à lógica de negócios específica do provedor de TV por assinatura de duas maneiras principais:

* Para uma lógica de negócios que é independente e aplicada pelo provedor de TV por assinatura quando uma solicitação de autorização é recebida, o Adobe fornece os dados necessários (por exemplo, ID de dispositivo exclusiva, endereço IP) para apoiar a imposição.
* Para uma lógica de negócios que exija a intervenção do usuário ou um tratamento específico por parte da Adobe, as propriedades personalizadas podem ser mantidas para cada provedor de TV por assinatura. Essas configurações podem incluir fluxos de trabalho predefinidos acionados em pontos específicos do processo de autenticação.

Para obter mais detalhes sobre o processo de integração do provedor de TV por assinatura, consulte os documentos [guia de início rápido do MVPD](/help/authentication/kickstart/mvpd-kickstart-guide.md) e [guia de integração do MVPD](/help/authentication/integration-guide-mvpds/mvpd-integration-guide-overview.md).

### Fluxo de direitos {#entitlement-flow}

A Autenticação do Adobe Pass atua como proxy e facilita o fluxo de direitos entre Programadores e MVPDs, oferecendo interfaces seguras e consistentes para ambas as partes.

Para Programadores, a Autenticação do Adobe Pass fornece APIs como parte de uma camada **Standard** ou **Premium**:

* APIs padrão de autenticação da Adobe Pass:
   * [DCR DA API REST](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md)
   * [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)

* APIs de autenticação Premium do Adobe Pass:
   * [Redefinir API Temp Pass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#reset-tempass-api-access)
      * [Recurso TempPass](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md)
   * [API de degradação](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md#degradation-api-access)
      * [Recurso de degradação](/help/authentication/integration-guide-programmers/features-premium/degraded-access/degradation-feature.md)
   * [API de monitoramento do serviço de qualificação](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-api.md)

Para obter mais detalhes sobre o fluxo de qualificação, consulte a documentação do [Guia de Integração do Programador](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md#entitlement-flow).

#### Noções básicas sobre direitos {#understanding-entitlements}

A solução Adobe Pass Authentication baseia-se na criação de direitos — dados específicos gerados após a conclusão bem-sucedida de workflows de autenticação e autorização. Esses direitos concedem acesso a conteúdo protegido, mas têm um tempo de vida limitado. Quando um direito expira, ele deve ser renovado reiniciando os processos de autenticação ou autorização.

Para obter mais informações sobre direitos, consulte os seguintes documentos:

* **Perfis**

  Após a autenticação bem-sucedida, a Autenticação do Adobe Pass cria um perfil autenticado (&quot;duradouro&quot;) associado ao aplicativo solicitante, ao dispositivo e ao identificador do provedor de serviços (identificador do solicitante).

* **[Metadados de usuário](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)**

  Após a autenticação bem-sucedida (e em alguns casos também após a autorização), a Autenticação do Adobe Pass recebe metadados do usuário da MVPD que podem expô-los ao aplicativo solicitante.

* **[Decisões](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md)**

  Após a autorização bem-sucedida, a Autenticação do Adobe Pass cria uma decisão de autorização (&quot;longa vida&quot;) associada ao aplicativo solicitante, dispositivo, identificador de provedor de serviços (identificador do solicitante) e um recurso protegido específico (identificador de recursos).

* **[Tokens de mídia](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md)**

  Após a autorização bem-sucedida, a Autenticação do Adobe Pass cria um token de mídia (&quot;vida curta&quot;) que é associado a uma solicitação de reprodução bem-sucedida e fornece suporte às práticas recomendadas do setor para mitigar fraudes (por exemplo, extração de fluxo).

Os valores de vida útil (&quot;TTL&quot;) para perfis e decisões são definidos com base em acordos entre programadores e provedores de TV por assinatura, que concordam em um valor que melhor atende a todos os envolvidos.

#### Criação da interface do usuário {#building-user-interface}

Os programadores são responsáveis por projetar e implementar a interface do usuário (UI) para o fluxo de trabalho de direitos em seu site ou aplicativo, enquanto certos elementos, como o processo de logon, são tratados pelo provedor de TV por assinatura.

No mínimo, os programadores devem:

* **Implementar uma interface de seleção de provedor**
   * Permitir que novos usuários identifiquem o provedor de TV por assinatura e façam logon pela primeira vez.
   * Alguns provedores de TV por assinatura redirecionam os usuários para uma página de logon externa, enquanto outros exigem logon em um iframe. Os programadores devem implementar uma função de retorno de chamada para gerar o iframe quando necessário.

* **Gerenciar uma lista de provedores de TV por Assinatura com suporte**
   * Garantir que os usuários possam acessar o conteúdo somente por meio de provedores aprovados.

* **Indicar status de autenticação**
   * Mostrar quando um usuário é autenticado no aplicativo ou site.

* **Identificar recursos protegidos**
   * Indique claramente qual conteúdo requer autorização antes de visualizar.
   * Atualize a interface do usuário para refletir a autorização bem-sucedida assim que o acesso for concedido.

## Perguntas frequentes {#faqs}

**O que é TV em Todos os Lugares?**

TV Everywhere é uma iniciativa do setor que permite que os clientes de TV por assinatura acessem o conteúdo premium ao qual já estão inscritos em vários dispositivos conectados à Internet. Isso inclui computadores pessoais, tablets, smartphones, consoles de jogos, decodificadores de sinais e Smart TVs. O principal desafio é garantir um processo de autenticação simples e fácil de usar, permitindo que os clientes acessem o conteúdo de suas assinaturas sem vários logons ou barreiras técnicas.

**O que é a Autenticação do Adobe Pass e como ela oferece suporte à TV em Qualquer Lugar?**

A Autenticação Adobe Pass dá vida à TV em todos os lugares ao verificar com segurança o direito do usuário ao conteúdo de maneira simples e eficiente. É um serviço hospedado que facilita a rápida integração de back-end com base em regras de negócios definidas por programadores e provedores de TV por assinatura. Isso resulta em um tempo de entrada no mercado mais rápido para todas as partes interessadas, um ambiente mais seguro que minimiza as fraudes e uma melhor experiência do usuário, com mais conteúdo de TV acessível em várias plataformas.

**Como a Autenticação do Adobe Pass é entregue?**

A Autenticação do Adobe Pass é fornecida como uma solução de Software as a Service (SaaS). Essa abordagem garante a comunicação segura entre usuários finais, programadores e provedores de TV por assinatura para a validação do direito ao conteúdo.

**O que torna a Autenticação do Adobe Pass diferente de outras soluções da TV Everywhere?**

A Autenticação Adobe Pass oferece várias vantagens em relação às soluções alternativas:

* **Logon Único Ininterrupto (SSO)** - Ao contrário das integrações diretas com provedores individuais, a Autenticação da Adobe Pass habilita uma experiência de logon persistente à medida que os usuários se movem entre sites e aplicativos diferentes.
* **Penetração ampla no mercado** - Uma vez que o programador se integra à autenticação Adobe Pass, ele obtém acesso imediato a operadores de TV por assinatura que cobrem mais de 90% das residências dos EUA.
* **Integração com o ecossistema da Adobe** - Funciona perfeitamente com outras soluções da Adobe para entrega, proteção e monetização de conteúdo, incluindo o Adobe Analytics.

**Qual é o nível de segurança da Autenticação Adobe Pass?**

A segurança é uma prioridade. A autenticação da Adobe Pass garante que somente os usuários autorizados possam acessar o conteúdo premium ao vincular o acesso ao dispositivo do usuário. Também fornece opções para limitar o número de fluxos, sessões ou dispositivos simultâneos por residência.

**A quais dispositivos a Autenticação do Adobe Pass oferece suporte?**

A Autenticação Adobe Pass foi projetada para funcionar em uma grande variedade de dispositivos, como dispositivos baseados na Web, dispositivos móveis ou dispositivos conectados à TV.

**A Autenticação do Adobe Pass custa algo para os usuários finais?**

Não. A Autenticação Adobe Pass é gratuita para usuários finais.
