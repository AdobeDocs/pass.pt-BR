---
title: Lista de verificação da REST API V2
description: Lista de verificação da REST API V2
source-git-commit: f0001d86f595040f4be74f357c95bd2919dadf15
workflow-type: tm+mt
source-wordcount: '2535'
ht-degree: 0%

---

# Lista de verificação da REST API V2 {#rest-api-v2-checklist}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

Este documento agrega em um local os requisitos obrigatórios e as práticas recomendadas para programadores que implementam aplicativos clientes que consomem a [API REST V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) da Autenticação do Adobe Pass.

O documento a seguir deve ser considerado parte de seus critérios de aceitação ao implementar a REST API V2 e deve ser usado como uma lista de verificação para garantir que todas as etapas necessárias tenham sido tomadas para obter uma integração bem-sucedida.

## Requisitos obrigatórios {#mandatory-requirements}

### 1. Fase de registro {#mandatory-requirements-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisitos</th>
      <th style="background-color: #EFF2F7;">Riscos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Escopo do aplicativo registrado</i></td>
      <td>Use um aplicativo registrado com escopo REST API v2.</td>
      <td>Riscos de acionamento de respostas de erro "Não autorizado" HTTP 401, sobrecarga de recursos do sistema e latência crescente.</td>
   </tr>
    <tr>
      <td style="background-color: #DEEBFF;"><i>Armazenamento em cache de credenciais do cliente</i></td>
      <td>Armazene as credenciais do cliente no armazenamento persistente e reutilize-as para cada solicitação de token de acesso.</td>
      <td>Há risco de perda de autenticação quando as credenciais do cliente forem geradas novamente.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Cache de tokens de acesso</i></td>
      <td>Armazene os tokens de acesso no armazenamento persistente e reutilize-os até que expirem — não solicite um novo token para cada chamada REST API v2.</td>
      <td>Riscos de sobrecarga dos recursos do sistema, aumento da latência e possível acionamento de respostas de erro HTTP 429 "Muitas solicitações".</td>
   </tr>
</table>

### 2. Fase de configuração {#mandatory-requirements-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisitos</th>
      <th style="background-color: #EFF2F7;">Riscos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Recuperação de configuração</i></td>
      <td>Recupere a resposta de configuração somente quando for necessário solicitar que o usuário selecione o MVPD (provedor de TV) antes da Fase de autenticação.<br/><br/>Não há necessidade de recuperar a resposta da configuração quando:<ul><li>O usuário já está autenticado.</li><li>O usuário recebe acesso temporário.</li><li>A autenticação do usuário expirou, mas o usuário pode ser solicitado a confirmar que ainda é assinante do MVPD selecionado anteriormente.</li></ul></td>
      <td>Riscos de sobrecarga dos recursos do sistema e aumento da latência.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Armazenamento em Cache de Seleção do Provedor de TV</i></td>
      <td>Armazene a seleção do provedor de TV por assinatura (MVPD) do usuário no armazenamento persistente para usá-la em todas as fases subsequentes:<ul><li>No armazenamento de resposta de configuração, o usuário selecionou MVPD "id".</li><li>No armazenamento de resposta de configuração, o usuário selecionou "displayName" do MVPD.</li><li>No armazenamento de resposta de configuração, o usuário selecionou "logoUrl" do MVPD.</li></ul></td>
      <td>Riscos de sobrecarga dos recursos do sistema e aumento da latência.</td>
   </tr>
</table>

