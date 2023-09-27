---
title: Notas de versão da Autenticação Adobe Pass 2.67
description: Notas de versão da Autenticação Adobe Pass 2.67
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '125'
ht-degree: 0%

---

# Notas de versão da Autenticação Adobe Pass 2.67 {#authn-267-rn}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-267}

* [Número da Build](#build-number-267)
* [Novos recursos](#new-features-267)
* [Atualizações do MVPD](#mvpd-updates-267)
* [Correções de erros](#bug-fixes-267)

### Número da Build {#build-number-267}

Autenticação do Adobe Pass: adobe-pass-**2.67.0.1**
Data de lançamento: **12/09/2023 - 14/09/2023**

### Novos recursos {#new-features-267}

* Continuação das atualizações internas para a nova API REST.
* Melhorias contínuas na arquitetura interna.

### Atualizações do MVPD {#mvpd-updates-267}

* Atualizações para **DirecTV Porto Rico** integração com o Adobe. Entre em contato com seu TAM para obter mais detalhes.

#### Correções de erros {#bug-fixes-267}

* Correção de um problema que quebrava o SSO, obtido usando o cabeçalho Adobe-Subject-Token, entre aplicativos que usavam a API REST e o SDK FireTV.
