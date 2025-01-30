---
title: Perguntas frequentes sobre procedimentos de suporte
description: Perguntas frequentes sobre procedimentos de suporte
exl-id: 1d754e5a-d5fa-4411-8932-2a36294da6eb
source-git-commit: 0ab1fc212752dd4a4d6e12a4ab1287ef74e4a282
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 0%

---

# Perguntas frequentes sobre procedimentos de suporte {#support-procedures-faqs}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Este documento descreve as perguntas frequentes (FAQ) sobre procedimentos de suporte para incidentes graves (nível de GRAVIDADE 1) que afetam a autenticação da Adobe Pass e seus parceiros.

## Perguntas frequentes {#faqs}

### O que é um incidente de GRAVIDADE 1? {#support-procedures-faqs-1}

Um incidente de nível GRAVIDADE 1 é uma situação ativa no ambiente de produção que impede a conclusão dos fluxos de autenticação ou autorização para um canal e um MVPD, afetando um grande número de assinantes.

Exemplos de incidentes de GRAVIDADE 1

* Durante o processo de autenticação, o usuário não é redirecionado para a página de logon depois que seleciona a MVPD em qualquer navegador compatível.

* Durante o processo de autenticação, o usuário fica preso em uma página de erro de Adobe sem poder reiniciar o fluxo de autenticação.

* O parceiro recebe vários relatórios que os usuários não podem autenticar ou autorizar com uma MVPD específica.

* O Ativador de acesso de produção hospedado em https://entitlement.auth.adobe.com/entitlement/v4/AccessEnabler.js não está disponível.

### O que é um incidente de nível não GRAVIDADE 1?

O Adobe dará suporte a investigações sobre esses problemas, mas eles não são considerados incidentes de GRAVIDADE 1:

* Um ou alguns assinantes não podem se autenticar e permanecem na página de logon do MVPD.

* Um ou alguns assinantes são autenticados, mas não podem reproduzir vídeos.

* Alguns ou todos os assinantes encontram um erro de JavaScript no site Programador.

### Como são tratados os incidentes de GRAVIDADE 1?

Um incidente de nível GRAVIDADE 1 pode ser iniciado pelo Adobe ou por um parceiro de autenticação da Adobe Pass. As etapas para cada um são descritas abaixo.

**Fluxo iniciado pelo parceiro**

1. O parceiro identifica um incidente de nível de Gravidade 1 que requer atenção imediata do Adobe.

1. O parceiro envia um email para **tve-support@adobe.com** incluindo **URGENT - INCIDENT** na linha de assunto e adicionando as seguintes informações:
   * Título
   * Descrição e etapas a serem reproduzidas
   * SO / Navegador
   * SDK e versão
   * Dispositivos afetados
   * % de usuários afetados
   * Logs de rastreamento ou dispositivo HTTP demonstrando o problema
   * (opcional) Capturas de tela ou vídeos disponíveis que demonstram o problema

1. Se o Adobe não responder ao tíquete dentro de um período, o parceiro pode ligar para o seguinte número: **1-657-312-4623**.

>[!IMPORTANT]
>
> Se você não incluir &quot;URGENT-INCIDENT&quot; no título do ticket, ele não será selecionado pelo nosso sistema de notificação.

**fluxo iniciado por Adobe**

Para um problema de autenticação do Adobe Pass:

1. O Adobe identifica um problema interno e abre um ticket no nosso sistema de rastreamento.

1. O Adobe notifica o gerente de programa e o contato técnico do parceiro, especificando o número do ticket e o impacto estimado do problema.

1. O Adobe trabalha para resolver o incidente e mantém todos os parceiros afetados informados.

Para um problema de parceiro (Programador/MVPD):

1. Adobe identifica um problema relacionado à integração com um MVPD ou em um dos sites do Programador.

1. O Adobe notifica o parceiro afetado seguindo os procedimentos de suporte em vigor com esse parceiro e abre um tíquete com a organização de suporte do parceiro.

1. Se, durante a análise de impacto, o Adobe identificar que o problema se enquadra em uma das decisões pré-acordadas sobre cenários de incidente, ele agirá de acordo sem esperar pelo comentário do parceiro.

1. O Adobe aguardará atualizações do parceiro e uma notificação quando o serviço for restaurado.

### O que são decisões pré-acordadas sobre cenários de incidentes?

Determinadas situações com ações padrão que serão executadas se o cenário ocorrer:

|    | Cenário | Descrição | Ações |
|----|--------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| S1 | O código Adobe identifica um problema com a integração de um MVPD durante as operações normais de produção. | Durante as operações normais de produção, o Adobe identifica um problema com um dos MVPDs que torna impossível executar os fluxos de autenticação/autorização (por exemplo, certificados expirados, respostas SAML expiradas, portas fechadas, parâmetros alterados etc.) | O Adobe notificará o MVPD e os programadores afetados.  O Adobe </br></br> desativará este MVPD para todos os programadores afetados. O Adobe </br></br> abrirá um tíquete com a MVPD seguindo o procedimento de suporte acordado com essa MVPD |
| S2 | O Adobe ativa um novo MVPD para um Programador e o Programador permite o MVPD antes da data de lançamento. | O Adobe está ativando um novo MVPD para o site de um Programador e o site já está exibindo o novo MVPD no seletor, mesmo que não seja necessário. | O Adobe notificará o programador sobre a nova MVPD que aparece no seletor antes da data programada. O programador </br></br> executará uma ação para removê-lo do seletor, se necessário. |
| S3 | O Adobe ativa um novo MVPD para um programador mesmo se o MVPD não estiver pronto para entrar em produção | O Adobe está ativando um novo MVPD para um Programador, mas o MVPD ainda não implantou o suporte para a integração, portanto, os fluxos de autenticação/autorização não podem ser executados | O Adobe fará a implantação somente se solicitado pelo programador </br></br>. O programador será responsável por garantir a permissão do MVPD uma vez que todos os testes tenham sido executados. |
