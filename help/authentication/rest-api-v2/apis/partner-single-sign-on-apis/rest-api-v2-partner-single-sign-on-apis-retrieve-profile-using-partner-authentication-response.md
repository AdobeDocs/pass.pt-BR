---
title: Recuperar perfil usando resposta de autenticação de parceiro
description: REST API V2 - Recuperar perfil usando resposta de autenticação de parceiro
source-git-commit: dc9fab27c7eced2be5dd9f364ab8f2d64f8e4177
workflow-type: tm+mt
source-wordcount: '729'
ht-degree: 2%

---


# Recuperar perfil usando resposta de autenticação de parceiro {#retrieve-profile-using-partner-authentication-response}

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
      <td>/api/v2/{serviceProvider}/profiles/sso/{partner}</td>
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
      <td style="background-color: #DEEBFF;">parceiro</td>
      <td>O nome do parceiro (por exemplo, Apple) que fornece a estrutura de logon único integrada aos fluxos de autenticação da Adobe Pass.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parâmetros de corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">SAMLResponse</td>
      <td>
        A resposta de autenticação de parceiro que contém os metadados de usuário necessários para criar e salvar um perfil de parceiro.
        <br/><br/>
        O valor deve ser codificado em Base64 e depois codificado em URL.
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
      <td style="background-color: #DEEBFF;">Identificador de dispositivo AP</td>
      <td>A geração da carga do identificador de dispositivo está descrita na documentação <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         A geração da carga de informações do dispositivo está descrita na documentação <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a>.
         <br/><br/>
         É altamente recomendável sempre usá-lo quando a plataforma do dispositivo do aplicativo permitir a provisão explícita de valores válidos.
         <br/><br/>
         Quando fornecido, o back-end da Autenticação do Adobe Pass mesclará explicitamente valores definidos com valores extraídos implicitamente (por padrão).
         <br/><br/>
         Quando não for fornecido, o back-end da Autenticação do Adobe Pass usará os valores extraídos implicitamente (por padrão).
      </td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Partner-Framework-Status</td>
      <td>
        A geração da carga de logon único para o método Partner está descrita na documentação <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a>.
        <br/><br/>
        Para obter mais detalhes sobre os fluxos habilitados para logon único usando um parceiro, consulte a documentação do <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md">Logon único usando fluxos do parceiro</a>.</td>
      <td>opcional</td>
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
      <td>201</td>
      <td>Criado em</td>
      <td>
        O corpo da resposta contém um mapa de perfis válidos, que pode estar vazio.
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
      <td>201</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Tipo de conteúdo</td>
      <td>application/json</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Corpo</th>
      <th style="background-color: #EFF2F7"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">perfis</td>
      <td>
         JSON que contém um mapa de pares de chaves e valores.
         <br/><br/>
         O elemento principal é definido pelo seguinte valor:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Valor</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">mvpd</td>
               <td>O identificador exclusivo interno associado ao Provedor de identidade durante o processo de integração.</td>
               <td><i>obrigatório</i></td>
            </tr>
         </table>
         O elemento value é definido pelos seguintes atributos:
         <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Atributo</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notBefore</td>
               <td>O carimbo de data/hora antes do qual o perfil não é válido.</td>
               <td><i>obrigatório</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">notAfter</td>
               <td>O carimbo de data/hora após o qual o perfil não é válido.</td>
               <td><i>obrigatório</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">emissor</td>
               <td>
                  A entidade proprietária do perfil.
                  <br/><br/>
                  Os valores possíveis são:
                  <ul>
                    <li><b>Apple</b><br/>O perfil foi criado como resultado de: logon único usando Apple parceiro.</li>
                  </ul>
               </td>
               <td><i>obrigatório</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">type</td>
               <td>
                  O tipo do perfil.
                  <br/><br/>
                  Os valores possíveis são:
                  <ul>
                    <li><b>appleSSO</b><br/>O perfil foi criado como resultado de: logon único usando o Apple parceiro.</li>
                  </ul>
               </td>
               <td><i>obrigatório</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">atributos</td>
               <td>
                    A lista de atributos de metadados do usuário.
                    <br/><br/>
                    Esses atributos podem ser:
                    <ul>
                        <li>Obrigatório, como "userId"</li>
                        <li>Não obrigatório, como "zip", "householdId", "maxRating" etc.</li>
                    </ul>
                    Os valores dos atributos podem ser:
                    <ul>
                        <li>simples</li>
                        <li>lista</li>
                        <li>mapa</li>
                    </ul>
               </td>
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

