---
title: Fornecer lista do MVPD
description: Fornecer lista do MVPD
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 2%
---
# (Herdado) Fornecer lista do MVPD {#provide-mvpd-list}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

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

| Endpoint | Chamado </br>por | </br>Parâmetros de entrada | HTTP </br>Método | Resposta | Resposta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/config/{requestorId}</br></br>Por exemplo:</br></br>&lt;SP_FQDN>/api/v1/config/sampleRequestId | Adobe Pass Authentication | &#x200B;1.  Solicitante</br> (componente de Caminho)</br>_2.  deviceType (desaprovado)_ | GET | XML ou JSON contendo a lista de MVPDs. | 200 |

{style="table-layout:auto"}


| Parâmetro de entrada | Descrição |
| --------------- | ------------------------------------------------------------- |
| solicitante | O requestorId do Programador para o qual esta operação é válida. |
| *deviceType* | Tipo de dispositivo. |

{style="table-layout:auto"}

### Exemplo de resposta {#sample-response}

Igual à resposta XML do MVPD existente ao servlet /config

Observação: todos os MVPDs configurados para usar o Platform SSO terão as seguintes propriedades extras no nó correspondente (JSON/XML):

* **enablePlatformServices (booleano): sinalizador** indicando se este MVPD é integrado via SSO da Plataforma
* **boardingStatus (cadeia de caracteres): sinalizador** indicando se o MVPD oferece suporte total a Platform SSO (SUPORTADO) ou se o MVPD só aparece no seletor de plataforma (SELETOR)
* **displayInPlatformPicker (booleano):** este MVPD deve aparecer no seletor de plataforma
* **platformMappingId (cadeia de caracteres):** o identificador deste MVPD, conforme conhecido pela plataforma
* **requiredMetadataFields (matriz de cadeia de caracteres):** os campos de metadados do usuário devem estar disponíveis em um logon bem-sucedido
