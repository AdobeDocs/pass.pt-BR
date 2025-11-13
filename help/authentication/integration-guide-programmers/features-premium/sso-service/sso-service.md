---
title: Serviço de logon único da Adobe
description: Saiba mais sobre o Serviço SSO da Adobe Pass que permite autenticação contínua em vários dispositivos e aplicativos.
exl-id: a1ff85d4-f7d2-4dea-b82f-d29730d9012f
source-git-commit: 151c64276377be5ef21bca4c0d3eaa04ac3da495
workflow-type: tm+mt
source-wordcount: '3615'
ht-degree: 2%

---


# Serviço de logon único da Adobe {#sso-service}

Este documento descreve casos de uso, endpoints e API do serviço de logon único da Adobe.

**Revisão atual - 1.0.0**

## Escopo {#scope}

O Adobe Pass SSO Service permite autenticação contínua em vários dispositivos e aplicativos, fornecendo uma experiência unificada ao usuário e mantendo os padrões de segurança e conformidade. Este serviço atende à necessidade cada vez maior de autenticação entre plataformas no cenário atual de transmissão em vários dispositivos.

## Visão geral {#overview}

### O que é {#what-it-is}

Uma solução abrangente de Logon único que permite aos usuários autenticar uma vez e gerenciar a transferência de autenticação de maneira informada para outros dispositivos

### Desafios atuais de autenticação {#current-challenges}

* O usuário deve se autenticar em cada serviço de streaming ao qual tiver uma assinatura
* O usuário deve se autenticar separadamente em cada dispositivo ou aplicativo
* Devido à dificuldade de digitar uma senha no fluxo de autenticação em algumas plataformas, isso pode resultar em maiores taxas de abandono

### Oportunidade para um serviço Adobe Pass SSO {#opportunity}

* Aumento do número de serviços de streaming que exigem sua própria autenticação
* Demanda crescente por experiências ininterruptas entre dispositivos
* Aumento do número de dispositivos de transmissão por residência
* Necessidade de autenticação unificada por residência

### Benefícios para os negócios {#business-benefits}

#### Provedores de conteúdo {#content-providers}

* **Maior engajamento do usuário** - A experiência perfeita leva a tempos de sessão mais longos
* **Atrito Reduzido** - Barreiras de autenticação mais baixas aumentam o consumo de conteúdo
* **Retenção aprimorada** - A melhor experiência do usuário reduz a rotatividade
* **Redução de custos** - Menos tíquetes de suporte relacionados a problemas de autenticação

#### Usuários finais {#end-users}

* **Experiência contínua** - Autentique uma vez, acesse em qualquer lugar
* **Economia de tempo** - Nenhum processo de logon repetido
* **Flexibilidade de Dispositivo** - Alternar entre dispositivos sem interrupção
* **Experiência consistente** - Autenticação uniforme em todas as plataformas

#### IdPs (MVPDs, Telcos etc.) {#idps}

* Os MVPDs podem ser informados de dispositivos adicionais usando um SSO autenticado
* **Satisfação do Usuário Aprimorada** - Experiência de autenticação aprimorada
* **Carga de suporte reduzida** - Menos chamadas de suporte relacionadas à autenticação
* **Vantagem competitiva** - Melhor experiência do usuário que a concorrência

## Casos de uso {#use-cases}

### Mapeamento de identidade {#identity-mapping}

O serviço cria a capacidade de vincular um D2C e uma conta TVE no mesmo aplicativo.

Mais serviços de transmissão estão criando pacotes para serem vendidos a terceiros (MVPD/MVPD virtual/Telco etc.). Os usuários podem acabar tendo que lidar com várias contas no mesmo aplicativo. Para criar uma experiência de autenticação contínua, um serviço precisa conectar essas contas para simplificar a experiência de logon.

O usuário fará a autenticação com sua conta D2C e, em seguida, fará a autenticação UMA VEZ com uma conta diferente (por exemplo, MVPD). Com nosso serviço, essas contas serão vinculadas, o que significa que, a partir de agora, as autenticações subsequentes em diferentes dispositivos poderão ser feitas somente com a conta D2C.

### SSO entre dispositivos {#cross-device-sso}

A autenticação em um dispositivo conectado à TV pode ser mais complicada do que em um telefone celular. Uma boa experiência do usuário seria autenticar no telefone e, em seguida, transmitir essa autenticação para a TV inteligente.

## Componentes-chave {#key-components}

* **API de Token de Serviço** - componente chave que gerencia com segurança o mecanismo de Logon Único
* **API de Lista** - O aplicativo pode ajudar o usuário a entender a lista de dispositivos em seu ecossistema
* **API de link** - Aplicativo que permite ao usuário adicionar outros dispositivos em seu ecossistema
* **Desvincular API** - Aplicativo habilita o usuário a remover dispositivos em seu ecossistema

