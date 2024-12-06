---
title: Recuperar solicitação de perfil SSO da plataforma
description: Recuperar solicitação de perfil SSO da plataforma
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 1%

---

# Recuperar solicitação de perfil SSO da plataforma {#retrieve-platform-sso-profile-request}

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

Esse recurso produz solicitações de perfil para uma ID do solicitante e uma tupla MVPD.


| Endpoint | Chamado </br>por | Entrada   </br>Parâmetros | HTTP </br>Método | Resposta | Resposta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/{requestor}/profile-requests/{mvpd} | Aplicativo de Streaming</br></br>ou</br></br>Serviço de Programador | 1. solicitante (parâmetro de caminho)</br>2. mvpd (parâmetro de caminho)</br>3. deviceType (Obrigatório) | GET | O Content-Type da resposta será application/octet-stream, pois a carga real é opaca para o aplicativo cliente.</br></br>A resposta deve ser encaminhada pelo aplicativo para o mecanismo SSO da Plataforma</br></br>para obter um SSO de Perfil. | 200 - Sucesso   </br>400 - Solicitação inválida |


| Parâmetro de entrada | Descrição |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| solicitante | O requestorId do Programador para o qual esta operação é válida. |
| mvpd | A ID de MVPD para a qual esta operação é válida. |
| deviceType | A plataforma do Apple para a qual estamos tentando obter uma solicitação de perfil.  **iOS** ou **tvOS**. |
