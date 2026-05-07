---
title: Notas de versão da Autenticação Adobe Pass 3.7.0
description: Notas de versão da Autenticação Adobe Pass 3.7.0
source-git-commit: 89b5fbd8e8510cbf84ce7908e8cf86551e7a0cb9
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---

# Notas de versão da Autenticação Adobe Pass 3.7.0 {#authn-370-rn}

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-370}

* [Número da Build](#build-number-370)
* [Visão geral da versão](#release-overview-370)

### Número da Build {#build-number-370}

Autenticação Adobe Pass: adobe-pass-**3.7.0.2**\
Data de Lançamento: **05/12/2026 - 14/05/2026**

### Visão geral da versão {#release-overview-370}

Esta versão do está focada em atualizações de integração do MVPD, correções de erros e melhorias no Painel TVE.

#### Integrações do MVPD

* Adição de suporte a PKCE para autenticação do MVPD com base em OAuth2.

#### Aprimoramentos

* Lançamento do painel TVE versão 1.5.1.

#### Correções de erros

* Correção de um problema que fazia com que o Apple SSO ignorasse determinadas incompatibilidades de configuração do MVPD.
* Correção de um problema que causava erros HTTP 500 na negação de autorização devido a caracteres inválidos no cabeçalho de resposta.
