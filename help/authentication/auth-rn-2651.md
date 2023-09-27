---
title: Notas de versão da Autenticação do Adobe Pass 2.65.1
description: Notas de versão da Autenticação do Adobe Pass 2.65.1
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 0%

---

# Notas de versão da Autenticação do Adobe Pass 2.65.1 {#authn-2651-rn}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-2651}

* [Número da Build](#build-number-2651)
* [Visão geral da versão](#release-overview-2651)

### Número da Build {#build-number-2651}

Autenticação do Adobe Pass: adobe-pass-**2.65.1**
Data de lançamento: **20/06/2023 - 22/06/2023**

### Visão geral da versão {#release-overview-2651}

Esta versão adiciona o seguinte:

A partir desta versão, introduziremos suporte técnico para adicionar regras de limitação de taxa em todas as APIs, a fim de proteger o ecossistema geral contra o uso incorreto.

O objetivo inicial será monitorar o comportamento dos aplicativos para estabelecer os valores-limite adequados, e nenhum limite será aplicado como parte desta versão. Para casos em que o tráfego gerado por um aplicativo específico excede determinados limites, trabalharemos com cada cliente para validar a implementação.

As regras de limitação de taxa serão aplicadas no final deste ano, após validarmos o tráfego gerado de todos os aplicativos do cliente.
