---
title: Notas de versão da Autenticação Adobe Pass 2.69
description: Notas de versão da Autenticação Adobe Pass 2.69
exl-id: d031c4c5-dbd5-4a77-b298-a53b992cc4c5
source-git-commit: 8552a62f4d6d80ba91543390bf0689d942b3a6f4
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 0%

---

# Notas de versão da Autenticação Adobe Pass 2.69 {#pt-authn-269-rn}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-269}

* [Número da Build](#build-number-269)
* [Novos recursos](#new-features-269)

### Número da Build {#build-number-269}

Autenticação Adobe Pass: adobe-pass-**2.69**
Data de lançamento: **02/27/2024 - 29/02/2024**

### Novos recursos {#new-features-269}

#### Diversos {#misc}

* Vulnerabilidades de segurança corrigidas.
* Melhorias na redefinição da camada de segurança Temp Pass com o Dynamic Client Registration (DCR).
   * Encontre mais detalhes aqui: [Redefinir Temp Pass](reset-temp-pass.md)
* Melhorias nos relatórios de Identificação de plataforma.

#### REST APIs {#rest-apis}

* Desenvolvimento contínuo de novas APIs REST.
   * Uma próxima versão dedicada apresentará novos endpoints e fluxos, que serão anunciados em uma notificação separada.
   * A atualização da documentação para uso dessas novas APIs está em andamento.

#### Painel TVE {#tve-dashboard}

* Desenvolvimento contínuo para o novo Painel TVE.
   * Uma próxima versão dedicada apresentará o novo Painel TVE, que será anunciado em uma notificação separada.
   * A atualização da documentação para uso deste novo Painel TVE está em andamento.

#### JavaScript SDK 4.7.0 {#js-sdk}

* Remoção da versão obsoleta 2.0.1 do SDK do JavaScript do Access Enabler devido a vulnerabilidades de segurança.
   * Siga o link para obter mais detalhes: [Notas de versão do Adobe Pass Authentication JavaScript 4.7.0](authn-rn-javascript-470.md)
