---
title: Serviço Web MVPD de Proxy
description: Serviço Web MVPD de Proxy
exl-id: f75cbc4d-4132-4ce8-a81c-1561a69d1d3a
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '955'
ht-degree: 0%

---

# Serviço Web Proxy MVPD {#proxy-mvpd-wbservice}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview-proxy-mvpd-webserv}

Um &quot;MVPD de proxy&quot; é um MVPD que, além de gerenciar sua própria integração com a Autenticação do Adobe Pass, também gerencia o processo de direito em nome de um grupo de &quot;MVPDs com proxy&quot; associados. Esse arranjo é transparente para os programadores.

Para implementar o recurso ProxyMVPD, a Autenticação Adobe Pass fornece serviços Web RESTful, com os quais ProxyMVPDs podem enviar e recuperar listas de ProxyMVPDs. O protocolo usado para essa API pública é REST HTTP, com as seguintes suposições:

* O MVPD Proxy usa o método HTTP GET para recuperar a lista dos MVPDs integrados atuais.
* O Proxy MVPD usa o método POST HTTP para atualizar a lista dos MVPDs suportados.

## Serviços de proxy MVPD {#proxy-mvpd-services}

* [Recuperar MVPDs com proxy aplicado](#retriev-proxied-mvpds)
* [Enviar MVPDs com proxy aplicado](#submit-proxied-mvpds)

### Recuperar MVPDs com proxy aplicado {#retriev-proxied-mvpds}

Recupera a lista atual de MVPDs com proxy aplicado para o ProxyMVPD identificado pelo parâmetro apikey.

| Endpoint | Chamado por | Cabeçalhos de solicitação | Método HTTP | Resposta HTTP |
|---|---|---|---|---|
| &lt;fqdn>/control/v1/proxiedMvpds | ProxyMVPD | apikey (Obrigatório) | GET | <ul><li> 200 (ok) - A solicitação foi processada com êxito e a resposta contém uma lista de MVPDs Proxies no formato XML</li><li>401 (não autorizado) - Autenticação de usuário necessária ou autorização não concedida para credenciais fornecidas.  Indica uma das seguintes opções:<ul><li>O token apikey não está presente no cabeçalho da solicitação</li><li>A solicitação é originada de um endereço IP que não está presente na lista de permissões</li><li>O token não é válido</li></ul></li><li>403 (proibido) - Indica que a operação não é suportada para os parâmetros fornecidos, ou o proxy MVPD não está definido como proxy ou está ausente</li><li>405 (método não permitido) - Um método HTTP diferente de GET ou POST foi usado. O método HTTP geralmente não é compatível ou não é compatível com esse endpoint específico.</li><li>500 (erro interno do servidor) - Um erro foi gerado no lado do servidor durante o processo de solicitação.</li></ul> |

Exemplo de ondulação:

`curl -X GET -H "apikey: ???provided-by-adobe???" "https://mgmt-prequal.auth-staging.adobe.com/control/v1/proxiedMvpds"`


Exemplo de resposta XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxiedMvpds>
    <proxiedMvpd>
        <id>oneMvpdId</id>
        <displayName>MVPD Name</displayName>
        <logoURL></logoURL>
    </proxiedMvpd>
    <proxiedMvpd>
        <id ProviderID="ProviderID_Value_Sent_On_IdPEntry">mvpdPickerId</id>
        <displayName>MVPD Name Two</displayName>
        <logoURL></logoURL>
        <requestorIds>
            <requestorId>TheRequestorId_IntegratedWith</requestorId>
        </requestorIds>
    </proxiedMvpd>
    <proxiedMvpd>
        <id>anotherMvpdId</id>
        <displayName>Another MVPD</displayName>
        <logoURL></logoURL>
        <iframeSize>
            <iframeHeight>400</iframeHeight>
            <iframeWidth>340</iframeWidth>
        </iframeSize>
        <requestorIds>
            <requestorId>FirstIntegratedRequestorId</requestorId>
            <requestorId>SecondIntegratedRequestorId</requestorId>
        </requestorIds>
    </proxiedMvpd>
</proxiedMvpds>
```

### Enviar MVPDs com proxy aplicado {#submit-proxied-mvpds}

Envia uma matriz de MVPDs integrados ao Proxy MVPD identificado pelo parâmetro apikey.

| Endpoint | Chamado por | Cabeçalhos de solicitação | Método HTTP | Resposta HTTP |
|:------------------------------:|:---------:|:--------------------------------------------:|:-----------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| &lt;fqdn>/control/v1/proxiedMvpds | ProxyMVPD | apikey (Obrigatório) proxied-mvpds (Obrigatório) | POST | <ul><li>201 (criado) - O push foi processado com êxito</li><li>400 (solicitação incorreta) - o servidor não sabe como processar a solicitação:<ul><li>O XML de entrada não adere ao esquema publicado nesta especificação</li><li>Os mvpds com proxy não têm IDs exclusivas</li><li>O requestorIds enviado não existe. Motivo do contêiner Outro Servlet para o código de resposta 400</li></ul><li>401 (não autorizado) - A apikey não é válida ou o IP do chamador não está na lista de permissões</li><li>403 (proibido) - Indica que a operação não é suportada para os parâmetros fornecidos, ou o proxy MVPD não está definido como proxy ou está ausente</li><li>405 (método não permitido) - Um método HTTP diferente de GET ou POST foi usado. O método HTTP geralmente não é compatível ou não é compatível com esse endpoint específico.</li><li>500 (erro interno do servidor) - Um erro foi gerado no lado do servidor durante o processo de solicitação.</li></ul> |

Exemplo de ondulação:

`curl -X POST -H "apikey: <API_KEY>" "https://mgmt-prequal.auth.adobe.com/control/v1/proxiedMvpds" -d "proxied-mvpds=%3CproxiedMvpds%3E%3CproxiedMvpd%3E%3CdisplayName%3EFirst%20MVPD%20Name%3C%2FdisplayName%3E%3Cid%3EfirstMVPDId%3C%2Fid%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3C%2FproxiedMvpd%3E%3CproxiedMvpd%3E%3Cid%20ProviderID%3D%22ProviderID_Value_Sent_On_IdPEntry%22%3EmvpdPickerId%3C%2Fid%3E%3CdisplayName%3EMVPD%20Name%20Two%3C%2FdisplayName%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3CrequestorIds%3E%3CrequestorId%3ETHE_REQUESTOR_ID%3C%2FrequestorId%3E%3C%2FrequestorIds%3E%3C%2FproxiedMvpd%3E%3C%2FproxiedMvpds%3E"`



Exemplo XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxiedMvpds>
    <proxiedMvpd>
        <id>oneMvpdId</id>
        <displayName>MVPD Name</displayName>
        <logoURL></logoURL>
    </proxiedMvpd>
    <proxiedMvpd>
        <id ProviderID="ProviderID_Value_Sent_On_IdPEntry">mvpdPickerId</id>
        <displayName>MVPD Name Two</displayName>
        <logoURL></logoURL>
        <requestorIds>
            <requestorId>TheRequestorId_IntegratedWith</requestorId>
        </requestorIds>
    </proxiedMvpd>
    <proxiedMvpd>
        <id>anotherMvpdId</id>
        <displayName>Another MVPD</displayName>
        <logoURL></logoURL>
        <iframeSize>
            <iframeHeight>400</iframeHeight>
            <iframeWidth>340</iframeWidth>
        </iframeSize>
        <requestorIds>
            <requestorId>FirstIntegratedRequestorId</requestorId>
            <requestorId>SecondIntegratedRequestorId</requestorId>
        </requestorIds>
    </proxiedMvpd>
</proxiedMvpds>
```


### Frequência de Lançamento {#posting-frequency}

A Autenticação do Adobe Pass recomenda que os ProxyMVPDs enviem sua lista de ProxyMVPDs somente quando houver uma alteração em relação ao envio anterior.

### Exclusão de MVPDs com Proxy {#delete-proxied-freqency}

Se o ProxyMVPD envia um registro XML com uma lista ProxyMVPDs vazia, essa lista vazia será armazenada em nosso sistema como qualquer lista, excluindo efetivamente a lista anterior.



## Formato XSD {#xsd-format}

O Adobe definiu o seguinte formato aceito para publicar/recuperar MVPDs com proxy de/para nosso serviço público da Web:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
           xmlns:pxm="http://tve.adobe.com/data/proxiedmvpd"
           targetNamespace="http://tve.adobe.com/data/proxiedmvpd"
           elementFormDefault="qualified"
           version="1.0">
    <xs:complexType name="iframeSize">
        <xs:all>
            <xs:element name="iframeHeight" type="xs:int" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="iframeWidth" type="xs:int" minOccurs="1" maxOccurs="1" nillable="false"/>
        </xs:all>
    </xs:complexType>
    <xs:complexType name="requestorIds">
        <xs:annotation>
            <xs:documentation>List of requestors/programmers integrated with the proxied MVPD</xs:documentation>
        </xs:annotation>
        <xs:sequence>
            <xs:element name="requestorId" type="xs:string" minOccurs="1" maxOccurs="unbounded" nillable="false">
                <xs:annotation>
                    <xs:documentation>The requestor/programmer identifier recognized by Adobe</xs:documentation>
                </xs:annotation>
            </xs:element>
        </xs:sequence>
    </xs:complexType>
    <xs:complexType name="proxiedMvpd">
        <xs:all>
            <xs:element name="id" minOccurs="1" maxOccurs="1" nillable="false">
                <xs:annotation>
                    <xs:documentation>The id must conform to the regular expression: ([a-zA-Z0-9]+((\-)|[_])*)</xs:documentation>
                </xs:annotation>
                <xs:complexType>
                    <xs:simpleContent>
                        <xs:extension base="xs:string">
                            <xs:attribute name="ProviderID">
                                <xs:simpleType>
                                    <xs:restriction base="xs:string">
                                        <xs:minLength value="1"/>
                                        <xs:maxLength value="128"/>
                                    </xs:restriction>
                                </xs:simpleType>
                            </xs:attribute>
                        </xs:extension>
                    </xs:simpleContent>
                </xs:complexType>
            </xs:element>
            <xs:element name="displayName" type="xs:string" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="logoURL" type="xs:anyURI" minOccurs="1" maxOccurs="1" nillable="false"/>
            <xs:element name="iframeSize" type="pxm:iframeSize" minOccurs="0" maxOccurs="1"/>
            <xs:element name="requestorIds" type="pxm:requestorIds" minOccurs="0" maxOccurs="1"/>
        </xs:all>
    </xs:complexType>
    <xs:element name="proxiedMvpds">
        <xs:annotation>
            <xs:documentation>List of Proxied MVPD</xs:documentation>
        </xs:annotation>
        <xs:complexType>
            <xs:sequence>
                <xs:element name="proxiedMvpd" type="pxm:proxiedMvpd" minOccurs="0" maxOccurs="unbounded"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

**Observações sobre os elementos:**

* `id` (obrigatório) - A ID de MVPD com proxy deve ser uma string relevante para o nome do MVPD, usando qualquer um dos seguintes caracteres (já que será exposta aos Programadores para fins de rastreamento):
   * Quaisquer caracteres alfanuméricos, sublinhado (&quot;_&quot;) e hífen (&quot;-&quot;).
   * O idID deve estar em conformidade com a seguinte expressão regular:
     `(a-zA-Z0-9((-)|_)*)`

     Assim, ele deve ter pelo menos um caractere, começar com uma letra e continuar com qualquer letra, dígito, traço ou sublinhado.

* `iframeSize` (opcional) - O elemento iframeSize é opcional e define o tamanho do iFrame se a página de autenticação MVPD deve estar em um iFrame. Caso contrário, se o elemento iframeSize não estiver presente, a autenticação ocorrerá em uma página de redirecionamento completa do navegador.
* `requestorIds` (opcional) - Os valores requestorIds serão fornecidos pelo Adobe. Um requisito é que um MVPD com proxy seja integrado a pelo menos um requestorId. Se a tag &quot;requestorIds&quot; não estiver presente no elemento MVPD com proxy, esse MVPD com proxy será integrado a todos os solicitantes disponíveis integrados no MVPD de proxy.
* `ProviderID` (opcional) - Quando o atributo ProviderID está presente no elemento de ID, o valor de ProviderID é enviado na solicitação de autenticação SAML para o Proxy MVPD como o Proxy MVPD / SubMVPD ID ID (em vez do valor da ID). Nesse caso, o valor de id será usado somente no seletor de MVPD apresentado na página Programador e internamente pela Autenticação Adobe Pass. O comprimento do atributo ProviderID deve ter entre 1 e 128 caracteres.

## Segurança {#security}

Para que uma solicitação seja considerada válida, ela deve respeitar as seguintes regras:

* O cabeçalho da solicitação deve conter o parâmetro apikey de segurança. (Essa é uma chave de aplicativo que identificará exclusivamente as chamadas do MVPD do Proxy.)
* A solicitação deve vir de um endereço IP específico que foi permitido.
* A solicitação deve ser enviada pelo protocolo SSL.

O Adobe fornecerá o valor (estático) do token. Esse valor é usado no processo de autenticação e autorização.  Quaisquer parâmetros presentes no cabeçalho da solicitação que não estejam listados acima serão ignorados.

Exemplo de ondulação:

`curl -X GET -H "apikey: ???provided-by-adobe???" "https://mgmt-prequal.auth-staging.adobe.com/control/v1/proxiedMvpds"`

## Pontos de Extremidade de Serviço Web MVPD do Proxy para os Ambientes de Autenticação do Adobe Pass {#proxy-mvpd-wevserv-endpoints}

* **URL de produção:** https://mgmt.auth.adobe.com/control/v1/proxiedMvpds
* **URL de preparo:** https://mgmt.auth-staging.adobe.com/control/v1/proxiedMvpds
* **URL de pré-produção:** https://mgmt-prequal.auth.adobe.com/control/v1/proxiedMvpds
* **URL de pré-preparação:** https://mgmt-prequal.auth-staging.adobe.com/control/v1/proxiedMvpds

<!--
>[!RELATEDINFORMATION]
>* [Proxy MVPD SAML integration](/help/authentication/proxy-mvpd-saml-int.md)
>* [User metadata exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Technical paper](/help/authentication/technical-paper.md)
>* [Adobe Pass Authentication glossary](/help/authentication/glossary.md)
-->
