---
title: Códigos de erro aprimorados
description: Códigos de erro aprimorados
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 21b4ad42709351eac1c2089026f84a43deb50f8a
workflow-type: tm+mt
source-wordcount: '2593'
ht-degree: 3%

---

# Códigos de erro aprimorados {#enhanced-error-codes}

>[!IMPORTANT]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Os códigos de erro aprimorados representam um recurso de autenticação da Adobe Pass que fornece informações adicionais sobre erros para aplicativos clientes integrados com o:

* APIs REST de autenticação da Adobe Pass:
   * [REST API v1](./rest-api-overview.md)
   * [REST API v2](./rest-api-v2/apis/rest-api-v2-apis-overview.md)
* API pré-autorizada dos SDKs de autenticação da Adobe Pass:
   * [JavaScript SDK (API pré-autorizada)](./preauthorize-api-javascript-sdk.md)
   * [iOS/tvOS SDK (API pré-autorizada)](./preauthorize-api-ios-tvos-sdk.md)
   * [Android SDK (API pré-autorizada)](./preauthorize-api-android-sdk.md)

  _(*) A API pré-autorizada é a única API SDK de Autenticação do Adobe Pass que fornece suporte para Códigos de Erro Aprimorados._

>[!IMPORTANT]
>
> Os aplicativos que integram a API REST de Autenticação do Adobe Pass v2 se beneficiarão dos Códigos de erro aprimorados por padrão sem exigir configuração adicional.
>
> <br/>
>
> Os aplicativos que integram a API REST de autenticação do Adobe Pass v1 ou a API pré-autorizada de SDKs podem se beneficiar dos Códigos de erro aprimorados somente se o recurso estiver explicitamente habilitado.
>
> <br/>
>
> Para habilitar explicitamente este recurso, crie um tíquete por meio do nosso [Zendesk](https://adobeprimetime.zendesk.com) e peça ajuda ao seu Gerente técnico de conta (TAM).

## Representação {#enhanced-error-codes-representation}

Os Códigos de Erro Aprimorados podem ser representados no formato `JSON` ou `XML`, dependendo da API de Autenticação Adobe Pass integrada e do valor do cabeçalho &quot;Accept&quot; usado (ou seja, `application/json` ou `application/xml`):

| API de autenticação do Adobe Pass | JSON | XML |
|-------------------------------|---------|---------|
| REST API v1 | &amp;verificar; | &amp;verificar; |
| REST API v2 | &amp;verificar; |         |
| API pré-autorizada de SDKs | &amp;verificar; |         |

>[!IMPORTANT]
>
> A Autenticação do Adobe Pass pode comunicar Códigos de erro aprimorados a aplicativos clientes de dois modos:
>
> <br/>
>
> * **Informações de erro de nível superior**: neste caso, o objeto ***&quot;erro&quot;*** está localizado no nível superior, portanto, o corpo da resposta pode conter somente o objeto ***&quot;erro&quot;***.
> * **Informações de erro no nível do item**: neste caso, o objeto ***&quot;erro&quot;*** está localizado no nível do item, portanto, o corpo da resposta pode conter um objeto ***&quot;erro&quot;*** para todos os itens que tiveram um erro durante a manutenção.
>
> <br/>
>
> Consulte a documentação pública de cada API de autenticação integrada do Adobe Pass para determinar as especificações de representação dos Códigos de erro aprimorados.

Consulte as seguintes respostas HTTP contendo exemplos de Códigos de Erro Aprimorados representados como `JSON` ou `XML`.

>[!BEGINTABS]

>[!TAB API REST v1 - Informações de erro de nível superior (JSON)]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json
        
{
  "action": "none",
  "status": 400,
  "code": "invalid_requestor",
  "message": "The requestor parameter is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "8bcb17f9-b172-47d2-86d9-3eb146eba85e"
}
```

>[!TAB API REST v1 - XML (Informações de erro de nível superior)]

```XML
HTTP/1.1 400 Bad Request
Content-Type: application/xml

<error>
  <action>none</action>
  <status>400</status>
  <code>invalid_requestor</code>
  <message>The requestor parameter is missing or invalid.</message>
  <helpUrl>https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html</helpUrl>
  <trace>8bcb17f9-b172-47d2-86d9-3eb146eba85e</trace>
</error>
```

>[!TAB API REST v1 - Informações de erro no nível do item (JSON)]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "resources": [
    {
      "id": "TestStream1",
      "authorized": true
    },
    {
      "id": "TestStream2",
      "authorized": false,
      "error": {
        "action": "retry",
        "status": 403,
        "code": "network_connection_failure",
        "message": "Unable to contact your TV provider services",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!TAB REST API v2 - Informações de erro de nível superior (JSON)]

```JSON
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "action": "none",
  "status": 400,
  "code": "invalid_parameter_service_provider",
  "message": "The service provider parameter value is missing or invalid.",
  "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
  "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
}
```

>[!TAB REST API v2 - Informações de erro no nível do item (JSON)]

```JSON
HTTP/1.1 200 OK
Content-Type: application/json