## Casos de uso detalhados {#use-cases-detailed}

### D2C-TVE SSO {#d2c-tve-sso}

Esse caso de uso permite a vinculação entre credenciais D2C e TVE(MVPD) em um dispositivo e a utilização desse perfil vinculado em outros dispositivos no mesmo aplicativo.

![Fluxo SSO D2C-TVE](../../../assets/sso_service_d2c_1.png)

![Fluxo de SSO entre Dispositivos](../../../assets/sso_service_d2c_2.png)

### SSO entre dispositivos {#cross-device-sso-detailed}

Esse caso de uso permite que os usuários já autenticados em um dispositivo reutilizem o perfil autenticado no mesmo aplicativo D2C ou TVE instalado em outros dispositivos (aplicativo necessário para implementar o Adobe Pass REST V2 para autenticação e autorização).

## Como integrar o Adobe SSO Service em um aplicativo D2C-TVE {#integration}

### Etapa 1 - Obter um identificador comum {#step-1}

Para integrar o Adobe SSO Service, uma implementação de aplicativo deve estabelecer uma ID exclusiva e persistente para usar como atributo de identificador comum SSO em X-SSO-ID. Isso pode ser obtido ao autenticar um usuário com um serviço D2C e reter um atributo sobre essa autenticação.

### Etapa 2 - Obter um token de serviço {#step-2}

O uso do identificador comum no X-SSO-ID no endpoint POST /serviceToken recuperará um JWT assinado com a seguinte carga:

```json
{
  "iss": "ssoservicetoken",
  "sub": "unique_common_identifier",
  "nbf": 1758093558,
  "exp": 1758097158,
  "iat": 1758093558
}
```

O token de serviço tem &quot;iat&quot; - emitido em e &quot;exp&quot; - hora de expiração em que o token de serviço é válido. Em caso de expiração, um novo JWT pode ser obtido usando o endpoint GET /serviceToken com o token de serviço expirado.

### Etapa 3 - Autenticar usando a API REST V2 do Adobe Pass com um TVE MVPD {#step-3}

A autenticação com o Adobe Pass deve ser implementada usando o Token de Serviço: [REST API V2 - Fluxos do Token de Serviço de Logon Único](https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-flows/rest-api-v2-single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows)

### Etapa 4 - Vincular outro dispositivo {#step-4}

No aplicativo autenticado, é possível criar um link para ser usado em um dispositivo diferente usando a API /link

```json
{
    "status": "CREATED",
    "code": "228128",
    "notBefore": 1758094617220,
    "notAfter": 1758098217220
}
```

O &quot;código&quot; é um código de link de vida curta, em forma de uma sequência de seis dígitos, que o usuário introduzirá em um aplicativo não autenticado em um dispositivo secundário

### Etapa 5 - Recuperar autenticação do Logon Único {#step-5}

Em um dispositivo diferente, uma vez que o usuário tenha introduzido o código, o aplicativo poderá:

* recuperar identidade do serviço D2C
* recuperar perfil do MVPD do Adobe Pass com REST V2 API

O perfil do MVPD será válido por meio do SSO para o período em que a autenticação inicial foi obtida.

Caso o MVPD invalide o perfil ou o usuário opte por fazer logoff do MVPD, o aplicativo usando a API REST V2 do Adobe Pass não terá mais um registro de perfil e deverá exigir que o usuário se autentique novamente com a MVPD.

### Etapa 6 - Gerenciar Logon Único em outros dispositivos {#step-6}

Um aplicativo pode obter informações sobre todos os outros dispositivos vinculados ao mesmo identificador comum usando a API /list.

A qualquer momento, um dispositivo pode ser removido do Logon único desvinculando esse dispositivo com a API /unlink.

## APIs {#apis}

### API do token de serviço {#service-token-api}

#### Descrição {#service-token-description}

A API do token de serviço pode ser usada para solicitar e gerenciar tokens de serviço que habilitam a funcionalidade de Logon único (SSO) entre vários aplicativos ou dispositivos. Esses tokens de serviço identificam perfis autenticados (perfis de SSO) e são essenciais para estabelecer e manter conexões SSO.

>[!WARNING]
>
>Os tokens de serviço contêm informações de autenticação confidenciais. Os aplicativos devem manipular esses tokens com segurança e nunca expô-los a pessoas não confiáveis.

A API do token de serviço fornece dois endpoints principais:

* **POST /api/{serviceProvider}/serviceToken** - Obter um token de serviço JWS recém-criado
* **GET /api/{serviceProvider}/serviceToken** - Atualizar um token de serviço JWS existente

Se a solicitação da API do token de serviço não puder ser atendida devido a um erro dos serviços de autenticação da Adobe Pass, informações adicionais sobre o erro serão incluídas como parte da resposta da API.

#### POST - serviceToken {#post-service-token}

##### Solicitação {#post-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">caminho</td>
      <td>/api/{serviceProvider}/serviceToken</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parâmetros de caminho</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>O identificador do provedor de serviços para o qual o token está sendo solicitado.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorização</td>
      <td>A geração da carga do token do portador está descrita na documentação do cabeçalho <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Autorização</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Identificador de dispositivo AP</td>
      <td>
         A geração da carga do identificador de dispositivo está descrita na documentação do cabeçalho <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.
         <br/><br/>
         Esse identificador é usado como o identificador SSO padrão quando X-SSO-ID não é fornecido.
      </td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         As informações do dispositivo conforme especificado na documentação do cabeçalho <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-x-device-info">X-Device-Info</a>.
         <br/><br/>
         <b>É altamente recomendável</b> usar quando a plataforma do dispositivo do aplicativo permitir que sejam fornecidos valores válidos explicitamente.
         <br/><br/>
         O back-end da Autenticação do Adobe Pass mesclará valores definidos explicitamente com valores extraídos implicitamente. Quando não fornecido, os valores extraídos padrão serão usados.
      </td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO-LINK</td>
      <td>
         O código de link que associa essa solicitação a um perfil autenticado existente. Quando fornecida, a resposta incluirá um token de serviço para SSO com o perfil que gerou o código do link.
         <br/><br/>
         Normalmente, isso é usado quando um aplicativo ou dispositivo secundário deseja se conectar a um perfil autenticado de um aplicativo ou dispositivo principal.
      </td>
      <td>obrigatório se x-sso-id não for fornecido</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-SSO-ID</td>
      <td>
         O identificador comum no qual o aplicativo solicita basear o SSO.
         <br/><br/>
         Quando fornecido, esse identificador será usado para estabelecer um perfil de SSO comum entre dispositivos e/ou aplicativos.
      </td>
      <td>obrigatório se x-sso-link não for fornecido</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Aceitar</td>
      <td>
         O tipo de mídia aceito pelo aplicativo cliente.
         <br/><br/>
         Se especificado, deve ser application/json.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>O agente do usuário do aplicativo cliente.</td>
      <td>opcional</td>
   </tr>
</table>

##### Resposta {#post-service-token-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Código</th>
      <th style="background-color: #EFF2F7;">Texto</th>
      <th style="background-color: #EFF2F7;">Descrição</th>
   </tr>
   <tr>
      <td>201</td>
      <td>Criado em</td>
      <td>
        O token de serviço foi gerado e retornado com êxito no corpo da resposta.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Solicitação inválida</td>
      <td>
        A solicitação é inválida, o cliente precisa corrigir a solicitação e tentar novamente. O corpo da resposta pode conter informações de erro que seguem a documentação de <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de erro aprimorados</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Não autorizado</td>
      <td>
        O token de acesso é inválido, o cliente precisa obter um novo token de acesso e tentar novamente. Para obter mais detalhes, consulte a documentação <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Visão geral do registro dinâmico do cliente</a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Erro interno do servidor</td>
      <td>
        O servidor encontrou um problema. O corpo da resposta pode conter informações de erro que seguem a documentação de <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de erro aprimorados</a>.
      </td>
   </tr>
</table>

###### Sucesso {#success-post-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>201</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Tipo de conteúdo</td>
      <td>application/json</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">status</td>
      <td>Status HTTP (por exemplo, "CRIADO")</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>Uma Assinatura Web JSON (JWS) codificada na Base64 contendo o token de serviço. Esse token pode ser usado em chamadas de API subsequentes para identificar o perfil autenticado e habilitar a funcionalidade SSO.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Época ms ou 0 no erro</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>Época ms ou 0 no erro</td>
      <td><i>obrigatório</i></td>
   </tr>
</table>

###### Erro {#error-post-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 500</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Tipo de conteúdo</td>
      <td>application/json</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>O corpo da resposta pode fornecer informações adicionais de erro que seguem a documentação de <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de erro aprimorados</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
</table>

## Amostras {#samples-post-service-token}

### &#x200B;1. Solicitar um novo token de serviço (com ID de SSO)

>[!BEGINTABS]

