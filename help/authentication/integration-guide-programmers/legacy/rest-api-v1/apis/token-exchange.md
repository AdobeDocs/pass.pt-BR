---
title: Trocar um token SSO da plataforma por um token de Adobe
description: Trocar um token SSO da plataforma por um token de Adobe
exl-id: 5ab60268-8f97-4755-8281-be45e812ed7f
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 0%

---

# (Herdado) Trocar um token SSO da plataforma por um token de Adobe {#exchange-a-platform-sso-token-for-an-adobe-token}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

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

Permite que um perfil SSO da Platform seja &quot;trocado&quot; por um token Adobe.

| Endpoint | Chamado </br>por | Entrada   </br>Parâmetros | HTTP </br>Método | Resposta | Resposta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/tokens/authn | Aplicativo de Streaming</br></br>ou</br></br>Serviço de Programador | 1. solicitante (Obrigatório)</br>    </br>2.  deviceId (Obrigatório)</br>    </br>3.  mvpd (Obrigatório)</br>    </br>4.  deviceType (Obrigatório)</br>    </br>5.  SAMLResponse (Obrigatório)</br>    </br>6.  deviceUser (obsoleto)</br>    </br>7.  appId (obsoleto) | POST | A resposta bem-sucedida será 204 Sem conteúdo, indicando que o token foi criado com êxito e está pronto para uso para os fluxos de autorização. | 204 - Sem conteúdo   </br>400 - Solicitação inválida |


| Parâmetro de entrada | Descrição |
| --- | --- |
| solicitante | O requestorId do Programador para o qual esta operação é válida. |
| deviceId | Os bytes de id do dispositivo. |
| mvpd | A MVPD Id para a qual esta operação é válida. |
| deviceType | A plataforma do Apple para a qual estamos tentando obter uma solicitação de perfil.  **iOS** ou **tvOS**. |
| SAMLResponse | O perfil real foi retornado pelo SSO da Platform. |
| _deviceUser_ | O identificador do usuário do dispositivo. |
| _appId_ | O id/nome do aplicativo. |
