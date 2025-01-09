---
title: Glossário do Dynamic Client Registration (DCR)
description: Glossário do Dynamic Client Registration (DCR)
source-git-commit: 5622cad15383560e19e8111f12a1460e9b118efe
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 0%

---

# Glossário do Dynamic Client Registration (DCR) {#rest-api-dcr-glossary}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Este documento fornece definições para termos usados ao integrar o Registro de Cliente Dinâmico (DCR) da Autenticação do Adobe Pass.

>[!MORELIKETHIS]
> 
> * [Glossário da REST API v2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md)

## Termos do Glossário {#glossary-terms}

### Um {#a}

#### Token de acesso {#access-token}

O token de acesso é um token gerado pela Autenticação Adobe Pass como resultado do processo [Registro Dinâmico de Cliente (DCR)](#dcr) destinado a garantir o acesso a APIs protegidas.

### C {#c}

#### Credenciais do cliente {#client-credentials}

As credenciais do cliente são um conjunto de valores exclusivos gerados durante o processo [Registro Dinâmico de Cliente (DCR)](#dcr) e devem ser usados para obter um [token de acesso](#access-token).

#### Esquema personalizado {#custom-scheme}

O esquema personalizado é um valor exclusivo que faz referência a um aplicativo [Programador](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer), que pode ser gerado e baixado do [Painel TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) do Adobe Pass e deve ser usado como redirecionamento final em aplicativos executados em dispositivos iOS.

### D {#d}

#### DCR {#dcr}

O Registro de Cliente Dinâmico (DCR) é um mecanismo de autorização definido por [RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591) e é baseado na estrutura de autorização OAuth 2.0 descrita por [RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749).

O DCR é entregue a um [Programador](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) como um serviço de Autenticação do Adobe Pass que pode habilitar ainda mais o acesso a APIs protegidas.

Para obter mais informações, consulte a documentação [Visão geral do registro dinâmico do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

### R {#r}

#### Aplicativo registrado {#registered-application}

O aplicativo registrado é um conceito de Autenticação do Adobe Pass que armazena informações sobre o aplicativo [Programador](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#programmer) que requer a continuação do processo [Registro de Cliente Dinâmico (DCR)](#dcr).

### S {#s}

#### Declaração de software {#software-statement}

A instrução de software é um JSON Web Token (JWT) que pode ser baixado do [Painel TVE](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-glossary.md#tve-dashboard) do Adobe Pass e deve ser usado como parte do [processo de Registro Dinâmico de Cliente (DCR)](#dcr).
