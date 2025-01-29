---
title: Avaliação de prevenção de rastreamento no Apple Safari
description: Avaliação de prevenção de rastreamento no Apple Safari
exl-id: a3362020-92ff-4232-b923-e462868730d5
source-git-commit: c1f891fabd47954dc6cf76a575c3376ed0f5cd3d
workflow-type: tm+mt
source-wordcount: '1849'
ht-degree: 0%

---

# Avaliação de prevenção de rastreamento (herdado) - Apple Safari {#tracking-prevention-assessment-apple-safari}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

## Safari 10 {#safari10}

**Detalhes**

A partir do Safari 10, as configurações padrão de privacidade do navegador farão com que os recursos de Logon único (SSO), Logout único (SLO) e autenticação passiva parem de funcionar. O Single Sign-On (SSO) e a autenticação passiva não funcionarão mesmo em
a mesma sessão entre várias guias ou janelas do navegador.

Essas alterações afetam e estão afetando os processos de autenticação da Adobe Pass
para as seguintes versões do AccessEnabler JavaScript SDK: v2 (versões 2.x), v3 (versões 3.x), v4 (versões 4.x).

### Mitigação {#mitigation-safari10}

Para atenuar essas limitações, você pode instruir o usuário a alterar as configurações de privacidade do navegador Safari 10 e usar a opção &quot;**Sempre permitir**&quot; para a entrada &quot;**Cookies e dados do site**&quot; na guia Privacidade do navegador, em Preferências, conforme mostrado na imagem abaixo.

![](../../../assets/always-allow-safari10.png)


## Safari 11 {#safari11}

**Detalhes**

>[!IMPORTANT]
>
>Todos os detalhes acima da seção Safari 10 ainda se aplicam ao Safari 11.

