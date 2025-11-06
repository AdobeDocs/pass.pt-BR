---
title: Serviço Web Proxy MVPD
description: Serviço Web Proxy MVPD
exl-id: f75cbc4d-4132-4ce8-a81c-1561a69d1d3a
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1027'
ht-degree: 0%

---


# Serviço Web Proxy MVPD {#proxy-mvpd-wbservice}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Antes de usar o serviço Web Proxy MVPD, verifique se os seguintes pré-requisitos estão sendo atendidos:
>
> * Obtenha as credenciais do cliente conforme descrito na documentação da API [Recuperar credenciais do cliente](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-client-credentials.md).
> * Obtenha o token de acesso conforme descrito na documentação da API [Recuperar token de acesso](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
>
> Consulte a documentação [Visão Geral do Registro Dinâmico do Cliente](../integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md) para obter mais informações sobre como criar um aplicativo registrado e baixar a instrução de software.

## Visão geral {#overview-proxy-mvpd-webserv}

Um &quot;Proxy MVPD&quot; é uma MVPD que, além de gerenciar sua própria integração com a Autenticação do Adobe Pass, também gerencia o processo de direito em nome de um grupo de &quot;MVPDs com proxy&quot; associados. Esse arranjo é transparente para os programadores.

Para implementar o recurso ProxyMVPD, a Autenticação Adobe Pass fornece serviços Web RESTful, com os quais ProxyMVPDs podem enviar e recuperar listas de ProxyMVPDs. O protocolo usado para essa API pública é REST HTTP, com as seguintes suposições:

&#x200B;- O Proxy MVPD usa o método HTTP GET para recuperar a lista dos MVPDs integrados atuais.
&#x200B;- O Proxy MVPD usa o método HTTP POST para atualizar a lista de MVPDs compatíveis.

## Serviços de proxy da MVPD {#proxy-mvpd-services}

&#x200B;- [Recuperar MVPDs com proxy](#retriev-proxied-mvpds)
&#x200B;- [Enviar MVPDs com proxy](#submit-proxied-mvpds)

### Recuperar MVPDs com proxy aplicado {#retriev-proxied-mvpds}

Recupera a lista atual de MVPDs com proxy integrado ao MVPD de proxy identificado.

| Endpoint | Chamado por | Parâmetros de solicitação | Cabeçalhos de solicitação | Método HTTP | Resposta HTTP |
|--------------------------------------------------------------------------|-----------|-----------------------|---------------------------|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| &lt;FQDN>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | Autorização (Obrigatória) | GET | <ul><li> 200 (ok) - A solicitação foi processada com êxito e a resposta contém uma lista de MVPDs Proxies no formato XML</li><li>401 (não autorizado) - Indica um dos seguintes:<ul><li>O cliente DEVE solicitar um novo access_token</li><li>A solicitação é originada de um endereço IP que não está presente na lista de permissões</li><li>O token não é válido</li></ul></li><li>403 (proibido) - Indica que a operação não é suportada para os parâmetros fornecidos ou que o MVPD proxy não está definido como proxy ou está ausente</li><li>405 (método não permitido) - Um método HTTP diferente de GET ou POST foi usado. O método HTTP geralmente não é compatível ou não é compatível com esse endpoint específico.</li><li>500 (erro interno do servidor) - Um erro foi gerado no lado do servidor durante o processo de solicitação.</li></ul> |

Exemplo de ondulação:

`curl -X GET -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/ProxyMVPD_Adobe/mvpds"`


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

Envia uma matriz de MVPDs integrados ao Proxy MVPD identificado.

| Endpoint | Chamado por | Parâmetros de solicitação | Cabeçalhos de solicitação | Método HTTP | Resposta HTTP |
|:------------------------------------------------------------------------:|:---------:|-----------------------|:---------------------------------------------------:|:-----------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| &lt;FQDN>/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds | ProxyMVPD | proxy-mvpd-identifier | Autorização (Obrigatória) por proxy-mvpds (Obrigatória) | POST | <ul><li>201 (criado) - O push foi processado com êxito</li><li>400 (solicitação incorreta) - o servidor não sabe como processar a solicitação:<ul><li>O XML de entrada não adere ao esquema publicado nesta especificação</li><li>Os mvpds com proxy não têm IDs exclusivas</li><li>O requestorIds enviado não existe. Motivo do contêiner Outro Servlet para o código de resposta 400</li></ul><li>401 (não autorizado) - Indica um dos seguintes:<ul><li>O cliente DEVE solicitar um novo access_token</li><li>A solicitação é originada de um endereço IP que não está presente na lista de permissões</li><li>O token não é válido</li></ul></li><li>403 (proibido) - Indica que a operação não é suportada para os parâmetros fornecidos ou que o MVPD proxy não está definido como proxy ou está ausente</li><li>405 (método não permitido) - Um método HTTP diferente de GET ou POST foi usado. O método HTTP geralmente não é compatível ou não é compatível com esse endpoint específico.</li><li>500 (erro interno do servidor) - Um erro foi gerado no lado do servidor durante o processo de solicitação.</li></ul> |

Exemplo de ondulação:

`curl -X POST -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth.adobe.com/control/v3/mvpd-proxies/ProxyMVPD_Adobe/mvpds" -d "proxied-mvpds=%3CproxiedMvpds%3E%3CproxiedMvpd%3E%3CdisplayName%3EFirst%20MVPD%20Name%3C%2FdisplayName%3E%3Cid%3EfirstMVPDId%3C%2Fid%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3C%2FproxiedMvpd%3E%3CproxiedMvpd%3E%3Cid%20ProviderID%3D%22ProviderID_Value_Sent_On_IdPEntry%22%3EmvpdPickerId%3C%2Fid%3E%3CdisplayName%3EMVPD%20Name%20Two%3C%2FdisplayName%3E%3ClogoURL%3E%3C%2FlogoURL%3E%3CrequestorIds%3E%3CrequestorId%3ETHE_REQUESTOR_ID%3C%2FrequestorId%3E%3C%2FrequestorIds%3E%3C%2FproxiedMvpd%3E%3C%2FproxiedMvpds%3E"`



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

A Adobe definiu o seguinte formato aceito para publicar/recuperar MVPDs com proxy de/para nosso serviço público da Web:

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

**Observações sobre elementos:**

&#x200B;- `id` (obrigatório) - A ID do MVPD com proxy deve ser uma cadeia de caracteres relevante ao nome do MVPD, usando qualquer um dos seguintes caracteres (já que será exposta aos Programadores para fins de rastreamento):
&#x200B;- Quaisquer caracteres alfanuméricos, sublinhado (&quot;_&quot;) e hífen (&quot;-&quot;).
&#x200B;- O idID deve estar em conformidade com a seguinte expressão regular:
`(a-zA-Z0-9((-)|_)*)`

    Portanto, ele deve ter pelo menos um caractere, começar com uma letra e continuar com qualquer letra, dígito, traço ou sublinhado.

&#x200B;- `iframeSize` (opcional) - O elemento iframeSize é opcional e define o tamanho do iFrame se a página de autenticação do MVPD deve estar em um iFrame. Caso contrário, se o elemento iframeSize não estiver presente, a autenticação ocorrerá em uma página de redirecionamento completa do navegador.
&#x200B;- `requestorIds` (opcional) - Os valores requestorIds serão fornecidos pela Adobe. Um requisito é que um MVPD com proxy deve ser integrado a pelo menos uma requestorId. Se a tag &quot;requestorIds&quot; não estiver presente no elemento MVPD com proxy, essa MVPD com proxy será integrada a todos os solicitantes disponíveis integrados na MVPD de proxy.
&#x200B;- `ProviderID` (opcional) - Quando o atributo ProviderID está presente no elemento de ID, o valor de ProviderID será enviado na solicitação de autenticação SAML para o Proxy MVPD como a ID de Proxy MVPD/SubMVPD (em vez do valor de ID). Nesse caso, o valor de id será usado somente no seletor de MVPD apresentado na página Programador e internamente pela Autenticação Adobe Pass. O comprimento do atributo ProviderID deve ter entre 1 e 128 caracteres.

## Segurança {#security}

Para que uma solicitação seja considerada válida, ela deve respeitar as seguintes regras:

&#x200B;- O cabeçalho da solicitação deve conter o token de acesso Oauth2 de segurança obtido conforme descrito na documentação da API [Recuperar token de acesso](../integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md).
&#x200B;- A solicitação deve vir de um endereço IP específico que foi permitido.
&#x200B;- A solicitação deve ser enviada pelo protocolo SSL.

Quaisquer parâmetros presentes no cabeçalho da solicitação que não estejam listados acima serão ignorados.

Exemplo de ondulação:

`curl -X GET -H "Authorization: Bearer <access_token_here>" "https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/<proxy-mvpd-identifier>/mvpds"`

## Pontos de extremidade do serviço da Web Proxy MVPD para os ambientes de autenticação da Adobe Pass {#proxy-mvpd-wevserv-endpoints}

&#x200B;- **URL de Produção:** https://mgmt.auth.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
&#x200B;- **URL de Preparo:** https://mgmt.auth-staging.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
&#x200B;- **URL de pré-produção:** https://mgmt-prequal.auth.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds
&#x200B;- **URL de Pré-Preparo:** https://mgmt-prequal.auth-staging.adobe.com/control/v3/mvpd-proxies/&lt;proxy-mvpd-identifier>/mvpds

<!--
>[!RELATEDINFORMATION]
>* [Proxy MVPD SAML integration](/help/authentication/proxy-mvpd-saml-int.md)
>* [User metadata exchange](/help/authentication/mvpd-user-metadata-exchng.md)
>* [Technical paper](/help/authentication/technical-paper.md)
>* [Adobe Pass Authentication glossary](/help/authentication/glossary.md)
-->
