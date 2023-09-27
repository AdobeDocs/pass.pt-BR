---
title: Guia do SDK do JavaScript
description: Guia do SDK do JavaScript
exl-id: d57f7a4a-ac77-4f3c-8008-0cccf8839f7c
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '940'
ht-degree: 0%

---

# Guia do SDK do JavaScript {#javascript-sdk-cookbook}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#intro}

Este documento descreve os workflows de direito que um aplicativo de nível superior do programador implementa para uma integração JavaScript com o serviço de Autenticação da Adobe Pass. Os links para a Referência da API JavaScript são incluídos em todo o.

Observe também que a [Informações relacionadas](#related) inclui um link para um conjunto de amostras de código JavaScript.

## Fluxos de Direitos {#entitlement}

1. [Pré-requisitos](#prereq)
2. [Fluxo de inicialização](#startup)
3. [Fluxo de autenticação](#authn)
4. [Fluxo de autorização](#authz)
5. [Exibir fluxo de mídia](#logout)

</br>

![](assets/javascript-flows.png)


## Pré-requisitos {#prereq}

**Dependências:**

- Biblioteca de autenticação da Adobe Pass (AccessEnabler), trabalhe com seu Gerente de conta de autenticação da Adobe Pass para organizar isso.
- RequestorId de autenticação da Adobe Pass válido, trabalhe com seu gerente de conta de autenticação da Adobe Pass para organizar isso.

Crie suas funções de retorno de chamada:

- `entitlementLoaded`
</br>

**Acionador:** O AccessEnabler carregou e concluiu a inicialização.

- `displayProviderDialog(mvpds)`

  **Acionador:** `getAuthentication(),` somente se o usuário não tiver selecionado um provedor (um MVPD) e ainda não estiver autenticado. O parâmetro mvpds é uma matriz de provedores disponíveis para o usuário.

- `setAuthenticationStatus(status, errorcode)`

  **Acionador:**
   - `checkAuthentication()`sempre.
   - `getAuthentication()` somente se o usuário já estiver autenticado e tiver selecionado um provedor.

  O status retornado é sucesso ou falha; o código de erro descreve o tipo da falha.

- `createIFrame(width, height)`

  **Acionador:** `setSelectedProvider(providerID)`, somente se o provedor selecionado estiver configurado para ser exibido em um IFrame.

  >[!NOTE]
  >
  >Um provedor é configurado para renderizar sua tela de autenticação como um redirecionamento ou em um iFrame e o Programador precisa considerar ambos.

- `sendTrackingData(event, data)`

  **Acionadores:** `checkAuthentication(), getAuthentication(),checkAuthorization(), getAuthorization(), setSelectedProvider()`.  A variável `event` indica qual evento de direito ocorreu; a variável `data` parameter é uma lista de valores relacionados ao evento.
- `setToken(token, resource)`
  **Acionador:** `checkAuthorization()`e `getAuthorization()` após uma autorização bem-sucedida para visualizar um recurso.   A variável `token` é o token de mídia de vida curta; a variável `resource` parâmetro é o conteúdo que o usuário está autorizado a visualizar.

- `tokenRequestFailed(resource, code, description)`
  **Acionador:**`checkAuthorization()` e`getAuthorization()`  após uma autorização malsucedida.\
  A variável `resource` é o conteúdo que o usuário estava tentando visualizar; a variável `code` é o código de erro que indica que tipo de falha ocorreu; a variável `description` descreve o erro associado ao código de erro.

- `selectedProvider(mvpd)`

  **Acionador:** [`getSelectedProvider()`](#$getSelProv O `mvpd` O parâmetro fornece informações sobre o provedor selecionado pelo usuário.

- `setMetadataStatus(metadata, key, arguments)`

  **Acionador:** `getMetadata().`\
  A variável `metadata` fornece os dados específicos solicitados; o parâmetro de chave é a chave usada no `getMetadata()`pedido; e a `arguments` é o mesmo dicionário que foi passado para `getMetadata()`.


## 2. Fluxo de inicialização

**I. Carregue o JavaScript do AccessEnabler:**

**Para perfil temporário**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

ou...

**Para perfil de produção**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

**Acionadores:** Quando a inicialização é concluída, a autenticação do Adobe Pass chama o `entitlementLoaded()` função de retorno de chamada. Este é o ponto de entrada para a comunicação do aplicativo com o AccessEnabler.


**II.** Chame `setRequestor()`para estabelecer a identidade do Programador; transmita no site do Programador `requestorID` e (opcionalmente) uma matriz de endpoints de Autenticação do Adobe Pass.

**Acionadores:** Nenhum, mas habilita `displayProviderDialog()` para ser chamado quando necessário.


**III.** Chame `checkAuthentication()` para verificar uma autenticação existente sem iniciar o processo completo [fluxo de autenticação].  Se essa chamada for bem-sucedida, você poderá prosseguir diretamente para a `authorization flow`.  Caso contrário, vá para a página `authentication flow`.

**Dependência:** Uma chamada bem-sucedida para `setRequestor()`(essa dependência também se aplica a todas as chamadas subsequentes).

**Acionadores:** `setAuthenticationStatus()` retorno de chamada

</br>

## 3. Fluxo de autenticação</span>


**Dependência:** Uma chamada bem-sucedida para `setRequestor()`(essa dependência também se aplica a todas as chamadas subsequentes).


Chame `getAuthentication()` para obter o status de autenticação OU acionar o fluxo de autenticação do provedor.

**Acionadores:**

- `displayProviderDialog()`se o usuário ainda não tiver sido autenticado
- `setAuthenticationStatus()` se a autenticação já tiver ocorrido

A conclusão do fluxo de autenticação é alcançada quando o AccessEnabler chama `setAuthenticationStatus()`com `isAuthenticated == 1`.

## 4. Fluxo de autorização {#authz}

**Dependências:**

- Uma chamada bem-sucedida para `setRequestor()` (essa dependência também se aplica a todas as chamadas subsequentes).
- ResourceID(s) válido(s) acordado(s) com o MVPD(s). Observe que as ResourceIDs devem ser as mesmas que as usadas em quaisquer outros dispositivos ou plataformas e serão as mesmas em MVPDs.

Chame `getAuthorization()` e transmita a ResourceID para a mídia solicitada. Uma chamada bem-sucedida retornará um Token de mídia curta, que confirma que o usuário está autorizado a visualizar a mídia solicitada.

- Se a chamada for bem-sucedida: o usuário tem um token de Autenticação válido e está autorizado a assistir à mídia solicitada.
- Se a chamada falhar: examine a exceção lançada para determinar seu tipo (AuthN, AuthZ ou algo diferente):
- Se a chamada tiver sido um erro de Autenticação, reinicie o Fluxo de Autenticação.
- Se a chamada foi um erro de AuthZ, o usuário não está autorizado a assistir à mídia solicitada e algum tipo de mensagem de erro deve ser exibido para o usuário.
- Se houve algum outro erro (erro de conexão, erro de rede etc.) em seguida, exiba uma mensagem de erro apropriada para o usuário.

Use o Verificador de token de mídia para validar o shortMediaToken retornado de um êxito `getAuthorization()` chame.


**Dependência:** O Short Media Token Verifier (incluído na biblioteca AccessEnabler)

- Se a validação for bem-sucedida: Exibir/Reproduzir a mídia solicitada para o usuário.
- Se falhar: O token AuthZ era inválido, a solicitação de mídia deve ser recusada e uma mensagem de erro deve ser exibida ao usuário.

## 5. Exibir Fluxo De Mídia {#logout}

- O usuário seleciona a mídia para visualizar.
   - A mídia está protegida?
      - Seu aplicativo verifica se a mídia está protegida:
         - Se a mídia estiver protegida, o aplicativo iniciará o fluxo de autorização (AuthZ) acima.
         - Se a mídia não estiver protegida, continue com o fluxo Exibir mídia.
         - Reproduzir mídia

## Configurar a ID do visitante {#visitorID}

Configurar um [Experience Cloud visitorID](https://experienceleague.adobe.com/docs/id-service/using/home.html) o valor é muito importante do ponto de vista analítico. Depois que um valor de EC visitorID é definido, o SDK enviará essas informações junto com cada chamada de rede e o serviço de autenticação da Adobe Pass coletará essas informações. Dessa forma, é possível correlacionar os dados de análise do serviço de Autenticação da Adobe Pass com quaisquer outros relatórios de análise que você tenha de outros aplicativos ou sites. É possível encontrar informações sobre como configurar a EC visitorID [aqui](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=en).


>[!NOTE]
>
>Observe que esse suporte a funcionalidade está disponível a partir do JS SDK versão 3.1.0.

<!--
### Related Information (#related)

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
* **JavaScript SDK Code Samples**
-->
