---
title: Fornecer Lista MVPD
description: Fornecer Lista MVPD
exl-id: db2d8f19-d0b9-4195-bf0b-f9de0d96062b
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# Fornecer Lista MVPD {#provide-mvpd-list}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Endpoints da REST API {#clientless-endpoints}

&lt;reggie_fqdn>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Estágios - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Estágios - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descrição {#description}

Retorna a lista de MVPDs configurados para o solicitante.

| Endpoint | Chamado  </br>Por | Entrada   </br>Params | HTTP  </br>Método | Resposta | HTTP  </br>Resposta |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/config/{requestorId}</br></br>Por exemplo:</br></br>&lt;sp_fqdn>/api/v1/config/sampleRequestId | Autenticação Adobe Pass | 1. Solicitante</br>    (Componente do caminho)</br>_2.  deviceType (obsoleto)_ | GET | XML ou JSON contendo a lista de MVPDs. | 200 |

{style="table-layout:auto"}


| Parâmetro de entrada | Descrição |
| --------------- | ------------------------------------------------------------- |
| solicitante | O requestorId do Programador para o qual esta operação é válida. |
| *deviceType* | Tipo de dispositivo. |

{style="table-layout:auto"}

### Exemplo de resposta {#sample-response}

Igual à resposta XML do MVPD existente ao servlet /config

Observação: todos os MVPDs configurados para usar o Platform SSO terão as seguintes propriedades extras no nó correspondente (JSON/XML):

* **enablePlatformServices (booleano):** sinalizador que indica se este MVPD está integrado via SSO da Plataforma
* **boardingStatus (string):** sinalizador que indica se o MVPD oferece suporte total ao SSO da Plataforma (SUPORTADO) ou se o MVPD só aparece no seletor de plataforma (SELETOR)
* **displayInPlatformPicker (booleano):** esse MVPD deve aparecer no seletor de plataforma?
* **platformMappingId (cadeia de caracteres):** o identificador deste MVPD conforme conhecido pela plataforma
* **requiredMetadataFields (matriz de sequência):** os campos de metadados do usuário que devem estar disponíveis em um logon bem-sucedido
