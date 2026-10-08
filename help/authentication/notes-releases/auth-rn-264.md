---
title: Notas de versão da Autenticação Adobe Pass 2.64
description: Notas de versão da Autenticação Adobe Pass 2.64
exl-id: 4db21026-a0c2-4e33-b01f-4ccae824a110
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%
---
# Notas de versão da Autenticação Adobe Pass 2.64 {#authn-264-rn}

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-264}

* [Número da Build](#build-number-264)
* [Visão geral da versão](#release-overview-264)

### Número da Build {#build-number-264}

Autenticação Adobe Pass: adobe-pass-**2.64**

Data de Lançamento: **11/08/2022 - 10/11/2022**

### Visão geral da versão {#release-overview-264}

* Atualizações da infraestrutura, destinadas a reduzir os tempos de resposta do servidor, melhorando o desempenho geral do sistema.
* Melhorias no novo mecanismo de identificação da plataforma.
* Capacidade de bloquear respostas de autenticação não solicitadas de MVPDs em que o parâmetro &quot;in_response_to&quot; está ausente da asserção SAML.

#### Correções de erros

* Correção da formatação incorreta de alguns tokens TempPass herdados.
* Problemas secundários foram solucionados no fluxo de autenticação da segunda tela.
