---
title: Benefícios do uso do parâmetro deviceType sem cliente nas métricas de autenticação do Adobe Pass
description: Benefícios do uso do parâmetro deviceType sem cliente nas métricas de autenticação do Adobe Pass
exl-id: a5004887-d5fa-468e-971b-10806519175b
source-git-commit: 913b2127d2189bec1a7e6e197944f1512b764893
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---

# (Herdado) Benefícios do uso do parâmetro deviceType sem cliente nas métricas de autenticação do Adobe Pass {#benefits-of-using-the-clientless-devicetype-parameter-in-primetime-authentication-metrics}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

</br>

## Contexto

Embora seja opcional, o parâmetro `deviceType` da API sem Cliente, quando presente, é usado nas métricas de Autenticação do Adobe Pass que estão sendo expostas por meio de [Monitoramento do Serviço de Qualificação](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md).

Considerando que a conexão entre o parâmetro `deviceType` e seus **benefícios** nas métricas de Autenticação do Adobe Pass não foi declarada inicialmente, o escopo desta nota técnica é adicionar mais informações sobre eles.

## Explicação

O parâmetro `deviceType` estava presente na API sem cliente desde a primeira versão, mas suas implicações nas métricas de Autenticação do Adobe Pass foram adicionadas em uma versão mais recente.



>[!IMPORTANT]
>
>Se o parâmetro `deviceType` estiver definido corretamente, ele terá o seguinte **benefício** no Monitoramento do Serviço de Qualificação: ele oferece métricas que são [analisadas por tipo de dispositivo](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md#clientless_device_type) ao usar Clientless, para que diferentes tipos de análise possam ser executados para, por exemplo, Roku, Apple TV, Xbox etc.


Para obter mais informações sobre a API de Monitoramento do Serviço de Qualificação, consulte a [árvore de detalhamento,](/help/premium-workflow/esm/entitlement-service-monitoring-overview.md#esm_dimensions) (recursos) disponível no ESM 2.0.

>[!NOTE]
>
>O conteúdo desta nota técnica também foi adicionado à [API sem cliente](#clientless_device_type).




## Implementação

Para se beneficiar totalmente das métricas de Autenticação do Adobe Pass, há dois tipos de [APIs sem cliente](#web_srvs_summary) que estão sendo usadas no momento e que precisam ter o `deviceType` correto definido:

1. As APIs que têm `regcode` como parâmetro obrigatório e usarão o parâmetro `deviceType` definido ao criar o `regcode`, com a seguinte chamada de API:
   - [\&lt;REGGIE\_FQDN\>/reggie/v1/](#reg_serv)

1. APIs que têm `deviceType` como parâmetro opcional:
   - [\&lt;SP\_FQDN\>/api/v1/checkauthn](#check_authn_token)
   - [&lt;span class=&quot;s1&quot;>](#retrieve_authn_token)
   - [\&lt;SP\_FQDN\>/api/v1/authorize](#init_authz)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/authz](#retrieve_authz_token)
   - [\&lt;SP\_FQDN\>/api/v1/tokens/media](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/mediatoken](#short_media)
   - [\&lt;SP\_FQDN\>/api/v1/preauthorize](#PreAuthZ_Resources)
   - [\&lt;SP\_FQDN\>/api/v1/logout](#init_logout)

Nossa recomendação é usar o parâmetro `deviceType` e transmitir o tipo de dispositivo sem cliente correto para todas as APIs.
