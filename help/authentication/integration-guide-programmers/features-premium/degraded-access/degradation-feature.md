---
title: Recurso de degradação
description: Recurso de degradação
exl-id: c7d6685b-a235-42eb-9c9c-0ffa1747f614
source-git-commit: 49a6a75944549dbfb062b1be8a053e6c99c90dc9
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# Recurso de degradação {#degradation-feature}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

No mundo dinâmico dos esportes ao vivo e dos principais eventos, é essencial garantir uma experiência perfeita para o espectador. O alto tráfego durante esses eventos pode sobrecarregar os pontos de acesso de autenticação e autorização do Distribuidor de programação de vídeo multicanal (MVPD), resultando em atrasos ou interrupções.

A Autenticação da Adobe Pass soluciona esses desafios com seu **Recurso de Degradação**, uma solução que permite o desvio temporário de pontos de acesso específicos de autenticação e autorização da MVPD. Esse recurso é especialmente importante durante eventos de pico de tráfego, em que os tempos de resposta podem ser degradados devido a uma carga pesada nos sistemas MVPD.

O **Recurso de Degradação** pode ser uma proteção vital para os programadores, garantindo a continuidade do serviço. Embora seu público principal inclua esportes ao vivo e canais de notícias, sua utilidade se estende a qualquer programador que busque reduzir o risco de interrupções causadas por endpoints do MVPD.

>[!IMPORTANT]
>
> A API de degradação é um recurso premium e requer uma licença atual do Adobe.

Ao aplicar uma regra de degradação, os programadores podem ativar temporariamente a autenticação ou autorização automática, garantindo acesso ininterrupto ao conteúdo pelo período em que a degradação for aplicada. As ações de degradação são sempre iniciadas por um programador com base em acordos pré-acordados com os MVPDs. Embora o Adobe não acione diretamente a degradação no momento, os recursos futuros podem incluir o gerenciamento pró-ativo se os Contratos de nível de serviço (SLAs) forem estabelecidos.

Este recurso foi projetado para ser usado junto com uma [API de monitoramento de uso](/help/authentication/integration-guide-programmers/features-premium/esm/entitlement-service-monitoring-overview.md) e com base nos contratos prévios com MVPDs, a Autenticação da Adobe Pass oferece uma ferramenta poderosa para equilibrar a experiência do usuário, a confiabilidade e o controle operacional durante momentos críticos.

>[!IMPORTANT]
>
> Esse recurso não permite ignorar o próprio serviço de Autenticação do Adobe Pass. Se a Autenticação Adobe Pass estiver indisponível, não haverá um mecanismo integrado neste serviço para facilitar o acesso do usuário. Nesses casos, os sites ou aplicativos podem optar por implementar suas próprias soluções alternativas de roteamento para manter a entrega de conteúdo.

## Acesso à API de degradação {#degradation-api-access}

Antes de acessar a [API de Degradação](#degradation-api), você deve concluir as etapas necessárias no processo de Registro de Cliente Dinâmico (DCR). Esse processo obrigatório garante que você tenha o token de acesso necessário para interagir com a API de degradação.

Para obter instruções abrangentes, consulte a documentação [Visão geral do registro dinâmico do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## API de degradação {#degradation-api}

A API de degradação é uma API RESTful que permite aos programadores gerenciar regras de degradação para MVPDs específicos. A API fornece o meio de ativar, remover e recuperar o status das regras de degradação que estão ativas.

Para saber mais sobre a API de degradação, consulte o seguinte documento do Zendesk [Autenticação do Adobe Pass | Degradation API v3](https://tve.zendesk.com/hc/en-us/articles/33912526308372-Adobe-Pass-Authentication-Degradation-API-v3) e procure o arquivo PDF para baixar.

## REST API V2 {#rest-api-v2}

Para aproveitar o recurso de Degradação, é necessário implementar atualizações de código para modificar a forma como o aplicativo TV Everywhere (TVE) interage com a [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) de Autenticação da Adobe Pass.

Para obter um guia abrangente sobre essas atualizações e os fluxos de trabalho associados, consulte a documentação de [Fluxos de acesso degradados](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/degraded-access-flows/rest-api-v2-access-degraded-flows.md).
