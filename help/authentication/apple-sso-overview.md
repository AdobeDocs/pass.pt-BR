---
title: Visão geral do Apple SSO
description: Visão geral do Apple SSO
exl-id: 7cf47d01-a35a-4c85-b562-e5ebb6945693
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '1452'
ht-degree: 0%

---

# Visão geral do Apple SSO {#apple-sso-overview}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#Introduction}

O Apple fornece uma API que permite que as pessoas façam logon em sua conta de provedor de TV no nível do sistema do dispositivo, eliminando a necessidade de autenticação aplicativo por aplicativo.

Assim, a Autenticação Apple e Adobe Pass fez uma parceria para criar a experiência de usuário de Logon único (SSO) na plataforma no ecossistema da TV em todos os lugares para proprietários de iPhone, iPad e Apple TV.

Para se beneficiar da experiência do usuário de Logon único (SSO) em um dispositivo Apple, há uma lista de pré-requisitos que devem ser concluídos.

</br>

## Pré-requisitos {#Prerequisites}

O pré-requisito pode se aplicar a uma ou várias entidades envolvidas no negócio da TVE, como Programadores, MVPDs, Autenticação do Adobe Pass ou Apple.

</br>

### Programador {#Programmer}

Para se beneficiar da experiência do usuário de Logon Único (SSO), um Programador deve:

1. Use pelo menos o Xcode versão 8 e o iOS/tvOS versão 10.

