---
title: Códigos de erro aprimorados
description: Códigos de erro aprimorados
exl-id: 2b0a9095-206b-4dc7-ab9e-e34abf4d359c
source-git-commit: 87639ad93d8749ae7b1751cd13a099ccfc2636ac
workflow-type: tm+mt
source-wordcount: '2207'
ht-degree: 2%

---

# Códigos de erro aprimorados {#enhanced-error-codes}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview}

Este documento descreve a lista de códigos de erro da API e informações adicionais sobre o erro retornadas aos aplicativos.

Para usar Códigos de erro aprimorados no aplicativo Programadores, é necessário fazer uma solicitação à equipe de suporte para habilitá-la com uma alteração de configuração.

## Tratamento de erro de resposta {#response-error-handling}

Na maioria dos cenários, a API de autenticação da Adobe Pass inclui informações de erro adicionais no corpo da resposta para fornecer o **contexto significativo** do motivo pelo qual um determinado erro ocorreu e/ou possíveis soluções para corrigir automaticamente o problema.  *No entanto, em alguns casos específicos envolvendo fluxos de autenticação ou logout, os serviços de Autenticação da Adobe Pass podem retornar uma resposta de HTML ou um corpo vazio. Consulte a documentação da API para obter mais informações.*

Embora certos tipos de erros possam ser tratados automaticamente (como tentar novamente uma solicitação de autorização no caso de um tempo limite da rede ou exigir que o usuário se autentique novamente se a sessão expirar), outros tipos podem exigir alterações de configuração ou interação com a equipe de atendimento ao cliente. É importante para os programadores coletar e fornecer informações completas sobre os erros em tais casos.

A API de autenticação da Adobe Pass retorna códigos de status HTTP no intervalo de 400 a 500 para indicar falhas ou erros. Cada código de status HTTP tem determinadas implicações:

- Os códigos de erro 4xx implicam que o erro é gerado pelo cliente e que o cliente precisa fazer trabalho adicional para corrigi-lo (por exemplo, obter um token de acesso antes de chamar APIs protegidas ou fornecer qualquer parâmetro necessário)

- Os códigos de erro 5xx significam que o erro é gerado pelo servidor e que ele precisa fazer trabalho adicional para corrigi-lo.

As informações adicionais sobre o erro são incluídas no campo &quot;erro&quot; no corpo da resposta.

<table>
<thead>
  <tr>
    <th>Nome</th>
    <th>Tipo</th>
    <th>Exemplo</th>
    <th>Descrição</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>erro</td>
    <td><i>objeto</i></td>
    <td><strong>JSON</strong>
    <br>
    <code>{<br>&nbsp;&nbsp;&nbsp;&nbsp;"status" : 403,<br>&nbsp;&nbsp;&nbsp;&nbsp;"code" : "network_connection_failure",<br>&nbsp;&nbsp;&nbsp;&nbsp;"message" : "Unable to contact your TV provider<br>&nbsp;&nbsp;&nbsp;&nbsp;services",<br>&nbsp;&nbsp;&nbsp;&nbsp;"helpUrl" : "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",<br>&nbsp;&nbsp;&nbsp;&nbsp;"trace" : "12f6fef9-d2e0-422b-a9d7-60d799abe353",<br>&nbsp;&nbsp;&nbsp;&nbsp;"action" : "retry"<br>}
    </code>
    <p>
    <p>
    <strong>XML</strong>
    <br>
    <code>&lt;error&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;status&gt;403&lt;/status&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;code&gt;network_connection_failure&lt;/code&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;message&gt;Unable to contact your TV provider services&lt;/message&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;helpUrl&gt;https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html&lt;/helpUrl&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;trace>12f6fef9-d2e0-422b-a9d7-60d799abe353&lt;/trace&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&lt;action>retry&lt;/action&gt;<br>&lt;/error&gt;
    </code>
    </td>
    <td>Refere-se aos objetos de erro ou coleção coletados durante a tentativa de concluir a solicitação.</td>
  </tr>
</tbody>
</table>

</br>

