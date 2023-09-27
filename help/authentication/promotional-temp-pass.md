---
title: Temp Pass Promocional
description: Temp Pass Promocional
exl-id: 705c1ba9-0430-4e3b-add1-d9e4da3f82d1
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1529'
ht-degree: 0%

---

# Temp Pass Promocional {#promotional-temp-pass}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Resumo dos recursos {#feature-summary}

O Temp Pass promocional permite que os programadores ofereçam acesso temporário a seu conteúdo protegido para usuários que não tenham credenciais de conta com um MVPD.

O Temp Pass Promocional é destinado a ser usado para executar campanhas promocionais em que um usuário, após fornecer informações de identificação válidas (por exemplo, endereço de email) para o Programador, poderá consumir um **número predefinido de títulos de VOD diferentes para um período predefinido**.

>[!IMPORTANT]
>
>O Adobe não armazena informações pessoais identificáveis (PII). Portanto, o Programador deve definir um hash sobre as informações fornecidas pelo usuário único nas APIs de autenticação da Adobe Pass.

O Temp Pass Promocional é criado em cima da variável [Temp Pass (Aprovação temporária)](/help/authentication/temp-pass.md) recurso, o que significa que inclui toda a funcionalidade Aprovação temporária.

Quando o número máximo predefinido de títulos de VOD ou o período predefinido for excedido, esse usuário não poderá acessar o conteúdo no mesmo dispositivo ou usando as mesmas informações de identificador do usuário (por exemplo, endereço de email) até que os tokens de autorização sejam apagados do servidor de autenticação da Adobe Pass.

>[!NOTE]
>
>O Temp Pass faz parte do pacote de Fluxo de trabalho Premium. Entre em contato com seu representante de vendas da Adobe Pass, se estiver interessado em usar essa funcionalidade.

## Comparação entre Aprovação Temporária e Aprovação Temporária Promocional {#tp-ptp-comparison}

| Temp Pass (Aprovação temporária) | Temp Pass Promocional |
|----------------------------------|----------------------------------------------------------------------------------------|
| Acesso ao conteúdo <ul><li>baseado em tempo</li></ul> | Acesso ao conteúdo <ul><li>baseado em tempo</li><li>com base no número de recursos</li></ul> |
| Segurança de acesso baseada em <ul><li>ID do dispositivo</li></ul> | Segurança baseada em <ul><li>ID do dispositivo</li><li>hash sobre as informações do identificador do usuário fornecidas (por exemplo, email)</li></ul> |
| API de erro do cliente disponível | API de erro do cliente disponível |
| Redefinição/limpeza disponível | Redefinição/limpeza disponível |

## Detalhes do recurso {#feature-details}

Esse recurso permite que os usuários acessem o conteúdo promocional de um dispositivo específico (telefone e tablet) após terem fornecido informações exclusivas, como o endereço de email, no aplicativo do Programador.

O Programador fornecerá um hash sobre a PII do usuário nas APIs de autenticação e autorização. Esse hash será usado junto com a ID do dispositivo na geração de uma chave exclusiva para identificar o usuário e o dispositivo.

Com base na ID do dispositivo e nas informações fornecidas pelo usuário e seguindo a lógica abaixo, a autenticação da Adobe Pass determinará se o usuário está em uma nova avaliação ou em uma existente:

* novo hash sobre informações fornecidas pelo usuário (por exemplo, e-mail), nova ID de dispositivo => nova avaliação
* hash existente sobre informações fornecidas pelo usuário (por exemplo, email), nova ID de dispositivo => avaliação existente (com hash existente sobre informações fornecidas pelo usuário (por exemplo, email))
* novo hash sobre informações fornecidas pelo usuário (por exemplo, email), ID do dispositivo existente => avaliação existente (com ID do dispositivo existente)
* hash existente em relação às informações fornecidas pelo usuário (por exemplo, e-mail), ID do dispositivo existente => avaliação existente

>[!NOTE]
>A validação e o hash das informações fornecidas pelo usuário são manipulados pelo programador, não pelo Adobe.

**O recurso Aprovação temporária promocional pode ser configurado com base nas seguintes propriedades:**

* Chave de informações fornecida pelo usuário (por exemplo, email)
* Número de recursos que o usuário está autorizado a consumir
* TTL - o período no qual o usuário tem direito a consumir o número configurado de recursos

### Metadados do usuário {#user-metadata}

