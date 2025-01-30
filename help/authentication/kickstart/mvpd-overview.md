---
title: Visão geral para MVPDs
description: Visão geral para MVPDs
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '2727'
ht-degree: 0%

---

# Uma visão geral dos MVPDs {#mvpd-overview}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#intro}

Esta visão geral destina-se aos Distribuidores de programação de vídeo multicanal (MVPDs). Para obter a documentação adicional, incluindo o Kickstart e guias de integração, consulte a seção Informações Relacionadas no final deste documento.



TV Everywhere (TVE) é o agora conhecido movimento do setor que permite que os assinantes de TV por assinatura acessem o conteúdo pelo qual já pagam, em vários dispositivos, dentro e fora de suas casas.  Para os provedores de TV por assinatura, a TVE cria novas oportunidades, tanto para preservar os relacionamentos existentes com os clientes quanto para permitir novos relacionamentos. No entanto, juntamente com essas oportunidades surgem desafios. No cenário da TVE, os programadores fornecem o conteúdo, mas os MVPDs mantêm as informações do cliente para verificar se os visualizadores em potencial são assinantes válidos.



Coordenar a autenticação e autorização do visualizador com um programador pode ser simples, mas coordenar com dezenas ou centenas de programadores diferentes torna-se cada vez mais complexo. No entanto, com o Adobe® Pass, os MVPDs só precisam implementar uma única integração simples para obter acesso a todo o ecossistema da TVE, incluindo Programadores como NBCUniversal Media, Turner Broadcasting (TBS, TNT, CNN), Fox Broadcast Networks, Hulu e assim por diante.  A Autenticação do Adobe Pass fornece uma estrutura de integração que torna simples e segura a determinação dos direitos do usuário.



Resumindo: a autenticação do Adobe Pass faz a mediação segura de transações de direitos entre Programadores e MVPDs, facilitando o acesso do visualizador ao conteúdo de assinatura. Em outras palavras, a autenticação da Adobe Pass facilita e agiliza o acesso dos clientes certos ao conteúdo correto.


Com a Autenticação Adobe Pass, os MVPDs recebem:

Fácil integração com programadores.  Fornecer conectividade instantânea de vários proprietários de conteúdo com uma única integração.

Maior engajamento do cliente.  Ofereça suporte a uma experiência perfeita e de marca, à medida que seus clientes visualizam o conteúdo em várias plataformas e dispositivos.

Autenticação segura.  Garanta que somente usuários e dispositivos autorizados tenham acesso ao conteúdo premium e (opcionalmente) limite o número de dispositivos e fluxos simultâneos que podem se conectar por conta da família.

## Perguntas frequentes {#faq}

Qual é o nível de segurança da autenticação do Adobe Pass? A prioridade número um da arquitetura de autenticação da Adobe Pass é garantir que somente visualizadores autorizados sejam autenticados e tenham acesso ao conteúdo premium. A Autenticação Adobe Pass vincula rigorosamente o acesso a dispositivos de visualização e pode ajudar a limitar fluxos, sessões e/ou dispositivos para uma determinada residência.


O Flash Player é necessário? A autenticação Adobe Pass para TV Everywhere é independente de plataforma e player, integrando-se a qualquer aplicativo de reprodução, incluindo Silverlight e HTML 5. Além disso, a Autenticação Adobe Pass fornece suporte nativo a dispositivos como telefones e tablets com iOS e Android.


Quais dispositivos são compatíveis com a Autenticação Adobe Pass? A autenticação Adobe Pass é suportada por praticamente qualquer dispositivo com o kit da Web HTML5 para experiências de visualização no navegador. Além disso, a autenticação da Adobe Pass continua distribuindo SDKs (Software Development Kits, kits de desenvolvimento de software) nativos para várias plataformas específicas de dispositivos, incluindo aplicativos iOS, Android™, Xbox360 (obsoleto) e Adobe Air® (obsoleto). Mais recentemente, a Autenticação Adobe Pass lançou uma solução sem cliente, para dispositivos que não podem renderizar páginas do navegador (por exemplo, TVs &quot;inteligentes&quot;, decodificadores de sinais e consoles de jogos).  A capacidade de renderizar páginas do navegador é um requisito para autenticar usuários com MVPDs.


