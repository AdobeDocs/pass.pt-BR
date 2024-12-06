---
title: Autorização de simulação do MVPD
description: Autorização de simulação do MVPD
exl-id: da2e7150-b6a8-42f3-9930-4bc846c7eee9
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '750'
ht-degree: 0%

---

# Autorização de simulação do MVPD

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#mvpd-preflight-authz-intro}

A &quot;Autorização de comprovação&quot; é uma verificação de autorização simples para vários recursos. Os programadores o usam principalmente para decorar suas interfaces (por exemplo, indicando o status de acesso com ícones de bloqueio e desbloqueio).

Atualmente, a Autenticação Adobe Pass pode oferecer suporte à Autorização de comprovação de duas maneiras para MVPDs, por meio de atributos de resposta AuthN ou por meio de uma solicitação AuthZ multicanal.  Os cenários a seguir descrevem o custo/benefício das diferentes maneiras de implementar a autorização de comprovação:

* **Cenário Mais Consagrado** - O MVPD fornece a lista de recursos pré-autorizados durante a fase de autorização (Autenticação Multicanal).
* **Cenário do Pior Caso** - Se um MVPD não der suporte a nenhuma forma de autorização de vários recursos, o servidor de Autenticação do Adobe Pass executará uma chamada de autorização para o MVPD para cada recurso na lista de recursos. Esse cenário tem um impacto (proporcional ao número de recursos) no tempo de resposta da solicitação de autorização de comprovação. Ele pode aumentar a carga nos servidores Adobe e MVPD, causando problemas de desempenho. Além disso, ele gerará eventos de solicitações/respostas de autorização sem a necessidade real de uma reprodução.
* **Obsoleto** - O MVPD fornece a lista de recursos pré-autorizados durante a fase de autenticação; portanto, não serão necessárias chamadas de rede, nem mesmo a solicitação de comprovação, pois a lista está armazenada em cache no cliente.

Embora os MVPDs não precisem oferecer suporte à autorização de comprovação, as seções a seguir descrevem alguns métodos de autorização de comprovação que a Autenticação do Adobe Pass pode oferecer suporte, antes de retornar ao cenário de pior caso acima.

## Comprovação em AuthN {#preflight-authn}

Esse cenário de comprovação é compatível com OLCA (Cabeçalhos). A seção 7.5.2 da Especificação da Interface de Autenticação e Autorização 1.0, intitulada &quot;Declaração de Atributo na Asserção de Autenticação&quot;, descreve como uma resposta de autenticação SAML pode conter uma lista de recursos pré-autorizados. Se um IdP suportar isso, o servidor de autenticação da Adobe Pass poderá gerar a lista de recursos previamente configurada no momento da autenticação e armazená-la em cache no cliente, juntamente com o token de autenticação. Este método também alcança o melhor cenário, e nenhuma chamada de rede será executada quando o Programador chamar checkPreauthorizedResources(), já que tudo já está no cliente.

### Lista de Recursos Personalizados na Instrução de Atributo SAML {#custom-res-saml-attr}

A resposta de autenticação SAML do IdP deve incluir uma AttributeStatement que contenha nomes de recursos que o AdobePass deve autorizar.  Alguns MVPDs fornecem isso no seguinte formato:

```XML
<saml:AttributeStatement>
  <saml:Attribute Name="authorized_resources">
    <saml:AttributeValue>MMOD</saml:AttributeValue>
    <saml:AttributeValue>Olympics2012</saml:AttributeValue>
  </saml:Attribute>
</saml:AttributeStatement>
```

A amostra acima apresenta uma lista contendo dois recursos pré-autorizados: &quot;MMOD&quot; e &quot;Olympic2012&quot;.

Isso efetivamente atinge o melhor cenário, e nenhuma chamada de rede será executada quando o Programador chamar checkPreauthorizedResources(), já que tudo já está no cliente.

## Comprovação de vários canais em AuthZ {#preflight-multich-authz}

Essa implementação de comprovação também é compatível com OLCA (Cablelabs).  A Especificação da Interface de Autenticação e Autorização 1.0 (seções 7.5.3 e 7.5.4) descreve métodos para solicitar informações de Autorização de um MVPD usando Asserções SAML ou XACML. Essa é a maneira recomendada de consultar o status de autorização para MVPDs que não oferecem suporte a isso como parte do fluxo de autenticação. A Autenticação do Adobe Pass emite uma única chamada de rede para o MVPD a fim de recuperar a lista de recursos autorizados.