A partir do Safari 11, o navegador introduz o [mecanismo de Prevenção de Rastreamento Inteligente](https://webkit.org/blog/7675/intelligent-tracking-prevention/)(ITP), uma tecnologia que usa heurística para impedir o rastreamento entre sites. Essa heurística afeta a maneira como os cookies de terceiros são armazenados e repetidos em chamadas de rede, o que significa que, dependendo da ativação do mecanismo ITP, o navegador Safari bloqueará os cookies de terceiros na comunicação entre o cliente e o modelo do servidor.

O serviço de Autenticação do Adobe Pass usa e depende de cookies como parte do processo de autenticação **para funcionar**. Em situações em que o processo de autenticação ocorre automaticamente (por exemplo, Temp Pass) ou em implementações que usam iFrames ou funcionalidade &quot;sem atualização&quot;, os cookies Adobe são considerados cookies de terceiros e bloqueados por padrão. Para quaisquer outros casos, o Safari usa um algoritmo de aprendizado de máquina que pode sinalizar todos os cookies do serviço de autenticação de aprovação do Adobe como cookies de rastreamento, sendo, portanto, assunto do bloqueio da ITP.

Concluindo, um usuário do navegador Safari 11 pode não conseguir se autenticar em um site habilitado para Autenticação do Adobe Pass após a ativação do mecanismo de Prevenção de Rastreamento Inteligente (ITP), especialmente quando estiver usando vários sites habilitados para Autenticação do Adobe Pass. Portanto, a experiência de autenticação do usuário pode ser inesperada e indefinida, variando de incapacidade de fazer logon a uma duração de autenticação menor do que a esperada.

Essas alterações afetam e têm impacto nos processos de autenticação da Adobe Pass para as seguintes versões do AccessEnabler JavaScript SDK: v2 (versões 2.x), v3 (versões 3.x).

### Mitigação {#mitigation-safari11}

Tanto para o AccessEnabler JavaScript SDK v3 (versões 3.x) quanto para o AccessEnabler JavaScript SDK v4 (versões 4.x), a biblioteca contém um mecanismo capaz de identificar as situações em que a autenticação do usuário foi bloqueada devido à falta de cookies necessários. Nessas situações, a biblioteca aciona um retorno de chamada de erro específico [N130](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference), que é repassado para o site habilitado para Autenticação Adobe Pass para ser usado como sinal para instruir o usuário a realizar ações que possam atenuar o problema. Para se beneficiar desse mecanismo, o site deve implementar a especificação [Relatório de Erros](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md).

Para o AccessEnabler JavaScript SDK v2 (versões 2.x), a biblioteca não oferece o mecanismo descrito acima, portanto, o site habilitado para Autenticação da Adobe Pass não pode ser sinalizado quando instruir o usuário a tomar ações para atenuar o problema.

A lista de ações que podem atenuar os problemas acima **aplica-se a todas as três versões** do AccessEnabler JavaScript SDK.

Quando o retorno de chamada de erro [N130](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference) é recebido pelo site do implementador, o usuário deve ser instruído a desabilitar a Prevenção de Rastreamento Inteligente (ITP) e habilitar cookies de terceiros:

* No caso do Mac OS X High Sierra e posterior: ao desmarcar a opção &quot;**Impedir rastreamento entre sites**&quot; para a entrada &quot;**Rastreamento de sites**&quot; na guia Privacidade do navegador em Preferências, conforme mostrado na imagem abaixo.

  ![](../../../assets/uncheck-prvnt-cr-st-tr-safari11.png)


* No caso do Mac OS X Sierra e anteriores: Marcar a opção &quot;**Sempre permitir**&quot; para a entrada &quot;**Cookies e dados do site**&quot; na guia Privacidade do navegador em Preferências, conforme mostrado na imagem abaixo.

  ![](../../../assets/always-allow-safari11.png)

## Safari 12 {#safari12}

**Detalhes**

>[!IMPORTANT]
>
>Todos os detalhes acima das seções Safari 10 e Safari 11 ainda se aplicam no caso do Safari 12.

Esta seção detalha os problemas de compatibilidade do **AccessEnabler JavaScript SDK versões 4.x** no Safari 12.

>[!NOTE]
>
>Lembre-se que, no caso das versões 2.x do AccessEnabler JavaScript SDK e 3.x do AccessEnabler JavaScript SDK, ambas usam cookies de terceiros para os processos de autenticação e, devido à ITP e políticas de cookies de terceiros iniciadas no Safari 11, a experiência de autenticação do usuário pode ser inesperada e indefinida, desde a incapacidade de fazer logon até uma duração de autenticação menor que a esperada.


### Funcionalidade certificada do AccessEnabler JavaScript SDK v4 (versões 4.x) no Safari 12 {#certified-functionality-of-accessenabler-javacscript=sdk-v4}

Os fluxos de **Autenticação** que usam a interação do usuário sempre funcionarão, mesmo que o navegador do usuário tenha cookies de terceiros desabilitados, pois a partir da versão 4.0 o AccessEnabler JavaScript SDK não usa mais cookies de terceiros para os processos de autenticação.

>[!NOTE]
>
>O usuário DEVE interagir com o site para abrir pop-ups de logon e/ou interagir com a página de logon do MVPD.

**As operações de Autorização/Comprovação/Metadados do Usuário** estão totalmente funcionais, desde que o usuário já esteja autenticado.

### Problemas conhecidos do AccessEnabler JavaScript SDK v4 (versões 4.x) no Safari 12 {#known-issues-of-accessenabler-javascript-sdk-4}

* SSO e SLO

   * Devido à forma como o localStorage é implementado no Safari a partir do Safari 10, o JS SDK não pode mais compartilhar o estado de logon por meio de um iFrame de domínio comum. Isso significa que o usuário precisa fazer logon em todos os sites que usam o AccessEnabler JavaScript SDK. Fazer logoff também não exclui tokens de autenticação entre sites, portanto, o usuário precisa fazer logoff de cada site habilitado para autenticação da Adobe Pass.

* Temp Pass (Aprovação temporária)

   * Para passagens temporárias, o AccessEnabler JavaScript SDK usa um mecanismo de individualização para bloquear um token de autenticação para um dispositivo específico (instância do navegador). Devido aos novos mecanismos no Safari 12 criados para impedir o rastreamento, a impressão digital que estamos computando e usando no mecanismo de individualização **será a mesma para todos os usuários que têm o mesmo endereço IP**. Levamos o IP do cliente em consideração para fins de individualização, mas mesmo assim o impacto é nos usuários que compartilham o mesmo endereço IP público. Para esses usuários, calcularemos a mesma ID de individualização e a passagem temporária será vinculada a ela. Isso significa que, uma vez que esse usuário use um passe temporário, ninguém mais terá acesso a ele \! Isso afeta especialmente usuários corporativos, instituições de ensino ou qualquer outra organização que tenha vários usuários usando NAT ou um proxy comum para acessar a Internet.

>[!NOTE]
>
>Esse problema afetará os usuários somente se o implementador usar Temp Pass como resultado da interação do usuário. Caso contrário, a autenticação Temp Pass estará sujeita aos **Fluxos automáticos** abaixo.

* Fluxos automáticos

   * Tentativa de fluxos de autenticação em um modo automatizado, sem qualquer interação do usuário, não terá êxito no Safari 12 ao usar o JS SDK 4.0. Observe que o próximo JS SDK 4.1 corrige todos os problemas com fluxos automatizados.

Casos de uso afetados por esse problema:

* Autenticação TempPass (Visualização gratuita) automática - para esses fluxos, o SDK emitirá um erro N130.

* Autenticação passiva (falha silenciosamente) - o usuário é solicitado a selecionar esta MVPD e inserir credenciais

### Mitigação {#mitigation-safari12}

**SSO e SLO**

Não há mitigação conhecida disponível ou possível no momento da redação desses artigos. A Apple introduziu uma &quot;API de acesso de armazenamento&quot; no Safari 12 (`https://webkit.org/blog/8124/introducing-storage-access-api`), mas a implementação atual não se aplica ao localStorage, mas somente a cookies. Além disso, a API exige interação do usuário para ser usada. Depois de usá-la, o usuário também receberá uma caixa de diálogo de permissão semelhante à mostrada abaixo.

![](../../../assets/permission-dialog-apple.png)


Neste ponto, esses requisitos/prompts do Safari não se alinham aos nossos requisitos de UX e não temos um comportamento consistente como em outros navegadores, em que o SSO &quot;apenas funciona&quot; depois de salvar um token em um domínio comum localStorage.

**Temp Pass**

Para atenuar os problemas de individualização e ter uma interação com o usuário, recomendamos que você use o **[Temp Pass Promocional](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)** de forma interativa e forneça pelo menos uma informação adicional sobre o usuário (por exemplo, endereço de email).

## Safari 13 {#safari13}

**Detalhes**

>[!IMPORTANT]
>
>Todos os detalhes acima da seção Safari 10 até a seção Safari 12 ainda se aplicam no caso do Safari 13.


A partir do Safari 13, o navegador introduz novas alterações na [Prevenção de Rastreamento Inteligente](https://webkit.org/blog/7675/intelligent-tracking-prevention/) (ITP), tornando a heurística por trás do mecanismo mais rígida no processo de sinalização de cookies de terceiros como cookies de rastreamento, a fim de impedir o rastreamento entre sites.

Conforme descrito nas seções anteriores, o serviço de Autenticação da Adobe Pass usa e depende de cookies de terceiros como parte dos processos de autenticação quando os implementadores usam o AccessEnabler JavaScript SDK v2 (versões 2.x) e o AccessEnabler JavaScript SDK v3 (versões 3.x). Em comparação com versões anteriores do navegador Safari, quando a ITP estava entrando após gastar um tempo para &quot;saber&quot; sobre a interação entre o usuário e as partes envolvidas (sites do programador e Adobe), o navegador Safari 13 está bloqueando desde o início os cookies de terceiros que são considerados cookies de rastreamento na comunicação entre o cliente e o modelo do servidor.

Em conclusão, um usuário do navegador Safari 13 provavelmente não poderá iniciar novas autenticações em um site habilitado para Autenticação do Adobe Pass que esteja usando uma versão mais antiga do AccessEnabler JavaScript SDK, v2 (versões 2.x) ou v3 (versões 3.x). Isso ocorre porque todos os cookies necessários do serviço de autenticação Primetime do Adobe estão bloqueados pelo ITP, impossibilitando o serviço de atender à solicitação de autenticação.

A biblioteca do AccessEnabler JavaScript SDK v4 (versões 4.x) não usa cookies de terceiros para o processo de autenticação, portanto, suas operações não são afetadas de forma alguma pelas alterações do Safari 13.

### Mitigação {#mitigation-safari13}

Em primeiro lugar, recomendamos a migração do **para o AccessEnabler JavaScript SDK versões 4.x** para ter um comportamento estável e previsível no navegador Safari.

Em segundo lugar, para o AccessEnabler JavaScript SDK v3 (versões 3.x), a biblioteca contém um mecanismo capaz de identificar as situações em que a autenticação dos usuários foi bloqueada devido à falta de cookies necessários. Nessas situações, a biblioteca aciona um retorno de chamada de erro específico ([N130](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference)), que é repassado ao site habilitado para Autenticação Adobe Pass para ser usado como um sinal para instruir o usuário a realizar ações que possam atenuar o problema. Para se beneficiar desse mecanismo, o site deve implementar a especificação [Relatório de Erros](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md).

Para o AccessEnabler JavaScript SDK v2 (versões 2.x), a biblioteca não oferece o mecanismo descrito acima, portanto, o site habilitado para Autenticação da Adobe Pass não pode ser sinalizado quando instruir o usuário a tomar ações para atenuar o problema.

Quando o retorno de chamada de erro [N130](/help/authentication/integration-guide-programmers/legacy/error-reporting/error-reporting.md#advanced-error-codes-reference) é recebido pelo site do implementador, o usuário deve ser instruído a desabilitar a Prevenção de Rastreamento Inteligente (ITP) e habilitar cookies de terceiros:

* No caso do Mac OS X High Sierra e posterior: ao desmarcar a opção &quot;**Impedir rastreamento entre sites**&quot; para a entrada &quot;**Rastreamento de sites**&quot; na guia Privacidade do navegador em Preferências, conforme mostrado na imagem abaixo.

  ![](../../../assets/prvnt-cross-site-tr-safari13.png)

* No caso do Mac OS X Sierra e anteriores: Marcando a opção &quot;</span>A opção &quot;**Sempre permitir**&quot; para a entrada &quot;**Dados de cookies e sites**&quot; na guia Privacidade do navegador, em Preferências, conforme mostrado na imagem abaixo.

  ![](../../../assets/always-allow-safari13.png)
