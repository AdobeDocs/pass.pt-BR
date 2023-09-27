---
title: Atualizações de cookies - Sinalizadores SameSite e Seguro
description: Atualizações de cookies - Sinalizadores SameSite e Seguro
exl-id: cc1f60fd-fa64-48cb-a185-dba562a54c33
source-git-commit: 59672b44074c472094ed27a23d6bfbcd7654c901
workflow-type: tm+mt
source-wordcount: '910'
ht-degree: 0%

---

# Atualizações de cookies - Sinalizadores SameSite e Seguro {#cookies-updates---samesite-and-secure-flags}

>[!NOTE]
>
>O conteúdo desta página é fornecido apenas para fins informativos. O uso desta API requer uma licença atual do Adobe. Não é permitida nenhuma utilização não autorizada.

</br>


## Atualizações {#Updates}

Esta seção destaca as alterações introduzidas pelo navegador Chrome e pela Autenticação do Adobe Pass para lidar com cookies de terceiros.



### Atualizações do Chrome 80 {#Chrome}

A partir do Chrome versão 80 (exceto a versão 82), os cookies que não especificam um *SameSite* atributo será tratado como se fosse *SameSite=Lax*. Portanto, os cookies que precisam ser entregues em um contexto entre sites devem especificar explicitamente a *SameSite=None*, e também devem ser marcados com a tag *Seguro* atributo e entregue *HTTPS*. Mais detalhes sobre essas atualizações podem ser lidos na página oficial do chromium: <https://www.chromium.org/updates/same-site> e também de <https://web.dev/samesite-cookies-explained/>.


### Atualizações de autenticação do Adobe Pass {#Pass-Updates}

No momento, o serviço de autenticação da Adobe Pass depende de alguns cookies considerados cookies de terceiros do ponto de vista do navegador, incluindo o Chrome, para funcionar em combinação com algumas plataformas e versões dos SDKs de autenticação da Adobe Pass. Portanto, para estar em conformidade com as alterações futuras e continuar a fornecer esses cookies em um contexto entre sites desses SDKs mais antigos, o serviço de Autenticação da Adobe Pass implementa as alterações necessárias no *adobe-pass-2.55.1* versão.

Essas alterações em relação à *adobe-pass-2.55.1* a versão envolve adicionar o *Seguro* e *SameSite=None* Os atributos de todos os cookies passados para todos os SDKs de autenticação da Adobe Pass ao usar navegadores Chrome a partir da versão 80 e superior (exceto a versão 82).

A próxima seção apresenta alguns problemas em potencial para uma lista de plataformas e versões dos SDKs de autenticação da Adobe Pass, caso um usuário esteja usando o navegador Chrome 80 e superior (exceto a versão 82).

## Solução de problemas {#Troubleshooting}

Ao navegar por esta seção, lembre-se de que todos os cookies do serviço de Autenticação do Adobe Pass devem ter *Seguro* atributo definido em *adobe-pass-2.55.1* para todos os navegadores, enquanto a variável *SameSite=None* o atributo deve ser definido somente para navegadores Chrome versão 80 e superior (exceto a versão 82).


### Solução de problemas gerais {#General}

1. É importante observar que alguns agentes do usuário são incompatíveis com o *SameSite=None* atributo.

   - Versões do Chrome do Chrome 51 para o Chrome 66 (incluindo as duas extremidades). Essas versões do Chrome rejeitarão um cookie com *SameSite=None*. Isso também afeta versões mais antigas de navegadores derivados do Chromium, bem como o Android WebView. Esse comportamento estava correto de acordo com a versão da especificação do cookie naquele momento, mas com a adição do novo valor &quot;Nenhum&quot; à especificação, esse comportamento foi atualizado no Chrome 67 e mais recente. (Antes do Chrome 51, o atributo SameSite era totalmente ignorado e todos os cookies eram tratados como se fossem *SameSite=None*.)
   - Versões do navegador de UC no Android anteriores à versão 12.13.2. As versões mais antigas rejeitarão um cookie com *SameSite=None*. Esse comportamento estava correto de acordo com a versão da especificação do cookie naquele momento, mas com a adição do novo valor &quot;Nenhum&quot; à especificação, esse comportamento foi atualizado nas versões mais recentes do Navegador de Comunicação Unificada.
   - Versões do Safari e navegadores incorporados no MacOS 10.14 e todos os navegadores no iOS 12. Essas versões tratarão incorretamente os cookies marcados com *SameSite=None* como se estivessem marcados *SameSite=Strict*. Esse erro foi corrigido nas versões mais recentes do iOS e do MacOS.


