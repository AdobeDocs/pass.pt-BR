---
title: Procedimentos de escalonamento de monitoramento de simultaneidade
description: Procedimentos de escalonamento de monitoramento de simultaneidade
exl-id: eb110465-3a74-489e-a521-0e17f5aeecb8
source-git-commit: ed340643e807d786638d59f9bf07d73b7f909a72
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 0%

---

# Procedimentos de escalonamento de monitoramento de simultaneidade {#esc-procedures}

>[!NOTE]
>
>Ligue para a linha direta: +1-657-312-4623 e envie um email para `tve-support@adobe.com` incluindo &quot;URGENTE - INCIDENTE&quot; na linha de assunto.


## Introdução {#cm-escalation-intro}

Este documento descreve os procedimentos de suporte para incidentes graves (nível **SEVERITY 1**) que afetam a Autenticação da Adobe Pass, o Monitoramento de Simultaneidade da Adobe Pass e seus parceiros.

## Definição do nível de gravidade 1 do escalonamento {#defn-escl-sevrityone-level}

Um incidente de nível **SEVERITY 1** é uma situação **LIVE**, **acontecendo no ambiente de produção**, que não permite a conclusão dos fluxos de autenticação e/ou autorização para um canal e uma MVPD, afetando um grande número de assinantes da MVPD que estão executando o fluxo.

## Exemplos de incidentes de Gravidade 1 {#exampl-sevone-incident}

* O Ativador de Acesso para produção hospedado em <http://entitlement.auth.adobe.com/entitlement/AccessEnabler.js> não está disponível.

* Para uma MVPD específica, o Adobe não redireciona mais/exibe a página de logon, depois que o usuário seleciona a MVPD (em qualquer um dos navegadores compatíveis).

* O parceiro recebe um grande número de relatórios que os usuários não podem autenticar/autorizar com uma MVPD específica.

* Durante o processo de autenticação, o usuário fica preso em uma página de erro do Adobe sem a possibilidade de reiniciar o fluxo de autenticação/autorização.


## Exemplos do que é *NOT* um incidente de Gravidade 1 {#exampl-not-sev1}

*Para problemas desses tipos, a Adobe fornecerá suporte para investigações, mas não são incidentes de Gravidade 1:*

* Um ou alguns assinantes não podem executar o fluxo devido a um problema de versão do Flash (Flash ausente, bloqueadores Flash, versão incorreta do Flash).
* Um ou alguns assinantes não podem se autenticar e permanecem na página de logon do MVPD.
* Um ou alguns assinantes são autenticados, mas não podem reproduzir vídeos.
* Um/poucos/todos os assinantes encontram um erro de JavaScript no site Programador.

## Fluxos de escalonamento de gravidade 1 {#sevone-escalation-flows}

Os incidentes de gravidade 1 podem ser iniciados pela Adobe ou por um parceiro de autenticação da Adobe Pass. As etapas para cada um são apresentadas abaixo.

### Fluxo iniciado pelo parceiro {#partner-initiated-flow}

1. O parceiro identifica um incidente de Gravidade 1 (conforme descrito acima) que requer a atenção imediata da Adobe.

1. O parceiro envia um email para tve-support@adobe.com incluindo &quot;URGENT - INCIDENT&quot; na linha de assunto e adicionando as seguintes informações:

   * Título
   * Descrição e etapas a serem reproduzidas
   * SO
   * Navegador
   * Versão do Flash
   * (opcional) Capturas de tela ou vídeos disponíveis que demonstram o problema

1. Se a Adobe não responder ao ticket em 30 minutos, o parceiro chama o número abaixo:

   * **1-205-693-9813**


**Se você não incluir &quot;URGENT-INCIDENT&quot; no título do tíquete, ele não será selecionado pelo nosso sistema de notificação.**

### Fluxo iniciado pelo Adobe {#adobe-initiated-flow}

**...por um problema de autenticação do Adobe Pass**

1. O Adobe identifica um problema interno e abre um ticket no nosso sistema de rastreamento.

1. A Adobe notifica o gerente de programa e o contato técnico do parceiro, especificando o número do ticket e o impacto estimado do problema.

1. A Adobe trabalha para a resolução do incidente e mantém todos os parceiros afetados informados.


**...para um problema de parceiro (Programmer/MVPD)**

1. O Adobe identifica um problema relacionado à integração com uma MVPD ou em um dos sites do Programador.

1. A Adobe notifica o parceiro afetado **seguindo os procedimentos de suporte em vigor com esse parceiro** e abre um tíquete com a organização de suporte do parceiro.

1. Se, durante a análise de impacto, a Adobe identificar que o problema pertence a uma das decisões pré-acordadas em cenários de incidente (consulte a seção &quot;Decisões pré-acordadas em cenários de incidente&quot; abaixo), ela agirá adequadamente sem esperar pelo parceiro1. entrada do.

1. A Adobe aguardará atualizações do parceiro e uma notificação do parceiro quando o serviço for restaurado.

### Decisões pré-acordadas sobre cenários de incidente {#pre-agreed-decisions}

Há algumas situações em que uma ação padrão será executada no caso da ocorrência desse cenário:

|    | Cenário | Descrição | Ações |
|:--:|:------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| S1 | O Adobe identifica um problema com a integração de uma MVPD durante as operações normais de produção. | Durante as operações normais de produção, a Adobe identifica um problema com um dos MVPDs que torna impossível executar os fluxos de autenticação/autorização (por exemplo, certificados expirados, respostas SAML expiradas, portas fechadas, parâmetros alterados etc.) | A Adobe notificará a MVPD e os programadores afetados. O Adobe desativará este MVPD para todos os programadores afetados. A Adobe abrirá um tíquete com a MVPD seguindo o procedimento de suporte acordado com essa MVPD |
| S2 | O Adobe ativa um novo MVPD para um Programador e o Programador coloca a MVPD na lista de permissões antes da data de lançamento. | A Adobe está ativando um novo MVPD para o site de um Programador e o site já está exibindo o novo MVPD no seletor, mesmo que não seja necessário. | A Adobe notificará o programador sobre a nova MVPD que aparece no seletor antes da data agendada. O programador tomará medidas para removê-lo do seletor, se necessário. |
| S3 | O Adobe ativa um novo MVPD para um programador mesmo se o MVPD não estiver pronto para entrar em produção | A Adobe está ativando um novo MVPD para um Programador, mas o MVPD ainda não implantou o suporte para a integração, portanto, os fluxos de autenticação/autorização não podem ser executados | O Adobe fará a implantação somente se solicitado pelo programador. O programador será responsável por garantir a lista de permissões do MVPD depois que todos os testes forem executados. |

### Expectativas De Resposta Para Incidentes De Gravidade 1 {#response-expectations}

* Resposta inicial: 30 minutos (24 horas por dia, 7 dias por semana)
* Plano de ação: 1 hora (24 horas por dia, 7 dias por semana)
* Resolução: ASAP (24 horas por dia, 7 dias por semana)
