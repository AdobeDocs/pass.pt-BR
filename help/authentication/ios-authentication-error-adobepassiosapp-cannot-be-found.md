---
title: Erro de autenticação do iOS - adobepass.ios.app não pode ser encontrado
description: Erro de autenticação do iOS - adobepass.ios.app não pode ser encontrado
exl-id: cd97c6fb-f0fa-45c2-82c1-f28aa6b2fd12
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---

# Erro de autenticação do iOS - adobepass.ios.app não pode ser encontrado {#ios-authentication-error-adobepass.ios.app-cannot-be-found}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

## Problema {#issue}

O usuário está passando pelo Fluxo de autenticação e, após inserir suas credenciais com êxito com seu provedor, é redirecionado de volta para uma página de erro, uma página de pesquisa ou alguma outra página personalizada informando que `adobepass.ios.app` não pôde ser encontrada/resolvida.

## Explicação {#explanation}

No iOS, `adobepass.ios.app` é usado como o URL final de redirecionamento para indicar que o fluxo de Autenticação foi concluído. Neste ponto, o aplicativo precisa fazer uma solicitação ao AccessEnabler para obter o token de autenticação e finalizar o fluxo de autenticação.

O problema é que `adobepass.ios.app` não existe de fato e acionará uma mensagem de erro no `webView`. As versões anteriores do DemoApp para iOS presumiam que esse erro sempre seria acionado no final do fluxo de Autenticação e foi configurado para lidar com ele adequadamente (`indidFailLoadWithError`).

**Nota:** Esse problema foi corrigido em versões posteriores do DemoApp (incluídas no download do SDK do iOS).

Infelizmente, essa suposição NÃO está correta. Há alguns servidores DNS ou Proxy &quot;inteligentes&quot; que não apenas transmitem o erro gerado, mas executam um dos seguintes procedimentos:

- Criar uma página de erro personalizada
- Encaminhe para uma página de pesquisa ou para algum outro tipo de página ou portal do cliente.

Nesses casos, a resposta que retorna ao WebView do iOS será uma resposta perfeitamente válida no que diz respeito ao WebView e NÃO acionará o erro do qual o DemoApp antigo dependia.

## Solução {#solution}

NÃO faça a mesma suposição que o DemoApp faz. Em vez disso, intercepte a solicitação antes que ela seja executada (em `shouldStartLoadWithRequest`) e manuseá-lo adequadamente.

Exemplo de como interceptar a solicitação antes de ela ser executada:

```obj-c
- (BOOL)webView:(UIWebView*)localWebView shouldStartLoadWithRequest:(NSURLRequest*)request navigationType:(UIWebViewNavigationType)navigationType {

NSString *absolutePath = [[request URL] absoluteString]; 
if ([absolutePath isEqualToString:ADOBEPASS_REDIRECT_URL] && ![APP_DELEGATE getAuthenticationWasCalled]) {

// user was logged ok => call getAuthenticationToken() 
[APP_DELEGATE setGetAuthenticationWasCalled:YES]; 
[[APP_DELEGATE accessEnabler] getAuthenticationToken];
return NO;

}

return YES;

}
```

Algumas observações:

- NUNCA usar `adobepass.ios.app` diretamente em qualquer lugar no código. Em vez disso, use a constante `ADOBEPASS_REDIRECT_URL`
- A variável `return NO;` a instrução impedirá que a página carregue
- Certifique-se de que o `getAuthenticationToken` A chamada de é chamada uma vez e somente uma vez no código. Várias chamadas para `getAuthenticationToken` resultará em resultados indefinidos.
