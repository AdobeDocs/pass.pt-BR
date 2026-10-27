---
title: Notas de versão da Autenticação Adobe Pass 3.10.0
description: Notas de versão da Autenticação Adobe Pass 3.10.0
product_v2:
  - id: f002a92a-b99f-47a4-90c8-65e0e415bc7a
    internal-label: Pass
source-git-commit: d60b5256f5809b52122d70d04eb873018707fcaf
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 0%
---
# Notas de versão da Autenticação Adobe Pass 3.10.0 {#authn-3100-rn}

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-3100}

* [Número da Build](#build-number-3100)
* [Visão geral da versão](#release-overview-3100)

### Número da Build {#build-number-3100}

Autenticação Adobe Pass: adobe-pass-**3.10.0**\
Data de Lançamento: **27/10/2026 - 29/10/2026**

### Visão geral da versão {#release-overview-3100}

Esta versão do está focada em controles de Logon único, melhorias de metadados do usuário e melhorias de confiabilidade para a REST API V2 e o gerenciamento de configuração do MVPD.

#### Aprimoramentos

* Adição de controle em nível de integração para o Logon único (SSO) do token de serviço. O SSO do token de serviço é usado somente quando habilitado explicitamente para a integração.
* Adição de suporte para metadados de usuário personalizados (por exemplo, `packageType`) durante autenticação, autorização e comprovação. As decisões de autorização e comprovação da REST API V2 agora incluem metadados de usuário configurados, incluindo respostas em cache, respeitando as configurações de acesso e criptografia de metadados.

#### Correções de erros

* Correção de um problema que resultava em falha de solicitações de perfil REST API V2 Apple SSO com um erro de resposta SAML inválido para integrações usando MVPDs OAuth2 por meio de SAML.
* Correção de um problema em que as decisões de comprovação e autorização para o mesmo recurso podiam substituir os tempos de expiração do cache e os metadados do usuário entre si.
* Correção de um problema em que a exclusão de MVPDs com proxy deixava configurações de plataforma sem referência que impediam salvamentos de configuração subsequentes.
