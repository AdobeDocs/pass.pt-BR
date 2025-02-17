---
title: Notas de versão da Autenticação Adobe Pass 2.63
description: Notas de versão da Autenticação Adobe Pass 2.63
exl-id: 40987328-6d41-4948-aa4a-bab31f98a18a
source-git-commit: 134a9a13373717ff7772a9d765bbd7b3b4943a85
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 0%

---

# Notas de versão da Autenticação Adobe Pass 2.63 {#authn-263-rn}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-263}

* [Número da Build](#build-number-263)
* [Visão geral da versão](#release-overview-263)

### Número da Build {#build-number-263}

Autenticação Adobe Pass: adobe-pass-**2.63**

Data de Lançamento: **09/20/2022 - 22/09/2022**

### Visão geral da versão {#release-overview-263}

#### Melhorar o mecanismo de identificação da plataforma

* A partir desta versão, aprimoramos o mecanismo usado para identificar um dispositivo e não dependerá mais da implementação do lado do cliente. Isso fornecerá uma granularidade mais precisa ao aplicar regras de negócios no nível da plataforma e uma melhor compreensão dos valores de tráfego nos relatórios ESM.

* Uma nova versão do ESM será lançada em breve, com relatórios novos e aprimorados que expõem os campos relacionados à plataforma.

* Para obter mais detalhes sobre as alterações planejadas, entre em contato com seu TAM.

#### Autodegradação do MVPD

Esse recurso fornece aos MVPDs a capacidade de ignorar temporariamente seus próprios endpoints de autenticação e autorização para cenários de tráfego alto quando a carga nesses respectivos endpoints se tornar muito alta.

#### Adicionar ID com proxy no cabeçalho das chamadas de autorização

Este recurso adiciona a ID de um MVPD com proxy Synacor no cabeçalho da chamada de autorização. Isso permite que o Synacor configure regras de negócios para cada proxy individual (por exemplo, para domínios diferentes por proxy (MVPD).

#### Painel TVE

Nesta versão, corrigimos um problema em que o TTL de authN ou authZ definido no nível da MVPD não era calculado corretamente nos relatórios de configuração.

#### JavaScript SDK 4.6.0

* Remoção do uso da função `eval`, tornando o SDK compatível com a Política de Segurança de Conteúdo.
* Correção de um problema que impedia a conclusão bem-sucedida do fluxo de autenticação quando o Armazenamento local do navegador era limpo explicitamente por um Aplicativo de parceiro.
