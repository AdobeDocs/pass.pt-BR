---
title: Amazon fireTV SSO - Guia de início do programador
description: Amazon fireTV SSO - Guia de início do programador
exl-id: cf9ba614-57ad-46c3-b154-34204b38742d
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '793'
ht-degree: 0%

---

# Amazon fireTV SSO - Guia de início do programador {#amazon-firetv-sso---programmer-kick-off-guide}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

</br>

## Introdução {#intro}

Este documento descreve as informações necessárias para integrar a nova **SDK fireTV da Autenticação do Adobe Pass** no aplicativo fireTV. Esse novo SDK aproveita a integração no nível do SO na plataforma fireTV da Amazon, oferecendo **Logon único** suporte. Para se beneficiar do Logon único, é necessário um pequeno esforço da sua parte para migrar seu aplicativo da API sem cliente para o novo SDK do fireTV. Há algumas alterações nos fluxos de autenticação que serão detalhadas abaixo.

## Arquitetura de alto nível e integração no nível do SO {#high}

Para alcançar o Logon único entre os aplicativos da TV Everywhere na plataforma FireTV da Amazon e para melhorar a experiência geral nessa plataforma, decidimos integrar nosso SDK principal no nível do sistema operacional fireTV. Os programadores serão solicitados a compilar em uma biblioteca de stub fornecida pelo Adobe. A funcionalidade real será fornecida pela biblioteca Adobe presente no FireTV OS da Amazon.

Até que o Amazon forneça um simulador fireTV que incorpore nossa biblioteca no nível do SO, o desenvolvimento será possível somente usando dispositivos fireTV reais.

## Benefícios {#bene}

* Single Sign On entre todos os aplicativos de TV Everywhere alimentados por Adobe na plataforma FireTV da Amazon com todos os MVPDs integrados.
* Capacidade de se beneficiar do HBA (com MVPDs compatíveis).
* Capacidade de usar o SDK fireTV mais recente sem a necessidade de atualizar seus aplicativos sempre que uma nova versão do SDK for lançada.
* Todos os aplicativos TVE se beneficiam do uso da biblioteca de sistema compartilhado, removendo a necessidade de ter uma cópia local da biblioteca do AccessEnabler. Isso também garante que todos os aplicativos estejam usando a mesma versão do SDK.
* Autenticação de tela única: sem necessidade de código de registro e fluxos de trabalho de segunda tela.

## Migração do aplicativo baseado em API sem cliente para o aplicativo baseado em SDK do fireTV {#migra1}

Para migrar da API sem cliente para o FireTV SDK, é necessário remover a base de código relacionada à API sem cliente e integrar o novo FireTV SDK.

Comparado com o aplicativo baseado em API sem cliente, com o novo SDK do fireTV, a autenticação é movida para a primeira tela; não há mais necessidade de uma segunda autenticação de tela.

Isso requer que os programadores adicionem um seletor de MVPD em seus aplicativos para que os usuários possam escolher seu provedor de TV diretamente no dispositivo fireTV. Após a seleção do MVPD, o usuário verá a página de logon do MVPD no dispositivo fireTV.

Os wireframes dos fluxos de usuário que descrevem os cenários normal, HBA e SSO no fireTV podem ser encontrados em [Fluxo do usuário de logon do Amazon Fire TV - MVPD](https://xd.adobe.com/view/9058288e-4b67-43a1-9d5b-5f76ede6c51e/).

## Migração de um aplicativo com base em Android SDK para um aplicativo com base em FireTV SDK {#migra2}

Este novo SDK do FireTV é muito semelhante ao nosso SDK do Android existente e à documentação atual que temos para **integração do Android SDK** <!--http://tve.helpdocsonline.com/android-technical-overview-->O pode ser usado até que os documentos do SDK do fireTV estejam prontos. Se você já tiver aplicativos Android que usam nosso Android SDK, a integração do FireTV SDK no aplicativo FireTV deve ser simples.

Em comparação com o Android SDK existente, no FireTV SDK, o processo de autenticação será mais simples de desenvolver, pois as tarefas de gerenciamento/apresentação da página de logon MVPD e recuperação do token AuthN serão executadas internamente pela biblioteca do AccessEnabler.

## Perguntas frequentes {#faq}

1. Como o **SSO** trabalhar?

   * O SSO funcionará em todos os aplicativos de Programador alimentados pela Autenticação Adobe Pass que estão usando o novo SDK do fireTV no mesmo dispositivo FireTV da Amazon
   * SSO entre aplicativos Programmer implementados na API REST sem cliente e aplicativos implementados no SDK fireTV **NÃO será suportado**

1. Qual é a cobertura MVPD do FireTV SSO?

   * **Todos os MVPDs** integrado pela Adobe Pass Authentication terá suporte técnico para SSO no SDK do fireTV.

1. Além de usar o novo SDK, que outro **alterações no fluxo de trabalho** Os programadores devem estar cientes do?

   * Os programadores precisam implementar um seletor de MVPD para a plataforma fireTV.

1. Haverá alguma alteração na autenticação **TTLs**?

   * Não há alteração no comportamento em relação aos TTLs de autenticação.
   * O primeiro token de autenticação válido será usado para executar o SSO e, nesse caso, todos os outros aplicativos que serão autenticados por meio do SSO usarão o mesmo TTL até expirar. Assim, ao navegar de um aplicativo para outro, o segundo aplicativo compartilhará o TTL do primeiro aplicativo autenticado.

1. Como o **API de degradação** trabalhar?

   * Não há alterações necessárias para a API de degradação. A experiência do usuário será a mesma dos dispositivos Android.

1. Como **TempPass** os fluxos são afetados?

   * Os fluxos do TempPass são de tela única e se comportam como em qualquer outro dispositivo nativo.

1. Outra funcionalidade de Adobe funcionará como antes?

   * Toda a funcionalidade de autenticação da Adobe Pass funcionará no fireTV como em dispositivos Android.
