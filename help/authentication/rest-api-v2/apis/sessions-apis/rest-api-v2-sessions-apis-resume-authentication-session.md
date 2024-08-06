---
title: Retomar sessão de autenticação
description: REST API V2 - Retomar sessão de autenticação
source-git-commit: dc9fab27c7eced2be5dd9f364ab8f2d64f8e4177
workflow-type: tm+mt
source-wordcount: '753'
ht-degree: 1%

---


# Retomar sessão de autenticação {#resume-authentication-session}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> A implementação da REST API V2 é limitada pela documentação do [Mecanismo de limitação](/help/authentication/throttling-mechanism.md).

## Solicitação {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">caminho</td>
      <td>/api/v2/{serviceProvider}/sessions/{code}</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parâmetros de caminho</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">serviceProvider</td>
      <td>O identificador exclusivo interno associado ao provedor de serviços durante o processo de integração.</td>
      <td><i>obrigatório</i></td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;">código</td>
      <td>O código de autenticação obtido após a criação da sessão de autenticação no dispositivo de streaming.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parâmetros de corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd</td>
      <td>
        O identificador exclusivo interno associado ao Provedor de identidade durante o processo de integração.
        <br/><br/>
        Se a plataforma do dispositivo de transmissão tiver limitações no fornecimento de um valor, um aplicativo terá que retomar a sessão de autenticação e fornecer um valor válido.
      </td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">domainName</td>
      <td>
        O domínio de origem do aplicativo que executa o logon MVPD.
        <br/><br/>
        Se a plataforma do dispositivo de transmissão tiver limitações no fornecimento de um valor, um aplicativo terá que retomar a sessão de autenticação e fornecer um valor válido.
      </td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">redirectUrl</td>
      <td>
        O URL final de redirecionamento para o qual o agente do usuário navega quando o fluxo de autenticação do MVPD é concluído.
        <br/><br/>
        O valor deve ser codificado em URL.
        <br/><br/>
        Se a plataforma do dispositivo de transmissão tiver limitações no fornecimento de um valor, um aplicativo terá que retomar a sessão de autenticação e fornecer um valor válido.
        </td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorização</td>
      <td>A geração da carga do token de portador está descrita na documentação do <a href="../../../dynamic-client-registration-api.md">Registro de Cliente Dinâmico</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Tipo de conteúdo</td>
      <td>
         O tipo de mídia aceito para os recursos que estão sendo enviados.
         <br/><br/>
         Deve ser application/x-www-form-urlencoded.
      </td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Encaminhado-Para</td>
      <td>
         O endereço IP do dispositivo de streaming.
         <br/><br/>
         É altamente recomendável sempre usá-lo para implementações de servidor para servidor, especialmente quando a chamada é feita pelo serviço do programador, em vez do dispositivo de transmissão.
         <br/><br/>
         Para implementações de cliente para servidor, o endereço IP do dispositivo de transmissão é enviado implicitamente.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Aceitar</td>
      <td>
         O tipo de mídia aceito pelo aplicativo cliente.
         <br/><br/>
         Se especificado, deve ser application/json.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">User-Agent</td>
      <td>O agente do usuário do aplicativo cliente.</td>
      <td>opcional</td>
   </tr>
</table>

## Resposta {#response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Código</th>
      <th style="background-color: #EFF2F7;">Texto</th>
      <th style="background-color: #EFF2F7;">Descrição</th>
   </tr>
   <tr>
      <td>200</td>
      <td>OK</td>
      <td>
        O corpo da resposta contém informações sobre as próximas ações necessárias para executar a autenticação.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Solicitação inválida</td>
      <td>
        A solicitação é inválida, o cliente precisa corrigir a solicitação e tentar novamente. O corpo da resposta pode conter informações de erro que seguem a documentação de <a href="../../../enhanced-error-codes.md">Códigos de erro aprimorados</a>.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Não autorizado</td>
      <td>
        O token de acesso é inválido, o cliente precisa obter um novo token de acesso e tentar novamente. Para obter mais detalhes, consulte a documentação do <a href="../../../dynamic-client-registration-api.md">Registro de Cliente Dinâmico</a>.
      </td>
   </tr>
   <tr>
      <td>405</td>
      <td>Método não permitido</td>
      <td>
        O método HTTP é inválido, o cliente precisa usar um método HTTP permitido para o recurso solicitado e tentar novamente. Para obter mais detalhes, consulte a seção <a href="#request">Solicitação</a>.
      </td>
   </tr>
   <tr>
      <td>500</td>
      <td>Erro interno do servidor</td>
      <td>
        O servidor encontrou um problema. O corpo da resposta pode conter informações de erro que seguem a documentação de <a href="../../../enhanced-error-codes.md">Códigos de erro aprimorados</a>.
      </td>
   </tr>
