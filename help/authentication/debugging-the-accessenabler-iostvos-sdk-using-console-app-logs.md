---
title: Depuração do SDK iOS/tvOS do AccessEnabler usando logs de aplicativo do console
description: Depuração do SDK iOS/tvOS do AccessEnabler usando logs de aplicativo do console
exl-id: 0dad325e-db15-4ea0-a87a-75409eaf8d46
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 0%

---

# Depuração do SDK iOS/tvOS do AccessEnabler usando logs de aplicativo do console {#debugging-the-accessenabler-iostvos-sdk-using-console-app-logs}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.


## Visão geral

O escopo deste documento é capturar e apresentar a evolução do mecanismo de registro do SDK do iOS/tvOS do AccessEnabler junto com alguns detalhes úteis para depurar a estrutura do AccessEnabler usando os logs de aplicativo do console.

## Estado do Mecanismo de Log

A finalidade do mecanismo de registro AccessEnabler iOS/tvOS é emitir mensagens úteis para solucionar possíveis problemas que um aplicativo usando a estrutura AccessEnabler poderia encontrar devido a ele.

### AccessEnabler iOS/tvOS 3.5.0 e superior

A partir da versão AccessEnabler iOS/tvOS 3.5.0, o mecanismo de registro apresenta os seguintes aprimoramentos à medida que as alterações são feitas:

* A estrutura do AccessEnabler usa a estrutura recomendada pela Apple [OSLog](https://developer.apple.com/documentation/os/oslog) execução.

* A estrutura do AccessEnabler apresenta a capacidade de filtrar logs de aplicativo do Console com base no Subsistema: **com.adobe.pass.AccessEnabler**. Todas as mensagens emitidas pelo SDK fazem parte de com.adobe.pass.AccessEnabler.

* A estrutura do AccessEnabler apresenta a capacidade de filtrar logs de aplicativo do Console com base em Qualquer (prefixo): **[AccessEnabler]**. Todas as mensagens emitidas pelo SDK recebem o prefixo [AccessEnabler].

* A estrutura do AccessEnabler apresenta a capacidade de filtrar logs de aplicativo do Console com base na Categoria: **depurar**, **erro** em conjunto com qualquer um dos dois critérios acima: Subsistema ou Qualquer um (prefixo).

## Depuração usando logs de aplicativo do Console

Dependendo dos problemas investigados, talvez você queira incluir ou excluir as mensagens de registro emitidas pela estrutura do AccessEnabler, portanto, encontre abaixo alguns detalhes úteis que podem ajudá-lo durante as investigações e ao usar os logs de aplicativo do Console.


### AccessEnabler iOS/tvOS 3.5.0 e superior

#### Incluindo {#including}

Primeiro de tudo para poder ver qualquer uma das mensagens de registro emitidas pela estrutura do AccessEnabler que você **deve** selecione as opções &quot;Incluir mensagens informativas&quot; e &quot;Incluir mensagens de depuração&quot; na seção Ação do aplicativo do console, conforme apresentado na imagem abaixo.

![](assets/include-info-debug-msg.png)


Para poder depurar a funcionalidade do SDK do AccessEnabler iOS/tvOS e **consulte** Os logs da estrutura do AccessEnabler podem ser:

* Pesquise no aplicativo Console usando **Subsistema** opção que É igual ao valor com.adobe.pass.AccessEnabler como na imagem abaixo.

![](assets/subsys-console-app.png)

* Pesquise no aplicativo Console usando **Qualquer** opção que Contém a variável
  [AccessEnabler] como na imagem abaixo.

![](assets/any-optn-console-app.png)

Juntamente com os dois critérios acima, você também pode usar o **Categoria** em conjunto com **Subsistema** ou **Qualquer um (prefixo)** para pesquisar explicitamente por **depurar** ou **erro** mensagens de nível emitido pelo SDK iOS/tvOS do AccessEnabler.

#### Excluindo

Para poder depurar melhor a funcionalidade de outros componentes e **excluir** Os logs da estrutura do AccessEnabler podem ser:

* Pesquise no aplicativo Console usando **Subsistema** que não é igual ao valor com.adobe.pass.AccessEnabler.
* Pesquise no aplicativo Console usando **Qualquer** opção que não contém a variável [AccessEnabler] valor.

## Relatando um problema

Quando você estiver relatando um problema para a Autenticação Adobe Pass, considere as seguintes sugestões:

* tente fornecer as etapas de reprodução.
* tente fornecer as versões do sistema operacional e os modelos de dispositivo nos quais o problema ocorre.
* tente fornecer a versão do SDK iOS/tvOS do AccessEnabler que está apresentando o problema.
* tente capturar e anexar todas as mensagens de log do SDK do AccessEnabler iOS/tvOS usando uma das duas opções apresentadas na [Incluindo](#including) seção.
