---
title: Iniciar autenticação
description: Iniciar autenticação
exl-id: 55dddd29-68d6-4aae-8744-307fea285e29
source-git-commit: ea064031c3a1fee3298d85cf442c40bd4bb56281
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 0%

---

# Iniciar autenticação {#initiate-authentication}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!NOTE]
>
> A implementação da REST API é limitada por [Mecanismo de limitação](/help/authentication/throttling-mechanism.md)

## Endpoints da REST API {#clientless-endpoints}

&lt;reggie_fqdn>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Estágios - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;sp_fqdn>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Estágios - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Descrição {#description}

Inicia o processo de autenticação informando sobre um evento de seleção MVPD. Cria um registro no banco de dados de Autenticação do Adobe Pass, que é reconciliado quando uma resposta bem-sucedida é recebida do MVPD.



| Endpoint | Chamado  </br>Por | Entrada   </br>Params | HTTP  </br>Método | Resposta | HTTP  </br>Resposta |
| --- | --- | --- | --- | --- | --- |
| &lt;sp_fqdn>/api/v1/authenticate | Módulo AuthN | 1. requestor_id (Obrigatório)</br>2.  mso_id (Obrigatório)</br>3.  reg_code (Obrigatório)</br>4.  domain_name (Obrigatório)</br>5.  noflash=true -  </br>    (Obrigatório, parâmetro residual)</br>6.  no_iframe=true (Obrigatório, Parâmetro residual)</br>7.  parâmetros extras (opcional)</br>8.  redirect_url (Obrigatório) | GET | O aplicativo web de logon é redirecionado para a página de logon do MVPD. | 302 para implementações de redirecionamento completo |

{style="table-layout:auto"}


| Parâmetro de entrada | Descrição |
| --- | --- |
| requestor_id | O solicitante do Programador para o qual esta operação é válida. |
| mso_id | A ID de MVPD para a qual esta operação é válida. |
| reg_code | O código de registro gerado pelo serviço Reggie. |
| nome_do_domínio | O domínio de origem. |
| redirect_url | O URL de redirecionamento do Aplicativo Web de Logon após a conclusão da autenticação. |

{style="table-layout:auto"}

</br>

>[!IMPORTANT]
> 
>**Importante: Parâmetros obrigatórios -** Independentemente da implementação do lado do cliente, todos os parâmetros acima são obrigatórios.
>
>
>Exemplo:
>
>```
>domain_name=loginwebapp.com
>mso_id=sampleMvpdId
>reg_code=RO0885W
>requestor_id=sampleRequestorId
>noflash=true
>redirect_url=http://loginwebapp.com
>```

>[!IMPORTANT]
> 
>**Importante: parâmetros opcionais**
>
>A chamada também pode conter parâmetros opcionais que habilitam outras funcionalidades como:
>
> * generic\_data - permite o uso de [TempPass Promocional](/help/authentication/promotional-temp-pass.md)
>
>```JSON
>Example:
>   generic_data=("email":"email@domain.com")
>```


### **Notas** {#notes}

* O valor de `domain_name` O parâmetro deve ser definido como um dos nomes de domínio registrados com a Autenticação Adobe Pass. Para obter mais detalhes, consulte [Registro e inicialização](/help/authentication/programmer-overview.md).

* [Evite usar &#39;&amp;&#39;reg\_code in /authenticate request (Nota técnica)](/help/authentication/clientless-avoid-using-reg-code-in-authenticate-request.md)

* A variável `redirect_url` O parâmetro deve ser o último em ordem

* O valor de `redirect_url` O parâmetro deve ser codificado em URL
