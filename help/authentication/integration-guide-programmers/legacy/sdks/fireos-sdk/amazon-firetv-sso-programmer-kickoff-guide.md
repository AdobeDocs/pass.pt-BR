---
title: Amazon fireTV SSO - Guia de início do programador
description: Amazon fireTV SSO - Guia de início do programador
exl-id: cf9ba614-57ad-46c3-b154-34204b38742d
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '805'
ht-degree: 0%

---

# (Herdado) Amazon fireTV SSO - Guia de início do programador {#amazon-firetv-sso---programmer-kick-off-guide}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

</br>

## Introdução {#intro}

Este documento descreve as informações necessárias para integrar a nova **FireTV SDK** da Autenticação Adobe Pass ao seu aplicativo fireTV. Esta nova SDK aproveita a integração no nível do SO na plataforma fireTV da Amazon, oferecendo assim suporte ao **Logon único**. Para se beneficiar do Logon único, é necessário um pequeno esforço da sua parte para migrar seu aplicativo da API sem cliente para o novo FireTV SDK. Há algumas alterações nos fluxos de autenticação que serão detalhadas abaixo.

## Arquitetura de alto nível e integração no nível do SO {#high}

Para alcançar o Logon único entre os aplicativos da TV Everywhere na plataforma fireTV da Amazon e para melhorar a experiência geral nessa plataforma, decidimos integrar nosso SDK principal no nível do sistema operacional fireTV. Os programadores serão solicitados a compilar em uma biblioteca de stub fornecida pelo Adobe. A funcionalidade real será fornecida pela biblioteca Adobe presente no FireTV OS da Amazon.

Até que o Amazon forneça um simulador fireTV que incorpore nossa biblioteca no nível do SO, o desenvolvimento será possível somente usando dispositivos fireTV reais.

## Benefícios {#bene}

* Single Sign On entre todos os aplicativos de TV Everywhere alimentados por Adobe na plataforma FireTV da Amazon com todos os MVPDs integrados.
* Capacidade de se beneficiar do HBA (com MVPDs compatíveis).
* Capacidade de usar o FireTV SDK mais recente sem a necessidade de atualizar seus aplicativos sempre que uma nova versão do SDK for lançada.
* Todos os aplicativos TVE se beneficiam do uso da biblioteca de sistema compartilhado, removendo a necessidade de ter uma cópia local da biblioteca do AccessEnabler. Isso também garante que todos os aplicativos estejam usando a mesma versão do SDK.
* Autenticação de tela única: sem necessidade de código de registro e fluxos de trabalho de segunda tela.

## Migração do aplicativo baseado em API sem cliente para o aplicativo baseado em SDK fireTV {#migra1}

Para migrar da API sem cliente para o fireTV SDK, é necessário remover a base de código relacionada à API sem cliente e integrar a nova SDK do fireTV.

Comparado com o aplicativo baseado em API sem cliente, com o novo FireTV SDK, a autenticação passa para a primeira tela, não há mais necessidade de uma segunda autenticação de tela.

Isso requer que os programadores adicionem um seletor de MVPD em seus aplicativos para que os usuários possam escolher seu provedor de TV diretamente no dispositivo fireTV. Após a seleção do MVPD, o usuário verá a página de logon do MVPD no dispositivo fireTV.

Os wireframes dos fluxos de usuário que descrevem os cenários normal, HBA e SSO no fireTV podem ser encontrados em [Fluxo de usuário de entrada do Amazon Fire TV - MVPD](https://xd.adobe.com/view/9058288e-4b67-43a1-9d5b-5f76ede6c51e/).

## Migração do aplicativo com base no Android SDK para o aplicativo com base no fireTV SDK {#migra2}

Este novo FireTV SDK é muito semelhante ao nosso Android SDK existente, e a documentação atual que temos para **integração do nosso Android SDK** <!--http://tve.helpdocsonline.com/android-technical-overview-->pode ser usada até que os documentos do FireTV SDK estejam prontos. Se você já tiver aplicativos do Android que usam nosso Android SDK, a integração do FireTV SDK com seu aplicativo FireTV deve ser simples.

Em comparação com o Android SDK existente, no fireTV SDK o processo de autenticação será mais simples de desenvolver, pois as tarefas de gerenciamento/apresentação da página de logon do MVPD e recuperação do token AuthN serão executadas internamente pela biblioteca AccessEnabler.

## Perguntas frequentes {#faq}

1. Como o **SSO** funcionará?

   * O SSO funcionará em todos os aplicativos do Programador alimentados pela Autenticação Adobe Pass que estão usando o novo FireTV SDK no mesmo dispositivo Amazon fireTV
   * O SSO entre os aplicativos Programador implementados na API REST sem Cliente e os aplicativos implementados no fireTV SDK **NÃO terão suporte**

1. Qual é a cobertura da MVPD sobre o FireTV SSO?

   * **Todos os MVPDs** integrados pela Autenticação Adobe Pass terão suporte técnico para SSO no fireTV SDK.

1. Além de usar a nova SDK, quais outras **alterações no fluxo de trabalho** os programadores devem estar cientes?

   * Os programadores precisam implementar um seletor de MVPD para a plataforma fireTV.

1. Haverá alguma alteração nos **TTLs** de autenticação?

   * Não há alteração no comportamento em relação aos TTLs de autenticação.
   * O primeiro token de autenticação válido será usado para executar o SSO e, nesse caso, todos os outros aplicativos que serão autenticados por meio do SSO usarão o mesmo TTL até expirar. Assim, ao navegar de um aplicativo para outro, o segundo aplicativo compartilhará o TTL do primeiro aplicativo autenticado.

1. Como a **API de degradação** funciona?

   * Não há alterações necessárias para a API de degradação. A experiência do usuário será a mesma dos dispositivos Android.

1. Como os fluxos de **TempPass** são afetados?

   * Os fluxos do TempPass são de tela única e se comportam como em qualquer outro dispositivo nativo.

1. Outra funcionalidade de Adobe funcionará como antes?

   * Toda a funcionalidade de Autenticação do Adobe Pass funcionará no fireTV como em dispositivos Android.
