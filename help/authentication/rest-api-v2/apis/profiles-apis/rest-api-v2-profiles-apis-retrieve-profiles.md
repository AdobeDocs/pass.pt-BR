---
title: Recuperar perfis
description: REST API V2 - Recuperar perfis
source-git-commit: 3cff9d143eedb35155aa06c72d53b951b2d08d39
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 1%

---


# Recuperar perfis {#retrieve-profiles}

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
      <td>/api/v2/{serviceProvider}/profiles</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>GET</td>
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
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorização</td>
      <td>A geração da carga do token do portador está descrita na documentação do cabeçalho <a href="../../appendix/headers/rest-api-v2-appendix-headers-authorization.md">Autorização</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Identificador de dispositivo AP</td>
      <td>A geração da carga do identificador de dispositivo está descrita na documentação do cabeçalho <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         A geração da carga de informações do dispositivo está descrita na documentação do cabeçalho <a href="../../appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a>.
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
      <td style="background-color: #DEEBFF;">Adobe-Subject-Token</td>
      <td>
        A geração da carga de logon único para o método de identidade da plataforma está descrita na documentação do cabeçalho <a href="../../appendix/headers/rest-api-v2-appendix-headers-adobe-subject-token.md">Adobe-Subject-Token</a>.
        <br/><br/>
        Para obter mais detalhes sobre os fluxos habilitados para logon único usando uma identidade de plataforma, consulte a documentação do <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md">Logon único usando fluxos de identidade de plataforma</a>.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AD-Service-Token</td>
      <td>
        A geração da carga de logon único para o método Token de Serviço está descrita na documentação do cabeçalho <a href="../../appendix/headers/rest-api-v2-appendix-headers-ad-service-token.md">AD-Service-Token</a>.
        <br/><br/>
        Para obter mais detalhes sobre os fluxos habilitados para logon único usando um token de serviço, consulte a documentação do <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-service-token-flows.md">Logon único usando fluxos de token de serviço</a>.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">AP-Partner-Framework-Status</td>
      <td>
        A geração da carga de logon único para o método Partner está descrita na documentação do cabeçalho <a href="../../appendix/headers/rest-api-v2-appendix-headers-ap-partner-framework-status.md">AP-Partner-Framework-Status</a>.
        <br/><br/>
        Para obter mais detalhes sobre os fluxos habilitados para logon único usando um parceiro, consulte a documentação do <a href="../../flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-partner-flows.md">Logon único usando fluxos do parceiro</a>.</td>
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
        O token de acesso é inválido, o cliente precisa obter um novo token de acesso e tentar novamente. Para obter mais detalhes, consulte a documentação <a href="../../../dcr-api/dynamic-client-registration-overview.md">Visão geral do registro dinâmico do cliente</a>.
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
                    <li><b>mvpd (por exemplo, Spectrum, Cablevision, etc.)</b><br/>O perfil foi criado como resultado de: autenticação básica, logon único usando a identidade da plataforma ou logon único usando o token de serviço.</li>
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
                    <li><b>regular</b><br/>O perfil foi criado como resultado de: autenticação básica.</li>
                    <li><b>appleSSO</b><br/>O perfil foi criado como resultado de: logon único usando o Apple parceiro.</li>
                    <li><b>platformSSO</b><br/>O perfil foi criado como resultado de: logon único usando a identidade da plataforma.</li>
                    <li><b>serviceTokenSSO</b><br/>O perfil foi criado como resultado de: logon único usando token de serviço.</li>
                  </ul>
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

### 1. Recuperar todos os perfis autenticados existentes e válidos obtidos por autenticação básica

>[!BEGINTABS]

>[!TAB Solicitação]

