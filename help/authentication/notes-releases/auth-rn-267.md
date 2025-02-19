---
title: Notas de versão da Autenticação Adobe Pass 2.67
description: Notas de versão da Autenticação Adobe Pass 2.67
exl-id: d899fe96-a273-4681-90a5-bde54cc2f3b3
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 0%

---

# Notas de versão da Autenticação Adobe Pass 2.67 {#authn-267-rn}

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-267}

* [Número da Build](#build-number-267)
* [Visão geral da versão](#release-overview-267)

### Número da Build {#build-number-267}

Autenticação Adobe Pass: adobe-pass-**2.67.0.1**

Data de Lançamento: **09/12/2023 - 09/14/2023**

### Visão geral da versão {#release-overview-267}

* Continuação das atualizações internas para a nova API REST.
* Melhorias contínuas na arquitetura interna.

#### Atualizações do MVPD

* Atualizações da integração **DirecTV Puerto Rico** com o Adobe. Entre em contato com seu TAM para obter mais detalhes.

#### Correções de erros

* Correção de um problema que quebrava o SSO, obtido usando o cabeçalho Adobe-Subject-Token, entre aplicativos que usavam a API REST e o FireTV SDK.