### 3. Fase de autenticação {#mandatory-requirements-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisitos</th>
      <th style="background-color: #EFF2F7;">Riscos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Início do mecanismo de pesquisa</i></td>
      <td>Inicie o mecanismo de sondagem sob as seguintes condições:<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md">Autenticação executada no aplicativo (tela) primário</a></b><ul><li>O aplicativo principal (streaming) deve iniciar o polling quando o usuário atingir a página de destino final, depois que o componente do navegador carregar o URL especificado para o parâmetro "redirectUrl" na solicitação de ponto de extremidade de sessões.</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md">Autenticação executada em um aplicativo secundário (tela)</a></b><ul><li>O aplicativo principal (transmissão) deve iniciar o polling assim que o usuário iniciar o processo de autenticação, logo após receber a resposta do endpoint de sessões e exibir o código de autenticação ao usuário.</li></ul></td>
      <td>Riscos de sobrecarga dos recursos do sistema, aumento da latência e possível acionamento de respostas de erro HTTP 429 "Muitas solicitações".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Interrupção do mecanismo de pesquisa</i></td>
      <td>Pare o mecanismo de sondagem sob as seguintes condições:<br/><br/><b>Autenticação bem-sucedida</b><ul><li>As informações de perfil do usuário foram recuperadas com êxito, confirmando o status de autenticação. Portanto, a sondagem não é mais necessária.</li></ul><br/><b>Sessão de autenticação e expiração do código</b><ul><li>A sessão de autenticação e o código expiram, o usuário deve reiniciar o processo de autenticação e a pesquisa usando o código de autenticação anterior deve ser interrompida imediatamente.</li></ul><br/><b>Novo código de autenticação gerado</b><ul><li>Se o usuário solicitar um novo código de autenticação, a sessão existente será invalidada e a pesquisa usando o código de autenticação anterior deverá ser interrompida imediatamente.</li></ul></td>
      <td>Riscos de sobrecarga dos recursos do sistema, aumento da latência e possível acionamento de respostas de erro HTTP 429 "Muitas solicitações".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Configuração do mecanismo de pesquisa</i></td>
      <td>Configure a frequência do mecanismo de sondagem nas seguintes condições:<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-primary-application-flow.md">Autenticação executada no aplicativo (tela) primário</a></b><ul><li>O aplicativo principal (transmissão) deve pesquisar a cada 3-5 segundos.</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md">Autenticação executada em um aplicativo secundário (tela)</a></b><ul><li>O aplicativo principal (transmissão) deve pesquisar a cada 3-5 segundos.</li></ul></td>
      <td>Riscos de sobrecarga dos recursos do sistema, aumento da latência e possível acionamento de respostas de erro HTTP 429 "Muitas solicitações".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Armazenamento em cache de perfis</i></td>
      <td>Armazene partes das informações de perfil do usuário no armazenamento persistente para melhorar o desempenho e minimizar chamadas desnecessárias da API REST v2.<br/><br/>O armazenamento em cache deve se concentrar nos seguintes campos de resposta de perfis:<br/><br/><b>mvpd</b><ul><li>O aplicativo cliente pode usá-lo para rastrear o provedor de TV selecionado pelo usuário e continuar a usá-lo durante as Fases de Pré-autorização ou Autorização.</li><li>Quando o perfil de usuário atual expira, o aplicativo cliente pode usar a seleção MVPD lembrada e solicitar a confirmação do usuário.</li></ul><br/><b>atributos</b><ul><li>Usado para personalizar a experiência do usuário com base em diferentes chaves de metadados de usuário (por exemplo, zip, maxRating etc.).</li><li>Os metadados do usuário ficam disponíveis após a conclusão do fluxo de autenticação, portanto, o aplicativo cliente não precisa consultar um endpoint separado para recuperar as informações de metadados do usuário, pois já estão incluídas nas informações do perfil.</li><li>Determinados atributos de metadados podem ser atualizados durante a fase de autorização, dependendo da MVPD (por exemplo, Estatuto) e do atributo de metadados específico (por exemplo, householdID). Como resultado, o aplicativo cliente pode precisar consultar as APIs de perfis novamente após a autorização para recuperar os metadados do usuário mais recentes.</li></ul></td>
      <td>Riscos de sobrecarga dos recursos do sistema, aumento da latência e possível acionamento de respostas de erro HTTP 429 "Muitas solicitações".</td>
   </tr>
</table>

