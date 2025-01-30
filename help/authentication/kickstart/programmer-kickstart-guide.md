---
title: Guia de início rápido do programador
description: Guia de início rápido do programador
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: 37858fa83aecbdf443a4a6058c78e4f9246eee42
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 0%

---

# Guia de início rápido do programador {#programmer-kickstart-guide}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Este guia de início rápido destina-se a provedores de conteúdo (programadores) que planejam integrar a Autenticação Adobe® Pass aos seus sites ou aplicativos.

Este documento descreve as principais etapas iniciais para garantir um início tranquilo e eficiente do processo de integração. Ela tem como objetivo esclarecer expectativas e fornecer orientação sobre como colaboraremos com parceiros para obter integrações bem-sucedidas.

O Adobe fornece uma variedade de recursos para ajudar você a integrar a Autenticação Adobe Pass ao seu site ou aplicativo. Consulte as menções **&quot;Você fornecerá&quot;** e **&quot;Adobe fornecerá&quot;** de cada seção abaixo.

## Configurar processo {#setup-process}

O processo de configuração envolve, entre outras, as seguintes etapas:

![Processo de integração de autenticação do Adobe® Pass](../assets/progr-flow-int-lifecycle.png)

*Processo de integração de autenticação do Adobe® Pass*

**Você fornecerá** durante a fase inicial:

* **Provedor de serviços (identificador do solicitante)**

  Esta é uma sequência de caracteres que identificará exclusivamente a marca do site ou do aplicativo que está fazendo solicitações de autenticação da Adobe Pass. A cadeia de caracteres em si é arbitrária, mas precisa ser acordada entre o Adobe e o Programador

* **Informações do canal**

  Este é um conjunto de strings usado para identificar os canais de conteúdo solicitados pelo provedor de serviço. Em muitos casos, o canal e o provedor de serviços são os mesmos. No entanto, um único identificador pode representar vários canais de conteúdo. Essas cadeias de caracteres de nome de canal devem estar alinhadas aos canais de TV a cabo correspondentes. Observe que alguns MVPDs podem validar esse valor durante o processo de autenticação e/ou autorização.

* **Nomes de domínio**

  Essa lista incluirá os nomes de domínio reais listados para Adobe para representar o provedor de serviços. Ele garante que somente seus domínios autorizados possam acessar a Autenticação do Adobe Pass usando seus metadados. Certifique-se de fornecer e identificar claramente os nomes de domínio para ambientes de produção e de preparo (teste), pois eles podem ser diferentes.

**Você fornecerá** via MVPD:

* **Conjuntos de credenciais**

  Essas são credenciais usadas para autenticar e autorizar, ou apenas autenticar, o usuário com a MVPD. Normalmente, essas credenciais consistem em um nome de usuário e uma senha, que a MVPD fornecerá para ambos os perfis (armazenamento temporário e produção).

* **Identificadores de recursos**

  Esses são identificadores exclusivos para os canais de conteúdo, programas, episódios ou ativos que o provedor de serviços deseja proteger. Esses identificadores são usados para solicitar decisões de autorização e devem ser acordados com a MVPD.

>[!IMPORTANT]
>
> O Programador é responsável pela coordenação com a MVPD para finalizar quaisquer contratos comerciais necessários. Enquanto isso, a autenticação da Adobe Pass colaborará com a MVPD para garantir que a integração técnica seja estabelecida corretamente:
>
> * **Novo MVPD**
>
>     Se o MVPD não estiver integrado ao Adobe, o código personalizado deverá ser desenvolvido com base nos requisitos específicos do MVPD. Até que esse desenvolvimento seja concluído, o MVPD não estará disponível e o teste do produto com esse MVPD não poderá continuar.
>
> * **MVPD Existente**
>
>     Se o MVPD já estiver integrado ao Adobe, o processo de conectividade será significativamente simplificado. Na maioria dos casos, a conectividade pode ser estabelecida rapidamente por meio de ajustes de configuração, em vez de desenvolvimento extensivo.
>
> Todas as integrações exigem esforços conjuntos de controle de qualidade (QA), inclusive testes feitos pela MVPD, já que o usuário final é, em última análise, um cliente da MVPD. A coordenação dos ciclos de teste geralmente depende da disponibilidade de recursos do MVPD, o que pode causar possíveis atrasos.

## Acesso ao suporte ao cliente {#access-customer-support}

O **Adobe fornecerá** acesso ao nosso sistema de suporte ao cliente via [Zendesk](https://tve.zendesk.com/home). Para acessar o Zendesk, você deve se registrar e criar uma conta em https://tve.zendesk.com/home. Não há limite para o número de usuários que você pode registrar. Depois de registrado, você pode exibir e compartilhar comentários em qualquer tíquete enviado.

A equipe de autenticação da Adobe Pass está disponível para ajudá-lo com qualquer pergunta ou problema técnico que você possa encontrar durante o processo de integração. Entre em contato conosco em [tve-support@adobe.com](mailto:tve-support@adobe.com).

## Acesso à documentação {#access-documentation}

O **Adobe fornecerá** acesso à nossa documentação pública via [Adobe Experience League](https://experienceleague.adobe.com/en/docs/pass/authentication/home).

A equipe de Autenticação da Adobe Pass fornece documentação abrangente para os recursos e APIs disponíveis na seção [Guia de Integração para Programadores](/help/authentication/integration-guide-programmers/programmer-integration-guide-overview.md). Consulte o índice desta seção para obter links com informações detalhadas sobre cada tópico.

## Acesso à ferramenta de teste {#access-testing-tool}

O **Adobe fornecerá** acesso à nossa ferramenta de exploração de APIs através do site [Adobe Developer](https://developer.adobe.com/adobe-pass/).

## Acesso à ferramenta de gerenciamento de configuração {#access-configuration-management-tool}

O **Adobe fornecerá** acesso a uma ferramenta de autoatendimento para gerenciar sua configuração e seus dados através do [Painel do Adobe Pass TVE](https://experience.adobe.com/pass/authentication).

A equipe de Autenticação do Adobe Pass fornece documentação abrangente para o uso do Painel TVE na seção [Guia do Usuário para o Painel TVE](/help/authentication/user-guide-tve-dashboard/tve-dashboard-overview.md). Consulte o índice desta seção para obter links com informações detalhadas sobre cada tópico.
