---
title: Android SDK com registro dinâmico do cliente
description: Android SDK com registro dinâmico do cliente
exl-id: 8d0c1507-8e80-40a4-8698-fb795240f618
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '1301'
ht-degree: 0%

---

# (Herdado) Android SDK com Registro de cliente dinâmico {#android-sdk-with-dynamic-client-registration}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

## Introdução {#Intro}

O Android AccessEnabler SDK for Android foi modificado para habilitar a Autenticação sem usar cookies de sessão. À medida que mais e mais navegadores restringem o acesso a cookies, outro método precisa ser usado para permitir a autenticação.

Para o Android, o uso das Guias personalizadas do Chrome restringe o acesso a cookies de outros aplicativos.

>**Android SDK 3.0.0** apresenta:

- o registro de cliente dinâmico substitui o mecanismo de registro de aplicativo atual com base na ID do solicitante assinada e na autenticação de cookie de sessão
- Guias personalizadas do Chrome para fluxos de autenticação

>[!NOTE]
>
>Para versões mais antigas do Android sem o suporte a Guias personalizadas do Chrome, a autenticação do WebView será usada de forma semelhante às versões mais antigas do AccessEnabler SDK.


## Registro de cliente dinâmico {#DCR}

O Android SDK v3.0+ usará o procedimento de Registro Dinâmico do Cliente conforme definido na [Visão Geral do Registro Dinâmico do Cliente](../../../rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).


## Demonstração de recursos {#Demo}

