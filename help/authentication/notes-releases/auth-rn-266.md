---
title: Notas de versão da Autenticação Adobe Pass 2.66
description: Notas de versão da Autenticação Adobe Pass 2.66
exl-id: 7c3cd007-ed2b-455f-8f70-6ec5d0a6552a
source-git-commit: ecafc3a92f691203d8113a741f0b6cd00a134e80
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---

# Notas de versão da Autenticação Adobe Pass 2.66 {#authn-266-rn}

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-266}

* [Número da Build](#build-number-266)
* [Visão geral da versão](#release-overview-266)

### Número da Build {#build-number-266}

Autenticação Adobe Pass: adobe-pass-**2.66.0.1**

Data de Lançamento: **07/11/2023 - 07/13/2023**

### Visão geral da versão {#release-overview-266}

Com esta versão, continuamos a fazer atualizações internas para a nova API REST.

#### Correções de erros

* Correção do fluxo de logout para MVPDs baseados em SAML, em que o parâmetro RelayState estava ausente na solicitação de logout. Direcionaremos as atualizações de configuração após o lançamento para restaurar o fluxo de logout dos MVPDs afetados.
* Adição da capacidade de atualizar certificados SSL em nossa configuração para endpoints de autorização do SOAP.
* Correção de um caso de exceção em que dados incorretos eram registrados no campo Programador em alguns relatórios ESM.
