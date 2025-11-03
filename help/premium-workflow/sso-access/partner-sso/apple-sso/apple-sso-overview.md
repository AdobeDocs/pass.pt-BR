---
title: Visão geral do Apple SSO
description: Visão geral do Apple SSO
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 2afe9ea2a814817757f1ab28484a84466da68d62
workflow-type: tm+mt
source-wordcount: '1260'
ht-degree: 0%

---

# Visão geral do Apple SSO {#apple-sso-overview}

>[!IMPORTANT]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

O Apple fornece aos usuários a capacidade de fazer logon em sua conta de provedor de TV no nível do sistema do dispositivo, eliminando a necessidade de autenticação aplicativo por aplicativo.

A Autenticação do Adobe Pass fez parceria com a Apple para criar a experiência de usuário de Logon único (SSO) de parceiro no ecossistema da TV em todos os lugares para proprietários de iPhone, iPad e Apple TV.

Para se beneficiar da experiência do usuário de Logon único (SSO) em um dispositivo Apple, há uma lista de pré-requisitos documentados abaixo que devem ser concluídos.

O resultado final deve criar uma experiência de acordo com os seguintes fluxos de usuário, que recomendamos consultar antes de começar a desenvolver seu aplicativo:

* Fluxos de usuários do [Logon Único (SSO) para dispositivos iPhone e iPad](https://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf).
* Fluxos de usuários do [Logon Único (SSO) para dispositivos Apple TV](https://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf).

## Pré-requisitos {#apple-sso-prerequisites}

Os pré-requisitos de integração podem se aplicar a uma ou várias entidades envolvidas no negócio da TVE, como Programadores, MVPDs, Autenticação do Adobe Pass ou Apple.

### Programador {#apple-sso-prerequisites-programmer}

Para se beneficiar da experiência do usuário de Logon Único (SSO), um Programador deve:

* Entre em contato com a Apple para habilitar a [Estrutura da Conta do Assinante do Vídeo](https://developer.apple.com/documentation/videosubscriberaccount) como parte da sua Identificação da Equipe da Apple e configure a [Qualificação de Logon Único do Assinante do Vídeo](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) como parte da sua Conta de Desenvolvedor do Apple.

   * Use o Xcode versão 8 ou superior e o iOS/tvOS versão 10 ou superior.

* Habilite o Logon Único (SSO) para cada integração e plataforma desejada (iOS/tvOS) por meio do [Painel do Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication), definindo a propriedade `Enable Single Sign On` como `Yes`.

| Habilitar logon único no Adobe | Apple **Integrado (com Suporte)** MVPDs | MVPDs do **Seletor** do Apple | Apple **Não Integrado (Sem Suporte)** MVPDs |
|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| Sim (Ativado) | Os fluxos de autenticação e logout envolvem as soluções de autenticação da Apple e da Adobe Pass, enquanto todos os outros fluxos (autorização, pré-autorização, metadados etc.) são atendidos exclusivamente pela autenticação da Adobe Pass. | Os fluxos de autenticação e logout recorrerão aos fluxos comuns atendidos exclusivamente pela autenticação da Adobe Pass. | Os fluxos de autenticação e logout recorrerão aos fluxos comuns atendidos exclusivamente pela autenticação da Adobe Pass. |
| Não (Desabilitado) | Os fluxos de autenticação e logout recorrerão aos fluxos comuns atendidos exclusivamente pela autenticação da Adobe Pass. | Os fluxos de autenticação e logout recorrerão aos fluxos comuns atendidos exclusivamente pela autenticação da Adobe Pass. | Os fluxos de autenticação e logout recorrerão aos fluxos comuns atendidos exclusivamente pela autenticação da Adobe Pass. |

* Integre os fluxos de usuário de Logon único (SSO) usando uma das seguintes soluções oferecidas pela Autenticação do Adobe Pass para usuários finais de aplicativos clientes em execução no iOS, iPadOS ou tvOS.

   * A API REST V2 de Autenticação do Adobe Pass é compatível com o Logon Único de Parceiro (SSO).

     Consulte a documentação [Guia de SSO do Apple (REST API V2)](apple-sso-cookbook-rest-api-v2.md).

   * A API REST V1 herdada da Autenticação do Adobe Pass tem suporte para Logon único de parceiro (SSO).

     Consulte a documentação [(Herdado) Guia de SSO Apple (REST API V1)](/help/authentication/integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-rest-api-v1.md).

   * O herdado Adobe Pass Authentication AccessEnabler iOS/tvOS SDK é compatível com o Partner Single Sign-On (SSO).

     Consulte a documentação [(Herdado) Guia de SSO do Apple (iOS/tvOS SDK)](/help/authentication/integration-guide-programmers/legacy/sso-access/apple-sso-cookbook-iostvos-sdk.md).

### MVPD {#apple-sso-prerequisites-mvpd}

Para se beneficiar da experiência do usuário de Logon único (SSO), um MVPD deve:

* Entre em contato com a Apple para iniciar o processo de integração no lado da Apple.

   * Solicite a documentação técnica sobre como integrar e desenvolver um aplicativo TVML do JavaScript capaz de lidar com o formulário de logon do usuário.

* Entre em contato com a Autenticação da Adobe Pass para iniciar o processo de integração no lado da Adobe.

   * Forneça o valor da string que representa o identificador do provedor de TV atribuído pela Apple durante o processo de integração.

## Perguntas frequentes {#FAQ}

* Caso algo dê errado com o fluxo de trabalho SSO do Apple, o aplicativo que usa o Adobe Pass Authentication AccessEnabler iOS/tvOS SDK pode recorrer ao fluxo de autenticação normal?

  Isso é possível, mas requer que uma alteração de configuração seja executada por meio do [Painel do Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication) para definir o **Habilitar Logon Único** em **NÃO** para a integração e plataforma desejadas (iOS/tvOS). Esteja ciente de que o aplicativo cliente reconhecerá a alteração de configuração somente após chamar a API [setRequestor](/help/authentication/integration-guide-programmers/legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md#setReqV3).


* O aplicativo saberá quando uma autenticação ocorreu como resultado de um logon por meio do Apple SSO?

  Essas informações estão disponíveis como parte da chave de metadados do usuário: *tokenSource*, que deve retornar o valor da cadeia de caracteres: &quot;Apple&quot; neste caso.


* O aplicativo saberá quando uma autenticação aconteceu como resultado de um logon por meio do Apple SSO em outro aplicativo?

  Esta informação não está disponível.


* O que acontece se um usuário entrar acessando a seção *`Settings -> TV Provider`* no iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* no tvOS usando uma MVPD que não esteja integrada ao aplicativo?

  Quando o usuário inicia o aplicativo, ele não é autenticado por meio do fluxo de trabalho SSO do Apple. Portanto, o aplicativo teria que recorrer ao fluxo de autenticação regular e apresentar seu próprio seletor de MVPD.


* O que acontece se um usuário entrar pela *`Settings -> TV Provider`* no iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* na seção tvOS usando uma MVPD que tenha o **Habilitar Logon Único** definido em **NÃO** por meio do [Painel do Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication) para a plataforma iOS/tvOS?

  Quando o usuário inicia o aplicativo, ele não é autenticado por meio do fluxo de trabalho SSO do Apple. Portanto, o aplicativo teria que recorrer ao fluxo de autenticação regular e apresentar seu próprio seletor de MVPD.


* O que acontece se um usuário tiver uma MVPD que não é integrada (não compatível) pelo Apple, mas está presente no seletor de Apple?

  Quando o usuário inicia o aplicativo, ele só seleciona o MVPD por meio do fluxo de trabalho SSO do Apple sem concluir o fluxo de autenticação. Portanto, o aplicativo teria que recorrer ao fluxo de autenticação regular, mas poderia usar o MVPD já selecionado.


* O que acontece se um usuário tiver uma MVPD que não é integrada (não é compatível) pelo Apple?

  Quando o usuário inicia o aplicativo, o usuário seleciona a opção de seleção &quot;Outros provedores de TV&quot; por meio do fluxo de trabalho SSO do Apple. Portanto, o aplicativo teria que recorrer ao fluxo de autenticação regular e apresentar seu próprio seletor de MVPD.


* O que acontece se um usuário tiver uma MVPD degradada por meio do [Painel do Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication)?

  Quando o usuário inicia o aplicativo, ele é autenticado por meio do mecanismo de degradação e não pelo fluxo de trabalho SSO do Apple. A experiência deve ser perfeita para o usuário, enquanto o aplicativo será informado por meio do código de aviso *N010* caso esteja usando o Adobe Pass Authentication AccessEnabler iOS/tvOS SDK.


* A ID de usuário do MVPD mudará entre os fluxos de autenticação SSO da Apple e SSO que não sejam da Apple?

  A expectativa é que a ID do usuário não seja alterada, mas precisa ser verificada para cada provedor selecionado.


* Haverá alguma alteração nos TTLs de autenticação?

  A Autenticação do Adobe Pass continuará a respeitar os TTLs exigidos pelos Programadores para sua integração com cada MVPD. Ao navegar de um aplicativo do Programador para outro aplicativo do Programador por meio do SSO do Apple, o segundo aplicativo terá o TTL da integração Programador x MVPD correspondente (ele não compartilhará o TTL do primeiro aplicativo autenticado)

|                                      | TTL de Autenticação do Adobe Pass expirado | TTL de autenticação do Adobe Pass válido |
|--------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| **TTL de token de dispositivo do Apple expirado** | o usuário NÃO está autenticado (o seletor de MVPD deve ser exibido) | O usuário é autenticado e o TTL é o tempo restante de seu token/perfil de autenticação da Adobe Pass |
| **TTL de token de dispositivo do Apple válido** | O usuário é autenticado silenciosamente e obtém outro token/perfil de Autenticação do Adobe Pass com o TTL especificado no Painel TVE | O usuário é autenticado e o TTL é o tempo restante de seu token/perfil de autenticação da Adobe Pass |
