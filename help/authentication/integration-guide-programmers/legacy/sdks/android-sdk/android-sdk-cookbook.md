---
title: Guia do Android SDK
description: Guia do Android SDK
exl-id: 7f66ab92-f52c-4dae-8016-c93464dd5254
source-git-commit: 79b3856e3ab2755cc95c3fcd34121171912a5273
workflow-type: tm+mt
source-wordcount: '1703'
ht-degree: 0%

---

# Guia do Android SDK (herdado) {#android-sdk-cookbook}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

</br>

## Introdução {#intro}

Este documento descreve os workflows de direito que um aplicativo de nível superior do programador pode implementar por meio das APIs expostas pela biblioteca Android AccessEnabler.


A solução de direito de autenticação da Adobe Pass para o Android é dividida em dois domínios:

- O domínio da interface do usuário — essa é a camada de aplicativo de nível superior que implementa a interface do usuário e usa os serviços fornecidos pela biblioteca do AccessEnabler para fornecer acesso ao conteúdo restrito.
- O domínio AccessEnabler - é aqui que os workflows de direito são implementados no formato de:
   - Chamadas de rede feitas aos servidores back-end da Adobe
   - Regras de lógica de negócios relacionadas aos workflows de autenticação e autorização
   - Gerenciamento de vários recursos e processamento do estado do fluxo de trabalho (como o cache de token)

O objetivo do domínio AccessEnabler é ocultar todas as complexidades dos workflows de direito e fornecer ao aplicativo de camada superior (por meio da biblioteca AccessEnabler) um conjunto de primitivos de direito simples com os quais você implementa os workflows de direito:

1. Defina a identidade do solicitante.

1. Verifique e obtenha autenticação em relação a um provedor de identidade específico.

1. Verifique e obtenha autorização para um recurso específico.

1. Fazer logoff.

A atividade de rede do AccessEnabler ocorre em um thread diferente, de modo que o thread de interface do usuário nunca é bloqueado. Como resultado, o canal de comunicação bidirecional entre os dois domínios de aplicativos deve seguir um padrão totalmente assíncrono:

- A camada de aplicativo da interface envia mensagens para o domínio AccessEnabler por meio das chamadas de API expostas pela biblioteca AccessEnabler.
- O AccessEnabler responde à camada da interface do usuário por meio dos métodos de retorno de chamada incluídos no protocolo AccessEnabler que a camada da interface do usuário registra na biblioteca AccessEnabler.

## Fluxos de Direitos {#entitlement}