>[!TAB Solicitação]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-ID: <sso_id>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    Accept: application/json
```

>[!TAB Resposta]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### &#x200B;2. Solicitar um novo token de serviço (com Link SSO)

>[!BEGINTABS]

>[!TAB Solicitação]

```HTTPS
POST /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    X-SSO-LINK: <link_code>
    AP-Device-Identifier: fingerprint <base64_device_id>
    X-Device-Info: <base64_device_info>
    User-Agent: <user_agent>
    Accept: application/json
```

>[!TAB Resposta]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{
  "status": "CREATED",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

#### GET - serviceToken {#get-service-token}

##### Solicitação {#get-service-token-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">caminho</td>
      <td>/api/{serviceProvider}/serviceToken</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parâmetros de caminho</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>O identificador do provedor de serviços para o qual o token está sendo solicitado.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorização</td>
      <td>A geração da carga do token do portador está descrita na documentação do cabeçalho <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Autorização</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         Um token de serviço obtido anteriormente que precisa ser atualizado.
         <br/><br/>
         Este token deve ser válido ou ter expirado recentemente para se qualificar para atualização.
      </td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Aceitar</td>
      <td>
         O tipo de mídia aceito pelo aplicativo cliente.
         <br/><br/>
         Se especificado, deve ser application/json.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>O agente do usuário do aplicativo cliente.</td>
      <td>opcional</td>
   </tr>
</table>

##### Resposta {#get-service-token-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Código</th>
      <th style="background-color: #EFF2F7;">Texto</th>
      <th style="background-color: #EFF2F7;">Descrição</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        O token de serviço foi atualizado com êxito e retornado no corpo da resposta.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Solicitação inválida</td>
      <td>
        A solicitação é inválida, o cliente precisa corrigir a solicitação e tentar novamente. O corpo da resposta pode conter informações de erro que seguem a documentação de <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de erro aprimorados</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Não autorizado</td>
      <td>
        O token de acesso ou o token de serviço é inválido. O cliente precisa obter um novo token de acesso ou token de serviço e tentar novamente. Para obter mais detalhes, consulte a documentação <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Visão geral do registro dinâmico do cliente</a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Erro interno do servidor</td>
      <td>
        O servidor encontrou um problema. O corpo da resposta pode conter informações de erro que seguem a documentação de <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de erro aprimorados</a>.
      </td>
   </tr>
</table>

###### Sucesso {#success-get-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>200</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Tipo de conteúdo</td>
      <td>application/json</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">status</td>
      <td>Status HTTP (por exemplo, "OK")</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">jws</td>
      <td>Uma Assinatura Web JSON (JWS) codificada na Base64 contendo o token de serviço atualizado. Esse token pode ser usado em chamadas de API subsequentes para identificar o perfil autenticado e habilitar a funcionalidade SSO.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>Época ms ou 0 no erro</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>Época ms ou 0 no erro</td>
      <td><i>obrigatório</i></td>
   </tr>
</table>

###### Erro {#error-get-service-token}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 500</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Tipo de conteúdo</td>
      <td>application/json</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>O corpo da resposta pode fornecer informações adicionais de erro que seguem a documentação de <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de erro aprimorados</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
</table>

## Amostras {#samples-get-service-token}

### &#x200B;1. Solicitação para atualizar um token de serviço

>[!BEGINTABS]

>[!TAB Solicitação]

```HTTPS
GET /api/{serviceProvider}/serviceToken HTTP/1.1

    Authorization: Bearer <access_token>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Resposta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "notBefore": 1710000000000,
  "notAfter": 1710003600000
}
```

>[!ENDTABS]

### Vincular API {#link-api}

#### Descrição {#link-description}

A API de link pode ser usada para solicitar um código de link (ou código QR) que pode habilitar o Logon único (SSO) entre vários aplicativos ou dispositivos. Esse código de link permite que os usuários conectem um novo aplicativo ou dispositivo a um perfil autenticado (perfil SSO) existente, fornecendo uma experiência SSO contínua em aplicativos ou dispositivos.

A API de link exige que um token de serviço válido seja fornecido no cabeçalho AD-Service-Token.

O código de link gerado é normalmente exibido aos usuários em um aplicativo ou dispositivo principal e inserido em um aplicativo ou dispositivo secundário para estabelecer a conexão com o SSO. O código do link tem um período de validade limitado (normalmente de 5 a 30 minutos) e deve ser usado uma vez.

Se a solicitação da API de link não puder ser atendida devido a um erro nos serviços de autenticação da Adobe Pass, informações adicionais sobre o erro serão incluídas como parte do resultado da resposta da API de link.

#### POST - link {#post-link}

##### Solicitação {#post-link-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">caminho</td>
      <td>/api/{serviceProvider}/link</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parâmetros de caminho</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>O identificador do provedor de serviços para o qual o token está sendo solicitado.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorização</td>
      <td>A geração da carga do token do portador está descrita na documentação do cabeçalho <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Autorização</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Identificador de dispositivo AP</td>
      <td>A geração da carga do identificador de dispositivo está descrita na documentação do cabeçalho <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         A geração do token de serviço é descrita pela documentação da API do token de serviço.
         <br/><br/>
         Este token de serviço identifica o perfil autenticado para o qual o código do link será gerado.
      </td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Aceitar</td>
      <td>
         O tipo de mídia aceito pelo aplicativo cliente.
         <br/><br/>
         Se especificado, deve ser application/json.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>O agente do usuário do aplicativo cliente.</td>
      <td>opcional</td>
   </tr>
</table>

##### Resposta {#post-link-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Código</th>
      <th style="background-color: #EFF2F7;">Texto</th>
      <th style="background-color: #EFF2F7;">Descrição</th>
   </tr>
   <tr>
      <td>201</td>
      <td>Criado em</td>
      <td>
        O código do link foi gerado e retornado com êxito no corpo da resposta.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Solicitação inválida</td>
      <td>
        A solicitação é inválida, o cliente precisa corrigir a solicitação e tentar novamente. O corpo da resposta pode conter informações de erro que seguem a documentação de <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de erro aprimorados</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Não autorizado</td>
      <td>
        O token de acesso é inválido, o cliente precisa obter um novo token de acesso e tentar novamente. Para obter mais detalhes, consulte a documentação <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Visão geral do registro dinâmico do cliente</a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Erro interno do servidor</td>
      <td>
        O servidor encontrou um problema. O corpo da resposta pode conter informações de erro que seguem a documentação de <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de erro aprimorados</a>.
      </td>
   </tr>
</table>

###### Sucesso {#success-post-link}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>201</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Tipo de conteúdo</td>
      <td>application/json</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">status</td>
      <td>Status HTTP (por exemplo, "CRIADO")</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">link</td>
      <td>Um código numérico ou alfanumérico curto que pode ser usado em um aplicativo ou dispositivo secundário para estabelecer uma conexão SSO.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notBefore</td>
      <td>O carimbo de data e hora (em milissegundos desde a época) quando o código do link se torna válido.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">notAfter</td>
      <td>O carimbo de data e hora (em milissegundos desde a época) quando o código do link expira.</td>
      <td><i>obrigatório</i></td>
   </tr>
</table>

###### Erro {#error-post-link}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 500</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Tipo de conteúdo</td>
      <td>application/json</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>O corpo da resposta pode fornecer informações adicionais de erro que seguem a documentação de <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de erro aprimorados</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
</table>

## Amostras {#samples-post-link}

### &#x200B;1. Solicitar um código de link para um perfil autenticado existente

>[!BEGINTABS]

>[!TAB Solicitação]

```HTTPS
POST /api/{serviceProvider}/link HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Resposta]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json

