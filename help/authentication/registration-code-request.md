---
title: Página de registro
description: Página de registro
exl-id: 581b8e2e-7420-4511-88b9-f2cd43a41e10
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 0%

---

# Página de registro {#registration-page}

## Endpoints da REST API {#clientless-endpoints}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!NOTE]
>
> A implementação da REST API é limitada por [Mecanismo de limitação](/help/authentication/throttling-mechanism.md)

&lt;REGGIE_FQDN>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Preparo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Preparo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descrição {#create-reg-code-svc}

Retorna o Código de registro gerado aleatoriamente e o URI da página de logon.

| Endpoint | Chamado </br>por | Entrada   </br>Parâmetro | HTTP </br>Método | Resposta | Resposta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestor}/regcode</br>Por exemplo:</br>REGGIE_FQDN/reggie/v1/sampleRequestorId/regcode | Aplicativo de Streaming</br>ou</br>Serviço de Programador | 1. solicitante </br>    (Componente do caminho)</br>2.  deviceId (com hash)   </br>    (Obrigatório)</br>3.  device_info/X-Device-Info (Obrigatório)</br>4.  mvpd (Opcional)</br>5.  ttl (Opcional)</br>6.  _deviceType_</br> 7.  _deviceUser_ (Obsoleto)</br>8.  _appId_ (obsoleto) | POST | XML ou JSON que contém um código de registro e informações ou detalhes de erro, caso seja malsucedido. Consulte schemas e exemplos abaixo. | 201 |

{style="table-layout:auto"}

| Parâmetro de entrada | Descrição |
| --- | --- |
| solicitante | O requestorId do Programador para o qual esta operação é válida. |
| deviceId | Os bytes de id do dispositivo. |
| device_info/</br>X-Device-Info | Informações do dispositivo de transmissão.</br>**Observação**: isso PODE ser passado para device_info como um parâmetro de URL, mas devido ao tamanho potencial desse parâmetro e às limitações no comprimento de uma URL GET, DEVE ser passado como X-Device-Info no cabeçalho http. </br>Veja os detalhes completos em [Passando Informações sobre Dispositivo e Conexão](/help/authentication/passing-client-information-device-connection-and-application.md). |
| mvpd | A ID de MVPD para a qual esta operação é válida. |
| ttl | Por quanto tempo esse regcode deve durar em segundos.</br>**Observação**: o valor máximo permitido para ttl é de 36.000 segundos (10 horas). Valores mais altos resultam em uma resposta HTTP 400 (solicitação incorreta). Se `ttl` for deixado em branco, a Autenticação Adobe Pass definirá um valor padrão de 30 minutos. |
| _deviceType_ | O tipo de dispositivo (por exemplo, Roku, PC).</br>Se este parâmetro estiver definido corretamente, o ESM oferecerá métricas que são [analisadas por tipo de dispositivo](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) ao usar o sem cliente, para que diferentes tipos de análise possam ser executados, por exemplo, Roku, Apple TV e Xbox.</br>Consulte, [Vantagens de usar o parâmetro de tipo de dispositivo sem cliente em métricas de passagem ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**Observação**: device_info substituirá esse parâmetro. |
| _deviceUser_ | O identificador do usuário do dispositivo. |
| _appId_ | O id/nome do aplicativo. </br>**Observação**: device_info substitui este parâmetro. |

{style="table-layout:auto"}


>[!CAUTION]
>
>**Endereço IP do Dispositivo de Streaming**
></br>
>Para implementações de Cliente para servidor, o Endereço IP do dispositivo de transmissão é implicitamente enviado com esta chamada.  Para implementações de Servidor para Servidor, em que a chamada **regcode** é feita pelo Serviço de Programador e não pelo Dispositivo de Streaming, o seguinte cabeçalho é necessário para passar o Endereço IP do Dispositivo de Streaming:
>
>
>```
>X-Forwarded-For : <streaming_device_ip> 
>```
>
>onde `<streaming\_device\_ip>` é o endereço IP público do Dispositivo de Streaming.
></br></br>
>Exemplo: </br>
>
>```
>POST /reggie/v1/{req_id}/regcode HTTP/1.1</br>X-Forwarded-For:203.45.101.20
>```
>
</br>

### Esquema XML de resposta {#xml-schema}


#### Código de registro XSD {#registration-code-xsd}

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

</br>

| Nome do elemento | Descrição |
| --------------- | ------------------------------------------------------------------------------------ |
| id | UUID gerado pelo Serviço de Código de Registro |
| código | Código de registro gerado pelo Serviço de código de registro |
| solicitante | ID do Solicitante |
| mvpd | ID do Mvpd |
| gerado | Carimbo de data e hora de criação do Código de registro (em milissegundos desde 1° de janeiro de 1970 GMT) |
| expira em | Carimbo de data e hora quando o código de registro expira (em milissegundos desde 1° de janeiro de 1970 GMT) |
| deviceId | Identificador exclusivo do dispositivo (ou token XSTS) |
| deviceType | Tipo de dispositivo |
| deviceUser | Usuário conectado ao dispositivo |
| appId | ID do aplicativo |
| appVersion | Versão do aplicativo |
| registrationURL | URL do Aplicativo Web de Logon a ser exibido para o usuário final |

{style="table-layout:auto"}


</br>

### XSD da mensagem de erro  {#error-message}

```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns="rest.pass.adobe.com"
               targetNamespace="rest.pass.adobe.com"
               attributeFormDefault="unqualified"
               elementFormDefault="unqualified">
        <xs:element name="error">
            <xs:complexType>
                <xs:all>
                    <xs:element name="status" type="xs:int" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="message" type="xs:string" minOccurs="1" maxOccurs="1"/>
                    <xs:element name="details" type="xs:string" minOccurs="0" maxOccurs="1"/>
                </xs:all>
            </xs:complexType>
        </xs:element>
    </xs:schema>
```


### Exemplo de resposta {#sample-response}

**XML:**

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

**JSON:**

```JSON
    {
        "id": "678f9fea-9d364b29-246c-488f-b97e-298566d1b9e0",
        "code": "D4BDU2W",
        "requestor": "sampleRequestorId",
        "mvpd": "sampleMvpdId",
        "generated": 1348039555877,
        "expires": 1348043155877,
        "info": {
            "deviceId": "dGhpc0l.kQUR1bW15RGV2.aWNlSWQ=",
            "deviceType": "xboxOne",
            "deviceUser": "JD",
            "appId": "2345",
            "appVersion": "2.0",
            "registrationURL": "http://loginwebapp.com"
        }
    }
```
