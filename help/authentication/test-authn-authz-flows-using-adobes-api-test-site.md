---
title: Como testar fluxos de autenticação e autorização usando o site de teste da API do Adobe
description: Como testar fluxos de autenticação e autorização usando o site de teste da API do Adobe
exl-id: 04af4aed-35e4-44cb-98ce-7643165a8869
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 0%

---

# Como testar fluxos de autenticação e autorização usando o site de teste da API do Adobe {#How-to-test-auth-flows}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

Para testar os fluxos de AuthN e AuthZ, preparamos um **Site de teste da API** que está à sua disposição. Nossa equipe de suporte terá o prazer de fornecer as credenciais a você. Você pode entrar em contato conosco em **support@tve.zendesk.com**.


## Parte I {#part-I}

Para testar o ambiente VERSÃO, pule diretamente para a Parte II.  Para testar no ambiente de pré-qualificação, consulte [Configuração do ambiente e teste na pré-qualificação](/help/authentication/setting-up-your-environment-and-testing-in-prequal.md).

## Parte II

Após concluir a parte I, execute as seguintes etapas:


1. Abrir página da Web: [Teste de API de preparo](https://sp.auth-staging.adobe.com/apitest/api.html).
1. Carregue o ativador de acesso do seguinte URL:
   * [Acessar o javascript do ativador para preparo](https://entitlement.auth-staging.adobe.com/entitlement/js/AccessEnabler.js).
   * OU
   * [Acessar o javascript do ativador para produção](https://entitlement.auth.adobe.com/entitlement/js/AccessEnabler.js).
   * EM SEGUIDA, clique no ícone &quot;**Carregar ativador de acesso** botão &quot;.
1. Agora defina o valor da id do solicitante como &quot;**requestorID**&quot; e clique no botão &quot;setRequestor&quot;.
1. Depois disso, pressione o botão &quot;getAuthentication&quot; e aguarde até que o seletor de exibição seja exibido.
1. Selecione o &quot;**MVPD**&quot; no seletor.
1. Insira suas credenciais no campo &quot;**MVPD**&quot; página de logon.
1. Depois de ser redirecionado de volta, refaça as etapas de 1 a 3
1. Depois de refazer a etapa 3 no &quot;setAuthenticationStatus&quot;, você deve ver o valor &quot;1&quot;. Se a autenticação não funcionar, a caixa de diálogo MVPD será exibida.
1. Para testar a autorização, no campo de entrada do recurso, digite &quot;**requestorID**&quot; e clique no botão &quot;getAuthorization&quot;.
1. Como resultado, na caixa de texto &quot;setToken&quot;-\>&quot;resource id&quot;, o recurso será exibido e, na caixa de texto &quot;setToken&quot;-\>&quot;token&quot;, shortAuthorizationToken será exibido, significando que authZ foi bem-sucedido.
1. Agora você pode clicar no botão &quot;logout&quot; para excluir os tokens.
