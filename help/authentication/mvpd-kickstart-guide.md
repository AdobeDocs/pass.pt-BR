---
title: Plano de integração direta do MVPD
description: Plano de integração direta do MVPD
exl-id: 6423cc9a-a45a-4cde-b562-4cb72c98e505
source-git-commit: 8896fa2242664d09ddd871af8f72d8858d1f0d50
workflow-type: tm+mt
source-wordcount: '1071'
ht-degree: 0%

---

# Guia de início rápido do MVPD: plano de integração direta do MVPD {#mvpd-dir-int-plan}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#mvpd-kickstart-intro}

Bem-vindo à Autenticação Adobe Pass para TV em todos os lugares.  Esperamos trabalhar com você.

>[!NOTE]
>
>Este é o Guia de Kickstart para Distribuidores de Programação de Vídeo Multicanal (MVPDs). Se você for um Programador (provedor de conteúdo), consulte o [Guia de Kickstart de Programadores](/help/authentication/programmer-kickstart-guide.md).

O suporte está disponível em todos os momentos através do sistema de tíquetes de Autenticação da Adobe Pass no Zendesk. Também é aqui que você pode encontrar amostras, documentação e tutoriais em vídeo para nossos processos. Para usar o [Zendesk](https://adobeprimetime.zendesk.com/), você terá que se registrar e criar uma conta em https://tve.zendesk.com/home. Não há limite para a quantidade de usuários que você pode registrar e que podem ver ou publicar comentários em um tíquete arquivado. Todas as perguntas de suporte devem ser enviadas para: tve-support em adobe.com

**Contatos da equipe**:

**Suporte** - Para todas as perguntas, incidentes ou solicitações de recursos **tve-support@adobe.com**.

## 1. Reuniões de lançamento {#kickoff-meetings}

O âmbito destas reuniões é o início de discussões técnicas entre o Adobe e o MVPD. Nesta fase do processo, a documentação precisa ser compartilhada de ambos os lados. Como acompanhamento, o Adobe precisa abrir um tíquete em nosso sistema de emissão de tíquetes (https://tve.zendesk.com/) para rastrear o status da integração.

## 2. Características {#features}

O Adobe realizará uma chamada semanal de status para discutir e acompanhar a programação geral, as etapas, o cronograma e os detalhes de implementação da integração. Nessa fase, a Adobe faz uma revisão das especificações do MVPD. O resultado deve ser uma página de especificação que detalhe todos os recursos necessários para o MVPD. O MVPD enviará ao Adobe um documento de especificação, detalhando a implementação de autenticação/autorização do MVPD.

Itens a esclarecer, consulte [Recursos de Integração do MVPD](/help/authentication/mvpd-integr-features.md).

Há várias configurações que precisarão ser descritas detalhadamente neste ponto:

* **URL do logotipo do MVPD** - Este é um arquivo com as seguintes dimensões: 112 x 33 pixels. O logotipo é exibido pelos Programadores em seus sites quando o usuário clica no botão &quot;Sign in&quot; para selecionar seu provedor de TV paga.
* **Valores de TTL (tempo de vida útil)** - O TTL geralmente é definido pelo MVPD durante o processo de autenticação/autorização. No entanto, o Adobe pode substituir esses valores TTL e fornecer valores diferentes, dependendo do que é acordado entre o Programador e o MVPD.
* **Nome para exibição** - É exibido pelos Programadores em seus sites quando o usuário clica no botão &quot;Entrar&quot; para selecionar seu provedor de TV por Assinatura.
* **Credenciais de teste** - Ambos os perfis (preparo e produção) devem ter uma lista de credenciais de teste.

>[!IMPORTANT]
>
>Cada vez que um usuário iniciar um fluxo de direitos, ele será associado a uma única ID de usuário opaca e exclusiva.  A ID de usuário é usada para identificar o usuário de um aplicativo do Programador, mas é originada do MVPD.

>[!CAUTION]
>
>Um aviso importante para cada MVPD: a ID de usuário NÃO deve conter nenhuma PII (Informações de identificação pessoal), informação que possa ser usada sozinha ou com outras informações para identificar, contactar ou localizar o usuário.

## 2. Intercâmbio de metadados {#metadata-ex}

Os dois lados precisam trocar os metadados de todos os ambientes envolvidos (produção, preparo etc.).

* **Adobe**
   * Para o ambiente de preparo, os metadados da controladora de armazenamento Adobe podem ser recuperados de: [Metadados da controladora de armazenamento de preparo de autenticação](https://sp.auth-staging.adobe.com/sp/metadata)
   * Para o ambiente de produção, os metadados de Adobe de SP podem ser recuperados de: [Metadados de SP de produção de autenticação](https://sp.auth.adobe.com/sp/metadata)

* **MVPD**
   * Para adicionar metadados (preparo/produção).

## 4. Lista de permissões de IP {#allow-ip-list}

Os seguintes IPs devem estar na lista de permissões no firewall do MVPD. Entre em contato com o Adobe para obter a lista de IPs.

* A Autenticação Adobe Pass exige que os firewalls sejam abertos nas portas 80 e 443, para permitir acesso a recursos restritos.

* O MVPD precisa adicionar uma lista de endereços IP para servidores de autenticação e autorização (se esse for o caso).

## 5. Desenvolvimento {#deve}

A duração da fase de desenvolvimento será determinada após a revisão das especificações e levando em consideração os itens que os dois lados aprovam. O Adobe precisa gravar o código personalizado da parte de autorização.

>[!NOTE]
>
>Observe que a autorização é executada por recurso. A transação de autorização normalmente é executada com uma sequência de ID, transmitida do site do Programador, representando o canal para o qual o usuário está solicitando autorização. Essa ID de recurso é estabelecida entre o Programador e o MVPD e pode ser tão granular quanto necessário.

## 6. Ambientes de Adobe {#adobe-env}

O Adobe fornece ambientes diferentes para diferentes estágios do processo de desenvolvimento:

* **Pré-qualificação** (PRÉ-QUAL): o ambiente PRÉ-QUAL contém o próximo candidato a lançamento. O Adobe integra inicialmente novos parceiros neste ambiente, antes de atualizar a integração para o ambiente Versão. Os parceiros têm duas semanas para testar no ambiente PRÉ-QUAL e devem solicitar explicitamente quaisquer alterações na configuração PRÉ-QUAL (entre em contato com o representante da Adobe para obter detalhes sobre o processo de solicitação de alteração). Correções de erros acionam novas implantações neste ambiente.
* **Versão** (VERSÃO): a compilação de produção atual de Adobe foi implantada em um ambiente ativo aqui.

Para obter mais informações sobre como usar ambientes Adobe, consulte [Entendendo os ambientes Adobe](/help/authentication/understanding-the-adobe-environments.md)

## 7. Implantação em preparo {#stag-env}

Com base nos metadados recebidos do MVPD, o Adobe criará e configurará um novo MVPD no sistema de autenticação da Adobe Pass. Ele será implantado no ambiente de preparo Adobe prequal e será configurado com nosso Programador de teste (TestDistributors).

O MVPD precisa fazer a mesma implantação em seu ambiente de controle de qualidade/preparo/teste.

## 8. Testar e solucionar problemas {#tes-troubleshoot}

Nesta fase, o Adobe e o MVPD testam e solucionam problemas da integração. Para ajudar a testar a integração, a equipe de Autenticação do Adobe Pass pode usar o site de teste da API Adobe. Para saber mais sobre como usar o site de teste da API Adobe, consulte [Fluxos de autenticação e autorização de teste usando o site de teste da API Adobe](/help/authentication/test-authn-authz-flows-using-adobes-api-test-site.md).

Quando os testes e a solução de problemas forem concluídos com êxito, a integração será ativada no ambiente de preparo da versão do Adobe. Neste ponto, o Adobe pode integrar o MVPD com um programador real.

## 9. Implantação de produção {#prod-dep}

* O MVPD precisa implantar primeiro no perfil de produção, para testar a conectividade.

* O Adobe é implantado em produção pré-existente.

* Todos os participantes agora podem testar no perfil de produção.

* Se tudo estiver OK nesse ponto, o Adobe poderá mover a integração para o ambiente de produção de lançamento (&quot;live&quot;), que está disponível para todos os usuários.

## 10. Procedimentos de escalonamento {#esc-proc}

Quando a integração estiver em produção, é essencial fornecer a melhor experiência ao cliente. Para garantir a melhor resposta no caso de um problema de servidor inativo, os MVPDs devem fornecer documentação de procedimento de escalonamento para problemas trazidos à atenção do Adobe.

Por sua vez, o Adobe fornece MVPDs com o processo de encaminhamento de Autenticação Adobe Pass mais recente.


<!--- [!RELATEDINFORMATION]
>
>* [Programmer Kickstart Guide](/help/authentication/programmer-kickstart-guide.md)
>* [MVPD Integration Guide](/help/authentication/mvpd-integr-features.md)
-->
