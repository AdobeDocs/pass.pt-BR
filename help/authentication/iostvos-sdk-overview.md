---
title: Visão geral do SDK do iOS/tvOS
description: Visão geral do SDK do iOS/tvOS
exl-id: b02a6234-d763-46c0-bc69-9cfd65917a19
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '3704'
ht-degree: 0%

---

# Visão geral do SDK do iOS/tvOS {#iostvos-sdk-overview}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.


</br>


## Introdução {#intro}

O iOS AccessEnabler é uma biblioteca Objetive-C iOS/tvOS que permite que aplicativos móveis usem os serviços de direito da Adobe Pass Authentication for TV Everywhere. A implementação consiste na *AccessEnabler* que define a API de direito e a variável *DelegaçãodeDireitos* e *[StatusDireito](#ios%20entitlement%20status)* protocolos que descrevem os retornos de chamada que a biblioteca aciona. A interface junto com o protocolo é mencionada com um nome comum: a biblioteca AccessEnabler.

## Requisitos do iOS e do tvOS {#reqs}

Para obter os requisitos técnicos atuais relacionados à plataforma iOS e tvOS e à autenticação da Adobe Pass, consulte [Requisitos de plataforma/dispositivo/ferramenta](#ios)e consulte as notas de versão incluídas no download do SDK. Ao longo do restante desta página, você verá seções que observam alterações que se aplicam a versões específicas do SDK e superiores. Por exemplo, esta é uma observação legítima em relação ao SDK 1.7.5:

## Noções básicas sobre fluxos de trabalho de clientes nativos {#flows}

Os workflows do cliente nativo normalmente são os mesmos ou muito semelhantes aos dos clientes de Autenticação do Adobe Pass baseados em navegador. No entanto, há algumas exceções, conforme descrito abaixo.

- [Fluxo de trabalho de pós-inicialização](#post-init)
- [Fluxo de trabalho genérico de autenticação inicial](#generic)
- [Fluxo de trabalho de logout](#logout)


### Fluxo de trabalho de pós-inicialização {#post-init}

Todos os workflows de direito suportados pelo AccessEnabler presumem que você tenha chamado anteriormente [`setRequestor()`](#setReq) para estabelecer sua identidade. Você faz essa chamada para fornecer a ID do solicitante apenas uma vez, normalmente durante a fase de inicialização/configuração do aplicativo.


Com um cliente nativo do iOS, após sua chamada inicial para [`setRequestor()`](#setReq), você tem uma escolha sobre como proceder:

- Você pode começar a fazer chamadas de direito imediatamente e permitir que elas sejam enfileiradas silenciosamente, se necessário.

- Você poderá receber uma confirmação do sucesso/fracasso do [`setRequestor()`](#setReq) através da aplicação do [`setRequestorComplete()`](#setReqComplete) retorno de chamada.

- Você pode executar as duas ações acima.

É sua escolha fazer com que seu aplicativo aguarde a notificação do sucesso do [`setRequestor()`](#setReq) ou que depende do mecanismo de fila de chamadas do AccessEnabler. Como todas as solicitações de autorização e autenticação subsequentes precisam da ID do solicitante e das informações de configuração associadas, a variável [`setRequestor()`](#setReq) O método do bloqueia efetivamente todas as chamadas de API de autenticação e autorização até que a inicialização seja concluída.



### Fluxo de trabalho genérico de autenticação inicial {#generic}

A finalidade desse fluxo de trabalho é fazer logon em um usuário com seu MVPD. Após um logon bem-sucedido, o servidor de back-end emite um token de autenticação para o usuário. Embora a autenticação seja normalmente feita como parte do processo de autorização, a seguir está uma descrição de como ela pode funcionar isoladamente, sem incluir nenhuma etapa de autorização.

Observe que, embora esse fluxo de trabalho seja diferente para clientes nativos do fluxo de trabalho típico de autenticação baseada em navegador, as etapas 1 a 5 são as mesmas para clientes nativos e clientes baseados em navegador.

1. Seu aplicativo inicia o fluxo de trabalho de autenticação com uma chamada para o AccessEnabler `getAuthentication() `Método de API, que verifica se há um token de autenticação armazenado em cache válido.
1. Se o usuário estiver autenticado no momento, o AccessEnabler chamará o [`setAuthenticationStatus()`](#setAuthNStatus) função de retorno de chamada, transmitindo um status de autenticação indicando sucesso e encerrando o fluxo.
1. Se o usuário não estiver autenticado no momento, o AccessEnabler continuará o fluxo de autenticação determinando se a última tentativa de autenticação do usuário foi bem-sucedida com um determinado MVPD. Se uma ID de MVPD estiver armazenada em cache E a variável `canAuthenticate` sinalizador é verdadeiro OU um MVPD foi selecionado usando [`setSelectedProvider()`](#setSelProv), o usuário não verá a caixa de diálogo de seleção do MVPD. O fluxo de autenticação continua usando o valor em cache do MVPD (ou seja, o mesmo MVPD usado durante a última autenticação bem-sucedida). Uma chamada de rede é feita ao servidor de back-end e o usuário é redirecionado para a página de logon do MVPD (Etapa 6 abaixo).
1. Se nenhuma ID de MVPD estiver armazenada em cache E nenhum MVPD estiver selecionado usando [`setSelectedProvider()`](#setSelProv) OU a variável `canAuthenticate` for definido como falso, a variável [`displayProviderDialog()`](#dispProvDialog) o retorno de chamada é chamado. Esse retorno de chamada instrui seu aplicativo a criar a interface do usuário que apresenta ao usuário uma lista de MVPDs para escolher. Uma matriz de objetos MVPD é fornecida, contendo as informações necessárias para que você crie o seletor de MVPD. Cada objeto MVPD descreve uma entidade MVPD e contém informações como a ID do MVPD (por exemplo, XFINITY, AT\&amp;T etc.) e o URL onde o logotipo do MVPD pode ser encontrado.
1. Depois que um MVPD específico for selecionado, seu aplicativo deverá informar o AccessEnabler sobre a escolha do usuário. Depois que o usuário selecionar o MVPD desejado, você informará o AccessEnabler sobre a seleção do usuário por meio de uma chamada para o [`setSelectedProvider()`](#setSelProv) método.
1. O iOS AccessEnabler chama o `navigateToUrl:` retorno de chamada ou `navigateToUrl:useSVC:` retorno de chamada para redirecionar o usuário para a página de logon do MVPD. Ao acionar um deles, o AccessEnabler faz uma solicitação ao aplicativo para criar um `UIWebView/WKWebView or SFSafariViewController` e para carregar o URL fornecido no repositório de retorno de chamada `url` parâmetro. Este é o URL do endpoint de autenticação no servidor back-end. Para tvOS AccessEnabler, a variável [status()](#status_callback_implementation) o retorno de chamada é chamado com um `statusDictionary` e a pesquisa da segunda autenticação de tela é iniciada imediatamente. A variável `statusDictionary` contém o `registration code` que precisa ser usado para a segunda autenticação de tela.
1. No caso do iOS AccessEnabler, o usuário acessa a página de logon do MVPD para inserir suas credenciais por meio do aplicativo `UIWebView/WKWebView or SFSafariViewController `controlador. Observe que várias operações de redirecionamento ocorrem durante essa transferência e seu aplicativo deve monitorar os URLs carregados pelo controlador durante as várias operações de redirecionamento.
1. No caso do iOS AccessEnabler, quando a variável `UIWebView/WKWebView or SFSafariViewController` controlador carrega um URL personalizado específico que seu aplicativo deve fechar o controlador e chamar AccessEnabler&#39;s `handleExternalURL:url `método da API. Observe que esse URL personalizado específico é realmente inválido e não se destina ao controlador para carregá-lo. Ele só deve ser interpretado pelo aplicativo como um sinal de que o fluxo de autenticação foi concluído e que é seguro fechar o `UIWebView/WKWebView or SFSafariViewController` controlador. Caso seu aplicativo precise usar um `SFSafariViewController `controlador, o URL personalizado específico é definido pela variável `application's custom scheme` (por exemplo: `adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), caso contrário, esse URL personalizado específico será definido pela variável `ADOBEPASS_REDIRECT_URL` constante (ou seja, `adobepass://ios.app`).
1. Assim que o aplicativo fechar o `UIWebView/WKWebView or SFSafariViewController` e chama o AccessEnabler&#39;s `handleExternalURL:url `Método API, o AccessEnabler recupera o token de autenticação do servidor back-end e informa ao aplicativo que o fluxo de autenticação foi concluído. O AccessEnabler chama o [`setAuthenticationStatus()`](#setAuthNStatus) retorno de chamada com código de status 1, indicando sucesso. Se houver um erro durante a execução dessas etapas, a variável [`setAuthenticationStatus()`](#setAuthNStatus) O retorno de chamada é disparado com um código de status 0, indicando falha de autenticação, bem como um código de erro correspondente.


>[!WARNING]
>
> Durante as etapas em que o AccessEnabler renuncia ao controle do aplicativo (ou seja, quando a caixa de diálogo de seleção do provedor é exibida ou quando UIWebView/WKWebView ou SFSafariViewController é exibido), o usuário tem a oportunidade de cancelar o fluxo de autenticação. Nessas situações, seu aplicativo é responsável por informar o AccessEnabler sobre esse evento e chamar o [`setSelectedProvider()`](#setSelProv) método da API, transmitindo nulo como um parâmetro. Isso dá ao AccessEnabler a chance de limpar seu estado interno e redefinir o fluxo de autenticação. No tvOS, você pode usar o mesmo método para cancelar a pesquisa de autenticação.


### Fluxo de trabalho de logout {#logout}

Para clientes nativos, o logout é tratado de forma semelhante ao processo de autenticação descrito acima.

1. Seu aplicativo inicia o fluxo de trabalho de logout com uma chamada para o AccessEnabler `logout() `método da API. O logout é o resultado de uma série de operações de redirecionamento HTTP devido ao fato de que o usuário precisa ser desconectado dos servidores de Autenticação do Adobe Pass e também dos servidores do MVPD. Como esse fluxo não pode ser concluído com uma simples solicitação HTTP emitida pela biblioteca do AccessEnabler, um `UIWebView/WKWebView or SFSafariViewController` é necessário instanciar o controlador para poder seguir as operações de redirecionamento HTTP.

1. Um padrão semelhante ao fluxo de autenticação é empregado. O iOS AccessEnabler aciona o `navigateToUrl:` retorno de chamada ou o `navigateToUrl:useSVC:` para criar um `UIWebView/WKWebView or SFSafariViewController` e para carregar o URL fornecido no repositório de retorno de chamada `url` parâmetro. Este é o URL do ponto de extremidade de logout no servidor back-end. Para o tvOS AccessEnabler, nem o `navigateToUrl:` retorno de chamada ou o `navigateToUrl:useSVC:` o retorno de chamada é chamado.

1. À medida que passa por vários redirecionamentos, seu aplicativo deve monitorar a atividade do `UIWebView/WKWebView or SFSafariViewController `e detectar o momento em que carrega um URL personalizado específico. Observe que esse URL personalizado específico é realmente inválido e não se destina ao controlador para carregá-lo. Ela deve ser interpretada somente pelo seu aplicativo como um sinal de que o fluxo de logout foi concluído e que é seguro fechar a controladora. Quando o controlador carrega esse URL personalizado específico, seu aplicativo deve fechar o controlador e chamar o AccessEnabler&#39;s `handleExternalURL:url `método da API. Caso seu aplicativo precise usar um `SFSafariViewController `controlador, o URL personalizado específico é definido pela variável `application's custom scheme` (por exemplo,`adbe.u-XFXJeTSDuJiIQs0HVRAg://adobe.com`), caso contrário, esse URL personalizado específico será definido pela variável ` ADOBEPASS_REDIRECT_URL  `constante (ou seja, `adobepass://ios.app`).

1. No final, o AccessEnabler chamará o [`setAuthenticationStatus()`](#setAuthNStatus) retorno de chamada com código de status 0, indicando sucesso do fluxo de logout.

O fluxo de logout é diferente do fluxo de autenticação porque o usuário não precisa interagir com o `UIWebView/WKWebView or SFSafariViewController`  de qualquer forma. Portanto, o Adobe recomenda que você torne o controle invisível (ou seja, oculto) durante o processo de logout.

## Tokens {#tokens}

- [Definições e uso](#definitions)
- [Diretrizes de armazenamento em cache](#caching)
- [Persistência](#persistence)
- [Formato](#format)
- [Vinculação de dispositivo](#device_binding)


### Definições e uso {#definitions}

A solução de direito de autenticação da Adobe Pass gira em torno da geração de dados (tokens) específicos que a autenticação da Adobe Pass gera após a conclusão bem-sucedida dos fluxos de trabalho de autenticação e autorização. Esses tokens são armazenados localmente no dispositivo iOS do cliente.



Os tokens têm uma duração limitada; após a expiração, os tokens precisam ser reemitidos por meio da reinicialização dos workflows de autenticação e/ou autorização.



Há três tipos de tokens emitidos durante os workflows de direito:

- **Token de autenticação:** O resultado final do fluxo de trabalho de autenticação do usuário será um GUID de autenticação que o AccessEnabler poderá usar para fazer consultas de autorização em nome do usuário. Este GUID de autenticação terá um valor TTL (time-to-live) associado que pode ser diferente da própria sessão de autenticação do usuário. Um token de autenticação será gerado ao associar o GUID de autenticação ao dispositivo que inicia as solicitações de autenticação.
- **Token de autorização:** Concede acesso a um recurso protegido específico identificado por uma ID de recurso exclusiva. Consiste em uma concessão de autorização emitida pela parte autorizadora junto com a resourceID original. Essas informações estão vinculadas ao dispositivo que inicia a solicitação.
- **Token de mídia de vida curta:** O AccessEnabler concede acesso ao aplicativo de hospedagem para um determinado recurso, retornando um token de mídia de curta duração. Esse token é gerado com base no token de autorização adquirido anteriormente para esse recurso específico. Além disso, esse token não está vinculado ao dispositivo e a duração associada é significativamente menor (padrão: 5 minutos).

Após a autenticação e a autorização bem-sucedidas, a Autenticação do Adobe Pass emitirá tokens de autenticação, autorização e de mídia de vida curta. Esses tokens devem ser armazenados em cache no dispositivo do usuário e usados pela duração de suas vidas associadas.



### Diretrizes de armazenamento em cache {#caching}

- Token de autenticação
- Token de autorização
- Token de mídia de vida curta


#### Token de autenticação

- **AccessEnabler 1.7:** Esse SDK introduz um novo método de armazenamento de token, permitindo vários buckets de Programador-MVPD e, portanto, vários tokens de autenticação. Agora, o mesmo layout de armazenamento é usado para o cenário &quot;Autenticação por solicitante&quot; e para o fluxo de autenticação normal. A única diferença entre os dois é a maneira como a autenticação é realizada: &quot;Authentication per Requestor&quot; (Autenticação por solicitante) contém uma nova melhoria (Autenticação passiva) que possibilita que o AccessEnabler execute a autenticação de canal de retorno, com base na existência de um token de autenticação no armazenamento (para um programador diferente). O usuário só precisa se autenticar uma vez e esta sessão será usada para obter tokens de autenticação em aplicativos adicionais. Esse fluxo de canal traseiro ocorre durante o [`setRequestor()`](#setReq) e é mais transparente para o Programador. **No entanto, há um requisito importante aqui: o Programador DEVE chamar setRequestor() do thread da interface do usuário principal.**
- **AccessEnabler 1.6 e posterior:** A maneira como os tokens de autenticação são armazenados em cache no dispositivo depende do &quot;**Autenticação por solicitante&quot;** sinalizador associado ao MVPD atual:

<!-- end list -->

1. Se o recurso &quot;Autenticação por solicitante&quot; estiver desativado, um único token de autenticação será armazenado localmente na área de trabalho global; esse token será compartilhado entre todos os aplicativos integrados ao MVPD atual.
1. Se o recurso &quot;Autenticação por solicitante&quot; estiver ativado, um token será explicitamente associado ao programador que executou o fluxo de autenticação (o token não será armazenado na área de trabalho global, mas em um arquivo privado visível apenas para esse aplicativo do programador). Mais especificamente, o Logon único (SSO) entre diferentes aplicativos será desativado; o usuário precisará executar o fluxo de autenticação explicitamente ao alternar para um novo aplicativo (desde que o Programador do segundo aplicativo esteja integrado ao MVPD atual e que não exista um token de autenticação para esse Programador no cache local).



#### Token de autorização

A qualquer momento, somente UM token de autorização POR RECURSO é armazenado em cache pelo AccessEnabler. Pode haver vários tokens de autorização em cache, mas eles estão associados a recursos diferentes. Sempre que um novo token de autorização for emitido e já existir um antigo para *o mesmo recurso*, o novo token substitui o valor em cache existente.



#### Token de mídia

O token de mídia de vida curta NÃO deve ser armazenado em cache. O token de mídia deve ser recuperado do servidor sempre que uma API de autorização for chamada, pois está restrita ao uso único.



### Persistência {#persistence}

Os tokens precisam ser persistentes em execuções consecutivas do mesmo aplicativo. Isso significa que após a aquisição dos tokens de autenticação e autorização e o usuário fechar o aplicativo, os mesmos tokens ficarão disponíveis para o aplicativo quando o usuário reabrir o aplicativo. Além disso, é desejável que esses tokens sejam mantidos em vários aplicativos. Em outras palavras, depois que um usuário usa um aplicativo para fazer logon com um provedor de identidade específico (obtendo com êxito tokens de autenticação e autorização), os mesmos tokens podem ser usados por meio de um aplicativo diferente, e o usuário não recebe mais a solicitação de credenciais ao fazer logon por meio do mesmo provedor de identidade. Esse tipo de fluxo de trabalho de autenticação/autorização ininterrupta é o que torna a solução de autenticação da Adobe Pass uma implementação real do TV-Everywhere.



## iOS

A biblioteca iOS AccessEnabler contorna os problemas de compartilhamento de dados entre aplicativos ao armazenar os dados do token em uma estrutura de dados &quot;semelhante à área de transferência&quot; chamada de *colar quadro*. Esse recurso compartilhado no nível do sistema fornece os principais ingredientes que permitem a implementação do caso de uso de tokens persistentes desejado:

- **Suporte para armazenamento estruturado** - O quadro de colagem não é apenas uma estrutura de memória simples e linear semelhante a um buffer. Ele fornece um mecanismo de armazenamento semelhante a um dicionário que permite a indexação de dados com base em valores de chave especificados pelo usuário.
- **Suporte para persistência de dados usando o sistema de arquivos subjacente** - O conteúdo da estrutura do quadro de colagem pode ser marcado como persistente, nesse caso, os dados são salvos na memória interna do dispositivo.



Depois que um token específico for colocado no cache de token, sua validade será verificada em momentos diferentes pela biblioteca do AccessEnabler.  Um token válido é definido como:

- O TTL do token não expirou.
- O emissor do token está incluído na lista de provedores de identidade permitidos.



## tvOS

Como no tvOS a área de transferência não está disponível, a biblioteca tvOS AccessEnabler usa o NsUserDefaults como uma opção de armazenamento. Isso resolve o problema da persistência dos tokens de autenticação e autorização, mas as informações armazenadas não podem ser compartilhadas entre aplicativos diferentes.



**Alterações na área de trabalho do iOS 10 -** Ao atualizar de uma versão anterior do iOS, a área de trabalho será apagada. Isso significa que todos os aplicativos precisam ser reautenticados.



**Alterações na área de transferência do iOS 7 -** Devido a alterações no funcionamento das áreas de trabalho no iOS 7, haverá um SSO cruzado limitado entre os aplicativos em execução no iOS 7. Aplicativos que tenham o mesmo `<Bundle Seed ID>`(também conhecido como `<Team ID>`) compartilhará tokens, o que significa que os aplicativos A1 e A2 do mesmo Programador X compartilharão tokens, enquanto o aplicativo A1 (Programador X) e o aplicativo A3 (Programador Y) não compartilharão tokens.

- A ID de propagação do pacote/ID da equipe é a mesma entre 2 aplicativos se forem gerados pelo mesmo perfil de provisionamento. Encontre mais informações neste link:
  [http://developer.apple.com/library/ios/\#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html](http://developer.apple.com/library/ios/#documentation/general/conceptual/DevPedia-CocoaCore/AppID.html)
- Essa limitação de &quot;SSO cruzado&quot; estará presente no iOS 7, independentemente do SDK de autenticação da Adobe Pass usado.

Leia esta nota técnica para obter mais informações sobre como configurar o SSO no iOS 7 e superior (a nota técnica se aplica ao Access Enabler v1.8 e superior): <https://tve.zendesk.com/entries/58233434-Configuring-Pay-TV-pass-SSO-on-iOS>



### Armazenamento de token (AccessEnabler 1.7)

A partir do AccessEnabler 1.7, o armazenamento de token pode suportar várias combinações de Programador-MVPD, dependendo de uma estrutura de mapa aninhada de vários níveis que possa conter vários tokens de autenticação. Esse novo armazenamento não afeta a API pública do AccessEnabler de nenhuma maneira e não requer alterações por parte do programador. Veja um exemplo que ilustra essa nova funcionalidade:

1. Open App1 (desenvolvido pelo Programmer1).
1. Autentique com MVPD1 (que está integrado ao Programador1).
1. Suspender / Fechar o aplicativo atual e abrir o App2 (desenvolvido pelo Programador2).
1. Vamos supor que o Programador 2 não esteja integrado ao MVPD2; portanto, o usuário NÃO será autenticado no App 2.
1. Autentique com MVPD2 (que está integrado ao Programador2) no App2.
1. Volte para o App1; o usuário ainda será autenticado com o Programador1.

Nas versões anteriores do AccessEnabler, a Etapa 6 renderizava o usuário como não autenticado, pois o armazenamento do token anteriormente só tinha suporte para um token de autenticação.



Fazer logoff de uma sessão de Programador / MVPD limpará todo o armazenamento subjacente, incluindo todos os outros tokens de autenticação de Programador / MVPD no dispositivo. Por outro lado, cancelar o fluxo de autenticação (chamando [`setSelectedProvider(null)`](#setSelProv)) NÃO limpará o armazenamento subjacente, mas afetará somente a tentativa de autenticação atual do Programador/MVPD (apagando o MVPD do Programador atual).



### Importador de token (AccessEnabler 1.7)

Outro recurso relacionado ao armazenamento incluído no AccessEnabler 1.7 permite importar tokens de autenticação de áreas de armazenamento mais antigas. Esse &quot;Importador de tokens&quot; ajuda a obter compatibilidade entre versões consecutivas do AccessEnabler, mantendo o estado do SSO mesmo quando a versão de armazenamento é atualizada. O importador é executado durante o [`setRequestor()`](#setReq) O fluxo e o são executados nos dois cenários a seguir (supondo que nenhum token de autenticação válido para o Programador atual esteja presente no armazenamento atual):

- A primeira instalação de um aplicativo 1.7 desenvolvido por um programador específico
- Caminho de upgrade para um AccessEnabler futuro que usa um novo armazenamento

A operação de importação é transparente para o Programador e não requer nenhuma alteração de código no aplicativo cliente.



### Limpeza de token (AccessEnabler 1.7.5)

A partir do AccessEnabler 1.7.5, esse serviço é executado em [`setRequestor()`](#setReq)`. `Ele foi desenvolvido como resultado do switch iOS 7 do endereço WiFi MAC para o IDFA para rastreamento. A limpeza garante que o armazenamento atual contenha apenas tokens de autenticação válidos (válidos em termos da ID do dispositivo, antes calculada usando o endereço do MAC, antes do iOS7). O Limpador de tokens remove todos os tokens inválidos.



O serviço de Limpeza de token é mais útil quando um aplicativo AccessEnabler 1.7.5 é usado no iOS 6 e, em seguida, o usuário atualiza para o iOS 7. Quando isso acontecer, todos os tokens de autenticação obtidos no iOS 6 serão inválidos (devido ao algoritmo da ID do dispositivo, que mudou de usar o endereço do MAC para o IDFA). A limpeza apagará todos os tokens inválidos e o usuário não será autenticado.



Sem a remoção de tokens inválidos pela Limpeza de token, o AccessEnabler não conseguiria obter autorizações devido à presença de tokens de autenticação inválidos. Isso seria complicado para o usuário final depurar, já que as mensagens de erro de autorização podem ser criptografadas e não muito úteis para determinar a causa do problema.



### Formato {#format}

- [Token de autenticação](#authn_token)
- [Token de autorização](#authz_token)
- [Token de mídia curta](#short_token)
- [Vinculação de dispositivo](#device_binding)


Observe que o formato dos tokens AuthN e AuthZ é incluído aqui apenas para obter informações de fundo. A estrutura desses tokens pode ser alterada pela Autenticação Adobe Pass a qualquer momento. Os programadores não precisam saber a estrutura exata dos tokens AuthN e AuthZ para implementar seus aplicativos, pois os tokens AuthN e AuthZ não são expostos no dispositivo local. O token Short Media *é* ao aplicativo do programador.



#### Token de autenticação {#authn_token}

A listagem abaixo apresenta o formato do token de autenticação:

```
  <signatureInfo>base64(...)<signatureInfo>
  <simpleAuthenticationToken>
      <simpleTokenAuthenticationGuid>71C69B91-F327-F185-F29E-2CE20DC560F5</simpleTokenAuthenticationGuid>
      <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
      <simpleTokenDomainName>adobe.com</simpleTokenDomainName>
      <simpleTokenExpires>2011/03/19 02:29:34 GMT +0200</simpleTokenExpires>
      <simpleTokenMsoID>Adobe</simpleTokenMsoID>
      <simpleTokenDeviceID>
          <simpleTokenFingerprint>
              HASH(true device identification info)
          </simpleTokenFingerprint>
      </simpleTokenDeviceID>   
  </simpleAuthenticationToken>
```


#### Token de autorização {#authz_token}

A lista abaixo apresenta o formato do token de autorização:

```
  <signatureInfo>base64(...)<signatureInfo>
  <simpleAuthorizationToken>
      <simpleTokenRequestorID>TEST_REQUESTOR</simpleTokenRequestorID>
      <simpleTokenResourceID>TEST_RESOURCE</simpleTokenResourceID>
      <simpleTokenTTL>2011/03/17 14:40:08 GMT +0200</simpleTokenTTL>
      <simpleTokenMsoID>Adobe</simpleTokenMsoID>
      <simpleTokenDeviceID>
          <simpleTokenFingerprint>
              HASH(true device identification info)
          </simpleTokenFingerprint>
      </simpleTokenDeviceID>
  </simpleAuthorizationToken>
```


#### Token de mídia curta {#short_token}

A lista abaixo apresenta o formato do token de mídia curta. Esse token é exposto ao aplicativo do Programador. Ele é transferido para o aplicativo do Programador ao final de um processo de qualificação bem-sucedido:

```
  <signatureInfo>signature<signatureInfo>
  <shortAuthorizationToken>
    <sessionGUID>session_guid</sessionGUID>
    <requestorID>requestor_id</requestorID>
    <resourceID>resource_id</resourceID>
    <ttl>ttl_in_ms</ttl>
    <issueTime>issue_time</issueTime>
    <mvpdId>mvpd_id</mvpdId>
    <proxyMvpdId>proxy_mvpd_id</proxyMvpdId>
  </shortAuthorizationToken>
```


### Vinculação de dispositivo {#device_binding}

Nas listagens XML acima, observe a tag intitulada `simpleTokenFingerprint`. A finalidade dessa tag é manter informações de individualização de ID de dispositivo nativas. A biblioteca do AccessEnabler pode obter essas informações de individualização e disponibilizá-las para os serviços de autenticação da Adobe Pass durante as chamadas de qualificação. O serviço usará essas informações e as incorporará aos tokens reais, vinculando, assim, efetivamente os tokens a um dispositivo específico. O objetivo final disso é tornar os tokens intransferíveis entre dispositivos.



Como esse é obviamente um recurso relacionado à segurança, essas informações são inerentemente &quot;confidenciais&quot; do ponto de vista da segurança. Como resultado, essas informações precisam ser protegidas contra violações e espionagem. O problema de espionagem é resolvido enviando as solicitações de autenticação/autorização pelo protocolo HTTPS. A proteção contra violação é tratada assinando digitalmente as informações de identificação do dispositivo. A biblioteca do AccessEnabler calcula uma ID de dispositivo a partir das informações fornecidas pelo dispositivo e, em seguida, envia a ID de dispositivo &quot;em branco&quot; para os servidores de autenticação da Adobe Pass como um parâmetro de solicitação. Os servidores de Autenticação do Adobe Pass assinam digitalmente a ID do dispositivo com a chave privada Adobe e a adicionam ao token de autenticação retornado ao AccessEnabler. Assim, a ID do dispositivo é vinculada ao token de autenticação. Durante o fluxo de autorização, o AccessEnabler envia novamente a ID do dispositivo sem criptografia, juntamente com o token de autenticação. A falha do processo de validação resultará automaticamente na falha dos workflows de autenticação/autorização. Os servidores de Autenticação do Adobe Pass aplicam a chave privada à ID do dispositivo e a comparam com o valor no token de autenticação. Se não corresponderem, o fluxo de direitos falhará.



**Observação sobre a ligação de dispositivos no AccessEnabler 1.7.5:** A partir do AccessEnabler 1.7.5, há uma mudança em como a ID do dispositivo é calculada para fornecer informações de individualização para um dispositivo iOS. Esta alteração reflete uma mudança no iOS 7: a partir do iOS 7, a Apple não mais fornece o endereço WiFi MAC como uma opção de rastreamento, em favor do seu Identificador para anunciantes (IDFA). Como as informações de individualização de um aplicativo executado no iOS 7 são baseadas no IDFA e essas informações são incorporadas aos tokens de fluxo de direito, isso significa que há vários efeitos possíveis diferentes na experiência do usuário que resultam dessa alteração. Os diferentes efeitos possíveis são baseados em qual versão do iOS o usuário está atualizando combinada com qual versão do AccessEnabler o programador está atualizando. Para obter detalhes sobre essa alteração, consulte as Notas de versão incluídas com o AccessEnabler SDK 1.7.5.

<!--
## Related Information {#related}


- [iOS/tvOS Integration Cookbook](#)
- [iOS/tvOS API Reference](#)
- [Handling MVPDs with 'Not Trusted Certificates' in Adobe Pass
  authentication native SDK (Tech Note)](#)
- [Registering Native Clients](#)
- [Generating Digital Certificates](#)
- [Understanding Tokens](#understanding_tokens)
- [Identifying Protected Resources](#)
- [SSO on iOS when using the Adobe Pass Authentication Access
  Enabler](#)
-->
