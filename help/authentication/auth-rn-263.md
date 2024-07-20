---
title: Notas de versão da Autenticação Adobe Pass 2.63
description: Notas de versão da Autenticação Adobe Pass 2.63
exl-id: 40987328-6d41-4948-aa4a-bab31f98a18a
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 0%

---

# Notas de versão da Autenticação Adobe Pass 2.63 {#pt-authn-263-rn}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-263}

* [Número da Build](#build-number)
* [Novos recursos](#new-features)

### Número da Build {#build-number-263}

Autenticação Adobe Pass: adobe-pass-**2.63**
Data de lançamento: **09/20/2022 - 22/09/2022**

### Novos recursos {#new-features-263}

#### Melhorar o mecanismo de identificação da plataforma {#pf-identification-mech}

* A partir desta versão, aprimoramos o mecanismo usado para identificar um dispositivo e não dependerá mais da implementação do lado do cliente. Isso fornecerá uma granularidade mais precisa ao aplicar regras de negócios no nível da plataforma e uma melhor compreensão dos valores de tráfego nos relatórios ESM.

* Uma nova versão do ESM será lançada em breve, com relatórios novos e aprimorados que expõem os campos relacionados à plataforma.

* Para obter mais detalhes sobre as alterações planejadas, entre em contato com seu TAM.

#### Autodegradação do MVPD {#mvpd-self-degradation}

Esse recurso fornece aos MVPDs a capacidade de ignorar temporariamente seus próprios endpoints de autenticação e autorização para cenários de tráfego alto quando a carga nesses respectivos endpoints se tornar muito alta.


#### Adicionar ID com proxy no cabeçalho das chamadas de autorização {#add-proxied-id}

Este recurso adiciona a ID de um MVPD com proxy Synacor no cabeçalho da chamada de autorização. Isso permite que o Synacor configure regras de negócios para cada proxy individual (por exemplo, para domínios diferentes por proxy (MVPD).


#### Painel TVE {#tve-dashboard}

Nesta versão, corrigimos um problema em que o authN ou o authZ TTL definido no nível MVPD não era calculado corretamente nos relatórios de configuração.


#### JavaScript SDK 4.6.0 {#js-sdk}

* Remoção do uso da função `eval`, tornando o SDK compatível com a Política de Segurança de Conteúdo.
* Correção de um problema que impedia a conclusão bem-sucedida do fluxo de autenticação quando o Armazenamento local do navegador era limpo explicitamente por um Aplicativo de parceiro.
