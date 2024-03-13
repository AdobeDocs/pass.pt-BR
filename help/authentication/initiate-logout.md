---
title: Iniciar logout
description: Iniciar logout
exl-id: 9625b5a2-31d9-4e20-8703-4a9e4eeb1618
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 1%

---

# Iniciar logout {#initiate-logout}

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

Remova os tokens AuthN e AuthZ do armazenamento.


| Endpoint | Chamado  </br>Por | Entrada   </br>Params | HTTP  </br>Método | Resposta | HTTP  </br>Resposta |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/logout | Aplicativo de transmissão</br></br>ou</br></br>Serviço de programador | 1. requerente</br>2.  deviceId (Obrigatório)</br>3.  device_info/X-Device-Info (Obrigatório)</br>4.  _deviceType_</br> 5.  _deviceUser_ (Obsoleto)</br>6.  _appId_ (Obsoleto) | DELETE | Nenhum | 204 |


| Parâmetro de entrada | Descrição |
| --- | --- |
| solicitante | O requestorId do Programador para o qual esta operação é válida. |
| deviceId | Os bytes de id do dispositivo. |
| device_info/</br></br>X-Device-Info | Informações do dispositivo de transmissão.</br></br>**Nota**: isso PODE ser passado para device_info como um parâmetro de URL, mas devido ao tamanho potencial desse parâmetro e às limitações no comprimento de um URL do GET, ele DEVE ser passado como X-Device-Info no cabeçalho http. </br></br><!--See the full details in [Passing Device and Connection Information](http://tve.helpdocsonline.com/passing-device-information)-->. |
| _deviceType_ | O tipo de dispositivo (por exemplo, Roku, PC).</br></br>Se esse parâmetro estiver definido corretamente, o ESM oferecerá métricas que são [detalhado por tipo de dispositivo](/help/authentication/entitlement-service-monitoring-overview.md#clientless_device_type) ao usar sem cliente, para que diferentes tipos de análise possam ser executados para, por exemplo, Roku, Apple TV, Xbox etc.</br></br>Consulte [Benefícios do uso do parâmetro do tipo de dispositivo sem cliente nas métricas de passagem ](/help/authentication/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Nota**: device_info substituirá esse parâmetro. |
| _deviceUser_ | O identificador do usuário do dispositivo.</br></br>**Nota**: Se usado, deviceUser deve ter os mesmos valores que no [Criar código de registro](/help/authentication/registration-code-request.md) solicitação. |
| _appId_ | O id/nome do aplicativo. </br></br>**Nota**: device_info substitui esse parâmetro. Se usado, `appId` deve ter os mesmos valores que no [Criar código de registro](/help/authentication/registration-code-request.md) solicitação. |

>[!IMPORTANT]
> 
>A chamada de logout tem a seguinte limitação no momento: ela limpa os tokens AuthN e AuthZ do armazenamento (ou seja, no lado do Programador/Autenticação do Adobe Pass), mas **não** chame o ponto de extremidade de logout do MVPD.
