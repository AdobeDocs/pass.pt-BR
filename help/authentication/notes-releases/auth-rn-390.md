---
title: Notas de versão da Autenticação Adobe Pass 3.9.0
description: Notas de versão da Autenticação Adobe Pass 3.9.0
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: 9cd75fbc66d5395a899c272d94774cbaf7ea3d07
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 0%
---
# Notas de versão da Autenticação Adobe Pass 3.9.0 {#authn-390-rn}

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-390}

* [Número da Build](#build-number-390)
* [Visão geral da versão](#release-overview-390)

### Número da Build {#build-number-390}

Autenticação Adobe Pass: adobe-pass-**3.9.0.1**\
Data de Lançamento: **09/08/2026 - 10/09/2026**

### Visão geral da versão {#release-overview-390}

Essa versão do está focada em melhorias na REST API V2 e nas métricas ESM.

#### Aprimoramentos

* Aprimoramento do Logon único do parceiro REST API V2 para garantir que uma solicitação de autenticação válida seja retornada para MVPDs configurados com OAuth2.
* Decisões da REST API V2 aprimoradas para retornar uma resposta de erro clara quando a autorização falhar, em vez de uma resposta vazia.
* Geração de código de registro aprimorada para evitar caracteres visualmente ambíguos, facilitando a leitura e a inserção corretas dos códigos.
* Aprimoramentos do painel ESM com suporte para métricas de AuthZ de comprovação.
