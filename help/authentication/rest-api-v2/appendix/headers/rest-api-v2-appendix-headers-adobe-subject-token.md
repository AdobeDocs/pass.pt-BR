---
title: Cabeçalho - Adobe-Subject-Token
description: REST API V2 - Cabeçalho - Adobe-Subject-Token
exl-id: 906d88f4-3b8f-491a-ab58-8e63d3b958d8
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '166'
ht-degree: 2%

---

# Cabeçalho - Adobe-Subject-Token {#header-adobe-subject-token}

>[!NOTE]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview}

O cabeçalho de solicitação <b>Adobe-Subject-Token</b> contém o identificador de plataforma exclusivo como `JWS` ou `JWE` obtido de um serviço de identidade ou biblioteca em execução fora dos sistemas de Autenticação Adobe Pass.

Esse cabeçalho foi projetado para ser usado em fluxos habilitados para logon único (SSO) que usam o método de identidade da plataforma.

Para obter mais detalhes sobre os fluxos habilitados para o logon único (SSO) que usam o método de Identidade da Plataforma, consulte a documentação do [Logon único usando fluxos de identidade da plataforma](../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Sintaxe {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>Token de Assunto do Adobe</b>: &lt;identificador_plataforma_exclusiva&gt;</td>
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

* [Guia do Amazon SSO (REST API V2)](../../../single-sign-on/platform-single-sign-on/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v1.md)

## Exemplos {#examples}

Consulte os exemplos descritos para as seguintes plataformas:

* [Guia do Amazon SSO (REST API V2)](../../../single-sign-on/platform-single-sign-on/amazon-single-sign-on/amazon-sso-cookbook-rest-api-v1.md)
