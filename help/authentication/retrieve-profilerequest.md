---
title: Recuperar solicitação de perfil SSO da plataforma
description: Recuperar solicitação de perfil SSO da plataforma
exl-id: 44fd4e26-4d9a-4607-ac2c-b85d848f5fc6
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
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
> A implementação da REST API é limitada por [Mecanismo de limitação](/help/authentication/throttling-mechanism.md)

## Endpoints da REST API {#clientless-endpoints}

&lt;reggie_fqdn>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Estágios - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Estágios - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>

## Descrição {#description}

Esse recurso produz solicitações de perfil para uma ID do solicitante e uma tupla MVPD.


| Endpoint | Chamado  </br>Por | Entrada   </br>Params | HTTP  </br>Método | Resposta | HTTP  </br>Resposta |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/{requestor}/profile-requests/{mvpd} | Aplicativo de transmissão</br></br>ou</br></br>Serviço de programador | 1. solicitante (parâmetro de caminho)</br>2. mvpd (parâmetro de caminho)</br>3. deviceType (Obrigatório) | GET | O Content-Type da resposta será application/octet-stream, pois a carga real é opaca para o aplicativo cliente.</br></br>A resposta deve ser encaminhada pelo aplicativo para a Plataforma</br></br>Mecanismo SSO para obter um SSO de perfil. | 200 - Sucesso   </br>400 - Solicitação inválida |


| Parâmetro de entrada | Descrição |
| --------------- | -------------------------------------------------------------------------------------------------------- |
| solicitante | O requestorId do Programador para o qual esta operação é válida. |
| mvpd | A ID de MVPD para a qual esta operação é válida. |
| deviceType | A plataforma do Apple para a qual estamos tentando obter uma solicitação de perfil.  Ou **iOS** ou **tvOS**. |
