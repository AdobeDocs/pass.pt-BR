---
title: Guia de integração do MVPD
description: Guia de integração do MVPD
exl-id: b918550b-96a8-4e80-af28-0a2f63a02396
source-git-commit: 07bb12f7983f39b58e1b9795fdaa1bec4f68e674
workflow-type: tm+mt
source-wordcount: '1307'
ht-degree: 0%

---

# Guia de integração do MVPD {#mvpd-integration-guide}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

Este guia de integração é destinado aos Distribuidores de programação de vídeo multicanal (MVPDs) que planejam integrar com a Autenticação de passagem Adobe®.

TV Everywhere (TVE) é uma iniciativa transformadora no setor de TV por assinatura, permitindo que os assinantes acessem o conteúdo pelo qual já pagam em vários dispositivos, seja em casa ou em qualquer lugar. Para os provedores de TV por assinatura, a TVE oferece oportunidades significativas, entre as quais o fortalecimento das relações com os clientes existentes e a abertura de portas para novos clientes. No entanto, essas oportunidades trazem desafios.

No ecossistema da TVE, **Programadores** fornecem o conteúdo, enquanto **MVPDs** (Distribuidores de Programação de Vídeo Multicanal) gerenciam os dados do cliente necessários para verificar se os visualizadores são assinantes qualificados. Embora a coordenação da autenticação e da autorização com um único programador possa ser gerenciável, fazê-lo com dezenas ou até centenas de programadores introduz uma complexidade considerável.

É aqui que a **Adobe® Pass Authentication** simplifica o processo. Os MVPDs só precisam implementar uma única integração simplificada com o Adobe Pass para obter acesso a todo o ecossistema da TVE. A estrutura de integração fornecida acelera o tempo de entrada no mercado, fornece um ambiente seguro para reduzir fraudes e melhora a experiência do cliente, fornecendo mais conteúdo de TV em várias plataformas.

## Autenticação Adobe Pass para TV em todos os lugares {#adobe-pass-authentication-for-tv-everywhere}

A Autenticação do Adobe Pass funciona como uma solução SaaS (Software as a Service) projetada para permitir a integração rápida de back-end (servidor para servidor), seguindo as regras de negócios de Distribuidores de Programação de Vídeo Multicanal (MVPDs) e Programadores.

### Padrões e protocolos {#standards-protocols}

A autenticação do Adobe Pass está totalmente alinhada aos padrões e protocolos emergentes da TV Everywhere (TVE), oferecendo suporte à integração e conformidade ininterruptas em todo o ecossistema.

* **Especificação de CableLabs OLCA (Online Content Access)**\
  A autenticação Adobe Pass adere à especificação CableLabs OLCA, que define os requisitos técnicos e a arquitetura para o fornecimento de conteúdo de vídeo de fontes online para clientes de TV por assinatura. A Adobe participou ativamente do projeto de teste de interoperabilidade CableLabs em junho de 2011, passando com êxito no processo de teste para uma implementação do Provedor de serviços.

A Autenticação do Adobe Pass foi projetada para oferecer suporte a vários protocolos (por exemplo, SAML, OAuth 2.0, etc.), essa flexibilidade permite expansões futuras, incluindo protocolos personalizados, com base nas necessidades em evolução.

A maioria das integrações usa o protocolo SAML (Security Assertion Markup Language), um padrão primário para autenticação. A Autenticação do Adobe Pass atua como um Provedor de Serviço de proxy na estrutura SAML, persistindo na resposta de autenticação SAML como um token seguro no domínio comum do Adobe.

Como resultado, a autenticação da Adobe Pass é independente de protocolo, projetada para se alinhar aos padrões OLCA. Esses padrões estabelecem uma estrutura comum para programadores, MVPDs e provedores de serviços, garantindo uma abordagem coesa para a implementação dos recursos da TVE.

### Integração e suporte {#integration-support}

A autenticação da Adobe Pass colabora com as equipes técnicas da MVPD para configurar integrações personalizadas de acordo com seus requisitos específicos, da seguinte maneira:

* **Integrações padrão**\
  Oferecido gratuitamente, incluindo documentação e suporte básico por email.

* **Suporte Avançado**\
  Para personalizações significativas ou cronogramas acelerados, pode ser aplicada uma taxa de suporte.

A Autenticação do Adobe Pass oferece suporte à manipulação eficiente da lógica de negócios do MVPD, da seguinte maneira:

* **Lógica Comercial Independente**\
  Para a lógica de negócios aplicada integralmente pela MVPD durante as solicitações de autorização, a Adobe fornece os dados necessários. Esses dados podem incluir, mas não se limitam a, a ID de dispositivo exclusiva e o endereço IP do dispositivo do usuário que faz a solicitação.

