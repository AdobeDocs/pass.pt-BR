---
title: Glossário
description: Glossário de termos no Monitoramento de simultaneidade
exl-id: 3b3b36fe-9f04-4de9-bd84-9f8d766bbc71
source-git-commit: f30b6814b8a77424c13337d44d7b247105e0bfe2
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---

# Glossário {#glossary}

## ID da Conta {#accid-defn}

* Conta de MVPD de um assinante, geralmente correspondente à conta de faturamento real. Essa conta deve ser identificável pela MVPD em seu próprio sistema.

## Ação {#action-defn}

* O tipo de acesso que o assunto solicita; os valores possíveis para o CM são ***iniciar*** ou ***continuar*** uma sessão de streaming.

## Fluxo ativo {#active-stream-defn}

* Um fluxo que recebeu pelo menos um evento (pulsação) nos últimos 90 segundos.

* ***Observação:*** se o último evento no fluxo for do tipo parar (`?event=stop`), ele não será contado. Esta é uma otimização que permite ao reprodutor fechar explicitamente um fluxo para que não seja mais considerado &quot;ativo&quot;.

## Aplicativo {#application-defn}

* Desenvolvido pelo locatário para acesso ao conteúdo de vídeo
* Toma e impõe decisões sobre o acesso ao conteúdo com base nas informações fornecidas pelo Serviço de Monitoramento de Simultaneidade (isso é válido no caso [Ponto de Informações de Política](/help/concurrency-monitoring/policy-info-pt-versionone.md))
* Terá uma **ID de aplicativo** exclusiva fornecida pela Adobe.

## Serviço de monitoramento de concorrência {#cm-service-defn}

* Atua como um sistema de monitoramento para os assinantes, apoiando os MVPDs e programadores em seus requisitos de aplicação de políticas entre aplicativos.
* Recebe pulsações que indicam a atividade do fluxo.
* Atua como um _Ponto de decisão de política_ avaliando solicitações de autorização com base na atividade do usuário e fornecendo uma resposta de permissão/negação.
* Atua como um _Ponto de Informações de Política_ relatando o número de fluxos ativos (e metadados de fluxo adicionais) para um assinante.

## Ambiente {#env-defn}

* Informações adicionais relevantes para a solicitação, como configurações ou hora do sistema.

## MVPD {#mvpd-defn}

* Distribuidor de programação de vídeo multicanal.
* Atua como Provedor de Autenticação e Autorização, mas também pode ser Provedor de Serviço ou Conteúdo.

## Política {#policy-defn}

* O conceito de controle de acesso principal no CM definido como um target e uma ou mais regras agrupadas em um nome exclusivo.

## Ponto de Administração de Política (PAP) {#policy-admin-pt-defn}

* Ponto que gerencia as políticas de autorização de acesso. Isso não será documentado aqui, mas acabaremos fornecendo um console de autoatendimento para que os clientes gerenciem suas políticas de acesso.

## Ponto de decisão de política (PDP) {#policy-decn-pt-defn}

* Ponto que avalia as solicitações de acesso em relação às políticas de autorização antes de emitir decisões de acesso.

## Ponto de Aplicação da Política (PEP) {#policy-ef-pt-defn}

* O ponto que intercepta a solicitação de acesso do usuário a um recurso, faz uma solicitação de decisão ao PDP e impõe essa decisão na solicitação. No momento, este é o aplicativo cliente e não há plano de transferir essa função para o Monitoramento de simultaneidade.

## PIP (Ponto de Informações de Política) {#policy-info-pt-defn}

* Uma origem de valores de atributo. O Monitoramento de simultaneidade atua como um ponto de informações, fornecendo:
   * metadados de fluxo de passagem.
   * métricas de atividade relacionadas aos fluxos simultâneos.

## Programador {#programmer-defn}

* Atua como um provedor de serviço e conteúdo.
* Depende do aplicativo cliente implantado que se integra ao serviço de Monitoramento de simultaneidade para aplicar as políticas de segurança definidas com base nos dados de serviço mencionados acima.
* Precisa oferecer suporte à MVPD para coletar a atividade do assinante e aplicar as regras de limitação quando estiver em suas propriedades.
* Também pode estar interessado em limitar o acesso simultâneo ao conteúdo em todos os portais de destino, como uma regra separada.

  *P: Por que o Programador e a ID do Solicitante não são como no restante da Autenticação do Adobe Pass?*

  *A: o motivo é permitir que os Programadores usem este parâmetro de forma flexível para transmitir ou isolar dados entre suas propriedades, dependendo de seus casos de uso.*

## Recurso {#resource-defn}

* O conteúdo real que um assunto deseja consumir. O recurso geralmente carrega atributos relacionados ao proprietário (editor) e também pode fornecer informações extras, como gênero ou qualquer outra coisa.

## Regra {#rule-defn}

* Uma função booleana avaliada em relação a um fluxo específico e à atividade do usuário relevante para determinar se o acesso deve ser permitido ou negado para esse fluxo.

## Sessão de transmissão {#streaming-session-defn}

* Uma sessão de vídeo de fluxo contínuo iniciada por um assunto para consumir um recurso específico.

## Assunto {#subj-defn}

* O consumidor do conteúdo (vídeo) na Internet. Estamos deliberadamente evitando o termo _&#x200B;**usuário**&#x200B;_, já que o Monitoramento de simultaneidade geralmente lida com IDs de conta da MVPD (que envolvem vários usuários reais compartilhando o mesmo contrato, por exemplo, membros da família de uma família).

* Para cada fluxo, o sujeito pode ser aprimorado com atributos relacionados à pessoa real usando o serviço, seu dispositivo conectado à rede e assim por diante.

## Assinante {#subscriber-defn}

* O cliente pagante de uma MVPD ou uma pessoa que compartilha as credenciais de um cliente pagante
* Pode ser interrompido para assistir ao conteúdo pelo Serviço de monitoramento de simultaneidade, pelo aplicativo cliente usando o serviço mencionado acima.
* Na melhor das hipóteses, ele nunca nota a existência do Serviço de monitoramento de simultaneidade

## Target {#target-defn}

* Um predicado de fluxo que retornará se a regra é aplicável a um determinado fluxo. O destino implícito no CM será qualquer fluxo criado por um aplicativo que faça referência à política em questão. Além disso, as condições de valor de atributo podem ser adicionadas para ajustar a filtragem de atividade antes de aplicar as regras.

## Inquilino {#tenant-defn}

* Uma organização do cliente de Monitoramento de Simultaneidade.
