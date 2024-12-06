---
title: Integração dos dados do lado do servidor de autenticação da Adobe Pass ao Adobe Analytics
description: Integração dos dados do lado do servidor de autenticação da Adobe Pass ao Adobe Analytics
exl-id: c1f1f2a3-c98c-4aed-92ad-1f9bfd80b82b
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '1139'
ht-degree: 0%

---

# Integração dos dados do lado do servidor de autenticação da Adobe Pass ao Adobe Analytics

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Os clientes da Autenticação da Adobe Pass desejam ver os dados do lado do servidor da Autenticação da Adobe Pass (Adobe Pass) no painel da Adobe Analytics para facilitar o consumo.

Os dados servirão para rastrear métricas TVE importantes, como taxas de conversão de autenticação por MVPD, usuários únicos com base na ID de usuário do MVPD e muito mais.

Não se destina a substituir uma implementação do lado do cliente, se já existir, pois a atividade do usuário não pode ser rastreada além dos eventos específicos abaixo na ausência de uma ID de visitante. Se os clientes fornecerem uma ID de visitante em chamadas Pass, poderemos desbloquear outro tipo de integração do Analytics, em tempo real, que pode unir todos os eventos Pass aos dados existentes do cliente, mais detalhes sobre esse novo tipo de possível integração aqui: &quot;[Uso da ID Experience Cloud na Autenticação Adobe Pass](/help/authentication/integration-guide-programmers/features-premium/analytics/exp-cloud-id-authn.md)&quot;

## Métricas incluídas {#metrics-included-int-authn-analyt}

| Evento | Descrição |
|----------------------------|----------------------------------------------------------------------------------------------------------------------|
| AuthN solicitado | Número de fluxos de autenticação iniciados |
| Autenticação pendente | Número de tokens de autenticação gerados com êxito (ignorando se o cliente realmente o obteve) |
| AuthN OK | Número de tokens de autenticação obtidos com êxito pelos usuários |
| AuthZ solicitado | Número de tentativas de autorização |
| AuthZ OK | Número de autorizações bem-sucedidas |
| Falha de AuthZ | Número de autorizações negadas por MVPDs no nível do aplicativo |
| Reproduzir solicitação | Número de tokens de mídia curtos gerados (que são assimilados pelo número de solicitações de reprodução) |
| Logout solicitado | Número de fluxos de logout iniciados |
| Logout concluído | Número de fluxos de logout concluídos com êxito |
| Falha no logout | Número de fluxos de logout com falha |
| Pré-autorização solicitada | Número de fluxos de pré-autorização iniciados |
| Pré-autorização OK | Número de eventos de pré-autorização bem-sucedidos com os recursos que foram pré-autorizados |
| Pré-autorização negada | Número de eventos de pré-autorização com os recursos aos quais foi negada a pré-autorização |
| Falha na pré-autorização | Número de eventos de pré-autorização que falharam |

| Nome do Adobe Analytics | Descrição |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Canal | A ID do solicitante usada para executar a solicitação de direito |
| MVPD | O MVPD responsável por conceder o direito ao usuário |
| Proxy | O proxy MVPD (que será &quot;Direto&quot; para integrações diretas) |
| Tipo de SDK | O SDK do cliente usado (Flash, HTML5, nativo do Android, iOS, sem clientes etc.) |
| Versão do SDK | A versão do SDK do cliente de autenticação da Adobe Pass |
| ID do recurso | O título do recurso real envolvido na solicitação de autorização (extraído da carga MRSS como o item/título, se fornecido) |
| Tipo de erro de AuthZ | O motivo das falhas, conforme relatado pela Autenticação Adobe Pass <br/>. Estes são os valores mais comuns <br/> **noAuthZ** = o MVPD respondeu que o usuário não tem o canal em seu pacote<br/> **rede** = não foi possível acessar o MVPD (o MVPD tem um problema no momento da chamada e não respondeu)<br/> **norefreshtoken** = isso é estritamente para implementações OAuth e pode ocorrer se o usuário alterou sua senha ou o MVPD a negou por algum motivo. Normalmente resulta em uma nova autenticação<br/> **incompatibilidade** = se a solicitação é feita de um dispositivo diferente daquele que tinha o token de autenticação. Pode ocorrer se os usuários tentarem enganar o sistema, mas a maioria disso ocorreu no contexto do antigo SDK do JavaScript, em que a ID do dispositivo estava usando o endereço IP como parte do cálculo. Se um usuário assistisse à TVE em casa e depois no trabalho, esse erro seria disparado e ele teria que se autenticar novamente<br/> **inválido** = solicitação inválida, parâmetros ausentes ou inválidos<br/>  **authzNone** = Os programadores podem negar autorizações para uma combinação específica de channelMVPD. Isso é disparado por uma API de back-end à qual os programadores têm acesso<br/> **fraude** = é um mecanismo de proteção da nossa parte. Se o usuário não conseguir a autorização e, em seguida, solicitar novamente um número de vezes em um curto intervalo (segundos), negamos a chamada diretamente. Normalmente, isso acontece quando um Programador tem um bug em sua implementação que solicita autorização constantemente se falhar. |
| Tipo de token | Quando os tokens são criados devido à AuthZ All e AuthN All, precisamos saber o que é causado por uma medida de degradação.<br/> São:<br/> &quot;normal&quot; = O caso normal<br/> &quot;authnall&quot; = Quando AuthN All está habilitado<br/> &quot;authzall&quot; = Quando AuthZ All está habilitado<br/> &quot;hba&quot; = Quando HBA está habilitado |
| Tipo de dispositivo sem cliente | A plataforma do dispositivo (alternativa), usada atualmente para Clientless.<br/> Os valores podem ser:<br/> N/D - o evento não se originou de um SDK sem Cliente<br/> Desconhecido - Como o parâmetro deviceType de uma **API sem Cliente** é opcional, há chamadas que não contêm nenhum valor.<br/> Qualquer outro valor enviado por meio da **API sem cliente**. Por exemplo, xbox, appletv e roku. |
| ID de usuário MVPD | Substitui a ID de visitante baseada em cookie |