{            
   "status": "CREATED",
   "code": "123456",
   "notBefore": 1623840000000,
   "notAfter": 1623842700000
}
```

>[!ENDTABS]

### Desvincular API {#unlink-api}

#### Descrição {#unlink-description}

A API de desvinculação pode ser usada para solicitar a remoção de um ou vários dispositivos de um perfil autenticado (perfil SSO). Essa API permite que os usuários desconectem dispositivos de sua configuração de SSO, fornecendo controle sobre quais dispositivos têm acesso a seus perfis autenticados.

>[!WARNING]
>
>A API Unlink requer que um token de serviço válido seja fornecido no cabeçalho AD-Service-Token.

Se a solicitação Unlink API não puder ser atendida devido a um erro nos serviços de autenticação da Adobe Pass, informações adicionais sobre o erro serão incluídas como parte do resultado da resposta Unlink API.

#### POST - desvincular {#post-unlink}

##### Solicitação {#post-unlink-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">caminho</td>
      <td>/api/{serviceProvider}/unlink</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parâmetros de caminho</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>O identificador do provedor de serviços.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parâmetros de corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">dispositivos</td>
      <td>
         Matriz de identificadores de dispositivos a serem desvinculados.
         <br/><br/>
         Exemplo:<br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
      </td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorização</td>
      <td>A geração da carga do token do portador está descrita na documentação do cabeçalho <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Autorização</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Tipo de conteúdo</td>
      <td>
         O tipo de mídia aceito para os recursos que estão sendo enviados.
         <br/><br/>
         Deve ser application/json.
      </td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Identificador de dispositivo AP</td>
      <td>A geração da carga do identificador de dispositivo está descrita na documentação do cabeçalho <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         A geração do token de serviço é descrita pela documentação da API do token de serviço.
         <br/><br/>
         Este token de serviço identifica o perfil autenticado para o qual os dispositivos serão desvinculados.
      </td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Aceitar</td>
      <td>
         O tipo de mídia aceito pelo aplicativo cliente.
         <br/><br/>
         Se especificado, deve ser application/json.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>O agente do usuário do aplicativo cliente.</td>
      <td>opcional</td>
   </tr>
</table>

##### Resposta {#post-unlink-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Código</th>
      <th style="background-color: #EFF2F7;">Texto</th>
      <th style="background-color: #EFF2F7;">Descrição</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        Os dispositivos solicitados foram desvinculados com êxito da instalação de SSO.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Solicitação inválida</td>
      <td>
        A solicitação é inválida, o cliente precisa corrigir a solicitação e tentar novamente. O corpo da resposta pode conter informações de erro que seguem a documentação de <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de erro aprimorados</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Não autorizado</td>
      <td>
        O token de acesso é inválido, o cliente precisa obter um novo token de acesso e tentar novamente. Para obter mais detalhes, consulte a documentação <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Visão geral do registro dinâmico do cliente</a>.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Método não permitido</td>
      <td>
        O método HTTP é inválido, o cliente precisa usar um método HTTP permitido para o recurso solicitado e tentar novamente.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Erro interno do servidor</td>
      <td>
        O servidor encontrou um problema. O corpo da resposta pode conter informações de erro que seguem a documentação de <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de erro aprimorados</a>.
      </td>
   </tr>
</table>

###### Sucesso {#success-post-unlink}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>200</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Tipo de conteúdo</td>
      <td>application/json</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">status</td>
      <td>Informações sobre o resultado da operação: "OK"</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">unlinkedDevices</td>
      <td>
         Lista de dispositivos desvinculados com sucesso.
         <br/><br/>
         Exemplo:<br/><code>["deviceid1", "deviceid2", "deviceid3"]</code>
      </td>
      <td><i>obrigatório</i></td>
   </tr>
</table>

###### Erro {#error-post-unlink}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 405, 500</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Tipo de conteúdo</td>
      <td>application/json</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>O corpo da resposta pode fornecer informações adicionais de erro que seguem a documentação de <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de erro aprimorados</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
</table>

## Amostras {#samples-post-unlink}

### &#x200B;1. Solicitação para desvincular dispositivos de um perfil de SSO

>[!BEGINTABS]

>[!TAB Solicitação]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!TAB Resposta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid2",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### &#x200B;2. Solicitação para desvincular dispositivos com sucesso parcial

>[!BEGINTABS]

>[!TAB Solicitação]

```HTTPS
POST /api/{serviceProvider}/unlink HTTP/1.1

    Authorization: Bearer <access_token>
    Content-Type: application/json
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
    
