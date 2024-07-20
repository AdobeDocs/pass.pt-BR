---
title: Visão geral do SSO do Roku
description: Sobre o Roku SSO
exl-id: 77b154bc-c09f-49d4-b1af-cc33bc6dd22b
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---

# Visão geral do SSO do Roku {#overview}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#roku-sso-intro}

Este documento descreve as informações necessárias para aproveitar o recurso de Logon único em dispositivos Roku. A Autenticação do Adobe Pass colabora com o Roku para melhorar a experiência de logon do usuário e facilitar o Logon único em aplicativos da TV em todos os lugares para assinantes de TV.

A solução é baseada na API REST sem cliente da Autenticação Adobe Pass, de modo que a maioria dos programadores não precisará atualizar seus aplicativos para se beneficiar do Logon único.

## Habilitar o Roku SSO {#enable-roku-sso}

O Roku SSO é ativado por padrão, a menos que o Programador ou a solicitação MVPD para SSO seja desativada.

Cada programador pode ativar/desativar o SSO na plataforma Roku para integrações específicas.

### O que um programador deve fazer para que o Roku SSO funcione {#make-roku-sso-work}

Para aplicativos de Programadores que implementam a API REST em dispositivos clientes, o SSO do Roku funcionará sem qualquer alteração. O Roku OS adicionará dois cabeçalhos HTTP em qualquer solicitação aos pontos de extremidade de autenticação da Adobe Pass.

Para aplicativos de programadores que implementam uma solução de servidor para servidor para API REST, o programador deve entrar em contato com a equipe do Roku e solicitar uma configuração em que esses dois cabeçalhos serão enviados para seu domínio em todos os fluxos de API.

A ID de assinante fornecida pelo Roku que será usada em vez da ID de dispositivo transmitida pelo aplicativo para garantir o SSO entre aplicativos (e entre dispositivos).

Para detalhes específicos sobre o formato dos cabeçalhos necessários, entre em contato com o representante da Adobe.

### Possíveis problemas {#possible-issues}

Os programadores devem verificar se suas implementações atuais baseadas na API REST sem cliente do Adobe não impedem o SSO de plataforma do Roku. Veja abaixo uma lista de possíveis problemas e como eles devem ser resolvidos.

| Problema | Causa possível | Possíveis soluções |
|-|-|-|
| Nenhum cabeçalho de SSO Roku enviado para o Adobe | Usar HTTP em vez de HTTPS para chamadas para domínios de autenticação da Adobe Pass | Usar HTTPS |
| Logotipo do MVPD não mostrado/não atualizado para tokens de SSO | A interface do usuário depende do armazenamento local | Os aplicativos devem atualizar a interface do usuário (e o armazenamento local, se necessário) após verificar a autenticação |
| Logout acionado sem AuthZ | Design do aplicativo | O aplicativo deve ser atualizado para nunca realizar logout em segundo plano |

## Perguntas frequentes {#faq}

* **Como o SSO funcionará?**

  O SSO funcionará em todos os aplicativos do Programador alimentados pela Autenticação Adobe Pass em todos os dispositivos Roku associados ao mesmo usuário do Roku.
Nem todos os MVPDs permitirão o Roku SSO.

* **Haverá alguma alteração nos TTLs de autenticação?**

  O primeiro token de autenticação válido será usado para executar o SSO e, nesse caso, todos os outros aplicativos que serão autenticados por meio do SSO usarão o mesmo TTL até expirar. Assim, ao navegar de um aplicativo para outro, o segundo aplicativo compartilhará o TTL do primeiro aplicativo autenticado.

* **Outra funcionalidade de Adobe funcionará como antes?**

  Toda a funcionalidade de Autenticação do Adobe Pass funcionará como antes.

* **Existe um processo de aceitação/recusa do programador que se beneficia do SSO na plataforma Roku?**

  Essa será uma alteração de configuração no Painel Adobe TVE. Cada programador pode ativar/desativar o SSO na plataforma Roku para integrações específicas.