A Autenticação Adobe Pass suporta os padrões emergentes para a TV Everywhere? A autenticação Adobe Pass é compatível com a especificação CableLabs OLCA (Online Content Access), que fornece requisitos técnicos e arquitetura para a entrega de vídeo para um cliente de TV por assinatura a partir de fontes online. A Adobe participou do projeto conjunto de testes de interopt de CableLabs em junho de 2011 e passou no processo de teste para uma implementação do Provedor de serviços. A autenticação do Adobe Pass é verificada (completa e testada) em relação às especificações OLCA para autenticação. O componente de autorização foi concluído, mas a verificação de teste aguarda o lançamento do ambiente de teste CableLabs. A Adobe também é membro ativo do Consórcio Técnico de Autenticação Aberta (OATC) e participa em vários projetos de redação de especificações dos subcomitês como parte desse órgão.



O que é autenticação? A autenticação é o processo no qual uma MVPD confirma que determinado usuário é um cliente conhecido.



O que é autorização? A autorização é o processo no qual uma MVPD confirma que um usuário autenticado tem uma assinatura válida para um determinado recurso.



## Arquitetura {#architecture}

A Autenticação do Adobe Pass é um serviço hospedado que permite a integração rápida de back-end (servidor para servidor) com base nas regras de negócios exigidas por MVPDs e Programadores. Isso significa um rápido tempo de comercialização para todas as partes, um ambiente mais seguro para evitar fraudes e uma experiência superior para o cliente, com mais conteúdo de TV disponível para mais pessoas em mais plataformas.


A autenticação do Adobe Pass é oferecida por meio do modelo Software as a Service (SaaS) e permite que comunicações mais seguras ocorram entre usuários finais, MVPDs e Programadores, a fim de validar o direito ao conteúdo. Os componentes principais do serviço incluem o seguinte:

Server Side - O Servidor De Autenticação Adobe Pass Hospedado. Este é um servidor de aplicativos que realiza comunicação de canal de retorno (servidor para servidor) com os sistemas de autenticação de MVPDs.
Lado do cliente:
Ativador de acesso no lado do cliente - O Ativador de acesso é um pequeno arquivo carregado em uma página da Web ou aplicativo de reprodução do Programador do. Ele fornece APIs de direito ao aplicativo de visualização de conteúdo do Programador e se comunica com o Servidor de autenticação do Adobe Pass.
Serviços Web sem cliente (para dispositivos não compatíveis com a Web) - Serviços Web RESTful que fornecem APIs de direito para dispositivos como TVs inteligentes, consoles de jogos e decodificadores de sinais.

>[!NOTE]
>
>Como um MVPD, os serviços da Web devem ser capazes de reconhecer solicitações de autenticação e autorização da Autenticação Adobe Pass e responder com os dados necessários no formato esperado.
>

A Autenticação do Adobe Pass permite fornecer aos clientes gerenciamento de identidade federada, também conhecido como autenticação e autorização de logon único (SSO). Com a Autenticação Adobe Pass, não há necessidade de os assinantes fazerem logon novamente após sua primeira autenticação, desde que essa autenticação seja permitida pela MVPD para persistir. (Normalmente, 30 dias.) Para isso, a Autenticação da Adobe Pass fornece um domínio comum para tokens de autenticação para nossos clientes. Essas informações de estado de autenticação estão disponíveis para todos os sites participantes integrados a uma determinada MVPD.


Atualmente, a maioria das integrações da Autenticação Adobe Pass com MVPDs usa o protocolo SAML, um dos padrões de autenticação primários. A Autenticação Adobe Pass atua como um Provedor de Serviço de proxy na arquitetura SAML e mantém a resposta de autenticação SAML como um token seguro no domínio comum Adobe. A Autenticação do Adobe Pass é compatível com SAML 2.0. No entanto, enquanto a Autenticação Adobe Pass é normalmente usada com soluções SAML SSO neste ponto, a arquitetura de Autenticação Adobe Pass não está vinculada a nenhum protocolo específico. Portanto, o suporte para novos protocolos - como um baseado em OAuth 2.0 ou protocolos personalizados - pode ser adicionado ao longo do tempo.