A Autenticação Adobe Pass recebe a lista de recursos do aplicativo do Programador. A integração MVPD da Autenticação Adobe Pass pode então fazer uma chamada AuthZ incluindo todos esses recursos e, em seguida, analisar a resposta e extrair as várias decisões de permissão/negação.  O fluxo para a comprovação com o cenário AuthZ multicanal funciona da seguinte maneira:

1. O aplicativo do programador envia uma lista de recursos separada por vírgulas por meio da API do cliente de comprovação, por exemplo: &quot;TestChannel1,TestChannel2,TestChannel3&quot;.
1. A chamada de solicitação AuthZ de comprovação do MVPD contém os vários recursos e tem a seguinte estrutura:

```XML
<?xml version="1.0" encoding="UTF-8"?><soap11:Envelope xmlns:soap11="http://schemas.xmlsoap.org/soap/envelope/"> 
<soap11:Header/> 
<soap11:Body> 
  <xacml-samlp:XACMLAuthzDecisionQuery xmlns:xacml-samlp="urn:oasis:names:tc:xacml:2.0:profile:saml2.0:v2:schema:protocol" 
                                       CombinePolicies="false" Destination="https://login.idpexmaple.net/" ID="_3576604f382455d6495f342d9e07b69c" 
                                       IssueInstant="2013-02-07T10:31:40.333Z" Version="2.0"> 
  <saml2:Issuer xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion">https://saml.sp.auth-staging.adobe.com/on-behalf-of/TestDistributors</saml2:Issuer> 
  <xacml-context:Request xmlns:xacml-context="urn:oasis:names:tc:xacml:2.0:context:schema:os"> 
  <xacml-context:Subject SubjectCategory="urn:oasis:names:tc:xacml:1.0:subject-category:access-subject"> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:subject-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VFZTAQEAABQCe[...]</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Subject> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">TestChannel2</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Resource> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:resource:resource-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                                xsi:type="xacml-context:AttributeValueType">TestChannel3</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Resource> 
  <xacml-context:Action> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:action:action-id" 
                           DataType="http://www.w3.org/2001/XMLSchema#string"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">VIEW</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Action> 
  <xacml-context:Environment> 
  <xacml-context:Attribute AttributeId="urn:oasis:names:tc:xacml:1.0:subject:authn-locality:ip-address" 
                           DataType="urn:oasis:names:tc:xacml:2.0:data-type:ipAddress"> 
  <xacml-context:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
                                xsi:type="xacml-context:AttributeValueType">127.0.0.1</xacml-context:AttributeValue> 
  </xacml-context:Attribute> 
  </xacml-context:Environment> 
  </xacml-context:Request> 
  </xacml-samlp:XACMLAuthzDecisionQuery> 
</soap11:Body> 
</soap11:Envelope>
```

## Autorização Personalizada Para Vários Recursos {#custom-authz}

Alguns MVPDs têm endpoints de autorização que oferecem suporte à autorização para vários recursos em uma solicitação, mas não se enquadram no cenário descrito em AuthZ multicanal. Esses MVPDs específicos exigem trabalho personalizado.

O Adobe também pode suportar autorização de vários canais sem alterar a implementação existente.  Essa abordagem precisa ser revisada entre o Adobe e a equipe técnica do MVPD para garantir que funcione conforme esperado.

## MVPDs que oferecem suporte à autorização de comprovação {#mvpds-supp-preflight-authz}

A tabela a seguir lista os MVPDs compatíveis com a Autorização de comprovação, juntamente com o tipo de comprovação compatível e as limitações conhecidas:

| Abordagem de comprovação | MVPD | Notas |
|:-------------------------------:|:--------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------:|
| AuthZ multicanal | Comcast AT&amp;T Proxy Clearleap Charter_Direct Proxy GLDS Rogers Verizon OSN Bell Sasktel Optimum AlticeOne |                                                                    |
| Alinhamento de canais nos metadados do usuário | Suddenlink HTC | Todas as integrações diretas do Synacor também podem suportar esta abordagem. |
| Bifurcação e Junção | Todos os outros não listados acima | O número máximo padrão de recursos verificados = 5. |

<!--
![RelatedInformation]
>* [Logout](/help/authentication/usecase-mvpd-logout.md)
>* [Authorization](/help/authentication/authz-usecase.md)
>* [MVPD Integration Features](/help/authentication/mvpd-integr-features.md)
>* [MVPD User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Preflight Authorization - Programmer Integration Guide](/help/authentication/preflight-authz.md)
>* [AuthN and AuthZ Interface 1.0 Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank} 
-->
