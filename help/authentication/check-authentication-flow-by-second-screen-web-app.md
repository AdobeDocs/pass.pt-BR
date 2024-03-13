---
title: Verificar Fluxo de Autenticação por Aplicativo Web de Segunda Tela
description: Verificar Fluxo de Autenticação por Aplicativo Web de Segunda Tela
exl-id: 5807f372-a520-4069-b837-67ae41b7f79b
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 0%

---

# Verificar Fluxo de Autenticação por Aplicativo Web de Segunda Tela {#check-authentication-flow-by-second-screen-web-app}

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

Essa API deve ser consumida pelo segundo aplicativo Web de logon de tela para confirmar que a Autenticação do Adobe Pass reconheceu o logon bem-sucedido do MVPD. Recomendamos chamar essa API antes de mostrar uma mensagem de sucesso para o usuário final que o instrui a prosseguir para o console do dispositivo para continuar com os workflows.


| Endpoint | Chamado  </br>Por | Entrada   </br>Params | HTTP  </br>Método | Resposta | HTTP  </br>Resposta |
| --- | --- | --- | --- | --- | --- |
| SP_FQDN/api/v1/checkauthn/{código de registro} | Aplicativo Web de Logon | 1. código de registro  </br>    (Componente do caminho)</br>2.  solicitante  </br>    (Obrigatório) | GET | XML ou JSON que contém detalhes de erros, caso não seja bem-sucedido. | 200 - Sucesso   </br>403 - Proibido |

</br>

| Parâmetro de entrada | Descrição |
| ----------------- | --------------------------------------------------------------------------------------------- |
| código de registro | O valor do código de registro fornecido pelo usuário no início do fluxo de autenticação. |
| solicitante | O requestorId do Programador para o qual esta operação é válida. |


### Exemplo de resposta (em caso de erro) {#response}

```JSON
    {
        "status": 403,
        "message": "Forbidden"
    }
```

### [Voltar para Referência da API REST](/help/authentication/rest-api-reference.md)
