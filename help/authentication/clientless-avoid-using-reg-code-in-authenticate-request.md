---
title: Evite usar '&'reg_code in /authenticate Request
description: Evite usar '&'reg_code in /authenticate Request
exl-id: c0ecb6f9-2167-498c-8a2d-a692425b31c5
source-git-commit: 19ed211c65deaa1fe97ae462065feac9f77afa64
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 0%

---

# Evite usar &#39;&amp;&#39;reg_code in /authenticate Request {#clientless-avoid-using-reg_code-in-authenticate-request}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

</br>



## Problema

O navegador IE 9 interpreta &#39;\®&#39; como um comando especial e o converte para ®.

## Explicação

Se a variável `/authenticate` é composta da seguinte forma...


```
    <FQDN>authenticate? requestor_id=someRequestor&reg_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


... será interpretado pelo navegador IE conforme abaixo e será enviado para o Adobe neste formato:


```
    <FQDN>authenticate?requestor_id=someRequestor&reg;_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


O requestor\_id será interpretado como univision®\_code=EKAFIFM, já que não há &#39;&amp;&#39; e o Adobe não encontrará um `regCode` parâmetro ao qual associar o token.  Há uma chance de o token de autenticação não ser criado, nesse caso `/checkauthn` As chamadas do não localizarão nenhum token.



## Solução

Uma das seguintes opções deve resolver esse problema:

1. Evite usar o `&reg_code` parâmetro entre os outros parâmetros da cadeia de caracteres de consulta.  Em vez disso, mova-o para o primeiro parâmetro da string de consulta no URL da solicitação, tornando o URL da solicitação assim:


       &lt;fqdn>authenticate?reg_code =EKAFIFM&amp;requestor_id=someRequestor&amp;domain_name=someRequestor.com&amp;noflash=true&amp;mso_id=someMvpd&amp;redirect_url=someRequestor.redirect.url.html
   

   Deste modo, a Comissão `&reg` O parâmetro não será interpretado incorretamente.

1. Normalizar `&reg_code` como usar `&amp;reg_code`.

1. O Adobe poderia introduzir um novo recurso para enviar um código de erro de volta à segunda tela em resposta a uma chamada de autenticação, se a criação do token de autenticação falhasse.