### 1. Apple SSO habilitado e resposta SAML válida

>[!BEGINTABS]

>[!TAB Solicitação]

```JSON
POST /api/v2/REF30/profiles/sso/Apple
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

SAMLResponse=PHNhbWxwOlJlc3BvbnNlIHhtbG5zOnNhbWxwPSJ1cm46b2FzaXM6bmFtZXM6dGM6U0FNTDoyLjA6cHJvdG9jb2wiIH...
```

>[!TAB Resposta]

```JSON
HTTP/1.1 200 OK
 
{
    "profiles" : {
        "Cablevision" : {
            "notBefore" : 1623943955,
            "notAfter" : 1623951155,
            "issuer" : "Apple",
            "type" : "appleSSO",
            "attributes" : {
                "userId" : {
                    "value" : "BASE64_value_userId",
                    "state" : "plain"
                },
                "householdId" : {
                    "value" : "BASE64_value_householdId",
                    "state" : "plain"
                },
                "zip" : {
                    "value" : "BASE64_value_zip",
                    "state" : "enc"
                }       
            }
        }
     }
}  
```

>[!ENDTABS]

### 2. Perfil AppleSSO para uma integração degradada

>[!BEGINTABS]

>[!TAB Solicitação]

```JSON
POST /api/v2/REF30/profiles/sso/Apple HTTP/1.1
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:

SAMLResponse=PHNhbWxwOlJlc3BvbnNlIHhtbG5zOnNhbWxwPSJ1cm46b2FzaXM6bmFtZXM6dGM6U0FNTDoyLjA6cHJvdG9jb2wiIH...
```

>[!TAB Resposta]

```JSON
HTTP/1.1 200 OK
 
{
   "profiles":{
      "WOW":{
         "notBefore":1706636062704,
         "notAfter":1706696062704,
         "issuer":"Adobe",
         "type":"degraded",
         "attributes":{
            "userID":{
               "value":"95cf93bcd183214ac9e4433153cb8a9d180a463128c0a5d26f202e8c",
               "state":"plain"
            }
         }
      }
   }
}
```

>[!ENDTABS]

### 3. Fluxo de SSO do Apple quando a resposta SAML não é válida

>[!BEGINTABS]

>[!TAB Solicitação]

```JSON
POST /api/v2/REF30/profiles/sso/Apple HTTP/1.1 
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info: ....
AP-Partner-Framework-Status: ewogICAidXNlcl9wZXJtaXNzaW9ucyIgOiB7fSwKICAgIm12cGRfc3RhdHVzIiA6IHt9Cn0=
Content-Type: application/x-www-form-urlencoded
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)

Body:
        
SAMLResponse=PHNhbWxwOlJlc3BvbnNlIHhtbG5zOnNhbWxwPSJ1cm46b2FzaXM6bmFtZXM6dGM6U0FNTDoyLjA6cHJvdG9jb2wiIH...
```

>[!TAB Resposta]

```JSON
HTTP/1.1 403 OK
Content-Type: application/json; charset=utf-8
    
{
    "errors" : [
        {
            "code": "invalid_mvpd_response",
            "message": "The saml mvpd response is not valid",
            "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
            "action": "none"        
        } 
    ]
}   
```

>[!ENDTABS]
