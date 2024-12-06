---
title: Fornecer Lista MVPD
description: Fornecer Lista MVPD
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 2%

---

# Fornecer Lista MVPD {#provide-mvpd-list}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!NOTE]
>
> A implementação da REST API é limitada por [Mecanismo de limitação](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## Endpoints da REST API {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Preparo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Preparo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descrição {#description}

Retorna a lista de MVPDs configurados para o solicitante.

| Endpoint | Chamado </br>por | Entrada   </br>Parâmetros | HTTP </br>Método | Resposta | Resposta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/config/{requestorId}</br></br>Por exemplo:</br></br>&lt;SP_FQDN>/api/v1/config/sampleRequestorId | Adobe Pass Authentication | 1. Solicitante</br>    (Componente do caminho)</br>_2.  deviceType (desaprovado)_ | GET | XML ou JSON contendo a lista de MVPDs. | 200 |

{style="table-layout:auto"}


| Parâmetro de entrada | Descrição |
| --------------- | ------------------------------------------------------------- |
| solicitante | O requestorId do Programador para o qual esta operação é válida. |
| *deviceType* | Tipo de dispositivo. |

{style="table-layout:auto"}

### Exemplo de resposta {#sample-response}

Igual à resposta XML do MVPD existente ao servlet /config

Observação: todos os MVPDs configurados para usar o Platform SSO terão as seguintes propriedades extras no nó correspondente (JSON/XML):

* **enablePlatformServices (booleano): sinalizador** indicando se este MVPD está integrado via SSO da Plataforma
* **boardingStatus (cadeia de caracteres): sinalizador** indicando se o MVPD oferece suporte total a Platform SSO (SUPPORTED) ou se o MVPD aparece somente no seletor de plataforma (PICKER)
* **displayInPlatformPicker (booleano):** este MVPD deve aparecer no seletor de plataforma?
* **platformMappingId (cadeia de caracteres):** o identificador deste MVPD conforme conhecido pela plataforma
* **requiredMetadataFields (matriz de cadeia de caracteres):** os campos de metadados do usuário devem estar disponíveis em um logon bem-sucedido