1. É importante observar que os cookies com o *Seguro* o atributo deve ser enviado *HTTPS*, caso contrário, o cookie não acessará o serviço de Autenticação do Adobe Pass.

   - SDK JavaScript do AccessEnabler:
      - Obrigatório que a comunicação com *sp.auth.adobe.com* usos *HTTPS* para versões *2,35* e *3.5.0*, antes de introduzir o Registro de cliente dinâmico.
   - AccessEnabler iOS/tvOS SDK:
      - Obrigatório que a comunicação com *sp.auth.adobe.com* usos *HTTPS* para versões anteriores a *3.0.0*, antes de introduzir o Registro de cliente dinâmico.
   - SDK do Android AccessEnabler:
      - Obrigatório que a comunicação com *sp.auth.adobe.com* usos *HTTPS* para versões anteriores a *3.0.0*, antes de introduzir o Registro de cliente dinâmico.
   - SDK FireOS do AccessEnabler:
      - Obrigatório que a comunicação com *sp.auth.adobe.com* usos *HTTPS* para versão *2.0.4*.

</br>

### Solução de problemas do SDK JavaScript do AccessEnabler versão 2.35 {#235-Troubleshooting}

O fluxo de autenticação do usuário pode ser afetado no Chrome 80 e posterior (exceto a versão 82). Para garantir que o usuário não tenha problemas para se autenticar devido às atualizações acima, é possível:

- Verifique se *JSESSIONID* O cookie do é definido no navegador e ter a variável *SameSite=None* e *Seguro* atributos definidos.
- Verifique se *JSESSIONID* cookie do *https://sp.auth.adobe.com/authenticate/saml* solicitação de rede corresponde ao *JSESSIONID* cookie do *https://sp.auth.adobe.com/session* solicitação de rede.


### Solução de problemas do AccessEnabler JavaScript SDK versão 3.5.0 {#350-Troubleshooting}

O fluxo de autenticação do usuário pode ser afetado no Chrome 80 e posterior (exceto a versão 82). Para garantir que o usuário não tenha problemas para se autenticar devido às atualizações acima, é possível:

- Verifique se *JSESSIONID* O cookie do é definido no navegador e ter a variável *SameSite=None* e *Seguro* atributos definidos.
- Verifique se *JSESSIONID* cookie do *https://sp.auth.adobe.com/authenticate/saml* solicitação de rede corresponde ao *JSESSIONID* cookie do *https://sp.auth.adobe.com/session* solicitação de rede.
- Verifique se *pass\_sfp* O cookie do é definido no navegador e ter a variável *SameSite=None* e *Seguro* atributos definidos.
- Verifique se *pass\_sfp* cookie está definido em *https://sp.auth.adobe.com/session* solicitação de rede.


O fluxo de autorização do usuário pode ser afetado no Chrome 80 e posterior (exceto a versão 82). Para garantir que o usuário não tenha problemas para assistir a um recurso protegido, após ser autenticado com êxito, devido às atualizações acima, é possível:

- Verifique se *pass\_sfp* O cookie do é definido no navegador e ter a variável *SameSite=None* e *Seguro* atributos definidos.
- Verifique se *pass\_sfp* cookie está definido em *https://sp.auth.adobe.com/adobe-services/authorize* solicitação de rede.
- Verifique se *pass\_sfp* cookie está definido em *https://sp.auth.adobe.com/adobe-services/shortAuthorize* solicitação de rede.
