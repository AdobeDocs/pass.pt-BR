---
title: Verificar token de autenticação
description: Verificar token de autenticação
exl-id: 9020f261-44d8-4bd5-b85b-a8667679f563
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# Verificar token de autenticação {#check-authentication-token}

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

Indica se o dispositivo tem um token de autenticação não expirado.

| Endpoint | Chamado  </br>Por | Entrada   </br>Params | HTTP  </br>Método | Resposta | HTTP  </br>Resposta |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/checkauthn | Aplicativo de transmissão</br></br>ou</br></br>Serviço de programador | 1. solicitante (obrigatório)</br>2.  deviceId (Obrigatório)</br>3.  device_info/X-Device-Info (Obrigatório)</br>4.  _deviceType_ </br>5.  _deviceUser_ (Obsoleto)</br>6.  _appId_ (Obsoleto) | GET | XML ou JSON que contém detalhes de erros, caso não seja bem-sucedido. | 200 - Sucesso   </br>403 - Sem Sucesso |

{style="table-layout:auto"}


| Parâmetro de entrada | Descrição |
| --- | --- |
| solicitante | O requestorId do Programador para o qual esta operação é válida. |
| deviceId | Os bytes de id do dispositivo. |
| device_info/</br></br>X-Device-Info | Informações do dispositivo de transmissão.</br></br>**Nota**: PODE ser passado para device_info como um parâmetro de URL, mas devido ao tamanho potencial desse parâmetro e às limitações no comprimento de um URL GET, ELE DEVE ser passado como X-Device-Info no cabeçalho http. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)(/help/authentication/passing-client-information-device-connection-and-application.md)-->. |
| _deviceType_ | O tipo de dispositivo (por exemplo, Roku, PC).</br></br>Se esse parâmetro estiver definido corretamente, o ESM oferecerá métricas que são [detalhado por tipo de dispositivo](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) ao usar sem cliente, para que diferentes tipos de análise possam ser executados para, por exemplo, Roku, Apple TV, Xbox etc.</br></br>Para obter detalhes, consulte [Benefícios do uso do parâmetro deviceType sem cliente nas métricas de autenticação do Adobe Pass ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br>**Nota**: device_info substituirá esse parâmetro. |
| _deviceUser_ | O identificador do usuário do dispositivo. |
| _appId_ | O id/nome do aplicativo.</br>**Nota**: device_info substitui esse parâmetro. |

{style="table-layout:auto"}


## Resposta (se malsucedida) {#response}

```JSON
    <error>
      <status>403</status>
      <message>Authentication token expired</message>
    </error>
```

### [Voltar para Referência da API REST](/help/authentication/rest-api-reference.md)