## Detalhes {#details-int-authn-analyt}

* As métricas serão inseridas, evento por evento, no conjunto de relatórios específico por meio da API de inserção de dados do Adobe Analytics
* A inserção será agrupada e enviada a cada 30 minutos. Por isso, o relatório precisa ter carimbo de data e hora
* Cada cliente terá um ou vários conjuntos de relatórios. Uma ID de solicitante (canal) mapeará apenas para um conjunto de relatórios. Várias IDs de solicitante podem mapear para apenas um conjunto de relatórios.
* Os dados históricos podem ser fornecidos, mas é necessário tomar especial cuidado devido a preocupações com o tráfego/desempenho.
* A variável de visitante único é definida como a ID de usuário MVPD
* O mapeamento de eventos e eVars não pode ser configurado.


## SLA {#sla-int-authn-serv-anal}

Como não é um componente crítico, não há garantia da SLA para a integração.

## Configuração do conjunto de relatórios {#report-suite-config}

O relatório deve ter o carimbo de data e hora já que os eventos serão enviados em lotes.

### Eventos {#report-suite-config-events}


>[!NOTE]
>Todos devem ser definidos com:
>
>* Contador (sem sub-relações)

| Evento | Evento do Adobe Analytics |
|---------------------------------------|-----------------------|
| AuthN solicitado | event1 |
| Autenticação pendente | event2 |
| AuthN OK | event3 |
| AuthZ solicitado | event4 |
| AuthZ OK | event5 |
| Falha de AuthZ | evento6 |
| Reproduzir solicitação | event7 |
| Falha de autenticação | event8 |
| Solicitação de Token de Autenticação sem Cliente OK | evento9 |
| Falha na solicitação de token de autenticação sem cliente | event10 |
| Falha na solicitação de reprodução | event11 |
| Solicitação de logout | event12 |
| Logout concluído | event13 |
| Falha no logout | event14 |
| Solicitação de comprovação | event15 |
| Falha na comprovação | event16 |
| Comprovação concedida | event17 |
| Comprovação negada | evento18 |


### eVars {#evars}


>[!NOTE]
>Todos devem ser definidos com:
>
>* Alocação: Mais recente (último)
>* Expirar após: ocorrência
>* Tipo: sequência de caracteres de texto

| Propriedade | eVar |
|-----------------------------------|--------------------------------|
| Canal | EVAR 1 |
| MVPD | EVAR 2 |
| Proxy | EVAR 3 |
| Tipo de SDK | EVAR 4 |
| Versão do SDK | EVAR 5 |
| ID do recurso | EVAR 6 |
| Tipo de erro de AuthZ | EVAR 7 |
| Tipo de token | EVAR 8 |
| Tipo de dispositivo sem cliente | EVAR 9 |
| ID de usuário MVPD | visitorID (feito automaticamente) |
| ID de usuário MVPD | eVar 10 |
| Tipo de dispositivo | eVar 11 |
| Sistema operacional | eVar 12 |
| Tipo de hardware primário | eVar 13 |
| TTL | eVar 14 |
| Tipo de autenticação | eVar 15 |
| Versão da arquitetura do servidor | eVar 16 |
| Provedor de autenticação externo | eVar 17 |
| Latência | eVar 18 |
| Id Do Visitante | eVar 19 |
| Mecanismo SSO | eVar 20 |
| Modelo do dispositivo | eVar 21 |
| Versão do dispositivo | eVar 22 |
| Modelo de hardware do dispositivo | eVar 23 |
| Fornecedor de hardware do dispositivo | eVar 24 |
| Fabricante do hardware do dispositivo | eVar 25 |
| Versão do Hardware do Dispositivo | eVar 26 |
| Nome do sistema operacional do dispositivo | eVar 27 |
| Família de SOs do dispositivo | eVar 28 |
| Fornecedor do sistema operacional do dispositivo | eVar 29 |
| Versão do sistema operacional do dispositivo | eVar 30 |
| Nome do Navegador de Dispositivos | eVar 31 |
| Fornecedor do Navegador de Dispositivos | eVar 32 |
| Versão do Navegador de Dispositivos | eVar 33 |
| Tipo de dispositivo sem cliente normalizado | eVar 34 |

## Preços {#pricing}

Entre em contato com seu TAM para obter mais informações.
