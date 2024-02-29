---
title: Avaliação de prevenção de rastreamento no Apple Safari
description: Avaliação de prevenção de rastreamento no Apple Safari
source-git-commit: 579ce868b6ee94e1854bbc51145fc7840268db26
workflow-type: tm+mt
source-wordcount: '1826'
ht-degree: 0%

---

# Avaliação da prevenção de rastreamento - Apple Safari {#tracking-prevention-assessment-apple-safari}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Safari 10 {#safari10}

**Detalhes**

A partir do Safari 10, as configurações padrão de privacidade do navegador farão com que os recursos de Logon único (SSO), Logout único (SLO) e autenticação passiva parem de funcionar. O Logon único (SSO) e a autenticação passiva não funcionarão mesmo na mesma sessão entre várias guias ou janelas do navegador.

Essas alterações afetam e têm impacto nos processos de autenticação da Adobe Pass para as seguintes versões do SDK JavaScript do AccessEnabler: v2 (versões 2.x), v3 (versões 3.x), v4 (versões 4.x).

### Mitigação {#mitigation-safari10}

Para atenuar essas limitações, você pode instruir o usuário a alterar as configurações de privacidade do navegador Safari 10 e usar o &quot;**Sempre permitir** opção &quot; para &quot;**Cookies e dados de site**&quot; na guia Privacidade do navegador, em Preferências, conforme representado na imagem abaixo.

![](assets/always-allow-safari10.png)


## Safari 11 {#safari11}

**Detalhes**

>[!IMPORTANT]
>
>Todos os detalhes acima da seção Safari 10 ainda se aplicam ao Safari 11.