Para facilitar a execução da candidatura do programador, devem ser **as informações de metadados do usuário são expostas** no Passe Temp Promocional, com as chaves correspondentes (para ativar as chaves, entre em contato com tve-support@adobe.com):

* **remaining_resources**: o número de recursos restantes que o usuário atual está autorizado a consumir
* **used_assets**: a lista de recursos que o usuário atual já consumiu
* **data_expiração**: a data de expiração do usuário atual

### Como o tempo de exibição é calculado? {#compute-viewing-time}

O tempo em que uma Aprovação Temporária permanece válida não está correlacionado à quantidade de tempo que um usuário gasta visualizando o conteúdo no aplicativo do Programador. Após a solicitação inicial de autorização do usuário por meio do passo Temp Promocional, uma hora de expiração é calculada adicionando a hora inicial da solicitação atual ao TTL (intervalo de tempo de duração) especificado pelo Programador.

### Autenticação e autorização {#authn-authz}

Para fluxos de passagem temporária promocional, a autenticação e a autorização não se comunicam com um MVPD real, **portanto, todas as solicitações de autorização terão êxito** desde que todas essas condições sejam atendidas:

* os tokens de autorização são válidos para recursos especificados
* número de **used_assets** é menor do que o limite definido pelo Programador
* **data_expiração** o valor é posterior à data atual.

### Comportamento de comprovação {#preflight-beh}

Quando uma solicitação de comprovação ou pré-autorização é feita para um MVPD de aprovação temporária promocional, a resposta de Comprovação correspondente retornada conterá toda a lista de recursos da Solicitação de comprovação como comprovação bem-sucedida.

A lógica por trás disso é: as condições de autorização da Aprovação Temporária Promocional são baseadas no limite de tempo e número de recursos, em vez de recursos específicos. Mais especificamente, desde que a restrição de tempo seja atendida e o limite de recursos não seja excedido, os recursos chamados serão autorizados.

### SSO {#sso}

O SSO não está habilitado para instâncias de Aprovação Temporária Promocional, que está configurada com a opção &quot;Autenticação por Solicitante&quot; habilitada. Isso significa que quando o usuário alternar do aplicativo A para outro aplicativo B, ambos integrados com o mesmo Passe de temperatura promocional, o usuário não entrará automaticamente.

### Sair {#logout}

Todos os tokens em um dispositivo são excluídos ao fazer logoff. Portanto, a alternância do Passe Temp Promocional para um MVPD regular selecionado pelo usuário não deve depender dessa implementação. Recomenda-se utilizar a variável `setSelectedProvider(null)` para limpar o estado do aplicativo e reiniciar o fluxo de autenticação, o que tem uma melhor experiência do usuário.

### Diagrama de fluxo de passagem temporária promocional {#promo-tempass-flowdia}

![Diagrama de fluxo de passagem temporária promocional](assets/promo-temp-pass-flow.png)

*Figura: Fluxo de passagem temporária promocional*

## Implementar Aprovação Temporária Promocional {#impl-promo-tempass}

O Temp Pass Promocional requer as seguintes funcionalidades do lado do cliente:

* **Informações do identificador do usuário, por exemplo, propagação de endereço de email** (envio do endereço de email do usuário nos fluxos de autenticação e autorização). O e-mail é exigido pela Autenticação Adobe Pass para vincular os tokens de autenticação e autorização (semelhante ao caso do `device_ID`, obrigatório em todas as chamadas).
* **Forçar autenticação** - permitindo que o Programador force um fluxo de autenticação quando o usuário já estiver autenticado. Essa funcionalidade é necessária para forçar uma atualização de metadados do usuário (a chave de metadados do usuário **used_assets** contém o número de recursos disponíveis) toda vez que o aplicativo é iniciado. Como o usuário pode fazer logon em vários dispositivos, os metadados de usuário presentes no dispositivo durante a inicialização do aplicativo não são confiáveis, e precisamos atualizá-los para refletir o estado atual desse usuário específico (identificado pelo endereço de email).


>[!IMPORTANT]
>Forçar autenticação é possível somente no iOS e no Android.
>A Autenticação do Adobe Pass não tem um mecanismo integrado para interromper o streaming gratuito após X minutos. A autenticação da Adobe Pass cessará a emissão **autorização** e **mídia curta** tokens assim que o usuário consumir os recursos livres de Y. Cabe aos programadores restringir o acesso assim que o passe temporário promocional expirar.

