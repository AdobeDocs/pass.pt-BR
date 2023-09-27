---
title: Referência da API do SDK do JavaScript
description: Referência da API do SDK do JavaScript
exl-id: 48d48327-14e6-46f3-9e80-557f161acd8a
source-git-commit: 4eb5fc1eb1eea4c5e27bbee298db4b9b4ba2daef
workflow-type: tm+mt
source-wordcount: '2836'
ht-degree: 0%

---

# Referência da API do SDK do JavaScript {#javascript-sdk-api-reference}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Referência da API {#api-reference}

Essas funções iniciam solicitações de interação com um MVPD. Todas as chamadas são assíncronas; é necessário implementar [callbacks](#callbacks) para lidar com as respostas:

- [setRequestor()](#setReq)
- [getAuthorization()](#getAuthZ)
- [getAuthentication()](#getAuthN)
- [checkAuthentication()](#checkAuthN)
- [checkAuthorization()](#checkAuthZ)
- [checkPreauthorizedResources()](#checkPreauthRes)
- [getMetadata()](#getMeta)
- [setSelectedProvider()](#setSelProv)
- [logout()](#logout)


## setRequestor (inRequestorID, endpoints, opções){#setrequestor(inRequestorID,endpoints,options)}

**Descrição:** Identifica o site do qual as solicitações se originam.  Você deve fazer essa chamada antes de qualquer outra chamada de API em uma sessão de comunicação.

**Parâmetros:**

- *inRequestorID* - O identificador exclusivo que o Adobe atribuiu ao site de origem durante o registro.

- *endpoints* - Esse parâmetro é opcional. Pode ser um dos seguintes valores:

   - Uma matriz que permite especificar endpoints para serviços de autenticação e autorização fornecidos pelo Adobe (instâncias diferentes podem ser usadas para fins de depuração). Caso vários URLs sejam fornecidos, a lista MVPD é composta pelos endpoints de todos os provedores de serviços. Cada MVPD está associado ao provedor de serviços mais rápido; ou seja, o provedor que respondeu primeiro e que oferece suporte a esse MVPD. Por padrão (se nenhum valor for especificado), o provedor de serviços da Adobe é usado (<http://sp.auth.adobe.com/>).

  Exemplo:
   - `setRequestor("IFC", ["http://sp.auth-dev.adobe.com/adobe-services"])`

- *opções* - Um objeto JSON que contém o valor da ID do aplicativo, o valor da ID do visitante e as configurações sem atualização (logon em segundo plano) e as configurações de MVPD (iFrame). Todos os valores são opcionais.
   1. Se especificado, o Experience Cloud visitorID será relatado em todas as chamadas de rede executadas pela biblioteca. O valor pode ser usado posteriormente para relatórios de análise avançada.
   2. Se o identificador exclusivo do aplicativo for especificado -`applicationId` - o valor será adicionado a todas as chamadas subsequentes feitas pelo aplicativo como parte do cabeçalho HTTP X-Device-Info. Este valor pode ser obtido posteriormente de [ESM](/help/authentication/entitlement-service-monitoring-overview.md) relatórios usando a consulta adequada.

  **Nota:** Todas as chaves JSON fazem distinção entre maiúsculas e minúsculas.

  Exemplo:

```JSON
   setRequestor("IFC", {
      "visitorID": "THE_ECID_VALUE",
      "applicationId": "APP_ID_VALUE"
  })
```

- O Programador pode substituir as configurações do MVPD definidas na Autenticação do Adobe Pass, especificando se um iFrame é necessário ou não para logon (*iFrameRequired* ) e as dimensões do iFrame (*iFrameWidth* e *iFrameHeight* chaves). O objeto JSON tem o seguinte modelo:

```JSON
    {  
       "visitorID": <string>,
       "backgroundLogin": <boolean>,
       "backgroundLogout": <boolean>,
       "mvpdConfig":{  
          "MVPD_ID_1":{  
             "iFrameRequired": <boolean>,
             "iFrameWidth": <integer>,
             "iFrameHeight": <integer>
          },
          ...
          "MVPD_ID_N":{  
             "iFrameRequired": <boolean>,
             "iFrameWidth": <integer>,
             "iFrameHeight": <integer>
          }
       }
    }
```


Todas as chaves de nível superior no modelo acima são opcionais e têm valores padrão (*backgroundLogin*, *backgroundLog* são falsos por padrão e mvpdConfig é nulo - o que significa que nenhuma configuração de MVPD é substituída).


- **Nota**: a especificação de valores/tipos inválidos para os parâmetros acima resultará em um comportamento indefinido.



Este é um exemplo de configuração para o seguinte cenário: ativar o logon e o logout sem atualização, alterar MVPD1 para logon de redirecionamento de página completa (não iFrame) e MVPD2 para logon de iFrame com width=500 e height=300:

```JSON
    {  
       "backgroundLogin": true,
       "backgroundLogout": true,
       "mvpdConfig":{  
          "MVPD1":{  
             "iFrameRequired": false
          },
          "MVPD2":{  
             "iFrameRequired": true,
             "iFrameWidth": 500,
             "iFrameHeight": 300
          }
       }
    }
```


**Retornos de chamada disparados:** [setConfig()](#setconfigconfigxml-setconfigconfigxml)
</br>

[Voltar ao início](#top)

</br>

## getAuthorization(inResourceID, redirect_url) {#getauthorization(inresourceid,redirect_url)}

**Descrição:** Solicita autorização para o recurso especificado. Cada vez que um cliente tentar acessar um recurso autorizável, chame essa função para obter um token de autorização de curta duração do Ativador de acesso. As IDs de recursos são acordadas com o MVPD que fornece autorização.

Usa o token de autenticação em cache para o cliente atual. Se esse token não for encontrado, o iniciará o processo de autenticação primeiro e, em seguida, continuará com a autorização.

**Parâmetros:**

- `inResourceID` - A ID do recurso para o qual o usuário solicita autorização.
- `redirect_url` - Opcionalmente, forneça um URL de redirecionamento, para que o processo de autorização do MVPD retorne o usuário a essa página, em vez da página a partir da qual a autorização foi iniciada.


**Retornos de chamada disparados:** [setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken) no sucesso, [tokenRequestFailed](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage) no caso de falha

>[!CAUTION]
>
>Use checkAuthorization() em vez de getAuthorization() sempre que possível. O método getAuthorization() iniciará um fluxo de autenticação completo (se o usuário não estiver autenticado) e isso pode levar a uma implementação complicada por parte do programador.

</b>

[Voltar ao início](#top)

</br>

## getAuthentication(redirect_url) {#getauthentication(redirect_url}

**Descrição:** Solicita autenticação para o cliente atual. Normalmente chamado em resposta a um clique em um botão de Logon. Procura um token de autenticação em cache para o cliente atual. Se esse token não for encontrado, o iniciará o processo de autenticação. Isso chama a caixa de diálogo de seleção de provedor padrão ou personalizada e, em seguida, usa o provedor selecionado para redirecionar para a interface de logon do MVPD.

Quando bem-sucedido, o cria e armazena um token de autenticação para o usuário. Se a autenticação falhar, o provedor retornará uma mensagem de erro apropriada à [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode) retorno de chamada.

**Parâmetros:**

- redirect_url - Opcionalmente, forneça um URL de redirecionamento, para que o processo de autenticação do MVPD retorne o usuário para essa página, em vez da página a partir da qual a autenticação foi iniciada.

**Retornos de chamada disparados:** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode), [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[Voltar ao início](#top)

</br>

## checkAuthN {#checkauthn}

**Descrição:** Verifica o status de autenticação atual do cliente atual.  Não associado a nenhuma interface do usuário.

**Retornos de chamada disparados:** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

[Voltar ao início](#top)

</br>

## checkAuthorization(inResourceID) {#checkauthorization(inresourceid)}

**Descrição:** Este método é usado pelo aplicativo para verificar o status de autorização do cliente atual e do recurso fornecido. Ela é iniciada verificando o status de autenticação primeiro. Se não for autenticado, o retorno de chamada tokenRequestFailed() será acionado e o método será encerrado. Se o usuário estiver autenticado, ele também acionará o fluxo de autorização. Veja os detalhes no [getAuthorization()](método #getAuthZ.

>[!TIP]
>
> **Utilização de funções de verificação de status**  Não é necessário verificar o status da autenticação ou da autorização antes de solicitar a autorização. Você pode chamar essas funções, por exemplo, para atualizar sua própria exibição de status. Não os utilize quando precisar de qualquer interação com o usuário.

**Parâmetros:**

- `inResourceID` - A ID do recurso para o qual o usuário solicita autorização.


**Retornos de chamada disparados:**
[setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken), [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata), [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)

</br>

## checkPreauthorizedResources(recursos) {#checkPreauthorizedResources(resources)}

**Descrição:** Solicita o status de autorização &quot;comprovação&quot; para uma lista de recursos.

**Parâmetros:**

- *recursos*: o parâmetro resources é uma matriz de recursos para a qual a autorização deve ser verificada. Cada elemento na lista deve ser uma string que representa a ID do recurso. A ID do recurso está sujeita às mesmas limitações que a ID do recurso na `getAuthorization()` chamada, ou seja, é um valor acordado estabelecido entre o Programador e o MVPD, ou um fragmento de RSS de mídia.

</br>

## checkPreauthorizedResources(resources-cache=true) {#checkPreauthorizedResources(resources-cache=true)}

Esta variante de API está disponível a partir do JS SDK versão 4.0


**Parâmetros:**

- *recursos*: o parâmetro resources é uma matriz de recursos para a qual a autorização deve ser verificada. Cada elemento na lista deve ser uma string que representa a ID do recurso. A ID do recurso está sujeita às mesmas limitações que a ID do recurso na `getAuthorization()` chamada, ou seja, é um valor acordado estabelecido entre o Programador e o MVPD, ou um fragmento de RSS de mídia.

- *cache*: Opção de usar o cache interno ao verificar recursos pré-autorizados. Este é um parâmetro opcional, padronizando para **true**. Se verdadeiro, o comportamento é idêntico à API acima, o que significa que as chamadas subsequentes a essa função usarão um cache interno para resolver o recurso pré-autorizado. Passagem **false** para este parâmetro desabilitará o cache interno, resultando em uma chamada de servidor sempre que o **checkPreauthorizedResources** A API é chamada.

**Retornos de chamada disparados:** [preauthorizedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)

</br>

[Voltar ao início](#top)
</br>

## getMetadata(Key) {#getMetadata}

**Descrição:** Recupera informações expostas como metadados pela biblioteca do Access Enabler.

Há dois tipos de metadados:

- **Estático** (TTL do token de autenticação, TTL do token de autorização e ID do dispositivo)
- **Metadados do usuário** (Isso inclui informações específicas do usuário transmitidas do MVPD para o dispositivo do usuário durante os fluxos de Autenticação e/ou Autorização)

**Mais informações:** [Metadados do usuário](#UserMetadata)

**Parâmetros:**

- *key*: uma id que especifica os metadados solicitados:
   - Se a chave for `"TTL_AUTHN",` em seguida, a consulta é feita para obter o tempo de expiração do token de autenticação.

   - Se a chave for `"TTL_AUTHZ"` e params é uma matriz que contém a id do recurso como uma cadeia de caracteres, então a consulta é feita para obter a hora de expiração do token de autorização associado ao recurso especificado.

   - Se a chave for `"DEVICEID"` em seguida, a consulta é feita para obter a id do dispositivo atual. Observe que esse recurso está desativado por padrão e os programadores devem entrar em contato com o Adobe para obter informações sobre ativação e taxas.

   - Se a chave for da seguinte lista de tipos de metadados do usuário, um objeto JSON contendo os metadados do usuário correspondentes será enviado para o [`setMetadataStatus()`](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata) função de retorno de chamada:

   - `"zip"` - CEP

   - `"encryptedZip"` - Código postal criptografado

   - `"householdID"` - Identificador do agregado. No caso de um MVPD não suportar subcontas, isso será idêntico a userID.

   - `"maxRating"` - Classificação máxima dos pais para o usuário

   - `"userID"` - O identificador do usuário. No caso em que um MVPD oferece suporte a subcontas e o usuário não é a conta principal, a ID do usuário será diferente da ID da família.

   - `"channelID"` - A lista de canais que o usuário está autorizado a visualizar

   - `"is_hoh"` - Sinalizador que identifica se um usuário é chefe de família

   - `"encryptedZip"` - CEP criptografado

   - `"typeID"` - Sinalizador que identifica se a conta do usuário é a conta principal/secundária

   - `"primaryOID"` - Identificador do agregado

   - `"postalCode"` - Semelhante ao CEP

   - `"acctID"` - ID da conta

   - `"acctParentID"` - ID da conta principal

  **Nota**: os metadados reais do usuário disponíveis para um Programador dependem do que um MVPD disponibiliza.  Consulte [Metadados do usuário](#UserMetadata) para obter a lista atual de Metadados de usuário disponíveis.


Por exemplo:

```JSON
    // Assume that a reference to the AccessEnabler has been previously 
    // obtained and stored in the "ae" variable
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
        if (status ==  1) {
            //user is authenticated, request metadata
            ae.getMetadata("zip");
            ae.getMetadata("maxRating");
        } else {
            ...
      }
    }
```


**Retornos de chamada disparados:** [setMetadataStatus()](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)

</br>

[Voltar ao início](#top)

</br>


## setSelectedProvider(providerid) {#setSelectedProvider}

**Descrição:** Chame essa função quando o usuário tiver selecionado um MVPD na interface de seleção de provedor para enviar a seleção de provedor para o Ativador de acesso ou chamar essa função com um parâmetro nulo caso o usuário tenha descartado sua interface de seleção de provedor sem selecionar um provedor.

**Retornos de chamada disparados:**[ setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode), [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)

</br>

[Voltar ao início](#top)

</br>

## getSelectedProvider() {#getSelectedProvider}

**Descrição:** Recupera os resultados da seleção do cliente na caixa de diálogo de seleção do provedor. Isso pode ser usado a qualquer momento após a verificação de autenticação inicial.

Essa função é assíncrona e retorna o resultado à `selectedProvider()` função de retorno de chamada.

- **MVPD** O MVPD atualmente selecionado ou nulo se nenhum MVPD foi selecionado.
- **AE_State** O resultado da autenticação para o cliente atual: &quot;Novo usuário&quot;, &quot;Usuário não autenticado&quot; ou &quot;Usuário autenticado&quot;

**Retornos de chamada disparados:** [seletedProvider()](#getselectedprovider-getselectedprovider)

</br>

[Voltar ao início](#top)

</br>

## logout {#logout}

**Descrição:** Faz logout do cliente atual, limpando todas as informações de autenticação e autorização desse usuário. Exclui todos os tokens authN e authZ do sistema do cliente.

**Retornos de chamada disparados:** [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)
</br>

[Voltar ao início](#top)

</br>

## Definição de retorno de chamada {#calllback-definitions}

Você deve implementar esses retornos de chamada para lidar com as respostas às suas chamadas de solicitação assíncronas:

- [titlementLoaded()](#entitlementloaded-entitlementloaded)
- [setConfig()](#setconfigconfigxml-setconfigconfigxml)
- [displayProviderDialog()](#displayproviderdialogproviders-displayproviderdialogproviders)
- [createIFrame()](#createiframe-createiframeinwidthminheight)
- [setAuthenticationStatus()](#setauthenticationstatusisauthenticated-errorcode)
- [sendTrackingData()](#sendtrackingdatatrackingeventtype-trackingdata-sendtrackingdatatrackingeventtypetrackingdata)
- [setToken()](#settokeninrequestedresourceid-intoken-settokeninrequestedresourceidintoken)
- [tokenRequestFailed()](#tokenrequestfailedinrequestedresourceid-inrequesterrorcode-inrequestdetailederrormessage-tokenrequestfailedinrequestedresourceidinrequesterrorcodeinrequestdetailederrormessage)
- [preauthorizedResources()](#preauthorizedresourcesauthorizedresources-preauthorizedresourcesauthorizedresources)
- [setMetadataStatus()](#setmetadatastatuskey-encrypted-data-setmetadatastatuskeyencrypteddata)
- [seletedProvider()](#selectedproviderresult-selectedprovider)

</br>

## titlementLoaded() {#entitlementLoaded}

**Descrição:** Disparado quando o Ativador de acesso conclui a inicialização e está pronto para receber solicitações. Implemente essa chamada de retorno para saber quando você pode iniciar a comunicação com a API do Access Enabler.
</br>

[Voltar ao início](#top)

</br>

## setConfig(configXML) {#setconfig(configXML)}

**Descrição:** Implemente essa chamada de retorno para receber as informações de configuração e a lista MVPD.

**Parâmetros:**

- *configXML*: objeto xml que contém a configuração do SOLICITANTE atual, incluindo a lista MVPD.


**Acionado por:** [setRequestor()](#setrequestor-inrequestorid-endpoints-optionssetreq)

</br>

[Voltar ao início](#top)

</br>

## displayProviderDialog(provedores) {#displayproviderdialog(providers)}

**Descrição:** Implemente essa chamada de retorno para chamar sua própria interface personalizada de seleção de provedor. Sua caixa de diálogo deve usar o nome de exibição (e o logotipo opcional) para fornecer as opções do cliente. Quando o cliente tiver feito uma escolha e descartado a caixa de diálogo, envie a ID associada do provedor escolhido na chamada para *setSelectedProvider()*.

**Parâmetros:**

- *provedores* - Uma matriz de Objetos que representa os MVPDs solicitados:

```JSON
    var mvpd = {
        ID: "someprov",
        displayName: "Some Provider",
        logoURL: "http://www.someprov.com/images/logo.jpg"
    }
```

**Acionado por:** [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>[Voltar ao início](#top)

</br>

## createIFrame(inWidth, inHeight) {#createIFrame(inWidth,inHeight)}

**Descrição:** Implemente esse retorno de chamada se o usuário tiver selecionado um MVPD que requer um iFrame no qual exibir sua interface da página de logon de autenticação.

**Acionado por:**[ setSelectedProvider()](#setselectedproviderproviderid-setselectedprovider)

</br> [Voltar ao início](#top)

</br>

## setAuthenticationStatus(isAuthenticated, errorCode) {#set-authn-status-isauthn-error}

**Descrição:** Implemente essa chamada de retorno para receber o status de autenticação (1=autenticado ou 0=não autenticado) e uma mensagem de erro descritiva se ocorrer algum erro ao tentar determinar o status de autenticação (cadeia vazia na conclusão bem-sucedida da verificação).

>[!NOTE]
> 
>Se você estiver usando o atual, [Relatório de Erros Avançado](/help/authentication/error-reporting.md) sistema, você pode ignorar o parâmetro errorCode enviado para essa função.  No entanto, o sinalizador isAuthenticated ainda é usado para rastrear o status de autenticação de um usuário no fluxo de direitos


**Parâmetros:**

- *isAuthenticated* - Fornece o status de autenticação: 1 (autenticado) ou 0 (não autenticado).
- *errorCode* - Qualquer erro que ocorra ao determinar o status de autenticação. Uma cadeia de caracteres vazia, se nenhuma.


**Acionado por:** [checkAuthentication()](#checkauthn-checkauthn), [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid)

</br>

[Voltar ao início](#top)

</br>

## sendTrackingData(trackingEventType, trackingData) {#sendTrackingData(trackingEventType,trackingData)}

>[!CAUTION]
>
>O tipo de dispositivo e o sistema operacional são derivados do uso de uma biblioteca Java pública (<http://java.net/projects/user-agent-utils>) e a sequência de agente do usuário. Esteja ciente de que essas informações são fornecidas apenas como uma forma grosseira de dividir as métricas operacionais em categorias de dispositivos, mas esse Adobe não pode assumir nenhuma responsabilidade por resultados incorretos. Use a nova funcionalidade adequadamente.

**Descrição:** Implemente essa chamada de retorno para receber dados de rastreamento quando eventos específicos ocorrerem. Você pode usar isso, por exemplo, para rastrear quantos usuários fizeram logon com as mesmas credenciais. O rastreamento não está configurável no momento. Com Autenticação Adobe Pass 1.6, `sendTrackingData()` O também relata informações sobre o dispositivo, o cliente Access Enabler e o tipo de sistema operacional. A variável `sendTrackingData()` O retorno de chamada permanece compatível com versões anteriores.

- Valores possíveis para o tipo de dispositivo:
   - computador
   - tablet
   - dispositivo móvel
   - gameconsole
   - desconhecido

- Valores possíveis para o tipo de cliente do Access Enabler:
   - html5
   - ios
   - android


Passa o tipo de evento e uma matriz de informações associadas. Os tipos de evento são:

| mvpdSelection | O usuário selecionou um MVPD em uma caixa de diálogo de seleção de provedor. |
| ----------------------- | --------------------------------------------------------- |
| authenticationDetection | Verificação de autenticação concluída. |
| authorizationDetection | Uma solicitação de autorização foi concluída. |

</br>
Os dados são específicos para cada tipo de evento:
</br>

| Tipo de evento (String) | Dados (Matriz) |
|:--- | :--- |
| mvpdSelection | 0: MVPD selecionado |
|  | 1: Tipo de dispositivo |
|  | 2: Tipo de cliente do Access Enabler |
|  | 3: SO |
| authenticationDetection | 0: Se a solicitação de token foi bem-sucedida (true/false) |
|  | 1: ID DO MVPD |
|  | 2: GUID |
|  | 3: Token já no cache (true/false) |
|  | 4: Tipo de dispositivo |
|  | 5: Tipo de cliente do Access Enabler |
|  | 6: SO |
| authorizationDetection | 0: Se a solicitação de token foi bem-sucedida (true/false) |
|  | 1: ID DO MVPD |
|  | 2: GUID |
|  | 3: Token já no cache (true/false) |
|  | 4: Erro |
|  | 5: Detalhes |
|  | 6: Tipo de dispositivo |
|  | 7: Tipo de cliente do Access Enabler |
|  | 8: SO |


**Acionado por:** [checkAuthentication()](#checkauthn-checkauthn), [getAuthentication()](#getauthenticationredirecturl-getauthenticationredirecturl), [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)

</br>

[Voltar ao início](#top)

</br>

## setToken(inRequestedResourceID, inToken) {#setToken(inRequestedResourceID,inToken)}

**Descrição:** Implemente essa chamada de retorno para receber o token de mídia de vida curta (inToken) e a ID do recurso (inRequestedResourceID) para o qual foi feita uma solicitação de autorização ou uma solicitação de autorização de verificação e concluída com êxito.

**Acionado por:** [checkAuthorization()](#checkAuthZ), [getAuthorization()](#getAuthZ)
</br>

[Voltar ao início](#top)

</br>

## tokenRequestFailed(inRequestedResourceID, inRequestErrorCode, inRequestDetailedErrorMessage) {#token-request-failed-error-msg}

**Descrição:** Implemente este retorno de chamada para ser sinalizado quando uma autorização ou uma solicitação de autorização de verificação falhar. Pode, opcionalmente, ser usado por um MVPD para fornecer uma mensagem personalizada a ser exibida pelo Programador.

>[!IMPORTANT]
>
>Essa função de retorno de chamada faz parte do sistema de relatório de erros de autenticação da Adobe Pass original mais antigo. Ela é retida para compatibilidade com versões anteriores, mas não é necessário usar essa função se você tiver implementado seus próprios retornos de chamada usando o sistema atual de Relatório de Erros Avançado. O sistema de relatório de erros mais recente fornece informações mais detalhadas sobre por que uma autorização (ou outra operação) falhou, juntamente com os cursos de ação sugeridos para cada tipo de erro ou aviso.

**Parâmetros:**

- *inRequestedResourceID* - Uma string que fornece a ID do recurso que foi usada na solicitação de autorização.
- *inRequestErrorCode* - Uma string que exibe o código de erro de autenticação da Adobe Pass, indicando o motivo da falha. Os valores possíveis são &quot;Erro de usuário não autenticado&quot; e &quot;Erro de usuário não autorizado&quot;. Para obter mais detalhes, consulte &quot;Códigos de erro de retorno de chamada&quot; abaixo.
- *inRequestDetailedErrorMessage* - Uma sequência descritiva adicional adequada para exibição. Se essa cadeia descritiva não estiver disponível por algum motivo, a Autenticação Adobe Pass enviará uma cadeia de caracteres vazia **(&quot;&quot;)**.  Ele pode ser usado por um MVPD para transmitir mensagens de erro personalizadas ou mensagens relacionadas a vendas. Por exemplo, se um assinante tiver a autorização negada para um recurso, o MVPD poderá responder com uma `*inRequestDetailedErrorMessage*` como: **&quot;No momento, você não tem acesso a esse canal em seu pacote. Se quiser atualizar seu pacote, clique \*aqui\*.&quot;** A mensagem é passada pela Autenticação Adobe Pass por meio dessa chamada de retorno para o site do Programador. O Programador tem a opção de exibi-lo ou ignorá-lo. A Autenticação Adobe Pass também pode usar `*inRequestDetailedErrorMessage*` para notificar o Programador da condição que pode ter levado a um erro. Por exemplo, **&quot;Erro de rede ao se comunicar com o serviço de autorização do provedor.&quot;**



**Acionado por:**  [checkAuthorization()](#checkauthorizationinresourceid-checkauthorizationinresourceid), [getAuthorization()](#getauthorizationinresourceid-redirecturl-getauthorizationinresourceidredirecturl)
</br>

[Voltar ao início](#top)

</br>


## preauthorizedResources(authorizedResources) {#preauthorizedResources(authorizedResources)}

**Descrição:** Retorno de chamada acionado pelo Ativador de acesso que entrega a lista de recursos autorizados retornada após uma chamada para `checkPreauthorizedResources()`.

**Parâmetros:**

- *authorizedResources*: a lista de recursos autorizados.

**Acionado por:** [checkPreauthorizedResources()](#checkPreauthRes)
</br>

[Voltar ao início](#top)

</br>

## setMetadataStatus(chave, criptografada, dados) {#setMetadataStatus(key,encrypted,data)}

**Descrição:** Retorno de chamada acionado pelo Ativador de acesso que entrega os metadados solicitados por meio de um `getMetadata()` chame.

**Mais informações:** [Metadados do usuário](#userMetadata)

**Parâmetros:**

- *key (String)*: a chave dos metadados para os quais a solicitação foi feita.
- *criptografado (booleano)*: um sinalizador que indica se o &quot;valor&quot; está criptografado ou não. Se isso for &quot;true&quot;, o &quot;value&quot; será realmente uma representação JSON Web Encrypted do valor real.
- *dados (objeto JSON)*: um objeto JSON com a representação dos metadados. Para solicitações simples (&#39;`TTL_AUTHN`&#39;, &#39;`TTL_AUTHZ`&#39;, &#39;`DEVICEID`&#39;), o resultado é uma String (representando o TTL de autenticação, o TTL de autorização ou a ID do dispositivo). No caso de uma solicitação de metadados do usuário, o resultado pode ser um objeto primitivo ou JSON que representa a carga de metadados. A estrutura real dos objetos de metadados do usuário JSON é semelhante ao seguinte:

```JSON
    {
        updated: 1334243471,
        encrypted: ["encryptedProp"],
        data: {
            zip: ["12345", "34567"],
            maxrating: { 
                "MPAA": "PG-13",
                "VCHIP": "TV-Y", 
                "URL": "http://exam.pl/e/manage/ratings"
            },
            householdid: "3456",
            uid: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
            channelID: ["channel-1", "channel-2"]
    }
```


Por exemplo:

```JSON
    // Implement the setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
        if (encrypted) {
            //the metadata value is encrypted
            //needs to be decrypted by the programmer
            data = decrypt(data);
        }
        alert(key + "=" + data);
    }
```


**Acionado por:** [`getMetadata()`](#getmetadatakey-getmetadata)
</br>
[Voltar ao início](#top)

</br>

## seletedProvider(resultado) {#selectedProvider(result)}

**Descrição:** Implemente este retorno de chamada para receber o MVPD selecionado no momento e o resultado da autenticação do usuário atual encapsulado no `result` parâmetro. A variável `result` é um objeto com as seguintes propriedades:

- **MVPD** O MVPD atualmente selecionado ou nulo se nenhum MVPD foi selecionado.
- **AE\_State** O resultado da autenticação para o usuário atual, um dos &quot;Novo usuário&quot;, &quot;Usuário não autenticado&quot; ou &quot;Usuário autenticado

**Acionado por:** [getSelectedProvider()](#getSelProv)

</br>

[Voltar ao início](#top)

</br>

### Códigos de erro de retorno {#callback-error-codes}

| Erros genéricos | |
|:--- | :--- | 
| Erro interno | Ocorreu um erro no sistema ao tentar processar a solicitação. |
| Erro de provedor não selecionado | Ocorre quando o cliente cancela a caixa de diálogo de seleção do provedor |
| Erro de provedor não disponível | Ocorre quando nenhum provedor está disponível. |

| Erros de autenticação | |
|:--- | :--- | 
| Erro de autenticação genérica | Retornado quando o motivo não é conhecido ou não pode ser publicado. |
| Erro de autenticação interna | Ocorreu um erro no sistema durante a tentativa de autenticação. |
| Erro de usuário não autenticado | Usuário não autenticado. |
| Erro de várias solicitações de autenticação | Solicitações de autenticação adicionais foram recebidas antes da conclusão da primeira. |

| Erros de autorização | |
|:--- | :--- | 
| Erro de autorização genérico | Retornado quando o motivo não é conhecido ou não pode ser publicado. |
| Erro de autorização interna | Ocorreu um erro de sistema ao tentar autorizar. |
| Erro de usuário não autorizado | O cliente não está autorizado a visualizar o conteúdo solicitado. |

<!--

### Related Information {#Related-Infornation}

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK Cookbook](/help/authentication/javascript-sdk-cookbook.md)
* **JavaScript Code Samples**
* [Error Reporting](/help/authentication/error-reporting.md)
* [Understanding Tokens](#understanding_tokens)
* **Tracking Data in Adobe Pass Authentication**
-->

[Voltar ao início](#top)