As APIs do Adobe Pass que lidam com vários itens (API de pré-autorização etc.) podem indicar que o processamento falhou para um item específico e foi bem-sucedido para outros itens usando informações de erro no nível do item. Nesse caso, o objeto ***&quot;error&quot;*** está localizado no nível de item e o corpo da resposta pode conter vários objetos ***&quot;errors&quot;*** - consulte a documentação da API.

</br>

**Exemplo com êxito parcial e erro no nível do item**

```json
{
   "resources" : [
        {
            "id" : "TestStream1",
            "authorized" : true
        },
        {
            "id" : "TestStream2",
            "authorized" : false,
            "error" : {
 
               "status" : 403,
               "code" : "network_connection_failure",
               "message" : "Unable to contact your TV provider services",
               "details" : "",
               "helpUrl" : "https://experienceleague.adobe.com/docs/pass/authentication/auth-features/error-reportn/enhanced-error-codes.html",
               "trace" : "8bcb17f9-b172-47d2-86d9-3eb146eba85e",
               "action" : "retry"
            }
 
        }
    ]
}
```

</br>

Cada objeto de erro tem os seguintes parâmetros:

| Nome | Tipo | Exemplo | Restrito | Descrição |
|---|---|----|:---:|---|
| *status* | *inteiro* | *403* | &amp;verificar; | O código de status HTTP de resposta conforme documentado em RFC 7231 (<https://tools.ietf.org/html/rfc7231#section-6>) <ul><li>400 Solicitação incorreta</li><li>401 Não autorizado</li><li>403 Proibido</li><li>404 Não encontrado</li><li>405 Método não permitido</li><li>Conflito 409</li><li>410 Não existe mais</li><li>412 Falha na pré-condição</li><li>429 Muitas solicitações</li><li>Erro do servidor de intervalo 500</li><li>503 Serviço indisponível</li></ul> |
| *código* | *cadeia de caracteres* | *falha_de_conexão_de_rede* | &amp;verificar; | O código de erro padrão de autenticação do Adobe Pass. A lista completa dos códigos de erro está incluída abaixo. |
| *mensagem* | *cadeia de caracteres* | *Não é possível contatar os serviços do provedor de TV* | | Mensagem legível por humanos que pode ser exibida ao usuário final. |
| *detalhes* | *cadeia de caracteres* | *O pacote de assinatura não inclui o canal &quot;Online&quot;* | | Em alguns casos, uma mensagem detalhada é fornecida pelos endpoints de autorização do MVPD ou pelo Programador através de regras de degradação. <p> Observe que, se nenhuma mensagem personalizada tiver sido recebida dos serviços de parceiro, esse campo poderá não estar presente nos campos de erro. |
| *urlAjuda* | *url* | &quot;`http://`&quot; | | Um URL que vincula mais informações sobre por que este erro ocorreu e possíveis soluções. <p>O URI representa um URL absoluto e não deve ser deduzido do código de erro. Dependendo do contexto de erro, um URL diferente pode ser fornecido. Por exemplo, o mesmo código de erro bad_request produzirá URLs diferentes para os serviços de autenticação e autorização. |
| *rastreamento* | *cadeia de caracteres* | *12f6fef9-d2e0-422b-a9d7-60d799abe353* | | Um identificador exclusivo para essa resposta que pode ser usado ao entrar em contato com o suporte para identificar problemas específicos em cenários mais complexos. |
| *ação* | *cadeia de caracteres* | *tentar novamente* | &amp;verificar; | Ação recomendada para corrigir a situação: <ul><li> *nenhum* - Infelizmente não há nenhuma ação predefinida para corrigir esse problema. Isso pode indicar uma invocação incorreta da API pública</li><li>*configuração* - Uma alteração de configuração é necessária por meio do painel TVE ou contatando o suporte. </li><li>*registro de aplicativo* - O aplicativo deve se registrar novamente. </li><li>*autenticação* - O usuário deve autenticar ou autenticar novamente. </li><li>*autorização* - O usuário deve obter autorização para o recurso específico. </li><li>*degradação* - alguma forma de degradação deve ser aplicada. </li><li>*repetir* - Tentar novamente a solicitação pode resolver o problema</li><li>*repetir-após* - Tentar novamente a solicitação após o período de tempo indicado pode resolver o problema.</li></ul> |

</br>

**Notas:**

- ***Restrito*** coluna *indica se o respectivo valor de campo representa um conjunto finito* (por exemplo, códigos de status HTTP existentes para o campo &quot;*status*&quot;). Atualizações futuras dessa especificação poderão adicionar valores à lista restrita, mas não removerão nem alterarão as existentes. Campos irrestritos geralmente podem conter quaisquer dados, mas pode haver limitações em vigor para garantir um tamanho razoável.

- Cada resposta de Adobe conterá um &quot;Adobe-Request-Id&quot; que identifica a solicitação do cliente em todos os nossos serviços HTTP. O campo &quot;**rastreamento**&quot; complementa isso e eles devem ser relatados juntos.

## Códigos de status HTTP e códigos de erro {#http-status-codes-and-error-codes}

As inconsistências entre vários códigos de erro e seus códigos de status HTTP associados se devem aos requisitos de compatibilidade com versões anteriores de sdks e aplicativos mais antigos ( para
o exemplo *unknown\_application* gera 400 Bad Request enquanto *unknown\_software\_statement* produz 401 Unauthorized). A solução dessas inconsistências será direcionada em iterações futuras.

## Ações e códigos de erro {#actions-and-error-codes}

Para a maioria dos códigos de erro, várias ações podem ser qualificadas como caminhos para a correção do problema em andamento ou até mesmo várias ações podem ser necessárias para corrigi-las automaticamente. Optamos por indicar aquele com a maior probabilidade de correção do erro. As **ações** podem ser divididas em três categorias:

1. aqueles que tentam corrigir o contexto da solicitação (retry, retry-after)
1. aqueles que tentam corrigir o contexto de usuário no aplicativo
(registro de aplicativos, autenticação, autorização)
1. aqueles que tentam corrigir o contexto de integração entre um aplicativo
e um provedor de identidade (configuração, degradação)

Para a primeira categoria (repetir e repetir após), simplesmente repetir a mesma solicitação pode ser suficiente para resolver o problema. No caso de APIs que lidam com vários itens, o aplicativo deve repetir a solicitação e incluir apenas os itens com a ação &quot;repetir&quot; ou &quot;tentar depois&quot;. Para a ação &quot;*repetir-após*&quot;, um cabeçalho &quot;<u>Repetir-Após</u>&quot; indicará quantos segundos o aplicativo deve aguardar antes de repetir a solicitação.

Para a 2ª e a 3ª categorias, a implementação da ação real é altamente dependente dos recursos do aplicativo. Por exemplo, &quot;*degradação*&quot; pode ser implementada como &quot;alternar para etapas temporárias de 15 minutos para permitir a reprodução do conteúdo pelos usuários&quot; ou como &quot;ferramenta automática para aplicar a degradação AUTHN-ALL ou AUTHZ-ALL para sua integração com o MVPD especificado&quot;. Semelhante a uma ação &quot;*authentication*&quot; pode disparar uma autenticação passiva (autenticação de canal traseiro) em um tablet e um fluxo de autenticação de 2ª tela completo em TVs conectadas. É por isso que optamos por fornecer URLs completos com esquema e todos os parâmetros.

## Códigos de erro {#error-codes}

A tabela de erros abaixo lista os possíveis códigos de erro, mensagens associadas e possíveis ações.

| Ação | Código de erro | Código de status HTTP | Descrição |
|---|---|---|---|
| **nenhum** | *autorização_negada_por_mvpd* | 403 | O MVPD retornou uma decisão de &quot;Negação&quot; ao solicitar autorização para o recurso especificado. |
|  | *autorização_negada_por_controles_pelos_pais* | 403 | O MVPD retornou uma decisão &quot;Negar&quot; devido às configurações de controles dos pais para o recurso especificado. |
|  | *autorização_negada_por_programador* | 403 | A regra de degradação aplicada pelo Programador impõe uma decisão de &quot;Negação&quot; para o usuário atual. |
|  | *bad_request* | 400 | A solicitação de API é inválida ou está formada incorretamente. Consulte a documentação da API para determinar os requisitos da solicitação. |
|  | *individualization_service_unavailable* | 503 | A solicitação falhou devido ao serviço de individualização estar indisponível. |
|  | *erro_interno* | 500 | A solicitação falhou devido a um erro interno do servidor. |
|  | *hora_do_cliente_inválido* | 400 | A data/hora/fuso horário do computador cliente não está definida corretamente. Isso provavelmente levará a erros de autenticação/autorização. |
|  | *esquema_personalizado_inválido* | 400 | O esquema personalizado especificado usado no registro do aplicativo não é reconhecido. Verifique a configuração do painel TVE para obter os valores adequados do esquema personalizado. |
|  | *domínio_inválido* | 400 | O solicitante está usando um domínio inválido. Todos os domínios usados por uma ID de solicitante específica precisam ser colocados na lista de permissões pelo Adobe. |
|  | *cabeçalho_inválido* | 400 | A solicitação falhou porque continha um cabeçalho inválido. Consulte a documentação da API para determinar quais cabeçalhos são válidos para sua solicitação e se há limitações para o valor deles. |
|  | *método_http_inválido* | 405 | O método HTTP associado à solicitação não é compatível. Consulte a documentação da API para determinar os métodos HTTP compatíveis com sua solicitação. |
|  | *valor_de_parâmetro_inválido* | 400 | A solicitação falhou porque continha um parâmetro ou valor de parâmetro inválido. Consulte a documentação da API para determinar quais parâmetros são válidos para sua solicitação e se há limitações para o valor deles. |
|  | *valor_de_recurso_inválido* | 400 | A solicitação falhou porque um recurso inválido ou malformado foi usado. Revise a documentação da API para determinar como os recursos complexos devem ser codificados para sua solicitação e se há limitações para seu valor e/ou tamanho. |
|  | *código_de_registro_inválido* | 404 | O código de registro especificado não é mais válido ou expirou. |
|  | *configuração_de_serviço_inválida* | 500 | A solicitação falhou devido à configuração incorreta do serviço. |
|  | *cabeçalho_de_autenticação_ausente* | 400 | A solicitação falhou porque não contém o cabeçalho de autenticação necessário para a API específica. |
|  | *mapeamento_de_recursos_ausentes* | 400 | Não há mapeamento correspondente para o recurso especificado. Entre em contato com o suporte para corrigir o mapeamento necessário. |
|  | *pré-autorização_negada_por_mvpd* | 403 | O MVPD retornou uma decisão de &quot;Negação&quot; ao solicitar pré-autorização para o recurso especificado. |
|  | *pré-autorização_negada_por_programador* | 403 | As regras de degradação aplicadas pelo Programador impõem uma decisão de &quot;Negação&quot; para o usuário atual. |
|  | *registration_code_service_unavailable* | 503 | A solicitação falhou porque o serviço de código de registro não está disponível. |
|  | *serviço_indisponível* | 503 | A solicitação falhou devido ao fato de que o serviço de autenticação ou autorização não está disponível. |
|  | *access_token_unavailable* | 400 | A solicitação falhou devido a um erro inesperado ao recuperar o token de acesso. Verifique a configuração do painel TVE para obter instruções de software disponíveis e esquemas personalizados registrados. |
|  | *versão_cliente_sem_suporte* | 400 | Esta versão do SDK de autenticação da Adobe Pass é muito antiga e não é mais suportada. Consulte a documentação da API para conhecer as etapas necessárias para atualizar para a versão mais recente. |
| **configuração** | *rede_necessária_ssl* | 403 | Há um problema de conexão SSL com o serviço de parceiro de destino. Contate a equipe de suporte. |
|  | *muitos_recursos* | 403 | A solicitação de autorização ou pré-autorização falhou porque muitos recursos foram consultados. Entre em contato com a equipe de suporte para configurar corretamente as limitações de autorização e pré-autorização. |
|  | *programador_desconhecido* | 400 | O programador ou provedor de serviços não é reconhecido. Use o Painel TVE para registrar o programador especificado. |
|  | *aplicativo_desconhecido* | 400 | O aplicativo não é reconhecido. Use o Painel TVE para registrar o aplicativo especificado. |
|  | *integração_desconhecida* | 400 | A integração entre o programador especificado e o provedor de identidade não existe. Use o painel TVE para criar a integração necessária. |
|  | *instrução_desconhecida_de_software* | 401 | A instrução de software associada ao token de acesso não é reconhecida. Entre em contato com a equipe de suporte para esclarecer o status da declaração do software. |
| **registro de aplicativo** | *access_token_expired* | 401 | O token de acesso expirou. O aplicativo deve atualizar o token de acesso conforme indicado na documentação da API. |
|  | *assinatura_token_de_acesso_inválida* | 401 | A assinatura do token de acesso não é mais válida. O aplicativo deve atualizar o token de acesso conforme indicado na documentação da API. |
|  | *id_cliente_inválida* | 401 | O identificador de cliente associado não é reconhecido. O aplicativo deve seguir o processo de registro conforme indicado na documentação da API. |
| **autenticação** | *autenticação_sessão_expirada* | 410 | A sessão de autenticação atual expirou. O usuário deve autenticar novamente com um MVPD compatível para continuar. |
|  | *autenticação_sessão_ausente* | 401 | Não foi possível recuperar a sessão de autenticação associada a esta solicitação. O usuário deve autenticar novamente com um MVPD compatível para continuar. |
|  | *autenticação_sessão_invalidada* | 401 | A sessão de autenticação foi invalidada pelo provedor de identidade. O usuário deve autenticar novamente com um MVPD compatível para continuar. |
|  | *autenticação_sessão_emissor_incompatibilidade* | 400 | A solicitação de autorização falhou devido ao fato de que o MVPD indicado para o fluxo de autorização é diferente daquele que emitiu a sessão de autenticação. O usuário deve autenticar novamente com o MVPD desejado para continuar. |
|  | *autorização_negada_por_políticas_hba* | 403 | O MVPD retornou uma decisão de &quot;Negação&quot; devido a políticas de autenticação doméstica. A autenticação atual foi obtida usando um HBA (fluxo de autenticação baseado em início), mas o dispositivo não está mais em casa ao solicitar autorização para o recurso especificado. O usuário deve autenticar novamente com um MVPD compatível para continuar. |
|  | *identidade_não_reconhecida_por_mvpd* | 403 | A solicitação de autorização falhou devido ao fato de que a identidade do usuário não foi reconhecida pelo MVPD. |
| **autorização** | *autorização_expirada* | 410 | A autorização anterior do recurso especificado expirou. O usuário deve obter uma nova autorização para continuar. |
|  | *autorização_não_encontrada* | 404 | Nenhuma autorização foi encontrada para o recurso especificado. O usuário deve obter uma nova autorização para continuar. |
|  | *device_identifier_mismatch* | 403 | O identificador de dispositivo especificado não corresponde à identificação do dispositivo de autorização. O usuário deve obter uma nova autorização para continuar. |
| **tentar novamente** | *falha_de_conexão_de_rede* | 403 | Falha de conexão com o serviço de parceiro associado. Tentar novamente a solicitação pode resolver o problema. |
|  | *tempo_de_conexão_de_rede* | 403 | Houve um tempo limite de conexão com o serviço de parceiro associado. Tentar novamente a solicitação pode resolver o problema. |
|  | *erro_recebido_de_rede* | 403 | Erro de leitura ao recuperar a resposta do serviço de parceiro associado. Tentar novamente a solicitação pode resolver o problema. |
|  | *tempo_máximo_de_execução_excedido* | 403 | A solicitação não foi concluída no tempo máximo permitido. Tentar novamente a solicitação pode resolver o problema. |
| **tentar-após** | *muitas_solicitações* | 429 | Muitas solicitações foram enviadas em um determinado intervalo. O aplicativo pode repetir a solicitação após o período sugerido. |
|  | *limite_de_taxa_de_usuário* | 429 | Muitas solicitações foram emitidas por um usuário específico em um determinado intervalo. O aplicativo pode repetir a solicitação após o período sugerido. |
