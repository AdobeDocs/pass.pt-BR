---
title: Procedimentos de escalonamento
description: Procedimentos de escalonamento
exl-id: 1d754e5a-d5fa-4411-8932-2a36294da6eb
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '879'
ht-degree: 0%

---

# Procedimentos de escalonamento {#escalation-procedures}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
> 
>Ligue para a linha direta: **+1-205-693-9813** e envie um email para **tve-support@adobe.com**, incluindo **URGENT - INCIDENT** na linha de assunto.

## Introdução {#introduction}

Este documento descreve os procedimentos de suporte para incidentes graves (nível **SEVERITY 1**) que afetam a Autenticação do Adobe Pass, o Monitoramento de Simultaneidade do Adobe Pass e seus parceiros.


## Definição de um incidente de nível GRAVIDADE 1 {#definition-of-a-severity-1-level-incident}

Um incidente de nível **SEVERITY 1** é uma situação **LIVE**, **acontecendo no ambiente de produção**, que não permite a conclusão dos fluxos de autenticação e/ou autorização para um canal e um MVPD, afetando um grande número de assinantes do MVPD que estão executando o fluxo.


## Exemplos de incidentes de GRAVIDADE 1 {#examples-of-severity-1-incidentcs}

* O Ativador de Acesso para produção hospedado em `https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js` (ou `https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js`) não está disponível.

* Para um MVPD específico, o Adobe não redireciona mais/exibe a página de logon, depois que o usuário seleciona o MVPD (em qualquer um dos navegadores compatíveis).

* O parceiro recebe um grande número de relatórios que os usuários não podem autenticar/autorizar com um MVPD específico.

* Durante o processo de autenticação, o usuário fica preso em uma página de erro de Adobe sem a possibilidade de reiniciar o fluxo de autenticação/autorização.


| Exemplos do que é **NOT** um incidente de Gravidade 1 |
|---|
| Para problemas desses tipos, o Adobe fornecerá suporte para investigações, mas não são incidentes de Gravidade 1:<ul><li>Um ou alguns assinantes não podem executar o fluxo devido a um problema de versão do Flash (Flash ausente, bloqueadores de Flashes, versão incorreta do Flash).</li><li>Um ou alguns assinantes não podem se autenticar e permanecem na página de logon do MVPD.</li><li>Um ou alguns assinantes são autenticados, mas não podem reproduzir vídeos.</li><li>Um/poucos/todos os assinantes encontram um erro de JavaScript no site Programador</li></ul> |

## Fluxos de escalonamento de gravidade 1 {#severity-1-escalation-flows}

Os incidentes de gravidade 1 podem ser iniciados pelo Adobe ou por um parceiro de autenticação da Adobe Pass. As etapas para cada um são apresentadas abaixo.

### Fluxo iniciado pelo parceiro {#partner-initiated-flow}

1. O parceiro identifica um incidente de Gravidade 1 (conforme descrito acima) que requer atenção imediata da Adobe.
1. O parceiro envia um email para **tve-support@adobe.com** incluindo **URGENT - INCIDENT** na linha de assunto e adicionando as seguintes informações:
   * Título
   * Descrição e etapas a serem reproduzidas
   * SO / Navegador
   * SDK e versão
   * Dispositivos afetados
   * % de usuários afetados
   * Logs de rastreamento ou dispositivo HTTP demonstrando o problema
   * (opcional) Capturas de tela ou vídeos disponíveis que demonstram o problema
1. Se o Adobe não responder ao ticket em 30 minutos, o parceiro chama o seguinte número:
   **1-205-693-9813**
   >[!IMPORTANT]
   >Se você não incluir &quot;URGENT-INCIDENT&quot; no título do ticket, ele não será selecionado pelo nosso sistema de notificação**.

### Fluxo iniciado por Adobe {#adobe-initiated-flow}

#### ...por um problema de autenticação da Adobe Pass {#adobe-initiated-flow-authn-issue}

1. O Adobe identifica um problema interno e abre um ticket no nosso sistema de rastreamento.

1. Adobe notifica o gerente de programa e o contato técnico do parceiro, especificando o número do ticket e o impacto estimado do problema.

1. O Adobe trabalha para a resolução do incidente e mantém todos os parceiros afetados informados.

#### ...para um problema de parceiro (Programador/MVPD) {#adobe-initiated-flow-partner-issue}

1. Adobe identifica um problema relacionado à integração com um MVPD ou em um dos sites do Programador.

1. O Adobe notifica o parceiro afetado <u>seguindo os procedimentos de suporte em vigor com esse parceiro</u> e abre um tíquete com a organização de suporte do parceiro.

1. Se, durante a análise de impacto, o Adobe identificar que o problema pertence a uma das decisões pré-acordadas em cenários de incidentes, consulte **Decisões pré-acordadas em cenários de incidentes**, ele atuará adequadamente sem esperar a entrada do parceiro.

1. O Adobe aguardará atualizações do parceiro e uma notificação do parceiro quando o serviço for restaurado.

## Decisões pré-acordadas sobre cenários de incidente {#pre-agreed-descn}

Há algumas situações em que uma ação padrão será executada no caso da ocorrência desse cenário:

|   | Cenário | Descrição | Ações |
|---|---|---|---|
| S1 | Adobe identifica um problema com a integração de um MVPD durante as operações normais de produção. | Durante as operações normais de produção, o Adobe identifica um problema com um dos MVPDs que torna impossível executar os fluxos de autenticação/autorização (por exemplo, certificados expirados, respostas SAML expiradas, portas fechadas, parâmetros alterados etc.) | - o Adobe notificará o MVPD e os programadores afetados.  </br> </br> - O Adobe desativará este MVPD para todos os Programadores afetados. </br> </br> - o Adobe abrirá um tíquete com o MVPD seguindo o procedimento de suporte acordado com esse MVPD |
| S2 | O Adobe ativa um novo MVPD para um Programador e o Programador permite o MVPD antes da data de lançamento. | O Adobe está ativando um novo MVPD para o site de um Programador, e o site já está exibindo o novo MVPD no seletor, mesmo que não fosse suposto. | - Adobe notificará o programador sobre o novo MVPD que aparece no seletor antes da data programada. </br> </br> - O programador executará uma ação para removê-lo do seletor, se necessário. |
| S3 | Adobe ativa um novo MVPD para um Programador mesmo se o MVPD não estiver pronto para entrar em produção | O Adobe está ativando um novo MVPD para um Programador, mas o MVPD ainda não implantou o suporte para a integração, portanto, os fluxos de autenticação/autorização não podem ser executados | - O Adobe fará a implantação somente se solicitado pelo programador </br> </br> - O programador será responsável por garantir a permissão do MVPD assim que todos os testes forem executados. |

## Expectativas De Resposta Para Incidentes De Gravidade 1 {#response-expectations-for-severity-one-incidents}

* Resposta inicial: 30 minutos (24 horas por dia, 7 dias por semana)
* Plano de ação: 1 hora (24 horas por dia, 7 dias por semana)
* Resolução: ASAP (24 horas por dia, 7 dias por semana)
