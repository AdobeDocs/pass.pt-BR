---
title: Guia de integração do Amazon FireOS
description: Guia de integração do Amazon FireOS
exl-id: 1982c485-f0ed-4df3-9a20-9c6a928500c2
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '1425'
ht-degree: 0%

---

# Cookbook de integração do Amazon FireOS (herdado) {#amazon-fireos-integration-cookbook}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

</br>


## Introdução {#intro}

Este documento descreve os fluxos de trabalho de direito que um aplicativo de nível superior do programador pode implementar por meio das APIs expostas pela biblioteca `AccessEnabler` do Amazon FireOS.

A solução de direito de autenticação da Adobe Pass para o Amazon FireOS é dividida em dois domínios:

- O domínio da interface do usuário - esta é a camada de aplicativo de nível superior que implementa a interface do usuário e usa os serviços fornecidos pela biblioteca `AccessEnabler` para fornecer acesso ao conteúdo restrito.
- O domínio `AccessEnabler` - é aqui que os fluxos de trabalho de direito são implementados na forma de:
   - Chamadas de rede feitas para servidores back-end do Adobe
   - Regras de lógica de negócios relacionadas aos workflows de autenticação e autorização
   - Gerenciamento de vários recursos e processamento do estado do fluxo de trabalho (como o cache de token)

O objetivo do domínio `AccessEnabler` é ocultar todas as complexidades dos fluxos de trabalho de direitos e fornecer ao aplicativo de camada superior (por meio da biblioteca `AccessEnabler`) um conjunto de primitivos de direitos simples. Esse processo permite implementar os workflows de direito:

1. Defina a identidade do solicitante.
1. Verifique e obtenha autenticação em relação a um provedor de identidade específico.
1. Verifique e obtenha autorização para um recurso específico.
1. Fazer logoff.

A atividade de rede de `AccessEnabler` ocorre em um thread diferente, de modo que o thread de interface do usuário nunca é bloqueado. Como resultado, o canal de comunicação bidirecional entre os dois domínios de aplicativos deve seguir um padrão totalmente assíncrono:

- A camada de aplicativo da interface envia mensagens para o domínio `AccessEnabler` por meio das chamadas de API expostas pela biblioteca `AccessEnabler`.
- O `AccessEnabler` responde à camada da interface do usuário por meio dos métodos de retorno de chamada incluídos no protocolo `AccessEnabler` que a camada da interface do usuário registra na biblioteca `AccessEnabler`.

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

      - Acionado por `setRequestor()`, retorna êxito ou falha.     Success indica que você pode continuar com chamadas de direito.

   - [displayProviderDialog(mvpds)](#$displayProviderDialog)

      - Disparado por `getAuthentication()` somente se o usuário não tiver selecionado um provedor (MVPD) e ainda não estiver autenticado. O parâmetro `mvpds` é uma matriz de provedores disponíveis para o usuário.

   - [&quot;setAuthenticationStatus(status, motivo)&quot;](#$setAuthNStatus)

      - Disparado por `checkAuthentication()` toda vez. Disparado por `getAuthentication()` somente se o usuário já estiver autenticado e tiver selecionado um provedor.

      - O status retornado é autenticado ou não é autenticado, o motivo descreve uma falha de autenticação ou uma ação de logout.

   - [navigateToUrl(url)](#$navigateToUrl)

      - Ignorado no AmazonFireOS SDK, o método é usado em plataformas Android onde é acionado por `getAuthentication()` depois que o usuário seleciona um MVPD.  O parâmetro `url` fornece o local da página de logon do MVPD.

   - [&quot;sendTrackingData(event, data)&quot;](#$sendTrackingData)

      - Disparado por `checkAuthentication(), getAuthentication(), checkAuthorization(), getAuthorization(), setSelectedProvider()`.
O parâmetro `event` indica qual evento de direito ocorreu; o parâmetro `data` é uma lista de valores relacionados ao evento.

   - [`setToken(token, resource)`](#$setToken)

      - Acionado por `checkAuthorization()` e `getAuthorization()` após uma autorização bem-sucedida para exibir um recurso.
      - O parâmetro `token` é o token de mídia de vida curta;o parâmetro `resource` é o conteúdo que o usuário está autorizado a exibir.

   - [&quot;tokenRequestFailed(resource, code, description)&quot;](#$tokenRequestFailed)

      - Acionado por `checkAuthorization()` e `getAuthorization()` após uma autorização sem êxito.
      - O parâmetro `resource` é o conteúdo que o usuário estava tentando exibir; o parâmetro `code` é o código de erro que indica que tipo de falha ocorreu; o parâmetro `description` descreve o erro associado ao código de erro.

   - [`seletedProvider(mvpd)`](#$selectedProvider)

      - Disparado por `getSelectedProvider()`.
      - O parâmetro `mvpd` fornece informações sobre o provedor selecionado pelo usuário.

   - [`setMetadataStatus(metadados, chave, argumentos)`](#$setMetadataStatus)

      - Acionado por `getMetadata().`
      - O parâmetro `metadata` fornece os dados específicos solicitados; o parâmetro `key` é a chave usada na solicitação `getMetadata()`; e o parâmetro `arguments` é o mesmo dicionário passado para `getMetadata()`.

   - [&quot;preauthorizedResources(resources)&quot;](#$preauthResources)

      - Disparado por `checkPreauthorizedResources()`.
      - O parâmetro `authorizedResources` apresenta os recursos que o usuário está autorizado a exibir.


![](../../../../assets/android-entitlement-flows.png)


### B. Fluxo de inicialização {#startup_flow}

1. Inicie o aplicativo de nível superior.
1. Iniciar a autenticação do Adobe Pass.

   1. Chame [`getInstance`](#$getInstance) para criar uma única instância da Autenticação Adobe Pass `AccessEnabler`.

      - **Dependência:** Biblioteca FireOS da Amazon Nativa de Autenticação do Adobe Pass (`AccessEnabler`)

   1. Chame ` setRequestor()` para estabelecer a identificação do Programador; transmita no `requestorID` do Programador e (opcionalmente) uma matriz de pontos de extremidade de Autenticação Adobe Pass.

      - **Dependência:** RequestorID de Autenticação Adobe Pass Válido (trabalhe com seu Gerente de Conta de Autenticação Adobe Pass para organizar isso.)

      - **Acionadores:** retorno de chamada setRequestorComplete()

   Nenhuma solicitação de direito pode ser concluída até que a identidade do solicitante seja totalmente estabelecida. Isso significa que, enquanto setRequestor() ainda estiver em execução, todas as solicitações de direitos subsequentes (por exemplo, `checkAuthentication()`) serão bloqueadas.

   Você tem duas opções de implementação: depois que as informações de identificação do solicitante são enviadas ao servidor de backend, a camada de aplicativo da interface do usuário pode escolher uma das duas abordagens a seguir:</p>

   1. Aguarde o acionamento do retorno de chamada `setRequestorComplete()` (parte do delegado `AccessEnabler`).  Essa opção fornece a maior certeza de que `setRequestor()` foi concluído, portanto, ela é recomendada para a maioria das implementações.
   1. Continue sem aguardar o acionamento do retorno de chamada `setRequestorComplete()` e comece a emitir solicitações de direito. Essas chamadas (checkAuthentication, checkAuthorization, getAuthentication, getAuthorization, checkPreauthorizedResource, getMetadata, logout) são enfileiradas pela biblioteca `AccessEnabler`, que fará as chamadas de rede reais após `setRequestor()`. Ocasionalmente, essa opção pode ser interrompida se, por exemplo, a conexão de rede estiver instável.

1. Chame [checkAuthentication()](#$checkAuthN) para verificar uma autenticação existente sem iniciar o fluxo de Autenticação completo.  Se essa chamada for bem-sucedida, você poderá prosseguir diretamente para o Fluxo de autorização.  Caso contrário, prossiga para o Fluxo de autenticação.

- **Dependência:** uma chamada bem-sucedida para `setRequestor()` (essa dependência também se aplica a todas as chamadas subsequentes).

- **Acionadores:** retorno de chamada setAuthenticationStatus()

### C. Fluxo de autenticação {#authn_flow}

1. Chame [`getAuthentication()`](#$getAuthN) para iniciar o fluxo de autenticação ou obter a confirmação de que o usuário já está autenticado.

   **Acionadores:**

   - O retorno de chamada setAuthenticationStatus(), se o usuário já estiver autenticado.  Nesse caso, prossiga diretamente para o [Fluxo de Autorização](#authz_flow).
   - O retorno de chamada displayProviderDialog(), se o usuário ainda não estiver autenticado.

1. Apresentar ao usuário a lista de provedores enviada para `displayProviderDialog()`.
1. Depois que o usuário selecionar um provedor, um WebView abrirá a página do provedor para que o usuário faça logon

   >[!NOTE]
   >
   >Nesse momento, o usuário tem a oportunidade de cancelar o fluxo de autenticação. Se isso ocorrer, o `AccessEnabler` limpará seu estado interno e redefinirá o Fluxo de Autenticação.

1. Após um logon bem-sucedido do usuário, o WebView será fechado.
1. chame `getAuthenticationToken(),`, que instrui o `AccessEnabler` a recuperar o token de autenticação do servidor back-end.
1. [Opcional] Chame [`checkPreauthorizedResources(resources)`](#$checkPreauth) para verificar quais recursos o usuário está autorizado a visualizar. O parâmetro `resources` é uma matriz de recursos protegidos associada ao token de autenticação do usuário.

   **Acionadores:** Retorno de chamada de `preAuthorizedResources()`\
   **Ponto de execução:** após a conclusão do fluxo de autenticação

1. Se a autenticação tiver sido bem-sucedida, prossiga para o Fluxo de autorização.


### D. Fluxo de autorização {#authz_flow}

1. Chame [`getAuthorization()`](#$getAuthZ) para iniciar o fluxo de autorização.

   Dependência: ResourceID(s) válido(s) acordado(s) com a(s) MVPD(s).

   **Observação:** ResourceIDs devem ser os mesmos usados em qualquer outro dispositivo ou plataforma e serão os mesmos em MVPDs.

1. Validar autenticação e autorização.

   - Se a chamada `getAuthorization()` tiver êxito: o usuário tem tokens AuthN e AuthZ válidos (o usuário é autenticado e autorizado a assistir à mídia solicitada).
   - Se `getAuthorization()` falhar: examine a exceção lançada para determinar seu tipo (AuthN, AuthZ ou algo mais):
      - Se foi um erro de autenticação (AuthN), reinicie o fluxo de autenticação.
      - Se foi um erro de autorização (AuthZ), o usuário não está autorizado a assistir à mídia solicitada, e algum tipo de mensagem de erro deve ser exibido para o usuário.
      - Se houver algum outro tipo de erro (erro de conexão, erro de rede etc.), exiba uma mensagem de erro apropriada para o usuário.

1. Valide o token de mídia curta.

   Use a biblioteca Verificador de Token de Mídia de Autenticação da Adobe Pass para verificar o token de mídia de vida curta retornado da chamada `getAuthorization()` acima:

   - Se a validação for bem-sucedida: Reproduza a mídia solicitada para o usuário.
   - Se a validação falhar: O token AuthZ era inválido, a solicitação de mídia deve ser recusada e uma mensagem de erro deve ser exibida ao usuário.

1. Retorne ao fluxo normal do aplicativo.

### E. Fluxo de mídia de visualização {#media_flow}

1. O usuário seleciona a mídia a ser exibida.
1. A mídia está protegida?  Seu aplicativo verifica se a mídia selecionada está protegida:
   - Se a mídia selecionada estiver protegida, o aplicativo iniciará o [Fluxo de Autorização](#authz_flow) acima.
   - Se a mídia selecionada não estiver protegida, reproduza a mídia para o usuário.

### F. Fluxo de logout {#logout_flow}

1. Ligue para [`logout()`](#$logout) para desconectar o usuário. O `AccessEnabler` limpa todos os valores e tokens em cache obtidos pelo usuário para o MVPD atual em todos os solicitantes que compartilham o logon por meio do Logon único. Depois de limpar o cache, o `AccessEnabler` faz uma chamada de servidor para limpar as sessões do lado do servidor.  Como a chamada do servidor pode resultar em um redirecionamento SAML para o IdP (isso permite a limpeza da sessão no lado do IdP), essa chamada deve seguir todos os redirecionamentos. Por esse motivo, essa chamada será tratada em um controle WebView, invisível para o usuário.

   **Observação:** o fluxo de logout difere do fluxo de autenticação na medida em que o usuário não precisa interagir com o WebView de nenhuma maneira. Assim, é possível (e recomendado) tornar o controle do WebView invisível (ou seja: oculto) durante o processo de logout.
