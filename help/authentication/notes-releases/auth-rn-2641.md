---
title: Notas de versão da Autenticação do Adobe Pass 2.64.1
description: Notas de versão da Autenticação do Adobe Pass 2.64.1
exl-id: b0edbd90-ebb5-40a7-9034-1699dccfadb5
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '137'
ht-degree: 0%

---

# Notas de versão da Autenticação do Adobe Pass 2.64.1 {#authn-264-rn}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-2641}

* [Número da Build](#build-number-2641)
* [Visão geral da versão](#release-overview-2641)

### Número da Build {#build-number-2641}

Autenticação Adobe Pass: adobe-pass-**2.64.1**
Data de Lançamento: **01/31/2023 - 02/02/2023**

### Visão geral da versão {#release-overview-2641}

Esta versão adiciona o seguinte:

* A capacidade de bloquear respostas de autenticação não solicitadas de MVPDs em que o parâmetro &quot;in_response_to&quot; está ausente da asserção SAML.
* Uma validação aprimorada sobre o parâmetro redirect_url para atender aos requisitos de segurança.
* Um registro aprimorado das informações do dispositivo para solicitações de autenticação de segunda tela, usando as informações fornecidas pelo dispositivo inicial.
