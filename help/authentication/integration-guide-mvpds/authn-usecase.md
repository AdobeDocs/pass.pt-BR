---
title: Autenticação MVPD
description: Autenticação MVPD
exl-id: 9ff4a46e-a37b-414c-a163-9e586252a9c3
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1851'
ht-degree: 0%

---

# Autenticação MVPD {#mvpd-authn}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#mvpd-authn-overview}

A função real de provedor de serviços (SP) é mantida por um programador, mas a autenticação da Adobe Pass serve como proxy do SP para esse programador. Usar a Autenticação do Adobe Pass como intermediário permite que os MVPDs e os Programadores evitem a necessidade de personalizar seus processos de direito de acordo com cada caso.

As etapas abaixo apresentam a sequência de eventos, usando a Autenticação do Adobe Pass, quando um Programador solicita autenticação de uma MVPD compatível com SAML. Observe que o componente Ativador de acesso para autenticação da Adobe Pass está ativo no cliente do usuário/assinante. A partir daí, o Access Enabler facilita todas as etapas do fluxo de autenticação.

1. Quando o usuário solicita acesso ao conteúdo protegido, o Ativador de acesso inicia a autenticação (AuthN) em nome do Programador (SP).
1. O aplicativo do SP apresenta um &quot;Seletor de MVPD&quot; ao usuário para obter seu provedor de TV por assinatura (MVPD). Em seguida, o SP redireciona o navegador do usuário para o serviço de provedor de identidade (IdP) selecionado da MVPD.  Este é &quot;**Logon iniciado pelo programador**&quot;.  O MVPD envia a resposta do IdP para o serviço de consumidor de asserção SAML da Adobe, onde é processado.
1. Por fim, o Access Enabler redireciona o navegador de volta para o site da controladora de armazenamento, informando a controladora do status (sucesso/falha) da solicitação de Autenticação.

## A solicitação de autenticação {#authn-req}

Conforme apresentado nas etapas acima, durante o fluxo de Autenticação, um MVPD deve aceitar uma solicitação de Autenticação baseada em SAML e enviar uma resposta de Autenticação SAML.

