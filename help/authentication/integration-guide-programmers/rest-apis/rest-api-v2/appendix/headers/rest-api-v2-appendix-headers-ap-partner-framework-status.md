---
title: Cabeçalho - AP-Parceiro-Estrutura-Status
description: REST API V2 - Cabeçalho - AP-Parceiro-Estrutura-Status
exl-id: f589d948-e23e-43d4-81c2-8db0e7a40e93
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 1%

---

# Cabeçalho - AP-Parceiro-Estrutura-Status {#header-ap-partner-framework-status}

>[!NOTE]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview}

O cabeçalho de solicitação <b>AP-Partner-Framework-Status</b> contém informações de status obtidas de uma estrutura do parceiro para obter o logon único (SSO).

## Sintaxe {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>AP-Partner-Framework-Status</b>: &lt;informações_de_status_do_framework_do_parceiro&gt;</td>
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

<b>&lt;informações_de_status_da_estrutura_do_parceiro></b>

O valor `Base64-encoded` do elemento JSON que contém os seguintes atributos:

<table>
   <tr>
      <th style="background-color: #EFF2F7; width: 15%;">Atributo</th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td>frameworkPermissionInfo</td>
      <td>
         Este é um atributo obrigatório.
         <br/><br/>
         As informações de status de permissões do usuário retornadas pela estrutura do parceiro e processadas pelo aplicativo.
         <br/><br/>
         Este é um elemento JSON com os seguintes atributos:
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">Atributo</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>accessStatus</td>
               <td>
                  Este é um atributo obrigatório.
                  <br/><br/>
                  É uma enumeração com os seguintes valores possíveis:
                  <br/>
                  <ul>
                     <li>granted - O usuário permitiu que o aplicativo acessasse informações de assinatura.</li>
                     <li>denied - o usuário negou o aplicativo para acessar informações de assinatura.</li>
                     <li>pendente - o usuário ainda não optou por permitir que o aplicativo acessasse as informações de assinatura.</li>
                     <li>notDetermined - O aplicativo não tem permissão para acessar informações de assinatura.</li>
                  </ul>
               </td>
            </tr>
            <tr>
               <td>erro</td>
               <td>
                  Este é um atributo opcional.
                  <br/><br/>
                  Isso pode ser usado para transmitir o erro da estrutura do parceiro, caso um seja acionado ao consultar as informações de status de permissões do usuário.
                  <br/><br/>
                  Este é um elemento JSON com os seguintes atributos:
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">Atributo</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>código</td>
                        <td>Uma string que identifica exclusivamente o erro conforme definido pela estrutura do parceiro.</td>
                     </tr>
                     <tr>
                        <td>mensagem</td>
                        <td>Uma string que contém a descrição do erro conforme definido pela estrutura do parceiro.</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
   <tr>
      <td>frameworkProviderInfo</td>
      <td>
         Este é um atributo obrigatório.
         <br/><br/>
         As informações de status de logon do provedor retornadas pela estrutura do parceiro e processadas pelo aplicativo.
         <br/><br/>
         Este é um elemento JSON com os seguintes atributos:
         <br/>
         <table>
            <tr>
               <th style="background-color: #EFF2F7; width: 15%;">Atributo</th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td>id</td>
               <td>
                  Este é um atributo obrigatório.
                  <br/><br/>
                  É a mappingId que identifica o MVPD usado durante o fluxo de autenticação no nível da estrutura do parceiro.
               </td>
            </tr>
            <tr>
               <td>expirationDate</td>
               <td>
                  Este é um atributo obrigatório.
                  <br/><br/>
                  Essa é a data de expiração do perfil do usuário autenticado, caso o usuário tenha se conectado com êxito usando um MVPD compatível no nível da estrutura do parceiro.
               </td>
            </tr>
            <tr>
               <td>erro</td>
               <td>
                  Este é um atributo opcional.
                  <br/><br/>
                  Isso pode ser usado para transmitir o erro da estrutura do parceiro, caso um seja acionado ao consultar as informações de status de logon do provedor.
                  <br/><br/>
                  Este é um elemento JSON com os seguintes atributos:
                  <br/>
                  <table>
                     <tr>
                        <th style="background-color: #EFF2F7; width: 15%;">Atributo</th>
                        <th style="background-color: #EFF2F7;"></th>
                     </tr>
                     <tr>
                        <td>código</td>
                        <td>Uma string que identifica exclusivamente o erro conforme definido pela estrutura do parceiro.</td>
                     </tr>
                     <tr>
                        <td>mensagem</td>
                        <td>Uma string que contém a descrição do erro conforme definido pela estrutura do parceiro.</td>
                     </tr>
                  </table>
               </td>
            </tr>
         </table>
      </td>
   </tr>
</table>

## Exemplos {#examples}

```JSON
// Partner framework status information
// {
//    "frameworkPermissionInfo": {
//        "accessStatus": "....",
//        "error": {
//            "code" : "....",
//            "message" : "...."
//        }
//     },
//    "frameworkProviderInfo" : {
//        "id" : "....",
//        "expirationDate" : "....",
//        "error" : {
//            "code" : "...",
//            "message" : "....."
//        }
//     }
// }  
 
// Base64-encoded
// ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAg
// ImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAg
// ICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAg
// ICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIs
// CiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
 
AP-Partner-Framework-Status: ewogICAgImZyYW1ld29ya1Blcm1pc3Npb25JbmZvIjogewogICAgICAgICJhY2Nlc3NTdGF0dXMiOiAiLi4uLiIsCiAgICAgICAgImVycm9yIjogewogICAgICAgICAgICAiY29kZSIgOiAiLi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uIgogICAgICAgIH0KICAgIH0sCiAgICAiZnJhbWV3b3JrUHJvdmlkZXJJbmZvIiA6IHsKICAgICAgICAiaWQiIDogIi4uLi4iLAogICAgICAgICJleHBpcmF0aW9uRGF0ZSIgOiAiLi4uLiIsCiAgICAgICAgImVycm9yIiA6IHsKICAgICAgICAgICAgImNvZGUiIDogIi4uLiIsCiAgICAgICAgICAgICJtZXNzYWdlIiA6ICIuLi4uLiIKICAgICAgICB9CiAgICB9Cn0gIA==
```
