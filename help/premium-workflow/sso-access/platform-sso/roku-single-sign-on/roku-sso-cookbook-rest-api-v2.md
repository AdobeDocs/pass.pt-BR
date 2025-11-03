---
title: Guia do Roku SSO (REST API V2)
description: Guia do Roku SSO (REST API V2)
exl-id: 77b154bc-c09f-49d4-b1af-cc33bc6dd22b
source-git-commit: e4d243ebf293f3ecc38e532d77116c065a22ebd2
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 0%

---

# Guia do Roku SSO (REST API V2) {#roku-sso-cookbook-rest-api-v2}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

A API REST V2 de Autenticação do Adobe Pass é compatível com o Logon Único da Plataforma (SSO) para usuários finais de aplicativos clientes em execução no RokuOS.

Este documento atua como uma extensão para a [Visão geral da REST API V2](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/rest-api-v2-overview.md) existente, que fornece uma exibição de alto nível e o documento que descreve como implementar o [Logon único usando fluxos de identidade de plataforma](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/flows/single-sign-on-access-flows/rest-api-v2-single-sign-on-platform-identity-flows.md).

## Logon único do Roku usando fluxos de identidade da plataforma {#cookbook}

A Autenticação do Adobe Pass colabora com o Roku para melhorar a experiência de logon do usuário e facilitar o Logon único (SSO) em aplicativos do TV Everywhere para assinantes de TV.

### Pré-requisitos {#prerequisites}

Antes de prosseguir com o logon único do Roku usando fluxos de identidade de plataforma, verifique se o SSO do Roku está ativado. O Roku SSO é ativado por padrão, a menos que o Programador ou o MVPD solicite que o SSO seja desativado.

Cada Programador pode Habilitar ou Desabilitar o Logon Único (SSO) na plataforma Roku para integrações específicas por meio do [Painel do Adobe Pass TVE](https://experience.adobe.com/pass/authentication).

### Fluxo de trabalho (WRK) {#workflow}

**Cliente para Servidor**

Para aplicativos de programador que utilizam uma arquitetura cliente-para-servidor para integrar a REST API V2, o Roku SSO funciona perfeitamente sem qualquer modificação.

O RokuOS anexa automaticamente dois cabeçalhos HTTP a todas as solicitações enviadas para os pontos de extremidade de autenticação da Adobe Pass.

**Servidor a servidor**

Para aplicativos de Programador que utilizam uma arquitetura de servidor para servidor para integrar a REST API V2, o Programador deve coordenar com a equipe do Roku para configurar esses cabeçalhos para serem incluídos em todos os fluxos de API direcionados ao seu domínio.

Para habilitar o SSO entre aplicativos e dispositivos, a ID de assinante fornecida pelo Roku deve ser usada em vez da ID do dispositivo quando transmitida pelo aplicativo.

Para obter mais detalhes, consulte a seguinte documentação:

* [Cabeçalho - X-Roku-Reserved-Roku-Connect-Token](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-x-roku-reserved-roku-connect-token.md)
* [Cabeçalho - AP-Identificador de dispositivo](/help/authentication/integration-guide-programmers/rest-apis/rest-api-v2/appendix/headers/rest-api-v2-appendix-headers-ap-device-identifier.md)

Para obter detalhes específicos sobre o formato dos cabeçalhos necessários, entre em contato com o representante da Adobe.

### Perguntas frequentes {#faqs}

* **Como o SSO funcionará?**

  O SSO funcionará em todos os aplicativos do Programador alimentados pela Autenticação Adobe Pass em todos os dispositivos Roku associados ao mesmo usuário do Roku. Nem todos os MVPDs permitirão o Roku SSO.


* **Haverá alguma alteração nos TTLs de autenticação?**

  O primeiro token de autenticação válido será usado para executar o SSO e, nesse caso, todos os outros aplicativos que serão autenticados por meio do SSO usarão o mesmo TTL até expirar. Assim, ao navegar de um aplicativo para outro, o segundo aplicativo compartilhará o TTL do primeiro aplicativo autenticado.


* **Outra funcionalidade do Adobe funcionará como antes?**

  Toda a funcionalidade de Autenticação do Adobe Pass funcionará como antes.


* **Existe um processo de aceitação/recusa do programador que se beneficia do SSO na plataforma Roku?**

  Essa será uma alteração de configuração no Painel TVE do Adobe. Cada programador pode ativar ou desativar o SSO na plataforma Roku para integrações específicas.


* **Quais são alguns problemas comuns?**

  Os programadores devem verificar se suas implementações atuais baseadas na REST API do Adobe não impedem o SSO de plataforma do Roku.

  Veja abaixo uma lista de possíveis problemas e como eles devem ser resolvidos.

| Problema | Causa possível | Possíveis soluções |
|--------------------------------------------------|----------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| Nenhum cabeçalho de SSO do Roku enviado para o Adobe | Usar HTTP em vez de HTTPS para chamadas para domínios de autenticação da Adobe Pass | Usar HTTPS |
| Logotipo do MVPD não mostrado/não atualizado para tokens SSO | A interface do usuário depende do armazenamento local | Os aplicativos devem atualizar a interface do usuário (e o armazenamento local, se necessário) após verificar a autenticação |
| Logout acionado sem AuthZ | Design do aplicativo | O aplicativo deve ser atualizado para nunca realizar logout em segundo plano |