{
  "decisions": [
    {
      "resource": "REF30",
      "serviceProvider": "REF30",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": true,
      "token": {
        "issuedAt": 1697094207324,
        "notBefore": 1697094207324,
        "notAfter": 1697094802367,
        "serializedToken": "PHNpZ25hdHVyZUluZm8..."
      }
    },
    {
      "resource": "REF40",
      "serviceProvider": "REF40",
      "mvpd": "Cablevision",
      "source": "mvpd",
      "authorized": false,
      "error" : {
        "action": "retry",
        "status": 403,
        "code": "network_connection_failure",
        "message": "Unable to contact your TV provider services",
        "details": "Your subscription package does not include the \"Live\" channel",
        "helpUrl": "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
        "trace": "12f6fef9-d2e0-422b-a9d7-60d799abe353"
      }
    }
  ]
}
```

>[!ENDTABS]

Os Códigos de Erro Aprimorados incluem os seguintes `JSON` campos ou `XML` atributos:

| Nome | Tipo | Exemplo | Restrito | Descrição |
|-----------|-----------|---------------------------------------------------------------------------------------------------------------------|:----------:|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *ação* | *cadeia de caracteres* | *tentar novamente* | &amp;verificar; | A Autenticação Adobe Pass recomendou uma ação que pode corrigir a situação conforme definido neste documento. <br/><br/> Para obter mais detalhes, consulte a seção [Ação](#enhanced-error-codes-action). |
| *status* | *inteiro* | *403* | &amp;verificar; | O código de status de resposta HTTP conforme definido no documento [RFC 7231](https://tools.ietf.org/html/rfc7231#section-6). <br/><br/> Para obter mais detalhes, consulte a seção [Status](#enhanced-error-codes-status). |
| *código* | *cadeia de caracteres* | *falha_de_conexão_de_rede* | &amp;verificar; | O código do identificador exclusivo de Autenticação do Adobe Pass associado ao erro, conforme definido neste documento. <br/><br/> Para obter mais detalhes, consulte a seção [Código](#enhanced-error-codes-code). |
| *mensagem* | *cadeia de caracteres* | *Não é possível contatar os serviços do provedor de TV* |            | A mensagem legível por humanos que pode ser exibida ao usuário final em alguns casos. <br/><br/> Para obter mais detalhes, consulte a seção [Tratamento de Resposta](#enhanced-error-codes-response-handling). |
| *detalhes* | *cadeia de caracteres* | *O pacote de assinatura não inclui o canal &quot;Online&quot;* |            | A mensagem detalhada que pode ser fornecida por um parceiro de serviços em alguns casos, <br/><br/> Este campo pode não estar presente caso o parceiro de serviços não forneça uma mensagem personalizada. |
| *urlAjuda* | *url* | *https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html* |            | O URL da documentação pública de Autenticação do Adobe Pass, que vincula mais informações sobre por que esse erro ocorreu e possíveis soluções. <br/><br/> Este campo contém uma URL absoluta e não deve ser deduzido a partir do código de erro, dependendo do contexto de erro uma URL diferente pode ser fornecida. |
| *rastreamento* | *cadeia de caracteres* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* |            | O identificador exclusivo da resposta que pode ser usado ao entrar em contato com o suporte da Autenticação da Adobe Pass para solucionar problemas específicos. |

>[!IMPORTANT]
>
> A coluna **Restrito** indica se o respectivo campo contém um valor de um conjunto finito, enquanto campos irrestritos podem conter dados.
>
> <br/>
>
> Atualizações futuras neste documento poderiam adicionar valores aos conjuntos finitos, mas não removerão ou alterarão os existentes.

### Ação {#enhanced-error-codes-representation-action}

Os Códigos de erro aprimorados incluem um campo &quot;ação&quot; que fornece uma ação recomendada que pode corrigir a situação.

Os valores possíveis para o campo &quot;action&quot; incluem:

| Ação | Descrição | Categoria |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------|
| nenhum | Não há nenhuma ação predefinida para corrigir esse problema, mas em alguns casos, isso pode indicar uma invocação inadequada da API. | Corrija o contexto da solicitação. |
| configuração | O aplicativo cliente requer uma alteração de configuração, na maioria das vezes conduzida pelo Adobe Pass TVE Dashboard. | Corrija o contexto de configuração da integração. |
| registro de aplicativos | O aplicativo cliente requer que o se registre novamente. | Corrija o contexto do aplicativo cliente. |
| autenticação | O aplicativo cliente requer a autenticação ou reautenticação do usuário. | Corrija o contexto do aplicativo cliente. |
| autorização | O aplicativo cliente requer a obtenção de autorização para o recurso especificado. | Corrija o contexto do aplicativo cliente. |
| tentar novamente | O aplicativo cliente requer uma nova tentativa de solicitação. | Corrija o contexto da solicitação. |

_(*) Para alguns erros, várias ações podem ser possíveis soluções, mas o campo &quot;ação&quot; indica aquela com a maior probabilidade de correção do erro._

### Status {#enhanced-error-codes-representation-status}

Os Códigos de erro aprimorados incluem um campo &quot;status&quot; que indica o código do status HTTP associado ao erro.

Os valores possíveis para o campo &quot;status&quot; incluem:

| Código | Frase de motivo |
|------|-----------------------|
| 400 | Solicitação inválida |
| 401 | Não autorizado |
| 403 | Proibido |
| 404 | Não encontrado |
| 405 | Método não permitido |
| 410 | Sumiu |
| 412 | Falha na pré-condição |
| 500 | Erro interno do servidor |

Códigos de erro aprimorados com um &quot;status&quot; 4xx geralmente aparecem quando o erro é gerado pelo cliente e, na maioria das vezes, significa que o cliente requer trabalho adicional para corrigi-lo.

Códigos de erro aprimorados com um &quot;status&quot; 5xx geralmente aparecem quando o erro é gerado pelo servidor e, na maioria das vezes, significa que o servidor requer trabalho adicional para corrigi-lo.

>[!IMPORTANT]
>
> Há casos em que o código de status de resposta HTTP é diferente do campo &quot;status&quot; do Código de erro aprimorado, especialmente ao interagir com uma API de autenticação da Adobe Pass que comunica Códigos de erro aprimorados como informações de erro no nível do item.

### Código {#enhanced-error-codes-representation-code}

Os códigos de erro aprimorados incluem um campo &quot;código&quot; que fornece um identificador exclusivo de autenticação da Adobe Pass associado ao erro.

Os valores possíveis para o campo &quot;código&quot; são agregados [abaixo](#enhanced-error-codes-list) em duas listas, com base na API de Autenticação Adobe Pass integrada.

## Listas {#enhanced-error-codes-lists}

### REST API v1 {#enhanced-error-codes-lists-rest-api-v1}

A tabela abaixo lista possíveis Códigos de erro aprimorados que um aplicativo cliente pode encontrar ao ser integrado à API REST de autenticação da Adobe Pass v1.

| Ação | Código | Status | Mensagem |
|--------------------|---------------------------------------------------|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **nenhum** | *solicitante_inválido* | 400 | O parâmetro do solicitante está ausente ou é inválido. |
|                    | *informações_de_dispositivo_inválidas* | 400 | Informações do dispositivo ausentes ou inválidas. |
|                    | *id_de_dispositivo_inválida* | 400 | O identificador do dispositivo está ausente ou é inválido. |
|                    | *recurso_ausente* | 400, 412 | O parâmetro de recurso está ausente. |
|                    | *malformed_authz_request* | 400, 412 | A solicitação de autorização é nula ou inválida. |
|                    | *pré-autorização_negada_por_mvpd* | 403 | O MVPD retornou uma decisão de &quot;Negação&quot; ao solicitar pré-autorização para o recurso especificado. |
|                    | *autorização_negada_por_mvpd* | 403 | O MVPD retornou uma decisão de &quot;Negação&quot; ao solicitar autorização para o recurso especificado. |
|                    | *autorização_negada_por_controles_pelos_pais* | 403 | O MVPD retornou uma decisão &quot;Negar&quot; devido às configurações de controles dos pais para o recurso especificado. |
|                    | *erro_interno* | 400, 405, 500 | A solicitação falhou devido a um erro interno do servidor. |
| **configuração** | *integração_desconhecida* | 400, 412 | A integração entre o programador especificado e o provedor de identidade não existe. Use o painel TVE para criar a integração necessária. |
|                    | *muitos_recursos* | 403 | A solicitação de autorização ou pré-autorização falhou porque muitos recursos foram consultados. Entre em contato com a equipe de suporte para configurar corretamente as limitações de autorização e pré-autorização. |
| **autenticação** | *autenticação_sessão_emissor_incompatibilidade* | 400 | A solicitação de autorização falhou devido ao fato de que o MVPD indicado para o fluxo de autorização é diferente daquele que emitiu a sessão de autenticação. O usuário deve autenticar novamente com o MVPD desejado para continuar. |
|                    | *autorização_negada_por_políticas_hba* | 403 | O MVPD retornou uma decisão de &quot;Negação&quot; devido a políticas de autenticação doméstica. A autenticação atual foi obtida usando um HBA (fluxo de autenticação baseado em início), mas o dispositivo não está mais em casa ao solicitar autorização para o recurso especificado. O usuário deve autenticar novamente com um MVPD compatível para continuar. |
|                    | *autorização_negada_por_sessão_invalidada* | 403 | A sessão de autenticação foi invalidada pelo provedor de identidade. O usuário deve autenticar novamente com um MVPD compatível para continuar. |
|                    | *identidade_não_reconhecida_por_mvpd* | 403 | A solicitação de autorização falhou devido ao fato de que a identidade do usuário não foi reconhecida pelo MVPD. |
|                    | *autenticação_sessão_invalidada* | 403 | A sessão de autenticação foi invalidada pelo provedor de identidade. O usuário deve autenticar novamente com um MVPD compatível para continuar. |
|                    | *autenticação_sessão_ausente* | 403, 412 | Não foi possível recuperar a sessão de autenticação associada a esta solicitação. O usuário deve autenticar novamente com um MVPD compatível para continuar. |
|                    | *autenticação_sessão_expirada* | 403, 412 | A sessão de autenticação atual expirou. O usuário deve autenticar novamente com um MVPD compatível para continuar. |
|                    | *pré-autorização_autenticação_sessão_ausente* | 412 | Não foi possível recuperar a sessão de autenticação associada a esta solicitação. O usuário deve autenticar novamente com um MVPD compatível para continuar. |
|                    | *pré-autorização_autenticação_sessão_expirada* | 412 | A sessão de autenticação atual expirou. O usuário deve autenticar novamente com um MVPD compatível para continuar. |
| **autorização** | *autorização_não_encontrada* | 403, 404 | Nenhuma autorização foi encontrada para o recurso especificado. O usuário deve obter uma nova autorização para continuar. |
|                    | *autorização_expirada* | 410 | A autorização anterior do recurso especificado expirou. O usuário deve obter uma nova autorização para continuar. |
| **tentar novamente** | *erro_recebido_de_rede* | 403 | Erro de leitura ao recuperar a resposta do serviço de parceiro associado. Tentar novamente a solicitação pode resolver o problema. |
|                    | *tempo_de_conexão_de_rede* | 403 | Houve um tempo limite de conexão com o serviço de parceiro associado. Tentar novamente a solicitação pode resolver o problema. |
|                    | *tempo_máximo_de_execução_excedido* | 403 | A solicitação não foi concluída no tempo máximo permitido. Tentar novamente a solicitação pode resolver o problema. |

### API pré-autorizada de SDKs {#enhanced-error-codes-lists-sdks-preauthorize-api}

Consulte a [seção](#enhanced-error-codes-list-rest-api-v1) anterior para obter possíveis Códigos de erro aprimorados que um aplicativo cliente pode encontrar quando integrado à API pré-autorizada dos SDKs de autenticação da Adobe Pass.

### REST API v2 {#enhanced-error-codes-lists-rest-api-v2}

A tabela abaixo lista possíveis Códigos de erro aprimorados que um aplicativo cliente pode encontrar ao ser integrado à API REST v2 de autenticação da Adobe Pass.

| Ação | Código | Status | Mensagem |
|------------------------------|--------------------------------------------------------|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **nenhum** | *invalid_parameter_service_provider* | 400 | O valor do parâmetro do provedor de serviços está ausente ou é inválido. |
|                              | *invalid_parameter_mvpd* | 400 | O valor do parâmetro mvpd está ausente ou é inválido. |
|                              | *código_de_parâmetro_inválido* | 400 | O valor do parâmetro de código está ausente ou é inválido. |
|                              | *recursos_de_parâmetro_inválidos* | 400 | O valor do parâmetro de recursos está ausente ou é inválido. |
|                              | *url_redirecionamento_de_parâmetro_inválido* | 400 | O valor do parâmetro de URL de redirecionamento está ausente ou é inválido. |
|                              | *invalid_parameter_partner* | 400 | O valor do parâmetro de parceiro está ausente ou é inválido. |
|                              | *resposta_saml_parâmetro_inválido* | 400 | O valor do parâmetro de resposta SAML está ausente ou é inválido. |
|                              | *informações_de_dispositivo_de_cabeçalho_inválido* | 400 | O valor do cabeçalho de informações do dispositivo está ausente ou é inválido. |
|                              | *identificador_de_dispositivo_de_cabeçalho_inválido* | 400 | O valor do cabeçalho do identificador de dispositivo está ausente ou é inválido. |
|                              | *identidade_de_cabeçalho_inválida_para_acesso_temporário* | 400 | A identidade do valor do cabeçalho de acesso temporário está ausente ou é inválida. |
|                              | *cabeçalho_inválido_pfs_permission_access_not_present* | 400 | O valor de status de acesso da permissão do cabeçalho de status da estrutura do parceiro não está presente. |
|                              | *cabeçalho_inválido_pfs_permissão_acesso_não_determinado* | 400 | O valor do status de acesso da permissão do cabeçalho de status da estrutura do parceiro não foi determinado. |
|                              | *cabeçalho_inválido_pfs_permission_access_not_granted* | 400 | O valor de status de acesso da permissão do cabeçalho de status da estrutura do parceiro não é concedido. |
|                              | *cabeçalho_inválido_pfs_provider_id_not_determination* | 400 | O valor da ID do provedor do cabeçalho de status da estrutura do parceiro não está associado a um mvpd conhecido. |
|                              | *incompatibilidade_de_id_de_provedor_de_cabeçalho_inválido* | 400 | O valor da ID do provedor do cabeçalho de status da estrutura do parceiro não corresponde ao mvpd enviado como parâmetro. |
|                              | *integração_inválida* | 400 | A integração entre o provedor de serviços especificado e o mvpd não existe ou está desabilitada. |
|                              | *sessão_de_autenticação_inválida* | 400 | A sessão de autenticação associada a esta solicitação está ausente ou é inválida. |
|                              | *pré-autorização_negada_por_mvpd* | 403 | O MVPD retornou uma decisão de &quot;Negação&quot; ao solicitar pré-autorização para o recurso especificado. |
|                              | *autorização_negada_por_mvpd* | 403 | O MVPD retornou uma decisão de &quot;Negação&quot; ao solicitar autorização para o recurso especificado. |
|                              | *autorização_negada_por_controles_pelos_pais* | 403 | O MVPD retornou uma decisão &quot;Negar&quot; devido às configurações de controles dos pais para o recurso especificado. |
|                              | *autorização_negada_pela_regra_de_degradação* | 403 | A integração entre o provedor de serviços especificado e o mvpd tem uma regra de degradação aplicada que nega a autorização para os recursos solicitados. |
|                              | *erro_interno_do_servidor* | 500 | A solicitação falhou devido a um erro interno do servidor. |
| **configuração** | *muitos_recursos* | 403 | A solicitação de autorização ou pré-autorização falhou porque muitos recursos foram consultados. Entre em contato com a equipe de suporte para configurar corretamente as limitações de autorização e pré-autorização. |
|                              | *certificado_de_metadados_de_usuário_de_configuração_inválido* | 500 | A configuração do certificado de metadados do usuário está ausente ou é inválida. |
|                              | *invalid_configuration_temporary_access* | 500 | A configuração de acesso temporário é inválida. |
|                              | *plataforma_de_configuração_inválida* | 500 | A configuração da plataforma está ausente ou é inválida para integração. |
|                              | *id_da_plataforma_de_configuração_inválida* | 500 | A configuração da ID da plataforma está ausente ou é inválida. |
|                              | *característica_de_plataforma_de_configuração_inválida* | 500 | A configuração de característica da plataforma está ausente ou é inválida. |
|                              | *características_de_categoria_de_plataforma_de_configuração_inválida* | 500 | A configuração de característica de categoria de plataforma está ausente ou é inválida. |
|                              | *serviços_de_plataforma_de_configuração_inválidos* | 500 | A configuração dos serviços de plataforma está ausente ou é inválida para integração. |
|                              | *configuração_inválida_mvpd_plataforma* | 500 | A configuração da plataforma mvpd está ausente ou é inválida para mvpd e plataforma. |
|                              | *invalid_configuration_mvpd_platform_boarding_status* | 500 | A configuração do status de integração da plataforma mvpd está ausente ou é inválida para mvpd e plataforma. |
|                              | *invalid_configuration_mvpd_platform_profile_exchange* | 500 | A configuração de troca de perfil da plataforma mvpd está ausente ou é inválida para mvpd e plataforma. |
| **registro de aplicativo** | *acesso_token_service_provider* inválido | 401 | O token de acesso é inválido devido ao provedor de serviço inválido. |
|                              | *invalid_access_token_client_application* | 401 | O token de acesso é inválido devido ao aplicativo cliente inválido. |
| **autenticação** | *perfil_autenticado_ausente* | 403 | O perfil autenticado associado a esta solicitação está ausente. |
|                              | *perfil_autenticado_expirado* | 403 | O perfil autenticado associado a esta solicitação expirou. |
|                              | *perfil_autenticado_invalidado* | 403 | O perfil autenticado associado a esta solicitação foi invalidado. |
|                              | *temporary_access_duration_limit_aded* | 403 | O limite de duração do acesso temporário foi excedido. |
|                              | *limite_de_recursos_de_acesso_temporário* | 403 | O limite de recursos de acesso temporário foi excedido. |
|                              | *autorização_negada_por_políticas_hba* | 403 | O MVPD retornou uma decisão de &quot;Negação&quot; devido a políticas de autenticação doméstica. A autenticação atual foi obtida por meio de um fluxo de autenticação baseado em casa e, mas o dispositivo não está mais em casa ao solicitar autorização para o recurso especificado. O usuário deve autenticar novamente com um MVPD compatível para continuar. |
|                              | *autorização_negada_por_sessão_invalidada* | 403 | A sessão de autenticação foi invalidada pelo provedor de identidade. O usuário deve autenticar novamente com um MVPD compatível para continuar. |
|                              | *identidade_não_reconhecida_por_mvpd* | 403 | A solicitação de autorização falhou devido ao fato de que a identidade do usuário não foi reconhecida pelo MVPD. |
| **tentar novamente** | *erro_recebido_de_rede* | 403 | Erro de leitura ao recuperar a resposta do serviço de parceiro associado. Tentar novamente a solicitação pode resolver o problema. |
|                              | *tempo_de_conexão_de_rede* | 403 | Houve um tempo limite de conexão com o serviço de parceiro associado. Tentar novamente a solicitação pode resolver o problema. |
|                              | *tempo_máximo_de_execução_excedido* | 403 | A solicitação não foi concluída no tempo máximo permitido. Tentar novamente a solicitação pode resolver o problema. |

## Tratamento de resposta {#enhanced-error-codes-response-handling}

>[!IMPORTANT]
>
> Há Códigos de erro aprimorados que podem ser manipulados automaticamente no código do aplicativo do cliente, como tentar novamente uma solicitação de autorização no caso de um tempo limite da rede ou exigir que o usuário reautentique quando a sessão expirar, mas outros tipos podem exigir alterações de configuração ou interação da equipe de atendimento ao cliente da Autenticação da Adobe Pass.
>
> <br/>
>
> Portanto, é importante coletar e fornecer informações completas sobre o erro ao criar um tíquete por meio do nosso [Zendesk](https://adobeprimetime.zendesk.com), para garantir que as alterações necessárias sejam feitas antes de iniciar o novo aplicativo ou novo recurso.

Em resumo, ao manipular respostas que contêm Códigos de erro aprimorados, você deve considerar o seguinte:

1. **Verificar ambos os valores de status**: sempre verifique o código de status de resposta HTTP e o campo &quot;status&quot; do Código de Erro Aprimorado. Elas podem ser diferentes e ambas fornecem informações valiosas.

1. **Informações de erro agnósticas para nível superior versus nível de item**: trate informações de erro de nível superior e nível de item agnósticas à maneira como são comunicadas, verifique se você pode lidar com ambas as formas de transmissão de Códigos de Erro Aprimorados.

1. **Lógica de repetição**: para erros que exijam uma nova tentativa, verifique se as novas tentativas são feitas com retrocesso exponencial para evitar sobrecarga do servidor. Além disso, no caso de APIs de autenticação da Adobe Pass que lidam com vários itens de uma só vez (por exemplo, API pré-autorizada), você deve incluir na solicitação repetida somente os itens marcados com &quot;tentar novamente&quot; e não a lista inteira.

1. **Alterações de configuração**: para erros que exijam alterações de configuração, verifique se as alterações necessárias foram feitas antes de iniciar o novo aplicativo ou novo recurso.

1. **Autenticação e autorização**: para erros relacionados à autenticação e autorização, você deve solicitar que o usuário se autentique novamente ou obtenha uma nova autorização, conforme necessário.

1. **Comentários do usuário**: opcional, use os campos de &quot;mensagem&quot; e (possíveis) &quot;detalhes&quot; legíveis por humanos para informar o usuário sobre o problema. A mensagem de texto &quot;details&quot; pode ser transmitida dos endpoints de pré-autorização ou autorização do MVPD ou do Programador ao aplicar as regras de degradação.
