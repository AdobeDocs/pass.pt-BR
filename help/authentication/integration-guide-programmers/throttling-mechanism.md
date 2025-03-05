---
title: Mecanismo de limitação
description: Saiba mais sobre o mecanismo de limitação usado na autenticação do Adobe Pass. Explore uma visão geral desse mecanismo nesta página.
exl-id: f00f6c8e-2281-45f3-b592-5bbc004897f7
source-git-commit: 6b803eb0037e347d6ce147c565983c5a26de9978
workflow-type: tm+mt
source-wordcount: '1141'
ht-degree: 0%

---

# Mecanismo de limitação {#throttling-mechanism}

Todos os clientes do Pass Authentication precisam acessar a API do Pass Authentication para cada um de seus usuários, de acordo com as instruções e o business case.

A Autenticação de aprovação introduz um mecanismo de limitação para garantir a distribuição equitativa de recursos entre os usuários de nossos clientes.

Esse mecanismo é importante por alguns motivos:

- Uma das regras de ouro da arquitetura multilocatário é que o comportamento de um usuário não deve afetar outra pessoa.
- A limitação de taxa é importante para as APIs, pois é fácil cometer erros ao integrar com uma API. Como as APIs são programáticas, é relativamente fácil enviar acidentalmente mais solicitações do que o esperado.

## Visão geral do mecanismo {#mechanism-overview}

### Implementações do cliente

A Autenticação de aprovação fornece diretrizes e SDK para interagir com a API, mas não controla como o cliente a usa. Algumas implementações podem ter uma implementação rudimentar e podem inundar o serviço com solicitações desnecessárias de API, seja acidentalmente ou não, o que pode significar que outros usuários tenham lentidão ou problemas de capacidade.

O próprio serviço deve ser capaz de lidar com qualquer capacidade razoável. Mas não importa o desempenho ou a escalabilidade de um serviço, há sempre limites. Dessa forma, o serviço deve ter limites configurados para o número de chamadas aceitas em um intervalo de tempo específico.

### Introdução à limitação

A autenticação de aprovação é baseada na identificação do usuário e em um algoritmo de limitação da taxa do token bucket com valores predefinidos para controlar o acesso do dispositivo de cada usuário à nossa API.

### Mecanismo de identificação do dispositivo

O mecanismo de limitação proposto usa os dispositivos identificados individualmente, com a ajuda do cabeçalho &quot;X-Forwarded-For&quot;. Os limites serão aplicados da mesma forma para cada dispositivo.

### Atualizações necessárias

As implementações de servidor para servidor devem encaminhar os endereços IP do cliente usando o mecanismo de cabeçalho &quot;X-Forwarded-For&quot;.

Você pode encontrar mais detalhes sobre como passar o cabeçalho X-Forwarded-For [aqui](legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md).

### Limites e endpoints reais {#throttling-mechanism-limits}

Atualmente, o limite padrão permite no máximo 1 solicitação por segundo, com uma intermitência inicial de 10 solicitações (permissão única na primeira interação do cliente identificado, o que deve permitir que a inicialização seja concluída com êxito). Isso não deve afetar nenhum caso comercial normal em todos os nossos clientes.

O mecanismo de limitação será habilitado nos seguintes pontos de extremidade:

- /o/client/register
- /o/client/token
- /o/client/scopes
- /o/client/validate
- /api/v2/
- /api/v1/tokens/usermetadata
- /api/v1/tokens/authn
- /api/v1/tokens/authz
- /api/v1/tokens/media
- /api/v1/config/
- /api/v1/checkauthn
- /api/v1/logout
- /api/v1/authorize
- /api/v1/preauthorize
- /api/v1/mediatoken
- /api/v1/authenticate/freepreview
- /api/v1/authenticate/
- /api/v1/+/profile-requests/.+
- /api/v1/identities
- /adobe-services/config/
- /reggie/v1/+/regcode
- /reggie/v1/+/regcode/+

### Desambiguação da implementação do SDK

Como os clientes que usam a Autenticação Adobe Pass fornecidos pelos SDKs não estão interagindo explicitamente com nenhum endpoint, esta seção apresentará as funções conhecidas, como elas se comportam ao encontrar uma resposta de limitação e as ações que devem ser tomadas.

#### setRequestor

Ao atingir o limite usando a função `setRequestor` da SDK, o SDK retornará um código de erro CFG429 por meio do retorno de chamada `errorHandler`.

#### getAuthorization

Ao atingir o limite de aceleração usando a função `getAuthorization` da SDK, o SDK retornará um código de erro Z100 pela chamada de retorno `errorHandler`.

#### checkPreauthorizedResources

Ao atingir o limite de aceleração usando a função `checkPreauthorizedResources` da SDK, o SDK retornará um código de erro P100 pela chamada de retorno `errorHandler`.

#### getMetadata

Ao atingir o limite usando a função `getMetadata` da SDK, o SDK retornará uma resposta vazia por meio do retorno de chamada `setMetadataStatus`.

Para cada detalhe de implementação específico, consulte a documentação específica do SDK.

- [Referência da API do JavaScript SDK](legacy/sdks/javascript-sdk/javascript-sdk-api-reference.md)
- [Referência da API do Android SDK](legacy/sdks/android-sdk/android-sdk-api-reference.md)
- [Referência da API do iOS/tvOS](legacy/sdks/ios-tvos-sdk/iostvos-sdk-api-reference.md)

### Alterações na resposta da API e resposta {#throttling-mechanism-response}

Quando identificamos que o limite foi violado, marcaremos essa solicitação com um status de resposta específico (HTTP 429 Demasiadas solicitações), instruindo que você tenha consumido todos os tokens atribuídos ao dispositivo do usuário (endereço IP) para o intervalo de tempo.

