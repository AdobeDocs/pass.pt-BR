---
title: Suporte para logon único
description: Suporte para logon único
exl-id: edc3719e-c627-464c-9b10-367a425698c6
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1135'
ht-degree: 0%

---

# Suporte para logon único

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Visão geral {#overview-sso-support}

Este documento descreve os tipos de Logon único compatíveis e viabilizados pela Autenticação Adobe Pass em diferentes plataformas. O escopo deste documento é esclarecer o que é ou não suportado, qual é a cobertura MVPD para cada método SSO e o que é necessário dos programadores para poderem se beneficiar do SSO em cada plataforma.

Depois que um usuário faz logon com suas credenciais do MVPD, a Autenticação do Adobe Pass gera um token seguro que representa a sessão de Autenticação do MVPD e vincula esse token ao dispositivo do usuário usando uma ID de dispositivo. A Autenticação do Adobe Pass armazena o token/ID do dispositivo em um servidor ou no dispositivo. Isso permite que os usuários insiram suas credenciais com menos frequência, mantendo as transações seguras.

>[!NOTE]
>
>Os workflows de SSO fazem parte do pacote Premium Workflow. Entre em contato com seu representante de vendas da Adobe Pass, se estiver interessado em usar essa funcionalidade.

## Status atual do SSO em várias plataformas {#current-sso-status-platforms}

| Plataforma/dispositivo | Suporte a SSO | Tipo de SSO | Cobertura MVPD | Notas |
|:-------------------:|:-----------:|:---------------------------------------:|-----------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| Web (JavaScript) | Sim | Token de autenticação compartilhado (Adobe SSO) | Todos | Sem SSO entre navegadores Siga as instruções no Guia de integração do programador para JavaScript. Após seguir as instruções, o SSO é ativado por padrão.  A habilitação da autenticação por solicitante interrompe o SSO |
| iOS | Sim | SSO da plataforma - troca de tokens | Dependendo do suporte da Apple, a lista pode ser encontrada aqui | A partir do iOS 10, o Apple &amp; Adobe introduziu a funcionalidade SSO para os programadores participantes e MVPDs. Ao usar o SDK do iOS do Adobe mais recente ou usar a API REST sem clientes do Adobe e implementar a funcionalidade SSO do Apple, você pode se beneficiar do SSO em dispositivos iOS. Mais detalhes sobre a implementação do SDK aqui e mais detalhes sobre a implementação sem clientes aqui. Observações adicionais: - Se não quiser usar o Apple SSO, você ainda poderá ter um SSO limitado entre aplicativos do mesmo fornecedor (mesma ID do pacote) que podem compartilhar armazenamento e uma ID (IDFV). Portanto, o SSO é limitado apenas aos aplicativos do mesmo fornecedor. |
| Android | Sim | Token de autenticação compartilhado (Adobe SSO) | Todos | Se o usuário não aceitar a solicitação de permissão WRITE_EXTERNAL_STORAGE, a biblioteca usará um armazenamento local em modo seguro. A implicação nesse caso é que não haverá SSO entre aplicativos diferentes ao usar o armazenamento local. |
| tvOS - nova Apple TV | Sim | SSO da plataforma - troca de tokens | Dependendo do suporte da Apple, a lista pode ser encontrada aqui | A partir do tvOS 10, o Apple &amp; Adobe introduziu a funcionalidade SSO para os programadores participantes e MVPDs. Ao usar o SDK tvOS de Adobe mais recente ou ao usar a API REST sem cliente do Adobe e implementar a funcionalidade SSO do Apple, você pode se beneficiar do SSO em dispositivos tvOS. Mais detalhes sobre o tvOS SDK: aqui e aqui e mais detalhes sobre a implementação sem clientes aqui. |
| Roku | Sim | Token de autenticação compartilhado (Adobe SSO) | Lista completa de cobertura significativa a ser fornecida em breve. | O Roku SSO funciona imediatamente com a API sem clientes para todos os clientes que respeitam as diretrizes do Roku, nenhuma implementação especial necessária. O SSO é baseado nas informações de identificação do dispositivo que o Roku está enviando com segurança para o Adobe. |
| Amazon FireTV | Sim | Token de autenticação compartilhado (Adobe SSO) | Lista completa de cobertura significativa a ser fornecida em breve. | O SDK do FireTV fornece suporte para Logon único com base nos recursos do Android. O SSO nesta plataforma é possível somente entre aplicativos que estão usando o SDK do Adobe FireTV por enquanto. Mais informações sobre o novo SDK do FireTV aqui. Os aplicativos FireTV implementados com base na API sem cliente poderão se beneficiar do SSO até o final de 2018. |
| Xbox 360 | Não |                                         |                                                     | Não há uma ID de dispositivo que possamos aproveitar. Há uma ID do aplicativo, portanto, os usuários não precisam se autenticar sempre. |
| Xbox One | Não |                                         |                                                     | Não há uma ID de dispositivo que possamos aproveitar. Há uma ID do aplicativo, portanto, os usuários não precisam se autenticar sempre. |
| Windows 8/10 | Não |                                         |                                                     | Não há uma ID de dispositivo que possamos aproveitar. Há uma ID do aplicativo, portanto, os usuários não precisam se autenticar sempre. |
| Samsung TVs | Não |                                         |                                                     | Não há uma ID de dispositivo que possamos aproveitar. Há uma ID do aplicativo, portanto, os usuários não precisam se autenticar sempre. |

### Notas no Xbox 360 e Xbox One {#notes-xbox-360}

* **Xbox 360**- O Xbox 360 depende do Serviço Live para fornecer o token que incorpora a deviceID. As camadas do Live Service no valor appID para deviceID, tornando-o escopo somente para o aplicativo. Para o Xbox 360, a Microsoft forneceu ao Adobe uma biblioteca Java para ajudar na análise do token.

* **Xbox One**- Será emitido um token da Web JSON criptografado com o certificado/chave do editor e assinado pelo Microsoft. O Adobe extrai a deviceID de um parâmetro chamado DPI (ID do dispositivo emparelhado), diferente do parâmetro Xbox 360 PDID (ID do dispositivo do parceiro). A PDID também existe no Xbox One, mas deve ser substituída por este novo parâmetro &quot;ID emparelhado do dispositivo&quot; (DPI).


### Desabilitar SSO {#disable-sso}

Em determinadas situações, alguns aplicativos ou sites desejarão desativar o SSO para atender a casos de negócios avançados.

* **Para JS e SDKs nativos** - A equipe de suporte da Autenticação do Adobe Pass pode desativar o SSO para um par de ID do solicitante/MVPD. Nenhum trabalho é necessário em sites ou aplicativos nativos.  Depois que o SSO é desativado pela equipe de suporte da Autenticação do Adobe Pass, as autenticações executadas usando o par RequestorId / MVPD especificado não serão compartilhadas com sites ou aplicativos que usam IDs diferentes do Solicitante. Além disso, as autenticações existentes com IDs do Solicitante diferentes não serão válidas para a combinação ID do Solicitante/MVPD na qual o SSO foi desabilitado. Tecnicamente, a desativação do SSO é realizada vinculando o token de autenticação à combinação específica ID do solicitante/MVPD.
* **Para API sem cliente** - Você pode desativar o SSO no fluxo de autenticação sem cliente especificando um parâmetro appId não vazio nas chamadas REST. Você pode usar qualquer string como valor, desde que essa string seja exclusiva para a ID do solicitante. Observe que para a API sem cliente, o programador/implementador deve alterar o site ou aplicativo para adicionar esse parâmetro específico do solicitante.

>[!IMPORTANT]
>
>NOTA IMPORTANTE PARA SSO DA API SEM CLIENTE: alguns MVPDs exigem que cada rede (ID do solicitante) execute seu próprio fluxo de autenticação. Para os fluxos baseados em SDK (iOS etc.), isso é manipulado automaticamente pelo SDK. No entanto, para as APIs sem cliente, isso precisa ser tratado pelo Programador. Recomendamos enfaticamente que os programadores não ativem fluxos de SSO para APIs sem cliente neste momento e, em vez disso, usem uma combinação de ID de dispositivo + ID de aplicativo para a ID de dispositivo. O Adobe também funcionará no aprimoramento dos fluxos da API sem cliente para que o SSO adequado possa ser estabelecido.

### Sair {#logout-sso-support}

Os programadores precisam estar cientes de que a ação &quot;Logout&quot; no contexto de Logon único, quando executada em um aplicativo/site, excluirá todos os tokens no dispositivo e o usuário será desconectado dos aplicativos/sites.

Se as condições de SSO forem atendidas (independentemente de o SSO estar ou não ativado ou desativado), o Logout será executado e todas as informações de autenticação e autorização serão excluídas.