```JSON
GET /api/v2/REF30/profiles
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB Resposta]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
 
{
    "profiles" : {
        "Cablevision" : {
            "notBefore" : 1623943955,
            "notAfter" : 1623951155,
            "issuer" : "Cablevision",
            "type" : "regular",
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
                },
                "parental-controls" : {
                    "value" : BASE64_value_parental-controls,
                    "state" : "plain"
                }          
            }
        },
        "Spectrum" : {
            "notBefore" : 1623943955,
            "notAfter" : 1623951155,
            "issuer" : "Spectrum",
            "type" : "regular",
            "attributes" : {
                "userId" : {
                    "value" : "BASE64_value_userId",
                    "state" : "plain"
                }
            }
        }
     }
}
```

>[!ENDTABS]

### 2. Recupere todos os perfis autenticados existentes e válidos, incluindo os obtidos por autenticação de logon único usando o método Token de Serviço

>[!BEGINTABS]

>[!TAB Solicitação]

```JSON
GET /api/v2/REF30/profiles
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
AD-Service-Token : eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJkZDNmYWIyN2NmMjg0ZmU2ZWU0ZDY3ZmExZjY4MzE3YyIsImlzcyI6IkFkb2JlIiw.....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB Resposta]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
   "profiles": {
      "AdobeShibboleth": {
         "notBefore": 1748073636999,
         "notAfter": 1748105173000,
         "issuer": "AdobeShibboleth",
         "type": "serviceTokenSSO",
         "attributes": {
            "upstreamUserID": {
               "value": "AAdzZWNyZXQxydCkywfPBl0KExk8OWhdbUBVDDJBttfKD7RAcRlc32Pbuwd1...",
               "state": "plain"
            },
            "userID": {
               "value": "AAdzZWNyZXQxydCkywfPBl0KExk8OWhdbUBVDDJBttfKD7RAcRlc32Pbuwd14aTV....",
               "state": "plain"
            },
            "mvpd": {
               "value": "AdobeShibboleth",
               "state": "plain"
            }
         }
      },
      "Spectrum": {
         "notBefore": 1623943955,
         "notAfter": 1623951155,
         "issuer": "Spectrum",
         "type": "regular",
         "attributes": {
            "userId": {
               "value": "BASE64_value_userId",
               "state": "plain"
            }
         }
      }
   }
}
```

>[!ENDTABS]

### 3. Recupere todos os perfis autenticados existentes e válidos, incluindo os obtidos por autenticação de logon único usando o método de identidade da plataforma

>[!BEGINTABS]

>[!TAB Solicitação]

```JSON
GET /api/v2/REF30/profiles
 
Authorization: Bearer ....
AP-Device-Identifier: fingerprint YmEyM2QxNDEtZDcxNS01NjFjLTk0ZjQtZTllNGM5NjZiMWVi
X-Device-Info ....
Adobe-Subject-Token : eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiIyMmM4MDU1MjEzMDIwYzhmZGYzOGZkMTI1YWViMzUzYSIsImlzcyI6....
Accept: application/json
User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 14.5 like Mac OS X; en_US)
```

>[!TAB Resposta]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8    
 
{
   "profiles": {
      "AdobePass_SMI": {
         "notBefore": 1724337476000,
         "notAfter": 1724345252000,
         "issuer": "AdobePass_SMI",
         "type": "platformSSO",
         "attributes": {
            "upstreamUserID": {
               "value": "38524bdc3d1caac0b3e139003ea0954e15ad9648",
               "state": "plain"
            },
            "userID": {
               "value": "38524bdc3d1caac0b3e139003ea0954e15ad9648",
               "state": "plain"
            },
            "mvpd": {
               "value": "AdobePass_SMI",
               "state": "plain"
            }
         }
      },
      "Cablevision": {
         "notBefore": 1623943955,
         "notAfter": 1623951155,
         "issuer": "Spectrum",
         "type": "regular",
         "attributes": {
            "userId": {
               "value": "BASE64_value_userId",
               "state": "plain"
            }
         }
      }
   }
}
```

>[!ENDTABS]