O Adobe trabalha com uma equipe técnica da MVPD para configurar a Autenticação Adobe Pass de forma a atender às necessidades de quaisquer integrações existentes. A integração é gratuita para MVPDs, pressupondo uma integração &quot;padrão&quot; e requisitos mínimos de suporte (documentação e suporte básico por email). Se uma MVPD exigir suporte significativo ou um cronograma escalonado, uma taxa de suporte poderá ser cobrada ou o provedor poderá querer trabalhar com terceiros familiarizados com nossa solução, como a Synacor.


A Autenticação da Adobe Pass também oferece suporte à manipulação eficiente da lógica de negócios do MVPD, como demonstrado a seguir:

Para uma lógica de negócios que é independente e pode ser aplicada pela MVPD quando uma solicitação de autorização é recebida, o Adobe fornece os dados necessários necessários para dar suporte à aplicação da lógica de negócios quando a MVPD recebe uma solicitação de autorização. Esses dados podem incluir, mas não se limitam a, a ID de dispositivo exclusiva do usuário que faz a solicitação e o endereço IP do dispositivo.

Para uma lógica de negócios que requer intervenção do usuário e/ou tratamento específico pela solução Adobe, o Adobe pode manter algumas propriedades personalizadas para cada MVPD. Essas configurações/políticas específicas da MVPD incluem a ativação de fluxos de trabalho predefinidos que podem ser iniciados em pontos específicos do fluxo de trabalho de nível superior. Para obter detalhes sobre o suporte a propriedades personalizadas, entre em contato com o representante da Adobe.

O diagrama a seguir ilustra o relacionamento do MVPD e do Programador com esses componentes de Autenticação do Adobe Pass:

![](../assets/high-level-architecture-nflows.png)

*Figura: arquitetura e fluxos de alto nível*

## Componentes de autenticação do Adobe Pass {#components}

A seguir é fornecida uma visão geral de alguns dos principais componentes do ecossistema de autenticação da Adobe Pass. Isso inclui:

* [Os serviços Web do Access Enabler / sem clientes](#ae)
* [O servidor back-end hospedado em Adobe](#backend)
* [Tokens](#tokens)

### Serviços da Web do Access Enabler/sem cliente {#ae}

O Access Enabler facilita todas as interações de autenticação e autorização com o usuário e é executado localmente em seu sistema. É o Ativador de acesso que lida com os workflows de direito reais com o MVPD, enquanto o Programador mantém a responsabilidade pela página da Web de nível superior ou pelo aplicativo do reprodutor.

Os serviços da Web sem clientes são fornecidos pela Autenticação Adobe Pass para dispositivos que não podem renderizar páginas da Web.  Para esses dispositivos, o processo de qualificação é iniciado e o conteúdo é exibido no dispositivo inteligente, enquanto a autenticação com um MVPD ocorre em um dispositivo habilitado para a Web (PC, smartphone e tablet).

O Access Enabler:

* Inicia workflows de autenticação e autorização específicos do MVPD.
* Armazena em cache as respostas de autorização bem-sucedidas por recurso/canal do programador para minimizar o tráfego de solicitação desnecessário.
* Pode ser configurado para fluxos de trabalho predefinidos específicos para cada MVPD, como o registro explícito do dispositivo.
* Ela está disponível destas formas:
   * Um arquivo SWF que o tempo de execução do Flash Player pode executar
   * Um arquivo JS executado diretamente pelo navegador
   * Um Ativador de acesso nativo para várias plataformas, incluindo iOS, Android e Xbox.

### Servidor de back-end hospedado em Adobe {#backend}

O servidor back-end de autenticação da Adobe Pass, hospedado pelo Adobe:

* Provisiona os workflows de autenticação e autorização com os MVPDs que exigem comunicação servidor a servidor entre a Autenticação do Adobe Pass e o operador.
* Mantém a configuração dos sites e aplicativos do Programador.
* Hospeda os arquivos de componente baixáveis do Access Enabler.
* Gera tokens de autenticação e autorização.

### Tokens {#tokens}

A solução de direitos de autenticação da Adobe Pass se concentra na geração de dados específicos que são obtidos após a conclusão bem-sucedida de workflows de autenticação/autorização. Esses dados são chamados de tokens. Eles têm uma vida útil limitada e são armazenados com segurança em locais que dependem da plataforma. Após a expiração, os tokens devem ser emitidos novamente por meio da reinicialização dos workflows de autenticação e/ou autorização.

Há três tipos de tokens emitidos durante os workflows de autenticação/autorização. Duas são de &quot;longa duração&quot;, proporcionando continuidade na experiência de visualização do usuário. O terceiro, um token de vida curta, fornece suporte às práticas recomendadas do setor para mitigar fraudes por meio da extração de fluxo. Os valores de vida útil (&quot;TTL&quot;) para tokens são definidos com base em acordos entre MVPDs e Programadores. Você decide sobre um valor de TTL que melhor atende a sua empresa e seus clientes.

**O token de autenticação de longa duração**. O sucesso da autenticação ocorre quando um cliente usa a autenticação da Adobe Pass para fazer logon com êxito em sua conta da MVPD. A Autenticação do Adobe Pass produz um token de autenticação de longa duração (&quot;authN&quot;) vinculado ao dispositivo solicitante e (dependendo da MVPD) um identificador exclusivo global (&quot;GUID&quot;) que identifica o usuário de forma anônima.

**O token de autorização de longa duração**. Após a autorização bem-sucedida, a Autenticação do Adobe Pass cria um token de autorização de longa vida (&quot;authZ&quot;). Esse token não é portátil, pois está vinculado ao dispositivo solicitante e a um recurso protegido específico (por exemplo, um canal, série ou episódio). O Access Enabler usa o token de authZ de longa duração para criar os tokens de mídia de curta duração usados para o acesso de visualização real.

**O token de mídia de vida curta**. Depois que o usuário é autorizado, a Autenticação Adobe Pass gera um token de authZ e o usa para gerar um token de mídia de uso único e de vida curta, assinado pelo Adobe e criptografado para evitar violação durante o Exchange. Como o token de vida curta é exposto ao site incorporado por meio da API do Access Enabler ou dos serviços da Web sem cliente, antes de fornecer acesso ao recurso protegido, o servidor de mídia do Programador deve usar um componente de Autenticação do Adobe Pass, o Verificador de token de mídia, para validar o token.

## Ciclo de vida de integração do MVPD {#lifecycle}

A ilustração a seguir mostra o ciclo de vida da integração entre a Autenticação do Adobe Pass e uma MVPD.

![](../assets/mvpd-int-lifecycle.png)

*Figura: ciclo de vida da integração do MVPD*

## Fluxograma de Direitos {#chart}

O fluxograma a seguir apresenta o processo geral de confirmação de direito usando a Autenticação Adobe Pass:

![](../assets/authn-authz-entitlmnt-flow.png)

*Figura: processo de confirmação de direitos usando a Autenticação Adobe Pass*

## Etapas de autenticação {#authn-steps}

As etapas a seguir apresentam um exemplo do fluxo de autenticação da Autenticação do Adobe Pass.  Essa é a parte do processo de qualificação em que um Programador determina se o usuário é um cliente válido de uma MVPD.  Nesse cenário, o usuário é um assinante válido de uma MVPD.  O usuário está tentando exibir o conteúdo protegido usando um aplicativo de Flash do Programador:

1. O usuário navega até a página do Programador na Web, que carrega o aplicativo do Flash do Programador e os componentes do Adobe Pass Authentication Access Enabler no computador do usuário. O aplicativo Flash usa o Access Enabler para definir a identificação do Programador com a Autenticação do Adobe Pass, e a Autenticação do Adobe Pass prioriza o Ativador de acesso com os dados de configuração e estado desse Programador (o &quot;solicitante&quot;). O Ativador de acesso deve receber esses dados do servidor antes de executar outras chamadas de API.  Nota técnica: o Programador definiu sua identidade com o método `setRequestor()` do Ativador de Acesso.
1. Quando o usuário tenta visualizar o conteúdo protegido do Programador, o aplicativo do Programador apresenta ao usuário uma lista de MVPDs, a partir dos quais o usuário seleciona um provedor.
1. O usuário é redirecionado para um servidor de Autenticação do Adobe Pass, onde uma solicitação SAML criptografada para o MVPD selecionado pelo usuário é criada. Essa solicitação é enviada como uma solicitação de autenticação em nome do Programador para a MVPD. Dependendo do sistema do MVPD, o navegador do usuário é então redirecionado para o site do MVPD para fazer logon ou um iFrame de logon é criado no aplicativo do Programador.
1. Em ambos os casos (redirecionamento ou iFrame), o MVPD aceita a solicitação e exibe sua página de logon.
1. O usuário faz logon com a MVPD, a MVPD valida o status do usuário como um cliente pagante e, em seguida, a MVPD cria sua própria sessão HTTP.
1. Quando o usuário é validado, o MVPD cria uma resposta (SAML e criptografada), que o MVPD envia de volta para a Autenticação do Adobe Pass.
1. A Autenticação da Adobe Pass recebe a resposta da MVPD, vê que há uma sessão HTTP de Autenticação da Adobe Pass aberta, valida a resposta [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) da MVPD e redireciona de volta para o site do Programador.
1. O site do Programador é recarregado, o Ativador de acesso é recarregado e o Programador chama setRequestor() novamente.  A segunda chamada para setRequestor() é necessária porque a configuração atual foi alterada - agora há um sinalizador presente que informa ao Ativador de acesso que um token de autenticação está aguardando para ser gerado no servidor.
1. O Ativador de acesso vê que há uma autenticação pendente e solicita o token do servidor de autenticação da Adobe Pass. O token é recuperado do servidor chamando os recursos DRM do Flash Player.
1. O token de autenticação é armazenado no cache LSO do Flash Player do programador; a autenticação agora está concluída e a sessão é destruída no servidor de autenticação da Adobe Pass.

## Etapas de autorização {#authz-steps}

As etapas a seguir continuam da seção anterior ([Etapas de Autenticação](#authn-steps)):

1. Quando o usuário tenta acessar o conteúdo protegido do Programador, o aplicativo do Programador primeiro verifica se há um token de Autenticação no computador ou dispositivo local do usuário.  Se esse token não estiver lá, as [Etapas de autenticação](#authn-steps) acima serão seguidas.  Se o token de autenticação estiver lá, o fluxo de autorização continuará com o aplicativo do programador iniciando uma chamada para o ativador de acesso com uma solicitação para obter os direitos de visualização do usuário para um item específico de conteúdo protegido.
1. O item específico de conteúdo protegido é representado por um &quot;identificador de recurso&quot;.  Pode ser uma sequência simples ou uma estrutura mais complexa, mas, em qualquer caso, a natureza do identificador de recursos é acordada antecipadamente entre o Programador e a MVPD.  O aplicativo do Programador passa o identificador de recurso para o Ativador de acesso.  O Ativador de acesso verifica um token de AuthZ no computador ou dispositivo local do usuário.  Se o token AuthZ não estiver lá, o Ativador de acesso passará a solicitação para o servidor de autenticação Adobe Pass de back-end.
1. O servidor de Autenticação do Adobe Pass se comunica com o endpoint de autorização MVPDs usando protocolos padronizados.  Se a resposta do MVPD indicar que o usuário tem direito a visualizar o conteúdo protegido, o servidor de autenticação da Adobe Pass criará um token de AuthZ e o transmitirá de volta para o Access Enabler, que armazena o token de AuthZ no computador do usuário.
1. Com um token de AuthZ armazenado na máquina ou no dispositivo do usuário, o aplicativo do Programador chama o Ativador de acesso para obter um token de mídia do servidor de autenticação da Adobe Pass e fornece esse token para o aplicativo do Programador.
1. Por fim, o aplicativo do Programador usa o componente Verificador de token de mídia para confirmar se o usuário correto está visualizando o conteúdo correto e, com o token de mídia instalado, o usuário pode visualizar o conteúdo protegido.

<!--
>![RELATEDINFORMATION]
>
>*   Kickstart Guides, [MVPD kickstart](/help/authentication/mvpd-kickstart-guide.md) and [programmer kickstart](/help/authentication/programmer-kickstart-guide.md). These guides explain the initial steps to take to begin integrating with Adobe Pass Authentication.
>
>*   [MVPD Integration Guide](/help/authentication/mvpd-kickstart-guide.md). This is a lower level technical guide for MVPDs, directed primarily to the software engineers who code and test the applications and systems involved in the integration.
>
>*   [Overview For Programmers](/help/authentication/programmer-overview.md). The same high level of conceptual information as in this MVPD overview, but directed toward the content providers (Programmers).
-->