1. [Pré-requisitos](#prereqs)
1. [Fluxo de inicialização](#startup_flow)
1. [Fluxo de autenticação](#authn_flow)
1. [Fluxo de autorização](#authz_flow)
1. [Exibir fluxo de mídia](#media_flow)
1. [Fluxo de saída](#logout_flow)

### A. Pré-requisitos {#prereqs}

1. Crie suas funções de retorno de chamada:
   - [`setRequestorComplete()`](#$setRequestorComplete)

     Acionado por `setRequestor()`, retorna êxito ou falha.\
     Success indica que você pode continuar com chamadas de direito.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

     Disparado por `getAuthentication()` somente se o usuário não tiver selecionado um provedor (MVPD) e ainda não estiver autenticado.\
     O parâmetro `mvpds` é uma matriz de provedores disponíveis para o usuário.

   - [&quot;setAuthenticationStatus(status, errorcode)&quot;](#$setAuthNStatus)

     Disparado por `checkAuthentication()` toda vez.\
     Disparado por `getAuthentication()` somente se o usuário já estiver autenticado e tiver selecionado um provedor.

     O status retornado é sucesso ou falha, o código de erro descreve o tipo da falha.

   - [navigateToUrl(url)](#$navigateToUrl)

     Disparado por `getAuthentication()` depois que o usuário seleciona uma MVPD. O parâmetro `url` fornece o local da página de logon do MVPD.

   - [&quot;sendTrackingData(event, data)&quot;](#$sendTrackingData)

     Disparado por `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`.\
     O parâmetro `event` indica qual evento de direito ocorreu; o parâmetro `data` é uma lista de valores relacionados ao evento.

   - [`setToken(token, resource)`](#$setToken)

     Acionado por `checkAuthorization()` e `getAuthorization()` após uma autorização bem-sucedida para exibir um recurso.\
     O parâmetro `token` é o token de mídia de vida curta; o parâmetro `resource` é o conteúdo que o usuário está autorizado a exibir.

   - [&quot;tokenRequestFailed(resource, code, description)&quot;](#$tokenRequestFailed)

     Acionado por `checkAuthorization()` e `getAuthorization()` após uma autorização sem êxito.\
     O parâmetro `resource` é o conteúdo que o usuário estava tentando exibir; o parâmetro `code` é o código de erro que indica que tipo de falha ocorreu; o parâmetro `description` descreve o erro associado ao código de erro.

   - [`seletedProvider(mvpd)`](#$selectedProvider)

     Disparado por `getSelectedProvider()`.\
     O parâmetro `mvpd` fornece informações sobre o provedor selecionado pelo usuário.

   - [`setMetadataStatus(metadados, chave, argumentos)`](#$setMetadataStatus)

     Acionado por `getMetadata().`\
     O parâmetro `metadata` fornece os dados específicos solicitados; o parâmetro `key` é a chave usada na solicitação `getMetadata()`; e o parâmetro `arguments` é o mesmo dicionário passado para `getMetadata()`.

   - [&quot;preauthorizedResources(resources)&quot;](#$preauthResources)

     Disparado por `checkPreauthorizedResources()`.\
     O parâmetro `authorizedResources` apresenta os recursos que o usuário está autorizado a exibir.


![](../../../../assets/android-entitlement-flows.png)


### B. Fluxo de inicialização {#startup_flow}

1. Inicie o aplicativo de nível superior.
1. Iniciar autenticação do Adobe Pass

   a. Chame [`getInstance`](#$getInstance) para criar uma única instância do Adobe Pass Authentication AccessEnabler.

   - **Dependência:** Autenticação Adobe Pass Nativa
Biblioteca da Android (AccessEnabler)

   b. Chame ` setRequestor()` para estabelecer a identificação do Programador; transmita no `requestorID` do Programador e (opcionalmente) uma matriz de pontos de extremidade de Autenticação Adobe Pass.

   - **Dependência:** RequestorID de Autenticação Adobe Pass Válido\
     (Trabalhe com seu gerente de conta de autenticação da Adobe Pass para organizar isso.)

   - **Acionadores:** retorno de chamada setRequestorComplete()

   | NOTA |     |
   | --- | --- |  
   |  | Nenhuma solicitação de direito pode ser concluída até que a identidade do solicitante seja totalmente estabelecida. Isso significa que, enquanto setRequestor() ainda estiver em execução, todas as solicitações de direitos subsequentes (por exemplo, `checkAuthentication()`) serão bloqueadas.<br><br>Você tem duas opções de implementação: Depois que as informações de identificação do solicitante forem enviadas para o servidor back-end, a camada de aplicativo da interface poderá escolher uma das duas seguintes abordagens:<br><br>1.  Aguarde o acionamento do retorno de chamada `setRequestorComplete()` (parte do delegado AccessEnabler).  Essa opção fornece a maior certeza de que `setRequestor()` foi concluído, portanto, ela é recomendada para a maioria das implementações.<br>2.  Continue sem aguardar o acionamento do retorno de chamada `setRequestorComplete()` e comece a emitir solicitações de direito. Essas chamadas (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) são enfileiradas pela biblioteca AccessEnabler, que fará as chamadas de rede reais após `setRequestor(). `Essa opção poderá ser interrompida ocasionalmente se, por exemplo, a conexão de rede estiver instável. |

   <!--Removed bad image link from first note cell above. ![](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/images/icons/1313859077_lightbulb.png) -->

1. Chame [checkAuthentication()](#$checkAuthN) para verificar uma autenticação existente sem iniciar o fluxo de Autenticação completo.   Se essa chamada for bem-sucedida, você poderá prosseguir diretamente para o Fluxo de autorização.  Caso contrário, prossiga para o Fluxo de autenticação.

   - **Dependência:** uma chamada bem-sucedida para `setRequestor()` (essa dependência também se aplica a todas as chamadas subsequentes).

   - **Acionadores:** retorno de chamada setAuthenticationStatus()

### C. Fluxo de autenticação {#authn_flow}

1. Chame [`getAuthentication()`](#$getAuthN) para iniciar o fluxo de autenticação ou obter a confirmação de que o usuário já está autenticado.\
   **Acionadores:**
   - O retorno de chamada setAuthenticationStatus(), se o usuário já estiver autenticado.  Nesse caso, prossiga diretamente para o [Fluxo de Autorização](#authz_flow).
   - O retorno de chamada displayProviderDialog(), se o usuário ainda não estiver autenticado.

1. Apresentar ao usuário a lista de provedores enviada para `displayProviderDialog()`.

1. Depois que o usuário selecionar um provedor, obtenha a URL da MVPD do usuário no retorno de chamada `navigateToUrl()`.  Abra um WebView e direcione esse controle do WebView para o URL.

1. Por meio do WebView instanciado na etapa anterior, o usuário acessa a página de logon do MVPD e insere credenciais de logon. Várias operações de redirecionamento ocorrem no WebView.

   **Observação:** neste momento, o usuário tem a oportunidade de cancelar o fluxo de autenticação. Se isso ocorrer, a camada da interface do usuário será responsável por informar o AccessEnabler sobre esse evento, chamando `setSelectedProvider()` com `null` como parâmetro. Isso permite que o AccessEnabler limpe seu estado interno e redefina o Fluxo de autenticação.

1. Após um logon bem-sucedido pelo usuário, a camada do aplicativo detecta o carregamento de um &quot;URL de redirecionamento personalizado&quot; (ou seja: `http://adobepass.android.app`). Na verdade, esse URL personalizado é um URL inválido que não se destina ao carregamento do WebView. É um sinal de que o Fluxo de Autenticação foi concluído e que o WebView precisa ser fechado.

1. Feche o controle WebView e chame `getAuthenticationToken()`, que instrui o AccessEnabler a recuperar o token de autenticação do servidor back-end.

1. [Opcional] Chame [`checkPreauthorizedResources(resources)`](#$checkPreauth) para verificar quais recursos o usuário está autorizado a visualizar. O parâmetro `resources` é uma matriz de recursos protegidos associada ao token de autenticação do usuário.\
   **Acionadores:** Retorno de chamada de `preAuthorizedResources()`\
   **Ponto de execução:** após a conclusão do fluxo de autenticação

1. Se a autenticação tiver sido bem-sucedida, prossiga para o Fluxo de autorização.



### D. Fluxo de autorização {#authz_flow}

1. Chame [getAuthorization()](#$getAuthZ) para iniciar a autorização
fluxo.

   Dependência: ResourceID(s) válido(s) acordado(s) com a(s) MVPD(s).

   **Observação:** ResourceIDs devem ser os mesmos usados em qualquer outro dispositivo ou plataforma e serão os mesmos em MVPDs.

1. Validar autenticação e autorização.

   - Se a chamada `getAuthorization()` tiver êxito: o usuário tem tokens AuthN e AuthZ válidos (o usuário é autenticado e autorizado a assistir à mídia solicitada).
   - Se `getAuthorization()` falhar: examine a exceção lançada para determinar seu tipo (AuthN, AuthZ ou algo mais):
      - Se foi um erro de autenticação (AuthN), reinicie o fluxo de autenticação.
      - Se foi um erro de autorização (AuthZ), o usuário não está autorizado a assistir à mídia solicitada, e algum tipo de mensagem de erro deve ser exibido para o usuário.
      - Se houver algum outro tipo de erro (erro de conexão, erro de rede etc.), exiba uma mensagem de erro apropriada para o usuário.

1. Valide o token de mídia curta.\
   Use a biblioteca do Verificador de Token de Mídia de Autenticação do Adobe Pass para verificar o token de mídia de curta duração retornado da chamada `getAuthorization()` acima:

   - Se a validação for bem-sucedida: Reproduza a mídia solicitada para o usuário.
   - Se a validação falhar: O token AuthZ era inválido, a solicitação de mídia deve ser recusada e uma mensagem de erro deve ser exibida ao usuário.

1. Retorne ao fluxo normal do aplicativo.

### E. Fluxo de mídia de visualização {#media_flow}

1. O usuário seleciona a mídia a ser exibida.
2. A mídia está protegida?  Seu aplicativo verifica se a mídia selecionada está protegida:
- Se a mídia selecionada estiver protegida, o aplicativo iniciará o [Fluxo de Autorização](#authz_flow) acima.
- Se a mídia selecionada não estiver protegida, reproduza a mídia para o usuário.



### F. Fluxo de logout {#logout_flow}

1. Ligue para [`logout()`](#$logout) para desconectar o usuário.\
   O AccessEnabler apaga todos os valores e tokens em cache para o MVPD atual para o solicitante atual e também para os solicitantes com Logon único. Depois de limpar o cache, o AccessEnabler faz uma chamada de servidor para limpar as sessões do lado do servidor.  Como a chamada do servidor pode resultar em um redirecionamento SAML para o IdP (isso permite a limpeza da sessão no lado do IdP), essa chamada deve seguir todos os redirecionamentos. Por esse motivo, essa chamada deve ser tratada em um controle WebView.

   a. Seguindo o mesmo padrão do fluxo de trabalho de autenticação, o domínio AccessEnabler faz uma solicitação à camada de aplicativo da interface do usuário (por meio do retorno de chamada `navigateToUrl()`) para criar um controle WebView e instruir esse controle a carregar a URL do ponto de extremidade de logout no servidor back-end.

   b. Novamente, a interface do usuário deve monitorar a atividade do controle WebView e detectar o momento em que o controle, à medida que passa por vários redirecionamentos, carrega a URL personalizada do aplicativo (ou seja: `http://adobepass.android.app/`). Depois que esse evento ocorrer, a camada de aplicativo da interface do usuário fechará o WebView e o processo de logout será concluído.

   **Observação:** o fluxo de logout difere do fluxo de autenticação na medida em que o usuário não precisa interagir com o WebView de nenhuma maneira. A camada de aplicativo da interface do usuário usa uma WebView para verificar se todos os redirecionamentos estão sendo seguidos. Assim, é possível (e recomendado) tornar o controle do WebView invisível (ou seja, oculto) durante o processo de logout.



### Fluxos de usuário para logon com vários MVPDs e logout {#user_flows}

[Aqui](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AndroidSSOUserFlows.pdf) você tem um documento que descreve o comportamento ao usar vários MVPDs e o que está acontecendo quando o usuário faz logoff de um aplicativo.

O comportamento descrito está disponível ao usar o Android SDK versão >= 2.0.0.
