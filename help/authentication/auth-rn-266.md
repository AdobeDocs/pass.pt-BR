---
title: Notas de versão da Autenticação Adobe Pass 2.66
description: Notas de versão da Autenticação Adobe Pass 2.66
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '148'
ht-degree: 0%

---

# Notas de versão da Autenticação Adobe Pass 2.66 {#authn-266-rn}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-266}

* [Número da Build](#build-number-266)
* [Visão geral da versão](#release-overview-266)

### Número da Build {#build-number-266}

Autenticação do Adobe Pass: adobe-pass-**2.66.0.1**
Data de lançamento: **11/07/2023 - 13/07/2023**

### Visão geral da versão {#release-overview-266}

Com esta versão, continuamos a fazer atualizações internas para a nova API REST.

#### Correções de erros {#release-overview-bugfixes-266}

* Correção do fluxo de logout para MVPDs baseados em SAML, em que o parâmetro RelayState estava ausente na solicitação de logout. Direcionaremos as atualizações de configuração após o lançamento para restaurar o fluxo de logout dos MVPDs afetados.
* Adição da capacidade de atualizar certificados SSL em nossa configuração para pontos de acesso de autorização SOAP.
* Correção de um caso de exceção em que dados incorretos eram registrados no campo Programador em alguns relatórios ESM.
