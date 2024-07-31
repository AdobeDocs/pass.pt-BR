---
title: Referência da API do SDK do Android
description: Referência da API do SDK do Android
exl-id: f932e9a1-2dbe-4e35-bd60-a4737407942d
source-git-commit: e4567dd870790d11b9c5904d635fc00bdd1a23d2
workflow-type: tm+mt
source-wordcount: '4537'
ht-degree: 0%

---

# Referência da API do SDK do Android {#android-sdk-api-reference}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#intro}

Este documento detalha os métodos e retornos de chamada expostos pelo SDK do Android para autenticação da Adobe Pass, compatível com a Autenticação da Adobe Pass versões 1.7 e posteriores. Os métodos e as funções de retorno de chamada descritos aqui são definidos nos arquivos de cabeçalho AccessEnabler.h e EntitlementDelegate.h.

Consulte [https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library](https://tve.zendesk.com/hc/en-us/articles/204963219-Android-Native-AccessEnabler-Library) para obter o SDK do Android AccessEnabler mais recente.


**Observação:** a equipe de Autenticação da Adobe Pass incentiva você a usar somente as APIs *públicas* de Autenticação da Adobe Pass:

- APIs públicas estão disponíveis *e totalmente testadas* em todos os tipos de clientes com suporte. Para qualquer recurso público, garantimos que cada tipo de cliente tenha uma versão correspondente dos métodos associados.</span>
- As APIs públicas devem ser o mais estáveis possível, para oferecer compatibilidade com versões anteriores e garantir que as integrações de parceiros não sejam interrompidas. No entanto, para APIs *não*-públicas, reservamos o direito de alterar sua assinatura em qualquer ponto futuro. Se você encontrar um fluxo específico que não possa ser suportado por meio de uma combinação das chamadas de API de autenticação do Adobe Pass públicas atuais, a melhor abordagem é informar o. Levando em consideração suas necessidades, podemos modificar as APIs públicas e fornecer uma solução estável a partir de agora.

## API do Android {#api}

- [getInstance](#getInstance)
- [setOptions](#setOptions)
- [setRequestor](#setRequestor)
- [setRequestorComplete](#setRequestorComplete)
- [checkAuthentication](#checkAuthN)
- [getAuthentication](#getAuthN)
- [displayProviderDialog](#displayProviderDialog)
- [setSelectedProvider](#setSelectedProvider)
- [navigateToUrl](#navigagteToUrl)
- [getAuthenticationToken](#getAuthNToken)
- [setAuthenticationStatus](#setAuthNStatus)
- pré-autorizar
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

### Factory.getInstance {#getInstance}

**Descrição:** Instancia o objeto do Ativador de Acesso. Deve haver uma única instância do Access Enabler por instância do aplicativo.

| Chamada de API: construtor |
| --- |
| **estática pública** AccessEnabler **getInstance**(Context appContext, String softwareStatement, String redirectUrl)<br>        **throws** AccessEnablerException <br><br>**public static** AccessEnabler getInstance(Context appContext, String env_url, String softwareStatement, String redirectUrl)<br>**throws** AccessEnablerException |

**Disponibilidade:** v3.1.2+

**Parâmetros:**

- *appContext*: contexto de aplicativo do Android.
- env\_url: para testar usando o ambiente de preparo do Adobe, env\_url pode ser definido como &quot;sp.auth-staging.adobe.com&quot;

**Obsoleto:**

```
    public static AccessEnabler getInstance(Context appContext)
        throws AccessEnablerException
```



### setRequestor {#setRequestor}

**Descrição:** Define a identidade do Programador. Cada programador recebe uma ID exclusiva ao se registrar no Adobe para o sistema de autenticação da Adobe Pass. Ao lidar com SSO e tokens remotos, o estado de autenticação pode mudar quando o aplicativo estiver em segundo plano, setRequestor pode ser chamado novamente quando o aplicativo for colocado em primeiro plano para sincronizar com o estado do sistema (busque um token remoto se o SSO estiver habilitado ou exclua o token local se um logout tiver ocorrido enquanto isso).

A resposta do servidor contém uma lista de MVPDs juntamente com algumas informações de configuração anexadas à identidade do Programador. A resposta do servidor é usada internamente pelo código Access Enabler. Somente o status da operação (ou seja, SUCCESS/FAIL) é apresentado ao seu aplicativo por meio do retorno de chamada setRequestorComplete().

Se o parâmetro *urls* não for usado, a chamada de rede resultante será direcionada ao URL do provedor de serviços padrão: o ambiente de Liberação/Produção do Adobe.

Se um valor for fornecido para o parâmetro *urls*, a chamada de rede resultante será direcionada a todas as URLs fornecidas no parâmetro *urls*. Todas as solicitações de configuração são acionadas simultaneamente em threads separados. O primeiro respondente tem prioridade ao compilar a lista de MVPDs. Para cada MVPD na lista, o Ativador de acesso lembra o URL do provedor de serviços associado. Todas as solicitações de direito subsequentes são direcionadas ao URL associado ao provedor de serviços que foi emparelhado com o MVPD de destino durante a fase de configuração.

| Chamada de API: configuração do solicitante |
| --- |
| ```public void setRequestor(String requestorId)``` |

**Disponibilidade:** v3.0+

| Chamada de API: configuração do solicitante |
| --- |
| ```public void setRequestor(String requestorId, ArrayList<String> urls)``` |

**Disponibilidade:** v3.0+


**Parâmetros:**

- *requestorID*: a identificação exclusiva associada ao Programador. Transmita o identificador exclusivo atribuído pelo Adobe ao seu site quando você se registrou pela primeira vez no serviço de autenticação da Adobe Pass.

- *signedRequestorID*: uma cópia da ID do solicitante que é assinada digitalmente com sua chave privada. <!--For more details. see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

- *urls*: parâmetro opcional; por padrão, o provedor de serviços da Adobe é usado (http://sp.auth.adobe.com/). Essa matriz permite especificar endpoints para serviços de autenticação e autorização fornecidos pelo Adobe (instâncias diferentes podem ser usadas para fins de depuração). Você pode usar essa opção para especificar várias instâncias do provedor de serviços de Autenticação da Adobe Pass. Ao fazer isso, a lista MVPD é composta pelos endpoints de todos os provedores de serviços. Cada MVPD está associado ao provedor de serviços mais rápido; ou seja, o provedor que respondeu primeiro e que oferece suporte a esse MVPD.

**Retornos de chamada disparados:** `setRequestorComplete()`

Obsoleto:

    public void setRequestor(String requestorId, String signedRequestorId)
    
    public void setRequestor (String requestorId, String signedRequestorId, ArrayList&lt;String> urls)

[Voltar à API do Android...](#api)

### setRequestorComplete {#setRequestorComplete}

**Descrição:** Retorno de chamada disparado pelo Habilitador de Acesso que informa ao aplicativo que a fase de configuração foi concluída. Esse é um sinal de que o aplicativo pode começar a emitir solicitações de direito. Nenhuma solicitação de direito pode ser emitida pelo aplicativo até que a fase de configuração seja concluída.

| Retorno de chamada: configuração do solicitante concluída |
| --- |
| java public void setRequestorComplete(int status) |

**Disponibilidade:** v1.0+

**Parâmetros:**

- *status*: pode assumir um dos seguintes valores:
   - SDK \>= 3.4.0
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - a fase de configuração foi concluída com êxito
      - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - falha na fase de configuração
   - SDK \&lt; 3.4
      - `AccessEnabler.ACCESS_ENABLER_STATUS_SUCCESS` - a fase de configuração foi concluída com êxito
      - `AccessEnabler.ACCESS_ENABLER_STATUS_ERROR` - falha na fase de configuração

**Acionado por:** `setRequestor()`

[Voltar à API do Android...](#api)

### setOptions {#setOptions}

**Descrição:** Configura as opções globais do SDK. Aceita um **Map\&lt;String, String\>** como argumento. Os valores do mapa serão passados para o servidor junto com cada chamada de rede feita pelo SDK.

Os valores serão passados ao servidor independentemente do fluxo atual (autenticação/autorização). Se quiser alterar os valores, você pode chamar esse método a qualquer momento.

| Chamada de API: setOptions |
| --- |
| public void setOptions(HashMap&lt;String,String> opções) |

**Disponibilidade:** v1.9.2+

**Parâmetros:**

- *opções*: um mapa&lt;cadeia de caracteres, cadeia de caracteres> contendo opções globais do SDK. Atualmente, as seguintes opções estão disponíveis:
   - **applicationProfile** - Ele pode ser usado para fazer configurações de servidor baseadas nesse valor.
   - **ap_vi** - A ID do Experience Cloud (visitorID). Esse valor pode ser usado posteriormente para relatórios de análise avançada.
   - **ap_ai** - A Advertising ID
   - **device_info** - Informações do cliente conforme descrito aqui: [Passando informações do cliente para a conexão e o aplicativo do dispositivo](/help/authentication/passing-client-information-device-connection-and-application.md).

[Voltar ao início...](#apis)


### checkAuthentication {#checkAuthN}

**Descrição:** Verifica o status de autenticação. Ele faz isso procurando um token de autenticação válido no espaço de armazenamento de token local. Esse método não executa chamadas de rede e recomendamos chamá-lo na thread principal. Ele é usado pelo aplicativo para consultar o status de autenticação do usuário e atualizar a interface de acordo (ou seja, atualizar a interface de logon/logout). O status de autenticação é comunicado ao aplicativo por meio do retorno de chamada [*setAuthenticationStatus()*](#setAuthNStatus).

Se um MVPD suportar o recurso &quot;Autenticação por solicitante&quot;, vários tokens de autenticação poderão ser armazenados em um dispositivo.  Para obter detalhes sobre esse recurso, consulte a seção [Diretrizes de armazenamento em cache](#$caching) na Visão geral técnica da Android.

| Chamada de API: verificar status de autenticação |
| --- |
| public void checkAuthentication() |

**Disponibilidade:** v1.0+

**Parâmetros:** Nenhum

**Retornos de chamada disparados:** `setAuthenticationStatus()`

[Voltar à API do Android...](#api)


### getAuthentication {#getAuthN}

**Descrição:** inicia o fluxo de trabalho de autenticação completa. Ele é iniciado verificando o status de autenticação. Se ainda não estiver autenticado, a máquina de estado do fluxo de autenticação é iniciada:

- Se a última tentativa de autenticação tiver sido bem-sucedida, a fase de seleção do MVPD será ignorada e o retorno de chamada [*navigateToUrl()*](#navigagteToUrl) será acionado. O aplicativo usa essa chamada de retorno para instanciar o controle WebView que apresenta ao usuário a página de login do MVPD.
- Se a última tentativa de autenticação não tiver sido bem-sucedida ou se o usuário tiver feito logout explicitamente, o retorno de chamada [*displayProviderDialog()*](#displayProviderDialog) será acionado. Seu aplicativo usa esse retorno de chamada para exibir a interface de seleção de MVPD. Além disso, o aplicativo é necessário para retomar o fluxo de autenticação, informando a biblioteca do Ativador de Acesso sobre a seleção de MVPD do usuário por meio do método [setSelectedProvider()](#setSelectedProvider).

À medida que as credenciais do usuário são verificadas na página de logon do MVPD, sua aplicação é solicitada a monitorar as várias operações de redirecionamento que ocorrem enquanto o usuário é autenticado na página de logon do MVPD. Quando as credenciais corretas são inseridas, o controle WebView é redirecionado para uma URL personalizada definida pela constante *AccessEnabler.ADOBEPASS\_REDIRECT\_URL*. Este URL não deve ser carregado pelo WebView. O aplicativo deve interceptar esse URL e interpretar esse evento como um sinal de que a fase de logon foi concluída. Em seguida, ele deve entregar o controle ao Access Enabler para concluir o fluxo de autenticação (chamando o método *getAuthenticationToken()*).

Se um MVPD suportar o recurso &quot;Autenticação por solicitante&quot;, vários tokens de autenticação poderão ser armazenados em um dispositivo (um por programador).  Para obter detalhes sobre esse recurso, consulte a seção [Diretrizes de armazenamento em cache](#$caching) na Visão geral técnica da Android.

Finalmente, o status de autenticação é comunicado ao aplicativo por meio do retorno de chamada *setAuthenticationStatus()*.



| Chamada de API: inicia o fluxo de autenticação |
| --- |
| public void getAuthentication() |

**Disponibilidade:** v1.0+

| Chamada de API: inicia o fluxo de autenticação |
| --- |
| public void getAuthentication(boolean forceAuthN, Map&lt;String, Object> genericData) |

**Disponibilidade:** v1.8+

**Parâmetros:**

- *forceAuthn*: um sinalizador que especifica se o fluxo de autenticação deve ser iniciado, independentemente de o usuário já estar autenticado ou não.
- *dados*: um mapa que consiste em pares de valores chave a serem enviados para o serviço de passe de TV por Assinatura. O Adobe pode usar esses dados para habilitar funcionalidades futuras sem alterar o SDK.

**Retornos de chamada disparados:** `setAuthenticationStatus(), displayProviderDialog(), navigateToUrl(), sendTrackingData()`


[Voltar à API do Android...](#api)

### displayProviderDialog {#displayProviderDialog}

**Descrição** Retorno de chamada disparado pelo Habilitador de Acesso para informar ao aplicativo que os elementos apropriados da interface do usuário precisam ser instanciados para permitir que o usuário selecione o MVPD desejado. O retorno de chamada fornece uma lista de objetos MVPD com informações adicionais que podem ajudar a criar corretamente o painel da interface de seleção (como o URL que aponta para o logotipo do MVPD, nome de exibição amigável etc.)

Depois que o usuário seleciona o MVPD desejado, o aplicativo de camada superior deve retomar o fluxo de autenticação, chamando *setSelectedProvider()* e transmitindo a ID do MVPD correspondente à seleção do usuário.

>[!NOTE]
>
> Anulando o fluxo de autenticação
> </br></br>
> Observe que esse é um ponto em que o usuário pode pressionar o botão &quot;Voltar&quot;, o que é equivalente à anulação do fluxo de autenticação. Nesse cenário, seu aplicativo deve chamar o método `setSelectedProvider()`, transmitindo *null* como o parâmetro, para dar ao Ativador de Acesso a oportunidade de redefinir sua máquina de estado de autenticação.

| Callback: exibir a interface de seleção de MVPD |
| --- |
| `public void displayProviderDialog(ArrayList<Mvpd> mvpds)` |

**Disponibilidade:** v1.0+

**Parâmetros**:

- *mvpds*: Lista de objetos MVPD que contêm informações relacionadas ao MVPD que o aplicativo pode usar para criar os elementos da interface de seleção MVPD.

**Acionado por:** `getAuthentication(), getAuthorization()`

[Voltar à API do Android...](#api)


### setSelectedProvider {#setSelectedProvider}

**Descrição:** Esse método é chamado pelo seu aplicativo para informar ao Ativador de Acesso a seleção de MVPD do usuário. O aplicativo pode usar esse método para selecionar ou alterar o provedor de serviços usado para autenticação.

Se o MVPD selecionado for um MVPD TempPass, ele será autenticado automaticamente com esse MVPD sem precisar chamar getAuthentication() posteriormente.

Observe que isso não é possível para a Passagem Temp Promocional, onde parâmetros extras são fornecidos no método getAuthentication().

Ao passar *null* como parâmetro, o Ativador de Acesso presume que o usuário cancelou o fluxo de autenticação (ou seja, pressionou o botão &quot;Voltar&quot;) e responde redefinindo a máquina de estado de autenticação e chamando o retorno de chamada *setAuthenticationStatus()* com o código de erro `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR`.

| Chamada de API: definir o provedor selecionado no momento |
| --- |
| public void setSelectedProvider(Cadeia de caracteres mvpdId) |

**Disponibilidade:** v1.0+

**Parâmetros:** Nenhum

**Retornos de chamada disparados:** `setAuthenticationStatus(), sendTrackingData(), navigateToUrl()`

[Voltar à API do Android...](#api)


### navigateToUrl {#navigagteToUrl}

**Obsoleto:** a partir do Android SDK 3.0, navigateToUrl será usado somente se a Guia Personalizada do Chrome não estiver presente no dispositivo

**Descrição:** Retorno de chamada disparado pelo Habilitador de Acesso, que informa ao aplicativo que o usuário precisa ter a página de logon do MVPD para inserir suas credenciais. O Ativador de acesso passa como parâmetro o URL da página de logon do MVPD. Seu aplicativo é necessário para instanciar um controle WebView e direcioná-lo para este URL. Além disso, o aplicativo é necessário para monitorar as URLs carregadas pelo controle do WebView e interceptar a operação de redirecionamento direcionada à URL personalizada definida pela constante `AccessEnabler.ADOBEPASS_REDIRECT_URL (deprecated)`. Após este evento, o aplicativo deve fechar ou ocultar o controle WebView e devolver o controle à biblioteca do Access Enabler, chamando o método *getAuthenticationToken()*. O Ativador de acesso conclui o fluxo de autenticação recuperando o token de autenticação do servidor back-end e armazenando-o localmente no armazenamento de token.

>[!WARNING]
>
> **Anulando o fluxo de autenticação** <br>Observe que este é um ponto em que o usuário pode pressionar o botão &quot;Voltar&quot;, o que é equivalente à anulação do fluxo de autenticação. Nesse cenário, o aplicativo deve chamar o método _setSelectedProvider()_ transmitindo _null_ como parâmetro e permitindo que o Habilitador de Acesso redefina seu computador de estado de autenticação.

| Callback: exibir página de logon do MVPD |
| --- |
| public void navigateToUrl(String url) |

**Disponibilidade:** v1.0+

**Parâmetros:**

- *url*: a URL que aponta para a página de logon do MVPD

**Acionado por:** `getAuthentication(), setSelectedProvider()`

[Voltar à API do Android...](#api)


### getAuthenticationToken {#getAuthNToken}

**Obsoleto:** a partir do Android SDK 3.0, já que a Guia Personalizada do Chrome é usada para autenticação, esse método não é mais usado no aplicativo.

**Descrição:** conclui o fluxo de autenticação solicitando o token de autenticação do servidor back-end. Esse método deve ser chamado pelo aplicativo somente em resposta ao evento em que o controle WebView que hospeda a página de logon MVPD é redirecionado para a URL personalizada definida pela constante `AccessEnabler.ADOBEPASS_REDIRECT_URL`.

| Chamada de API: recuperar o token de autenticação |
| --- |
| public void getAuthenticationToken() |

**Disponibilidade:** v1.0+

**Parâmetros:**

- *cookies*: cookies definidos no domínio de destino (consulte o aplicativo de demonstração no SDK para obter uma implementação de referência).

**Retornos de chamada disparados:** `setAuthenticationStatus()`, `sendTrackingData()`

[Voltar à API do Android...](#api)


### setAuthenticationStatus {#setAuthNStatus}

**Descrição:** Retorno de chamada disparado pelo Habilitador de Acesso que informa
a aplicação do status do fluxo de autenticação. Há muitos
locais em que esse fluxo pode falhar, como resultado da
devido a outros cenários imprevistos (ou seja, a capacidade da rede para
conectividade, etc.). Esse retorno de chamada informa a aplicação de
o status de sucesso/falha do fluxo de autenticação, enquanto também
fornecer informações adicionais sobre o motivo da falha, quando necessário.

| Callback: relatar o status do fluxo de autenticação |
| --- |
| public void setAuthenticationStatus(int status, String errorCode) |

**Disponibilidade:** v1.0+

**Parâmetros:**

- *status*: pode assumir um dos seguintes valores:
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS` - fluxo de autenticação concluído com êxito
   - `AccessEnablerConstants.ACCESS_ENABLER_STATUS_ERROR` - falha no fluxo de autenticação
- *código*: motivo da falha. Se o *status* for `AccessEnablerConstants.ACCESS_ENABLER_STATUS_SUCCESS`, então o *código* será uma cadeia de caracteres vazia (isto é, definida pela constante `AccessEnablerConstants.USER_AUTHENTICATED`). Em caso de falha, esse parâmetro pode ter um dos seguintes valores:
   - `AccessEnablerConstants.USER_NOT_AUTHENTICATED_ERROR` - Usuário não autenticado. Em resposta à chamada do método *checkAuthentication()* quando não há um token de autenticação válido no cache de token local.
   - `AccessEnablerConstants.PROVIDER_NOT_SELECTED_ERROR` - O AccessEnabler redefiniu a máquina de estado de autenticação depois que o aplicativo de camada superior passou *null* para `setSelectedProvider()` para anular o fluxo de autenticação.  Provavelmente, o usuário cancelou o fluxo de autenticação (ou seja, pressionou o botão &quot;Voltar&quot;).
   - `AccessEnablerConstants.GENERIC_AUTHENTICATION_ERROR` - Falha no fluxo de autenticação devido a motivos como indisponibilidade da rede ou cancelamento explícito do fluxo de autenticação pelo usuário.

**Acionado por:** `checkAuthentication(), getAuthentication(), checkAuthorization()`

[Voltar à API do Android...](#api)


### checkPreauthorizedResources {#checkPreauth}

>**Obsoleto:** a partir do Android SDK 3.6, a API pré-autorizada substitui checkPreauthorizedResources, fornecendo códigos de erro estendidos.

**Descrição:** Este método é usado pelo aplicativo para determinar se o usuário já está autorizado a exibir recursos protegidos específicos. O objetivo principal desse método é recuperar informações para usar na decoração da interface do usuário (por exemplo, indicando o status de acesso com ícones de bloqueio e desbloqueio).

| Chamada de API: definir o provedor selecionado no momento |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Disponibilidade:** v1.3+

**Parâmetros:** O parâmetro `resources` é uma matriz de recursos cuja autorização deve ser verificada. Cada elemento na lista deve ser uma string que representa a ID do recurso. A ID do recurso está sujeita às mesmas limitações que a ID do recurso na chamada `getAuthorization()`, ou seja, ela deve ser um valor acordado estabelecido entre o Programador e o MVPD ou um fragmento de RSS de mídia.

**Retorno de chamada disparado:** `preauthorizedResources()`

[Voltar à API do Android...](#api)


### checkPreauthorizedResources {#checkPreauth2}

**Obsoleto:** a partir do Android SDK 3.6, a API pré-autorizada substitui checkPreauthorizedResources, fornecendo códigos de erro estendidos.

**Descrição:** Este método é usado pelo aplicativo para determinar se o usuário já está autorizado a exibir recursos protegidos específicos. O objetivo principal desse método é recuperar informações para usar na decoração da interface do usuário (por exemplo, indicando o status de acesso com ícones de bloqueio e desbloqueio).

| Chamada de API: definir o provedor selecionado no momento |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources, boolean cache)` |

**Disponibilidade:** v3.1+

**Parâmetros:** O parâmetro `resources` é uma matriz de recursos cuja autorização deve ser verificada. Cada elemento na lista deve ser uma string que representa a ID do recurso. A ID do recurso está sujeita às mesmas limitações que a ID do recurso na chamada `getAuthorization()`, ou seja, ela deve ser um valor acordado estabelecido entre o Programador e o MVPD ou um fragmento de RSS de mídia.

O parâmetro `cache` especifica se a resposta de pré-autorização em cache pode ser usada ou não. Por padrão, o cache é verdadeiro. O SDK retornará uma resposta armazenada em cache anteriormente, se disponível.

**Retorno de chamada disparado:** `preauthorizedResources()`

[Voltar à API do Android...](#api)

### preauthorizedResources {#preauthResources}

**Obsoleto:** a partir do Android SDK 3.6, a API pré-autorizada substitui checkPreauthorizedResources, fornecendo códigos de erro estendidos. O retorno de chamada preauthorizedResources não será chamado na nova API.


**Descrição:** Retorno de chamada disparado por checkPreauthorizedResources(). Fornece uma lista de recursos que o usuário já está autorizado a visualizar.

| Chamada de API: definir o provedor selecionado no momento |
| --- |
| `public void checkPreauthorizedResources(ArrayList<String> resources)` |

**Disponibilidade:** v1.3+

**Parâmetros:** O parâmetro `resources` é uma matriz de recursos para a qual o usuário já está autorizado a visualizar.

**Acionado por:** `checkPreauthorizedResources()`

[Voltar à API do Android...](#api)

### <span id="checkAuthZ"></span>checkAuthorization

**Descrição:** Este método é usado pelo aplicativo para verificar o status de autorização. Ela é iniciada verificando o status de autenticação primeiro. Se não for autenticado, o retorno de chamada *setTokenRequestFailed()* será acionado e o método será encerrado. Se o usuário estiver autenticado, ele também acionará o fluxo de autorização. Consulte detalhes sobre o método *getAuthorization()*.

| Chamada de API: verificar status de autorização |
| --- |
| public void checkAuthorization(String resourceId) |

**Disponibilidade:** v1.0+

| Chamada de API: verificar status de autorização |
| --- |
| public void checkAuthorization(String resourceId, Map&lt;String, Object> genericData) |

**Disponibilidade:** v1.8+

**Parâmetros:**

- *resourceId*: a identificação do recurso para o qual o usuário solicita autorização.
- *dados*: um mapa que consiste em pares de valores chave a serem enviados para o serviço de passe de TV por Assinatura. O Adobe pode usar esses dados para habilitar funcionalidades futuras sem alterar o SDK.

**Retornos de chamada disparados:** `tokenRequestFailed(), setToken(),sendTrackingData(), setAuthenticationStatus()`

[Voltar à API do Android...](#api)


### <span id="getAuthZ"></span>getAuthorization

**Descrição:** Este método é usado pelo aplicativo para iniciar o fluxo de autorização. Se o usuário ainda não estiver autenticado, ele também iniciará o fluxo de autenticação. Se o usuário for autenticado, o Ativador de acesso continuará a emitir solicitações para o token de autorização (se nenhum token de autorização válido estiver presente no cache do token local) e para o token de mídia de vida curta. Depois que o token de mídia curta é obtido, o fluxo de autorização é considerado concluído. O retorno de chamada *setToken()* é disparado e o token de mídia curto é entregue como parâmetro ao aplicativo. Se, por qualquer motivo, a autorização falhar, o retorno de chamada *tokenRequestFailed()* será acionado, e o código de erro e os detalhes serão fornecidos.

| Chamada de API: iniciar o fluxo de autorização |
| --- |
| public void getAuthorization(String resourceId) |

**Disponibilidade:** v1.0+

| Chamada de API: iniciar o fluxo de autorização |
| --- |
| public void getAuthorization(String resourceId, Map&lt;String, Object> genericData) |

**Disponibilidade:** v1.8+

**Parâmetros:**

- *resourceId*: a identificação do recurso para o qual o usuário solicita autorização.
- *dados*: um mapa que consiste em pares de valores chave a serem enviados para o serviço de passe de TV por Assinatura. O Adobe pode usar esses dados para habilitar funcionalidades futuras sem alterar o SDK.

**Retornos de chamada disparados:** `tokenRequestFailed(), setToken(), sendTrackingData()`

>[!WARNING]
>
> **Retornos de chamada adicionais disparados** <br> Este método também pode disparar os seguintes retornos de chamada (se o fluxo de autenticação também for iniciado): *setAuthenticationStatus()*, *displayProviderDialog()*, *navigateToUrl()*

**OBSERVAÇÃO: use checkAuthorization() em vez de getAuthorization() sempre que possível. O método getAuthorization() iniciará um fluxo de autenticação completo (se o usuário não estiver autenticado) e isso pode levar a uma implementação complicada por parte do Programador.**

[Voltar à API do Android...](#api)


### setToken {#setToken}

**Descrição:** Retorno de chamada disparado pelo Habilitador de Acesso que informa ao aplicativo que o fluxo de autorização foi concluído com êxito. O token de mídia de vida curta também é fornecido como um parâmetro.

| Retorno de chamada: fluxo de autorização concluído com êxito |
| --- |
| public void setToken(Token de string, ResourceId de string) |

**Disponibilidade:** v1.0+

**Parâmetros:**

- *token*: o token de mídia de vida curta
- *resourceId*: o recurso para o qual a autorização foi obtida

**Acionado por:** `checkAuthorization()`, `getAuthorization()`


[Voltar à API do Android...](#api)

### tokenRequestFailed {#tokenRequestFailed}

**Descrição:** Retorno de chamada disparado pelo Habilitador de Acesso que informa ao aplicativo de camada superior que o fluxo de autorização falhou.

| Retorno de chamada: falha no fluxo de autorização |
| --- |
| public void tokenRequestFailed(String resourceId, <br>        String errorCode, String errorDescription) |

**Disponibilidade:** v1.0+

**Parâmetros:**

- *resourceId*: o recurso para o qual a autorização foi obtida
- *errorCode*: código de erro associado ao cenário de falha. Valores possíveis:
   - `AccessEnablerConstants.USER_NOT_AUTHORIZED_ERROR` - O usuário não pôde autorizar para o recurso fornecido
- *errorDescription*: detalhes adicionais sobre o cenário de falha. Se essa cadeia de caracteres descritiva não estiver disponível por algum motivo, a Autenticação Adobe Pass enviará uma cadeia de caracteres vazia **(&quot;)**.

  Essa cadeia de caracteres pode ser usada por um MVPD para passar mensagens de erro personalizadas ou mensagens relacionadas a vendas. Por exemplo, se um assinante tiver a autorização negada para um recurso, o MVPD poderá enviar uma mensagem como: &quot;No momento, você não tem acesso a esse canal em seu pacote. Se quiser atualizar seu pacote, clique aqui.&quot; A mensagem é passada pela Autenticação Adobe Pass por meio dessa chamada de retorno ao Programador, que tem a opção de exibi-la ou ignorá-la. A Autenticação do Adobe Pass também pode usar esse parâmetro para fornecer notificação da condição que pode ter levado a um erro. Por exemplo, &quot;Ocorreu um erro de rede ao se comunicar com o serviço de autorização do provedor.&quot;

**Acionado por:** `checkAuthorization(), getAuthorization()`

[Voltar à API do Android...](#api)

### logout {#logout}

**Descrição:** Use este método para iniciar o fluxo de logout. O logout é o resultado de uma série de operações de redirecionamento HTTP devido ao fato de que o usuário precisa ser desconectado dos servidores de Autenticação do Adobe Pass e também dos servidores do MVPD. Como resultado, esse fluxo não pode ser concluído com uma simples solicitação HTTP emitida pela biblioteca do Access Enabler. As Guias personalizadas do Chrome são usadas pelo SDK para executar as operações de redirecionamento HTTP. Esse fluxo estará visível para o usuário e será fechado quando concluído

| Chamada de API: iniciar o fluxo de logout |
| --- |
| public void logout() |

**Disponibilidade:** v1.0+

**Parâmetros:** Nenhum

**Retornos de chamada disparados:**

- `navigateToUrl()` para a versão do SDK anterior à 3.0
- `setAuthenticationStatus()` para versão do SDK > 3.0


[Voltar à API do Android...](#api)


### getSelectedProvider {#getSelectedProvider}

**Descrição:** Use este método para determinar o provedor selecionado no momento.

| Chamada de API: determine o MVPD selecionado no momento |
| --- |
| public void getSelectedProvider() |

**Disponibilidade:** v1.0+

**Parâmetros:** Nenhum

**Retornos de chamada disparados:** `selectedProvider()`

[Voltar à API do Android...](#api)


### <span id="selectedProvider"></span>seletedProvider

**Descrição:** Retorno de chamada disparado pelo Habilitador de Acesso que fornece informações sobre o MVPD selecionado no momento para o aplicativo.

| Callback: informações sobre o MVPD selecionado no momento |
| --- |
| public void seletedProvider(Mvpd mvpd) |


**Disponibilidade:** v1.0+

**Parâmetros:**

- *mvpd*: objeto que contém informações sobre o MVPD selecionado no momento

**Acionado por:** `getSelectedProvider()`

[Voltar à API do Android...](#api)


### getMetadata {#getMetadata}

**Descrição:** Use este método para recuperar informações expostas como metadados pela biblioteca do Ativador de Acesso. O aplicativo pode acessar essas informações fornecendo um objeto composto MetadataKey.

| Chamada de API: consulte o AccessEnabler para obter metadados |
| --- |
| `public void getMetadata(MetadataKey metadataKey)` |

**Disponibilidade:** v1.0+

Há dois tipos de metadados disponíveis para programadores:

- Metadados estáticos (TTL do token de autenticação, TTL do token de autorização e ID do dispositivo)
- Metadados do usuário (informações específicas do usuário, como ID do usuário e CEP; transmitidas de um MVPD para o dispositivo de um usuário durante os fluxos de Autenticação e/ou Autorização)

**Parâmetros:**

- *metadataKey*: uma estrutura de dados que encapsula uma variável key e args, com o seguinte significado:
   - Se a chave for `METADATA_KEY_USER_META` e os argumentos contiverem um objeto SerializableNameValuePair com nome = `METADATA_ARG_USER_META` e valor = `[metadata_name]`, a consulta será feita para metadados de usuário. A lista atual de tipos de metadados de usuário disponíveis:
      - `zip` - CEP

      - `householdID` - Identificador da família. Se um MVPD não oferecer suporte a subcontas, será idêntico a `userID`.

      - `maxRating` - Classificação máxima dos pais para o usuário

      - `userID` - O identificador do usuário. Se um MVPD der suporte a subcontas e o usuário não for a conta principal, `userID` será diferente de `householdID`.

      - `channelID` - Uma lista de canais que o usuário está autorizado a visualizar
   - Se a chave for `METADATA_KEY_DEVICE_ID`, será feita a consulta para obter a ID do dispositivo atual. Observe que esse recurso está desativado por padrão e os programadores devem entrar em contato com o Adobe para obter informações sobre ativação e taxas.
   - Se a chave for `METADATA_KEY_TTL_AUTHZ` e os argumentos contiverem um objeto SerializableNameValuePair com nome = `METADATA_ARG_RESOURCE_ID` e valor = `[resource_id]`, a consulta será feita para obter a hora de expiração do token de autorização associado ao recurso especificado.
   - Se a chave for `METADATA_KEY_TTL_AUTHN`, a consulta será feita para obter o tempo de expiração do token de autenticação.



>[!NOTE]
>
>Para o SDK 3.4.0, as constantes: `METADATA_KEY_USER_META, METADATA_KEY_DEVICE_ID, METADATA_KEY_TTL_AUTHZ, METADATA_KEY_TTL_AUTHN` estão disponíveis em com.adobe.adobepass.accessenabler.api.profile.UserProfileService.



>[!NOTE]
>
>Os metadados reais do usuário disponíveis para um Programador dependem do que um MVPD disponibiliza.  Essa lista será expandida à medida que novos metadados forem disponibilizados e adicionados ao sistema de autenticação da Adobe Pass.

**Retornos de chamada disparados:** [`setMetadataStatus()`](#setMetadaStatus)

**Mais Informações:** [Metadados de Usuário](/help/authentication/user-metadata-feature.md)

[Voltar à API do Android...](#api)

### setMetadataStatus {#setMetadaStatus}

**Descrição:** Retorno de chamada disparado pelo Habilitador de Acesso que fornece os metadados solicitados por uma chamada *getMetadata()*.

| Callback: resultado de solicitação de recuperação de metadados |
| --- |
| ```public void setMetadataStatus(MetadataKey key, MetadataStatus result)``` |

**Disponibilidade:** v1.0+

**Parâmetros:**

- *chave*: o objeto MetadataKey que contém a chave para a qual o valor de metadados é solicitado e os parâmetros associados (consulte o aplicativo de demonstração para obter uma implementação de referência).
- *resultado*: um objeto composto que contém os metadados solicitados. O objeto tem os seguintes campos:
   - *simpleResult*: uma Cadeia de Caracteres que representa o valor dos metadados quando a solicitação foi feita para TTL de Autenticação, TTL de Autorização ou ID do Dispositivo. Esse valor será nulo se a solicitação tiver sido feita para Metadados do usuário.

   - *userMetadataResult*: um objeto que contém a representação Java de uma carga de metadados de usuário JSON.\
     Por exemplo:

```json
          '{
          "street": "Main Avenue",
          "buildings": ["150", "320"]
          }'
```

é traduzido para o Java como:

```java
          Map("street" -> "Main Avenue", "buildings" -> List("150", "320")))
```

**A estrutura real dos objetos de metadados do usuário é semelhante à seguinte:**

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

- *criptografado*: valor booliano que especifica se os metadados recuperados estão criptografados ou não. Esse parâmetro é importante apenas para solicitações de metadados do usuário, não tem significado para metadados estáticos (por exemplo: TTL de autenticação) que são sempre recebidos não criptografados. Se esse parâmetro for definido como True, cabe ao Programador obter o valor de Metadados do Usuário não criptografado executando uma descriptografia RSA usando a chave privada da lista de permissões (a mesma chave privada usada para a assinatura da ID do solicitante na chamada [`setRequestor`](#setRequestor)).

**Acionado por:** [`getMetadata()`](#getMetadata)

**Mais Informações:** [Metadados de Usuário](/help/authentication/user-metadata-feature.md)


[Voltar à API do Android...](#api)


### getVersion {#getVersion}

**Descrição:** Este método pode ser usado para recuperar a versão da biblioteca AccessEnabler.

| Chamada de API: obter versão do AccessEnabler |
| --- |
| ```public static String getVersion()``` |


[Voltar à API do Android...](#api)

</br>

## Rastreamento de eventos {#tracking}

O Ativador de acesso aciona um retorno de chamada adicional que não é necessariamente relacionado aos fluxos de direito. A implementação da função de retorno de chamada de rastreamento de eventos chamada *sendTrackingData()* é opcional, mas permite que o aplicativo rastreie eventos específicos e compile estatísticas, como o número de tentativas de autenticação/autorização com êxito/falha. Abaixo está a especificação para o retorno de chamada *sendTrackingData()*:


### sendTrackingData {#sendTrackingData}

**Descrição:** Retorno de chamada disparado pelo Habilitador de Acesso que sinaliza ao aplicativo a ocorrência de vários eventos, como a conclusão/falha de fluxos de autenticação/autorização. O tipo de dispositivo, o tipo de cliente Access Enabler e o sistema operacional também são relatados por sendTrackingData().

>[!WARNING]
>
> O tipo de dispositivo e o sistema operacional são derivados pelo uso de uma biblioteca Java pública ([http://java.net/projects/user-agent-utils](http://java.net/projects/user-agent-utils)) e a sequência de agente do usuário. Esteja ciente de que essas informações são fornecidas apenas como uma forma grosseira de dividir as métricas operacionais em categorias de dispositivos, mas esse Adobe não pode assumir nenhuma responsabilidade por resultados incorretos. Use a nova funcionalidade adequadamente.


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
   - `android`

</br>

| Retorno de chamada: rastreamento de eventos |
| --- |
| ```public void sendTrackingData(Event event, ArrayList<String> data)``` |

**Disponibilidade:** v1.0+

**Parâmetros:**

- *evento*: o evento que está sendo rastreado. Há três tipos possíveis de eventos de rastreamento:
   - **authorizationDetection:** sempre que uma solicitação de token de autorização retornar (o tipo de evento é `EVENT_AUTHZ_DETECTION`)
   - **authenticationDetection:** sempre que ocorrer uma verificação de autenticação (o tipo de evento é `EVENT_AUTHN_DETECTION`)
   - **mvpdSelection:** quando o usuário seleciona um MVPD no formulário de seleção MVPD (o tipo de evento é `EVENT_MVPD_SELECTION`)
- *dados*: dados adicionais associados ao evento relatado. Esses dados são apresentados no formato de uma lista de valores.

A seguir estão instruções para interpretar os valores nos *dados*
matriz:

- Para o tipo de evento *`EVENT_AUTHN_DETECTION`:*
   - **0** - Se a solicitação de token foi bem-sucedida (true/false) e se o item acima for true:
   - **1** - Cadeia de caracteres de ID MVPD
   - **2** - GUID (md5 com hash)
   - **3** - Token já no cache (true/false)
   - **4** - Tipo de dispositivo
   - **5** - Tipo de cliente do Access Enabler
   - **6** - Tipo de sistema operacional

- Para o tipo de evento `EVENT_AUTHZ_DETECTION`
   - **0** - Se a solicitação de token foi bem-sucedida (true/false) e se bem-sucedida:
   - **1** - ID do MVPD
   - **2** - GUID (md5 com hash)
   - **3** - Token já no cache (true/false)
   - **4** - Erro
   - **5** - Detalhes
   - **6** - Tipo de dispositivo
   - **7** - Tipo de cliente do Access Enabler
   - **8** - Tipo de sistema operacional

- Para o tipo de evento `EVENT_MVPD_SELECTION`
   - **0** - ID do MVPD selecionado no momento
   - **1** - Tipo de dispositivo
   - **2** - Tipo de cliente do Access Enabler
   - **3** - Tipo de sistema operacional

**Acionado por:** `checkAuthentication()`, `getAuthentication()`, `checkAuthorization()`, `getAuthorization()`, `setSelectedProvider()`

[Voltar à API do Android...](#api)


<!--
## Related Information {#related}

- [Android Integration Cookbook](/help/authentication/android-sdk-cookbook.md)
- [Android Technical Overview](/help/authentication/android-sdk-overview.md)

-->
