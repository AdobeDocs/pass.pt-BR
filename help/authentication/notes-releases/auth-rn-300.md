---
title: Notas de versão da Autenticação Adobe Pass 3.0
description: Notas de versão da Autenticação Adobe Pass 3.0
exl-id: 9284151a-8458-44a3-937b-35f379ca0e4e
source-git-commit: b0d6c94148b2f9cb8a139685420a970671fce1f5
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 0%

---

# Notas de versão da Autenticação Adobe Pass 3.0 {#pt-authn-300-rn}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Esta página descreve novos recursos, alterações e problemas conhecidos com esta versão:

## Lado do servidor e clientes da Web {#server-side-web-clients-300}

* [Número da Build](#build-number-300)
* [Visão geral da versão](#release-overview-300)

### Número da Build {#build-number-2651}

Autenticação Adobe Pass: adobe-pass-**3.0**
Data de lançamento: **09/10/2024 - 09/12/2024**

### Novos recursos {#new-features-300}

#### REST API v2 {#rest-apis}

##### Código

* A partir do adobe-pass-**3.0**, os aplicativos clientes novos e existentes podem se integrar ou migrar para a nova API REST v2 para se beneficiar das funcionalidades mais recentes do Adobe Pass.

##### Documentação

* Para começar com a nova REST API v2, consulte os seguintes documentos:
   * [REST API v2 - APIs - Visão geral](../integration-guide-programmers/rest-apis/rest-api-v2/apis/rest-api-v2-apis-overview.md)
   * [REST API v2 - Fluxos - Visão geral](../integration-guide-programmers/rest-apis/rest-api-v2/flows/rest-api-v2-flows-overview.md)
* Os URLs de documentos públicos da REST API v1 foram alterados, consulte os seguintes documentos:
   * [REST API v1 - APIs - Visão geral](../integration-guide-programmers/legacy/rest-api-v1/rest-api-overview.md)
   * [REST API v1 - APIs - Referência](../integration-guide-programmers/legacy/rest-api-v1/rest-api-reference.md)

##### Ferramentas

* Para experimentar a nova API REST v2, consulte a nova página Autenticação do Adobe Pass do site [Adobe Developer](https://developer.adobe.com/adobe-pass).

### Correções de erros {#bug-fixes-300}

* Correção de um problema em que o parâmetro de URL de redirecionamento não era usado quando presente na solicitação de logout.
