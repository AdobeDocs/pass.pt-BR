---
title: Decisões
description: Decisões
exl-id: 1efd70af-8c1d-43c4-87fc-14488d42b23d
source-git-commit: a19f4fd40c9cd851a00f05f82adbabb85edd8422
workflow-type: tm+mt
source-wordcount: '988'
ht-degree: 0%

---

# Decisões {#decisions}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

As decisões são geradas pela [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) da Autenticação do Adobe Pass com base nas consultas de autorização ou pré-autorização do MVPD do usuário, determinando se o acesso a [conteúdo protegido](#protected-resources) é concedido ou negado.

Há dois tipos de decisões que são fornecidas, dependendo da API invocada:

* [Decisões de pré-autorização](#preauthorization-decisions) que são decisões informativas.
* [Decisões de autorização](#authorization-decisions) que são decisões autoritativas.

## Decisões de pré-autorização {#preauthorization-decisions}

A decisão de pré-autorização é uma decisão informativa que permite que o aplicativo cliente seja informado se a MVPD pode permitir ou negar o acesso do usuário a um [recurso protegido](#protected-resources).

O objetivo da pré-autorização (autorização de comprovação) é permitir que o aplicativo exiba informações precisas sobre o conteúdo que o usuário pode estar qualificado a visualizar. Isso é feito aprimorando a interface do usuário com indicadores, como ícones bloqueados ou desbloqueados, para refletir o status de acesso.

>[!IMPORTANT]
>
> A decisão de pré-autorização não deve ser usada de forma autoritativa para reproduzir recursos, pois essa é a finalidade de uma [decisão de autorização](#authorization-decisions).

O uso da API de pré-autorização não é obrigatório. O aplicativo cliente pode ignorar isso se quiser apresentar um catálogo de recursos sem nenhuma filtragem.

Se o aplicativo cliente pretende usar esse recurso, é importante observar que as decisões de pré-autorização só podem ser obtidas para um número limitado de recursos por solicitação de API, normalmente até 5.

>[!IMPORTANT]
> 
> O número máximo de recursos pode ser aumentado somente após chegar a um acordo com os MVPDs e representantes de autenticação da Adobe Pass. Depois de acordadas, as alterações podem ser implementadas por meio do Painel do Adobe Pass TVE por um administrador em sua organização ou um representante de autenticação da Adobe Pass que atue em seu nome.
> 
> Para obter mais detalhes, consulte a documentação do [Guia do Usuário de Integrações do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#add-more-properties).

Os MVPDs podem oferecer suporte à pré-autorização por meio de vários mecanismos, cada um com implicações distintas para o desempenho e o número máximo de recursos que podem ser manipulados em uma única solicitação de API.

Para obter mais detalhes sobre os mecanismos existentes que oferecem suporte à pré-autorização, consulte a documentação da [Autorização de simulação do MVPD](/help/authentication/integration-guide-mvpds/mvpd-preflight-authz.md).

>[!IMPORTANT]
>
> Para MVPDs sem suporte completo de autorização de comprovação, o uso da pré-autorização deve ser acordado antecipadamente com os MVPDs e representantes de autenticação da Adobe Pass, pois pode resultar em problemas de desempenho e tempos de resposta mais lentos.

## Decisões de autorização {#authorization-decisions}

A decisão de autorização é uma decisão autoritativa que permite que o aplicativo cliente esteja em conformidade com a decisão da MVPD de permitir ou negar o acesso do usuário a um [recurso protegido](#protected-resources).

A finalidade da autorização é permitir que o aplicativo reproduza os recursos solicitados pelo usuário, após a validação de direitos com o MVPD e o recebimento de um token de mídia da Autenticação Adobe Pass.

>[!IMPORTANT]
> 
> A autenticação da Adobe Pass recomenda que os programadores usem a biblioteca Verificador de token de mídia para validar o token de mídia incluído em uma decisão de autorização, garantindo acesso seguro antes de iniciar o fluxo de vídeo.
> 
> Para obter mais detalhes, consulte a documentação dos [Tokens de mídia](/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md).

O uso da API de autorização é obrigatório. O aplicativo cliente não pode ignorar essa fase se quiser reproduzir recursos solicitados pelo usuário, pois requer a verificação com o MVPD de que o usuário tem direito antes de liberar o fluxo.

É importante observar que as decisões de autorização só podem ser obtidas para um número limitado de recursos por solicitação de API, normalmente 1.

>[!IMPORTANT]
>
> O número máximo de recursos pode ser aumentado somente após chegar a um acordo com os MVPDs e representantes de autenticação da Adobe Pass.

## Gerenciamento de TTL (Time-to-Live) de autorização {#authorization-ttl-management}

O TTL (Time-to-Live) de autorização define por quanto tempo um recurso permanece autorizado antes de precisar ser reautorizado. Esse período é limitado e deve ser acordado com os representantes da MVPD. Os valores de TTL podem variar com base em:

* Categoria da plataforma (por exemplo, desktop, dispositivo móvel, dispositivos conectados à TV)
* Plataforma específica (por exemplo, iOS, Android, tvOS, Roku, FireTV)

O TTL de autorização (authZ) pode ser exibido e alterado no [Painel do TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) da Adobe Pass por um dos administradores da organização ou por um representante da Autenticação da Adobe Pass que atue em seu nome.

Para obter mais detalhes, consulte a documentação do [Guia do Usuário de Integrações do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

## Recursos protegidos {#protected-resources}

Os recursos protegidos referem-se ao conteúdo otimizável, identificado por valores únicos definidos por acordos entre os MVPDs e os programadores participantes.

Os recursos protegidos seguem uma estrutura hierárquica em árvore, com cada nível fornecendo maior granularidade para autorização de conteúdo:

* Rede
   * Canal
      * Mostrar
         * Episódio
            * Ativo

>[!IMPORTANT]
>
> A pré-autorização (autorização de comprovação) foca nos recursos no nível do canal com identificadores que têm um formato de string simples ou MRSS.
> 
> Não recomendamos usar recursos com identificadores que incluam seções `CDATA` no caso de pré-autorização, pois eles são usados principalmente para recursos no nível do ativo definidos por um MRSS.

### Identificador do recurso {#resource-identifier}

O identificador exclusivo do recurso pode ter dois formatos:

* Um formato de string simples, como um identificador exclusivo de um canal (marca).
* Um formato RSS de mídia (MRSS) com informações adicionais, como título, classificações e metadados de controle dos pais.

No caso de um identificador de recurso simples, como &quot;REF30&quot; (que representa um canal), ele pode ser traduzido em um identificador de recurso RSS da seguinte maneira:

```RSS
    <rss version="2.0"> 
        <channel>
            <title>REF30</title>
        </channel>
    </rss>
```

No caso de um identificador de recursos mais complexo, o identificador de recursos RSS pode incluir informações adicionais de classificação, como se segue:

```RSS
    <rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
        <channel>
            <title>REF30</title>
            <media:rating scheme="urn:mpaa">pg</media:rating>
        </channel>
    </rss>
```

Os identificadores exclusivos são principalmente opacos à autenticação da Adobe Pass, no entanto, os transformadores podem ser aplicados com base nos recursos e requisitos da MVPD. Se o MVPD não puder reconhecer ou analisar um identificador de recurso, retornará um erro para a Autenticação Adobe Pass, que subsequentemente retransmite o erro para o aplicativo cliente usando um [Código de Erro Aprimorado](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

## REST API V2 {#rest-api-v2}

As decisões de pré-autorização podem ser recuperadas usando a seguinte API:

* [Recuperar decisões de pré-autorização usando mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)

As decisões de autorização podem ser recuperadas usando a seguinte API:

* [Recuperar decisões de autorização usando mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Consulte as seções **Resposta** e **Amostras** das APIs acima para entender a estrutura das decisões de pré-autorização e autorização.

Para obter mais detalhes sobre como e quando integrar as APIs acima, consulte os seguintes documentos:

* [Fluxo básico de pré-autorização executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [Fluxo de autorização básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)

>[!MORELIKETHIS]
>
> [Perguntas frequentes sobre a Fase de Pré-autorização](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#preauthorization-phase-faqs-general)
> [Perguntas Frequentes da Fase de Autorização](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-faqs.md#authorization-phase-faqs-general)
