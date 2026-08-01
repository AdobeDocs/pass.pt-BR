---
title: Excluir Registro de Registro
description: Excluir registro de registro
exl-id: 42707070-2e1f-4847-93fd-30025aef56c1
source-git-commit: 689e2f86550d9fa59337c15dd38767975a1d6d30
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 2%

---

# Excluir Registro (Herdado) {#delete-registration-record}

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


## Descrição {#delete-record}

Exclui o registro de código de registro e libera o código de registro para reutilização.

| Endpoint | Chamado </br>por | </br>Parâmetros de entrada | HTTP </br>Método | Resposta | Resposta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;REGGIE_FQDN>/reggie/v1/{requestorId}/regcode/{registrationCode}</br></br>Por exemplo:</br></br>&lt;REGGIE_FQDN>/reggie/v1/regcode/ER45RTY | Aplicativo de Streaming</br></br>ou</br></br>Serviço de Programador | &#x200B;1.  ID do Solicitante </br> (componente do Caminho)</br>2.  Código de registro </br> (componente do Caminho) | DELETE | Nenhum | 204 |

{style="table-layout:auto"}

</br>

| Parâmetro de entrada | Descrição |
| --- | --- |
| solicitante | O requestorId do Programador para o qual esta operação é válida. |
| código de registro | O valor do código de registro que seria exibido no dispositivo de transmissão (a ser inserido no fluxo de autenticação). |

{style="table-layout:auto"}

</br>

**[Voltar à Referência da API REST](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)**