## Segurança {#security}

>[!IMPORTANT]
>O Adobe não armazena informações pessoais identificáveis (PII). Portanto, o Programador deve definir um hash sobre as informações fornecidas pelo usuário único nas APIs de autenticação da Adobe Pass.

**Hash de informações do identificador do usuário**

O Adobe recomenda o uso de **SHA-2** ou a sua família **SHA-256**, **SHA-512** funciona em dados antes de serem enviados para o Adobe.

Por exemplo, **SHA-256** sobre **&quot;user@domain.com&quot;** é **&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&quot;**.

## Redefinir ou limpar Aprovação Temporária Promocional {#reset-promo-tempass}

Certas regras de negócios exigem uma limpeza regular do Passe Temporário Promocional. Para fazer isso, a Autenticação Adobe Pass fornece aos Programadores uma *público* API da Web, descrita abaixo:

| `DELETE https://mgmt.auth.adobe.com/reset-tempass/v2/reset` |
|----|
| <ul><li>Protocolo: **https**</li><li>Host:<ul><li>Versão: **mgmt.auth.adobe.com**</li><li>Pré- Igual: **mgmt-prequal.auth.adobe.com**</li></ul></li><li>Caminho: **/reset-tempass/v2/reset**</li><li>Parâmetros de consulta: **device_id=all&amp;requestor_id=THE_REQUESTOR_ID&amp;mvpd_id=THE_TEMPASS_MVPD_ID**</li><li>cabeçalhos: ApiKey: **1232293681726481**</li> <li>Resposta:<ul><li>Sucesso: **HTTP 204**</li><li>Falha: **HTTP 400** para solicitações incorretas, **HTTP 401** se ApiKey não for especificada, **HTTP 403** se ApiKey for inválida</li></ul></li></ul> |

Além dos requisitos para expurgar a Aprovação Temporária, a Aprovação Temporária Promocional usa o hash sobre as informações do identificador do usuário enviadas como **generic_data** sobre autenticação e autorização para limpeza.

O hash será enviado, não todo o JSON:

```cURL
$ curl -X DELETE -H "Authorization:Bearer H4j7cF3GtJX81BrsgDa10GwSizVz" "https://mgmt.auth.adobe.com/reset-tempass/v2.1/reset/generic?key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7&requestor_id=REF&mvpd_id=FlexibleTempPass"
```

### Clientes compatíveis {#supported-clients}

| Clientes de autenticação da Adobe Pass | Temp Pass Promocional | Redefinir ferramenta | Suporte a código de resposta dedicado/erro de cliente |
|:--------------------------------------:|:---------------------:|:----------:|:-----------------------------------------------:|
| Ativador de acesso JS | SIM | SIM | SIM (a partir da v 3.0.0) |
| IOS do cliente nativo | SIM | SIM | SIM (a partir da v 1.10) |
| Android do cliente nativo | SIM | SIM | SIM |
| API sem cliente | SIM | SIM | NÃO |


## Limitação {#limitations}

Esta seção descreve as limitações que se aplicam à implementação atual do Passe Temp Promocional.

### Sem cliente {#lim-clientless}

**Dispositivos inteligentes sem uma ID de dispositivo exclusiva**

Nem todos os aplicativos de dispositivos inteligentes podem fornecer uma ID de dispositivo exclusiva. Na ausência de uma, a Autenticação do Adobe Pass pode usar a UUID gerada pelo Serviço de código de registro de Adobe como a ID exclusiva do dispositivo. Isso significa que, quando o usuário sair, os tokens de autenticação e autorização serão excluídos. Depois que o usuário tentar autenticar novamente, desta vez com diferentes informações do usuário (por exemplo, email), ele poderá autorizar novamente. A Adobe recomenda adicionar um fluxo de interface do usuário que não permitirá que um usuário &quot;engane&quot; o sistema e adicione lógica para determinar se é um novo usuário que está solicitando uma avaliação ou uma avaliação existente.

**Redefinindo/limpando Aprovação Temporária**

Redefinir Temp Pass para Clientless não está disponível nos casos específicos do Xbox360 e do Xbox One, porque essas plataformas exigem análise adicional da ID do dispositivo, não é possível na ferramenta Redefinir Temp Pass.

<!--
>[!RELATEDINFORMATION]
>
>* [Preflight Authorization](/help/authentication/preflight-authz.md)
-->
