---
title: Notas de versão da Autenticação Adobe Pass 3.8.0
description: Notas de versão da Autenticação Adobe Pass 3.8.0
hold: true
source-git-commit: ce9e8de3d69699d03cf68c86be1bb811967501dc
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---

# Notas de versão da Autenticação Adobe Pass 3.8.0 {#authn-380-rn}

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-380}

* [Número da Build](#build-number-380)
* [Visão geral da versão](#release-overview-380)

### Número da Build {#build-number-380}

Autenticação Adobe Pass: adobe-pass-**3.8.0**\
Data de Lançamento: **08/11/2026 - 08/13/2026**

### Visão geral da versão {#release-overview-380}

Esta versão do está focada em estabilidade, melhorias e atualizações de segurança nos serviços de autenticação da Adobe Pass.

#### Correções de erros

* Correção de um problema que causava erros HTTP 500 em APIs V2 devido a determinados caracteres inválidos em deviceId.

#### Aprimoramentos

* Manuseio de token de atualização aprimorado para suportar a renovação do token de rolagem.
* Reconhecimento de visitorId aprimorado em dispositivos secundários para análise.
* Validação aprimorada de parâmetros de URL para fortalecer os controles de segurança e melhorar a integridade geral do sistema.
* Painel TVE versão 1.5.2 com pequenas melhorias na interface.
