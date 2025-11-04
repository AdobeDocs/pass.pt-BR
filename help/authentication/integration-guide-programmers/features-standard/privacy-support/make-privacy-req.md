---
title: Como fazer uma solicitação de privacidade
description: Como fazer uma solicitação de privacidade
exl-id: abb21306-98d6-4899-914a-bdfa85cbd204
source-git-commit: d0f08314d7033aae93e4a0d9bc94af8773c5ba13
workflow-type: tm+mt
source-wordcount: '558'
ht-degree: 0%

---

# Como fazer uma solicitação de privacidade {#howto-make-privacy-request}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

## Identificadores e namespaces {#identifier-namespace}

Ao enviar uma solicitação de Privacidade de acesso ou exclusão, o aplicativo do cliente precisa incluir os seguintes identificadores:

* **mvpdID** - Identificador exclusivo do MVPD.
* **userID** - Identifica com exclusividade o usuário de um aplicativo do Programador, mas é originado da MVPD. Consulte Compreensão das IDs de usuário na Visão geral do programador.
* **IMSOrgID** - a ID da organização do serviço Identity Management da Adobe Experience Cloud, que identifica exclusivamente o cliente na Adobe Experience Cloud


Verifique a amostra abaixo:

```JSON
"userIDs": [{
     "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",  -----> "Dish" is the id of the MVPD
     "type":"unregistered",
     "value":"1234-5678-8765-4321" ----> "1234-5678-8765-4321" is the userId associated with the MVPD account
}]
```

>[!IMPORTANT]
>
>Os usuários devem ser autenticados para gerar solicitações de privacidade para a Autenticação do Adobe Pass. Caso contrário, os programadores devem encontrar outros meios de extrair a ID de usuário do MVPD.

## Tipos de solicitações {#req-type}

A Autenticação do Adobe Pass é compatível com solicitações de Acesso e Exclusão.

### Access {#access-req}

Para uma solicitação de acesso:

forneceremos de volta um arquivo JSON que contém um resumo do número total de solicitações de autenticação e autorização criadas para esse Titular de dados.
todos esses eventos são filtrados por Cliente.


**Solicitar amostra**