A limitação expira após um segundo das primeiras 429 respostas. Cada aplicativo que recebe uma resposta 429 deve aguardar pelo menos 1 segundo antes de gerar uma nova solicitação.

Todos os aplicativos do cliente devem lidar adequadamente com a resposta &quot;429 muitas solicitações&quot;.

Esta é uma amostra de mensagem de resposta 429:

```
HTTP/2 429
date: Tue, 20 Feb 2024 11:21:53 GMT
content-type: text/html
content-length: 166
set-cookie: AWSALB=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/
set-cookie: AWSALBCORS=Btl/GzifUpMhUh+TQK63kU4i+gcJOIvAICVLnHTWt5pkrevNsMSQ5DMwM9KlRkNQ0UlXHIDbQoxDua0oVYYFKC8PDwxQjOuuRzxX2fozM+Jcazl2DSfaR7hU2mt2; Expires=Tue, 27 Feb 2024 11:21:53 GMT; Path=/; SameSite=None; Secure
server: openresty
access-control-allow-credentials: true
access-control-allow-methods: POST,GET,OPTIONS,DELETE
access-control-allow-headers: ap_11,ap_42,ap_z,ap_19,ap_21,ap_23,authorization,content-type,pass_sfp,AP-Session-Identifier,AP-Device-Identifier,AP-SDK-Identifier,X-Device-Info
access-control-expose-headers: pass_sfp,Authzf-Error-Code,Authzf-Sub-Error-Code,Authzf-Error-Details
p3p: CP="NOI DSP COR CURa ADMa DEVa OUR BUS IND UNI COM NAV STA"

<html>
<head><title>429 Too Many Requests</title></head>
<body>
<center><h1>429 Too Many Requests</h1></center>
<hr><center>openresty</center>
</body>
</html>
```

## Impacto e alterações necessárias

### Passando o cabeçalho X-Forwarded-For

Os clientes que usam uma implementação personalizada (incluindo servidores para servidores) para interagir com a API de autenticação de passagem devem garantir que possam capturar o endereço IP do usuário e encaminhá-lo corretamente, usando o cabeçalho X-Forwarded-For além da API de autenticação de passagem.

Consulte [aqui](legacy/rest-api-v1/cookbooks/rest-api-cookbook-servertoserver.md) para obter mais detalhes.

### Reação a novo código de resposta

Os clientes que usam uma implementação personalizada (incluindo as de servidor para servidor) para interagir com a API de autenticação de passagem devem garantir que qualquer chamada subsequente feita após receber um número excessivo de solicitações 429 inclua um período de espera mínimo de 1 segundo. Esse período de espera garante uma oportunidade de alterar esse mecanismo e obter uma resposta comercial válida.

## Exemplo de cenário para limitação

| Tempo desde a primeira solicitação | Resposta recebida | Explicação |
|--------------------------|-----------------------------------|-----------------------------------------------------------------------------------------------------------|
| Segundo 0 | A chamada recebe o código de status de sucesso | 1 chamadas consumidas do limite |
| Segundo 0,3 | A chamada recebe o código de status de sucesso | 1 chamada consumida do limite e 1 chamada marcada como intermitente |
| Segundo 0,6 | A chamada recebe o código de status de sucesso | 1 chamada consumida do limite e 2 chamadas marcadas como intermitência |
| Segundo 0,9 | A chamada recebe o código de status de sucesso | 1 chamada consumida do limite e 3 chamadas marcadas como intermitência |
| Segundo 1.2 | A chamada recebe o código de status de sucesso | 2 chamadas consumidas do limite e 3 chamadas marcadas como intermitência |
| Segundo 1.3 | A chamada recebe o código de status de sucesso | 2 chamadas consumidas do limite e 4 chamadas marcadas como intermitência |
| Segundo 1.4 | A chamada recebe o código de status de sucesso | 2 chamadas consumidas do limite e 5 chamadas marcadas como intermitência |
| Segundo 1,5 | A chamada recebe o código de status de sucesso | 2 chamadas consumidas do limite e 6 chamadas marcadas como intermitência |
| Segundo 1.6 | A chamada recebe o código de status de sucesso | 2 chamadas consumidas do limite e 7 chamadas marcadas como intermitência |
| Segundo 1,7 | A chamada recebe o código de status de sucesso | 2 chamadas consumidas do limite e 8 chamadas marcadas como intermitência |
| Segundo 1.8 | A chamada recebe o código de status de sucesso | 2 chamadas consumidas do limite e 9 chamadas marcadas como intermitência |
| Segundo 2.1 | A chamada recebe o código de status de sucesso | 3 chamadas consumidas do limite e 9 chamadas marcadas como intermitência |
| Segundo 2.2 | A chamada recebe o código de status de sucesso | 3 chamadas consumidas do limite e 10 chamadas marcadas como intermitência |
| Segundo 2.4 | A chamada recebe o código de status 429 | 3 chamadas consumidas do limite e 10 chamadas marcadas como intermitentes, e 1 chamada recebe &quot;429 muitas solicitações&quot; |
| Segundo 2.6 | A chamada recebe o código de status 429 | 3 chamadas consumidas do limite e 10 chamadas marcadas como intermitentes, e 2 chamadas recebem &quot;429 muitas solicitações&quot; |
| Segundo 2.8 | A chamada recebe o código de status 429 | 3 chamadas consumidas do limite e 10 chamadas marcadas como intermitentes, e 3 chamadas recebem &quot;429 muitas solicitações&quot; |
| Segundo 3.1 | A chamada recebe o código de status de sucesso | 4 chamadas consumidas do limite e 10 chamadas marcadas como intermitentes, e 3 chamadas recebem ‘429 muitas solicitações’ |