### 4. (Opcional) Fase de pré-autorização {#mandatory-requirements-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisitos</th>
      <th style="background-color: #EFF2F7;">Riscos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Recuperação de decisões de pré-autorização</i></td>
      <td>Use decisões de pré-autorização para filtragem de conteúdo e nunca para decisões de reprodução.</td>
      <td>Riscos de violação de acordos contratuais entre programadores, MVPDs e Adobe.<br/><br/>Riscos ao ignorar nossos sistemas de monitoramento e alerta.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Nova Tentativa de Recuperação de Decisões de Pré-autorização</i></td>
      <td>Manipule os <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">códigos de erro aprimorados</a> adequadamente e utilize o campo de ação para determinar as etapas de correção necessárias.<br/><br/>Somente um número limitado de códigos de erro aprimorados garante uma nova tentativa, enquanto a maioria requer resoluções alternativas, conforme especificado no campo de ação.<br/><br/>Verifique se qualquer mecanismo de repetição implementado para recuperar decisões de pré-autorização não resulta em um loop infinito e se ele limita as tentativas a um número razoável (ou seja, 2-3).</td>
      <td>Riscos de sobrecarga dos recursos do sistema, aumento da latência e possível acionamento de respostas de erro HTTP 429 "Muitas solicitações".</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Armazenamento em cache de decisões de pré-autorização</i></td>
      <td>O cache bem-sucedido permite decisões na memória para melhorar o desempenho e minimizar chamadas desnecessárias da API REST v2, pois as atualizações de assinatura enquanto o aplicativo está em execução não são frequentes.</td>
      <td>Riscos de sobrecarga dos recursos do sistema, aumento da latência e possível acionamento de respostas de erro HTTP 429 "Muitas solicitações".</td>
   </tr>
</table>

### 5. Fase de autorização {#mandatory-requirements-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisitos</th>
      <th style="background-color: #EFF2F7;">Riscos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Recuperação de decisões de autorização</i></td>
      <td>Obter decisões de autorização antes da reprodução — independentemente se uma decisão de pré-autorização existe.<br/><br/>Permitir que os fluxos continuem sem interrupções mesmo que o token de mídia expire durante a reprodução e solicite uma nova decisão de autorização contendo um token de mídia (novo) quando o usuário fizer sua próxima solicitação de reprodução, independentemente de ser para o mesmo recurso ou para um diferente.<br/><br/>Os fluxos ao vivo executados por longos períodos podem optar por solicitar uma nova decisão de autorização após operações de vídeo, como pausar conteúdo, iniciar interrupções comerciais ou modificar configurações no nível do ativo quando o MRSS for alterado.</td>
      <td>Riscos de violação de acordos contratuais entre programadores, MVPDs e Adobe.<br/><br/>Riscos ao ignorar nossos sistemas de monitoramento e alerta.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Nova Tentativa de Recuperação de Decisões de Autorização</i></td>
      <td>Manipule os <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">códigos de erro aprimorados</a> adequadamente e utilize o campo de ação para determinar as etapas de correção necessárias.<br/><br/>Somente um número limitado de códigos de erro aprimorados garante uma nova tentativa, enquanto a maioria requer resoluções alternativas, conforme especificado no campo de ação.<br/><br/>Certifique-se de que qualquer mecanismo de repetição implementado para recuperar decisões de autorização não resulte em um loop infinito e que limite as tentativas a um número razoável (ou seja, 2-3).</td>
      <td>Riscos de sobrecarga dos recursos do sistema, aumento da latência e possível acionamento de respostas de erro HTTP 429 "Muitas solicitações".</td>
   </tr>
</table>

### 6. Fase de saída {#mandatory-requirements-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisitos</th>
      <th style="background-color: #EFF2F7;">Riscos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Suporte para logout</i></td>
      <td>Implemente a API de logout para permitir que os usuários desconectem manualmente, encerrando seus perfis autenticados e seguindo o nome de ação da API REST v2 especificado para cada perfil removido:<ul><li>Para MVPDs que oferecem suporte a um endpoint de logout, o aplicativo cliente requer a navegação até o "url" fornecido em um user-agent.</li><li>Para perfis do tipo "appleSSO", o aplicativo cliente exige que o oriente o usuário para também fazer logoff do nível do parceiro (configurações do sistema da Apple).</li></ul></td>
      <td>Há risco de mau funcionamento do aplicativo cliente devido à falta de suporte no lado do aplicativo cliente.</td>
   </tr>
