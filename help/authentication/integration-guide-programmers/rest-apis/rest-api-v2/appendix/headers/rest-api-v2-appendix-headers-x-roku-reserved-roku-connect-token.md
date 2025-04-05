---
title: Cabeçalho - X-Roku-Reserved-Roku-Connect-Token
description: REST API V2 - Cabeçalho - X-Roku-Reserved-Roku-Connect-Token
source-git-commit: 640ba7073f7f4639f980f17f1a59c4468bfebcf4
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 2%

---

# Cabeçalho - X-Roku-Reserved-Roku-Connect-Token {#header-x-roku-reserved-roku-connect-token}

>[!NOTE]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview}

O cabeçalho de solicitação <b>X-Roku-Reserved-Roku-Connect-Token</b> contém o identificador de plataforma exclusivo como `JWS` ou `JWE` obtido de um serviço de identidade ou biblioteca em execução fora dos sistemas de Autenticação Adobe Pass.

Esse cabeçalho foi projetado para ser usado em fluxos habilitados para logon único (SSO) que usam o método de identidade da plataforma.

Para obter mais detalhes sobre os fluxos habilitados para o logon único (SSO) que usam o método de Identidade da Plataforma, consulte a documentação do [Logon único usando fluxos de identidade da plataforma](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Sintaxe {#syntax}

<table style="table-layout:auto">
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Roku-Reserved-Roku-Connect-Token</b>: &lt;identificador_plataforma_exclusiva&gt;</td>
   </tr>
   <tr>
      <td>Tipo de cabeçalho</td>
      <td>Cabeçalho da solicitação</td>
   </tr>
   <tr>
      <td>Padrão</td>
      <td>Não</td>
   </tr>
</table>

## Diretivas {#directives}

<b>identificador_de_plataforma_exclusiva</b>

A Assinatura Web JSON (`JWS`) ou a Criptografia Web JSON (`JWE`), que é um Token Web JSON assinado ou criptografado (`JWT`) contendo informações de identificador de plataforma exclusivas.

Isso está disponível para as seguintes plataformas:

* [Guia do Roku SSO (REST API V2)](../../../../features-standard/sso-access/platform-sso/roku-single-sign-on/roku-sso-cookbook-rest-api-v2.md)
