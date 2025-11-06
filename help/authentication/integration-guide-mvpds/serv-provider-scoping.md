---
title: Escopo do provedor de serviços
description: Escopo do provedor de serviços
exl-id: 730c43e1-46c0-4eec-b562-b1ad93cce6d3
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# Escopo do provedor de serviços {#service-provoider-scoping}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview}

A implementação padrão de uma integração da Autenticação Adobe Pass com uma MVPD é baseada na **Especificação OLCA**. A seção Authentication Requirements da especificação OLCA (6.5, Subject Identifier) indica que é possível indicar o escopo do Service Provider (SP) para o identificador de assunto. (O identificador de assunto é a ID de usuário ofuscada que o MVPD retorna à controladora.)  Em uma integração de Autenticação do Adobe Pass, é necessário que os MVPDs habilitem o escopo das solicitações de Autenticação do SP.

Com a Autenticação do Adobe Pass assumindo a função de SP para o Programador, é necessário implementar uma personalização que permita o escopo de SP da solicitação de autenticação.  Isso precisa ser feito para que o MVPD possa identificar a marca de rede transmitida na asserção SAML para o Provedor de identidade (IdP) da MVPD.  O escopo pode ser implementado de uma das duas maneiras descritas na próxima seção.

## Escopo do provedor de serviços {#service-provider-scoping}

A Autenticação do Adobe Pass oferece suporte às duas seguintes formas de habilitar o escopo de solicitações de autenticação da controladora de armazenamento:

* **A Abordagem de Emissor SAML.** Nesta abordagem, a &quot;ID do Solicitante&quot; é anexada à cadeia de caracteres do Emissor SAML na solicitação de Autenticação SAML.

* **A Abordagem De Propriedade De Escopo Personalizado.** Nessa abordagem, a &quot;ID do solicitante&quot; é incluída explicitamente como uma propriedade personalizada de &quot;Escopo&quot; na solicitação de Autenticação SAML.

>[!NOTE]
>
>A &quot;ID do solicitante&quot; é como a autenticação da Adobe Pass se refere à marca de rede do programador (por exemplo: &quot;CNN&quot; é uma das marcas da rede Turner).

### Abordagem de emissor SAML {#saml-issuer-approach}

Esta abordagem usa o elemento `<Issuer>` do SAML na solicitação de Autenticação SAML, conforme mostrado neste trecho:

```xml
...
<saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
    http://saml.sp.adobe.adobe.com/on-behalf-of/requestorID
</saml:Issuer>
...
```

### Abordagem de propriedade de escopo personalizado {#custom-scoping-property-approach}

Essa abordagem usa uma propriedade personalizada chamada &quot;Escopo&quot;, como mostrado neste trecho de uma solicitação de autenticação SAML:

```xml
...
<samlp:Scoping xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <samlp:RequesterID xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">requestorID</samlp:RequesterID>
</samlp:Scoping>
...
```

<!--
>[!RELATEDINFORMATION]
>* [MVPD Authentication](/help/authentication/authn-usecase.md)
>* **OLCA Specification**
-->
