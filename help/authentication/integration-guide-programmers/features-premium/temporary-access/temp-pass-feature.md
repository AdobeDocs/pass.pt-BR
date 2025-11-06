---
title: Recurso TempPass
description: Recurso TempPass
exl-id: 1df14090-8e71-4e3e-82d8-f441d07c6f64
source-git-commit: 9e085ed0b2918eee30dc5c332b6b63b0e6bcc156
workflow-type: tm+mt
source-wordcount: '2203'
ht-degree: 0%

---

# Recurso TempPass {#temp-pass-feature}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

O TempPass é um recurso versátil que permite que programadores ofereçam acesso temporário a seu conteúdo protegido para usuários sem credenciais de conta válidas do MVPD. Ele serve como uma ferramenta eficaz para envolver os visualizadores, seja por meio de cenários de acesso básicos ou campanhas promocionais direcionadas.

O TempPass é uma solução avançada para os programadores:

* **Envolva os visualizadores:** Experimente um conteúdo premium para atrair novos assinantes.
* **Impulsione promoções:** execute campanhas direcionadas para aumentar a exposição do conteúdo e criar fidelidade à marca.
* **Manter Controle:** gerencie períodos de acesso, imponha limites e redefina o acesso conforme necessário para alinhar-se às metas comerciais.

O recurso TempPass é fornecido introduzindo um pseudo-MVPD (chamado de &quot;Temp Pass&quot;) na configuração do servidor de Autenticação da Adobe Pass como uma integração com o Programador participante. O recurso TempPass está disponível em duas configurações:

* [Basic TempPass](#basic-temp-pass) para acesso baseado em tempo.
* [TempPass promocional](#promotional-temp-pass) para acesso flexível orientado por campanha.

>[!IMPORTANT]
>
> O recurso TempPass é premium e requer uma licença atual da Adobe.

A tabela a seguir fornece uma breve comparação dos recursos Básico e Promocional do TempPass:

| **Recurso** | **TempPass Básico** | **TempPass Promocional** |
|-------------------------------|------------------------------|-------------------------------------------------------------------------------------------------|
| **Acesso ao conteúdo** | <ul><li>Baseado em tempo</li></ul> | <ul><li>Baseado em tempo</li><li>Limitado a um número máximo de recursos</li></ul> |
| **Segurança De Acesso Baseada Em** | <ul><li>ID do dispositivo</li></ul> | <ul><li>ID do dispositivo</li><li>Hash de informações do identificador de usuário fornecido (por exemplo, email)</li></ul> |
| **Códigos de erro aprimorados** | Disponível | Disponível |
| **Recurso de Redefinição TempPass** | Disponível | Disponível |

>[!IMPORTANT]
> 
> A Autenticação do Adobe Pass não inclui um mecanismo integrado para interromper automaticamente o fluxo contínuo depois que o tempo alocado (X minutos) tiver decorrido. É de responsabilidade dos Programadores impor restrições de acesso assim que o TempPass expirar durante um stream em andamento.

Esteja você fornecendo uma prévia da sua biblioteca de conteúdo ou promovendo um evento de letreiro, o TempPass oferece as ferramentas para expandir seu público enquanto mantém o controle sobre o acesso.

## TempPass básico {#basic-temp-pass}

O recurso básico TempPass permite que programadores forneçam acesso limitado no tempo ao conteúdo, atendendo a vários cenários:

* **Visualizações curtas:** ofereça visualizações breves, como um período de acesso diário de 10 minutos, para atrair assinantes em potencial.
* **Acesso Baseado em Eventos:** Habilite o acesso mais longo para eventos importantes, como uma sessão de 4 horas.
* **Acesso de Combinação:** durações de mixagem e correspondência, como um período de exibição estendido inicial seguido de visualizações diárias mais curtas durante vários dias.

Certos eventos podem exigir acesso livre em fases ao conteúdo, como um período inicial de acesso livre estendido (por exemplo, 4 horas), seguido por intervalos diários de acesso livre mais curtos (por exemplo, 10 minutos por dia). Para implementar esse cenário, os Programadores devem coordenar com seu representante da Adobe para configurar dois TempPass MVPDs personalizados para suas necessidades.

Por exemplo, para fornecer uma sessão gratuita inicial de 4 horas seguida de sessões gratuitas diárias de 10 minutos, o Adobe pode configurar para o Programador:

* **TempPass1**: configurado com um TTL (Time-To-Live) de 4 horas para cobrir o período inicial de acesso livre.
* **TempPass2**: configurado com um TTL (Time-To-Live) de 10 minutos para os intervalos de acesso gratuito diário subsequentes.

Para garantir a funcionalidade adequada para o acesso diário, o TempPass2 deve ser redefinido para todos os dispositivos às 00:00 horas todos os dias.

### Detalhes do recurso {#basic-temp-pass-feature-details}

**Parâmetros de configuração:**

* **TTL (Tempo de Vida Útil):** Programadores podem especificar a duração do acesso. Esse TTL baseado em relógio expira independentemente do tempo de exibição real.

**Identificação de usuário:**

O recurso básico TempPass usa o identificador do dispositivo como um parâmetro de identificação do usuário.

A tabela a seguir ajuda você a entender como os parâmetros de identificação de usuário influenciam a experiência de avaliação do usuário:

| Identificador do dispositivo | Resultado |
|-------------------|----------------|
| Novo | Nova avaliação |
| Existente | Avaliação existente |

**Exibir cálculo de tempo:**

O TTL representa a duração do tempo de solicitação de autorização inicial até o tempo de expiração, independentemente do tempo real gasto na exibição do conteúdo. Cada solicitação futura verifica a hora atual do servidor em relação à hora de expiração armazenada para autorizar o acesso.

**Autenticação:**

A autenticação não é necessária para o Basic TempPass, permitindo que você prossiga diretamente para a etapa de autorização.

**Autorização:**

Como não há interação com uma MVPD real, o MVPD básico &quot;Aprovação temporária&quot; autorizará qualquer recurso, pois a Aprovação temporária é válida. No caso de uma autorização bem-sucedida, a biblioteca Verificador de token de mídia permanece aplicável para verificar o token de mídia e garantir a validação de recursos antes de iniciar a reprodução do conteúdo.

A decisão de autorização é baseada nos parâmetros de identificação do usuário e no TTL configurado. Para obter uma autorização bem-sucedida para um recurso, as seguintes condições devem ser atendidas por uma solicitação válida:

* **Duração não consumida:** a hora de expiração é calculada adicionando-se a hora de solicitação de autorização inicial (salva em nossos bancos de dados) ao TTL configurado. A hora atual do servidor é comparada com essa hora de expiração para determinar se o TempPass ainda é válido.

No caso de um usuário exceder o TTL configurado, não será mais possível visualizar o conteúdo no mesmo dispositivo, a menos que o TempPass seja redefinido.

**Pré-autorização:**

Quando uma solicitação de pré-autorização é feita para um MVPD básico &quot;Aprovação temporária&quot;, a resposta retornará toda a lista de recursos da solicitação como pré-autorizados com sucesso. Esse comportamento reflete a lógica de autorização, já que as condições de autorização se baseiam em limites de tempo, em vez de recursos específicos. Enquanto a restrição de tempo for válida, os recursos solicitados serão autorizados.

**Logout:**

O logout não é necessário para o Basic TempPass, permitindo alternar diretamente para a etapa de autenticação usando um MVPD de usuário real.

**Dados e análises de rastreamento:**

Durante o fluxo TempPass básico, os dados de rastreamento usam uma versão com hash do identificador do dispositivo, com o identificador do MVPD definido como &quot;Temp Pass&quot;. Os programadores devem diferenciar as métricas TempPass das métricas padrão do MVPD em suas implementações de análise.

## TempPass Promocional {#promotional-temp-pass}

O recurso TempPass promocional expande os recursos do TempPass básico, projetado especificamente para executar campanhas promocionais. Esse recurso permite que os programadores envolvam os usuários, permitindo o acesso a um número predefinido de títulos do VOD por um período específico após coletar a identificação válida do usuário, como um endereço de email.

O TempPass Promocional inclui toda a funcionalidade do TempPass básico, com flexibilidade adicional para:

* Definição do número máximo de títulos do VOD acessíveis durante o período promocional.
* Definindo o período durante o qual o acesso promocional é válido.

Depois que um usuário excede os limites de acesso predefinidos (número de títulos ou duração do VOD), ele não poderá mais visualizar o conteúdo no mesmo dispositivo ou com o mesmo identificador de usuário, a menos que o TempPass seja redefinido.

### Detalhes do recurso {#promotional-temp-pass-feature-details}

**Parâmetros de configuração:**

* **Chave de Informações do Usuário:** a chave usada para comunicar o identificador fornecido pelo usuário, como um endereço de email (ou seja, a chave é o email).
* **Número de Recursos:** Define quantos títulos do VOD um usuário pode acessar.
* **TTL (Tempo de Vida Útil):** A duração durante a qual o usuário pode consumir os recursos permitidos.

**Identificação de usuário:**

O recurso TempPass promocional usa o hash do identificador fornecido pelo usuário na parte superior do identificador do dispositivo como parâmetros de identificação do usuário.

>[!IMPORTANT]
>
> A validação e o hash do identificador fornecido pelo usuário são gerenciados pelo Programador, não pelo Adobe. A Adobe não armazena informações pessoais identificáveis (PII). Dessa forma, o Programador é responsável por gerar e enviar um hash do identificador exclusivo fornecido pelo usuário ao interagir com as APIs de autenticação da Adobe Pass.

A Adobe recomenda usar a família **SHA-2** ou suas funções **SHA-256** e **SHA-512** específicas nos dados antes de enviá-los para a Adobe. Por exemplo, **SHA-256** sobre **&quot;user@domain.com&quot;** é **&quot;f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b88b88d09069fb7&quot;**.

A tabela a seguir ajuda você a entender como os parâmetros de identificação de usuário influenciam a experiência de avaliação do usuário:

| Hash do Identificador Fornecido pelo Usuário | Identificador do dispositivo | Resultado |
|-------------------------------|-------------------|---------------------------------------------------------|
| Novo | Novo | Nova avaliação |
| Existente | Novo | Avaliação existente (com base no hash de identificador fornecido pelo usuário) |
| Novo | Existente | Avaliação existente (com base no identificador do dispositivo) |
| Existente | Existente | Avaliação existente |

**Exibir cálculo de tempo:**

O TTL representa a duração do tempo de solicitação de autorização inicial até o tempo de expiração, independentemente do tempo real gasto na exibição do conteúdo. Cada solicitação futura verifica a hora atual do servidor em relação à hora de expiração armazenada para autorizar o acesso.

**Autenticação:**

A autenticação não é necessária para o TempPass Promocional, permitindo que você prossiga diretamente para a etapa de autorização.

Para apoiar a implementação do aplicativo do Programador, o TempPass Promocional expõe as seguintes informações de metadados do usuário, acessíveis através das chaves correspondentes:

* **`remaining_resources`**: indica o número de recursos que o usuário ainda está autorizado a consumir.
* **`used_assets`**: fornece uma lista de recursos que o usuário já consumiu.
* **`expiration_date`**: exibe a data de expiração do passe temporário promocional do usuário.

**Autorização:**

Como não há interação com uma MVPD real, o MVPD &quot;Temp Pass&quot; promocional autorizará qualquer recurso, pois o TempPass é válido. No caso de uma autorização bem-sucedida, a biblioteca Verificador de token de mídia permanece aplicável para verificar o token de mídia e garantir a validação de recursos antes de iniciar a reprodução do conteúdo.

A decisão de autorização baseia-se nos parâmetros de identificação do usuário e no número configurado de recursos e TTL. Para obter uma autorização bem-sucedida para um recurso, as seguintes condições devem ser atendidas por uma solicitação válida:

* **Duração não consumida:** a hora de expiração é calculada adicionando-se a hora de solicitação de autorização inicial (salva em nossos bancos de dados) ao TTL configurado. A hora atual do servidor é comparada com essa hora de expiração para determinar se o TempPass ainda é válido.
* **Recursos não consumidos:** o número de recursos consumidos é rastreado (salvo em nossos bancos de dados). O número de recursos consumidos é comparado ao número configurado de recursos para determinar se o TempPass ainda é válido.

No caso de um usuário que exceda o TTL ou o número de recursos configurados, ele não poderá mais visualizar o conteúdo no mesmo dispositivo ou com o mesmo identificador fornecido pelo usuário, a menos que o TempPass seja redefinido.

**Pré-autorização:**

Quando uma solicitação de pré-autorização é feita para um MVPD Promocional &quot;Aprovação temporária&quot;, a resposta retornará toda a lista de recursos da solicitação como pré-autorizados com êxito. Esse comportamento reflete a lógica de autorização, considerando que as condições de autorização se baseiam em limites de tempo e no número total de recursos acessados, em vez de recursos específicos. Enquanto a restrição de tempo for válida e o limite de recursos não for excedido, os recursos solicitados serão autorizados.

**Logout:**

O logout não é necessário para o TempPass promocional, permitindo alternar diretamente para a etapa de autenticação usando um MVPD de usuário real.

**Dados e análises de rastreamento:**

Durante o fluxo promocional do TempPass, os dados de rastreamento usam uma versão com hash do identificador do dispositivo, com o identificador do MVPD definido como &quot;Temp Pass&quot;. Os programadores devem diferenciar as métricas TempPass das métricas padrão do MVPD em suas implementações de análise.

## Redefinir o acesso à API TempPass {#reset-tempass-api-access}

Antes de acessar a API Reset TempPass, você deve concluir as etapas necessárias no processo de Registro de Cliente Dinâmico (DCR). Esse processo obrigatório garante que você tenha o token de acesso necessário para interagir com a API Reset TempPass.

Para obter instruções abrangentes, consulte a documentação [Visão geral do registro dinâmico do cliente](/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md).

## Redefinir API TempPass - DELETE /reset-tempass/v3/reset {#reset-tempass-v3-reset}

Para redefinir um TempPass específico para um dispositivo ou para todos os dispositivos, a Autenticação Adobe Pass fornece aos programadores uma API que funciona para o TempPass básico e promocional.

### Solicitação {#reset-tempass-v3-reset-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">host</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">caminho</td>
      <td>/reset-tempass/v3/reset</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parâmetros de consulta</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">requestor_id</td>
      <td>O identificador exclusivo interno associado ao provedor de serviços durante o processo de integração.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>O identificador exclusivo interno associado ao TempPass durante o processo de integração.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">device_id</td>
      <td>
            O identificador de dispositivo para o qual essa operação de redefinição é válida.
            <br/><br/>
            Se nenhum valor for fornecido, a operação de redefinição se aplica a todos os dispositivos.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorização</td>
      <td>A geração da carga do token do portador está descrita na documentação <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md">Recuperar token de acesso</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
</table>

### Resposta {#reset-tempass-v3-reset-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Código</th>
      <th style="background-color: #EFF2F7;">Texto</th>
      <th style="background-color: #EFF2F7;">Descrição</th>
   </tr>
   <tr>
      <td>204</td>
      <td>Sem conteúdo</td>
      <td>
        A redefinição foi bem-sucedida.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Solicitação inválida</td>
      <td>
        A solicitação é inválida, o cliente precisa corrigir a solicitação e tentar novamente.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Não autorizado</td>
      <td>
        O token de acesso é inválido, o cliente precisa obter um novo token de acesso e tentar novamente. Para obter mais detalhes, consulte a documentação <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Visão geral do registro dinâmico do cliente</a>.
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>Proibido</td>
      <td>
        O token de acesso é inválido, o cliente precisa obter novas credenciais de cliente e um novo token de acesso e tentar novamente. Para obter mais detalhes, consulte a documentação <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Visão geral do registro dinâmico do cliente</a>.
      </td>
   </tr>
</table>

### Amostras {#reset-tempass-v3-reset-samples}

#### Redefinir TempPass para um dispositivo específico {#reset-tempass-v3-reset-specific-device}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=ba23d141-d715-561c-94f4-e9e4c966b1eb"
```

#### Redefinir TempPass para todos os dispositivos {#reset-tempass-v3-reset-all-devices}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset?requestor_id=REF30&mvpd_id=TempPass&device_id=all"
```

## Redefinir API TempPass - DELETE /reset-tempass/v3/reset/generic {#reset-tempass-v3-reset-generic}

Para redefinir um TempPass específico para uma chave genérica (hash de identificador fornecido pelo usuário) ou todas as chaves, a Autenticação Adobe Pass fornece aos Programadores uma API que funciona para o TempPass Promocional.

### Solicitação {#reset-tempass-v3-reset-generic-request}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">HTTP</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">host</td>
      <td>mgmt.auth.adobe.com</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">caminho</td>
      <td>/reset-tempass/v3/reset/generic</td>
      <td></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">método</td>
      <td>DELETE</td>
      <td></td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Parâmetros de consulta</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">requestor_id</td>
      <td>O identificador exclusivo interno associado ao provedor de serviços durante o processo de integração.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">mvpd_id</td>
      <td>O identificador exclusivo interno associado ao TempPass durante o processo de integração.</td>
      <td><i>obrigatório</i></td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">key</td>
      <td>
            O hash identificador fornecido pelo usuário para o qual esta operação de redefinição é válida.
            <br/><br/>
            Se nenhum valor for fornecido, a operação de redefinição se aplica a todos os usuários.
      </td>
      <td>opcional</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;">Cabeçalhos</th>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;"></th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;">Autorização</td>
      <td>A geração da carga do token do portador está descrita na documentação <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/apis/dynamic-client-registration-apis-retrieve-access-token.md">Recuperar token de acesso</a>.</td>
      <td><i>obrigatório</i></td>
   </tr>
</table>

### Resposta {#reset-tempass-v3-reset-generic-response}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Código</th>
      <th style="background-color: #EFF2F7;">Texto</th>
      <th style="background-color: #EFF2F7;">Descrição</th>
   </tr>
   <tr>
      <td>204</td>
      <td>Sem conteúdo</td>
      <td>
        A redefinição foi bem-sucedida.
      </td>
   </tr>
   <tr>
      <td>400</td>
      <td>Solicitação inválida</td>
      <td>
        A solicitação é inválida, o cliente precisa corrigir a solicitação e tentar novamente.
      </td>
   </tr>
   <tr>
      <td>401</td>
      <td>Não autorizado</td>
      <td>
        O token de acesso é inválido, o cliente precisa obter um novo token de acesso e tentar novamente. Para obter mais detalhes, consulte a documentação <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Visão geral do registro dinâmico do cliente</a>.
      </td>
   </tr>
   <tr>
      <td>403</td>
      <td>Proibido</td>
      <td>
        O token de acesso é inválido, o cliente precisa obter novas credenciais de cliente e um novo token de acesso e tentar novamente. Para obter mais detalhes, consulte a documentação <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-dcr/dynamic-client-registration-overview.md">Visão geral do registro dinâmico do cliente</a>.
      </td>
   </tr>
</table>

### Amostras {#reset-tempass-v3-reset-generic-samples}

#### Redefinir TempPass para uma chave específica {#reset-tempass-v3-reset-specific-key}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=f7ee5ec7312165148b69fcca1d29075b14b8aef0b5048a332b18b88d09069fb7"
```

#### Redefinir TempPass para todas as chaves {#reset-tempass-v3-reset-all-keys}

```curl
$ curl -H "Authorization: Bearer <access_token_here>" -X DELETE -v "https://mgmt.auth.adobe.com/reset-tempass/v3/reset/generic?requestor_id=REF30&mvpd_id=TempPass&key=all"
```

## REST API V2 {#rest-api-v2}

Para aproveitar o recurso TempPass, é necessário implementar atualizações de código para modificar a forma como o aplicativo TVE (TV Everywhere) interage com a [REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) da Adobe Pass Authentication.

Para obter um guia abrangente sobre essas atualizações e os fluxos de trabalho associados, consulte a documentação dos [Fluxos de Acesso Temporário](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/temporary-access-flows/rest-api-v2-access-temporary-flows.md).
