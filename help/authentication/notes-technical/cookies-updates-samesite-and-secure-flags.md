---
title: Atualizações de cookies - Sinalizadores SameSite e Seguro
description: Atualizações de cookies - Sinalizadores SameSite e Seguro
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: d982beb16ea0db29f41d0257d8332fd4a07a84d8
workflow-type: tm+mt
source-wordcount: '933'
ht-degree: 0%

---

# Atualizações de cookies - Sinalizadores SameSite e Seguro {#cookies-updates---samesite-and-secure-flags}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

</br>


## Atualizações {#Updates}

Esta seção destaca as alterações introduzidas pelo navegador Chrome e pela Autenticação Adobe Pass para lidar com cookies de terceiros.



### Atualizações do Chrome 80 {#Chrome}

A partir do Chrome versão 80 (exceto a versão 82), os cookies que não especificam um atributo *SameSite* serão tratados como se fossem *SameSite=Lax*. Portanto, os cookies que precisam ser entregues em um contexto entre sites devem especificar explicitamente o *SameSite=None*, e também devem ser marcados com o atributo *Secure* e entregues via *HTTPS*. Mais detalhes sobre essas atualizações podem ser lidos na página oficial do chromium: <https://www.chromium.org/updates/same-site> e também de <https://web.dev/samesite-cookies-explained/>.


### Atualizações de autenticação do Adobe Pass {#Pass-Updates}

No momento, o serviço de autenticação da Adobe Pass depende de alguns cookies considerados cookies de terceiros do ponto de vista do navegador, incluindo o Chrome, para funcionar em combinação com algumas plataformas e versões dos SDKs de autenticação da Adobe Pass. Portanto, para estar em conformidade com as alterações futuras e continuar a fornecer esses cookies em um contexto entre sites desses SDKs mais antigos, o serviço de Autenticação da Adobe Pass implementa as alterações necessárias na versão *adobe-pass-2.55.1*.

Essas alterações da versão *adobe-pass-2.55.1* envolvem a adição dos atributos *Secure* e *SameSite=None* para todos os cookies passados para todos os SDKs de Autenticação da Adobe Pass ao usar navegadores Chrome a partir da versão 80 e superior (exceto a versão 82).

A próxima seção apresenta alguns problemas em potencial para uma lista de plataformas e versões dos SDKs de autenticação da Adobe Pass, caso um usuário esteja usando o navegador Chrome 80 e superior (exceto a versão 82).

## Solução de problemas {#Troubleshooting}

Ao navegar por esta seção, lembre-se de que todos os cookies do serviço de Autenticação do Adobe Pass devem ter o atributo *Secure* definido na versão *adobe-pass-2.55.1* para todos os navegadores, enquanto o atributo *SameSite=None* deve ser definido somente para os navegadores Chrome versão 80 e posterior (exceto a versão 82).


### Solução de problemas gerais {#General}

1. É importante observar que alguns agentes do usuário são incompatíveis com o atributo *SameSite=None*.

   - Versões do Chrome do Chrome 51 para o Chrome 66 (inclusive em ambos os lados). Estas versões do Chrome rejeitarão um cookie com *SameSite=None*. Isso também afeta versões mais antigas de navegadores derivados do Chromium, bem como o Android WebView. Esse comportamento estava correto de acordo com a versão da especificação do cookie naquele momento, mas com a adição do novo valor &quot;Nenhum&quot; à especificação, esse comportamento foi atualizado no Chrome 67 e mais recente. (Antes do Chrome 51, o atributo SameSite era totalmente ignorado e todos os cookies eram tratados como se fossem *SameSite=None*.)
   - Versões do navegador de UC no Android anteriores à versão 12.13.2. As versões anteriores rejeitarão um cookie com *SameSite=None*. Esse comportamento estava correto de acordo com a versão da especificação do cookie naquele momento, mas com a adição do novo valor &quot;Nenhum&quot; à especificação, esse comportamento foi atualizado nas versões mais recentes do Navegador de Comunicação Unificada.
   - Versões do Safari e navegadores incorporados no MacOS 10.14 e todos os navegadores no iOS 12. Estas versões tratarão erroneamente os cookies marcados com *SameSite=None* como se estivessem marcados com *SameSite=Strict*. Esse erro foi corrigido nas versões mais recentes do iOS e do MacOS.


1. É importante observar que os cookies com o atributo *Secure* devem ser enviados por *HTTPS*; caso contrário, o cookie não alcançará o serviço de Autenticação do Adobe Pass.

   - SDK do JavaScript do AccessEnabler:
      - Obrigatório que a comunicação com *sp.auth.adobe.com* use *HTTPS* para as versões *2.35* e *3.5.0*, antes de introduzir o Registro de Cliente Dinâmico.
   - AccessEnabler iOS/tvOS SDK:
      - Obrigatório que a comunicação com *sp.auth.adobe.com* use *HTTPS* para versões anteriores a *3.0.0*, antes de introduzir o Registro de Cliente Dinâmico.
   - SDK do Android do AccessEnabler:
      - Obrigatório que a comunicação com *sp.auth.adobe.com* use *HTTPS* para versões anteriores a *3.0.0*, antes de introduzir o Registro de Cliente Dinâmico.
   - SDK FireOS do AccessEnabler:
      - Obrigatório que a comunicação com *sp.auth.adobe.com* use *HTTPS* para a versão *2.0.4*.

</br>

### Solução de problemas do AccessEnabler JavaScript SDK versão 2.35 {#235-Troubleshooting}

O fluxo de autenticação do usuário pode ser afetado no Chrome 80 e posterior (exceto a versão 82). Para garantir que o usuário não tenha problemas para se autenticar devido às atualizações acima, é possível:

- Verifique se o cookie *JSESSIONID* está definido no navegador e com os atributos *SameSite=None* e *Secure* definidos.
- Verifique se o cookie *JSESSIONID* da solicitação de rede *https://sp.auth.adobe.com/authenticate/saml* corresponde ao cookie *JSESSIONID* da solicitação de rede *https://sp.auth.adobe.com/session*.


### Solução de problemas do AccessEnabler JavaScript SDK versão 3.5.0 {#350-Troubleshooting}

O fluxo de autenticação do usuário pode ser afetado no Chrome 80 e posterior (exceto a versão 82). Para garantir que o usuário não tenha problemas para se autenticar devido às atualizações acima, é possível:

- Verifique se o cookie *JSESSIONID* está definido no navegador e com os atributos *SameSite=None* e *Secure* definidos.
- Verifique se o cookie *JSESSIONID* da solicitação de rede *https://sp.auth.adobe.com/authenticate/saml* corresponde ao cookie *JSESSIONID* da solicitação de rede *https://sp.auth.adobe.com/session*.
- Verifique se o cookie *pass\_sfp* está definido no navegador e com os atributos *SameSite=None* e *Secure* definidos.
- Verifique se o cookie *pass\_sfp* está definido na solicitação de rede *https://sp.auth.adobe.com/session*.


O fluxo de autorização do usuário pode ser afetado no Chrome 80 e posterior (exceto a versão 82). Para garantir que o usuário não tenha problemas para assistir a um recurso protegido, após ser autenticado com êxito, devido às atualizações acima, é possível:

- Verifique se o cookie *pass\_sfp* está definido no navegador e com os atributos *SameSite=None* e *Secure* definidos.
- Verifique se o cookie *pass\_sfp* está definido na solicitação de rede *https://sp.auth.adobe.com/adobe-services/authorize*.
- Verifique se o cookie *pass\_sfp* está definido na solicitação de rede *https://sp.auth.adobe.com/adobe-services/shortAuthorize*.
