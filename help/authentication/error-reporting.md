---
title: Relatório de erros
description: Relatório de erros
exl-id: a52bd2cf-c712-40a2-a25e-7d9560b46ba6
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '2989'
ht-degree: 2%

---

# Relatório de erros {#error-reporting}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.


## Visão geral {#overview}

O relatório de erros na Autenticação do Adobe Pass está implementado atualmente de duas maneiras diferentes:

* **Relatório de Erros Avançado** O implementador registra um retorno de chamada de erro no caso do [SDK JavaScript do AccessEnabler](#accessenabler-javascript-sdk) ou implementa um método de Interface chamado &quot;`status`&quot; no caso do [SDK iOS/tvOS do AccessEnabler](#accessenabler-ios-tvos-sdk) e do [SDK Android do AccessEnabler](#accessenabler-android-sdk), para receber relatórios de erros avançados. Os erros são categorizados em **Informações**, **Aviso** e **Erro** tipos. Este sistema de relatório é **assíncrono**, pois **não há garantia da ordem em que vários erros serão acionados**.  Para obter detalhes sobre o sistema avançado de relatório de erros, consulte a seção [Relatório de Erros Avançado](#advanced-error-reporting).

* **Relatório de Erros Original -** Um sistema de relatório estático no qual mensagens de erro são passadas para funções de retorno de chamada específicas quando solicitações específicas falham. Os erros são agrupados em tipos genéricos, de autenticação e de autorização. Para obter a lista de erros relatados no sistema original, consulte a seção [Relatório de Erros Original](#original-error-reporting).


## Relatório de Erros Avançado {#advanced-error-reporting}

* [SDK do JavaScript do AccessEnabler](#accessenabler-javascript-sdk)
* [AccessEnabler iOS/tvOS SDK](#accessenabler-ios-tvos-sdk)
* [SDK do Android do AccessEnabler](#accessenabler-android-sdk)
* [SDK FireOS do AccessEnabler](#accessenabler-fireos-sdk)

>[!IMPORTANT]
>
>A antiga API [Relatório de Erros Original](#original-error-reporting) continuará funcionando como antes. O relatório de erros avançado não interrompe a funcionalidade, mas o relatório de erros original NÃO receberá mais nenhuma atualização. Todos os novos erros e atualizações ocorrerão no sistema avançado de relatório de erros.

### SDK do JavaScript do AccessEnabler {#accessenabler-javascript-sdk}

O novo sistema de relatório de erros é opcional, portanto, o implementador pode registrar explicitamente um retorno de chamada de manipulador de erros para receber relatórios de erros avançados. O sistema inclui a capacidade de registrar e cancelar o registro de vários retornos de chamada de erro dinamicamente. Além disso, você pode registrar novos retornos de chamada de erro assim que o SDK JavaScript do AccessEnabler for carregado, sem a necessidade de executar outra inicialização (antes de chamar `setRequestor()`), o que significa que você tem a capacidade de receber relatórios avançados sobre erros de inicialização e configuração.


#### Implementação {#access-enab-js-imp}

yourErrorHandler(errorData:Object)


Sua função de retorno de chamada do manipulador de erros receberá um único objeto (um mapa) com a seguinte estrutura:

```JavaScript
    {
      errorId: "CFG410",
      level: "error",
      message: "This a fancy message",  // Optional
      .
      .                                 // Optional key/value pairs
      .
    }
```

### 1. Vincular {#bind}

**`.bind(eventType:String, handlerName:String):void`**

Anexa um manipulador para um evento.

**`eventType`** - SOMENTE o valor &quot;`errorEvent`&quot; resulta no SDK JavaScript do AccessEnabler acionando retornos de chamada de relatórios de erro avançados.

**`handlerName`** - uma cadeia de caracteres especificando o nome da função do manipulador de erros.


Ambos os parâmetros de associação devem usar apenas caracteres do seguinte conjunto: `[0-9a-zA-Z][-._a-zA-Z0-9]`; ou seja, os parâmetros devem começar com um número ou uma letra e podem incluir apenas hifens, pontos, sublinhados e caracteres alfanuméricos.  Além disso, os parâmetros não podem exceder 1024 caracteres.

**Exemplo** de manipuladores de erro de associação:

```JavaScript
accessEnabler.bind('errorEvent', 'myCustomErrorHandler');
accessEnabler.bind('errorEvent', 'errorLogger');
```

Devido a limitações técnicas, você não pode vincular um fechamento ou uma função anônima. Você deve especificar o nome do método no segundo parâmetro.


### 2. Desvincular {#unbind}

**`.unbind(eventType:String, handlerName:String=null):void`**

Remove um manipulador de eventos anexado anteriormente.

**`eventType`** - SOMENTE o valor &#39;`errorEvent`&#39; resulta no SDK JavaScript do AccessEnabler disparando retornos de chamada de relatórios de erro avançados.

**`handlerName`** - uma cadeia de caracteres especificando o nome da função do manipulador de erros; se for nula, ou estiver faltando, todos os manipuladores anexados para o `eventType` especificado serão removidos.

Ambos os parâmetros de associação devem usar apenas caracteres do seguinte conjunto: `[0-9a-zA-Z][-._a-zA-Z0-9]`; ou seja, os parâmetros devem começar com um número ou uma letra e podem incluir apenas hifens, pontos, sublinhados e caracteres alfanuméricos.  Além disso, os parâmetros não podem exceder 1024 caracteres.

**Exemplo** de remoção de um único manipulador de erros:

`accessEnabler.unbind('errorEvent', 'errorLogger');`

**Exemplo** removendo todos os manipuladores de erro:

`accessEnabler.unbind('errorEvent');`


### AccessEnabler iOS/tvOS SDK {#accessenabler-ios-tvos-sdk}

O novo sistema de relatório de erros é obrigatório, portanto, o implementador deve estar em conformidade explícita com o novo protocolo &quot;EntitlementStatus&quot; do Objetive C. Essa nova abordagem permite que os programadores recebam relatórios avançados de erros.

#### Implementação {#accessenab-ios-tvossdk-imp}

Um implementador precisa estar em conformidade com o seguinte protocolo **EntitlementStatus**:

**StatusDireito.h**

```OBJ-C
    #import <Foundation/Foundation.h>
     
    @protocol EntitlementStatus <NSObject>
    - (void)status:(NSDictionary *)statusDictionary;
    @end
```

Sua função **status** receberá um único objeto (um `NSDictionary`) com a seguinte estrutura:

```OBJ-C
    {
          errorId: "CFG410",
          level: "error",
          message: "This a fancy message",  // Optional
      .
      .                                 // Optional key/value pairs
      .
    }
```

**1. Declaração**

```OBJ-C
    @interface MyEntitlementStatusDelegate : NSObject <EntitlementStatus>
```

**2. Implementação**

```OBJ-C
    @implementation DemoAppAppDelegate     
    // very simple implementation
    - (void)status:(NSDictionary *)statusDict {
        NSLog(@"%@:\n%@", statusDict[@"level"], statusDict);
    }
```


### SDK do Android do AccessEnabler {#accessenabler-android-sdk}

O novo sistema de relatório de erros é obrigatório porque o implementador deve estar em conformidade explícita com o protocolo definido pela interface `IAccessEnablerDelegate`. Essa nova abordagem permite que os programadores recebam relatórios avançados de erros.

#### Implementação {#access-enablr-androidsdk-imp}

Um implementador precisa manipular o novo método `status` da interface`IAccessEnablerDelegate`. A função **`status`** receberá um único objeto **`AdvancedStatus`** com o seguinte modelo:

```C++
    class AdvancedStatus {
    
    String id; // Not Null
    String level; // Not Null
    String message; // Might be null
    String resource; // Might be null

    AdvancedStatus(String id, String level, String message, String resource) {
        this.id = id;
        this.level = level;
        this.message = message;
        this.resource = resource;
    } 
    
    //other private/public methods
    }
```

**Amostra**

```C++
    @Override
    public void status(AdvancedStatus advancedStatus) {
        String status = "status(" + advancedStatus.id + ", " + advancedStatus.level + ", " + advancedStatus.message + ", " + advancedStatus.resource + ")";
    
        Log.i(LOG_TAG, status);
    }
```

### SDK FireOS do AccessEnabler {#accessenabler-fireos-sdk}


O novo sistema de relatório de erros é obrigatório porque o implementador deve estar em conformidade explícita com o protocolo definido pela interface `IAccessEnablerDelegate`. Essa nova abordagem permite que os programadores recebam relatórios avançados de erros.

#### Implementação {#access-enab-fireos-sdk-}

Um implementador precisa lidar com o novo método `status`da interface`IAccessEnablerDelegate`. A função **`status`** receberá um único objeto **`AdvancedStatus`** com o seguinte modelo:

```C++
    class AdvancedStatus {
    
    String id; // Not Null
    String level; // Not Null
    String message; // Might be null
    String resource; // Might be null

    AdvancedStatus(String id, String level, String message, String resource) {
        this.id = id;
        this.level = level;
        this.message = message;
        this.resource = resource;
    } 
    
    //other private/public methods
    }
```

**Amostra**

```C++
    @Override
    public void status(AdvancedStatus advancedStatus) {
        String status = "status(" + advancedStatus.id + ", " + advancedStatus.level + ", " + advancedStatus.message + ", " + advancedStatus.resource + ")";
    
        Log.i(LOG_TAG, status);
    }
```

## Referência de códigos de erro avançada {#advanced-error-codes-reference}

A tabela a seguir lista e descreve os códigos de erro expostos pela API de erro mais recente, juntamente com as ações sugeridas para corrigi-los:

| ID | Nível | Descrição | Ações do desenvolvedor | Ações do usuário | JavaScript | iOS/tvOS | Android |
|---|-------------|------------|----------------|---|---|---|---|
| APL&amp; APL_ERROR | Erro | Erro genérico de SSO do Apple | O erro contém um campo de detalhes com o erro do VSA original. | n/d | n/d | Sim | n/d |
| VSA203 | Informações | O usuário decidiu fazer logout do aplicativo enquanto a autenticação ocorria como resultado de um logon por meio do SSO da plataforma. | Instrua/solicite que o usuário saia explicitamente do menu Configurações -> Contas -> Provedor de TV no tvOS. <br><br> Instrua/solicite que o usuário saia explicitamente de Configurações -> Provedor de TV no iOS/iPadOS. | Saia explicitamente do Settings -> Accounts -> TV Provider no tvOS. <br> <br> Sair explicitamente em Configurações -> Provedor de TV no iOS/iPadOS | n/d | Sim | n/d |
| VSA404 | Informações | A permissão da Conta de Assinante de Vídeo do Aplicativo não foi determinada. | Incentifique os usuários que se recusam a conceder permissão para acessar informações de assinatura explicando os benefícios da experiência do usuário de Logon único (SSO). | O usuário pode alterar sua decisão acessando as configurações do aplicativo (acesso ao Provedor de TV) ou a seção de Configurações -> Provedor de TV no iOS/iPadOS ou Configurações -> Contas -> Provedor de TV no tvOS. | n/d | Sim | n/d |
| VSA503 | Informações | Falha na solicitação de metadados de Conta de Assinante de Vídeo do Aplicativo. | O ponto de extremidade MVPD não está respondendo. O aplicativo pode fazer fallback para o fluxo de autenticação normal. | n/d | n/d | Sim | n/d |
| 500 | Erro | Erro interno | Use AccessEnablerDebug e inspecione logs de depuração (saída console.log) para determinar o que deu errado. | n/d | Sim | Sim | n/d |
| SEC403 | Erro | Erro de Segurança de Domínio. O solicitante está usando um domínio inválido. Todos os domínios usados por uma ID de solicitante específica precisam ser colocados na lista de permissões pelo Adobe. | - Carregar o AccessEnabler somente da lista de domínios permitidos <br> <br> - Entre em contato com o Adobe para gerenciar a lista de permissões do domínio para a ID do Solicitante usada <br> <br> - iOS: verifique se você está usando o certificado correto e se a assinatura foi criada corretamente | n/d | n/d | Sim | n/d |
| SEC412 | Aviso | [Disponível na Versão 2.5] A ID do Dispositivo não corresponde. Isso pode acontecer sempre que a plataforma subjacente alterar a ID do dispositivo. Nesse caso, os tokens existentes serão apagados e o usuário não será mais autenticado. Observe que isso está acontecendo legitimamente quando o usuário está usando o SDK do JS e está em roaming (no JS, o IP do cliente faz parte da ID do dispositivo). Caso contrário, isso pode ser uma indicação de uma tentativa de fraude - uma tentativa de copiar tokens de um dispositivo diferente. | - Monitore o número de avisos. Se o pico não ocorrer por um motivo aparente (nenhuma atualização recente do navegador; novos sistemas operacionais), isso pode ser um indicador de tentativas de fraude.  <br> <br>- Opcionalmente, informe ao usuário que ele precisa fazer logon novamente. | Faça logon novamente. | Sim | Sim | Sim, de 3.2 |
| SEC420 | Erro | Erro de Segurança HTTP ao se comunicar com os servidores de Autenticação do Adobe Pass. Normalmente, esse erro ocorre quando a falsificação/proxies está em vigor. | - Carregue `[https://]{SP_FQDN\}` no navegador e aceite manualmente os certificados SSL, por exemplo, **https://api.auth.adobe.com** ou **https://api.auth-staging.adobe.com** <br> <br>- Marcar os certificados de proxy como confiáveis | Se isso ocorre para um usuário comum, é uma indicação de um possível ataque do tipo &quot;man-in-the-middle&quot;! | Sim | Sim | Sim, de 3.2 |
| CFG100 | Aviso | A data/hora/fuso horário do computador cliente não está definida corretamente. Isso provavelmente levará a erros de autenticação/autorização. | - Informe o usuário para definir a hora correta. <br> <br> Execute ações para evitar fluxos de direitos, pois eles provavelmente falharão. | Defina a data/hora correta. | Sim | Sim | Sim, de 3.2 |
| CFG400 | Erro | Uma ID de Solicitante inválida foi fornecida. | O desenvolvedor DEVE especificar uma ID de Solicitante válida. | n/d | Sim | Sim | Sim, de 3.2 |
| CFG404 | Erro | Os servidores de Autenticação do Adobe Pass não foram encontrados. Isso pode acontecer em 3 instâncias: <br><br> - O desenvolvedor tem uma falsificação inválida em vigor. <br><br> -O usuário tem problemas de rede e não pode acessar os domínios de Autenticação Adobe Pass. <br><br> -Os servidores de Autenticação Adobe Pass estão configurados incorretamente. <br><br>  **Observação:** no Firefox, o CFG400 será exibido em vez do CFG404 (limitação do navegador) | - Verifique a falsificação. <br><br> -Verificar configurações de rede/DNS. <br><br> -Adobe de informação. | Verifique as configurações de rede/DNS. | Sim | Sim | Sim, de 3.2 |
| CFG410 | Erro | O AccessEnabler é muito antigo. | Informe o usuário para limpar os caches. | Limpe o cache do navegador. | Sim | n/d | Sim, de 3.2 |
| CFG5xx | Erro | Os servidores de Autenticação do Adobe Pass estão apresentando erros internos. xx pode ser qualquer número. | - Informar o usuário sobre a indisponibilidade da Autenticação do Adobe Pass. <br><br> - Ignorar Autenticação Adobe Pass. <br> <br> - Informar Adobe. | Tente mais tarde. | Sim | Sim | Sim, de 3.2 |
| N000 | Informações | O usuário não está autenticado. | n/d | Logon. | Sim | Sim | Sim, de 3.2 |
| N001 | Informações | Uma tentativa de autenticação passiva foi iniciada em segundo plano. Isso ocorrerá para MVPDs configurados com &quot;Autenticação por Solicitante&quot;. Embora o usuário seja autenticado automaticamente, isso afeta o desempenho na inicialização. | Opcionalmente, informe ao usuário, ou apresente uma interface que alerta o usuário, que &quot;o trabalho está em andamento&quot;. | Espere. | Sim | Sim | Sim, de 3.2 |
| N003 | Informações | O usuário seleciona a opção &quot;Outro provedor de TV&quot; no seletor Apple MVPD. | O retorno de chamada *displayProviderDialog* será chamado e o aplicativo poderá fazer fallback para o fluxo de autenticação regular. | Selecione o MVPD normal e continue com a tela de login. | n/d | Sim | n/d |
| N004 | Informações | O usuário seleciona um Provedor de TV que não é suportado pelo solicitante atual. | O retorno de chamada *displayProviderDialog* será chamado e o aplicativo poderá fazer fallback para o fluxo de autenticação regular. | Selecione o MVPD normal e continue com a tela de login. | n/d | Sim | n/d |
| N005 | Informações | O seletor de MVPD foi cancelado. | n/d | n/d | Sim | Sim | Sim, de 3.2 |
| N010 | Aviso | O usuário foi autenticado enquanto a regra de degradação autenticar tudo estava em vigor para o MVPD selecionado. | Opcionalmente, informe ao usuário que ele está recebendo acesso gratuito devido a dificuldades com o MVPD. | n/d | Sim | Sim | Sim, de 3.2 |
| N011 | Informações | O usuário foi autenticado usando TempPass. | - Informe o usuário. <br> <br> - Opcionalmente, apresentar uma lista de MVPDs comuns. | Opcionalmente, faça logon com o MVPD normal. | Sim | Sim | Sim, de 3.2 |
| N111 | Aviso | TempPass Expirado. | - Informar o usuário. <br> <br> - Apresentar uma lista de MVPDs comuns. <br> <br> - Ocultar a opção TempPass. | Faça logon com o MVPD normal. | Sim | Sim | Sim, de 3.2 |
| N130 | Erro | **Token de autenticação não encontrado na sessão.** Isso pode ser devido a um dos seguintes motivos: <br> <br> 1. O navegador tem cookies (de terceiros) desabilitados (Não se aplica ao SDK JavaScript do AccessEnabler versão 4.x) <br> <br> 2. O navegador tem Impedir rastreamento entre sites habilitado (Safari 11+) <br> <br> 3. A sessão expirou <br> <br> 4. Programador chama APIs de autenticação em sucessão incorreta <br> <br> OBSERVAÇÃO: este código de erro não está disponível para fluxos de autenticação de redirecionamento de página inteira. | 1. Avisar usuário para habilitar cookies (de terceiros) <br> <br> 2. Avisar usuário para desabilitar o rastreamento entre sites <br> <br> 3. Avisar usuário para autenticar novamente <br> <br> 4. Chamar APIs na ordem correta | 1. Habilitar cookies (de terceiros) <br> <br> 2. Desabilitar rastreamento entre sites <br> <br> 3. Reautenticar <br> <br> 4. N/A | Sim | Sim | Sim, de 3.2 |
| N500 | Erro | Erro interno. <br> <br> Observação: este é o &quot;Erro de Autenticação Genérico&quot; e o &quot;Erro de Autenticação Interna&quot; do sistema de erro original. Esse erro acabará sendo eliminado. | Use AccessEnablerDebug e inspecione logs de depuração (saída console.log) para determinar o que deu errado. | n/d | Sim | Sim | n/d |
| R401 | Erro | Erro ao tentar obter um token de acesso. <br> <br> Observação: este é um erro irrecuperável. Informe ao usuário que o aplicativo não está disponível. | - iOS: verifique a declaração do software e os esquemas personalizados em seu aplicativo. <br> <br> - JavaScript: verifique a instrução de software no aplicativo do site. <br> <br> Abra um tíquete usando o Zendesk e informe ao usuário que o sistema está temporariamente indisponível | n/d | Sim da v4.0 | Sim da v3.0 | Sim, de 3.2 |
| R400 | Erro | O aplicativo não está registrado. A instrução de software é inválida ou foi revogada. <br> <br> Observação: este é um erro irrecuperável. Informe ao usuário que o aplicativo não está disponível. | - iOS: verifique a declaração do software e os esquemas personalizados em seu aplicativo. <br> <br> - JavaScript: verifique a instrução de software no aplicativo do site. <br> <br> Abra um tíquete usando o Zendesk e informe ao usuário que o sistema está temporariamente indisponível | n/d | Sim da v4.0 | Sim da v3.0 | Sim, de 3.2 |
| REG500 | Erro | O código de registro não pôde ser obtido do servidor. <br> <br> Observação: este é um erro irrecuperável. Informe ao usuário que o aplicativo não está disponível. | Abra um ticket usando o Zendesk e informe ao usuário que o sistema está temporariamente indisponível. | n/d | Sim da v4.0 | Sim da v3.0 | Sim, de 3.2 |
| REGCODE | Sucesso | O aplicativo chamou a API setSelectedProvider na plataforma tvOS. | Instrua/avise o usuário a usar um segundo dispositivo (tela) para fazer logon usando o código de registro fornecido. | Use o regcode em um segundo dispositivo (tela) para iniciar a autenticação. | n/d | Sim Somente para tvOS | n/d |
| Z010 | Aviso | O usuário foi autorizado enquanto a regra de degradação autenticar tudo ou autorizar tudo estava em vigor para o MVPD selecionado. | Opcionalmente, informe ao usuário que ele está recebendo acesso gratuito devido a dificuldades com o MVPD. | n/d | Sim | Sim | Sim, de 3.2 |
| Z011 | Informações | O usuário foi autorizado a usar o TempPass | Opcionalmente, informar o usuário | n/d | Sim | Sim | Sim, de 3.2 |
| Z100 | Erro | Falha na autorização porque o usuário não tem assinatura para o recurso solicitado ou devido a outros motivos originados do MVPD, por exemplo, o vídeo não corresponde às configurações de Controle dos Pais da conta de usuário | - Não permitir reprodução. <br> <br> - Informar o usuário. <br> <br> - A chave &#39;message&#39; na mensagem de erro PODE conter uma mensagem mais detalhada fornecida pelo MVPD. | n/d | Sim | Sim | Sim, de 3.2 |
| Z110 | Erro | Autorização negada devido a recusas repetidas de MVPD. Possível tentativa de fraude ou DOS. | - Não permitir reprodução. <br> <br> - Informar o usuário. | n/d | Sim | Sim | Sim, de 3.2 |
| Z120 | Erro | Autorização negada devido a motivos técnicos na comunicação com o MVPD. Possível erro de rede. | - Não permitir reprodução. <br> <br> - Informe ao usuário que o MVPD enfrentou dificuldades e que ele deve tentar mais tarde. | Tente mais tarde. | Sim | Sim | Sim, de 3.2 |
| Z130 | Erro | Autorização negada porque um recurso inválido/malformado foi usado. | Verifique a cadeia de caracteres do recurso e corrija-a. Geralmente, esse erro é devido a um MRSS malformado ou ao uso de uma sequência de caracteres simples em vez de MRSS. | n/d | Sim | Sim | Sim, de 3.2 |
| Z169 | Erro | Autorização negada porque a regra de degradação authzNone foi aplicada ao recurso especificado. | Informar o usuário | n/d | Sim | Sim | Sim, de 3.2 |
| Z500 | Erro | Erro interno. <br> <br> Observação: este é o &quot;Erro de Autenticação Genérico&quot; e o &quot;Erro de Autenticação Interno&quot; herdados. Esse erro acabará sendo eliminado. | Use AccessEnablerDebug e inspecione logs de depuração (saída console.log) para determinar o que deu errado. | n/d | Sim | Sim | Sim, de 3.2 |
| P100 | Erro | Falha na pré-autorização. Provavelmente, isso se deve à solicitação de autorização para muitos recursos. | - NÃO use mais do que o número máximo de recursos permitidos. <br> <br> - Contate o suporte de Autenticação da Adobe Pass para localizar/configurar o número máximo de recursos permitidos. | n/d | Sim da v3.0 | Sim | Sim, de 3.2 |
| IS2XX | Erro | Esses códigos de erro são retornados quando os dados de resposta do endpoint do servidor de individualização têm um formato inválido ou não contêm as informações de individualização necessárias. | Abra um ticket usando o Zendesk e informe ao usuário que o sistema está temporariamente indisponível | n/d | Sim da v3.0 | n/d | n/d |
| IS4XX | Erro | Esses códigos de erro são retornados em caso de falha de endpoint de servidor de individualização 4XX - é o código de status HTTP da resposta. | Abra um ticket usando o Zendesk e informe ao usuário que o sistema está temporariamente indisponível | n/d | Sim da v3.0 | n/d | n/d |
| IS5XX | Erro | Esses códigos de erro são retornados no caso de falha de ponto de extremidade do servidor de individualização 5XX - é o código de status HTTP da resposta. | Abra um ticket usando o Zendesk e informe ao usuário que o sistema está temporariamente indisponível | n/d | Sim da v3.0 | n/d | n/d |
| IS0 | Erro | Esse código é retornado quando o endpoint do servidor de individualização não responde, portanto, a conexão expirou | Abra um ticket usando o Zendesk e informe ao usuário que o sistema está temporariamente indisponível | n/d | Sim da v3.0 | n/d | n/d |
| LS011 | Aviso | O AccessEnabler está usando um armazenamento volátil devido a problemas de LSO/LocalStorage e de WebStorage (ou indisponibilidade). <br> <br> A autenticação/autorização não persiste além da página atual. Cada carregamento de página resultará na autenticação do usuário. Os TTLs configurados não são aplicados em recarregamentos de página. | - Informe o usuário sobre as limitações. <br> <br> - Informar ao usuário como aumentar o espaço de armazenamento disponível. <br> <br> - Alternativamente, faça logout para limpar o armazenamento. | - Aumentar o armazenamento. <br> <br> - Faça logoff para limpar o armazenamento. | Sim | n/d | n/d |

<br>

## Relatório de Erros Original {#original-error-reporting}

Esta seção descreve o sistema original de relatório de erros, juntamente com os códigos de erro originais. No sistema original de relatório de erros, o AccessEnabler transmite erros para essas duas funções de retorno de chamada: `setAuthenticationStatus()` após uma chamada para `checkAuthentication()`; `tokenRequestFailed()`, após a falha de uma chamada para `checkAuthorization()` ou `getAuthorization()`.

O relatório de erros original e as APIs de status continuam a funcionar exatamente como antes. No entanto, avançar as APIs originais do relatório de erros não será atualizado. Todos os novos relatórios de erros e atualizações dos erros antigos serão refletidos SOMENTE no novo [sistema de Relatório de Erros Avançado](#advanced-error-reporting).


Para obter exemplos de uso do sistema original de relatório de erros, consulte as funções [Referência da API JavaScript](/help/authentication/javascript-sdk-api-reference.md):[setAuthenticationStatus()](/help/authentication/javascript-sdk-api-reference.md#set-authn-status-isauthn-error) e [tokenRequestFailed()](/help/authentication/javascript-sdk-api-reference.md#token-request-failed-error-msg), [Referência da API iOS/tvOS](/help/authentication/iostvos-sdk-api-reference.md): [setAuthenticationStatus()](/help/authentication/javascript-sdk-api-reference.md#setAuthNStatus)e [tokentRequestFailed()](/help/authentication/javascript-sdk-api-reference.md#tokenReqFailed), [Referência da API Android](/help/authentication/android-sdk-api-reference.md): [setAuthenticationStatus()](/help/authentication/android-sdk-api-reference.md#setAuthNStatus) e [ 6}tokenRequestFailed()](/help/authentication/android-sdk-api-reference.md#setAuthNStatus#tokenRequestFailed).

### Códigos de erro de retorno de chamada originais {#original-callback-error-codes}

| **Erros Genéricos** | |
|---|---|
| Erro interno | Ocorreu um erro no sistema ao tentar processar a solicitação. |
| Erro de provedor não selecionado | Ocorre quando o cliente cancela a caixa de diálogo de seleção do provedor. |
| Erro de provedor não disponível | Ocorre quando nenhum provedor está disponível. |
|  |  |
| **Erros de Autenticação** | |
| Erro de autenticação genérica | Retornado quando o motivo não é conhecido ou não pode ser publicado. |
| Erro de autenticação interna | Ocorreu um erro no sistema durante a tentativa de autenticação. |
| Erro de usuário não autenticado | Usuário não autenticado. |
|  |  |
| **Erros de Autorização** |  |
| Erro de autorização genérico | Retornado quando o motivo não é conhecido ou não pode ser publicado. |
| Erro de autorização interna | Ocorreu um erro de sistema ao tentar autorizar. |
| Erro de usuário não autorizado | O cliente não está autorizado a visualizar o conteúdo solicitado. |

<!--
## Related Information {#related-information}

* [JavaScript API Reference](/help/authentication/javascript-sdk-api-reference.md)
* [iOS/tvOS API Reference](/help/authentication/iostvos-sdk-api-reference.md)
* **Android API Reference**
-->