* **Suporte à Propriedade Personalizada**\
  Para lógica de negócios que requer intervenção do usuário ou tratamento específico do Adobe, a Adobe pode manter configurações personalizadas para cada MVPD. Essas políticas permitem fluxos de trabalho predefinidos que podem ser acionados em pontos específicos no fluxo de trabalho de direitos. Para obter detalhes, entre em contato com o representante da Adobe.

## Fluxo de direitos {#entitlement-flow}

O fluxo de qualificação é uma série de etapas que um aplicativo de Programador (TVE) deve concluir para transmitir conteúdo protegido. O fluxo inclui várias fases que envolvem interações com a MVPD:

* [Fase de autenticação](#authentication-phase)
* (Opcional) Fase de pré-autorização
* [Fase de autorização](#authorization-phase)
* Fase de saída

Na visita inicial de um usuário a um aplicativo de Programador (TVE), o fluxo de direito segue a sequência descrita. No entanto, em visitas subsequentes, o aplicativo pode ignorar determinadas etapas com base no status da autenticação e nas políticas de visualização aplicáveis.

>[!NOTE]
>
> O aplicativo Programador (TVE) é usado neste documento para se referir coletivamente aos tipos de aplicativos em execução em diferentes plataformas (navegadores, dispositivos móveis, dispositivos conectados à TV etc.) compatíveis com a Autenticação Adobe Pass.

### Fase de autenticação {#authentication-phase}

As etapas a seguir descrevem as etapas de alto nível no caso de uma integração SAML:

1. **Carregamento do Aplicativo (Site) do Programador**\
   O usuário navega até o aplicativo (site) do Programador, que integra a [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) de Autenticação do Adobe Pass.

1. **Solicitação de conteúdo protegido**\
   Quando o usuário tenta acessar um conteúdo protegido, o aplicativo do Programador exibe uma lista de MVPDs que o usuário pode selecionar.

1. **Inicialização da Solicitação de Autenticação**\
   Após a seleção do MVPD, o usuário é redirecionado para um servidor de Autenticação Adobe Pass. Aqui, uma solicitação de autenticação SAML criptografada para o MVPD selecionado é gerada, no caso de uma integração SAML. Essa solicitação é enviada em nome do Programador para a MVPD. Dependendo do sistema do MVPD, o navegador do usuário é redirecionado para a página de logon do MVPD ou um iFrame de logon é incorporado no aplicativo do Programador.

1. **Logon do MVPD**\
   O MVPD aceita a solicitação e apresenta sua interface de logon, via redirecionamento ou iFrame.

1. **Logon e validação do usuário**\
   O usuário faz logon com suas credenciais da MVPD. O MVPD valida o status de subscrição do usuário e estabelece sua própria sessão HTTP.

1. **Resposta do MVPD à Autenticação do Adobe Pass**\
   Quando a validação for concluída, o MVPD gerará uma resposta SAML (criptografada) e a enviará de volta para a Autenticação Adobe Pass.

1. **Geração de perfil**\
   A Autenticação do Adobe Pass verifica a resposta SAML, gera um perfil de usuário que é armazenado em cache e redireciona o usuário para o aplicativo do Programador (site).

### Fase de autorização {#authorization-phase}

**Etapas de alto nível**

As etapas a seguir descrevem as etapas de alto nível:

1. **Tratamento do Identificador de Recursos**\
   O conteúdo protegido é identificado por um [identificador de recurso](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md#resource-identifier), que pode ser uma cadeia de caracteres simples ou uma estrutura mais complexa. Esse identificador é predefinido e acordado entre o Programador e a MVPD. O aplicativo do Programador envia o identificador de recurso para a [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) da Autenticação do Adobe Pass.

1. **Verificação de autorização do MVPD**\
   O servidor de autenticação da Adobe Pass se comunica com o endpoint de autorização da MVPD usando protocolos padronizados.

1. **Resposta do MVPD à Autenticação do Adobe Pass**\
   Quando a validação for concluída, o MVPD confirmará se o usuário tem direito (ou não) de acessar o conteúdo e enviará uma resposta para a Autenticação Adobe Pass.

1. **Geração de token de mídia e decisão**\
   A Autenticação da Adobe Pass verifica a resposta, gera uma [decisão](/help/authentication/integration-guide-programmers/features-standard/entitlements/decisions.md) que é armazenada em cache e retorna a decisão que contém um token de mídia de volta ao aplicativo do Programador (site).

1. **Verificação de acesso ao conteúdo**\
   O aplicativo do Programador usa o [Verificador de Token de Mídia](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier) para confirmar se o usuário correto está acessando o conteúdo correto. Depois de validado, o usuário recebe acesso para visualizar o conteúdo protegido.

## Noções básicas sobre direitos {#understanding-entitlements}

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