{
  "devices": [
    "deviceid1",
    "unknowndevice",
    "deviceid3"
  ]
}
```

>[!TAB Resposta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "status": "OK",
  "unlinkedDevices": [
    "deviceid1",
    "deviceid3"
  ]
}
```

>[!ENDTABS]

### Listar API {#list-api}

#### Descrição {#list-description}

A API de lista pode ser usada para obter uma lista de dispositivos conectados atualmente a um perfil autenticado (perfil SSO). Essa API permite que usuários e aplicativos visualizem quais dispositivos fazem parte de sua configuração de SSO, fornecendo visibilidade e recursos de gerenciamento para sua experiência autenticada em vários dispositivos.

>[!WARNING]
>
>A API de Lista exige que um token de serviço válido seja fornecido no cabeçalho AD-Service-Token.

A API de lista retorna detalhes sobre cada dispositivo no perfil autenticado (perfil SSO), incluindo identificadores de dispositivos e metadados que podem ajudar os usuários a reconhecer seus dispositivos. Essas informações podem ser usadas para ajudar os usuários a tomar decisões informadas sobre quais dispositivos devem permanecer em sua configuração de SSO.

Se a solicitação da API de lista não puder ser atendida devido a um erro nos serviços de autenticação da Adobe Pass, informações adicionais sobre o erro serão incluídas como parte do resultado da resposta da API de lista.

#### GET - Lista {#get-list}