A partir do Safari 11, o navegador apresenta [Prevenção de rastreamento inteligente](https://webkit.org/blog/7675/intelligent-tracking-prevention/)(ITP), uma tecnologia que usa heurística para impedir o rastreamento entre sites. Essa heurística afeta a maneira como os cookies de terceiros são armazenados e repetidos em chamadas de rede, o que significa que, dependendo da ativação do mecanismo ITP, o navegador Safari bloqueará os cookies de terceiros na comunicação entre o cliente e o modelo do servidor.

O serviço de autenticação da Adobe Pass usa e depende de cookies como parte do processo de autenticação **para funcionar**. Em situações em que o processo de autenticação ocorre automaticamente (por exemplo, Temp Pass) ou em implementações que usam iFrames ou funcionalidade &quot;sem atualização&quot;, os cookies Adobe são considerados cookies de terceiros e bloqueados por padrão. Para quaisquer outros casos, o Safari usa um algoritmo de aprendizado de máquina que pode sinalizar todos os cookies do serviço de autenticação de aprovação do Adobe como cookies de rastreamento, sendo, portanto, assunto do bloqueio da ITP.

Concluindo, um usuário do navegador Safari 11 pode não conseguir se autenticar em um site habilitado para Autenticação do Adobe Pass após a ativação do mecanismo de Prevenção de Rastreamento Inteligente (ITP), especialmente quando estiver usando vários sites habilitados para Autenticação do Adobe Pass. Portanto, a experiência de autenticação do usuário pode ser inesperada e indefinida, variando de incapacidade de fazer logon a uma duração de autenticação menor do que a esperada.

Essas alterações afetam e têm impacto nos processos de autenticação da Adobe Pass para as seguintes versões do SDK JavaScript do AccessEnabler: v2 (versões 2.x), v3 (versões 3.x).

### Mitigação {#mitigation-safari11}

Tanto para o AccessEnabler JavaScript SDK v3 (versões 3.x) quanto para o AccessEnabler JavaScript SDK v4 (versões 4.x), a biblioteca contém um mecanismo capaz de identificar as situações em que a autenticação do usuário foi bloqueada devido à falta de cookies necessários. Nessas situações, a biblioteca aciona um retorno de chamada de erro específico [N130](/help/authentication/error-reporting.md#advanced-error-codes-reference), que é repassado para o site habilitado para Autenticação do Adobe Pass para ser usado como um sinal para instruir o usuário a tomar ações que possam atenuar o problema. Para beneficiar deste mecanismo, o sítio Web deve aplicar o [Relatório de erros](/help/authentication/error-reporting.md) especificação.

Para o SDK v2 do JavaScript do AccessEnabler (versões 2.x), a biblioteca não oferece o mecanismo descrito acima, portanto, o site habilitado para autenticação da Adobe Pass não pode ser sinalizado ao instruir o usuário a tomar ações para atenuar o problema.

A lista de ações que podem atenuar as questões acima **aplica-se a todas as três versões** do SDK JavaScript do AccessEnabler.

Quando [N130](/help/authentication/error-reporting.md#advanced-error-codes-reference) Se o retorno de chamada de erro for recebido pelo site do implementador, o usuário deve ser instruído a desativar a Prevenção de rastreamento inteligente (ITP) e ativar cookies de terceiros ao:

* No caso do Mac OS X High Sierra e posterior: desmarcação da opção &quot;**Impedir o rastreamento entre sites** Opção &quot; para &quot;**Rastreamento do site**&quot; na guia Privacidade do navegador, em Preferências, conforme representado na imagem abaixo.

  ![](assets/uncheck-prvnt-cr-st-tr-safari11.png)


* No caso do Mac OS X Sierra e versões anteriores: verifique a &quot;**Sempre permitir** opção &quot; para &quot;**Cookies e dados de site**&quot; na guia Privacidade do navegador, em Preferências, conforme representado na imagem abaixo.

  ![](assets/always-allow-safari11.png)

## Safari 12 {#safari12}

**Detalhes**

>[!IMPORTANT]
>
>Todos os detalhes acima das seções Safari 10 e Safari 11 ainda se aplicam no caso do Safari 12.

Esta seção detalha os problemas de compatibilidade do **SDK JavaScript do AccessEnabler versões 4.x** no Safari 12.

>[!NOTE]
>
>Lembre-se que, no caso das versões 2.x do SDK do JavaScript do AccessEnabler e 3.x do SDK do JavaScript do AccessEnabler, ambas usam cookies de terceiros para os processos de autenticação e, devido à ITP e políticas de cookies de terceiros que começam com o Safari 11, a experiência de autenticação do usuário pode ser inesperada e indefinida, desde a incapacidade de fazer logon até uma duração de autenticação menor que a esperada.


### Funcionalidade certificada do AccessEnabler JavaScript SDK v4 (versões 4.x) no Safari 12 {#certified-functionality-of-accessenabler-javacscript=sdk-v4}

**Autenticação** Os fluxos que usam a interação do usuário sempre funcionarão, mesmo se o navegador do usuário tiver cookies de terceiros desativados, porque a partir da versão 4.0, o SDK JavaScript do AccessEnabler não usará mais cookies de terceiros para os processos de autenticação.

>[!NOTE]
>
>O usuário DEVE interagir com o site para abrir pop-ups de logon e/ou interagir com a página de logon do MVPD.

**Autorização/Comprovação/Metadados do usuário** As operações do são totalmente funcionais, desde que o usuário já esteja autenticado.

### Problemas conhecidos do AccessEnabler JavaScript SDK v4 (versões 4.x) no Safari 12 {#known-issues-of-accessenabler-javascript-sdk-4}

* SSO e SLO

   * Devido à forma como o localStorage é implementado no Safari a partir do Safari 10, o SDK do JS não pode mais compartilhar o estado de logon por meio de um iFrame de domínio comum. Isso significa que o usuário precisa fazer logon em todos os sites que usam o SDK JavaScript do AccessEnabler. Fazer logoff também não exclui tokens de autenticação entre sites, portanto, o usuário precisa fazer logoff de cada site habilitado para autenticação da Adobe Pass.

* Temp Pass (Aprovação temporária)

   * Para passagens temporárias, o SDK JavaScript do AccessEnabler usa um mecanismo de individualização para bloquear um token de autenticação para um dispositivo específico (instância do navegador). Devido aos novos mecanismos no Safari 12 projetados para impedir o rastreamento, a impressão digital que estamos computando e usando no mecanismo de individualização **será o mesmo para todos os usuários com o mesmo endereço IP**. Levamos o IP do cliente em consideração para fins de individualização, mas mesmo assim o impacto é nos usuários que compartilham o mesmo endereço IP público. Para esses usuários, calcularemos a mesma ID de individualização e a passagem temporária será vinculada a ela. Isso significa que, uma vez que esse usuário use um passe temporário, ninguém mais terá acesso a ele \! Isso afeta especialmente usuários corporativos, instituições de ensino ou qualquer outra organização que tenha vários usuários usando NAT ou um proxy comum para acessar a Internet.

>[!NOTE]
>
>Esse problema afeta os usuários somente se o implementador usar a Aprovação Temporária como resultado da interação do usuário, caso contrário, a autenticação da Aprovação Temporária está sujeita a **Fluxos automáticos** abaixo.

* Fluxos automáticos

   * Tentativa de fluxos de autenticação em um modo automatizado, sem qualquer interação do usuário, não terá êxito no Safari 12 ao usar o JS SDK 4.0. Observe que o futuro SDK JS 4.1 corrige todos os problemas com fluxos automatizados.

Casos de uso afetados por esse problema:

* Autenticação TempPass (Visualização gratuita) automática - para esses fluxos, o SDK emitirá um erro N130.

* Autenticação Passiva (falha silenciosa) - o usuário é solicitado a selecionar este MVPD e inserir credenciais

### Mitigação {#mitigation-safari12}

**SSO e SLO**

Não há mitigação conhecida disponível ou possível no momento da redação desses artigos. A Apple introduziu uma &quot;API de acesso de armazenamento&quot; no Safari 12 (`https://webkit.org/blog/8124/introducing-storage-access-api`), mas a implementação atual não se aplica ao localStorage, mas somente aos cookies. Além disso, a API exige interação do usuário para ser usada. Depois de usá-la, o usuário também receberá uma caixa de diálogo de permissão semelhante à mostrada abaixo.

![](assets/permission-dialog-apple.png)


Neste ponto, esses requisitos/prompts do Safari não se alinham aos nossos requisitos de UX e não temos um comportamento consistente como em outros navegadores, em que o SSO &quot;apenas funciona&quot; depois de salvar um token em um domínio comum localStorage.

**Temp Pass (Aprovação temporária)**

Para atenuar os problemas de individualização e ter uma interação com o usuário, recomendamos que você use **[Temp Pass Promocional](/help/authentication/promotional-temp-pass.md)** De forma interativa e forneça pelo menos uma informação adicional sobre o usuário (por exemplo, endereço de email).

## Safari 13 {#safari13}

**Detalhes**

>[!IMPORTANT]
>
>Todos os detalhes acima da seção Safari 10 até a seção Safari 12 ainda se aplicam no caso do Safari 13.


A partir do Safari 13, o navegador introduz novas alterações no [Prevenção de rastreamento inteligente](https://webkit.org/blog/7675/intelligent-tracking-prevention/) (ITP), tornando a heurística por trás do mecanismo mais rígida no processo de sinalizar cookies de terceiros como cookies de rastreamento, a fim de evitar o rastreamento entre sites.

Conforme descrito nas seções anteriores, o serviço de Autenticação da Adobe Pass usa e depende de cookies de terceiros como parte dos processos de autenticação quando os implementadores usam o AccessEnabler JavaScript SDK v2 (versões 2.x) e o AccessEnabler JavaScript SDK v3 (versões 3.x). Em comparação com versões anteriores do navegador Safari, quando a ITP estava entrando após gastar um tempo para &quot;saber&quot; sobre a interação entre o usuário e as partes envolvidas (sites do programador e Adobe), o navegador Safari 13 está bloqueando desde o início os cookies de terceiros que são considerados cookies de rastreamento na comunicação entre o cliente e o modelo do servidor.

Em conclusão, um usuário do navegador Safari 13 provavelmente não poderá iniciar novas autenticações em um site habilitado para Autenticação do Adobe Pass que esteja usando uma versão mais antiga do SDK JavaScript do AccessEnabler, v2 (versões 2.x) ou v3 (versões 3.x). Isso ocorre porque todos os cookies necessários do serviço de autenticação Primetime do Adobe estão bloqueados pelo ITP, impossibilitando o serviço de atender à solicitação de autenticação.

A biblioteca do SDK v4 do JavaScript do AccessEnabler (versões 4.x) não usa cookies de terceiros para o processo de autenticação, portanto, suas operações não são afetadas de forma alguma pelas alterações do Safari 13.

### Mitigação {#mitigation-safari13}

Em primeiro lugar, recomendamos fortemente **Migração para o SDK JavaScript do AccessEnabler versões 4.x** para ter um comportamento estável e previsível no navegador Safari.

Em segundo lugar, para o SDK v3 do JavaScript do AccessEnabler (versões 3.x), a biblioteca contém um mecanismo capaz de identificar as situações em que a autenticação dos usuários foi bloqueada devido à falta de cookies necessários. Nessas situações, a biblioteca aciona um retorno de chamada de erro específico ([N130](/help/authentication/error-reporting.md#advanced-error-codes-reference)) que é repassado para o site habilitado para autenticação da Adobe Pass para ser usado como um sinal para instruir o usuário a tomar ações que possam mitigar o problema. Para beneficiar deste mecanismo, o sítio Web deve aplicar o [Relatório de erros](/help/authentication/error-reporting.md) especificação.

Para o SDK v2 do JavaScript do AccessEnabler (versões 2.x), a biblioteca não oferece o mecanismo descrito acima, portanto, o site habilitado para autenticação da Adobe Pass não pode ser sinalizado ao instruir o usuário a tomar ações para atenuar o problema.

Quando [N130](/help/authentication/error-reporting.md#advanced-error-codes-reference) Se o retorno de chamada de erro for recebido pelo site do implementador, o usuário deve ser instruído a desativar a Prevenção de rastreamento inteligente (ITP) e ativar cookies de terceiros ao:

* No caso do Mac OS X High Sierra e posterior: desmarcação da opção &quot;**Impedir o rastreamento entre sites** Opção &quot; para &quot;**Rastreamento do site**&quot; na guia Privacidade do navegador, em Preferências, conforme representado na imagem abaixo.

  ![](assets/prvnt-cross-site-tr-safari13.png)

* No caso do Mac OS X Sierra e anterior: verificando</span>he &quot;**Sempre permitir** opção &quot; para &quot;**Cookies e dados de site**&quot; na guia Privacidade do navegador, em Preferências, conforme representado na imagem abaixo.

  ![](assets/always-allow-safari13.png)
