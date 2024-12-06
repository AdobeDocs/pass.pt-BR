---
title: Retornar Registro de Registro
description: Retornar Registro de Registro
exl-id: 7b9e63a2-59b6-4123-a19b-ee1f021219ea
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '252'
ht-degree: 2%

---

# Retornar Registro de Registro {#return-registration-record}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!NOTE]
>
> A implementação da REST API é limitada por [Mecanismo de limitação](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## Endpoints da REST API {#clientless-endpoints}

`<REGGIE_FQDN>`:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Preparo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

`<SP_FQDN>`:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Preparo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)




## Descrição {#description}

Retorna o registro do código de registro contendo o código de registro UUID, o código de registro e a ID do dispositivo com hash.






| Endpoint | Chamado </br>por | Entrada   </br>Parâmetros | HTTP </br>Método | Resposta | Resposta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| `<REGGIE_FQDN>`;/reggie/v1/`{requestorId}`/regcode/`{registrationCode}`<p>Por exemplo:<p>`<REGGIE_FQDN>`/reggie/v1/sampleRequestorId/regcode/TJJCFK?format=xml | Aplicativo de Streaming</br></br>ou</br></br>Serviço de Programador | 1. solicitante </br>    (Componente do caminho)</br>2.  código de registro </br>    (Componente do caminho) | GET | XML ou JSON contendo um código de registro e informações. Consulte esquema e amostra abaixo. | 200 |

{style="table-layout:auto"}




| Parâmetro de entrada | Descrição |
| --- | --- |
| solicitante | O requestorId do Programador para o qual esta operação é válida. |
| código de registro | O valor do código de registro que seria exibido no dispositivo de transmissão (a ser inserido no fluxo de autenticação). |




## Esquema XML de resposta {#response-xml-schema}

### Código de registro XSD

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="model.mvc.reggie.pass.adobe.com"
            targetNamespace="model.mvc.reggie.pass.adobe.com"
            attributeFormDefault="unqualified"
            elementFormDefault="unqualified">
        <xs:element name="regcode">
            <xs:complexType>
                <xs:all>
                    <xs:element name="id" type="xs:string" />
                    <xs:element name="code" type="xs:string" />
                    <xs:element name="requestor" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="mvpd" type="xs:string" minOccurs="1" maxOccurs="1"/
                    <xs:element name="generated" type="xs:long" />
                    <xs:element name="expires" type="xs:long" />
                    <xs:element name="info" type="infoType" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
        <xs:complexType name="infoType">
            <xs:all>
                <xs:element name="deviceId" type="xs:base64Binary" minOccurs="1" maxOccurs="1"/>
                <xs:element name="deviceType" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="deviceUser" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appId" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="appVersion" type="xs:string" minOccurs="0" maxOccurs="1"/>
                <xs:element name="registrationURL" type="xs:anyURI" minOccurs="0" maxOccurs="1"/>
            </xs:all>
        </xs:complexType>
    </xs:schema>
```

| Nome do elemento | Descrição |
| --- | --- |
| id | UUID gerado pelo Serviço de Código de Registro |
| código | Código de registro gerado pelo Serviço de código de registro |
| solicitante | ID do Solicitante |
| mvpd | ID MVPD |
| gerado | Carimbo de data e hora de criação do Código de registro (em milissegundos desde 1° de janeiro de 1970 GMT) |
| expira em | Carimbo de data e hora quando o código de registro expira (em milissegundos desde 1° de janeiro de 1970 GMT) |
| deviceId | Identificador exclusivo do dispositivo (ou token XSTS) |
| deviceType | Tipo de dispositivo |
| deviceUser | Usuário conectado ao dispositivo |
| appId | ID do aplicativo |
| appVersion | Versão do aplicativo |
| registrationURL | URL do Aplicativo Web de Logon a ser exibido para o usuário final |

{style="table-layout:auto"}

### Exemplo de resposta {#sample-response}

```XML
    <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
    <ns2:regcode xmlns:ns2="model.mvc.reggie.pass.adobe.com">
        <id>678f9fea-a1cafec8-1ff0-4a26-8564-f6cd020acf13</id>
        <code>TJJCFK</code>
        <requestor>sampleRequestorId</requestor>
        <mvpd>sampleMvpdId</mvpd>
        <generated>1348039846647</generated>
        <expires>1348043446647</expires>
        <info>
            <deviceId>dGhpc0lkQUR1bW15RGV2aWNlSWQ=</deviceId>
            <deviceType>xbox</deviceType>
            <deviceUser>JD</deviceUser>
            <appId>2345</appId>
            <appVersion>2.0</appVersion>
            <registrationURL>http://loginwebapp.com</registrationURL>
        </info>
    </ns2:regcode>
```