Você deve fazer upload de um JSON com os identificadores de autenticação da Adobe Pass para os quais está enviando a solicitação de acesso a dados. Para ver a aparência de um JSON bem formado, consulte esta amostra:

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["access"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**Amostra de resposta**

```JSON
{
    "jobId": "d9a6b417-f619-4420-82a3-09f61fa8eff3",
    "requestId": "15765127177927284RX-739",
    "userKey": "John Dow",
    "action": "access",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/16/2019 04:11 PM GMT",
    "lastModifiedDate": "12/16/2019 04:15 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/16/2019 04:15 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-16T16:15:23.4Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "6",
                        "numberOfAuthorizationDecisions": "11"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/d9a6b417-f619-4420-82a3-09f61fa8eff3/d9a6b417-f619-4420-82a3-09f61fa8eff3.zip",
    "regulation": "ccpa"
}
```

### Excluir {#delete-req}

Você deve fazer upload de um JSON com os identificadores de autenticação da Adobe Pass para os quais está enviando a solicitação de exclusão de dados. Para ver a aparência de um JSON bem formado, consulte esta amostra:

**Solicitar amostra**

```JSON
{
    "companyContexts": [{
            "namespace": "imsOrgID",
            "value": "1234567890@AdobeOrg"
        }
    ],
    "users": [{
            "key": "John Dow",
            "action": ["delete"],
            "userIDs": [{
                "namespace":"http://www.adobe.com/primetimeAuthentication/Dish",
                "type":"unregistered",
                "value":"1234-5678-8765-4321"
             }]
         
        }
    ],
    
    "include":["primetimeAuthentication"],
    "regulation" : "ccpa"
}
```

**Amostra de resposta**

Para uma solicitação Delete:

* compartilhamos apenas um recibo de que os dados foram removidos, não um arquivo agregado com todos os dados removidos.
* o recibo incluído na resposta contém um resumo do número total de tokens de autenticação e autorização encontrados para esse titular de dados.

```JSON
{
    "jobId": "aab380d1-a0cd-4a0d-ba95-2649ee90c063",
    "requestId": "15759883098453100RX-074",
    "userKey": "John Dow",
    "action": "delete",
    "status": "complete",
    "submittedBy": "564f7c9a-e0c8-4e74-99b8-20317ae1e235@techacct.adobe.com",
    "createdDate": "12/10/2019 02:31 PM GMT",
    "lastModifiedDate": "12/10/2019 02:34 PM GMT",
    "userIds": [
        {
            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
            "value": "1234-5678-8765-4321",
            "type": "unregistered",
            "isDeletedClientSide": false
        }
    ],
    "productResponses": [
        {
            "product": "Adobe Pass Authentication",
            "retryCount": 0,
            "processedDate": "12/10/2019 02:34 PM GMT",
            "productStatusResponse": {
                "status": "complete",
                "results": {
                    "userContexts": [
                        {
                            "namespace": "http://www.adobe.com/primetimeAuthentication/Dish",
                            "namespaceId": 0,
                            "value": "1234-5678-8765-4321",
                            "type": "unregistered"
                        }
                    ],
                    "receiptData": {
                        "createdAt": "2019-12-10T14:34:55.274Z",
                        "message": "Data summary",
                        "numberOfAuthenticationSessions": "2",
                        "numberOfAuthorizationDecisions": "3"
                    }
                },
                "message": "Success"
            }
        }
    ],
    "downloadUrl": "https://va7gdprdevblob.blob.core.windows.net/va7gdprdevblobpublic/usa/4161962b9e8ef0027453d7cc02ecd93d/aab380d1-a0cd-4a0d-ba95-2649ee90c063/aab380d1-a0cd-4a0d-ba95-2649ee90c063.zip",
    "regulation": "ccpa"
}
```

## Como acionar uma solicitação {#trigger-req}

Há duas opções para os clientes enviarem solicitações de privacidade para o Adobe:

* **manualmente** - usando a [Interface do Usuário do Privacy Service](#privacy-service-ui)
* **automaticamente** - usando a [API do Privacy Service](#privacy-service-api)

### Ao usar a interface do Privacy Service {#privacy-service-ui}

Um [tutorial completo](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=pt-BR#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md) sobre como acessar e usar a Interface do Usuário do Privacy Service está disponível online por meio dos serviços da Adobe I/O. Além disso, os clientes podem usar este link para acessar a biblioteca de vídeos e artigos sobre Regulamentos de privacidade. Clique no menu Adobe Experience Cloud e GDPR. Isso abrirá vários vídeos - &quot;Como fazer a interface do GDPR&quot; explica como usá-la.

Na interface do usuário, os clientes precisam carregar sua própria IMSOrgID e um JSON contendo detalhes de solicitações do GDPR para cada produto.

### Ao usar a API do Privacy Service {#privacy-service-api}

O Adobe Experience Platform Privacy Service fornece uma facilitação comum e centralizada de solicitações de acesso/exclusão e solicitações de não participação na venda para dados privados.

A **documentação da API do Privacy Service** aborda detalhadamente como um cliente da Adobe pode se integrar à API do Adobe.

**Visualizar chamadas de API com o Postman (um software gratuito de terceiros):**

* [Coleção de Postman da API do Privacy Service no GitHub](https://github.com/adobe/experience-platform-postman-samples/blob/master/apis/experience-platform/Privacy%20Service%20API.postman_collection.json)
* [Guia de vídeo para criar o ambiente do Postman](https://video.tv.adobe.com/v/31682?captions=por_br)
* [Etapas para importar ambientes e coleções no Postman](https://learning.postman.com/docs/running-collections/intro-to-collection-runs/)


**Caminhos de API:**

* URL DO PLATFORM Gateway: `https://platform.adobe.io/`
* Caminho base para esta API: `/data/core/privacy/jobs`
* Exemplo de um caminho completo: `https://platform.adobe.io/data/core/privacy/jobs/ping`


**Cabeçalhos obrigatórios:**

* Todas as chamadas exigem os cabeçalhos `Authorization`, `x-gw-ims-org-id` e `x-api-key`. Para obter mais informações sobre como obter esses valores, consulte o **tutorial de autenticação**.
* Todas as solicitações com uma carga no corpo da solicitação (como chamadas POST, PUT e PATCH) devem incluir o cabeçalho `Content-Type` com um valor de `application/json`.

<!--

>[!RELATEDINFORMATION]
>
>* [Privacy Services Overview](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=pt-BR#!api-specification/markdown/narrative/tutorials/privacy_service_tutorial/privacy_service_ui_tutorial.md)
>* Privacy Service API documentation

-->
