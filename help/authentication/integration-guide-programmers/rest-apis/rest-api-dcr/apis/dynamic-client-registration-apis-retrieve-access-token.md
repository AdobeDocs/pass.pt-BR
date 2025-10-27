---
title: Recuperar token de acesso
description: API de registro dinâmico do cliente - Recuperar token de acesso
exl-id: 23287acf-5d56-46f0-b65e-79bf7d667708
source-git-commit: f0969a692380af3d20df37c49c37f7d1e85c7cfd
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 2%

---

# Recuperar token de acesso {#retrieve-access-token}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> A implementação da API de Registro de Cliente Dinâmico é limitada pela documentação do [Mecanismo de limitação](/help/authentication/integration-guide-programmers/throttling-mechanism.md).

## Solicitação {#request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">caminho</td>
      <td>/o/client/token</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>POST</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parâmetros de corpo</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_id</td>
      <td>
            A string do identificador do aplicativo cliente.
            <br/><br/>
            Para obter mais informações sobre como obter a cadeia de caracteres do identificador do cliente, consulte a documentação da API <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Recuperar credenciais do cliente</a>.
      </td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">client_secret</td>
      <td>
            A string secreta do aplicativo do cliente.
            <br/><br/>
            Para obter mais informações sobre como obter a cadeia de caracteres secreta do cliente, consulte a documentação da API <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Recuperar credenciais do cliente</a>.
      </td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">grant_type</td>
      <td>
            A string do tipo concessão (por exemplo, "client_credentials") que o aplicativo cliente pode usar para o endpoint do token do cliente.
            <br/><br/>
            Para obter mais informações sobre como obter a cadeia de caracteres de tipo de concessão, consulte a documentação da API <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Recuperar credenciais do cliente</a>.
      </td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
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
      <td style="background-color: #DEEBFF;">X-Device-Info</td>
      <td>
         A geração da carga de informações do dispositivo está descrita na documentação <a href="../../rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a>.
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
               <td style="background-color: #DEEBFF;">id</td>
               <td>O identificador opaco que pode ser usado para rastrear a atividade do usuário.</td>
               <td><i>obrigatório</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">access_token</td>
               <td>O valor do token de acesso que o aplicativo cliente deve usar para o cabeçalho de Autorização.</td>
               <td><i>obrigatório</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">created_at</td>
               <td>O tempo em milissegundos em que o token de acesso foi emitido.</td>
               <td><i>obrigatório</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">expires_in</td>
               <td>O tempo em segundos até o token de acesso expirar.</td>
               <td><i>obrigatório</i></td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">token_type</td>
               <td>O tipo de token (por exemplo, "portador").</td>
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
      <td>400</td>
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
      <td>
        Os valores possíveis são:
        <table style="table-layout:auto">
            <tr>
               <th style="background-color: #EFF2F7;">Valor</th>
               <th style="background-color: #EFF2F7"></th>
               <th style="background-color: #EFF2F7;"></th>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_request</td>
               <td>
                    A solicitação é inválida devido a um dos seguintes motivos:
                    <ul>
                        <li>A solicitação perde um parâmetro obrigatório.</li>
                        <li>A solicitação inclui um valor de parâmetro sem suporte (diferente do tipo de concessão).</li>
                        <li>A solicitação repete um parâmetro.</li>
                        <li>A solicitação inclui várias credenciais.</li>
                        <li>A solicitação usa mais de um mecanismo para autenticar o cliente.</li>
                        <li>A solicitação está malformada.</li>
                    </ul>
               </td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">invalid_client</td>
               <td>As credenciais do cliente são inválidas, o cliente precisa obter novas credenciais e tentar novamente. Para obter mais detalhes, consulte a documentação da API <a href="dynamic-client-registration-apis-retrieve-client-credentials.md">Recuperar credenciais do cliente</a>.</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">unauthorized_client</td>
               <td>O aplicativo cliente não está autorizado a usar este tipo de concessão de autorização.</td>
            </tr>
            <tr>
               <td style="background-color: #DEEBFF;">unsupported_grant_type</td>
               <td>O tipo de concessão de autorização não tem suporte no servidor de autorização.</td>
            </tr>
         </table>
      </td>
      <td><i>obrigatório</i></td>
   </tr>
</table>

## Amostras {#samples}

### Recuperar token de acesso {#samples-retrieve-access-token}

>[!BEGINTABS]

>[!TAB Solicitação]

```HTTPS
POST /o/client/token HTTP/1.1

    X-Device-Info: ewoJInByaW1hcnlIYXJkd2FyZVR5cGUiOiAiU2V0VG9wQm94IiwKCSJtb2RlbCI6ICJUViA1dGggR2VuIiwKCSJtYW51ZmFjdHVyZXIiOiAiQXBwbGUiLAoJIm9zTmFtZSI6ICJ0dk9TIgoJIm9zVmVuZG9yIjogIkFwcGxlIiwKCSJvc1ZlcnNpb24iOiAiMTEuMCIKfQ==
    Content-Type: application/x-www-form-urlencoded
    Accept: application/json
    User-Agent: Mozilla/5.0 (Apple TV; U; CPU AppleTV5,3 OS 11.0 like Mac OS X; en_US)
    
Body:
    
client_id=s6BhdRkqt3&client_secret=t7AkePiru4&grant_type=client_credentials
```

>[!TAB Resposta - Êxito]

```HTTPS
HTTP/1.1 201 Created

Content-Type: application/json;charset=UTF-8

{
  "id": "a932f8f0-210a-41a4-b2a8-377751f6b76f",  
  "access_token": "2YotnFZFEjr1zCsicMWpAA",
  "created_at": 1752148106221,
  "expires_in": 21600,
  "token_type": "bearer"
}
```

>[!TAB Resposta - Erro]

```HTTPS
HTTP/1.1 400 Bad Request

Content-Type: application/json;charset=UTF-8

{ "error": "invalid_request" }
```

>[!ENDTABS]
