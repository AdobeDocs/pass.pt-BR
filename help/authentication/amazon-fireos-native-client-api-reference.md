---
title: Referência da API do cliente nativo do Amazon FireOS
description: Referência da API do cliente nativo do Amazon FireOS
exl-id: 8ac9f976-fd6b-4b19-a80d-49bfe57134b5
source-git-commit: 2ccfa8e018b854a359881eab193c1414103eb903
workflow-type: tm+mt
source-wordcount: '3416'
ht-degree: 0%

---

# Referência da API do cliente nativo do Amazon FireOS {#amazon-fireos-native-client-api-reference}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

</br>

## Introdução {#intro}

Este documento detalha os métodos e retornos de chamada expostos pelo SDK do Amazon FireOS para autenticação da Adobe Pass, compatível com autenticação da Adobe Pass. Os métodos e as funções de retorno de chamada descritos aqui são definidos nos arquivos de cabeçalho AccessEnabler.h e EntitlementDelegate.h.

Consulte <https://tve.zendesk.com/hc/en-us/articles/115005561623-fire-TV-Native-AccessEnabler-Library> para obter o SDK do Amazon FireOS AccessEnabler mais recente.

>[!NOTE]
>
>A equipe de autenticação da Adobe Pass incentiva você a usar somente a autenticação da Adobe Pass *público* APIs:

- As APIs públicas estão disponíveis *e totalmente testado* em todos os tipos de clientes compatíveis. Para qualquer recurso público, garantimos que cada tipo de cliente tenha uma versão correspondente dos métodos associados.
- As APIs públicas devem ser o mais estáveis possível, para oferecer compatibilidade com versões anteriores e garantir que as integrações de parceiros não sejam interrompidas. No entanto, para *não*-APIs públicas, reservamos o direito de alterar sua assinatura em qualquer ponto futuro. Se você encontrar um fluxo específico que não possa ser suportado por meio de uma combinação das chamadas de API de autenticação do Adobe Pass públicas atuais, a melhor abordagem é informar o. Levando em consideração suas necessidades, podemos modificar as APIs públicas e fornecer uma solução estável a partir de agora.

## API do SDK do Amazon FireOS {#api}

