---
title: Recurso de metadados do usuário
description: Recurso de metadados do usuário
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a1
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1649'
ht-degree: 0%

---

# Metadados do usuário {#user-metadata}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

</br>
</br>

## Introdução {#intro}

O recurso de metadados do usuário permite que os programadores acessem diferentes tipos de dados específicos do usuário mantidos por MVPDs.  Os tipos de metadados do usuário incluem códigos postais, classificações dos pais, IDs de usuário e muito mais.  *Usuário* metadados é uma extensão do disponível anteriormente *estático* (TTL do token de autenticação, TTL do token de autorização e ID do dispositivo).


Pontos principais dos metadados do usuário:

- Transmitido para o aplicativo do Programador durante os fluxos de autenticação e autorização
- Os valores são salvos no token
- Os valores podem ser normalizados se diferentes MVPDs fornecerem dados em diferentes formatos
- Alguns parâmetros podem ser criptografados usando a chave do programador (por exemplo, código postal)
- Valores específicos são disponibilizados por Adobe, por meio de uma alteração de configuração

## Obter metadados do usuário {#obtaining}

Os metadados do usuário estão disponíveis para programadores por meio do AccessEnabler `getMetadata()` e por meio da `/usermetadata` ponto de extremidade na API sem cliente.  Consulte a documentação da API da plataforma para obter detalhes sobre o uso de `getMetadata()` e a chamada de retorno correspondente `setMetadataStatus()` (ou para endpoints e parâmetros usados na API sem cliente).

Os programadores obtêm metadados fornecendo uma chave para o tipo de metadados que desejam obter: `getMetadata(key)`.

Os metadados são retornados da seguinte maneira: `setMetadataStatus(key, encrypted, data)`:

| Parâmetro | Tipo | Descrição |
| ----------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `key` | String | Especifica o tipo de metadados solicitados |
| `encrypted` | Booleano | Um sinalizador booleano que indica se o &quot;valor&quot; está criptografado ou não. Se isso for &quot;true&quot;, o &quot;value&quot; será realmente uma representação JSON Web Encrypted do valor real. |
| `data` | Objeto | Um objeto JSON que contém a representação dos metadados |



A estrutura do parâmetro de dados, os valores variam entre os tipos:

| Chave | Tipo de valor | Amostra | Descrição |
| --- | --- | --- | --- |
| `zip` | Matriz JSON | \[&quot;77754&quot;, &quot;12345&quot;\] | Código postal |
| `householdID` | String JSON | 1o7241p | Identificador do agregado. Se o MVPD não suportar subcontas, será idêntico a `userID` |
| `maxRating` | Objeto JSON | { MPAA: &quot;NR&quot;, <br>  VCHIP: &quot;X&quot;  <br>  URL: &quot;http://manage.my/parental&quot; } | Classificação máxima dos pais para o usuário |
| `userID` | String JSON | 1o7241p | O identificador do usuário. No caso em que um MVPD oferece suporte a subcontas e o usuário não é a conta principal, `userID` será diferente de `householdID`. |
| `channelID` | Matriz JSON | \[&quot;canal-1&quot;, &quot;canal-2&quot; \] | Uma lista de canais que um usuário está autorizado a visualizar |
| `is_hoh` | String JSON | &quot;1&quot; | Um sinalizador que identifica se um usuário é chefe de família |
| `encryptedZip` | String JSON | &quot;&quot; | A Comcast expõe o código postal criptografado |
| `typeID` | String JSON | Principal | Um sinalizador que identifica se a conta do usuário é a conta principal/secundária |
| `primaryOID` | String JSON | &quot;uuidd1e19ec9-012c-124f-b520-acaf118d16a0&quot; | Identificador do agregado. Se `typeID` é Primário, conterá o valor de `userID` |
| hba_status | Booleano | &quot;true&quot; &quot;false&quot; | Um sinalizador booleano que indica se a Autenticação Baseada em Home está ativada para uma integração específica |
| allowMirroring | Booleano | &quot;true&quot; &quot;false&quot; | Indica se o espelhamento de tela é permitido para este dispositivo ou não |

>[!NOTE]
>
> **Nota:** Se o parâmetro de dados estiver criptografado, como geralmente é o caso para o **tecla zip**, a representação da chave de metadados será uma string JSON em vez de uma matriz ou um objeto.


**Importante:** Os metadados reais do usuário disponíveis para um Programador dependem do que um MVPD disponibiliza.  Os contratos legais devem ser assinados com os MVPDs antes que os metadados confidenciais do usuário (como código postal) sejam disponibilizados no ambiente de produção.

</br>


| Nome | Detalhes | Requer criptografia | Comentários |
| --- | --- | --- | --- |
| ID de usuário | Conforme fornecido pelo MVPD | Não | Esse é o valor que é então transformado em hash pelo Adobe e exposto no token de mídia e no retorno de chamada sendTrackingData().<br><br>O hash, neste caso, foi feito por motivos históricos<br><br>Essa ID pode ser uma ID da família ou uma ID de subconta. Normalmente, isso não é especificado. Apenas a ID vinculada ao logon que foi usada no momento (que pode ser principal ou subconta) |
| ID de usuário upstream | Fornecido pelo MVPD para ser usado apenas para fluxos de Monitoramento de Simultaneidade | Não | Esse valor é usado ao impor limites de simultaneidade em sites e aplicativos MVPD e Programador. <br><br>A ID também pode conter políticas de monitoramento de concorrência<br><br>Para a maioria dos MVPDs, esse valor é igual à ID do usuário |
| ID de Usuário Doméstico | Fornecidos pelo MVPD para serem usados principalmente para fluxos de Controle dos pais | Não | ID que permite aos programadores entender o uso doméstico vs. de subconta.<br><br>Às vezes, é usado como substituto dos Controles dos pais se as classificações verdadeiras não estiverem disponíveis (se o usuário tiver feito logon com a conta da família que pode assistir, o conteúdo classificado não será exibido)<br><br>Há muita variação entre os MVPDs sobre como isso é representado - ID de usuário da residência, ID do chefe de residência, bandeira do chefe de residência etc. |
| Chefe de família | Sinalização de sinalização se a conta corrente é ou não chefe de família | Não | veja acima |
| ID de tipo / ID primária | Identificadores da conta do agregado | Não | Indicadores específicos da AT&amp;T para chefes de família.<br><br>ID de Tipo = Sinalizador que identifica se a conta do usuário é conta principal/secundária<br><br>OID primário = Identificador do agregado. Se TypeID for Primário, conterá o valor da userID |
| Classificação máxima | A classificação máxima permitida para a conta atual | Não | Permite aos programadores filtrar o conteúdo que não é adequado para a conta.<br><br>Tem classificações de MPAA ou VCHIP |
| Linha de canal acima | Lista de canais disponíveis no pacote do usuário | Não | Usado para permitir/remover rapidamente vários canais de portais que agregam várias redes</br></br> *Observe que a autorização de comprovação geralmente oferece mais flexibilidade para esse caso de uso e deve ser usada em vez disso* <br><br>A especificação OLCA permite isso como uma Instrução de Atributo na resposta AuthN |
| Status do HBA | Indica se a autenticação ocorreu via HBA | Não |     |
| Código postal | CEP de cobrança do usuário | Sim | Usado para eventos de Difusão ou Esportes<br><br>Também pode ser fornecida com a resposta AuthZ para atualizações rápidas<br><br>Dados confidenciais, precisa de contratos legais MVPD |
| Código postal criptografado | CEP de faturamento do usuário (Comcast) | Sim | Como acima, mas criptografado pela Comcast |
| Idioma | Configurações de idioma do usuário | Não | Usado para exibir mensagens de acordo com as preferências do usuário |
| Permitir espelhamento | Indica se o espelhamento de tela é permitido para este dispositivo ou não | Não |     |



## Metadados disponíveis {#available_metadata}

A tabela a seguir apresenta o estado atual dos metadados do usuário no ecossistema de autenticação da Adobe Pass:


|     | **Legal **<br><br>**Acordo **<br><br>**Assinado (somente zip)** | **ID de usuário **<br><br>**no AuthN** | **Código postal **<br><br>**em AuthN/Z** | **Classificação **<br><br>**em AuthN/Z** | **Família **<br><br>**ID no AuthN/Z** | **ID do canal no AuthN** | **Chefe de família na AuthN** | **ID de tipo no AuthN** | **OID principal no AuthN** | Idioma | ID de usuário upstream **no AuthN** | Status do HBA | OnNet | inHome | Permitir Espelhamento em AuthZ | **Notas** |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Nome formal** | n/d | `userID` | `zip` | `maxRating` | `householdID` | `channelID` | is_hoh | typeID | primaryOID | idioma | upstreamUserID | hba_status | onNet | inHome | allowMirroring | 1. Para AuthN - O OiosamlMetadataParser deve ser alterado para que todos os analisadores tenham esse novo atributo ativado <br>2.  Para AuthZ - Não há uma forma genérica, pois a implementação de autorização é específica do MVPD |
| **Criptografia necessária** | n/d | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** |     |
| **Sensível** | n/d | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** |     |     |
| **Adobe IdP** | **Sim** | **Sim** | **Sim (somente AuthN)** | **Sim (somente AuthN)** | **Sim (somente AuthN)** | **Sim** | **Sim** | **Sim** | **Sim** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** | Nenhum contrato legal necessário, pode ser habilitado. |
| **Synacor** | **Sim** | **Sim** | **Sim (somente AuthN)** | **Sim (somente AuthN)** | **Sim (somente AuthN)** | **Sim** | **Sim** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Contrato legal que não abrange todos os MVPDs com proxy.**   <br>Este é um suporte genérico para o Synacor - possivelmente não acumulado para todos os seus MVPDs. |
| Prato | **Não** | **Sim** | **Sim (somente AuthN)** | **Sim (somente AuthN)** | **Sim (somente AuthN)** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** | Compartilha a mesma lista que todos os MVPDs do Synator, além de upstreamUserID. |
| Comcast | **Não** | **Sim** | **Não** | **Sim (somente AuthZ)** | **Sim (somente AuthZ)** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Sim** | **Não** | **Não** | **Não** |     |
| **AT&amp;T** | **Sim** | **Sim** | **Sim (somente AuthN)** | **Não** | **Sim (somente AuthN)** | **Não** | **Não** | **Sim** | **Sim** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** | Contrato legal assinado. |
| **Visão do cabo** | **Sim** | **Sim** | **Sim (somente AuthN)** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** | Contrato legal assinado. |
| **HTC** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** |     |
| **Massilon de proxy** | **Sim** | **Sim** | **Sim (somente AuthN)** | **Não** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** | Contrato legal assinado. |
| **Clearleap do Proxy** | **Sim** | **Sim** | **Sim (somente AuthN)** | **Sim (somente AuthZ)** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Sim** | **Não** | **Não** | **Não** | **Não** | Contrato legal assinado. |
| Rogers | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** |     |
| RCN | **Sim** | **Sim** | **Sim (somente AuthN)** | **Sim (somente AuthN)** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** |     |
| Carta | **Sim** | **Sim** | **Sim (somente AuthN)** | **Sim (somente AuthN)** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** |     |
| Verizon | **Não** | **Sim** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Sim** | **Não** | **Não** | **Não** |     |
| Eastlink | **Não** | **Sim** | **Sim (somente AuthN)** | **Sim (somente AuthN)** | **Sim (somente AuthN)** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** |     |
| GLDS de proxy | **Não** | **Sim** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** |     |
| DTV | **Sim** | **Sim** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** |     |
| COX | **Não** | **Sim** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** |     |
| Cogeco | **Não** | **Sim** | **Sim (somente AuthN)** | **Não** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** |     |
| Videotron | **Não** | **Sim** | **Sim (somente AuthN)** | **Não** | **Sim*** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** | Expõe a ID do usuário com o mesmo valor da ID do usuário |
| Espectro | **Sim** | **Sim** | **Sim (somente AuthN)** | **Sim (somente AuthN)** | **Sim (somente AuthN)** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Sim** | **Não** | **Não** | **Sim** |     |
| **Todos os outros **<br><br>**MVPDs** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Não** | **Sim** | **Não** | **Não** | **Não** | **Não** | **Ainda sem contrato legal, metadados confidenciais não estão disponíveis para produção.**  <br>Para todos os MVPDs, a ID de usuário está disponível sem trabalho extra. |


A lista de tipos de metadados do usuário será expandida à medida que novos tipos forem disponibilizados e adicionados ao sistema de autenticação da Adobe Pass.

## Amostras de código {#code_samples}

- [Amostra de código 1](#code_sample1)
- [Amostra de código 2 (getMetadata fictício)](#code_sample2)


### Amostra de código 1 {#code_sample1}

```
    // Assuming a reference to the AccessEnabler has been previously obtained and stored in the "ae" variable
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if(status ==1) {
        // User is authenticated, request metadata
        ae.getMetadata("zip");
        ae.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if(encrypted) {
        // The metadata value is encrypted
        // Needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```


### Amostra de código 2 (getMetadata fictício) {#code_sample2}

```
    // Assuming a reference to the AccessEnabler has been
    //   previously obtained and stored in the "ae" variable
     
    // Mock the getMetadata() method
    var aeMock = {
        getMetadata: function(key) {
          var data = null;
          // Set mock data based on the received key,
          // according to the format in the spec
          switch(key) {
            case 'zip': 
              data = [ "1235", "23456" ];
              break;
            case 'maxRating': 
              data = { "MPAA": "PG-13", "VCHIP": "TV-14" };
              break;
            default:
              break;
          }
          // Call the metadata status just like AccessEnabler does
          setMetadataStatus(key, false, data);
        }
     }
     
    ae.setRequestor("SITE");
    ae.checkAuthentication();
     
    function setAuthenticationStatus(status, reason) {
      if (status == 1) {
        // User is authenticated, request metadata using mock object
        aeMock.getMetadata("zip");
        aeMock.getMetadata("maxRating");
      } else {
        ...
      }
    }
     
    // Implement the  setMetadataStatus() callback
    function setMetadataStatus(key, encrypted, data) {
      if (encrypted) {
        // The metadata value is encrypted, so it
        //   needs to be decrypted by the programmer
         data = decrypt(data);
      }
      alert(key + "=" + data);
    }
```

<!---

For details on your particular platform, or to gain some insight into how User Metadata is processed on the MVPD side, see the appropriate link in Related Information below.  

## Related Information {#related}

- ActionScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- JavaScript - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaData)
- iOS - [getMetadata()](#getMeta), [setMetadataStatus()](#setMetaStatus)
- Android - [getMetadata()](#getMetadata), [setMetadataStatus()](#setMetadaStatus)
- Clientless - [AuthN Metadata](#authn_metadata)
- [MVPD Integration Guide: User Metadata Exchange](/help/authentication/mvpd-user-metadata-exchng.md)
-->