</table>

### 7. Parâmetros e cabeçalhos {#mandatory-requirements-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisitos</th>
      <th style="background-color: #EFF2F7;">Riscos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Enviar cabeçalho de autorização</i></td>
      <td>Envie o cabeçalho <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-authorization.md">Autorização</a> para cada solicitação REST API v2.</td>
      <td>Riscos de acionamento de respostas de erro "Não autorizado" HTTP 401, sobrecarga de recursos do sistema e latência crescente.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Enviar cabeçalho AP-Device-Identifier</i></td>
      <td>Envie o cabeçalho <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a> para cada solicitação REST API v2.<br/><br/>Mesmo quando a solicitação é originada de um servidor em nome de um dispositivo, o valor do cabeçalho AP-Device-Identifier deve refletir o identificador real do dispositivo de streaming.</td>
      <td>Riscos de disparar respostas de erro HTTP 400 "Solicitação inválida", sobrecarregando os recursos do sistema e aumentando a latência.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Enviar Cabeçalho X-Device-Info</i></td>
      <td>Envie o cabeçalho <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-device-info.md">X-Device-Info</a> para cada solicitação REST API v2.<br/><br/>Mesmo quando a solicitação se origina de um servidor em nome de um dispositivo, o valor do cabeçalho X-Device-Info deve refletir as informações reais do dispositivo de streaming.</td>
      <td>Os riscos são classificados como originários de uma plataforma desconhecida e tratados como inseguros, ficando sujeitos a regras mais restritivas, como TTLs de autenticação mais curtas.<br/><br/>Além disso, alguns campos, como o dispositivo de streaming connectionIp e connectionPort, são obrigatórios para recursos como Autenticação da Base Inicial do Spectrum.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Identificador de dispositivo estável</i></td>
      <td>Calcule e armazene um identificador de dispositivo estável que não é alterado nas atualizações ou reinicializações do cabeçalho <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md">AP-Device-Identifier</a>.<br/><br/>Para plataformas sem um identificador de hardware, gere um identificador exclusivo a partir dos atributos do aplicativo e mantenha-o.</td>
      <td>Há risco de perda de autenticação quando o identificador do dispositivo é alterado.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Seguir referências de APIs</i></td>
      <td>Verifique se você está enviando apenas os parâmetros e cabeçalhos esperados da REST API v2.</td>
      <td>Riscos de disparar respostas de erro HTTP 400 "Solicitação inválida", sobrecarregando os recursos do sistema e aumentando a latência.</td>
   </tr>
</table>

### 8. Tratamento de erros {#mandatory-requirements-error-handling}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisitos</th>
      <th style="background-color: #EFF2F7;">Riscos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Suporte aprimorado para manipulação de código de erro</i></td>
      <td>Manipule os <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md">códigos de erro aprimorados</a> adequadamente e utilize o campo de ação para determinar as etapas de correção necessárias.<br/><br/>Somente um número limitado de códigos de erro aprimorados garante uma nova tentativa, enquanto a maioria requer resoluções alternativas, conforme especificado no campo de ação.<br/><br/>A maioria dos códigos de erro aprimorados listados na documentação <a href="/help/authentication/integration-guide-programmers/features-standard/error-reporting/enhanced-error-codes.md#enhanced-error-codes-lists-rest-api-v2">Códigos de Erro Aprimorados - REST API V2</a> pode ser totalmente evitada se manipulada corretamente durante a fase de desenvolvimento, antes de iniciar o aplicativo.</td>
      <td>Riscos de sobrecarga dos recursos do sistema, aumento da latência e possível acionamento de respostas de erro HTTP 429 "Muitas solicitações".<br/><br/>Há risco de mau funcionamento do aplicativo cliente devido à falta de tratamento dos códigos de erro aprimorados, causando mensagens de erro não claras, orientação incorreta do usuário ou comportamento de fallback incorreto.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Suporte à manipulação de erros HTTP</i></td>
      <td>Diferencie entre manipular respostas de erro HTTP (por exemplo, 400, 401, 403, 404, 405, 500) e respostas de sucesso (por exemplo, 200, 201) que contêm cargas de códigos de erro aprimorados, conforme discutido acima.<br/><br/>Somente um número limitado de códigos de erro HTTP garante uma nova tentativa, enquanto a maioria requer resoluções alternativas.<br/><br/>A maioria das respostas de erro HTTP pode ser totalmente evitada se manipulada corretamente durante a fase de desenvolvimento antes da inicialização do aplicativo.</td>
      <td>Riscos de sobrecarga dos recursos do sistema, aumento da latência e possível acionamento de respostas de erro HTTP 429 "Muitas solicitações".<br/><br/>Há risco de mau funcionamento do aplicativo cliente devido à falta de tratamento dos códigos de erro aprimorados, causando mensagens de erro não claras, orientação incorreta do usuário ou comportamento de fallback incorreto.</td>
   </tr>
