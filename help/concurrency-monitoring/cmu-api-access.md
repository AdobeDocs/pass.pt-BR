---
title: Acesso à API CMU
description: Acesso à API CMU
source-git-commit: 598eb878168f6e352a8eae369cbc8cb833033328
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 0%

---

# Acesso à API de uso de monitoramento de simultaneidade {#cmu-api-usage-access}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada. Entre em contato com o representante da Adobe para tirar dúvidas sobre disponibilidade.

## Visão geral do procedimento de acesso {#api-access-procedure-overview}

Atualizamos o acesso aos relatórios CMU para que sejam compatíveis com o Protocolo de registro dinâmico do cliente OAuth 2.0. Um servidor de autorização OAuth 2.0 personalizado é implantado para atender às necessidades do aplicativo de Monitoramento de simultaneidade. \
Para que os aplicativos cliente utilizem a autorização OAuth 2.0, o servidor deve se registrar dinamicamente para obter informações específicas (credenciais do cliente) e poder interagir com ele. Como parte do processo de registro, o cliente deve apresentar um conjunto de metadados incorporados ao endpoint de registro do cliente.
Esses metadados são comunicados como uma declaração de software, que contém um &quot;software_id&quot; para permitir que nosso servidor de autorização correlacione diferentes instâncias de um aplicativo usando a mesma declaração de software.
Uma instrução de software é um JSON Web Token (JWT) que declara valores de metadados sobre o software cliente como um pacote. Quando apresentada ao servidor de autorização como parte de uma solicitação de registro do cliente, a instrução de software deve ser assinada digitalmente ou MACed usando JSON Web Signature (JWS). \
Você pode encontrar uma explicação mais detalhada sobre o que são as declarações de software e como elas funcionam na documentação oficial  <a href="https://datatracker.ietf.org/doc/html/rfc7591" target="_blank">[RFC7591]</a>.
Siga as etapas nas seções abaixo para obter acesso.

## Etapas do procedimento de acesso {#access-procedure-steps}

1. Ter um aplicativo registrado no servidor Adobe Pass DCR. Para esta etapa, entre em contato com nosso [Equipe de suporte](mailto:tve-support@adobe.com).
2. Obter a declaração de software
   1. Ir para o painel TVE <a href="https://console-preprod.auth.adobe.com/#!/" target="_blank"> Pré-produção </a>  ou <a href="https://console.auth.adobe.com/" target="_blank">PROD</a>
   2. Selecionar programador
   3. Ir para a guia Aplicativos
   4. Selecionar aplicativo
   5. Clique em Baixar Instrução de Software para obter um arquivo semelhante com a captura abaixo
      <figure>
          <img src="assets/software_statement_1_download.png"
               alt="Baixar Declaração de Software">
       </figure>
      <figure>
          <img src="assets/software_statement_2.png"
               alt="Amostra de instrução de software">
       </figure>

3. Obter token de acesso
   1. Obtenha credenciais do cliente usando a instrução de software obtida acima e executando a chamada abaixo. Dessa forma, um par client_id - client_secret será obtido, e poderá ser usado para obter o token de acesso.
      *Esta etapa não deve ser executada todas as vezes. Isso deve ser feito novamente somente quando as credenciais expiram.*
      <figure>
          <img src="assets/dcr_request_1_get_client_credentials.png"
               alt="Obter credenciais do cliente">
       </figure>

   2. Obtenha o token de acesso usando a chamada abaixo. Use esse token de acesso para chamar qualquer API CMU até que o token expire.
      *Esta etapa só deve ser executada se o último token gerado expirar.*
      <figure>
          <img src="assets/dcr_get_access_token_call.png"
               alt="Obter token de acesso">
       </figure>

4. Chame a API da CMU - consulte as informações relacionadas abaixo.
   <figure>
          <img src="assets/call_cmu_reports_sample.png"
               alt="Chamar API CMU">
       </figure>

## Informações relacionadas {#related-information}

* [Visão geral da CMU](/help/concurrency-monitoring/cm-usage-reports.md)
* [API CMU](/help/concurrency-monitoring/cmu-api.md)
