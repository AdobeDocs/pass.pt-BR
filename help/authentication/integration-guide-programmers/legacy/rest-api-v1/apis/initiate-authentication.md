---
title: Iniciar autenticação
description: Iniciar autenticação
exl-id: 55dddd29-68d6-4aae-8744-307fea285e29
source-git-commit: c1f891fabd47954dc6cf76a575c3376ed0f5cd3d
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---

# (Herdado) Iniciar autenticação {#initiate-authentication}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

>[!IMPORTANT]
>
> Mantenha-se informado sobre os anúncios mais recentes do produto de Autenticação da Adobe Pass e as linhas do tempo de desativação agregadas na página [Anúncios de produto](/help/authentication/product-announcements.md).

>[!NOTE]
>
> A implementação da REST API é limitada por [Mecanismo de limitação](/help/authentication/integration-guide-programmers/throttling-mechanism.md)

## Endpoints da REST API {#clientless-endpoints}

&lt;REGGIE_FQDN>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Preparo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

&lt;SP_FQDN>:

* Produção - [api.auth.adobe.com](http://api.auth.adobe.com/)
* Preparo - [api.auth-staging.adobe.com](http://api.auth-staging.adobe.com/)

</br>


## Descrição {#description}

Inicia o processo de autenticação informando sobre um evento de seleção do MVPD. Cria um registro no banco de dados de Autenticação do Adobe Pass, que é reconciliado quando uma resposta bem-sucedida é recebida do MVPD.



| Endpoint | Chamado </br>por | Entrada   </br>Parâmetros | HTTP </br>Método | Resposta | Resposta HTTP </br> |
| --- | --- | --- | --- | --- | --- |
| &lt;SP_FQDN>/api/v1/authenticate | Módulo AuthN | 1. requestor_id (Obrigatório)</br>2.  mso_id (Obrigatório)</br>3.  reg_code (obrigatório)</br>4.  nome_domínio (Obrigatório)</br>5.  noflash=true - </br>    (Obrigatório, Parâmetro residual)</br>6.  no_iframe=true (Obrigatório, parâmetro Residual)</br>7.  parâmetros extras (Opcional)</br>8.  redirect_url (Obrigatório) | GET | O Aplicativo web de logon é redirecionado para a página de logon do MVPD. | 302 para implementações de redirecionamento completo |

{style="table-layout:auto"}


| Parâmetro de entrada | Descrição |
| --- | --- |
| requestor_id | O solicitante do Programador para o qual esta operação é válida. |
| mso_id | A MVPD ID para a qual esta operação é válida. |
| reg_code | O código de registro gerado pelo serviço Reggie. |
| nome_do_domínio | O domínio de origem. |
| redirect_url | O URL de redirecionamento do Aplicativo Web de Logon após a conclusão da autenticação. |

{style="table-layout:auto"}

</br>

>[!IMPORTANT]
> 
>**Importante: Parâmetros obrigatórios -** Independentemente da implementação no lado do cliente, todos os parâmetros acima são obrigatórios.
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
> * generic\_data - habilita o uso de [TempPass Promocional](/help/authentication/integration-guide-programmers/features-premium/temporary-access/temp-pass-feature.md#promotional-temp-pass)
>
>```JSON
>Example:
>   generic_data=("email":"email@domain.com")
>```


### **Notas** {#notes}

* O valor do parâmetro `domain_name` deve ser definido como um dos nomes de domínio registrados com a Autenticação Adobe Pass. Para obter mais detalhes, consulte [Registro e Inicialização](/help/authentication/kickstart/programmer-overview.md).

* [Evite usar &#39;&amp;&#39;reg\_code in /authenticate request (Nota técnica)](/help/authentication/integration-guide-programmers/legacy/notes-technical/clientless-avoid-using-reg-code-in-authenticate-request.md)

* O parâmetro `redirect_url` deve ser o último a ser ordenado

* O valor do parâmetro `redirect_url` deve ser codificado em URL