</table>

### 9. Ensaios {#mandatory-requirements-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Requisitos</th>
      <th style="background-color: #EFF2F7;">Riscos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Teste do ciclo de vida</i></td>
      <td>Desenvolva e teste o aplicativo usando os ambientes oficiais de não produção de Autenticação do Adobe Pass:<ul><li>Pré-produção</li><li>Estágios de lançamento</li></ul><br/>Execute controle de qualidade (QA) completo nesses ambientes antes de iniciar a produção da versão.<br/><br/>Os aplicativos cliente não devem prosseguir para a Produção de Versão sem antes concluir a validação completa em ambientes que não sejam de produção.</td>
      <td>Há riscos de lançamento com defeitos críticos e graves.<br/><br/>A falta de um caminho de depuração curto e eficiente pode impedir que o Suporte e a Engenharia da Adobe intervenham rapidamente.</td>
   </tr>
</table>

## Práticas recomendadas {#recommended-practices}

### 1. Fase de registro {#recommended-practices-registration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Práticas</th>
      <th style="background-color: #EFF2F7;">Riscos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Validação de tokens de acesso</i></td>
      <td>Verifique proativamente a validade do token de acesso e atualize-o quando expirar.<br/><br/>Verifique se qualquer mecanismo de repetição para manipular erros "Não autorizados" do HTTP 401 atualiza primeiro o token de acesso antes de repetir a solicitação original.</td>
      <td>Riscos de acionamento de respostas de erro "Não autorizado" HTTP 401, sobrecarga de recursos do sistema e latência crescente.</td>
   </tr>
</table>

### 2. Fase de configuração {#recommended-practices-configuration-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Práticas</th>
      <th style="background-color: #EFF2F7;">Riscos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Cache de configuração</i></td>
      <td>Armazene a resposta de configuração na memória ou no armazenamento persistente por um curto período de tempo (por exemplo, de 3 a 5 minutos) para melhorar o desempenho e minimizar chamadas REST API v2 desnecessárias.</td>
      <td>Riscos de sobrecarga dos recursos do sistema e aumento da latência.</td>
   </tr>
</table>