##### Solicitação {#get-list-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">caminho</td>
      <td>/api/{serviceProvider}/list</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>GET</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parâmetros de caminho</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>O identificador do provedor de serviços.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorização</td>
      <td>A geração da carga do token do portador está descrita na documentação do cabeçalho <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-authorization">Autorização</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Identificador de dispositivo AP</td>
      <td>A geração da carga do identificador de dispositivo está descrita na documentação do cabeçalho <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
         A geração do token de serviço é descrita pela documentação da API do token de serviço.
         <br/><br/>
         Este token de serviço identifica o perfil autenticado para o qual a lista de dispositivos será recuperada.
      </td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Aceitar</td>
      <td>
         O tipo de mídia aceito pelo aplicativo cliente.
         <br/><br/>
         Se especificado, deve ser application/json.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>O agente do usuário do aplicativo cliente.</td>
      <td>opcional</td>
   </tr>
</table>

##### Resposta {#get-list-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Código</th>
      <th style="background-color: #EFF2F7;">Texto</th>
      <th style="background-color: #EFF2F7;">Descrição</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        A lista de dispositivos na configuração do SSO foi recuperada e retornada com êxito no corpo da resposta.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Solicitação inválida</td>
      <td>
        A solicitação é inválida, o cliente precisa corrigir a solicitação e tentar novamente. O corpo da resposta pode conter informações de erro que seguem a documentação de <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de erro aprimorados</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Não autorizado</td>
      <td>
        O token de acesso é inválido, o cliente precisa obter um novo token de acesso e tentar novamente. Para obter mais detalhes, consulte a documentação <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview">Visão geral do registro dinâmico do cliente</a>.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Método não permitido</td>
      <td>
        O método HTTP é inválido, o cliente precisa usar um método HTTP permitido para o recurso solicitado e tentar novamente.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Erro interno do servidor</td>
      <td>
        O servidor encontrou um problema. O corpo da resposta pode conter informações de erro que seguem a documentação de <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de erro aprimorados</a>.
      </td>
   </tr>
</table>

###### Sucesso {#success-get-list}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>200</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Tipo de conteúdo</td>
      <td>application/json</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">dispositivos</td>
      <td>
         JSON que contém um mapa de pares de chaves e valores.
         <br/><br/>
         <b>Chave:</b> deviceId - A carga do identificador do dispositivo conforme descrito na documentação do cabeçalho <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-appendix/rest-api-v2-appendix-headers/rest-api-v2-appendix-headers-ap-device-identifier">AP-Device-Identifier</a>
         <br/><br/>
         <b>Valor:</b> atributos - JSON que contém um mapa de atributos de metadados de dispositivo, incluindo:
         <ul>
            <li>tipo de dispositivo</li>
            <li>platform</li>
            <li>agente do usuário</li>
            <li>outros metadados relevantes que ajudam a identificar o dispositivo</li>
         </ul>
         Os valores dos atributos podem ser simples (string, número inteiro, booleano etc.)
      </td>
      <td><i>obrigatório</i></td>
   </tr>
</table>

###### Erro {#error-get-list}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 405, 500</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Tipo de conteúdo</td>
      <td>application/json</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>O corpo da resposta pode fornecer informações adicionais de erro que seguem a documentação de <a href="https://experienceleague.adobe.com/pt-br/docs/pass/authentication/integration-guide-programmers/standard-features/error-reporting/enhanced-error-codes">Códigos de erro aprimorados</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
</table>

## Amostras {#samples-get-list}

### &#x200B;1. Solicitação para listar dispositivos em um perfil SSO

>[!BEGINTABS]

>[!TAB Solicitação]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Resposta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {
    "deviceid1": {
      "deviceType": "smartTV",
      "model": "Samsung",
      "os": "Tizen",
      "osVersion": "5.0",
      "lastSeen": 1623840000000,
      "type": "regular"
    },
    "deviceid2": {
      "deviceType": "mobile",
      "model": "iPhone",
      "os": "iOS",
      "osVersion": "14.5",
      "lastSeen": 1623830000000,
      "type": "sso"
    },
    "deviceid3": {
      "deviceType": "tablet",
      "model": "iPad",
      "os": "iPadOS",
      "osVersion": "14.5",
      "lastSeen": 1623820000000,
      "type": "sso"
    }
  }
}
```

>[!ENDTABS]

### &#x200B;2. Solicitação para listar dispositivos sem dispositivos vinculados

>[!BEGINTABS]

>[!TAB Solicitação]

```HTTPS
GET /api/{serviceProvider}/list HTTP/1.1

    Authorization: Bearer <access_token>
    AP-Device-Identifier: fingerprint <base64_device_id>
    AD-Service-Token: <service_token>
    Accept: application/json
    User-Agent: <user_agent>
```

>[!TAB Resposta]

```HTTPS
HTTP/1.1 200 OK

Content-Type: application/json

