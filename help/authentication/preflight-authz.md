---
title: Autorização de comprovação
description: Autorização de comprovação
exl-id: 036b1a8e-f2dc-4e9a-9eeb-0787e40c00d9
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1518'
ht-degree: 0%

---

# Autorização de comprovação {#preflight-authorization}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

</br>

## Visão geral {#overview}

Esse recurso fornece uma verificação de autorização simples para vários recursos. O objetivo dessa verificação leve é decorar a interface do usuário (por exemplo, indicando o status de acesso com ícones de bloqueio e desbloqueio). A autorização de comprovação é o mais leve e eficiente possível, de modo que uma única chamada de API produz o status de autorização para uma lista de recursos. Observe que esse recurso não é autoritativo em relação à autorização de um recurso.

A `getAuthorization(resource)` ou `checkAuthorization(resource)` a chamada ainda DEVE ser feita antes de permitir a reprodução.

A autorização de comprovação também oferece suporte a um caso de uso diferente, em que o Programador precisa solicitar autorização para várias IDs de recursos para permitir a reprodução de um item do conteúdo de mídia. O programador pode fazer uma verificação inicial de comprovação nos recursos necessários e, dependendo da resposta, pode falhar antecipadamente se as condições comerciais não forem atendidas.

Para obter uma lista de MVPDs que oferecem suporte à autorização de comprovação, consulte [Autorização de simulação do MVPD](/help/authentication/mvpd-preflight-authz.md#preflight_support_list) página.

>[!NOTE]
>
> Observe que o uso desse recurso para os MVPDs que não têm suporte completo de autorização de comprovação precisa ser acordado antecipadamente com o Adobe e os MVPDs. O uso da autorização de comprovação nesses MVPDs se enquadra no &quot;pior cenário possível&quot; descrito [aqui](/help/authentication/mvpd-preflight-authz.md#intro) e podem levar a problemas de desempenho e tempo de resposta lento. </br>
> Observe também que o uso da autorização de comprovação com **mais de 5 recursos precisam ser explicitamente aceitos pelo Adobe**.

## API de comprovação do AccessEnabler {#AE_pre_api}

O AccessEnabler expõe um par de funções de API/retorno de chamada para implementar a autorização de comprovação. A chamada de API usa um único parâmetro que consiste em uma lista de recursos. A função de retorno de chamada recebe um único parâmetro que representa os recursos reais autorizados. Os exemplos a seguir estão em ActionScript, mas as chamadas estão disponíveis em todas as versões do AccessEnabler.

### checkPreauthorizedResources(Matriz:recursos):void {#checkPreauthRes}

Chame essa função no objeto AccessEnabler para solicitar o status de autorização para uma lista de recursos.

O parâmetro resources é a lista de recursos cuja autorização deve ser verificada. Cada elemento na lista deve ser uma string que representa a ID do recurso. A ID do recurso está sujeita às mesmas limitações que a ID do recurso na `getAuthorization()` chamada, ou seja, é acordado um valor estabelecido entre o Programador e o MVPD, ou um fragmento de RSS de mídia. Observe que a Autenticação Adobe Pass não gerencia recursos de nenhuma maneira, exceto por uma camada de mediação reduzida que pode transformar formatos de recursos dependendo do que o MVPD realmente suporta.

### preauthorizedResources(Matriz:authorizedResources) {#preauthRes}

Essa é uma função de retorno de chamada que deve ser implementada no aplicativo de camada superior do programador. O AccessEnabler chamará essa função após calcular a lista de recursos autorizados.


### Exemplo de JS

```javascript
    checkPreauthorizedResources(["CNBC","MSNBC"]);
    ...
    preauthorizedResources() {
        var resource = arguments[0];
        for(i in resources) {
            // Do things with resource list
            // such as decorate UI
            ...
        }
    }
```

## Informações de implementação {#details}

- [Comprovação usando ChannelID](#preflight_using_channelID)
- [POST / Pré-autorizar](#post)
- [Armazenamento](#storage)

A chamada de API tenta encontrar uma lista em cache de recursos autorizados para o usuário atual no armazenamento local do cliente. Se não houver nenhuma lista em cache, será feita uma chamada HTTPS aos servidores Adobe Pass para recuperar a lista.

O mecanismo de armazenamento em cache melhora os tempos de desempenho em chamadas subsequentes ignorando completamente a chamada de rede. Além disso, a lista em cache pode ser preenchida antecipadamente como parte do processo de autenticação.  (Para obter informações sobre como configurar esse cenário, consulte [Integração da autorização de comprovação](/help/authentication/authz-usecase.md#preflight_authz_int) na seção Autorização do Guia de Integração do MVPD).

Além disso, a lista de recursos em cache pode ser usada para otimizar o fluxo de autorização, no sentido de que, se uma lista de recursos em cache existir, `checkAuthorization()` O pode verificá-lo antes de fazer uma chamada de rede. Se o recurso não estiver na lista de recursos pré-autorizados, a verificação pode falhar sem precisar chamar os servidores de autenticação da Adobe Pass.


### Comprovação usando ChannelID {#preflight_using_channelID}

A partir da versão 2.4.1 da Autenticação Adobe Pass, o fluxo de Comprovação funciona da seguinte maneira:

1. Durante a autenticação, a Autenticação do Adobe Pass lê o `channelIID` elemento da resposta SAML do MVPD, e usa esse valor para definir a variável `authorizedResources` elemento no token de autenticação.
1. Dentro do `checkPreauthorizedResources()` função da API, a autenticação da Adobe Pass verifica se `authorizedResources` elemento está definido.
1. Se a variável `authorizedResources` for definido, a Autenticação Adobe Pass lerá esse valor e executará uma interseção entre a lista de recursos na `authorizedResources` e a lista de recursos recebidos do `checkPreauthorizedResources()` parâmetro.  O resultado dessa interseção é a lista final de recursos pré-autorizados.
1. Se a variável `authorizedResources` elemento não estiver definido, execute o fluxo implementado anteriormente, no qual a lista de recursos recebidos do `checkPreauthorizedResources()` é passado para o PreAuthorizationServlet. Esse servlet executa as chamadas de autorização para os endpoints do MVPD e retorna a lista de recursos pré-autorizados.

### Exemplo de simulação com ChannelID

O exemplo abaixo mostra um exemplo de alinhamento de canal. Observe que o nome do atributo usado para enviar a lista de canais pode diferir de um MVPD para outro:

```XML
    <saml:Attribute Name="visible_channels" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
        <saml:AttributeValue xsi:type="xs:string">MSNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNBC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FBN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">FNC</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TNT</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TBS</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">CNN</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TRUTV</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">TOON</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">HBO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">MAX</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">EPIXHD</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">BTN-BTN2GO</saml:AttributeValue>
        <saml:AttributeValue xsi:type="xs:string">SPEED-SPEED2</saml:AttributeValue>
    </saml:Attribute>
```


A variável `authorizedResources` elemento do elemento de autenticação aparece da seguinte maneira:

```JSON
    <authorizedResources>
        <authorizedResource resourceID="MSNBC"/>
        <authorizedResource resourceID="CNBC"/>
        <authorizedResource resourceID="FBN"/>
        <authorizedResource resourceID="FNC"/>
        <authorizedResource resourceID="TNT"/>
        <authorizedResource resourceID="TBS"/>
        <authorizedResource resourceID="CNN"/>
        <authorizedResource resourceID="TRUTV"/>
        <authorizedResource resourceID="TOON"/>
        <authorizedResource resourceID="HBO"/>
        <authorizedResource resourceID="MAX"/>
        <authorizedResource resourceID="EPIXHD"/>
        <authorizedResource resourceID="BTN-BTN2GO"/>
        <authorizedResource resourceID="SPEED-SPEED2"/>
    </authorizedResources>
```

O programador executa o `checkPreauthorizedResources()` chamada de API, transmitindo a seguinte lista de parâmetros:</span>

- &quot;MSNBC&quot;
- &quot;FBN&quot;
- &quot;TruTV&quot;
- &quot;fbc-fox&quot;

A implementação de Comprovação atual executa a interseção com a lista de recursos da `authorizedResources` elemento e retorna esta lista:

- &quot;MSNBC&quot;
- &quot;FBN&quot;
- &quot;TruTV&quot;



**Nota:** Observe que a interseção não diferencia maiúsculas de minúsculas.



### POST / Pré-autorizar {#post}

Esta chamada será executada automaticamente pelo AccessEnabler quando não existir nenhuma lista de recursos em cache, autorizada.


#### Solicitação {#req}

| Parâmetro | Tipo | Obrigatório | Descrição |
| --- | --- | --- | --- |
| `authentication_token` | string | SIM | O token de autenticação. |
| `resource_id` | string | SIM | Um único recurso. Isso pode ser especificado várias vezes, uma vez para cada elemento da matriz de recursos fornecida na variável `checkPreauthorizedResources()` chamada à API. |


**Nota:** O número máximo de recursos solicitados é configurável.
**O valor padrão máximo é 5.**


#### Resposta {#resp}

A resposta enviada pelo servlet pré-autorizado tem o seguinte formato:

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <resources>
      <resource>
        <id>TNT</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>TBS</id>
        <authorized>true</authorized>
      </resource>
      <resource>
        <id>CNN</id>
        <authorized>false</authorized>
      </resource>
    </resources>
```

### Armazenamento {#storage}

Uma lista de recursos pré-autorizados que o AccessEnabler obtém do provedor de serviços. Esta lista de recursos:

- É armazenado junto com os tokens AuthN e AuthZ
- É válido desde que o usuário esteja no mesmo site ou até que o token de autenticação expire
- É recuperado novamente sempre que o usuário acessa um novo site integrado de autenticação do Adobe Pass

Cada entrada contém a ID do recurso para a qual o usuário é pré-autorizado.

Por exemplo:


| ID do recurso | Autorizado |
| ----------- | ---------- |
| CNN | true |
| TNT | false |
| ... | ... |


Essa lista é chamada de &quot;cache de pré-autorização&quot;.

#### Fluxo {#flow}

1. O aplicativo/site do programador faz uma `checkPreauthorizedResources(resourceList)` chame.
1. O AccessEnabler verifica o token de autenticação para recursos autorizados:
   1. Se o token de autenticação contiver recursos autorizados, essa lista será autoritativa e nenhuma chamada deverá ser feita para obter essas informações. Os recursos de resourceList são pesquisados dentro da lista de recursos autorizados no token de autenticação e somente os encontrados são retornados pelo `preauthorizedResources()` retorno de chamada.
   1. Se o token de autenticação NÃO contiver recursos autorizados - `resourceList` é comparado à lista de recursos no cache de pré-autorização.
      1. Se a lista contiver os mesmos recursos, significa que uma chamada para o servidor já foi feita e a resposta já está no cache de pré-autorização. Somente os recursos autorizados serão retornados pelo `preauthorizedResources()` retorno de chamada.
      1. Se a lista NÃO contiver os mesmos recursos, o cliente precisará fazer uma chamada para o servidor para obter o estado de autorização dos recursos em resourceList. A resposta será obtida e armazenada no cache de pré-autorização, substituindo completamente os recursos antigos. Somente os recursos autorizados serão retornados pelo `preauthorizedResources()` retorno de chamada.


#### Recuperação de Lista {#listRetrieve}

Sempre que um `checkPreauthorizedResources()` for chamada, a lista de recursos para verificar a autorização será verificada em relação ao cache de pré-autorização. Se a lista contiver o mesmo conjunto de recursos, nenhuma chamada para o Provedor de Serviços será feita, pois todos os recursos necessários para acionar o `preauthorizedResources()` As chamadas de retorno já estão no cache.


#### logout() {#logout}

O cache de pré-autorização é esvaziado no logout.


## Dependências {#depends}

O desempenho da API de comprovação depende de implementações específicas do MVPD.  Para obter opções de implementação, consulte [Integração da autorização de comprovação](/help/authentication/authz-usecase.md#preflight_authz_int) na seção Autorização do Guia de Integração do MVPD.


## Segurança {#security}

As APIs do cliente estão disponíveis para todos os programadores.

A implementação usa HTTPS como um transporte, mas para ter uma chamada mais leve, nenhuma medida de segurança adicional é empregada (sem assinatura, sem FAXS).

**Atenção:** NÃO use essa API de maneira autoritativa para determinar se um usuário deve receber acesso a um recurso protegido. A finalidade dessa API é a decoração da interface e/ou a comprovação de decisões comerciais. A variável `getAuthorization()` e `checkAuthorization()` as chamadas devem ser sempre feitas antes de permitir a reprodução.


## Compatibilidade {#compat}

Este recurso é suportado em todos os tipos do AccessEnabler: AS, JS, AIR, iOS, Android, Xbox (no fluxo de Autenticação de 2ª tela).

A autorização de comprovação não oferece suporte a recursos de pré-autorização que contêm seções CDATA. O foco do sistema de comprovação atual é oferecer suporte à filtragem em nível de canal. Os recursos com seções CDATA provavelmente são recursos no nível do ativo. A comprovação oferece suporte a `mrss` recursos para pré-autorização no nível do canal, desde que não contenham CDATA.

## Integração Com Outros Recursos {#integ_w_other_features}

Uma possível otimização pode ocorrer no fluxo de autorização para falhar rapidamente se o recurso não estiver na lista de recursos pré-autorizados.

Nessa otimização, o endpoint de comprovação do servidor se integra ao mecanismo de Degradação.

Se uma regra &quot;Autenticar todos&quot; estiver em vigor para o MVPD e o Solicitante, o endpoint de comprovação simplesmente espelhará todos os recursos recebidos na solicitação.

O mesmo é verdadeiro se &quot;Todos AuthZ&quot; estiver ativado para, pelo menos, um dos recursos presentes na solicitação.

<!--
## Related Information

  - `checkPreauthorizedResources()`
      - [iOS](#checkPreauth)
      - [Android](#checkPreauth)
      - [JavaScript](#checkPreauthRes)
      - [ActionScript](#checkPreauthRes)
  - [Xbox](#2nd%20Screen%20Authentication)
  - [MVPD Integration Guide: Preflight AuthZ](#)
-->