### 3. Fase de autenticação {#recommended-practices-authentication-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Práticas</th>
      <th style="background-color: #EFF2F7;">Riscos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Validação do código de autenticação (segunda autenticação de tela)</i></td>
      <td>Valide o código de autenticação enviado por meio da entrada do usuário no aplicativo secundário (segunda) (tela) antes de chamar a API /api/v2/authenticate nas seguintes condições:<br/><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-with-preselected-mvpd">Autenticação executada no aplicativo secundário (tela) com mvpd pré-selecionado</a></b><ul><li>Use <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-resume-authentication-session.md">Retomar sessão de autenticação</a> - POST /api/v2/{serviceProvider}/sessions/{code}</li></ul><br/><b><a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/basic-access-flows/rest-api-v2-basic-authentication-secondary-application-flow.md#perform-authentication-within-secondary-application-without-preselected-mvpd">Autenticação executada no aplicativo secundário (tela) sem mvpd pré-selecionado</a></b><ul><li>Use <a href="/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/apis/sessions-apis/rest-api-v2-sessions-apis-retrieve-authentication-session-information-using-code.md">Recuperar sessão de autenticação</a> - GET /api/v2/{serviceProvider}/sessions/{code}</li></ul><br/>O aplicativo cliente receberia um erro se o código de autenticação fornecido fosse digitado incorretamente ou caso a sessão de autenticação expirasse.</td>
      <td>Arrisca várias respostas de erro e problemas de fluxo de trabalho durante a autenticação.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Suporte a múltiplos perfis</i></td>
      <td>Certifique-se de que o aplicativo cliente possa lidar com vários perfis, solicitando que o usuário selecione um perfil ou aplicando uma lógica personalizada, como selecionar automaticamente o perfil com o período de validade mais longo.</td>
      <td>Há risco de mau funcionamento do aplicativo cliente devido à falta de suporte no lado do aplicativo cliente.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>(Opcional) Suporte A Fluxos Não Básicos</i></td>
      <td>Suporte a fluxos não básicos, caso o negócio de aplicativos clientes exija:<ul><li>Fluxos de acesso degradados (recurso premium)</li><li>Fluxos de acesso temporário (recurso premium)</li><li>Fluxos de acesso de logon único (recurso padrão)</li></ul></td>
      <td>Corre o risco de criar uma experiência do usuário inferior à ideal.</td>
   </tr>
</table>

### 4. (Opcional) Fase de pré-autorização {#recommended-practices-preauthorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Práticas</th>
      <th style="background-color: #EFF2F7;">Riscos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Experiência do usuário</i></td>
      <td>Exiba feedback claro do usuário se uma decisão de pré-autorização for negada, usando as mensagens fornecidas por MVPDs ou Adobe por meio de códigos de erro aprimorados.</td>
      <td>Corre o risco de criar uma experiência do usuário inferior à ideal.</td>
   </tr>
</table>

### 5. Fase de autorização {#recommended-practices-authorization-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Práticas</th>
      <th style="background-color: #EFF2F7;">Riscos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Validação de tokens de mídia</i></td>
      <td>Valide tokens de mídia usando a biblioteca <a href="/help/authentication/integration-guide-programmers/features-standard/entitlements/media-tokens.md#media-token-verifier">Verificador de Token de Mídia</a>.</td>
      <td>Riscos de esquemas de fraude, como a extração de fluxo.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Experiência do usuário</i></td>
      <td>Exibir comentários claros do usuário se uma decisão de autorização for negada, usando as mensagens fornecidas por MVPDs ou Adobe por meio de códigos de erro aprimorados.</td>
      <td>Corre o risco de criar uma experiência do usuário inferior à ideal.</td>
   </tr>
</table>

### 6. Fase de saída {#recommended-practices-logout-phase}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Práticas</th>
      <th style="background-color: #EFF2F7;">Riscos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Experiência do usuário</i></td>
      <td>Evite chamar a API de logout automaticamente (de forma programática) em cenários como pré-autorização ou autorização negada, pois a API de logout só deve ser invocada em resposta a uma solicitação direta do usuário.</td>
      <td>Riscos de confusão do usuário quanto à falha da autenticação.</td>
   </tr>
</table>

### 7. Parâmetros e cabeçalhos {#recommended-practices-parameters-headers}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Práticas</th>
      <th style="background-color: #EFF2F7;">Riscos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Reutilizar código</i></td>
      <td>Reutilize o código da REST API v1 para calcular o identificador do dispositivo e as informações do dispositivo com ajustes secundários, mas verifique se você está enviando apenas os parâmetros e cabeçalhos esperados da REST API v2.<br/><br/>Reutilize o código da API REST v1 para chamar a API DCR e recuperar um token de acesso.</td>
      <td>-</td>
   </tr>