A [Especificação de Interface de Autenticação e Autorização de Acesso a Conteúdo Online (OLCA)](https://www.cablelabs.com/specifications/search?query=&category=&subcat=&doctype=&content=false&archives=false){target=_blanck} apresenta uma solicitação e uma resposta AuthN padrão. Embora a Autenticação Adobe Pass não exija que os MVPDs baseiem suas mensagens de direito nesse padrão, observar a especificação pode fornecer ao insight os atributos-chave necessários para uma transação de Autenticação.

>[!NOTE]
>
>A solicitação de Autenticação que um MVPD recebe com a Autenticação Adobe Pass contém uma assinatura digital. No entanto, o exemplo abaixo não mostra uma assinatura por motivos de brevidade. Para obter um exemplo que mostra uma assinatura digital, consulte o exemplo em [a resposta de autenticação](#authn-response) nas seções a seguir.

Exemplo de solicitação de autenticação SAML:

```XML
<?xml version="1.0" encoding="UTF-8"?>
<samlp:AuthnRequest  
    AssertionConsumerServiceURL=http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer          
    Destination=http://idp.com/SSOService
    ForceAuthn="false"
    ID="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
    IsPassive="false"
    IssueInstant="2010-08-03T14:14:54.372Z"
    ProtocolBinding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST"
    Version="2.0"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
        http://saml.sp.adobe.adobe.com
    </saml:Issuer>
    <ds:Signature xmlns:ds=_signature_block_goes_here_
    </ds:Signature>
    <samlp:NameIDPolicy
        AllowCreate="true"
        Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
        SPNameQualifier="http://saml.sp.adobe.adobe.com"/>
</samlp:AuthnRequest> 
```

A tabela abaixo explica os atributos e as tags que precisam estar em uma solicitação de autenticação, com os valores esperados padrão.

**Detalhes da Solicitação de Autenticação SAML**

| samlp:AuthnRequest | &lt;AuthnRequest> emitido pelo provedor de serviços para o provedor de identidade. |
|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| URLdoServiçoConsumidorAsserção | Este é o endpoint do Adobe para ser usado na resposta subsequente. Valor padrão: **http://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer** |
| Destino | Uma referência de URI que indica o endereço para o qual essa solicitação foi enviada. Isso é útil para impedir o encaminhamento mal-intencionado de solicitações a recipients não intencionais, uma proteção exigida por algumas associações de protocolo. Se estiver presente, o recipient real DEVERÁ verificar se a referência do URI identifica o local em que a mensagem foi recebida. Caso contrário, a solicitação DEVE ser descartada. Algumas associações de protocolo podem exigir o uso deste atributo. |
| ForceAuthn | O atributo ForceAuthn, se presente com um valor true, obriga o provedor de identidade a estabelecer essa identidade recentemente, em vez de depender de uma sessão existente que possa ter com o principal. |
| ID | Um identificador para a solicitação. Consulte [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} seção 1.3.4 para obter detalhes. |
| IsPassive | Um valor booleano. Se &quot;true&quot;, o provedor de identidade e o próprio agente do usuário NÃO DEVEM assumir visivelmente o controle da interface do usuário do solicitante e interagir com o apresentador de maneira perceptível. Se um valor não for fornecido, o padrão será &quot;false&quot;. |
| IssueInstant | O momento em que a resposta foi emitida. O valor de hora é codificado em UTC conforme descrito no [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} Seção 1.3.3. |
| ProtocolBinding | Uma referência de URI que identifica uma associação de protocolo SAML a ser usada ao retornar a mensagem &lt;Response>. Consulte [SAMLBind] para obter mais informações sobre associações de protocolo e referências URI definidas para elas. Valor padrão : urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST |
| Versão | A versão desta solicitação. |
| saml:Issuer | Identifica a entidade que gerou a mensagem de resposta. (Para obter mais informações sobre esse elemento, consulte SAML core 2.0-os Seção 2.2.5) |
| ds:Signature | Uma Assinatura XML que protege a integridade da declaração e a autentica, conforme descrito na Seção 5 do SAML Core 2.0-os |
| samlp:NameIDPolicy | Especifica restrições no identificador de nome a ser usado para representar o assunto solicitado. |
| AllowCreate | Um valor booliano usado para indicar se o provedor de identidade pode, no decorrer do atendimento da solicitação, criar um novo identificador para representar o principal. Padrão: verdadeiro |
| Formato | Especifica a referência de URI correspondente a um formato de identificador de nome Padrão: urn:oasis:names:tc:SAML:2.0:nameid-format:transient que a Adobe recomenda: urn:oasis:names:tc:SAML:2.0:nameid-format:persistent |
| SPNameQualifier | Especifica opcionalmente que o identificador da entidade da asserção seja retornado (ou criado) no namespace de um provedor de serviços que não seja o solicitante. Padrão: http://saml.sp.adobe.adobe.com |

## A resposta da autenticação {#authn-response}

Após receber e manipular a solicitação de autenticação, o MVPD agora deve enviar uma resposta de autenticação.

**Exemplo de Resposta de Autenticação SAML**

```XML
<?xml version="1.0" encoding="UTF-8"?> 
<samlp:Response Destination="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"
                ID="_0ac3a9dd5dae0ce05de20912af6f4f83a00ce19587"                             
                InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                IssueInstant="2010-08-17T11:17:50Z" Version="2.0"              
                xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion"
                xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol"
                xmlns:xs="http://www.w3.org/2001/XMLSchema"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <saml:Issuer xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion">
             http://idp.com/SSOService
    </saml:Issuer>
    <samlp:Status>
       <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success"/>
    </samlp:Status>
    <saml:Assertion ID="pfxb0662d76-17a2-a7bd-375f-c11046a86742"
                   IssueInstant="2010-08-17T11:17:50Z"
                   Version="2.0">
        <saml:Issuer>http://idp.com/SSOService</saml:Issuer>
        <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
          <ds:SignedInfo>
            <ds:CanonicalizationMethod
                     Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
            <ds:SignatureMethod
                     Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
            <ds:Reference URI="#pfxb0662d76-17a2-a7bd-375f-c11046a86742">
              <ds:Transforms>
                 <ds:Transform
                    Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>        
                 <ds:Transform
                            Algorithm=http://www.w3.org/2001/10/xml-exc-c14n#"/>
              </ds:Transforms>
              <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
              <ds:DigestValue>LgaPI2ASx/fHsoq0rB15Zk+CRQ0=</ds:DigestValue>
            </ds:Reference>
          </ds:SignedInfo>
          <ds:SignatureValue>
                POw/mCKF__shortened_for_brevity__9xdktDu+iiQqmnTs/NIjV5dw==
          </ds:SignatureValue>
          <ds:KeyInfo>
            <ds:X509Data>
                <ds:X509Certificate>
                 MIIDVDCCAjygAwIBA__shortened_for_brevity_utQ==
                </ds:X509Certificate>
            </ds:X509Data>
          </ds:KeyInfo>
      </ds:Signature>
      <saml:Subject>
        <saml:NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"
                     SPNameQualifier="https://saml.sp.auth.adobe.com">
            _5afe9a437203354aa8480ce772acb703e6bbb8a3ad
        </saml:NameID>
        <saml:SubjectConfirmation
                     Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
            <saml:SubjectConfirmationData
                  InResponseTo="_c0fc667e-ad12-44d6-9cae-bc7cf04688f8"
                  NotOnOrAfter="2010-08-17T11:22:50Z"                                          
                  Recipient="https://sp.auth.adobe.com/sp/saml/SAMLAssertionConsumer"/>
           </saml:SubjectConfirmation>
       </saml:Subject>
       <saml:Conditions NotBefore="2010-08-17T11:17:20Z"
                        NotOnOrAfter="2010-08-17T19:17:50Z">
           <saml:AudienceRestriction>
              <saml:Audience>https://saml.sp.auth.adobe.com</saml:Audience>
           </saml:AudienceRestriction>
       </saml:Conditions>
       <saml:AuthnStatement AuthnInstant="2010-08-17T11:17:50Z"
                   SessionIndex="_1adc7692e0fffbb1f9b944aeafce62aaa7d770cd9e">
        <saml:AuthnContext>
            <saml:AuthnContextClassRef>
                   urn:oasis:names:tc:SAML:2.0:ac:classes:Password
            </saml:AuthnContextClassRef>
        </saml:AuthnContext>
    </saml:AuthnStatement>
  </saml:Assertion>
</samlp:Response>
```


No exemplo acima, o Adobe SP espera buscar a ID do usuário do Subject/NameId. É possível configurar o Adobe SP para obter a ID do usuário de um atributo definido pelo cliente; a resposta deve conter um elemento como o seguinte:

```XML
<saml:AttributeStatement>
     <saml:Attribute Name="guid" NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
         <saml:AttributeValue xsi:type="xs:string">
               71C69B91-F327-F185-F29E-2CE20DC560F5
         </saml:AttributeValue>
    </saml:Attribute>
</saml:AttributeStatement>
```

**Detalhes de resposta de Autenticação SAML**

| samlp:Response | Resposta recebida pela Autenticação Adobe Pass. |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Destino | Uma referência de URI que indica o endereço para o qual essa solicitação foi enviada. Isso é útil para impedir o encaminhamento mal-intencionado de solicitações a recipients não intencionais, uma proteção exigida por algumas associações de protocolo. Se estiver presente, o recipient real DEVERÁ verificar se a referência do URI identifica o local em que a mensagem foi recebida. Caso contrário, a solicitação DEVE ser descartada. Algumas associações de protocolo podem exigir o uso deste atributo. |
| ID | Um identificador para a solicitação. Ele é do tipo xs:ID e DEVE seguir os requisitos especificados na Seção 1.3.4 do [SAML core 2.0-os](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf){target=_blank} para exclusividade do identificador. Os valores do atributo ID em uma solicitação e do atributo InResponseTo na resposta correspondente DEVEM corresponder. |
| InResponseTo | A ID de uma mensagem de protocolo SAML em resposta à qual uma entidade de atestado pode apresentar a declaração. O valor deve ser igual ao do atributo ID enviado na Solicitação de autenticação. Consulte SAML core 2.0-os |
| IssueInstant | A hora em que a solicitação foi emitida. |
| Versão | A versão da solicitação. |
| saml:Issuer | Identifica a entidade que gerou a mensagem de solicitação. (Para obter mais informações sobre esse elemento, consulte a Seção 2.2.5. SAML core 2.0-os ) |
| samlp:Status | Um código que representa o status da solicitação correspondente. |
| samlp:StatusCode | Um código que representa o status da atividade realizada em resposta à solicitação correspondente. |
| saml:Assertion | Este tipo especifica a informação básica comum a todas as afirmações. |
| ID | O identificador para esta asserção. |
| Versão | A versão desta afirmação. |
| IssueInstant | A hora em que a solicitação foi emitida. |
| ds:Signature | Uma Assinatura XML que protege a integridade da declaração e a autentica, conforme descrito na Seção 5 do SAML Core 2.0-os |
| ds:SignedInfo | A estrutura de SignedInfo inclui o algoritmo de canonização, um algoritmo de assinatura e uma ou mais referências. O elemento SignedInfo pode conter um atributo de ID opcional que permitirá a referência a ele por outras assinaturas e objetos. Consulte Sintaxe e processamento de assinatura XML |
| ds:CanonicalizationMethod | CanonicalizationMethod é um elemento obrigatório que especifica o algoritmo de canonização aplicado ao elemento SignedInfo antes da execução dos cálculos de assinatura. Consulte Sintaxe e processamento de assinatura XML |
| ds:SignatureMethod | SignatureMethod é um elemento necessário que especifica o algoritmo usado para a geração e validação de assinatura. Este algoritmo identifica todas as funções criptográficas envolvidas na operação de assinatura (por exemplo, hash, algoritmos de chave pública, MACs, preenchimento, etc.) Consulte Sintaxe e processamento de assinatura XML |
| ds:Reference | Referência é um elemento que pode ocorrer uma ou mais vezes. Especifica um algoritmo de compilação e um valor de compilação e, opcionalmente, um identificador do objeto que está sendo assinado, o tipo do objeto e/ou uma lista de transformações a serem aplicadas antes da compilação. Consulte Sintaxe e processamento de assinatura XML |
| ds:Transforms | O elemento opcional Transforms contém uma lista ordenada de elementos Transform; eles descrevem como o signatário obteve o objeto de dados que foi assimilado. A saída de cada Transformação serve como entrada para a próxima Transformação. A entrada para a primeira Transformação é o resultado da desreferência do atributo URI do elemento Reference. A saída da última transformação é a entrada para o algoritmo DigestMethod. Consulte Sintaxe e processamento de assinatura XML |
| ds:DigestMethod | DigestMethod é um elemento obrigatório que identifica o algoritmo de compilação a ser aplicado ao objeto assinado. Consulte Sintaxe e processamento de assinatura XML |
| ds:DigestValue | DigestValue é um elemento que contém o valor codificado do resumo. O resumo é sempre codificado usando base64. Consulte Sintaxe e processamento de assinatura XML |
| ds:SignatureValue | O elemento SignatureValue contém o valor real da assinatura digital; ele é sempre codificado usando base64. Consulte Sintaxe e processamento de assinatura XML |
| ds:KeyInfo | KeyInfo é um elemento opcional que permite ao(s) recipient(s) obter a chave necessária para validar a assinatura. Consulte Sintaxe e processamento de assinatura XML |
| ds:X509Data | Um elemento X509Data em KeyInfo contém um ou mais identificadores de chaves ou certificados X509. Consulte Sintaxe e processamento de assinatura XML |
| ds: X509Certificate | O elemento X509Certificate, que contém um certificado [X509v3] codificado em base64 |
| saml:Subject | O assunto da(s) declaração(ões) na afirmação. |
| saml:NameID | O elemento &lt;NameID> é do tipo NameIDType (consulte a Seção 2.2.2 no SAML core 2.0-os) e é usado em várias construções de asserção SAML, como os elementos &lt;Subject> e &lt;SubjectConfirmation>, e em várias mensagens de protocolo. |
| Formato | Uma referência de URI que representa a classificação das informações do identificador baseado em sequência. |
| SPNameQualifier | Qualifica ainda mais um nome com o nome de um provedor de serviços ou afiliação de provedores. Este atributo fornece um meio adicional para federar nomes com base na(s) terceira(s) parte(s) confiável(is). |
| saml:SubjectConfirmation | Informações que permitem a confirmação do assunto. Se for fornecida mais de uma confirmação de sujeito, então satisfazer qualquer um deles é suficiente para confirmar o sujeito para o propósito de aplicar a afirmação. |
| saml:SubjectConfirmationData | Informações de confirmação adicionais a serem usadas por um método de confirmação específico. Por exemplo, o conteúdo típico desse elemento pode ser um elemento <!--<ds:KeyInfo>-->, conforme definido na Sintaxe de Assinatura XML e na Especificação de processamento |
| NotOnOrAfter | Momento em que o assunto não pode mais ser confirmado. |
| Recipient | Um URI que especifica a entidade ou o local ao qual uma entidade de certificação pode apresentar a declaração. Por exemplo, esse atributo pode indicar que a asserção deve ser entregue a um endpoint de rede específico para impedir que um intermediário a redirecione para outro lugar. |
| saml:Conditions | O elemento &lt;Condição> serve como um ponto de extensão para novas condições. |
| NotBefore | O atributo NotBefore especifica o momento em que o intervalo de validade começa. |
| saml:AudienceRestriction | O elemento &lt;AudienceRestriction> especifica que a asserção é endereçada para um ou mais públicos-alvo específicos identificados pelos elementos &lt;Audience>. |
| saml:Audience | Uma referência de URI que identifica um público-alvo pretendido. |
| saml:AuthnStatement | Uma declaração de autenticação. |
| AuthnInstant | Especifica a hora em que a autenticação ocorreu. |
| SessionIndex | Especifica o índice de uma sessão específica entre a entidade de segurança identificada pela entidade e a autoridade de autenticação. |
| saml:AuthnContext | O contexto usado pela autoridade de autenticação até e incluindo o evento de autenticação que gerou esta declaração. |
| saml:AuthnContextClassRef | Uma referência de URI que identifica uma classe de contexto de autenticação que descreve a declaração de contexto de autenticação a seguir. |
