---
title: Guia de início rápido do MVPD
description: Guia de início rápido do MVPD
exl-id: 6423cc9a-a45a-4cde-b562-4cb72c98e505
source-git-commit: 2b9a8ce374f7a73cd815e9735d672e5c9ba285cc
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 0%

---

# Guia de início rápido do MVPD {#mvpd-kickstart-guide}

>[!IMPORTANT]
>
> O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Este guia de início rápido é destinado aos Distribuidores de programação de vídeo multicanal (MVPDs) que planejam integrar com a Autenticação de Adobe® Pass.

Este documento descreve as principais etapas iniciais para garantir um início tranquilo e eficiente do processo de integração. Ela tem como objetivo esclarecer expectativas e fornecer orientação sobre como colaboraremos com parceiros para obter integrações bem-sucedidas.

O Adobe fornece uma variedade de recursos para ajudá-lo a se integrar com a Autenticação Adobe Pass. Consulte as menções **&quot;Você fornecerá&quot;** e **&quot;Adobe fornecerá&quot;** de cada seção abaixo.

>[!CAUTION]
>
> Cada vez que um usuário inicia um fluxo de direitos, ele recebe uma ID de usuário única, opaca e exclusiva. Essa ID, originada da MVPD, é usada para identificar o usuário em um aplicativo do Programador.
>
> <br/>
>
> A ID de usuário não deve conter informações pessoais identificáveis (PII) ou dados que possam ser usados isoladamente ou em combinação com outros detalhes para identificar, entrar em contato ou localizar o usuário.

## Configurar processo {#setup-process}

O processo de configuração envolve, entre outras, as seguintes etapas:

![Processo de integração de autenticação do Adobe® Pass](../assets/mvpd-int-lifecycle.png)

*Processo de integração de autenticação do Adobe® Pass*

### Início {#kickoff}

**Você fornecerá** durante a fase inicial:

* **Nome de exibição**

  Esta é uma string exibida nos sites ou aplicativos do Programador ao solicitar que os usuários selecionem seu provedor de TV por Assinatura.

* **URL do Logotipo**

  Trata-se de um arquivo, medindo 112 por 33 pixels, que contém o logotipo exibido nos sites ou aplicativos do programador quando ele solicita que os usuários selecionem seu provedor de TV por assinatura.

* **Tempo de vida (TTL)**

  O TTL é um valor normalmente definido pelo MVPD como parte dos processos de autenticação ou autorização. No entanto, o Adobe pode substituir esses valores TTL e fornecer valores diferentes, dependendo do que é acordado entre o Programador e a MVPD.

* **Conjuntos de credenciais**

  Essas são credenciais usadas para autenticar e autorizar, ou apenas autenticar, o usuário com a MVPD. Normalmente, essas credenciais consistem em um nome de usuário e uma senha, que devem ser fornecidos para ambos os perfis (armazenamento temporário e produção).

### Troca de metadados (SAML) {#metadata-exchange-saml}

**Adobe fornecerá** durante a fase de troca de metadados:

* **Metadados do ambiente de preparo**

  Os metadados de Adobe podem ser recuperados de https://sp.auth-staging.adobe.com/sp/metadata.

* **Metadados do ambiente de produção**

  Os metadados de Adobe podem ser recuperados de https://sp.auth.adobe.com/sp/metadata.

**Você fornecerá** durante a fase de troca de metadados:

* **Metadados de preparo**

  Os metadados do MVPD para o ambiente de preparo.

* **Metadados de produção**

  Os metadados do MVPD para o ambiente de produção.

### Conectividade {#connectivity}

incluir na lista de permissões **Você fornecerá** uma maneira de converter IPs do Adobe, pois a Autenticação Adobe Pass requer firewalls para permitir o tráfego pelas portas 80 e 443 para habilitar o acesso a recursos restritos durante os processos de autenticação e autorização.

**Você fornecerá** uma implantação no perfil de preparo para testar a conectividade.

### Desenvolvimento {#development}

O **Adobe fornecerá** tempo de engenharia para trabalhar em conjunto com a MVPD para garantir que a integração técnica seja estabelecida corretamente. Esse processo envolve o desenvolvimento de um código personalizado adaptado aos requisitos específicos do MVPD.

