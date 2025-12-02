---
title: Notas de versão da Autenticação Adobe Pass 3.5.0
description: Saiba mais sobre os novos recursos, alterações e problemas conhecidos desta versão.
source-git-commit: 6ff46a124f5f3c78173028ae3efed68d71ee6e41
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 0%

---


# Notas de versão da Autenticação Adobe Pass 3.5.0

Última atualização: Terça-feira, 09 2025 00:00:00 GMT+0000 (Tempo Universal Coordenado)

* Tópicos
* Autenticação

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](https://experienceleague.adobe.com/pt-br/docs/pass/authentication/product-announcements).

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-350}

* [Número da Build](#build-number-350)
* [Visão geral da versão](#release-overview-350)

### Número da Build {#build-number-350}

Autenticação Adobe Pass: adobe-pass-**3.5.0**\
Data de lançamento: **12/09/2025 - 11/12/2025**

### Visão geral da versão {#release-overview-350}

#### REST API v2

* Adição de uma nova dimensão no ESM (&quot;api&quot;) para ajudar os clientes a criar relatórios que distinguiriam eventos com base no tipo de implementação: SDK, REST V1 ou REST V2.

#### Correções de erros

* Correção de um problema em que as mensagens de degradação personalizadas definidas no Painel do TVE não apareciam nos detalhes de erro da API REST V2.
* Correção de um problema na REST API V2 em que o código de erro `authenticated_profile_expired` não era retornado quando perfis autenticados expiravam.
* Correção de um problema em que os cálculos de latência de autorização e os valores TTL de comprovação estavam incorretos na REST API V2.
* Correção de um problema em que o formato de resposta inconsistente era retornado quando o token DCR expirava.