Assista a [este webinário](https://my.adobeconnect.com/pzkp8ujrigg1/), que fornece mais contexto sobre o recurso e contém uma demonstração sobre como gerenciar as instruções de software usando o Painel TVE e como testar as geradas usando um aplicativo de demonstração fornecido pela Adobe como parte do Android SDK.

## Alterações na API {#API}


### Factory.getInstance

**Descrição:** Instancia o objeto do Ativador de Acesso. Deve haver uma única instância do Access Enabler por instância do aplicativo.

| Chamada de API: construtor |
| --- |
| público estático AccessEnabler getInstance(Context appContext, String softwareStatement, String redirectUrl)<br>        gera AccessEnablerException |


**Disponibilidade:** v3.0+

**Parâmetros:**

- *appContext*: contexto de aplicativo do Android
- softwareStatement: valor obtido do Painel TVE ou *null* se &quot;software\_statement&quot; estiver definido em strings.xml
- redirectUrl : url exclusiva, um dos domínios na ordem inversa que foi explicitamente adicionado no Painel TVE ou *null* se &quot;redirect\_uri&quot; estiver definido em strings.xml

Observação: um softwareStatement ou um redirectUrl inválido fará com que o aplicativo não inicialize o AccessEnabler ou não registre o aplicativo para Autenticação e autorização do Adobe Pass
</br>
Observação: o parâmetro redirectUrl ou redirect\_uri em strings.xml deve ser o valor do domínio adicionado no Painel TVE para o aplicativo na ordem inversa ( por exemplo: para o domínio &#39;adobe.com&#39; adicionado no Painel TVE, o redirectUrl deve ser &#39;com.adobe&#39;.


### setRequestor

**Descrição:** Estabelece a identidade do Canal. Cada canal recebe uma ID exclusiva ao se registrar no Adobe para o sistema de autenticação da Adobe Pass. Ao lidar com SSO e tokens remotos, o estado de autenticação pode mudar quando o aplicativo estiver em segundo plano, setRequestor pode ser chamado novamente quando o aplicativo for colocado em primeiro plano para sincronizar com o estado do sistema (busque um token remoto se o SSO estiver habilitado ou exclua o token local se um logout tiver ocorrido enquanto isso).

A resposta do servidor contém uma lista de MVPDs juntamente com algumas informações de configuração anexadas à identidade do Canal. A resposta do servidor é usada internamente pelo código Access Enabler. Somente o status da operação (ou seja, SUCCESS/FAIL) é apresentado ao seu aplicativo por meio do retorno de chamada setRequestorComplete().

Se o parâmetro *urls* não for usado, a chamada de rede resultante será direcionada à URL do provedor de serviços padrão: o ambiente de Versão/Produção do Adobe.

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

- *requestorID*: o identificador exclusivo associado ao Canal. Transmita o identificador exclusivo atribuído pelo Adobe ao seu site quando você se registrou pela primeira vez no serviço de autenticação da Adobe Pass.
- *urls*: parâmetro opcional; por padrão, o provedor de serviços da Adobe é usado [http://sp.auth.adobe.com/](http://sp.auth.adobe.com/). Essa matriz permite especificar endpoints para serviços de autenticação e autorização fornecidos pelo Adobe (instâncias diferentes podem ser usadas para fins de depuração). Você pode usar essa opção para especificar várias instâncias do provedor de serviços de Autenticação da Adobe Pass. Ao fazer isso, a lista do MVPD é composta pelos endpoints de todos os provedores de serviços. Cada MVPD está associado ao provedor de serviços mais rápido; ou seja, o provedor que respondeu primeiro e que oferece suporte a esse MVPD.

Obsoleto:

- *signedRequestorID*: uma cópia da ID do solicitante que é assinada digitalmente com sua chave privada. <!--For more details, see [Registering Native Clients](http://tve.helpdocsonline.com/registering-native-clients)-->.

**Retornos de chamada disparados:** `setRequestorComplete()`

### logout

**Descrição:** Use este método para iniciar o fluxo de logout. O logout é o resultado de uma série de operações de redirecionamento HTTP devido ao fato de que o usuário precisa ser desconectado dos servidores de autenticação da Adobe Pass e também dos servidores da MVPD. Como resultado, esse fluxo abrirá uma janela ChromeCustomTab para executar o logout.

| Chamada de API: iniciar o fluxo de logout |
| --- |
| public void logout() |

**Disponibilidade:** v3.0+

**Parâmetros:** Nenhum

**Retornos de chamada disparados:** `setAuthenticationStatus()`
</br></br>

## Fluxo de implementação do programador {#Progr}

### **1. Registrar Aplicativo**

a. Obter software\_statement e redirect\_uri da Adobe Pass ( Painel TVE )

b. Há duas opções para transmitir esses valores para o Adobe Pass SDK:

Em strings.xml, adicione :

```XML
<string name="software_statement">[softwarestatement value]</string>
<string name="redirect_uri">application_url.com</string>
```

Chame AccessEnabler.getInstance(appContext,softwareStatement,
redirectUrl)


### &#x200B;2. Configurar Aplicativo

a. setRequestor(requestor\_id)

O SDK realizará as seguintes operações:

- registrar aplicativo: usando **software\_statement**, a SDK obterá um **client\_id, client\_secret, client\_id\_issued\_at, redirect\_uris, grant\_types**. Essas informações serão armazenadas no armazenamento interno do aplicativo.

- obtenha um **access\_token** usando client\_id, client\_secret e grant\_type=&quot;client\_credentials&quot;. Esse access\_token será usado em cada chamada feita pela SDK aos servidores da Adobe Pass

**Respostas de Erro de Token:**

| Respostas de erro | | |
| --- | --- | --- |
| HTTP 400 (Solicitação inválida) | {&quot;error&quot;: &quot;invalid\_request&quot;} | A solicitação não tem um parâmetro obrigatório, inclui um valor de parâmetro sem suporte (diferente do tipo de concessão), repete um parâmetro, inclui várias credenciais, usa mais de um mecanismo para autenticar o cliente ou está malformada. |
| HTTP 400 (Solicitação inválida) | {&quot;error&quot;: &quot;invalid\_client&quot;} | Falha na autenticação do cliente porque o cliente era desconhecido. O SDK DEVE se registrar novamente no servidor de autorização. |
| HTTP 400 (Solicitação inválida) | {&quot;error&quot;: &quot;unauthorized\_client&quot;} | O cliente autenticado não está autorizado a usar este tipo de concessão de autorização. |

- caso uma MVPD exija a Autenticação passiva, uma Guia personalizada do Chrome será aberta para ser executada de forma passiva com essa MVPD e será fechada ao concluir

b. checkAuthentication()

- true : ir para Autorização
- false : acesse Selecionar MVPD

c. getAuthentication : o SDK incluirá **access_token** nos parâmetros de chamada

- mvpd lembrado : ir para setSelectedProvider(mvpd_id)
- mvpd não selecionado : displayProviderDialog
- mvpd selecionado : ir para setSelectedProvider(mvpd_id)

d. setSelectedProvider

- O URL de autenticação mvpd\_id é carregado no ChromeCustomTabs
- logon bem-sucedido : delegate.setAuthenticationStatus ( SUCCESS )
- logon cancelado : redefinir seleção de MVPD
- O esquema de URL é estabelecido como &quot;adobepass://redirect_uri&quot; para capturar quando a autenticação é concluída

e. get/checkAuthorization : o SDK incluirá **access_token** no cabeçalho como Authorization: Bearer **access_token**

- se a autorização for bem-sucedida, será feita uma chamada para obter a
token de mídia

f. logout:

- O SDK excluirá um token válido para o solicitante atual (as autenticações obtidas por outros aplicativos e não via SSO permanecerão válidas)
- O SDK abrirá as Guias personalizadas do Chrome para alcançar o ponto de extremidade de logout mvpd_id. Depois de concluídas, as Guias personalizadas do Chrome serão fechadas
- O esquema de URL é estabelecido como &quot;adobepass://logout&quot; para capturar o momento em que o logout é concluído
- logout acionará um sendTrackingData(new Event(EVENT_LOGOUT,USER_NOT_AUTHENTICATED_ERROR) e um retorno de chamada : setAuthenticationStatus(0,&quot;Logout&quot;)

**Observação:** como cada chamada requer um **access_token,** possíveis códigos de erro abaixo são tratados no SDK.


| Respostas de erro | | |
| --- | ---|--- |
| invalid_request | 400 | A solicitação está malformada. O SDK deve interromper a execução de chamadas para o servidor. |
| invalid_client | 403 | A ID do cliente não tem mais permissão para executar solicitações. O sdk DEVE executar novamente o registro do cliente. |
| access_denied | 401 | O access\_token não é válido. O sdk DEVE solicitar um novo access_token. |