### Implantação em preparo {#deployment-staging}

O **Adobe fornecerá** uma compilação com as atualizações de código necessárias que serão implantadas primeiro no ambiente de preparo PRÉ-QUAL. Durante esta fase, as alterações de configuração necessárias também serão implementadas para integrar o MVPD com o provedor de serviços `TestDistributors` para fins de teste.

**Você e o Adobe fornecerão** tempo de controle de qualidade (QA) para garantir que a integração seja testada com êxito no ambiente de preparo PRÉ-QUAL. Após essa fase, o MVPD será movido para o ambiente de preparo de VERSÃO para testes adicionais com um programador real.

### Implantação em produção {#deployment-production}

**Você fornecerá** uma implantação no perfil de produção para testar a conectividade.

O **Adobe fornecerá** uma compilação com as atualizações de código necessárias que serão implantadas no ambiente de produção PRE-QUAL.

**Você e o Adobe fornecerão** tempo de controle de qualidade (QA) para garantir que a integração seja testada com êxito usando o perfil de produção. Se tudo estiver OK neste momento, o Adobe poderá mover a integração para o ambiente de produção RELEASE (&quot;live&quot;), que está disponível para todos os usuários.

>[!IMPORTANT]
>
> Quando a integração estiver ativa no ambiente de produção da VERSÃO, é fundamental manter uma experiência ideal do cliente. Para lidar com cenários de inatividade do servidor de maneira eficaz, os MVPDs devem fornecer documentação detalhada do procedimento de escalonamento ao Adobe para o gerenciamento desses problemas.
>
> Por sua vez, o Adobe garante que os MVPDs recebam a versão mais recente do processo de escalonamento da Autenticação Adobe Pass para a resolução simplificada de problemas.

## Acesso a ambientes {#access-environments}

O **Adobe fornecerá** acesso aos ambientes para diferentes estágios do processo de desenvolvimento:

* **Pré-qualificação (PRÉ-QUALIFICAÇÃO)**

  O ambiente PRÉ-QUAL hospeda o próximo candidato a lançamento e serve como plataforma de integração inicial para novos parceiros. Antes de migrar para o ambiente VERSÃO, os parceiros têm tempo para testar sua integração na PRÉ-QUAL.

* **Versão (VERSÃO)**

  O ambiente RELEASE hospeda o build de produção atual (estável).

Para obter mais informações sobre como usar esses ambientes, consulte a documentação [Noções básicas sobre ambientes de Adobe](/help/authentication/notes-technical/environments/understanding-the-adobe-environments.md).

>[!IMPORTANT]
> 
> As alterações de configuração nesses ambientes devem ser solicitadas explicitamente por meio do representante da Adobe, seguindo o processo estabelecido de solicitação de alteração.

## Acesso ao suporte ao cliente {#access-customer-support}

O **Adobe fornecerá** acesso ao nosso sistema de suporte ao cliente via [Zendesk](https://tve.zendesk.com/home). Para acessar o Zendesk, você deve se registrar e criar uma conta em https://tve.zendesk.com/home.

A equipe de Autenticação do Adobe Pass está disponível para responder a qualquer pergunta ou problema técnico que possamos encontrar durante o processo de integração. Entre em contato conosco em [tve-support@adobe.com](mailto:tve-support@adobe.com).

## Acesso à documentação {#access-documentation}

O **Adobe fornecerá** acesso à nossa documentação pública via [Adobe Experience League](https://experienceleague.adobe.com/pt-br/docs/pass/authentication/home).

A equipe de Autenticação da Adobe Pass fornece documentação abrangente para os recursos e fluxos de trabalho disponíveis na seção [Guia de Integração para MVPDs](/help/authentication/integration-guide-mvpds/mvpd-integration-guide-overview.md). Consulte o índice desta seção para obter links com informações detalhadas sobre cada tópico.

## Acesso à ferramenta de teste {#access-testing-tool}

O **Adobe fornecerá** acesso à nossa ferramenta de exploração de APIs através do site [Adobe Developer](https://developer.adobe.com/adobe-pass/).