</table>

### Sucesso {#success}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>200</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"></td>
      <td>
         Objeto JSON com os seguintes atributos:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Atributo</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionName</td>
               <td>
                  A ação que o dispositivo de streaming precisa executar para concluir o fluxo de autenticação.
                  <br/><br/>
                  Os valores possíveis são:
                  <ul>
                    <li><b>autenticar</b><br/>O dispositivo de streaming ou outro dispositivo precisa abrir a URL fornecida em um agente de usuário.</li>
                    <li><b>repetir</b><br/>O dispositivo de streaming ou outro dispositivo precisa fornecer os parâmetros ausentes e tentar retomar a sessão de autenticação usando o código.</li>
                    <li><b>autorizar</b><br/>O dispositivo de streaming pode continuar diretamente com fluxos de decisões.</li>
                  </ul> 
               <td><i>obrigatório</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">actionType</td>
               <td>
                  O tipo de interação que o dispositivo de streaming deve executar para continuar o fluxo com a ação especificada pelo atributo "actionName".
                  <br/><br/>
                  Os valores possíveis são:
                  <ul>
                    <li><b>interativo</b><br/>O fluxo continua com uma navegação até a URL fornecida usando um agente de usuário.</li>
                    <li><b>direct</b><br/>O fluxo continua com uma chamada direta para a URL fornecida usando um cliente HTTP disponível para a implementação do cliente.</li>
                  </ul>
               <td><i>obrigatório</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">missingParameters</td>
               <td>Os parâmetros ausentes que precisam ser fornecidos para concluir o fluxo de autenticação básico.</td>
               <td>opcional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">url</td>
               <td>O URL no qual o aplicativo cliente precisa navegar.</td>
               <td>opcional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">código</td>
               <td>O código de autenticação que pode ser usado em um aplicativo secundário para retomar a sessão de autenticação.</td>
               <td><i>obrigatório</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">sessionId</td>
               <td>O identificador opaco que pode ser usado para rastrear a atividade do usuário.</td>
               <td><i>obrigatório</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>O identificador exclusivo interno associado ao Provedor de identidade durante o processo de integração.</td>
               <td>opcional</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">serviceProvider</td>
               <td>O identificador exclusivo interno associado ao provedor de serviços durante o processo de integração.</td>
               <td><i>obrigatório</i></td>
            </tr>
         </table>
      </td>
      <td><i>obrigatório</i></td>
</table>

### Erro {#error}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Status</td>
      <td>400, 401, 405, 500</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Tipo de conteúdo</td>
      <td>application/json</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">erro</td>
      <td>O erro fornece informações adicionais que seguem a documentação de <a href="../../../enhanced-error-codes.md">Códigos de erro aprimorados</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
</table>

## Amostras {#samples}

### 1. Retomar sessão de autenticação sem fornecer valores para todos os parâmetros necessários

>[!BEGINTABS]

>[!TAB Solicitação]

```JSON
POST /api/v2/REF30/sessions/8BLW4RW
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
 
Body:

mvpd=Cablevision&domainName=adobe.com
```

>[!TAB Resposta]

```JSON
HTTP/1.1 200 OK
 
{           
   "actionName" : "retry",
   "actionType" : "interactive",
   "url" : "/v2/REF30/sessions/8BLW4RW",
   "missingParameters" : ["redirectUrl"]
   "code" : "8BLW4RW",
   "sessionId" : "3453453354jkopey",
   "mvpd" : "Cablevision",
   "serviceProvider" : "REF30"
}
```

>[!ENDTABS]
