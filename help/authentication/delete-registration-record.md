---
title: Excluir Registro de Registro
description: Excluir registro de registro
exl-id: 42707070-2e1f-4847-93fd-30025aef56c1
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 2%

---

# Excluir Registro de Registro {#delete-registration-record}

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


## Descrição {#delete-record}

Exclui o registro de código de registro e libera o código de registro para reutilização.

| Endpoint | Chamado  </br>Por | Entrada   </br>Params | HTTP  </br>Método | Resposta | HTTP  </br>Resposta |
| --- | --- | --- | --- | --- | --- |
| &lt;reggie_fqdn>/reggie/v1/{requestorId}/regcode/{registrationCode}</br></br>Por exemplo:</br></br>&lt;reggie_fqdn>/reggie/v1/regcode/ER45RTY | Aplicativo de transmissão</br></br>ou</br></br>Serviço de programador | 1. ID do solicitante  </br>    (Componente do caminho)</br>2.  Código de registro  </br>    (Componente do caminho) | DELETE | Nenhum | 204 |

{style="table-layout:auto"}

</br>

| Parâmetro de entrada | Descrição |
| --- | --- |
| solicitante | O requestorId do Programador para o qual esta operação é válida. |
| código de registro | O valor do código de registro que seria exibido no dispositivo de transmissão (a ser inserido no fluxo de autenticação). |

{style="table-layout:auto"}

</br>

### [Voltar para Referência da API REST](/help/authentication/rest-api-reference.md)
