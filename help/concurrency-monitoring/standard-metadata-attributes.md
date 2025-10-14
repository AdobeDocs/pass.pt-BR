---
title: Atributos de metadados padrão
description: Atributos de metadados padrão
exl-id: 99ffa98c-213f-47a5-a6e7-fbacb77875d0
source-git-commit: ae2e61152695b738b0bb08d1dcd81417f3bbdfb5
workflow-type: tm+mt
source-wordcount: '1053'
ht-degree: 4%

---

# Atributos de metadados padrão {#std-metadata-attributes}

Esta página tem como objetivo fornecer uma lista completa de atributos de metadados que o serviço de Monitoramento de Simultaneidade pode processar e que podem ser usados como base para políticas que podem ser implementadas. Os atributos de metadados padrão podem ser categorizados da seguinte maneira:

* Atributos incluídos por design (enviados em cada chamada de inicialização de sessão, pois são necessários no caminho do URL). Nenhuma chamada válida pode ser executada sem esses valores.
* Atributos de metadados: valores que precisam ser passados como dados de formulário durante a chamada de inicialização da sessão (se as políticas de back-end exigirem seus valores).

## Atributos exigidos pelo design {#attr-req-by-design}

A API de Monitoramento de Simultaneidade força os clientes a enviar os seguintes valores como parte de qualquer chamada de inicialização válida: [chamadas de iniciação de sessão](/help/concurrency-monitoring/restrict-concurr-usage-mult-apps.md#api-calls-descr).

| Nome do campo | Exemplo de valor | Onde usá-lo | Obtido de |
|---------------|-----------------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationId | 75b4-431b-adb2-eb6b9e546013 | Cabeçalho de autorização | Tíquete do Zendesk na integração |
| mvpdName | Sample_MVPD | Caminho do URI | Autenticação do Adobe Pass a partir do endpoint de configuração quando o usuário seleciona o MVPD |
| accountId | 12345 | Caminho do URI | Metadados upstreamUserID de autenticação do Adobe Pass após logon do usuário [Metadados do usuário upstreamUserID - Autenticação do Adobe Pass](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md) |


## Atributos de metadados {#metadata-attr}

Os campos na tabela abaixo podem ser usados por programadores e MVPDs para criar políticas que serão implementadas no Monitoramento de simultaneidade.

Com a [API v2.0](http://docs.adobeptime.io/cm-api-v2/), se qualquer um desses atributos for exigido pelas políticas definidas, uma tentativa de inicialização de sessão sem esse atributo resultará em uma Solicitação 400 inválida.


| Entidade | Nome do atributo | Tipo de dados | Descrição | Referência externa (por exemplo: EIDR, OATC) | Exemplo de valor | Regras de validação |
|---------------|-------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Empresa de mídia | programmerName | string | O nome do programador |                                                   | ProgrammerX |                                                                                   |
| Recurso | channel | string | O canal de TV |                                                   | ChannelY |                                                                                   |
|                 | assetId | string | O título &quot;amigável&quot; ou legível pelo consumidor a ser apresentado para este conteúdo | [Referência de campos de dados do EIDR 2.0](https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/EIDR_2_0_Data_Fields.pdf){target=_blank} | Ben-Hur |                                                                                   |
|                 | type | enumeração | Um valor que descreve o tipo geral de conteúdo representado pelo TveItem. Os valores enumerados incluem: filme broadcastEpisódio nonBroadcastEpisódio musicVideo awardsShow clip concerto conferência newsEvento sportingEvento trailer | [Prática Recomendada do Feed de Metadados OATC](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | broadcastEpisode | O campo deve corresponder a um dos itens na lista discriminada |
|                 | contentType | string | Este campo determina se o conteúdo solicitado é em tempo real ou VOD | N/A | live, vod | live ou vod |
|                 | gênero | string | O gênero do conteúdo que é transmitido. Descreve o tipo de programação geral | [Prática Recomendada de Feed de Metadados OATC](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | Comédia | Tipo de gênero válido |
|                 | duração | número | A duração do item de mídia em segundos | [Prática Recomendada do Feed de Metadados OATC](https://userfiles-kb.s3.amazonaws.com/userfiles/258/326/ckfinder/files/OATC%20Metadata%20Feed%201_0d_1%20OATC%20BOARD%20APPROVED%20FOR%20RELEASE%20%281%29.pdf?X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIMM7Q2VAGHGVAOHA%2F20230803%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230803T144225Z&X-Amz-SignedHeaders=host&X-Amz-Expires=1200&X-Amz-Signature=e61658133a4875ff48757b1a3bafb7627054ba6fc75c134a3dea9fa8022b45fa){target=_blank} | 1800 | sequência numérica |
| Dispositivo/Navegador | deviceId | string | O identificador exclusivo do dispositivo. | [Propriedades do Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | 2b6f0cc904d137be2e1730235f5664094b831186 |                                                                                   |
|                 | deviceName | string | O nome amigável deste dispositivo. |                                                   | Joe&#39;s iPad |                                                                                   |
|                 | marketingName | string | O nome de marketing (ou o nome amigável do cliente) de um dispositivo | [Propriedades do Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | iPhone 6s | nome de marketing válido |
|                 | mobileDevice | booleano | Verdadeiro se o dispositivo for para ser usado em movimento | [Propriedades do Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | verdadeiro, falso | verdadeiro, falso |
|                 | deviceModel | string | O nome do modelo do dispositivo, navegador ou outro componente | [Propriedades do Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | tablet, telefone, xbox. decodificador de sinais | nome do modelo de dispositivo válido |
|                 | osName | string | O sistema operacional em que o dispositivo está sendo executado | [Atlas de Dispositivos - Valores de propriedades predefinidas do sistema operacional](https://deviceatlas.com/device-data/explorer/#defined_property_values/877430/4121272){target=_blank} | Android, Windows 10, OS X, Linux, Outro Observação: você precisa estar conectado com um nome de usuário e senha no Device Atlas para visualizar os valores de propriedade | o valor esperado é um dos valores nas propriedades predefinidas do Device Atlas |
|                 | browserName | string | O nome ou o tipo de navegador no dispositivo | [Atlas de Dispositivos - Valores de propriedades predefinidos do navegador](https://deviceatlas.com/device-data/explorer/#defined_property_values/7/2705619){target=_blank} | O nome ou tipo do navegador no dispositivo.  Observação: você precisa estar conectado com um nome de usuário e senha no Device Atlas para exibir os valores de propriedade | o valor esperado é um dos valores nas propriedades predefinidas do Device Atlas |
|                 | browserVersion | string | A versão do navegador no dispositivo | [Propriedades do Device Atlas](https://deviceatlas.com/device-data/properties){target=_blank} | A versão do navegador no dispositivo |                                                                                   |
| Aplicativo | applicationName | string | O nome &quot;amigável&quot; ou legível do consumidor do aplicativo | N/A | Aplicativo_de_amostra |                                                                                   |
|                 | applicationId | string | A ID do aplicativo que identifica exclusivamente um aplicativo cliente. | N/A | de305d54-75b4-431b-adb2-eb6b9e546013 |                                                                                   |
|                 | applicationPlatform | string | A plataforma nativa do aplicativo | N/A | ios, android |                                                                                   |
|                 | applicationVersion | string | Esse valor pode ser usado para fins de análise | N/A | 1.0, 2.0 |                                                                                   |
| Assunto | accountId | string | A ID da conta do assunto do Monitoramento de simultaneidade (no escopo do MVPD) | N/A | conta-teste |                                                                                   |
|                 | contractType | string | premium, básico. Os clientes podem adicionar isso como metadados personalizados e usá-lo em seus próprios domínios | N/A | premium, básico |                                                                                   |
| Usuário | name | string | Alguns MVPDs fornecem informações relacionadas ao usuário específico que reproduz o conteúdo. | N/A |                                                                                                                                                         |                                                                                   |
|                 | hba | booleano | Identifica se o usuário tenta iniciar o fluxo a partir de sua localização inicial | N/A | verdadeiro, falso | verdadeiro ou falso |
| Localização | continente | string | O continente de onde a ID do dispositivo que está enviando a solicitação de reprodução se origina | N/A | América do Norte | nome de continente válido |
|                 | país | string | O país de onde a deviceID que envia a solicitação de reprodução é originária | N/A | EUA | nome do país válido |
|                 | state | string | O estado do qual a deviceID que envia a solicitação de reprodução se origina | N/A | CA | nome de estado válido |
|                 | city | string | A cidade de onde a deviceID que envia a solicitação de reprodução se origina | N/A | Cupertino | nome de cidade válido |
|                 | código postal | número | O código postal de onde a deviceID que envia a solicitação de reprodução é originária | N/A | 95014 | código postal válido |
| Fluxo | streamId | string | Gerado pelo serviço CM, não está no controle do cliente. É usado implicitamente quando regras do tipo maxstreams são definidas. | N/A | N/A | N/A |
|                 | streamCDN | string | indica a CDN de onde o fluxo foi buscado | N/A | N/A | N/A |

## Exemplos de uso de atributos de metadados para a criação de políticas {#examples-metadata-attr}

Os campos de metadados padrão podem ser usados para definir políticas do lado do servidor com base nos valores dos campos:

* Você pode configurar uma política para ser aplicada somente a valores de campo específicos (por exemplo, uma política iOS dedicada: onde `osType` é `iOS`)
* É possível limitar o número de valores distintos para um determinado campo. Alguns exemplos são os seguintes:
   * não mais do que X dispositivos distintos: `HAVING DISTINCT COUNT(deviceId) <= 2`
   * não mais do que X códigos postais distintos: `HAVING DISTINCT COUNT(zipcode) <= 3`
* Você pode limitar o número de fluxos ativos por valor de campo. Alguns exemplos são os seguintes:
   * não mais do que X fluxos ativos para um único tipo de dispositivo: `GROUP BY deviceType HAVING COUNT(streamId) <= 3`
   * não mais do que X fluxos ativos para fluxos de conteúdo ao vivo: `SELECT COUNT(streamId) AS streamCount WHERE contentType='live' HAVING streamCount <= 3`

Contate a equipe de Monitoramento de simultaneidade [criando um tíquete no Zendesk](mailto:tve-support@adobe.com) e indique quais políticas você deseja implementar.

Você pode encontrar mais exemplos de políticas e livros de cookies de integração no seguinte:

* [Ponto de decisão da política](/help/concurrency-monitoring/cm-policy-decision-point.md)
* [Console de API - Monitoramento de Simultaneidade de Adobe](http://docs.adobeptime.io/cm-api-v2/)
