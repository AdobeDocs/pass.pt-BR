---
title: Recursos protegidos
description: Recursos protegidos
source-git-commit: dbca6c630fcbfcc5b50ccb34f6193a35888490a3
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 0%

---

# Recursos protegidos {#protected-resources}

>[!IMPORTANT]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Os recursos protegidos representam conteúdo que os usuários podem transmitir e são identificados por valores únicos estabelecidos por meio de acordos entre MVPDs e Programadores participantes.

Os recursos protegidos seguem uma estrutura hierárquica em árvore, com cada nível fornecendo maior granularidade para autorização de conteúdo:

* Rede
   * Canal
      * Mostrar
         * Episódio
            * Ativo

## Identificadores de recursos protegidos {#identifiers}

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

## REST API V2 {#rest-api-v2}

As APIs de autenticação do Adobe Pass que recuperam decisões de pré-autorização e autorização exigem que o aplicativo cliente inclua os identificadores de recursos protegidos como parâmetros.

As decisões de pré-autorização e autorização podem ser recuperadas usando as seguintes APIs:

* [Recuperar decisões de pré-autorização usando mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-preauthorization-decisions-using-specific-mvpd.md)
* [Recuperar decisões de autorização usando mvpd específico](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/decisions-apis/rest-api-v2-decisions-apis-retrieve-authorization-decisions-using-specific-mvpd.md)

Consulte as seções **Solicitação** e **Amostras** das APIs acima para entender o formato necessário para fornecer os identificadores exclusivos de recursos protegidos.

Os identificadores exclusivos são opacos à autenticação da Adobe Pass, pois são transmitidos diretamente para a MVPD. Se o MVPD não puder reconhecer ou analisar um identificador de recurso, retornará um erro para a Autenticação Adobe Pass, que retransmite o erro de volta para o aplicativo cliente usando um [Código de Erro Aprimorado](/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md).

Para obter mais detalhes sobre como e quando integrar as APIs acima, consulte os seguintes documentos:

* [Fluxo básico de pré-autorização executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-preauthorization-primary-application-flow.md)
* [Fluxo de autorização básico executado no aplicativo principal](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authorization-primary-application-flow.md)
