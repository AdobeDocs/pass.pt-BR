---
title: Visualização gratuita para aprovação temporária e aprovação temporária promocional
description: Visualização gratuita para aprovação temporária e aprovação temporária promocional
exl-id: c584bf0c-15c4-4a4d-b6a2-8d15ee786fe3
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 0%

---

# (Herdado) Visualização Gratuita para Aprovação Temporária e Aprovação Temporária Promocional {#free-preview-for-temp-pass-and-promotional-temp-pass}

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

## Descrição {#description}

Permite a criação de um token de autenticação para Aprovação Temporária e Aprovação Temporária Promocional sem a necessidade de uma segunda tela.


| Endpoint | Chamado </br>por | Entrada   </br>Parâmetros | HTTP </br>Método | Resposta | Resposta HTTP </br> |
|-------------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------|
| &lt;SP_FQDN>/api/v1/authenticate/freepreview | Aplicativo de Streaming</br></br>ou</br></br>Serviço de Programador | &#x200B;1. requestor_id (Obrigatório)</br>    </br>2.  deviceId (Obrigatório)</br>    </br>3.  mso_id (Obrigatório)</br>    </br>4.  nome_domínio (Obrigatório)</br>    </br>5.  device_info/X-Device-Info (Obrigatório)</br>6.  deviceType</br>    </br>7.  deviceUser (obsoleto)</br>    </br>8.  appId (obsoleto)</br>    </br>9.  generic_data (Opcional) | POST | A resposta bem-sucedida será 204 Sem conteúdo, indicando que o token foi criado com êxito e está pronto para uso para os fluxos de autorização. | 204 - Sem conteúdo   </br>400 - Solicitação inválida |

<div>


| Parâmetro de entrada | Descrição |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| requestor_id | O requestorId do Programador para o qual esta operação é válida. |
| deviceId | Os bytes de id do dispositivo. |
| mso_id | A MVPD Id para a qual esta operação é válida. |
| nome_do_domínio | O nome de domínio para o qual um token será concedido. Ele está sendo comparado com os domínios do provedor de serviços quando um token de autorização está sendo concedido. |
| device_info/</br></br>X-Device-Info | Informações do dispositivo de transmissão.</br></br>**Observação**: isso PODE ser passado para device_info como um parâmetro de URL, mas devido ao tamanho potencial desse parâmetro e às limitações no comprimento de uma URL GET, ELE DEVE ser passado como X-Device-Info no cabeçalho http. </br></br>Veja os detalhes completos em [Passando Informações sobre Dispositivo e Conexão](/help/authentication/integration-guide-programmers/legacy/client-information/passing-client-information-device-connection-and-application.md). |
| _deviceType_ | O tipo de dispositivo (por exemplo, Roku, PC).</br></br>Se este parâmetro estiver definido corretamente, o ESM oferecerá métricas que são [analisadas por tipo de dispositivo](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md#clientless_device_type) ao usar o sem cliente, para que diferentes tipos de análise possam ser executados para, por exemplo, Roku, Apple TV, Xbox etc.</br></br>Consulte [Vantagens de usar parâmetros de tipo de dispositivo sem cliente &#x200B;](/help/authentication/integration-guide-programmers/legacy/notes-technical/benefits-of-using-the-clientless-devicetype-parameter-in-pass-metrics.md)</br></br>**Observação**: device_info substituirá esse parâmetro. |
| _deviceUser_ | O identificador do usuário do dispositivo.</br></br>**Observação**: se usado, deviceUser deve ter os mesmos valores que na solicitação [Criar código de registro](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| _appId_ | O id/nome do aplicativo. </br></br>**Observação**: device_info substitui este parâmetro. Se usado, `appId` deve ter os mesmos valores que na solicitação [Criar código de registro](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/apis/registration-code-request.md). |
| generic_data | Usado para limitar o escopo do token para aprovação temporária promocional. |


### [Voltar à Referência da API REST](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)
