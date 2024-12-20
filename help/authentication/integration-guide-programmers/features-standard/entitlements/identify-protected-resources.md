---
title: Identificação de recursos protegidos
description: Identificação de recursos protegidos
exl-id: e96aea02-54b2-491d-ba91-253c0d0e681c
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---

# Identificação de recursos protegidos {#identifying-protected-resources}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview}

Cada solicitação de autorização (ou solicitação para verificar a autorização) deve conter um identificador exclusivo para o recurso protegido para o qual o usuário está solicitando acesso. Um recurso protegido pode ser qualquer nível de conteúdo autorizado, conforme acordado entre um MVPD e os Programadores participantes. Os recursos protegidos potenciais devem se encaixar nessa estrutura de árvore de granularidade cada vez mais específica:

- Rede
   - Canal
      - Mostrar
         - Episódio
            - Ativo

</br>

## Formato RSS de Mídia {#media_rss}

Os recursos podem ser identificados por uma string simples (um identificador exclusivo para um canal) ou podem ser representados no formato Media RSS (MRSS), conforme acordado entre o Adobe (ou um parceiro autorizado de autenticação da Adobe Pass) e os MVPDs e Programadores participantes. A cadeia de caracteres RSS usada como um especificador de recurso pode incluir informações adicionais, como classificações e metadados de controle dos pais.


Se você usar um identificador de recurso simples, como &quot;TNT&quot;, presume-se que ele represente um canal, e é traduzido neste especificador de recurso RSS:

```RSS
    <rss version="2.0"> 
        <channel>
            <title>TNT</title>
        </channel>
    </rss>
```


Um especificador mais complexo pode incluir, por exemplo, informações de classificação adicionais. Você pode passar toda a cadeia de caracteres RSS para funções de Ativador de Acesso que exigem uma ID de recurso, como [`getAuthorization()`](/help/authentication/integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md):

```rss
    var resource = 
        '<rss version="2.0" xmlns:media="http://search.yahoo.com/mrss/"> 
             <channel>
                 <title>TNT</title>
                 <media:rating scheme="urn:mpaa">pg</media:rating>
             </channel>
         </rss>'; 
    getAuthorization(resource);
```

Os especificadores de recursos são opacos à autenticação Adobe Pass; eles são simplesmente transmitidos para o MVPD. Se o MVPD não reconhecer ou não puder analisar o especificador de recurso, retornará um erro à Autenticação Adobe Pass, que transmitirá o erro de volta à chamada de retorno `tokenRequestFailed()`.

<!--
## Related Information {#related}

-  User Metadata
-  Preflight Authorization
-->