{
  "devices": {}
}
```

>[!ENDTABS]

## Códigos de erro {#error-codes}

### Estrutura de resposta de erro {#error-structure}

Todas as respostas de erro incluem estes campos:

| Campo | Tipo | Descrição |
|:---|:---|:---|
| status | Integer | Código de status HTTP (400, 401, 500 etc.) |
| código | String | Código de erro legível por computador |
| mensagem | String | Descrição legível |
| ação | String | Ação sugerida para o cliente |
| helpUrl | String | Link de documentação |
| trace | String | ID exclusiva de solicitação (UUID) para correlação |

**Exemplo de Resposta de Erro**

```json
{
  "status": "BAD_REQUEST",
  "error": {
    "status": 400,
    "code": "header_missing",
    "message": "Required header is missing",
    "action": "check_headers",
    "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html?lang=pt-BR",
    "trace": "a1b2c3d4-e5f6-7890-abcd-ef1234567890"
  }
}
```

**Exemplo de Resposta Bem-sucedida**

```json
{
  "status": "OK",
  "serviceToken": "eyJhbGciOiJIUzI1NiIs...",
  "notBefore": 1697500800000,
  "notAfter": 1697587200000
}
```

>[!NOTE]
>
>As respostas bem-sucedidas NÃO incluem um campo de erro. As respostas de erro NÃO incluem campos de sucesso.

### Catálogo de códigos de erro {#error-catalog}

#### Erros gerais

| código | status | mensagem | ação |
|:---|:---|:---|:---|
| token_invalid | 400 | O token fornecido é inválido | get_new_token |
| token_expired | 401 | O token expirou | get_new_token |
| header_missing | 400 | Um cabeçalho obrigatório está ausente | check_headers |
| não autorizado | 401 | Acesso não autorizado | nenhum |
| internal_error | 500 | Erro interno | nenhum |

#### Erros de validação

| código | status | mensagem | ação |
|:---|:---|:---|:---|
| request_null | 400 | O objeto de solicitação não pode ser nulo | nenhum |

#### Erros de ServiceToken de POST

| código | status | mensagem | ação |
|:---|:---|:---|:---|
| header_missing | 400 | O cabeçalho x-sso-id ou x-sso-link é necessário para solicitações POST | check_headers |
| header_missing | 400 | O cabeçalho AP-Device-Identifier é necessário para solicitações POST | check_headers |

#### Erros do GET ServiceToken

| código | status | mensagem | ação |
|:---|:---|:---|:---|
| header_missing | 400 | O cabeçalho AD-Service-Token é necessário para solicitações GET | check_headers |
| cabeçalho_inválido | 401 | Assinatura JWT inválida no token de serviço do AD | get_new_token |
| cabeçalho_inválido | 401 | Erro ao validar assinatura JWT | get_new_token |
| cabeçalho_inválido | 401 | O assunto (sub) do JWT está ausente ou vazio no token de serviço do AD | get_new_token |
| cabeçalho_inválido | 401 | Erro ao extrair o assunto do JWT | get_new_token |

#### Erros de validação de link

| código | status | mensagem | ação |
|:---|:---|:---|:---|
| header_missing | 401 | O cabeçalho AD-Service-Token é necessário para solicitações de link | check_headers |
| cabeçalho_inválido | 401 | Assinatura JWT inválida no token de serviço do AD | get_new_token |
| cabeçalho_inválido | 401 | Erro ao validar assinatura JWT | get_new_token |

#### Desvincular Erros de Validação

| código | status | mensagem | ação |
|:---|:---|:---|:---|
| header_missing | 401 | O cabeçalho AD-Service-Token é necessário para desvincular solicitações | check_headers |
| cabeçalho_inválido | 401 | Assinatura JWT inválida no token de serviço do AD | get_new_token |
| cabeçalho_inválido | 401 | Erro ao validar assinatura JWT | get_new_token |
| cabeçalho_inválido | 401 | O assunto (sub) do JWT está ausente ou vazio no token de serviço do AD | get_new_token |
| request_invalid | 400 | A lista de dispositivos não pode ser nula ou vazia | check_request_body |

#### Listar Erros de Validação

| código | status | mensagem | ação |
|:---|:---|:---|:---|
| header_missing | 401 | O cabeçalho AD-Service-Token é necessário para solicitações de lista | check_headers |
| cabeçalho_inválido | 401 | Assinatura JWT inválida no token de serviço do AD | get_new_token |
| cabeçalho_inválido | 401 | Erro ao validar assinatura JWT | get_new_token |
| cabeçalho_inválido | 401 | O assunto (sub) do JWT está ausente ou vazio no token de serviço do AD | get_new_token |
| cabeçalho_inválido | 401 | Erro ao extrair o assunto do JWT | get_new_token |

