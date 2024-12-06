---
title: O fluxo de direitos do programador
description: O fluxo de direitos do programador
exl-id: b1c8623a-55da-4b7b-9827-73a9fe90ebac
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1823'
ht-degree: 0%

---

# O fluxo de direitos do programador {#prog-entitlement-flow}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview}

Este documento descreve o fluxo de direitos básicos da perspectiva do programador.  Para obter informações sobre recursos e casos de uso além da integração básica da TVE abordada aqui, consulte [Casos de uso de programadores](/help/authentication/integration-guide-programmers/programmer-use-cases.md).

A Autenticação Adobe Pass é a mediadora do fluxo de direitos entre os Programadores e os MVPDs, fornecendo interfaces seguras e consistentes para ambas as partes.  Do lado do programador, a Autenticação Adobe Pass fornece dois tipos gerais de interface de direito:

1. AccessEnabler - um componente do cliente que fornece uma biblioteca de APIs para aplicativos em dispositivos que podem renderizar páginas da Web (por exemplo, aplicativos Web, aplicativos para smartphone/tablet).
2. API sem cliente - serviços Web RESTful para dispositivos que não podem renderizar páginas da Web (por exemplo, decodificadores de sinais, consoles de jogos, TVs inteligentes). O requisito para renderizar páginas da Web vem do requisito do MVPD de que os usuários se autentiquem no site do MVPD.

Além da visão geral neutra em relação à plataforma apresentada aqui, há uma visão geral específica da API sem cliente aqui: Documentação da API sem cliente. O AccessEnabler é executado nativamente nas plataformas compatíveis (AS/JS na Web, Objetive-C no iOS e Java no Android). As APIs do AccessEnabler são consistentes em todas as plataformas compatíveis. Todas as plataformas que não oferecem suporte ao AccessEnabler usam a mesma API sem clientes.

Para ambos os tipos de interface, a Autenticação do Adobe Pass é a mediadora segura do fluxo de direitos entre o aplicativo do Programador e o MVPD do usuário:

![](../assets/prog-entitlement-flow.png)


*Figura: Ecossistema De Autenticação Do Adobe Pass*

>[!IMPORTANT]
>
>Observe no diagrama acima que há uma parte do fluxo de direitos que não passa pelos servidores de autenticação da Adobe Pass: o logon MVPD. Os usuários devem fazer logon na página de logon do MVPD. Devido a esse requisito, em dispositivos que não podem renderizar páginas da Web, o aplicativo do Programador deve direcionar os usuários para alternar para um dispositivo habilitado para a Web para fazer logon com o MVPD, após o que eles retornam ao dispositivo original para o restante do fluxo de direitos.

## Fluxo de direitos {#entitlement-flow}

Existem quatro sub-fluxos distintos que constituem o direito de base
fluxo:

1. [Fluxo de inicialização](/help/authentication/integration-guide-programmers/entitlement-flow.md#startup)
1. [Fluxo de autenticação](/help/authentication/integration-guide-programmers/entitlement-flow.md#authentication)
1. [Fluxo de autorização](/help/authentication/integration-guide-programmers/entitlement-flow.md#authorization)
1. [Faixa de logoff](/help/authentication/integration-guide-programmers/entitlement-flow.md#logout)

Na visita inicial de um usuário ao site de um Programador, o fluxo de direitos continua na ordem acima. No entanto, em visitas subsequentes, dependendo de os tokens de autenticação e autorização terem expirado ou não, ou dependendo das políticas de exibição, um usuário só pode passar por um ou dois sub-fluxos.

### Fluxo de inicialização {#startup}

Estabelece a identidade do Programador e do dispositivo e executa tarefas de inicialização. Isso é um pré-requisito para todas as chamadas de direito subsequentes.

**AccessEnabler**

* **`setRequestor()`** - Estabelece sua identificação com o AccessEnalber e, por extensão, com os servidores de Autenticação da Adobe Pass. Essa chamada é um precursor do restante do fluxo de direitos. Por exemplo, no JavaScript:

  ```JavaScript
    /* Define the requestor ID (Programmer/aggregator ID). */
      var requestorID = "sample_requestor_Id";
      ...
      // Callback indicating that the AccessEnabler swf has initialized
      function swfLoaded() {
          // AccessEnabler is loaded so we can use the API function it provides
          accessEnablerObject.setRequestor(requestorID); 
      ...
      }
  ```

**API sem cliente**

* **`\<REGGIE\_FQDN\>/reggie/v1/{requestorId}/regcode`** - Dependendo da plataforma, pode haver tarefas de pré-requisito a serem concluídas antes que o aplicativo chame regcode. Consulte a **Documentação da API sem cliente** para obter detalhes. Por exemplo, as plataformas Xbox exigem que você conclua as etapas de segurança prescritas antes de chamar regcode.

### Fluxo de autenticação {#authentication}

Autenticação bem-sucedida gera um token de Autenticação vinculado ao dispositivo e ao solicitante. A autenticação bem-sucedida é um pré-requisito para a Autorização.

**AccessEnabler**

* `checkAuthentication()` - Verifica se um token de autenticação em cache válido existe no cache de token local, sem disparar o fluxo de autenticação completo. Isso aciona a função de retorno de chamada `setAuthenticationStatus()`.
* `getAuthentication()` - Inicia o fluxo de autenticação completa. Se for bem-sucedida, a Autenticação do Adobe Pass gerará um token de Autenticação e o armazenará em cache no cliente. O usuário faz logon no site MVPDs selecionado, apresentado em um iFrame, janela pop-up ou visualização da Web, dependendo da plataforma. Isso aciona displayProviderDialog().

**API sem cliente**

* `<FQDN>/.../checkauthn` - A versão do serviço Web de `checkAuthentication()` acima.
* `<FQDN>/.../config` - Retorna a lista de MVPDs para o aplicativo de segunda tela.
* `<FQDN>/.../authenticate` - Inicia o fluxo de autenticação do aplicativo de segunda tela, redirecionando usuários para o MVPD selecionado para logon. Se for bem-sucedida, a Autenticação do Adobe Pass gera um token de Autenticação e o armazena no servidor, e o usuário retorna ao dispositivo original para concluir o fluxo de direitos.

Um token de autenticação será considerado válido se os dois pontos a seguir forem verdadeiros:

* O token de autenticação não expirou
* O MVPD associado ao token de autenticação está na lista de MVPDs permitidos para a ID do solicitante atual

#### Fluxo de trabalho de autenticação inicial genérico do AccessEnabler {#generic-ae-initial-authn-flow}

1. Seu aplicativo inicia o fluxo de trabalho de autenticação com uma chamada para `getAuthentication()`, que verifica se há um token de autenticação em cache válido. Este método tem um parâmetro `redirectURL` opcional; se você não fornecer um valor para `redirectURL`, após uma autenticação bem-sucedida o usuário retornará à URL da qual a autenticação foi inicializada.
1. O AccessEnabler determina o status de autenticação atual. Se o usuário estiver autenticado no momento, o AccessEnabler chamará sua função de retorno de chamada do `setAuthenticationStatus()`, transmitindo um status de autenticação indicando sucesso.
1. Se o usuário não for autenticado, o AccessEnabler continuará o fluxo de autenticação determinando se a última tentativa de autenticação do usuário foi bem-sucedida com um determinado MVPD. Se uma ID de MVPD for armazenada em cache E o sinalizador `canAuthenticate` for verdadeiro OU um MVPD tiver sido selecionado usando `setSelectedProvider()`, o usuário não será avisado com a caixa de diálogo de seleção de MVPD. O fluxo de autenticação continua usando o valor em cache do MVPD (ou seja, o mesmo MVPD usado durante a última autenticação bem-sucedida). Uma chamada de rede é feita ao servidor de back-end e o usuário é redirecionado para a página de logon do MVPD.

1. Se nenhuma ID de MVPD estiver armazenada em cache E nenhum MVPD tiver sido selecionado usando `setSelectedProvider()` OU o sinalizador `canAuthenticate` estiver definido como falso, o retorno de chamada `displayProviderDialog()` será chamado. Esse retorno de chamada direciona o aplicativo para criar a interface do usuário, que apresenta ao usuário uma lista de MVPDs para escolher. Uma matriz de objetos MVPD é fornecida, contendo as informações necessárias para que você crie o seletor de MVPD. Cada objeto MVPD descreve uma entidade MVPD e contém informações como a ID do MVPD e o URL onde o logotipo MVPD pode ser encontrado.

1. Depois que um MVPD é selecionado, seu aplicativo deve informar o AccessEnabler da escolha do usuário. Para clientes que não sejam do Flash, depois que o usuário selecionar o MVPD desejado, você informará o AccessEnabler sobre a seleção do usuário por meio de uma chamada para o método `setSelectedProvider()`. Clientes Flash despacham um `MVPDEvent` compartilhado do tipo &quot;`mvpdSelection`&quot;, transmitindo o provedor selecionado.

1. Quando o AccessEnabler é informado da seleção de MVPD do usuário, uma chamada de rede é feita ao servidor de back-end e o usuário é redirecionado para a página de logon do MVPD.

1. No fluxo de trabalho de autenticação, o AccessEnabler se comunica com a Adobe Pass Authentication e o MVPD selecionado para solicitar as credenciais do usuário (ID do usuário e senha) e verificar sua identidade. Enquanto alguns MVPDs redirecionam para seu próprio site para o logon, outros exigem que você exiba sua página de logon em um iFrame. Sua página deve incluir o retorno de chamada que cria um iFrame, caso o cliente escolha um desses MVPDs.<!-- For more information on creating a login iFrame, see  [Managing the Login IFrame](https://tve.helpdocsonline.com/managing-the-login-iframe)-->.

1. Depois que o usuário fizer logon, o AccessEnabler recuperará o token de autenticação e informará ao aplicativo que o fluxo de autenticação foi concluído. O AccessEnabler chama o retorno de chamada `setAuthenticationStatus()` com um código de status 1, indicando sucesso. Se houver um erro durante a execução dessas etapas, o retorno de chamada `setAuthenticationStatus()` será disparado com um código de status 0, indicando falha de autenticação, bem como um código de erro correspondente.

>[!IMPORTANT]
>No momento, a Comcast é o único MVPD que não fornece um URL estático para o logotipo. Os programadores devem obter os logotipos mais recentes e atualizados do [Portal do XFINITY Developer](https://developers.xfinity.com/products/tv-everywhere).
>

### Fluxo de autorização {#authorization}

A autorização é um pré-requisito para a exibição de conteúdo protegido. Uma autorização bem-sucedida gera um token AuthZ, juntamente com um token de mídia de curta duração, fornecido ao aplicativo do programador para fins de segurança. Observe que, para oferecer suporte ao fluxo de trabalho de autorização, você deve ter executado anteriormente a configuração necessária do solicitante e integrado o [Verificador de Token de Mídia](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-token-verifier-int.md). Com elas concluídas, é possível iniciar a autorização.

Seu aplicativo inicia a autorização quando um usuário solicita acesso a um recurso protegido. Você passa uma ID de recurso especificando o recurso solicitado (por exemplo, um canal, episódio etc.). Primeiro, seu aplicativo verifica se há um token de autenticação armazenado. Se nenhum for encontrado, você iniciará o processo de autenticação.

**AccessEnabler**

* `checkAuthorization()` - Verifica a autorização sem iniciar o fluxo de autorização completo. Geralmente, isso é usado para atualizar as informações de status exibidas na interface do usuário do aplicativo Programador.

* `getAuthorization()` - Inicia o fluxo de autorização completo.

Você fornece as seguintes funções de retorno de chamada para lidar com os resultados de
a chamada de autorização:

* `setToken()` - Se a autenticação foi bem-sucedida anteriormente e a autorização teve êxito, o AccessEnabler chama sua função de retorno de chamada `setToken()`, transmitindo o token de mídia de vida curta, indicando uma conclusão bem-sucedida para o fluxo de direitos de Autenticação do Adobe Pass. (Antes de permitir que o usuário visualize o conteúdo protegido, o aplicativo do programador verifica a validade do token de mídia usando o Verificador de token de mídia.

* `tokenRequestFailed()` - Se o usuário não estiver autorizado para o recurso solicitado (ou se a consulta falhar por qualquer outro motivo), o AccessEnabler chamará essa função de retorno de chamada (além de suas próprias funções de relatório de erros), transmitindo os detalhes da falha.

**API sem cliente**

* `\<FQDN\>/.../authorize` - Inicia o fluxo de autorização completo.

#### Fluxo de trabalho de autorização genérico do AccessEnabler {#generic-ae-authr-wf}

1. Forneça uma função de retorno de chamada que registre seu GUID de programador atribuído com o Ativador de Acesso, usando `setReqestor()`. Essa função de retorno de chamada é chamada quando o AccessEnabler é
baixado com sucesso.

1. Chame `getAuthorization()` quando um usuário solicitar acesso a um recurso protegido. Usando o `getAuthorization()`, passe uma ID de recurso especificando o recurso solicitado (por exemplo, um canal, episódio etc.). O AccessEnabler procura um token de autenticação em cache a ser transmitido com a solicitação de autorização. Se um não for encontrado, ele iniciará o fluxo de autenticação.
1. Forneça funções de retorno de chamada para manipular os resultados da autorização:

   * `setToken()` - Se a autorização for bem-sucedida ou se o usuário tiver sido autorizado anteriormente, o Ativador de Acesso continuará com o processo de autorização chamando sua função de retorno de chamada `setToken()`, transmitindo o token de autorização de curta duração.

   * `tokenRequestFailed()` - Se o usuário não estiver autorizado para o recurso solicitado (ou se a consulta falhar por qualquer outro motivo), o AccessEnabler chamará qualquer função de relatório de erros que você tenha registrado, além do retorno de chamada `tokenRequestFailed()`, transmitindo detalhes sobre a falha.

### Fluxo de logout {#logout}

Limpa tokens e outros dados associados ao fluxo de direitos do usuário atual.

**AccessEnabler**

* `logout()`

**API sem cliente**

* `\<FQDN\>/.../logout`

## Entender o comportamento do AccessEnabler {#ae-behavior}

Todas as chamadas de API do AccessEnabler são assíncronas (com uma exceção, anotada nas referências da API). Você pode chamar uma API um número arbitrário de vezes, no entanto, não há uma garantia forte de que as ações acionadas
pelas chamadas serão concluídas na mesma ordem em que as chamadas foram feitas. (Uma exceção a esta regra é o tempo de execução do Flash Player atual; não sendo multi-segmentado, ele irá garantir que as chamadas *do* sejam concluídas na ordem
eles são chamados.)

Para distinguir entre respostas e poder emparelhar respostas com chamadas, todos os retornos de chamada ecoam seus parâmetros de entrada. Isso inclui `setToken()` e `tokenRequestFailed()`, que são acionados por `checkAuthorization()`. (Para retornos de chamada `checkAuthorization()`, o recurso usado é ecoado de volta.) Aproveitando esse recurso, você pode distinguir qual resposta corresponde a qual chamada. Para usar esse recurso, você pode codificar algo como o seguinte:

```JavaScript
    for each (resource in ["TNT", "CNN", "TBS", "AdultSwim"] ) {
         ae.checkAuthorization(resource);
    }
    
    // Success callback
    function setToken(resource, token) {
         // Use "resource" to figure 
         // out which checkAuthorization
         // call triggered this response
    }
    
    // Old error callback
    function tokenRequestFailed(resource, error, details) {
         // use "resource" to figure
         // out in response to which
         // checkAuthorization call
         // this was triggered
    }
    
    // Error callback using new error api
    ae.bind("errorEvent',"errorHandler");
    
    function errorHandler(error) {
         if(error.resource) {        
              // Use error.resource to figure
              // which checkAuthorization call
              // triggered this response
         }
    }
```

### Perguntas frequentes sobre comportamento do AEM {#ae-beh-faq}

**Pergunta. O que acontece se eu fizer uma segunda chamada AccessEnabler antes da conclusão da primeira chamada?**

A primeira chamada continua a ser executada conforme a segunda chamada é executada (comunicações assíncronas).

**Pergunta. Há um número máximo de chamadas simultâneas que o AccessEnabler pode suportar?**

Nenhum limite é definido explicitamente no código AccessEnabler, portanto, você está limitado apenas pelos recursos de sistema disponíveis, bem como pela capacidade do MVPD.

<!--

>[!MORELIKETHIS]
>
>*   [Programmer use cases](/help/authentication/programmer-use-cases.md)
>*   Error Reporting
>**Platform Cookbooks:**
>*   Clientless integration cookbook
>*   iOS Integration Cookbook
>*   Android Integration Cookbook
>*   JavaScript Integration Cookbook
>*   ActionScript Integration Cookbook
>*   Windows 8 Integration Cookbook
>*   AIR Native Extension Overview
-->
