---
title: Utilização da Experience Cloud ID na autenticação da Adobe Pass
description: Utilização da Experience Cloud ID na autenticação da Adobe Pass
exl-id: 03354c01-5aad-4d81-beee-1c3834599134
source-git-commit: e4d243ebf293f3ecc38e532d77116c065a22ebd2
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 0%

---

# Utilização da Experience Cloud ID na autenticação da Adobe Pass

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

## O que é a Experience Cloud ID e como obtê-la? {#what-exp-cloud-id-obtain}

A Experience Cloud ID (ECID para abreviar) é uma ID exclusiva gerada pela Adobe Experience Cloud para cada usuário individual em seu aplicativo/site. A ECID é muito usada em todos os relatórios do Experience Cloud usados para vincular informações sobre um usuário específico em vários aplicativos/sites.

Se você já tiver um sistema em vigor que forneça uma ID de visitante, use a mesma ID para o escopo deste documento.

Uma maneira de obter a ECID é usar o Serviço da Experience Cloud ID. Você pode usar seu tipo de implementação preferido, com base no TDM, biblioteca JS, no lado do servidor, integração direta ou bibliotecas nativas para plataformas móveis. Para obter uma visão abrangente dos serviços, bibliotecas, guias de implementação e da SDK disponíveis, consulte: <https://experienceleague.adobe.com/docs/id-service/using/implementation/implementation-guides.html>

## Qual é o benefício de usar a Experience Cloud ID na autenticação da Adobe Pass? {#benefit-ex-cloud-id}

Se você configurar nossos SDKs e a API REST sem cliente para usar sua ECID, será possível vincular os dados coletados pela Autenticação Adobe Pass às soluções existentes da Experience Cloud. Isso permitirá que você entenda melhor a jornada e a experiência de seus clientes em todas as soluções fornecidas pela Adobe.

## Como usar a Experience Cloud ID na autenticação da Adobe Pass? {#how-to-ex-cloud-id-authn}

Depois de obter a ECID (explicada acima), é necessário transmitir essas informações para nossos SDKs e nossa API REST sem clientes. Essas informações serão posteriormente passadas para nossos servidores em cada chamada de rede feita pela SDK. O processo de configuração é diferente para cada SDK da seguinte maneira:

### JS SDK {#js-sdk}

Para o JavaScript, é necessário passar a ECID em um mapa como o terceiro parâmetro para a chamada setRequestor.

**Exemplo de uso:**

```JavaScript
accessEnabler.setRequestor("REQUESTOR_ID", ["ENDPOINT_URL"],
    {
        "visitorID": "THE_ECID_VALUE"
    }
);
```

### iOS/tvOS SDK {#ios-sdk}

Para o iOS/tvOS SDK, há um método dedicado chamado setOptions.

**Exemplo de uso:**

```JavaScript
accessEnabler.setOptions(
    [
        "visitorID": "THE_ECID_VALUE"
    ]
);
```

### Android/fireTV SDK {#android-sdk}

Para o Android/fireTV SDK, o mecanismo é semelhante ao iOS. Apenas o nome do parâmetro é diferente. A API está documentada aqui.

**Exemplo de uso:**

```JavaScript
String visitor_id = "THE_ECID_VALUE";

HashMap<String, String> options = new HashMap();
options.put("ap_vi",visitor_id);

accessEnabler.setOptions(options);
```

### API sem cliente {#clientless-api}

Ao usar o Adobe Pass por meio da REST API v1, o valor **ECID** deve ser enviado **em todas as APIs** como um parâmetro chamado **&#39;ap_vi&#39;**.

**Exemplo de uso:**

`GET: https://api.auth.adobe.com/api/v1/authorize?...&ap_vi=THE_ECID_VALUE`

### REST API V2 {#rest-api-v2}

Ao usar o Adobe Pass por meio da REST API v2, o valor **ECID** deve ser enviado **em todas as APIs** como um cabeçalho chamado **&#39;AP-Visitor-Identifier&#39;**.

**Exemplo de uso:**

`POST: https://api.auth.adobe.com/api/v2/${serviceProvider}/sessions/`\
Cabeçalhos:\
`AP-Visitor-Identifier: THE_ECID_VALUE`

