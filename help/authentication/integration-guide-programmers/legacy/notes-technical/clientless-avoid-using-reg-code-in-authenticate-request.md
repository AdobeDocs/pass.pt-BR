---
title: Evite usar '&'reg_code in /authenticate Request
description: Evite usar '&'reg_code in /authenticate Request
exl-id: c0ecb6f9-2167-498c-8a2d-a692425b31c5
source-git-commit: 3818dce9847ae1a0da19dd7decc6b7a6a74a46cc
workflow-type: tm+mt
source-wordcount: '246'
ht-degree: 0%

---

# (Herdado) Evite usar &#39;&amp;&#39;reg_code in /authenticate Request {#clientless-avoid-using-reg_code-in-authenticate-request}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

</br>



## Problema

O navegador IE 9 interpreta &#39;\&amp;reg&#39; como um comando especial e o converte em ®.

## Explicação

Se a solicitação `/authenticate` for composta da seguinte maneira...


```
    <FQDN>authenticate? requestor_id=someRequestor&reg_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


... será interpretado pelo navegador IE conforme abaixo e será enviado para o Adobe neste formato:


```
    <FQDN>authenticate?requestor_id=someRequestor&reg;_code=EKAFMFI&domain_name=someRequestor.com&noflash=true&mso_id=someMvpd&redirect_url=someRequestor.redirect.url.html
```


O requestor\_id será interpretado como univision®\_code=EKAFIFM, já que não há &#39;&amp;&#39; e o Adobe não encontrará um parâmetro `regCode` para associar ao token.  Há uma chance de o token de Autenticação não ser criado, caso em que `/checkauthn` chamadas não encontrarão nenhum token.



## Solução

Uma das seguintes opções deve resolver esse problema:

1. Evite usar o parâmetro `&reg_code` entre os outros parâmetros da cadeia de caracteres de consulta.  Em vez disso, mova-o para o primeiro parâmetro da string de consulta no URL da solicitação, tornando o URL da solicitação assim:


       &lt;FQDN>authenticate?reg_code =EKAFIFM&amp;requestor_id=someRequestor&amp;domain_name=someRequestor.com&amp;noflash=true&amp;mso_id=someMvpd&amp;redirect_url=someRequestor.redirect.url.html
   

   Dessa forma, o parâmetro `&reg` não será interpretado incorretamente.

1. Normalize `&reg_code` como usando `&amp;reg_code`.

1. O Adobe poderia introduzir um novo recurso para enviar um código de erro de volta à segunda tela em resposta a uma chamada de autenticação, se a criação do token de autenticação falhasse.
