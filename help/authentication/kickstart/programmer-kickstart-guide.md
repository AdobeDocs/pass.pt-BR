---
title: Guia de início rápido do programador
description: Guia de início rápido do programador
exl-id: 0aecdb81-9b97-4475-b0b0-654d916b2374
source-git-commit: 9dc25b66d12b05a8afe16d1a866707880b5d6a51
workflow-type: tm+mt
source-wordcount: '969'
ht-degree: 0%

---

# Guia de início rápido do programador {#programmer-kickstart-guide}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Introdução {#prog-kickstart-guide-intro}

Bem-vindo à Autenticação Adobe Pass para TV em todos os lugares. Esperamos trabalhar com você.

>[!NOTE]
>
>Este é o Guia de início rápido para programadores (provedores de conteúdo). Se você estiver com um Distribuidor de programação de vídeo multicanal (MVPD), consulte o [guia de início rápido do MVPD](/help/authentication/kickstart/mvpd-kickstart-guide.md).


Contatos de autenticação da Adobe Pass:

* Suporte - para todas as perguntas, incidentes ou solicitações de recursos, `tve-support@adobe.com`
* Um contato de ativação será atribuído ao seu projeto até o início do projeto.

As informações a seguir descrevem alguns primeiros passos importantes para um início sólido e eficiente. O objetivo é fornecer uma explicação e uma expectativa sobre como trabalharemos com parceiros para realizar integrações. Observe as seções &quot;você fornecerá&quot; / &quot;Adobe fornecerá&quot; abaixo para cada item. Eles são listados por meio de uma lista de verificação ou guia, conforme trabalhamos no projeto.

Este documento supõe que os programadores estejam inscritos para trabalhar com um parceiro escolhido da MVPD.

## Programação de lançamento {#release-schedule}

O ciclo de desenvolvimento do Adobe sprint está planejado para que você possa ver quando nossas versões estão programadas e como cada versão é promovida por meio de nosso sistema de desenvolvimento.

Após a conclusão de cada sprint, ele fica disponível para QE ou para ver novas implementações em nosso servidor UAT.

Aproximadamente a cada seis semanas, será feita uma versão para o servidor de pré-qualificação do Adobe. (Este é o servidor no qual mantemos nossa próxima versão proposta enquanto realizamos o QE final.) Essas builds incluirão todo o trabalho concluído no sprints desde a última queda. Uma janela de QE de duas semanas está disponível no momento para que os parceiros testem esta versão.

Supondo que nenhum problema crítico tenha ocorrido durante as duas semanas anteriores de teste, a versão será promovida para produção em tempo real. Isso significa que a integração estará disponível no ambiente de lançamento do Adobe, mas os parceiros escolhem quando tornarão o lançamento público.

<!--For the latest release schedule information, see the Release Calendar.-->

## Documentação de suporte {#supp-doc}

O Adobe fornecerá:

* Guia de Implantação: **`https://tve.zendesk.com/entries/498741-tve-deployment-guide`**
* Acesso ao nosso sistema de suporte ao cliente Zendesk. Também é aqui que você pode encontrar amostras, informações e tutoriais em vídeo sobre alguns dos processos do. Para acessar este documento no Zendesk, juntamente com outros documentos postados lá, você terá que se registrar e criar uma conta em `https://tve.zendesk.com/home`. Não há limite para a quantidade de usuários que você pode registrar.  Você pode ver e compartilhar comentários em qualquer ticket arquivado. Todas as perguntas de suporte devem ser endereçadas a `tve-support@adobe.com`.
* Biblioteca de Verificador de Token de Mídia: `https://tve.zendesk.com/entries/471323-media-token-validator-library`.

## Configuração de ambiente de teste {#test-env-setup}

O Adobe irá primeiro configurá-lo com o site de teste de Adobe, onde o Adobe atua como um MVPD para fins de teste. Sua equipe pode então configurar um site de teste que chame a API Adobe. Use o seletor de MVPD padrão e selecione &quot;Adobe&quot; como o idP.

Você fornecerá:

1. ID do solicitante. Esta é uma sequência de caracteres que identificará exclusivamente a marca do site ou do aplicativo que está fazendo solicitações de autenticação da Adobe Pass. A cadeia de caracteres em si é arbitrária, mas precisa ser acordada entre o Adobe e o Programador
1. Informações do canal. Este é um conjunto de strings que identifica os canais de conteúdo que estão sendo solicitados pela ID do solicitante. Em muitos casos, o canal e a ID do solicitante são iguais. No entanto, você pode ter vários canais de conteúdo que podem ser solicitados pela mesma ID. As cadeias de caracteres de nome de canal devem corresponder aos canais de TV a cabo. Alguns MVPDs validarão esse valor no protocolo AuthN e/ou AuthZ.
1. Nomes de domínio (a serem permitidos para essa ID do solicitante). Essa será uma lista de nomes de domínio reais que serão listados por Adobe para aceitar a ID do solicitante. Isso garante que somente seus domínios aprovados tenham acesso à Autenticação Adobe Pass com seus metadados. OBSERVAÇÃO: nomes de domínio válidos para produção podem ser diferentes para teste/preparo e ambos devem ser fornecidos e identificados.

o Adobe configurará a conta e o Adobe fornecerá:

* Logon e senha para acessar o site de teste

## Configuração com o MVPD {#setup-mvpd}

Esta seção descreve o que é necessário ao migrar do site de teste do Adobe para trabalhar com uma MVPD.

Você fornecerá (via MVPD):

* **Dois conjuntos de credenciais**:
   * AuthN + AuthZ : logon/senha para um usuário autenticado e autorizado
   * AuthN + Non-AuthZ : logon/senha para um usuário autenticado, mas não autorizado
* **Identificação do recurso**. Este é um identificador de conteúdo específico que será validado com um MVPD sobre o protocolo AuthZ. Isso pode ser no nível de canal, programa, episódio ou ativo; deve ser acordado com sua MVPD.

A Autenticação do Adobe Pass é compatível com um esquema de metadados baseado em MRSS, o que significa que as IDs de recurso podem ser tão específicas quanto necessário e podem incluir identificadores que podem ser exclusivos de um MVPD específico.

**NOVA integração com o MVPD**: é importante lembrar que o MVPD escolhido é parte integrante da conclusão de qualquer integração. O Adobe precisa gravar o código para cada MVPD de acordo com as especificações. Até que essas etapas sejam concluídas, você não poderá selecionar esse MVPD na caixa de diálogo nem concluir o teste do produto. O Adobe precisa agendar esse trabalho com antecedência para se adequar ao próximo sprint disponível. (Para obter informações sobre a programação atual, consulte o Calendário de lançamento.)

**Integrações existentes com o MVPD**: se o MVPD escolhido já estiver configurado com o Adobe, as etapas de conectividade deverão ser muito mais simples (mais rápidas) e, muitas vezes, a conectividade poderá ser alcançada por meio de alterações de configuração.

>[!NOTE]
>
>A MVPD AINDA terá que ativar o Programador e aprovar quaisquer negócios relevantes.

**QE com MVPDs**: todas as integrações envolverão QE conjunto e, como o usuário final é, em última análise, um cliente da MVPD, muitos definiram ciclos de teste antes de enviar para o &quot;ativo&quot;. Como isso envolve a programação de recursos do MVPD, essa é uma área potencial para atraso.

<!--
>[RELATEDINFORMATION]
>[MVPD Kickstart Guide](help\authentication\mvpd-kickstart-guide.md)
-->