- [getInstance](#getInstance)
- [setOptions](#fire_setOption)
- [setRequestor](#setRequestor)
- [setRequestorComplete](#setRequestorComplete)
- [checkAuthentication](#checkAuthN)
- [getAuthentication](#getAuthN)
- [displayProviderDialog](#displayProviderDialog)
- [setSelectedProvider](#setSelectedProvider)
- [navigateToUrl](#navigagteToUrl)
- [getAuthenticationToken](#getAuthNToken)
- [setAuthenticationStatus](#setAuthNStatus)
- [checkPreauthorizedResources](#checkPreauth)
- [preauthorizedResources](#preauthResources)
- [checkAuthorization](#checkAuthZ)
- [getAuthorization](#getAuthZ)
- [setToken](#setToken)
- [tokenRequestFailed](#tokenRequestFailed)
- [logout](#logout)
- [getSelectedProvider](#getSelectedProvider)
- [seletedProvider](#selectedProvider)
- [getMetadata](#getMetadata)
- [setMetadataStatus](#setMetadaStatus)
- [getVersion](#getVersion)

</br>

### Factory.getInstance {#getInstance}

**Descrição:** Instancia o objeto do Access Enabler. Deve haver uma única instância do Access Enabler por instância do aplicativo.

| Chamada de API: construtor |
| --- |
| ```public static AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        throws AccessEnablerException```<br><br> |
| ```public static AccessEnabler getInstance(Context appContext, String env_url, String softwareStatement, String redirectUrl) throws AccessEnablerException``` |

**Disponibilidade:** v3.0+


**Parâmetros:**

- *appContext*: contexto de aplicativo do Amazon Fire OS.
- softwareStatement
- redirectUrl : no caso do FireOS, o valor do parâmetro será ignorado e definido como padrão : adobepass://android.app
- env_url: para testar usando o ambiente de preparo do Adobe, env\_url pode ser definido como &quot;sp.auth-staging.adobe.com&quot;

**Obsoleto:**

```java
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```


### setRequestor {#setRequestor}

**Descrição:** Estabelece a identidade do Programador. Cada programador recebe uma ID exclusiva ao se registrar no Adobe para o sistema de autenticação da Adobe Pass. Esta configuração deve ser executada somente uma vez durante o ciclo de vida do aplicativo.

A resposta do servidor contém uma lista de MVPDs juntamente com algumas informações de configuração anexadas à identidade do Programador. A resposta do servidor é usada internamente pelo código Access Enabler. Somente o status da operação (ou seja, SUCCESS/FAIL) é apresentado ao seu aplicativo por meio do retorno de chamada setRequestorComplete().

Se a variável *urls* não for usado, a chamada de rede resultante será direcionada ao URL do provedor de serviços padrão: o ambiente de Liberação/Produção do Adobe.

Se um valor for fornecido para a variável *urls* parâmetro, a chamada de rede resultante será direcionada a todos os URLs fornecidos na variável *urls* parâmetro. Todas as solicitações de configuração são acionadas simultaneamente em threads separados. O primeiro respondente tem prioridade ao compilar a lista de MVPDs. Para cada MVPD na lista, o Ativador de acesso lembra o URL do provedor de serviços associado. Todas as solicitações de direito subsequentes são direcionadas ao URL associado ao provedor de serviços que foi emparelhado com o MVPD de destino durante a fase de configuração.

| Chamada de API: configuração do solicitante |
| --- |
| ```public void setRequestor(String requestorId)``` |


**Disponibilidade:** v3.0+


| Chamada de API: configuração do solicitante |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Disponibilidade:** v3.0+


**Parâmetros:**

- *requestorID*: o identificador exclusivo associado ao programador. Transmita o identificador exclusivo atribuído pelo Adobe ao seu site quando você se registrou pela primeira vez no serviço de autenticação da Adobe Pass.
- *urls*: parâmetro opcional; por padrão, o provedor de serviços da Adobe é usado (http://sp.auth.adobe.com/). Essa matriz permite especificar endpoints para serviços de autenticação e autorização fornecidos pelo Adobe (instâncias diferentes podem ser usadas para fins de depuração). Você pode usar essa opção para especificar várias instâncias do provedor de serviços de Autenticação da Adobe Pass. Ao fazer isso, a lista MVPD é composta pelos endpoints de todos os provedores de serviços. Cada MVPD está associado ao provedor de serviços mais rápido; ou seja, o provedor que respondeu primeiro e que oferece suporte a esse MVPD.

**Retornos de chamada disparados:** `setRequestorComplete()`



**Obsoleto:**

```
    public void setRequestor(String requestorId, String signedRequestorId)

    public void setRequestor(String requestorId, String signedRequestorId, ArrayList<String> urls)
```

</br>


### setRequestorComplete {#setRequestorComplete}

**Descrição:** Retorno de chamada acionado pelo Ativador de acesso que informa ao aplicativo que a fase de configuração foi concluída. Esse é um sinal de que o aplicativo pode começar a emitir solicitações de direito. Nenhuma solicitação de direito pode ser emitida pelo aplicativo até que a fase de configuração seja concluída.

| Retorno de chamada: configuração do solicitante concluída |
| --- |
| ```public void setRequestorComplete(int status)``` |

**Disponibilidade:** v1.0+

**Parâmetros:**

- *status*: Pode ter um dos seguintes valores:
   - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - a fase de configuração foi concluída com êxito
   - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - falha na fase de configuração

**Acionado por:** `setRequestor()`

</br>


### setOptions {#fire_setOption}

**Descrição:** Configura as opções globais do SDK. Aceita uma **Mapa\&lt;string string=&quot;&quot;>** como argumento. Os valores do mapa serão passados para o servidor junto com cada chamada de rede feita pelo SDK.

Os valores serão passados ao servidor independentemente do fluxo atual (autenticação/autorização). Se quiser alterar os valores, você pode chamar esse método a qualquer momento.



| Chamada de API: setOptions |
| --- |
| ```public void setOptions(HashMap<String,String> options)``` |

**Disponibilidade:** v3.0+

**Parâmetros:**

- *opções*: Um mapa\&lt;string string=&quot;&quot;> contendo opções globais do SDK. Atualmente, as seguintes opções estão disponíveis:
   - **applicationProfile** - Ele pode ser usado para fazer configurações de servidor com base nesse valor.
   - **ap\_vi** - O Serviço de ID de Experience Cloud. Esse valor pode ser usado posteriormente para relatórios de análise avançada.
   - **device\_info** - Informações sobre o dispositivo, conforme descrito em **Guia de informações do dispositivo de passagem**

</br>

### checkAuthentication {#checkAuthN}

**Descrição:** Verifica o status de autenticação. Ele faz isso procurando um token de autenticação válido no espaço de armazenamento de token local. Chamar esse método não executa chamadas de rede. Ele é usado pelo aplicativo para consultar o status de autenticação do usuário e atualizar a interface de acordo (ou seja, atualizar a interface de logon/logout). O status de autenticação é comunicado ao aplicativo por meio da [*setAuthenticationStatus()*](#setAuthNStatus) retorno de chamada.

Se um MVPD suportar o recurso &quot;Autenticação por solicitante&quot;, vários tokens de autenticação poderão ser armazenados em um dispositivo.

| Chamada de API: verificar status de autenticação |
| --- |
| ```public void checkAuthentication()``` |

**Disponibilidade:** v1.0+

**Parâmetros:** Nenhum

**Retornos de chamada disparados:** `setAuthenticationStatus()`

</br>

### getAuthentication {#getAuthN}

**Descrição:** Inicia o fluxo de trabalho de autenticação completa. Ele é iniciado verificando o status de autenticação. Se ainda não estiver autenticado, a máquina de estado do fluxo de autenticação é iniciada:

- Se a última tentativa de autenticação tiver sido bem-sucedida, a fase de seleção do MVPD será ignorada e um controle do WebView apresentará ao usuário a página de logon do MVPD.
- Se a última tentativa de autenticação não tiver sido bem-sucedida ou se o usuário tiver feito logout explicitamente, a variável [*displayProviderDialog()*](#displayProviderDialog) o retorno de chamada é disparado. Seu aplicativo usa esse retorno de chamada para exibir a interface de seleção de MVPD. Além disso, o aplicativo é necessário para retomar o fluxo de autenticação, informando a biblioteca do Ativador de acesso sobre a seleção de MVPD do usuário por meio da [setSelectedProvider()](#setSelectedProvider) método.

Se um MVPD suportar o recurso &quot;Autenticação por solicitante&quot;, vários tokens de autenticação poderão ser armazenados em um dispositivo (um por programador).

Por fim, o status de autenticação é comunicado ao aplicativo por meio da *setAuthenticationStatus()* retorno de chamada.

| Chamada de API: inicia o fluxo de autenticação |
| --- |
| ```public void getAuthentication()``` |

**Disponibilidade:** v1.0+

| Chamada de API: inicia o fluxo de autenticação |
| --- |
| ```public void getAuthentication(boolean forceAuthN, Map<String, Object> genericData)``` |

**Disponibilidade:** v1.0+

**Parâmetros:**

- *forceAuthn*: um sinalizador que especifica se o fluxo de autenticação deve ser iniciado, independentemente de o usuário já estar autenticado ou não.
- *dados*: um Mapa que consiste em pares de valores chave para ser enviado ao serviço de passe de TV por assinatura. O Adobe pode usar esses dados para habilitar funcionalidades futuras sem alterar o SDK.

**Retornos de chamada disparados:** `setAuthenticationStatus(), displayProviderDialog(), sendTrackingData()`

</br>

### displayProviderDialog {#displayProviderDialog}

**Descrição** O retorno de chamada disparado pelo Ativador de acesso para informar ao aplicativo que os elementos apropriados da interface do usuário precisam ser instanciados para permitir que o usuário selecione o MVPD desejado. O retorno de chamada fornece uma lista de objetos MVPD com informações adicionais que podem ajudar a criar corretamente o painel da interface de seleção (como o URL que aponta para o logotipo do MVPD, nome de exibição amigável etc.)

Depois que o usuário selecionar o MVPD desejado, o aplicativo de camada superior deverá retomar o fluxo de autenticação chamando *setSelectedProvider()* e transmitindo a ID do MVPD correspondente à seleção do usuário.


| **Callback: exibir a interface de seleção de MVPD** |
| --- |
| ```public void displayProviderDialog(ArrayList<Mvpd> mvpds)``` |

**Disponibilidade:** v1.0+

**Parâmetros**:

- *mvpds*: lista de objetos MVPD que contêm informações relacionadas ao MVPD que o aplicativo pode usar para criar os elementos da interface de seleção MVPD.

**Acionado por:** `getAuthentication(), getAuthorization()`

</br>

### setSelectedProvider {#setSelectedProvider}

**Descrição:** Esse método é chamado pelo aplicativo para informar o Ativador de acesso sobre a seleção de MVPD do usuário. Ao passar *null* como parâmetro, o Ativador de acesso redefiniu o MVPD atual para um valor nulo.

| **Chamada de API: definir o provedor selecionado no momento** |
| --- |
| ```public void setSelectedProvider(String mvpdId)``` |


**Disponibilidade:**v 1.0+

**Parâmetros:** Nenhum

**Retornos de chamada disparados:** `setAuthenticationStatus(), sendTrackingData()`
</br>

### navigateToUrl {#navigagteToUrl}

**Descrição:** Retorno de chamada acionado pelo Ativador de acesso no Android SDK. Ele deve ser ignorado no SDK do Amazon FireOS.

| **Callback: exibir página de logon do MVPD** |
| --- |
| ```public void navigateToUrl(String url)``` |

**Disponibilidade:** v1.0+

**Parâmetros:**

- *url*: o URL que aponta para a página de logon do MVPD

**Acionado por:** `getAuthentication(), setSelectedProvider()`

</br>

### getAuthenticationToken {#getAuthNToken}

**Descrição:** Conclui o fluxo de autenticação solicitando o token de autenticação do servidor back-end.

| **Chamada de API: recuperar o token de autenticação** |
| --- |
| ```public void getAuthenticationToken(String cookies)``` |

**Disponibilidade:** v1.0+

**Parâmetros:**

- *cookies*: Cookies definidos no domínio de destino (consulte o aplicativo de demonstração no SDK para obter uma implementação de referência).

**Retornos de chamada disparados:** `setAuthenticationStatus(), sendTrackingData()`

</br>

### setAuthenticationStatus {#setAuthNStatus}

**Descrição:** Retorno de chamada acionado pelo Ativador de acesso que informa a aplicação do status da autenticação. Há muitos locais em que o fluxo de autenticação pode falhar, como resultado da interação do usuário ou devido a outros cenários imprevistos (ou seja, problemas de conectividade de rede etc.). Essa chamada de retorno informa a aplicação do status de sucesso/falha da autenticação, além de fornecer informações adicionais sobre o motivo da falha, quando necessário.

Essa chamada de retorno também sinaliza quando o fluxo de logout é concluído.

| **Callback: relatar o status do fluxo de autenticação** |
| --- |
| ```public void setAuthenticationStatus(int status, String errorCode)``` |

**Disponibilidade:** v1.0+

**Parâmetros:**

- *status*: Pode ter um dos seguintes valores:
   - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - fluxo de autenticação concluído com sucesso
   - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - falha no fluxo de autenticação
   - `AccessEnabler.ACCESS_ENABLER_STATUS_LOGOUT` - logout
- *código*: motivo do status apresentado. Se *status* é `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS`, depois *código* é uma cadeia de caracteres vazia (ou seja, definida pela variável `AccessEnabler.USER_AUTHENTICATED` constante). Se não estiver autenticado, esse parâmetro pode ter um dos seguintes valores:
   - `AccessEnabler.USER_NOT_AUTHENTICATED_ERROR` - O usuário não está autenticado. Em resposta à *checkAuthentication()* chamada de método quando não há um token de autenticação válido no cache de token local.
   - `AccessEnabler.PROVIDER_NOT_SELECTED_ERROR` - O AccessEnabler redefiniu a máquina de estado de autenticação depois que o aplicativo de camada superior passou *null* para `setSelectedProvider()` para suspender o fluxo de autenticação.  Provavelmente, o usuário cancelou o fluxo de autenticação (ou seja, pressionou o botão &quot;Voltar&quot;).
   - `AccessEnabler.GENERIC_AUTHENTICATION_ERROR` - O fluxo de autenticação falhou devido a motivos como indisponibilidade de rede ou cancelamento explícito do fluxo de autenticação pelo usuário.
   - `AccessEnabler.LOGOUT` - O usuário não está autenticado devido a uma ação de logout.

**Acionado por:** `checkAuthentication(), getAuthentication(), checkAuthorization()`

</br>

### checkPreauthorizedResources {#checkPreauth}

**Descrição:** Esse método é usado pelo aplicativo para determinar se o usuário já está autorizado a exibir recursos protegidos específicos. O objetivo principal desse método é recuperar informações para usar na decoração da interface do usuário (por exemplo, indicando o status de acesso com ícones de bloqueio e desbloqueio).

| **Chamada de API: definir o provedor selecionado no momento** |
| --- |
| ```public void checkPreauthorizedResources(ArrayList<String> resources)``` |

**Disponibilidade:** v1.0+

**&lt;parameters: span=&quot;&quot; id=&quot;1&quot; translate=&quot;no&quot; /> A variável `resources` é uma matriz de recursos cuja autorização deve ser verificada.** Cada elemento na lista deve ser uma string que representa a ID do recurso. A ID do recurso está sujeita às mesmas limitações que a ID do recurso na `getAuthorization()` ou seja, deve ser um valor acordado estabelecido entre o Programador e o MVPD ou um fragmento de RSS de mídia.

**Retorno de chamada disparado:** `preauthorizedResources()`

</br>

### preauthorizedResources {#preauthResources}

**Descrição:** Retorno de chamada acionado por checkPreauthorizedResources(). Fornece uma lista de recursos que o usuário já está autorizado a visualizar.

| **Chamada de API: definir o provedor selecionado no momento** |
| --- |
| ```public void checkPreauthorizedResources(ArrayList<String> resources)``` |

**Disponibilidade:**v 1.0+

**Parâmetros:** A variável `resources` é uma matriz de recursos para a qual o usuário já está autorizado a visualizar.

**Acionado por:** `checkPreauthorizedResources()`

<br>

### checkAuthorization {#checkAuthZ}

**Descrição:** Esse método é usado pelo aplicativo para verificar o status da autorização. Ela é iniciada verificando o status de autenticação primeiro. Se não estiver autenticado, a variável *setTokenRequestFailed()* O retorno de chamada é acionado e o método é encerrado. Se o usuário estiver autenticado, ele também acionará o fluxo de autorização. Veja os detalhes no *getAuthorization()* método.

| **Chamada de API: verificar status de autorização** |
| --- |
| ```public void checkAuthorization(String resourceId)``` |

**Disponibilidade:** v1.0+

| **Chamada de API: verificar status de autorização** |
| --- |
| ```public void checkAuthorization(String resourceId, Map<String, Object> genericData)``` |

**Disponibilidade:** v1.0+

**Parâmetros:**

- *resourceId*: a ID do recurso para o qual o usuário solicita autorização.
- *dados*: um Mapa que consiste em pares de valores chave para ser enviado ao serviço de passe de TV por assinatura. O Adobe pode usar esses dados para habilitar funcionalidades futuras sem alterar o SDK.

**Retornos de chamada disparados:** `tokenRequestFailed(), setToken(), sendTrackingData(), setAuthenticationStatus()`

</br>

### getAuthorization {#getAuthZ}

**Descrição:** Este método é usado pelo aplicativo para iniciar o fluxo de autorização. Se o usuário ainda não estiver autenticado, ele também iniciará o fluxo de autenticação. Se o usuário for autenticado, o Ativador de acesso continuará a emitir solicitações para o token de autorização (se nenhum token de autorização válido estiver presente no cache do token local) e para o token de mídia de vida curta. Depois que o token de mídia curta é obtido, o fluxo de autorização é considerado concluído. A variável *setToken()* o retorno de chamada é acionado e o token de mídia curto é fornecido como um parâmetro para o aplicativo. Se, por qualquer motivo, a autorização falhar, a *tokenRequestFailed()* O retorno de chamada é disparado e o código de erro e os detalhes são fornecidos.

| **Chamada de API: iniciar o fluxo de autorização** |
| --- |
| ```public void getAuthorization(String resourceId)``` |

**Disponibilidade:** v1.0+

| **Chamada de API: iniciar o fluxo de autorização** |
| --- |
| ```public void getAuthorization(String resourceId, Map<String, Object> genericData)``` |

**Disponibilidade:** v1.0+

**Parâmetros:**

- *resourceId*: a ID do recurso para o qual o usuário solicita autorização.
- *dados*: um Mapa que consiste em pares de valores chave para ser enviado ao serviço de passe de TV por assinatura. O Adobe pode usar esses dados para habilitar funcionalidades futuras sem alterar o SDK.

**Retornos de chamada disparados:** `tokenRequestFailed(), setToken(), sendTrackingData()`

|     |     |
| --- | --- |
| ![](http://learn.adobe.com/wiki/images/icons/emoticons/warning.gif) | **Retornos de chamada adicionais acionados**  <br>Esse método também pode acionar os seguintes retornos de chamada (se o fluxo de autenticação também for iniciado): _setAuthenticationStatus()_, _displayProviderDialog()_ |

**OBSERVAÇÃO: use checkAuthorization() em vez de getAuthorization() sempre que possível. O método getAuthorization() iniciará um fluxo de autenticação completo (se o usuário não estiver autenticado) e isso pode levar a uma implementação complicada por parte do programador.**

</br>

### setToken {#setToken}

**Descrição:** Retorno de chamada acionado pelo Ativador de acesso que informa ao aplicativo que o fluxo de autorização foi concluído com êxito. O token de mídia de vida curta também é fornecido como um parâmetro.

| **Retorno de chamada: fluxo de autorização concluído com êxito** |
| --- |
| ```public void setToken(String token, String resourceId)``` |

**Disponibilidade:**v 1.0+

**Parâmetros:**

- *token*: o token de mídia de vida curta
- *resourceId*: o recurso para o qual a autorização foi obtida

**Acionado por:** `checkAuthorization(), getAuthorization()`

<br>

### tokenRequestFailed {#tokenRequestFailed}

**Descrição:** Retorno de chamada acionado pelo Ativador de acesso que informa ao aplicativo de camada superior que o fluxo de autorização falhou.

| **Retorno de chamada: falha no fluxo de autorização** |
| --- |
| ```public void tokenRequestFailed(String resourceId, <br>        String errorCode, String errorDescription)``` |

**Disponibilidade:** v1.0+

**Parâmetros:**

- *resourceId*: o recurso para o qual a autorização foi obtida
- *errorCode*: código de erro associado ao cenário de falha. Valores possíveis:
   - `AccessEnabler.USER_NOT_AUTHORIZED_ERROR` - O usuário não pôde autorizar para o recurso fornecido
- *errorDescription*: Detalhes adicionais sobre o cenário de falha. Se essa cadeia de caracteres descritiva não estiver disponível por algum motivo, a Autenticação Adobe Pass enviará uma cadeia de caracteres vazia >**(&quot;&quot;)**.  Essa cadeia de caracteres pode ser usada por um MVPD para passar mensagens de erro personalizadas ou mensagens relacionadas a vendas. Por exemplo, se um assinante tiver a autorização negada para um recurso, o MVPD poderá enviar uma mensagem como: &quot;No momento, você não tem acesso a esse canal em seu pacote. Se quiser atualizar seu pacote, clique aqui.&quot; A mensagem é passada pela Autenticação Adobe Pass por meio dessa chamada de retorno ao Programador, que tem a opção de exibi-la ou ignorá-la. A Autenticação do Adobe Pass também pode usar esse parâmetro para fornecer notificação da condição que pode ter levado a um erro. Por exemplo, &quot;Ocorreu um erro de rede ao se comunicar com o serviço de autorização do provedor.&quot;

**Acionado por:** `checkAuthorization(), getAuthorization()`

</br>

### logout {#logout}

**Descrição:** Use esse método para iniciar o fluxo de logout. O logout é o resultado de uma série de operações de redirecionamento HTTP devido ao fato de que o usuário precisa ser desconectado dos servidores de Autenticação do Adobe Pass e também dos servidores do MVPD.

| **Chamada de API: iniciar o fluxo de logout** |
| --- |
| ```public void logout()``` |

**Disponibilidade:** v1.0+

**Parâmetros:** Nenhum

**Retornos de chamada disparados:** Nenhum

</br>

### getSelectedProvider {#getSelectedProvider}

**Descrição:** Use este método para determinar o provedor selecionado no momento.

| **Chamada de API: determine o MVPD selecionado no momento** |
| --- |
| ```public void getSelectedProvider()``` |

**Disponibilidade:** v1.0+

**Parâmetros:** Nenhum

**Retornos de chamada disparados:** `selectedProvider()`

</br>

### seletedProvider {#selectedProvider}

**Descrição:** Retorno de chamada acionado pelo Ativador de acesso que fornece informações sobre o MVPD atualmente selecionado para o aplicativo.

| **Callback: informações sobre o MVPD selecionado no momento** |
| --- |
| ```public void selectedProvider(Mvpd mvpd)``` |

**Disponibilidade:** v1.0+

**Parâmetros:**

- *mvpd*: objeto que contém informações sobre o MVPD selecionado no momento

**Acionado por:** `getSelectedProvider()`

</br>

### getMetadata {#getMetadata}

**Descrição:** Use esse método para recuperar informações expostas como metadados pela biblioteca do Access Enabler. O aplicativo pode acessar essas informações fornecendo um objeto composto MetadataKey.

| **Chamada de API: consulte o AccessEnabler para obter metadados** |
| --- |
| ```public void getMetadata(MetadataKey metadataKey)``` |

**Disponibilidade:** v1.0+

Há dois tipos de metadados disponíveis para programadores:

- Metadados estáticos (TTL do token de autenticação, TTL do token de autorização e ID do dispositivo)
- Metadados do usuário (informações específicas do usuário, como ID do usuário e CEP; transmitidas de um MVPD para o dispositivo de um usuário durante os fluxos de Autenticação e/ou Autorização)

**Parâmetros:**

- *metadataKey*: uma estrutura de dados que encapsula uma variável key e args, com o seguinte significado:
   - Se a chave for `METADATA_KEY_TTL_AUTHN` em seguida, a consulta é feita para obter o tempo de expiração do token de autenticação.
   - Se a chave for `METADATA_KEY_TTL_AUTHZ` e args contém um objeto SerializableNameValuePair com nome = `METADATA_ARG_RESOURCE_ID` e valor = `[resource_id]`, a consulta é feita para obter a hora de expiração do token de autorização associado ao recurso especificado.
   - Se a chave for `METADATA_KEY_DEVICE_ID` em seguida, a consulta é feita para obter a id do dispositivo atual. Observe que esse recurso está desativado por padrão e os programadores devem entrar em contato com o Adobe para obter informações sobre ativação e taxas.
   - Se a chave for `METADATA_KEY_USER_META` e args contém um objeto SerializableNameValuePair com nome = `METADATA_KEY_USER_META` e valor = `[metadata_name]`, então a consulta é feita para metadados do usuário. A lista atual de tipos de metadados de usuário disponíveis:
      - `zip` - CEP
      - `householdID` - Identificador do agregado. Se um MVPD não suportar subcontas, será idêntico a `userID`.
      - `maxRating` - Classificação máxima dos pais para o usuário
      - `userID` - O identificador do usuário. Se um MVPD suportar subcontas e o usuário não for a conta principal,
      - `channelID` - Uma lista de canais que o usuário está autorizado a visualizar

Os metadados reais do usuário disponíveis para um Programador dependem do que um MVPD disponibiliza.  Essa lista será expandida à medida que novos metadados forem disponibilizados e adicionados ao sistema de autenticação da Adobe Pass.

**Retornos de chamada disparados:** [`setMetadataStatus()`](#setMetadaStatus)

**Mais informações:** [Metadados do usuário](#setmetadatastatus)

</br>

### setMetadataStatus {#setMetadaStatus}

**Descrição:** Retorno de chamada acionado pelo Ativador de acesso que entrega os metadados solicitados por meio de um *getMetadata()* chame.

| **Callback: resultado de solicitação de recuperação de metadados** |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**Disponibilidade:** v1.0+

**Parâmetros:**

- *key*: o objeto MetadataKey que contém a chave para a qual o valor de metadados é solicitado e os parâmetros associados (consulte o aplicativo de demonstração para obter uma implementação de referência).
- *resultado*: um objeto composto que contém os metadados solicitados. O objeto tem os seguintes campos:
   - *simpleResult*: uma string que representa o valor dos metadados quando a solicitação foi feita para TTL de autenticação, TTL de autorização ou ID do dispositivo. Esse valor será nulo se a solicitação tiver sido feita para Metadados do usuário.

   - *userMetadataResult*: um objeto que contém a representação Java de uma carga de metadados do usuário JSON. Por exemplo:

     ```json
     {
     "street": "Main Avenue",
     "buildings": ["150", "320"]
     }
     ```

     é traduzido para o Java como:

     ```java
     Map("street" -> "Main Avenue", "buildings" -> List("150", "320")))
     ```

     **A estrutura real dos objetos de metadados do usuário é semelhante ao seguinte:**

     ```json
     {
         updated: 1334243471,
         encrypted: ["encryptedProp"],
         data: {
             zip: ["12345", "34567"],
             maxRating: { 
                 "MPAA": "PG-13",
                 "VCHIP": "TV-Y", 
                 "URL": "http://exam.pl/e/manage/ratings"
             },
             householdID: "3456",
             userID: "BgSdasfsdk23/dsaf3+saASesadgfsShggssd=",
             channelID: ["channel-1", "channel-2"]
         }
     }
     ```


Esse valor é nulo quando a solicitação foi feita para metadados simples (TTL de autenticação, TTL de autorização ou ID do dispositivo).

- *criptografado*: valor booliano que especifica se os metadados recuperados estão criptografados ou não. Esse parâmetro é importante apenas para solicitações de metadados do usuário, não tem significado para metadados estáticos (por exemplo, TTL de autenticação) que são sempre recebidos não criptografados. Se esse parâmetro for definido como True, cabe ao Programador obter o valor de Metadados do usuário não criptografado executando uma descriptografia RSA usando a chave privada da lista de permissões (a mesma chave privada usada para a assinatura da ID do solicitante no [`setRequestor`](#setRequestor) chamada de ).

**Acionado por:** [`getMetadata()`](#getMetadata)

**Mais informações:** [Metadados do usuário](/help/authentication/user-metadata.md)

</br>

## getVersion {#getVersion}

**Descrição:** Use este método para recuperar a versão atual do AccessEnabler

| **Chamada de API: obter versão do AccessEnabler** |
| --- |
| ```public static String getVersion()``` |

## Rastreamento de eventos {#tracking}

O Ativador de acesso aciona um retorno de chamada adicional que não é necessariamente relacionado aos fluxos de direito. Implementar a função de retorno de chamada de rastreamento de eventos chamada *sendTrackingData()* é opcional, mas permite que o aplicativo rastreie eventos específicos e compile estatísticas, como o número de tentativas de autenticação/autorização bem-sucedidas/com falha. Abaixo está a especificação para o *sendTrackingData()* retorno de chamada:

### sendTrackingData {#sendTrackingData}

**Descrição:** Retorno de chamada acionado pelo Ativador de acesso que sinaliza para o aplicativo a ocorrência de vários eventos, como a conclusão/falha de fluxos de autenticação/autorização. O tipo de dispositivo, o tipo de cliente Access Enabler e o sistema operacional também são relatados por sendTrackingData().

>[!WARNING]
>
> O tipo de dispositivo e o sistema operacional são derivados por meio do uso de uma biblioteca Java pública (http://java.net/projects/user-agent-utils) e a sequência de agente do usuário. Esteja ciente de que essas informações são fornecidas apenas como uma forma grosseira de dividir as métricas operacionais em categorias de dispositivos, mas esse Adobe não pode assumir nenhuma responsabilidade por resultados incorretos. Use a nova funcionalidade adequadamente.

- Valores possíveis para o tipo de dispositivo:
   - `computer`
   - `tablet`
   - `mobile`
   - `gameconsole`
   - `unknown`

- Valores possíveis para o tipo de cliente do Access Enabler:
   - `flash`
   - `html5`
   - `ios`
   - `tvos`
   - `android`
   - `firetv`

| Retorno de chamada: rastreamento de eventos |
| --- |
| ```public void sendTrackingData(Event event, ArrayList<String> data)``` |

**Disponibilidade:** v1.0+

**Parâmetros:**

- *evento*: o evento que está sendo rastreado. Há três tipos possíveis de eventos de rastreamento:
   - **authorizationDetection:** sempre que uma solicitação de token de autorização for retornada (o tipo de evento é `EVENT_AUTHZ_DETECTION`)
   - **authenticationDetection:** sempre que ocorrer uma verificação de autenticação (o tipo de evento é `EVENT_AUTHN_DETECTION`)
   - **mvpdSelection:** quando o usuário seleciona um MVPD no formulário de seleção MVPD (o tipo de evento é `EVENT_MVPD_SELECTION`)
- *dados*: dados adicionais associados ao evento relatado. Esses dados são apresentados no formato de uma lista de valores.

A seguir estão instruções para interpretar os valores na variável *dados* matriz:

- Para tipo de evento *`EVENT_AUTHN_DETECTION`:*
   - **0** - Se a solicitação de token foi bem-sucedida (true/false) e se o exposto acima for true:
   - **1** - Sequência de caracteres de ID MVPD
   - **2** - GUID (md5 com hash)
   - **3** - Token já no cache (true/false)
   - **4** - Tipo de dispositivo
   - **5** - Tipo de cliente do Access Enabler
   - **6** - Tipo de sistema operacional

- Para tipo de evento `EVENT_AUTHZ_DETECTION`
   - **0** - Se a solicitação de token foi bem-sucedida (true/false) e, se bem-sucedida:
   - **1** - ID MVPD
   - **2** - GUID (md5 com hash)
   - **3** - Token já no cache (true/false)
   - **4** - Erro
   - **5** - Detalhes
   - **6** - Tipo de dispositivo
   - **7** - Tipo de cliente do Access Enabler
   - **8** - Tipo de sistema operacional

- Para tipo de evento `EVENT_MVPD_SELECTION`
   - **0** - ID do MVPD selecionado no momento
   - **1** - Tipo de dispositivo
   - **2** - Tipo de cliente do Access Enabler
   - **3** - Tipo de sistema operacional

**Acionado por:** `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`
