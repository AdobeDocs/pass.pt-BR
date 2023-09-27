---
title: Procedimentos de escalonamento de monitoramento de simultaneidade
description: Procedimentos de escalonamento de monitoramento de simultaneidade
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '823'
ht-degree: 0%

---


# Procedimentos de escalonamento de monitoramento de simultaneidade {#esc-procedures}

>[!NOTE]
>
>Ligue para a linha direta: +1-205-693-9813 e envie um email para `tve-support@adobe.com` incluindo &quot;URGENTE - INCIDENTE&quot; na linha de assunto.


## Introdução {#cm-escalation-intro}

O presente documento descreve os procedimentos de apoio a incidentes graves (**SEVERIDADE 1** nível) afetando a Autenticação do Adobe Pass, o Monitoramento de simultaneidade do Adobe Pass e seus parceiros.

## Definição do nível de gravidade 1 do escalonamento {#defn-escl-sevrityone-level}

A **SEVERIDADE 1** incidente de nível é um **LIVE** situação, **acontecendo no ambiente de produção**, que não permite a conclusão dos fluxos de autenticação e/ou autorização para um canal e um MVPD, afetando um grande número de assinantes do MVPD que está executando o fluxo.

## Exemplos de incidentes de Gravidade 1 {#exampl-sevone-incident}

* O Ativador de acesso de produção hospedado em <http://entitlement.auth.adobe.com/entitlement/AccessEnabler.js> não está disponível.

* Para um MVPD específico, o Adobe não redireciona mais/exibe a página de logon, depois que o usuário seleciona o MVPD (em qualquer um dos navegadores compatíveis).

* O parceiro recebe um grande número de relatórios que os usuários não podem autenticar/autorizar com um MVPD específico.

* Durante o processo de autenticação, o usuário fica preso em uma página de erro de Adobe sem a possibilidade de reiniciar o fluxo de autenticação/autorização.


## Exemplos do que é *NOT* um incidente de Gravidade 1 {#exampl-not-sev1}

*Para problemas desses tipos, o Adobe fornecerá suporte para investigações, mas não são incidentes de Gravidade 1:*

* Um ou alguns assinantes não podem executar o fluxo devido a um problema de versão do Flash (Flash ausente, bloqueadores de Flashes, versão incorreta do Flash).
* Um ou alguns assinantes não podem se autenticar e permanecem na página de logon do MVPD.
* Um ou alguns assinantes são autenticados, mas não podem reproduzir vídeos.
* Um/poucos/todos os assinantes encontram um erro de JavaScript no site Programador.

## Fluxos de escalonamento de gravidade 1 {#sevone-escalation-flows}

Os incidentes de gravidade 1 podem ser iniciados pelo Adobe ou por um parceiro de autenticação da Adobe Pass. As etapas para cada um são apresentadas abaixo.

### Fluxo iniciado pelo parceiro {#partner-initiated-flow}

1. O parceiro identifica um incidente de Gravidade 1 (conforme descrito acima) que requer atenção imediata da Adobe.

1. O parceiro envia um email para tve-support@adobe.com incluindo &quot;URGENT - INCIDENT&quot; na linha de assunto e adicionando as seguintes informações:

   * Título
   * Descrição e etapas a serem reproduzidas
   * SO
   * Navegador
   * Versão do Flash
   * (opcional) Capturas de tela ou vídeos disponíveis que demonstram o problema

1. Se o Adobe não responder ao ticket em 30 minutos, o parceiro chama o número abaixo:

   * **1-205-693-9813**


**Se você não incluir &quot;URGENT-INCIDENT&quot; no título do ticket, ele não será selecionado pelo nosso sistema de notificação.**

### Fluxo iniciado pelo Adobe {#adobe-initiated-flow}

**...por um problema de autenticação da Adobe Pass**

1. O Adobe identifica um problema interno e abre um ticket no nosso sistema de rastreamento.

1. O Adobe notifica o gerente de programa e o contato técnico do parceiro, especificando o número do ticket e o impacto estimado do problema.

1. O Adobe trabalha para a resolução do incidente e mantém todos os parceiros afetados informados.


**...para um problema de parceiro (Programador/MVPD)**

1. Adobe identifica um problema relacionado à integração com um MVPD ou em um dos sites do Programador.

1. Adobe notifica o parceiro afetado **procedimentos de apoio em vigor com esse parceiro** e abre um tíquete na organização de suporte do parceiro.

1. Se, durante a análise de impacto, o Adobe identificar que o problema pertence a uma das decisões pré-acordadas sobre cenários de incidente (consulte a seção &quot;Decisões pré-acordadas sobre cenários de incidente&quot; abaixo), ele agirá de acordo sem esperar pelo parceiro1. entrada do.

1. O Adobe aguardará atualizações do parceiro e uma notificação do parceiro quando o serviço for restaurado.

### Decisões pré-acordadas sobre cenários de incidente {#pre-agreed-decisions}

Há algumas situações em que uma ação padrão será executada no caso da ocorrência desse cenário:

|    | Cenário | Descrição | Ações |
|:---:|:---|:---|:---|
| S1 | Adobe identifica um problema com a integração de um MVPD durante as operações normais de produção. | Durante as operações normais de produção, o Adobe identifica um problema com um dos MVPDs que torna impossível executar os fluxos de autenticação/autorização (por exemplo, certificados expirados, respostas SAML expiradas, portas fechadas, parâmetros alterados etc.) | O Adobe notificará o MVPD e os programadores afetados. O Adobe desativará esse MVPD para todos os programadores afetados. O Adobe abrirá um ticket com o MVPD seguindo o procedimento de suporte acordado com esse MVPD |
| S2 | O Adobe ativa um novo MVPD para um Programador e o Programador coloca o MVPD na lista de permissões antes da data de lançamento. | O Adobe está ativando um novo MVPD para o site de um Programador, e o site já está exibindo o novo MVPD no seletor, mesmo que não fosse suposto. | O Adobe notificará o programador sobre o novo MVPD que aparece no seletor antes da data programada. O programador tomará medidas para removê-lo do seletor, se necessário. |
| S3 | Adobe ativa um novo MVPD para um Programador mesmo se o MVPD não estiver pronto para entrar em produção | O Adobe está ativando um novo MVPD para um Programador, mas o MVPD ainda não implantou o suporte para a integração, portanto, os fluxos de autenticação/autorização não podem ser executados | Adobe fará a implantação somente se solicitado pelo programador O programador será responsável por garantir a lista de permissões do MVPD uma vez que todos os testes foram realizados. |

### Expectativas De Resposta Para Incidentes De Gravidade 1 {#response-expectations}

* Resposta inicial: 30 minutos (24 horas por dia, 7 dias por semana)
* Plano de ação: 1 hora (24 horas por dia, 7 dias por semana)
* Resolução: ASAP (24 horas por dia, 7 dias por semana)
