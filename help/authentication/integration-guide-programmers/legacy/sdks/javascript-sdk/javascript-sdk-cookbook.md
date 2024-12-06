---
title: Guia do SDK do JavaScript
description: Guia do SDK do JavaScript
exl-id: d57f7a4a-ac77-4f3c-8008-0cccf8839f7c
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 0%

---

# Guia do SDK do JavaScript {#javascript-sdk-cookbook}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#intro}

Este documento descreve os workflows de direito que um aplicativo de nível superior do programador implementa para uma integração do JavaScript com o serviço de Autenticação da Adobe Pass. Os links para a Referência da API do JavaScript são incluídos em todo o.

Observe também que a seção [Informações Relacionadas](#related) inclui uma
link para um conjunto de amostras de código do JavaScript.

## Fluxos de Direitos {#entitlement}

1. [Pré-requisitos](#prereq)
2. [Fluxo de inicialização](#startup)
3. [Fluxo de autenticação](#authn)
4. [Fluxo de autorização](#authz)
5. [Exibir fluxo de mídia](#logout)

</br>

![](../../../../assets/javascript-flows.png)


## Pré-requisitos {#prereq}

**Dependências:**

- Biblioteca de autenticação da Adobe Pass (AccessEnabler), trabalhe com seu Gerente de conta de autenticação da Adobe Pass para organizar isso.
- RequestorId de autenticação da Adobe Pass válido, trabalhe com seu gerente de conta de autenticação da Adobe Pass para organizar isso.

Crie suas funções de retorno de chamada:

- `entitlementLoaded`
</br>

**Acionador:** o AccessEnabler carregou e concluiu a inicialização.

- `displayProviderDialog(mvpds)`

  **Acionador:** `getAuthentication(),` somente se o usuário não tiver selecionado um provedor (um MVPD) e ainda não estiver autenticado
O parâmetro mvpds é uma matriz de provedores disponíveis para o usuário.

- `setAuthenticationStatus(status, errorcode)`

  **Acionador:**
   - `checkAuthentication()`toda vez.
   - `getAuthentication()` somente se o usuário já estiver autenticado e tiver selecionado um provedor.

  O status retornado é sucesso ou falha; o código de erro descreve o tipo da falha.

- `createIFrame(width, height)`

  **Acionador:** `setSelectedProvider(providerID)`, somente se o provedor selecionado estiver configurado para ser exibido em um IFrame.

  >[!NOTE]
  >
  >Um provedor é configurado para renderizar sua tela de autenticação como um redirecionamento ou em um iFrame e o Programador precisa considerar ambos.

- `sendTrackingData(event, data)`

  **Acionadores:** `checkAuthentication(), getAuthentication(),checkAuthorization(), getAuthorization(), setSelectedProvider()`.  O parâmetro `event` indica qual evento de direito ocorreu; o parâmetro `data` é uma lista de valores relacionados ao evento.
- `setToken(token, resource)`
  **Acionador:** `checkAuthorization()`e `getAuthorization()` após uma autorização bem-sucedida para exibir um recurso.   O parâmetro `token` é o token de mídia de vida curta; o parâmetro `resource` é o conteúdo que o usuário está autorizado a exibir.

- `tokenRequestFailed(resource, code, description)`
  **Acionador:**`checkAuthorization()` e`getAuthorization()` após uma autorização malsucedida.\
  O parâmetro `resource` é o conteúdo que o usuário estava tentando exibir; o parâmetro `code` é o código de erro que indica que tipo de falha ocorreu; o parâmetro `description` descreve o erro associado ao código de erro.

- `selectedProvider(mvpd)`

  **Acionador:** [`getSelectedProvider()`](#$getSelProv O parâmetro `mvpd` fornece informações sobre o provedor selecionado por
o usuário.

- `setMetadataStatus(metadata, key, arguments)`

  **Acionador:** `getMetadata().`\
  O parâmetro `metadata` fornece os dados específicos solicitados; o parâmetro de chave é a chave usada na solicitação `getMetadata()`; e o parâmetro `arguments` é o mesmo dicionário passado para `getMetadata()`.


## 2. Fluxo de inicialização

**I. Carregar o AccessEnabler JavaScript:**

**Para Perfil de Preparo**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth-staging.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

ou...

**Para Perfil de Produção**

```JSON
<script type="text/javascript"         
src="https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js">
</script>"
```

**Acionadores:** quando a inicialização estiver concluída, o Adobe Pass
a autenticação chama a função de retorno de chamada `entitlementLoaded()`. Este é o ponto de entrada para a comunicação do aplicativo com o AccessEnabler.


**II.** Chame `setRequestor()` para estabelecer a
identidade do Programador; passe no `requestorID` e
(opcionalmente) uma matriz de endpoints de Autenticação do Adobe Pass.

**Acionadores:** nenhum, mas permite que `displayProviderDialog()` seja chamado quando necessário.


**III** Chame `checkAuthentication()` para verificar uma autenticação existente sem iniciar o [fluxo de autenticação] completo.  Se esta chamada tiver êxito, você poderá prosseguir diretamente para o `authorization flow`.  Caso contrário, prossiga para `authentication flow`.

**Dependência:** uma chamada bem-sucedida para `setRequestor()`(essa dependência também se aplica a todas as chamadas subsequentes).

**Acionadores:** Retorno de chamada de `setAuthenticationStatus()`

</br>

## 3. Fluxo de Autenticação</span>


**Dependência:** uma chamada bem-sucedida para `setRequestor()`(essa dependência também se aplica a todas as chamadas subsequentes).


Chame `getAuthentication()` para obter o status de autenticação OU para acionar o fluxo de autenticação do provedor.

**Acionadores:**

- `displayProviderDialog()`se o usuário ainda não tiver sido autenticado
- `setAuthenticationStatus()` se a autenticação já ocorreu

A conclusão do fluxo de autenticação é alcançada quando o AccessEnabler chama `setAuthenticationStatus()`com `isAuthenticated == 1`.

## 4. Fluxo de autorização {#authz}

**Dependências:**

- Uma chamada bem-sucedida para `setRequestor()` (essa dependência também se aplica a todas as chamadas subsequentes).
- ResourceID(s) válido(s) acordado(s) com o MVPD(s). Observe que as ResourceIDs devem ser as mesmas que as usadas em quaisquer outros dispositivos ou plataformas e serão as mesmas em MVPDs.

Chame `getAuthorization()` e passe o ResourceID para a mídia solicitada. Uma chamada bem-sucedida retornará um Token de mídia curta, que confirma que o usuário está autorizado a visualizar a mídia solicitada.

- Se a chamada for bem-sucedida: o usuário tem um token de Autenticação válido e está autorizado a assistir à mídia solicitada.
- Se a chamada falhar: examine a exceção lançada para determinar seu tipo (AuthN, AuthZ ou algo diferente):
- Se a chamada tiver sido um erro de Autenticação, reinicie o Fluxo de Autenticação.
- Se a chamada foi um erro de AuthZ, o usuário não está autorizado a assistir à mídia solicitada e algum tipo de mensagem de erro deve ser exibido para o usuário.
- Se houver algum outro erro (erro de conexão, erro de rede etc.), exiba uma mensagem de erro apropriada para o usuário.

Use o Verificador de Token de Mídia para validar o shortMediaToken retornado de uma chamada `getAuthorization()` bem-sucedida.


**Dependência:** O Verificador de Token de Mídia Curta (incluído com o
AccessEnabler (biblioteca)

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

Configurar um valor de [Experience Cloud visitorID](https://experienceleague.adobe.com/docs/id-service/using/home.html) é muito importante do ponto de vista de análise. Depois que um valor de EC visitorID é definido, o SDK enviará essas informações junto com cada chamada de rede e o serviço de autenticação da Adobe Pass coletará essas informações. Dessa forma, é possível correlacionar os dados de análise do serviço de Autenticação da Adobe Pass com quaisquer outros relatórios de análise que você tenha de outros aplicativos ou sites. Informações sobre como configurar a EC visitorID podem ser encontradas [aqui](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=en).


>[!NOTE]
>
>Observe que esse suporte a funcionalidade está disponível a partir do JS SDK versão 3.1.0.

<!--
### Related Information (#related)

* [JavaScript SDK Overview](/help/authentication/javascript-sdk-overview.md)
* [JavaScript SDK API Reference](/help/authentication/javascript-sdk-api-reference.md)
* **JavaScript SDK Code Samples**
-->