</table>

### 8. Ensaios {#recommended-practices-testing}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Práticas</th>
      <th style="background-color: #EFF2F7;">Riscos</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Testar cobertura</i></td>
      <td>Verifique se os seguintes fluxos básicos foram testados em dispositivos e plataformas:<br/><br/><b>Fluxos de autenticação</b><ul><li>Cenário de autenticação do aplicativo primário (tela)</li><li>Cenário de autenticação de aplicativo secundário (tela)</li></ul><br/><b>(Opcional) Fluxos de pré-autorização</b><ul><li>Cenário de decisões de licenciamento de teste</li><li>Testar cenário de decisões de negação</li></ul><br/><b>Fluxos de autorização</b><ul><li>Cenário de decisões de licenciamento de teste</li><li>Testar cenário de decisões de negação</li></ul><br/><b>Fluxos de logout</b><br/><br/>Além disso, teste outros fluxos de acesso, se aplicável:<br/><br/><ul><li>Fluxos de acesso degradados (recurso premium)</li><li>Fluxos de acesso temporário (recurso premium)</li><li>Fluxos de acesso de logon único (recurso padrão)</li></ul><br/>Aborde as principais integrações do MVPD (abrangendo os provedores mais usados).</td>
      <td>Corre o risco de falhas imprevistas na produção, especialmente em plataformas testadas com menos frequência ou fluxos raros, como cenários negativos.</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Ferramentas de teste</i></td>
      <td>Use o site <a href="https://developer.adobe.com/adobe-pass/">Adobe Developer</a>.</td>
      <td>-</td>
   </tr>
</table>

## Resumo {#summary}

<table style="table-layout:auto">
   <tr>
      <th style="background-color: #EFF2F7;">Fase</th>
      <th style="background-color: #EFF2F7;">Obrigatório</th>
      <th style="background-color: #EFF2F7;">(Recomendado)</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Registro</i></td>
      <td>Armazenar em cache credenciais de cliente<br/><br/>Tokens de acesso em cache</td>
      <td>Validar e atualizar tokens de acesso</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Configuração</i></td>
      <td>Minimizar recuperações de resposta de configuração</td>
      <td>Resposta de configuração de cache</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Autenticação</i></td>
      <td>O mecanismo de sondagem está ajustando <br/><br/>partes do cache de perfis</td>
      <td>Suporte a vários perfis<br/><br/>Recurso de Degradação de Suporte (se necessário ao negócio)<br/><br/>Recurso TempPass Suporte (se necessário ao negócio)<br/><br/>Recurso de Logon Único Suporte (se necessário ao negócio)</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Pré-autorização</i></td>
      <td>Decisões de pré-autorização de permissão de cache<br/><br/>Repetir ajuste do mecanismo</td>
      <td>Melhorar a experiência do usuário usando códigos de erro para decisões de pré-autorização negadas</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Autorização</i></td>
      <td>Recuperar decisão de autorização quando o usuário solicitar a reprodução<br/><br/>Repetir ajuste do mecanismo</td>
      <td>Melhore a experiência do usuário usando códigos de erro para decisões de autorização negadas<br/><br/>Validação de token de mídia</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Sair</i></td>
      <td>Implemente a API de logout para permitir que os usuários saiam manualmente</td>
      <td>Evite chamar automaticamente a API de logout</td>
   </tr>
   <tr>
      <th style="background-color: #EFF2F7;"></th>
      <th style="background-color: #EFF2F7;">Obrigatório</th>
      <th style="background-color: #EFF2F7;">(Recomendado)</th>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Parâmetros e cabeçalhos</i></td>
      <td>Siga as especificações de cabeçalhos obrigatórios</td>
      <td>Reutilizar código da API REST v1</td>
   </tr>
   <tr>
      <td style="background-color: #DEEBFF;"><i>Tratamento de erros</i></td>
      <td>Implementar tratamento de erros aprimorado<br/><br/>Implementar tratamento de erros HTTP</td>
      <td>-</td>
   </tr>
</table>