1. Tenha a [Direito de Logon Único de Assinante de Vídeo](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_video-subscriber-single-sign-on) configurados para sua conta de desenvolvedor do Apple. Entre em contato com a Apple para ativar [Estrutura da conta do assinante de vídeo](https://developer.apple.com/documentation/videosubscriberaccount) para a ID da equipe da Apple.

1. Habilite o Logon único (SIM) para cada integração desejada (Canal x MVPD) e plataforma desejada (iOS / tvOS) por meio da [Painel Adobe Primetime TVE](https://console.auth.adobe.com/).

1. Integre os workflows de SSO do Apple usando uma das duas soluções a seguir oferecidas pela equipe de autenticação da Adobe Pass:

   - A API REST de autenticação da Adobe Pass pode oferecer suporte à autenticação de logon único (SSO) da plataforma para usuários finais de aplicativos clientes em execução no iOS, iPadOS ou tvOS. Consulte também [Guia do Apple SSO (REST API)](/help/authentication/apple-sso-cookbook-rest-api.md).

   - O SDK iOS/tvOS do Adobe Pass Authentication AccessEnabler pode oferecer suporte à autenticação de logon único (SSO) da plataforma para usuários finais de aplicativos clientes em execução no iOS, iPadOS ou tvOS. Consulte também [Guia do Apple SSO (iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md).

   - **<u>Dica Profissional:</u>** Para ter acesso às informações de assinatura do usuário, o usuário deve dar permissão ao aplicativo para continuar, de modo semelhante a fornecer acesso à câmera ou ao microfone do dispositivo. Essa permissão deve ser solicitada por aplicativo e o dispositivo salvará a seleção do usuário. Lembre-se de que o usuário pode alterar sua decisão acessando as configurações do aplicativo (acesso de permissão ao Provedor de TV) ou a seção de *`Settings -> TV Provider`* no iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* no tvOS.

   - **<u>Dica Profissional:</u>** Recomendamos solicitar a permissão do usuário quando o aplicativo entrar no estado de primeiro plano, mas isso é apenas uma sugestão, pois o aplicativo pode verificar [permissão para acessar](https://developer.apple.com/documentation/videosubscriberaccount/vsaccountmanager/1949763-checkaccessstatus) as informações de assinatura do usuário a qualquer momento antes da solicitação de autenticação do usuário. Além disso, as APIs do SDK iOS/tvOS do AccessEnabler solicitarão automaticamente a permissão do usuário quando ele precisar.

   - **<u>Dica Profissional:</u>** Recomendamos incentivar os usuários que se recusam a dar permissão para acessar informações de assinatura explicando os benefícios da experiência do usuário de Logon único (SSO). Lembre-se de que o usuário pode alterar sua decisão acessando as configurações do aplicativo (acesso de permissão ao Provedor de TV) ou a seção de *`Settings -> TV Provider`* no iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* no tvOS.

O resultado deve criar uma experiência alinhada com os seguintes fluxos de usuário, que sugerimos consultar antes de começar a desenvolver seus aplicativos:

- [iPhone/iPad](http://tve.zendesk.com/hc/article_attachments/205624966/User_flows_AppleSSO_iOS_v2.pdf) fluxos de usuário
- [APPLE TV](http://tve.zendesk.com/hc/article_attachments/206669126/User_flows_tvOS.pdf) fluxos de usuário


>[!IMPORTANT]
>
> Quando o recurso Single Sign-On é **habilitado** para iOS/tvOS **e** no caso do Apple **Integrado (compatível) ou seletor** MVPDs os fluxos de autenticação/logout dos workflows de SSO do Apple envolvem soluções de autenticação da Apple e da Adobe Pass, enquanto todos os outros fluxos (autorização, pré-autorização, metadados etc.) será atendido exclusivamente pela Autenticação Adobe Pass.


>[!IMPORTANT]
>
> Quando o recurso Single Sign-On é **desabilitado** para iOS/tvOS **ou** no caso do Apple **não integrado (não suportado)** MVPDs os fluxos de autenticação/logout fallback dos workflows de SSO do Apple para os regulares atendidos exclusivamente pela Autenticação Adobe Pass.


>[!IMPORTANT]
>
> Um ganho principal do fluxo de trabalho do SSO do Apple é representado pelo fluxo de usuário de autenticação de uma tela, que também pode ser fornecido em TVs Apple quando o recurso de Logon único é **habilitado** para tvOS **e** no caso do Apple **Integrado (compatível)** MVPDs.


### MVPD {#MVPD}

Para se beneficiar da experiência do usuário de Logon Único (SSO), um MVPD deve:



1. Ser integrado ao fluxo de trabalho SSO do Apple no Apple. Entre em contato com a Apple para facilitar o processo de integração.
1. Forneça um aplicativo JavaScript TVML capaz de manipular o formulário de logon do usuário. Entre em contato com a Apple para receber a documentação adequada.
1. Forneça um valor de string que representa o identificador do provedor atribuído pela Apple durante o processo de integração. Entre em contato com a Autenticação Adobe Pass para executar alterações de configuração.

</br>

## Perguntas frequentes {#FAQ}

1. Caso algo dê errado com o fluxo de trabalho SSO do Apple, o aplicativo que usa o SDK do AccessEnabler iOS/tvOS pode fazer fallback para o fluxo de autenticação normal?
   - Isso é possível, mas requer que uma alteração de configuração seja executada no [Painel Adobe Primetime TVE](https://console.auth.adobe.com/). A variável *Habilitar Logon Único* deve ser definido em *NÃO* para a integração desejada (Canal x MVPD) e a plataforma desejada (iOS/tvOS).
   - O aplicativo só reconheceria a alteração de configuração depois de chamar [setRequestor](/help/authentication/iostvos-sdk-api-reference.md#setReqV3) caso esteja usando o SDK iOS/tvOS do AccessEnabler.
1. O aplicativo saberá quando uma autenticação ocorreu como resultado de um logon por meio do SSO da plataforma em outro dispositivo ou outro aplicativo?
   - Essas informações não estarão disponíveis.
1. O aplicativo saberá quando uma autenticação aconteceu como resultado de um logon por meio do SSO da plataforma no mesmo dispositivo?
   - Essas informações estão disponíveis como parte da chave de metadados do usuário: *tokenSource*, que deve retornar o valor da string: &quot;Apple&quot; neste caso.
1. O que acontece se um usuário entrar no pela *`Settings -> TV Provider`* no iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* na seção tvOS usando um MVPD que não está integrado ao aplicativo?
   - Quando o usuário inicia o aplicativo, ele não é autenticado por meio do fluxo de trabalho SSO do Apple. Portanto, o aplicativo teria que fazer fallback para o fluxo de autenticação regular e apresentar seu próprio seletor de MVPD.
1. O que acontece se um usuário entrar no pela *`Settings -> TV Provider`* no iOS/iPadOS ou *`Settings -> Accounts -> TV Provider`* na seção tvOS usando um MVPD que tenha o *Habilitar Logon Único* definido em *NÃO* no [Painel Adobe Primetime TVE](https://console.auth.adobe.com/) para a plataforma iOS/tvOS?
   - Quando o usuário inicia o aplicativo, ele não é autenticado por meio do fluxo de trabalho SSO do Apple. Portanto, o aplicativo teria que fazer fallback para o fluxo de autenticação regular e apresentar seu próprio seletor de MVPD.
1. O que acontece se um usuário tiver um MVPD que não seja integrado (não suportado) pelo Apple, mas que esteja presente no seletor de Apple?
   - Quando o usuário inicia o aplicativo, ele só seleciona o MVPD por meio do fluxo de trabalho SSO do Apple sem concluir o fluxo de autenticação. Portanto, o aplicativo teria que fazer fallback para o fluxo de autenticação regular, mas poderia usar o MVPD já selecionado.
1. O que acontece se um usuário tiver um MVPD que não seja integrado (não suportado) pelo Apple?
   - Quando o usuário inicia o aplicativo, o usuário seleciona a opção de seleção &quot;Outros provedores de TV&quot; por meio do fluxo de trabalho SSO do Apple. Portanto, o aplicativo teria que fazer fallback para o fluxo de autenticação regular e apresentar seu próprio seletor de MVPD.
1. O que acontece se um usuário tiver um MVPD que seja degradado pelo meio de [Painel Adobe Primetime TVE](https://console.auth.adobe.com/)?
   - Quando o usuário inicia o aplicativo, ele é autenticado por meio do mecanismo de degradação e não pelo fluxo de trabalho SSO do Apple.
   - A experiência deve ser perfeita para o usuário, enquanto o aplicativo será informado por meio do *N010* código de aviso caso esteja usando o SDK iOS/tvOS do AccessEnabler.
1. A ID do usuário MVPD mudará entre o SSO do Apple e o fluxo de autenticação SSO que não seja da Apple?
   - A expectativa é que a ID do usuário não seja alterada, mas precisa ser verificada para cada provedor selecionado.
1. Haverá alguma alteração nos TTLs de autenticação?
   - A Autenticação do Adobe Pass continuará a respeitar os TTLs exigidos pelos Programadores para sua integração com cada MVPD.
   - Ao navegar de um aplicativo do programador para outro aplicativo do programador por meio do Apple SSO, o segundo aplicativo terá o TTL de sua integração Programmer x MVPD correspondente (ele não compartilhará o TTL do primeiro aplicativo autenticado)

|                                      | TTL de Autenticação do Adobe Pass expirado | TTL de autenticação do Adobe Pass válido |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| **TTL de token de dispositivo do Apple expirado** | o usuário NÃO está autenticado (o seletor de MVPD deve ser exibido) | O usuário é autenticado e o TTL é o tempo restante de seu token de autenticação da Adobe Pass |
| **TTL de token de dispositivo válido da Apple** | O usuário é autenticado silenciosamente e obtém outro token de autenticação da Adobe Pass com o TTL especificado no Painel TVE | O usuário é autenticado e o TTL é o tempo restante de seu token de autenticação da Adobe Pass |

<!--

## Resources {#Resources}

- [Apple SSO Cookbook (REST API)](/help/authentication/apple-sso-cookbook-rest-api.md)
- [Apple SSO Cookbook (iOS/tvOS SDK)](/help/authentication/apple-sso-cookbook-iostvos-sdk.md)
- [Sign in with your TV provider on your iPhone, iPad, or iPod touch](https://support.apple.com/en-us/HT207035)
- [Use your pay TV or cable provider with Apple TV](https://support.apple.com/en-us/HT207035)
- [TV providers that let you sign in on your iPhone, iPad, or Apple TV](https://support.apple.com/en-us/HT208084)
- [TV Provider Authentication](https://developer.apple.com/design/human-interface-guidelines/tvos/system-capabilities/tv-provider-authentication/)
- [Apple Developer Documentation - Video Subscriber Account Framework](https://developer.apple.com/documentation/videosubscriberaccount)
-->
