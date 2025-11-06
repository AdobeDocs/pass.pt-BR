---
title: Autenticação baseada em casa (HBA)
description: Autenticação baseada em casa (HBA)
exl-id: abdc7724-4290-404a-8f93-953662cdc2bc
source-git-commit: ffedb5db269644c8d9c81480d27dff43bd4eb5d6
workflow-type: tm+mt
source-wordcount: '1308'
ht-degree: 2%

---

# Autenticação baseada em casa (HBA) {#home-based-authentication}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual da Adobe. Não é permitida nenhuma utilização não autorizada.

A Autenticação baseada em casa (HBA) é um recurso da TV Everywhere que permite que assinantes de TV paga acessem conteúdo de TV on-line sem inserir credenciais do MVPD quando conectados à rede doméstica, melhorando muito a experiência de autenticação.

De acordo com o Open Authentication Technology Committee (OATC):

> &quot;A autenticação automática in-home é o processo pelo qual um MVPD/OVD usa características da rede doméstica (ou identificadores acessíveis automaticamente entre dispositivos na rede doméstica) para autenticar qual conta de assinante está associada a essa rede doméstica, eliminando a necessidade de os usuários inserirem credenciais manualmente ao iniciarem uma sessão TVE para acessar conteúdo protegido.&quot;

Para obter mais informações sobre a Autenticação baseada em casa (HBA) e os padrões relevantes do setor, consulte os seguintes recursos:

>[!MORELIKETHIS]
>
> * [Acesso Instantâneo (HBA) por CTAM](http://www.ctamtve.com/instantaccess)
> * [Casos de Uso e Requisitos de Autenticação Baseada em Casa por OATC](https://tve.helpdocsonline.com/topic/awsfiles/download_files?ref=https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/Defining%20TVE%20Home-Based%20Authentication%20(HBA)%20%20Use%20Cases%20and%20Requirements%20Recommended%20Practice%20Version%201_0%20FINAL%20DRAFT%20FOR%20BOARD%20APPROVAL.pdf)
> * [Infográfico de Autenticação Baseado em Casa do Adobe](https://tve.helpdocsonline.com/topic/awsfiles/download_files?ref=https://dzf8vqv24eqhg.cloudfront.net/userfiles/258/326/ckfinder/files/AdobeNewsletterHBA.pdf?dc=201604260953-2640)
> * [Autenticação com MVPDs habilitados para OAuth2](/help/authentication/integration-guide-mvpds/authn-oauth2-protocol.md)
> * [Autenticação com MVPDs habilitados para SAML](/help/authentication/integration-guide-mvpds/authn-usecase.md)
> * [Metadados de usuário](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md)

## Vantagens do HBA {#hba-benefits}

A Autenticação baseada em casa (HBA) é um recurso essencial que remove a barreira de entrada dos visualizadores em casa com uma assinatura ativa por cabo. Essa barreira é um desafio significativo para os serviços da TV Everywhere, com quase metade de todas as tentativas de logon resultando em falha.

O HBA pode melhorar muito o engajamento do visualizador, fornecendo uma experiência perfeita e superior ao usuário para acessar o conteúdo da TV em todos os lugares.

## Suporte a HBA {#hba-support}

>[!IMPORTANT]
>
> Se você estiver interessado em aproveitar a funcionalidade do HBA, entre em contato com o representante de vendas de Autenticação da Adobe Pass para obter mais informações, pois determinados fluxos de HBA estão incluídos no pacote de Fluxo de trabalho Premium.

O HBA é suportado por vários MVPDs que estão integrados à autenticação da Adobe Pass, mas para se beneficiar do HBA, talvez seja necessário executar algumas etapas adicionais.

**MVPDs de SAML**

Para MVPDs SAML, o HBA é ativado somente no lado do MVPD.

**MVPDs do OAuth2**

Para MVPDs OAuth2, o HBA pode ser ativado ou desativado por meio do [Painel do Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication) seguindo as etapas do [Guia do Usuário de Integrações do Painel do TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-integrations.md#most-used-flows).

### MVPDs {#hba-support-mvpds}

**MVPDs de SAML**

A tabela a seguir fornece uma visão geral dos MVPDs habilitados para SAML que oferecem suporte a HBA:

| MVPD | Funcionalidade básica disponível? | Envia sinalizador na resposta de autenticação? |
|--------------|--------------------------------|----------------------------------------|
| DirecTV | Sim | Não |
| Prato | Sim | Não |
| Espectro | Sim | Sim |
| Cox | Sim | Não |
| AT&amp;T | Sim | Não |
| Verizon | Sim | Sim |
| Visão do cabo | Sim | Não |
| Mediacom | Sim | Não |
| Centro-continente | Sim | Não |
| Massilon | Sim | Não |
| AlticeOne | Sim | Sim |

**MVPDs do OAuth2**

A tabela a seguir fornece uma visão geral dos MVPDs ativados para OAuth2 que oferecem suporte a HBA:

| MVPD | Funcionalidade básica disponível? | Envia sinalizador na resposta de autenticação? |
|---------|--------------------------------|----------------------------------------|
| Comcast | Sim | Sim |

### Adobe Pass Authentication {#hba-support-adobe-pass-authentication}

Esta seção descreve a experiência habilitada para HBA e detalha o suporte fornecido pela Autenticação Adobe Pass, destacando os principais recursos, como:

* **Identificação do HBA:** Capacidade de indicar aos programadores se a autenticação foi HBA ou não (requer suporte da MVPD).
* **TTLs de Autenticação Configuráveis:** Capacidade de definir valores de TTL (Time-To-Live) de autenticação diferentes para autenticações HBA versus autenticações não HBA (requer suporte da MVPD).

A tabela a seguir fornece uma visão geral de alto nível da experiência do usuário em um HBA e do fluxo de autenticação regular (não-HBA):

| Tipo de autenticação | Experiência do usuário |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HBA | Os usuários selecionam o MVPD e são automaticamente autenticados quando conectados à rede doméstica. Quando a autenticação expirar, os usuários precisarão selecionar novamente a MVPD e, depois disso, serão autenticados novamente automaticamente. |
| Normal (não HBA) | Os usuários selecionam sua MVPD e são solicitados a inserir suas credenciais, independentemente da conexão com uma rede doméstica. Quando a autenticação expirar, os usuários deverão selecionar novamente sua MVPD e inserir suas credenciais novamente. |

**MVPDs de SAML**

A tabela a seguir fornece uma visão geral da implementação do HBA no caso de MVPDs habilitados para SAML:

| Ações do usuário | Ações do sistema |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| O usuário tenta reproduzir um vídeo.<br/><br/>O seletor de MVPD é exibido.<br/><br/>O usuário seleciona o MVPD e continua a fazer logon. | Uma verificação de antecedentes é realizada onde o MVPD aplica suas regras para identificação de usuários. Por exemplo, isso pode envolver o mapeamento do endereço IP do usuário para o endereço MAC de modems provisionados por distribuidor ou decodificadores de sinais conectados à banda larga. |
| Uma tela que persiste por aproximadamente 3 segundos é exibida.<br/><br/>Uma página intersticial pode informar ao usuário que ele está entrando automaticamente usando sua conta da MVPD. | O aplicativo abre o URL de autenticação em um agente do usuário, iniciando uma solicitação HTTP para o endpoint de autenticação da Adobe Pass.<br/><br/>O ponto de extremidade de Autenticação do Adobe Pass encaminha a solicitação ao ponto de extremidade de autenticação da MVPD por meio de um redirecionamento de agente do usuário.<br/><br/>Espera-se que o MVPD envie uma decisão de autenticação na forma de uma resposta SAML que inclua o sinalizador HBA (`hba_status`) com um valor &quot;true&quot; ou &quot;false&quot;.O <br/><br/>back-end de Autenticação do Adobe Pass faz uma solicitação ao ponto de extremidade do perfil de usuário do MVPD para expor o sinalizador `hba_status` como parte dos [metadados do usuário](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md). |
| O usuário é autenticado e agora pode navegar pelo conteúdo intitulado TV em todos os lugares. | O aplicativo recupera o perfil do usuário e pode continuar com o fluxo de decisões para reproduzir o conteúdo. |

**MVPDs do OAuth2**

A tabela a seguir fornece uma visão geral da implementação do HBA no caso de MVPDs ativados para OAuth2:

| Ações do usuário | Ações do sistema |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| O usuário tenta reproduzir um vídeo.<br/><br/>O seletor de MVPD é exibido.<br/><br/>O usuário seleciona o MVPD e continua a fazer logon. | Uma verificação de antecedentes é realizada onde o MVPD aplica suas regras para identificação de usuários. Por exemplo, isso pode envolver o mapeamento do endereço IP do usuário para o endereço MAC de modems provisionados por distribuidor ou decodificadores de sinais conectados à banda larga. |
| Uma tela que persiste por aproximadamente 3 segundos é exibida.<br/><br/>Uma página intersticial pode informar ao usuário que ele está entrando automaticamente usando sua conta da MVPD. | O aplicativo abre o URL de autenticação em um agente do usuário, iniciando uma solicitação HTTP para o endpoint de autenticação da Adobe Pass.<br/><br/>O ponto de extremidade de Autenticação do Adobe Pass encaminha a solicitação ao ponto de extremidade de autenticação da MVPD por meio de um redirecionamento de agente do usuário.<br/><br/>O ponto de extremidade de autenticação do MVPD envia um código de autorização para o ponto de extremidade de Autenticação do Adobe Pass.A Autenticação do <br/><br/>Adobe Pass usa o código de autorização para solicitar um token de atualização e um token de acesso do ponto de extremidade do token do MVPD.<br/><br/>Espera-se que a MVPD envie uma decisão de autenticação que inclua o sinalizador de HBA (`hba_status`) com um valor &quot;true&quot; ou &quot;false&quot; como parte de `id_token`.O <br/><br/>back-end de Autenticação do Adobe Pass faz uma solicitação ao ponto de extremidade do perfil de usuário do MVPD para expor o sinalizador `hba_status` como parte dos [metadados do usuário](/help/authentication/integration-guide-programmers/features-standard/entitlements/user-metadata.md).<br/><br/>O MVPD define o TTL do token de atualização como um valor acordado pela MVPD, e o Adobe define o TTL de autenticação como um valor menor ou igual ao valor do token de atualização. |
| O usuário é autenticado e agora pode navegar pelo conteúdo intitulado TV em todos os lugares. | O aplicativo recupera o perfil do usuário e pode continuar com o fluxo de decisões para reproduzir o conteúdo. |

>[!IMPORTANT]
>
> No fluxo de HBA para MVPDs usando o protocolo de autenticação OAuth 2.0, o MVPD emite um token de atualização com um TTL definido por seus requisitos de negócios, enquanto o Adobe emite um token de autenticação de HBA com um TTL que não deve exceder o TTL do token de atualização.

## Perguntas frequentes {#faqs}

1. Por que há uma distinção entre a implementação de HBA para os protocolos SAML e OAuth2?

   A separação entre a Autenticação baseada em casa (HBA) para os protocolos SAML e OAuth2 existe porque esses protocolos operam de forma diferente em termos de mecanismos de autenticação, configuração e flexibilidade de implementação. Para MVPDs SAML, nenhuma ação é necessária por parte do Programador para habilitar o HBA, enquanto para MVPDs OAuth2, o HBA pode ser ativado ou desativado por meio do [Painel do Adobe Pass TVE](https://experience.adobe.com/#/pass/authentication).

1. Quando o HBA é ativado, os usuários ainda precisam digitar seus nomes de usuário e senhas durante a autenticação inicial?

   Não, o nome de usuário e a senha não são obrigatórios.

1. Como você pode aplicar o controle dos pais?

   A Autenticação da Adobe Pass pode desabilitar o HBA para integrações com canais que precisam de aprovação de controle dos pais. Além disso, estamos trabalhando com o OATC em um documento UX que recomenda como configurar a experiência de HBA com controles dos pais.

1. Os provedores que oferecem suporte a HBA usam janelas TTL menores para HBA em comparação com a autenticação normal?

   A configuração de TTL é configurável. Recomendamos definir um TTL mais curto para integrações do MVPD com o HBA para evitar manipulações incorretas.
