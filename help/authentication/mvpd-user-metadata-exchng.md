---
title: Troca de metadados de usuário MVPD
description: Troca de metadados de usuário MVPD
exl-id: 8bce6acc-cd33-476c-af5e-27eb2239cad1
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '943'
ht-degree: 0%

---

# Troca de metadados de usuário MVPD

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#intro-user-metadata-exchange}

Os MVPDs mantêm metadados específicos do usuário sobre seus clientes que, em alguns casos, são compartilhados com Programadores. O objetivo da autenticação da Adobe Pass é intermediar uma troca desses &quot;metadados do usuário&quot;, mas não impor nenhum tipo de regra relacionada à troca. As regras de troca são para os MVPDs para trabalhar com seus parceiros Programadores.

Os tipos de metadados de usuário disponíveis para o Exchange atualmente incluem o seguinte:

* Código postal
* Classificação máxima (VChip ou MPAA)
* ID de usuário
* ID da família
* ID do canal

Usando esse recurso, os MVPDs e Programadores podem implementar casos de uso especiais, como o controle dos pais. Por exemplo, um MVPD pode transmitir dados de classificação dos pais a um Programador, que os usa para filtrar as opções de exibição disponíveis para um usuário.

Pontos principais dos metadados do usuário:

* O MVPD transmite metadados do usuário para o aplicativo do Programador durante os fluxos de autenticação e autorização
* A Autenticação Adobe Pass salva os valores de metadados nos tokens AuthN e AuthZ
* A Autenticação do Adobe Pass pode normalizar valores para MVPDs que fornecem metadados do usuário em diferentes formatos
* Alguns parâmetros podem ser criptografados usando a chave do programador
* Valores específicos são disponibilizados por Adobe, por meio de uma alteração de configuração

>[!NOTE]
>
>Os metadados do usuário são uma extensão para os metadados estáticos (TTL do token de autenticação, TTL do token de autorização e ID do dispositivo) disponíveis anteriormente na autenticação do Adobe Pass.

## Exemplos {#example-mvpd-user-metadata-exch}

### Controle dos Pais {#example-parental-control}

Este exemplo mostra a troca do seguinte:

* [Programador para o MVPD Metadata Exchange](#progr-mvpd-metadata-exch)

* [Fluxo de MVPD para Intercâmbio de Metadados de Programador](#mvpd-progr-exchange-flow)

### Programador para o MVPD Metadata Exchange {#progr-mvpd-metadata-exch}

Atualmente, a API do programador, a Autenticação do Adobe Pass e os Autorizadores MVPD suportam apenas a autorização no nível do canal. O canal é especificado como uma cadeia de texto sem formatação na chamada da API getAuthorization() do programador. Essa cadeia de caracteres é propagada completamente para o back-end de autorização do MVPD:

No aplicativo ou site do Programador, o usuário escolhe um MVPD compatível com XACML (neste exemplo, &quot;TNT&quot;). Para obter informações sobre XACML, consulte [Linguagem de marcação de controle de acesso extensível](https://en.wikipedia.org/wiki/XACML){target=_blank}.
O aplicativo do Programador forma uma solicitação AuthZ que inclui o recurso e seus metadados.  Este exemplo inclui uma classificação MPAA de &quot;pg&quot; no atributo de mídia do elemento de canal:

```XML
var resource = '<rss version="2.0" xmlns:media="http://video.search.yahoo.com/mrss/">
                    <channel> 
                        <title>TNT</title> 
                        <media:rating scheme="urn:mpaa">pg</media:rating>
                    </channel>
                </rss>';
getAuthorization(resource);
```

A Autenticação do Adobe Pass realmente oferece suporte a autorizações mais granulares, até o nível do ativo, quando suportado pelo MVPD e pelo Programador. O recurso e seus metadados são opacos ao Adobe; a intenção é estabelecer um formato padrão para especificar a ID do recurso e os metadados de uma maneira normalizada, a fim de enviar IDs de recurso para MVPDs diferentes.

>[!NOTE]
>
>Se o usuário escolher um MVPD compatível somente com canal, a Autenticação do Adobe Pass extrairá SOMENTE o título do canal (&quot;TNT&quot; no exemplo acima) e transmitirá somente o título para o MVPD.

### Fluxo de MVPD para Intercâmbio de Metadados de Programador {#mvpd-progr-exchange-flow}

A Autenticação Adobe Pass assume as seguintes premissas:

* O MVPD envia a classificação máxima como parte da resposta SAML
* Essas informações são salvas como parte do token de autenticação
* Uma API é fornecida pela autenticação da Adobe Pass para permitir que os programadores recuperem essas informações
* Os programadores implementam esse recurso no site ou aplicativo (por exemplo, para ocultar vídeos acima da classificação máxima para o usuário)

```XML
<saml:Assertion ID="pfxec5f92e0-8589-3fc3-c708-f4fb8e2fad59"
                 IssueInstant="2010-07-20T10:05:41Z" Version="2.0"
                 xmlns:xs="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <saml:AttributeStatement>
        <saml:Attribute
                Name="MaxTVRating"
                NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
            <saml:AttributeValue xsi:type="xs:string">tv-ma</saml:AttributeValue>
        </saml:Attribute>
        <saml:Attribute
                Name="MaxMovieRating"
                NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:basic">
            <saml:AttributeValue xsi:type="xs:string">nc-17</saml:AttributeValue>
        </saml:Attribute>
    </saml:AttributeStatement>
</saml:Assertion>
```

### Notas {#notes-mvpd-progr-metadata-exch-flow}

**Normalização e validação de recursos.** As IDs de recurso podem ser transmitidas como uma sequência de caracteres simples ou uma sequência de caracteres MRSS. Um programador pode decidir usar o formato de string simples ou o MRSS, mas precisará de um acordo prévio com o MVPD para que o MVPD saiba como tratar esse recurso.

**ID do recurso e especificação de metadados.** A Autenticação do Adobe Pass usa o padrão RSS com a extensão RSS de mídia para especificar um recurso e seus metadados. Juntamente com a extensão Media RSS, a Autenticação do Adobe Pass é compatível com uma grande variedade de metadados, como controles pelos pais (via `<media:rating>`) ou geolocalização (`<media:location>`).

A Autenticação do Adobe Pass também pode oferecer suporte à conversão transparente da cadeia de caracteres do canal herdado para o recurso RSS correspondente para MVPDs que exigem RSS. Na outra direção, a Autenticação do Adobe Pass oferece suporte à conversão de RSS+MRSS em título de canal simples, para MVPDs somente de canal.

**A Autenticação Adobe Pass garante total compatibilidade com versões anteriores de integrações existentes.** Ou seja, para programadores que usam autenticação no nível do canal, a Autenticação do Adobe Pass cuida de empacotar a ID do canal no formato necessário antes de enviá-la para um MVPD que entenda esse formato. O inverso também se aplica: se um Programador especificar todos os seus recursos em um novo formato, a Autenticação do Adobe Pass converterá o novo formato em uma string de canal simples se autorizar em relação a um MVPD que apenas faz autorização no nível do canal.

## Casos de uso de metadados do usuário {#user-metadata-use-cases}

Os casos de uso estão em constante mudança e expansão à medida que mais MVPDs fazem acordos legais e adicionam funcionalidade. Veja a seguir exemplos de para que os metadados do usuário podem ser usados.

* [ID de usuário MVPD](#mvpd-user-id)
* [ID da família](#household-user-id)
* [Código postal](#zip-code)
* [Classificação máxima (controle dos pais)](#max-rating-parental-control)
* [Alinhamento de canais](#channel-line-up)

### ID de usuário MVPD {#mvpd-user-id}

* Conforme fornecido pelo MVPD
* Não as informações de logon reais do usuário, pois são transformadas em hash pelo MVPD
* Pode ser usado para indicar problemas com ou para usuários específicos
* Criptografado
* Suporte MVPD: Todos os MVPDs

### ID de Usuário Doméstico {#household-user-id}

* Permite informações de boa métrica
* Criptografado
* Suporte a MVPD: alguns MVPDs

### Código postal {#zip-code}

* O CEP de faturamento do usuário
* Usado principalmente para aplicar regras de período de congelamento de evento esportivo
* Pode ser fornecido com a resposta AuthZ para atualizações rápidas
* Suporte a MVPD: alguns MVPDs

### Classificação máxima (controle dos pais) {#max-rating-parental-control}

* AuthN inicialmente, mais atualização AuthZ
* Filtrar conteúdo por meio da interface
* Classificações de MPAA ou VChip
* Suporte a MVPD: alguns MVPDs

### Alinhamento de canais {#channel-line-up}

* Os MVPDs podem fornecer uma lista de canais que o usuário tem direito a visualizar
* Permite uma pintura rápida da interface
* A especificação OLCA permite isso como uma Instrução de Atributo na resposta AuthN
* MVPDs compatíveis: alguns MVPDs

<!--
>[!RELATEDINFORMATION]
>
>* [Proxy MVPD Web Service](/help/authentication/proxy-mvpd-webserv.md)
>* [Content Metadata Exhange](/help/authentication/mvpd-content-metadata-exchange.md)
>* [OLCA AuthN / AuthZ Specification](https://www.cablelabs.com/specifications/CL-SP-AUTH1.0-I04-120621.pdf){target=_blank}
>* [User Metadata (Programmer Integration Guide)](/help/authentication/user-metadata-feature.md)
-->
